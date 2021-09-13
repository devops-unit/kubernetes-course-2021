# Pull the necessary images:
docker pull nathanleclaire/curl:latest
docker pull openjdk:8u111-jre-alpine

# Start the controller container, note that it has RW access to the Docker API socket:
docker run \
  -ti \
  --rm \
  --name=curl \
  --volume=/var/run/docker.sock:/var/run/docker.sock:rw \
  nathanleclaire/curl sh

# Note that we are now in a shell in the controller container.

# Install the JQ (JSON Query) utility in the controller container:
apk add --update jq

# List all running containers:
curl -s --unix-socket /var/run/docker.sock http:/containers/json | jq .[].Names
    "/curl"

# List all images:
curl -s --unix-socket /var/run/docker.sock http:/images/json | jq .[].RepoTags
    "nathanleclaire/curl:latest"

# Create a container:
CONTAINER_NAME="java-test"
curl \
  --silent \
  --unix-socket /var/run/docker.sock \
  "http:/containers/create?name=${CONTAINER_NAME}" \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{ "Image": "openjdk:8u111-jre-alpine", "Cmd": [ "java", "-version" ] }' | jq '.'

  {
    "Id": "602995e0d277e67417d9ad142959db7853a788bcd079ac33a72e24fb2db2f33c",
    "Warnings": null
  }

# Start the container:
curl \
  --silent \
  --unix-socket /var/run/docker.sock \
  "http:/containers/${CONTAINER_NAME}/start" \
  -X POST \
  -H "Content-Type: application/json" \
  --output /dev/null \
  --write-out "%{http_code}"

204/ #

# If you open a second shell to the Docker host system you will see that a Java container was run:
docker ps -a
CONTAINER ID        IMAGE                      COMMAND             CREATED             STATUS                     PORTS               NAMES
602995e0d277        openjdk:8u111-jre-alpine   "java -version"     40 seconds ago      Exited (0) 4 seconds ago                       java-test
7a53976521ec        nathanleclaire/curl        "sh"                16 minutes ago      Up 16 minutes                                  curl
