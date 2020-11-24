# Replication & Controllers

Deploying managed pods

## Liveness probes

Kubernetes can check if a container is still alive through liveness probes. You can specify a liveness probe for each container in the pod’s specification. Kubernetes will periodi- cally execute the probe and restart the container if the probe fails.
Kubernetes can probe a container using one of the three mechanisms:

- An HTTP GET probe performs an HTTP GET request on the container’s IP address, a port and path you specify. If the probe receives a response, and the response code doesn’t represent an error (in other words, if the HTTP response code is 2xx or 3xx), the probe is considered successful. If the server returns an error response code or if it doesn’t respond at all, the probe is considered a fail- ure and the container will be restarted as a result.
- A TCP Socket probe tries to open a TCP connection to the specified port of the container. If the connection is established successfully, the probe is successful. Otherwise, the container is restarted.
- An Exec probe executes an arbitrary command inside the container and checks the command’s exit status code. If the status code is 0, the probe is successful. All other codes are considered failures.

## Replication Controller

A ReplicationController constantly monitors the list of running pods and makes sure the actual number of pods of a “type” always matches the desired number.

3 parts of a Replication Controller:

- label selector
- replica count
- pod template

## THE EFFECT OF CHANGING THE CONTROLLER’S LABEL SELECTOR OR POD TEMPLATE

Changes to the label selector and the pod template have no effect on existing pods. Changing the label selector makes the existing pods fall out of the scope of the ReplicationController, so the controller stops caring about them.

## Commands
