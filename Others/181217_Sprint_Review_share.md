# 18.12.17 Sprint Review Shares
## Contents
- About minikube
- Using IBM Container Registry
- About Ingress & Ingress Controller
- About Istio basics

## About minikube
### change spec. for minikube

You can change minikube spec(CPU & RAM) with following command:
```
$ minikube stop
$ minikube delete
$ minikube --memory 8192 --cpus 2 start
```
***Default spec is 1 CPU core and 1024MB RAM.***

## Using IBM Container Registry
### Push image to IBM Container Registry(ICR)
To push image to ICR, image must be tagged like:
```
$ docker tag <source_image>:<tag> registry.<region>.bluemix.net/<my_namespace>/<new_image_repo>:<new_tag>
```
Then push image with the following:
```
$ docker push registry.<region>.bluemix.net/<my_namespace>/<image_repo>:<tag>
```

### Deploy `k8s deployment` with an image stored inside ICR.
Before pull an image from the ICR, you must make the registry token for accessing the registry:
```
$ ibmcloud cr token-add --description "<description>" --non-expiring -q
```
Create k8s `Secret` to store your token information:
```
$kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_url> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
```
| Options | Description |
| --- | --- |
| `<kubernetes_namespace>` | Required. The Kubernetes namespace of your cluster where you want to use the secret and deploy containers to. Run kubectl get namespaces to list all namespaces in your cluster. |
| `<secret_name>` | Required. The name that you want to use for your imagePullSecret. |
| `<registry_url>` | Required. The URL to the image registry where your namespace is set up: registry.<dedicated_domain> |
| `--docker-username=token` | Required. ***Do not change this value.*** |
| `<token_value>` | Required. The value of your registry token that you retrieved earlier. |
| `<docker-email>` | Required. If you have one, enter your Docker email address. If you do not have one, enter a fictional email address, as for example a@b.c. This email is mandatory to create a Kubernetes secret, but is not used after creation. |

After creating the `Secret`, you must put the `Secret` info inside deployment yaml file to make k8s to pull an image from ICR when k8s creates deployment.
```
apiVersion: v1
kind: Deployment
metadata:
  name: <deployment_name>
  namespace: <namespace_name>
  labels:
    <key_name>: <deployment_name> or <other_character_squence_to_classify_the_deployment>
spec:
  containers:
    - name: <container_name>
      image: registry.<dedicated_domain>/<my_namespace>/<my_image>:<tag>
  imagePullSecrets:
    - name: <secret_name>
```
Done.
## About Ingress & Ingress Controller
The Ingress resource is a Kubernetes resource that defines the rules for how to route incoming requests for apps.

Ingress, added in Kubernetes v1.1, exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the ingress resource.

     internet
        |
    [ Ingress ]
    --|-----|--
    [ Services ]

An ingress can be configured to give services externally-reachable URLs, load balance traffic, terminate SSL, and offer name based virtual hosting. An ingress controller is responsible for fulfilling the ingress, usually with a loadbalancer, though it may also configure your edge router or additional frontends to help handle the traffic.

An ingress does not expose arbitrary ports or protocols. Exposing services other than HTTP and HTTPS to the internet typically uses a service of type `Service.Type=NodePort` or `Service.Type=LoadBalancer`.

For `minikube`, nginx ingress controller can be easily setup by using following command:
```
$ minikube addons enable ingress
```

For `IBM Cloud`, Ingress is available for standard clusters only and requires at least two worker nodes per zone to ensure high availability and that periodic updates are applied. Also, Ingress Controller is given by default, so you do not necessarily have to setup an ingress controller apart.


## About Istio basics
### Service mesh
### Data Plane
#### Envoy

### Control Plane
#### Pilot

#### Mixer
#### Citadel