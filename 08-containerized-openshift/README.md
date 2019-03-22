# Example: containerized ShinyProxy with a Kubernetes cluster

In this example, ShinyProxy will run inside a Kubernetes cluster. Shiny containers will also be spawned
in the same cluster. To make the application accessible outside the cluster, a NodePort service is created.

## How to run

1. Download the `Dockerfile` from the folder `kube-proxy-sidecar`.
2. Open a terminal, go to the directory containing the Dockerfile, and run the following command to build it:

`curl -O https://storage.googleapis.com/kubernetes-release/release/v1.11.0/bin/linux/amd64/kubectl`
`sudo docker build . -t kube-proxy-sidecar`

3. Ensure the `kube-proxy-sidecar` image is available on all your kube nodes. E.g. by repeating the above steps on all nodes.
4. Download the `Dockerfile` from the folder `shinyproxy-example`.
5. Open a terminal, go to the directory containing the Dockerfile, and run the following command to build it:

`curl -O https://www.shinyproxy.io/downloads/shinyproxy-2.1.0.jar`
`sudo docker build . -t shinyproxy-example`

6. Ensure the `shinyproxy-example` image is available on all your kube nodes.
7. Open a terminal and connect to your Openshift Cluster( oc login ....).
8. Create a project to run shiny proxy in.
`oc new-project shinyproxy`
9. Download the 3 `yaml` files from the folder where this README is located.
10. Run the following command to ConfigMap containing the application.yml for your shinyproxy deployment(namespace used for the pods in this example is `shinyproxy`):

`oc create -f sp-configmap.yaml -n shinyproxy`

11. Run the following command to deploy a pod containing `shinyproxy-example` and `kube-proxy-sidecar`:

`oc create -f sp-deployment.yaml -n shinyproxy`

12. Run the following command to deploy a service exposing ShinyProxy within the cluster:

`oc create -f sp-service.yaml -n shinyproxy`

13. Run the following command to deploy a service exposing ShinyProxy outside the cluster:

`oc create -f sp-route.yaml -n shinyproxy`

## Notes on the configuration

* The `kube-proxy-sidecar` container is used to make the apiserver accessible on `http://localhost:8001` to the `shinyproxy-example` container.

* The service will expose ShinyProxy via a service within the openshift cluster.