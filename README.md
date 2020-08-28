# Istio Hand-on for kubernetes
- https://istio.io/
- https://github.com/Dickchesterwood/istio-fleetman

## Getting Started

What is Istio?
- Istio is a "Service Mesh"

What is a Service Mesh?
- A Service Mesh is an extra layer of software you deploy alongside your cluster (eg Kubernetes)

Data Plane
- The Proxies are collectively called the "Data Plane" in Istio
- Everything else is called the "Control Plane"

## Installing Minikube
- https://www.youtube.com/watch?v=5-LHcpkRA58
- https://kubernetes.io/docs/tasks/tools/install-minikube/

## Need to test with
- https://microk8s.io/docs

## Install kubectl on Linux

Install kubectl binary with curl on Linux
```bash
curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"
```

Make the kubectl binary executable.
```bash
chmod +x ./kubectl
```

Move the binary in to your PATH.
```bash
sudo mv ./kubectl /usr/local/bin/kubectl
```

Test to ensure the version you installed is up-to-date:
```bash
kubectl version --client
Client Version: version.Info{Major:"1", Minor:"18", GitVersion:"v1.18.8", GitCommit:"9f2892aab98fe339f3bd70e3c470144299398ace", GitTreeState:"clean", BuildDate:"2020-08-13T16:12:48Z", GoVersion:"go1.13.15", Compiler:"gc", Platform:"linux/amd64"}
```

## Install the Docker

```bash
curl -L https://raw.githubusercontent.com/douglasqsantos/DevOps/master/Docker/install-docker.sh | sudo bash
```

## Installing Minikube

If you're not installing via a package, you can download a stand-alone binary and use that.

```bash
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube
```

Move the binary in to your PATH.
```bash
sudo mv ./minikube /usr/local/bin/minikube
```

Install the dependencies
```bash
sudo apt-get install -y conntrack
```

Run the command below to start the minikube as root
```bash
minikube start --driver=none
```

Once minikube start finishes, run the command below to check the status of the cluster as root:
```bash
minikube status
```

If your cluster is running, the output from minikube status should be similar to as root:
```bash
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

After you have confirmed whether Minikube is working with your chosen hypervisor, you can continue to use Minikube or you can stop your cluster. To stop your cluster, run:
```bash
minikube stop
```

Starting from a fresh environment, delete the old config
```bash
minikube delete
```

Start with 4GB
```bash
minikube start --driver=none --memory 4096
```


Adding label to istio see the pods in a namespace
```bash
kubectl label namespace default istio-injection=enabled
```

## Telemetry

Delete old deploys
```bash
minikube delete
```

Starting up the minikube
```bash
minikube start --driver=none --memory 4096
```

Now we use the Files/1 Telemetry
```bash
cd 1\ Telemetry/
```

Deploying the istio-init
```bash
kubectl apply -f 1-istio-init.yaml
namespace/istio-system created
customresourcedefinition.apiextensions.k8s.io/meshpolicies.authentication.istio.io created
customresourcedefinition.apiextensions.k8s.io/policies.authentication.istio.io created
customresourcedefinition.apiextensions.k8s.io/httpapispecs.config.istio.io created
customresourcedefinition.apiextensions.k8s.io/httpapispecbindings.config.istio.io created
customresourcedefinition.apiextensions.k8s.io/quotaspecs.config.istio.io created
customresourcedefinition.apiextensions.k8s.io/quotaspecbindings.config.istio.io created
customresourcedefinition.apiextensions.k8s.io/destinationrules.networking.istio.io created
customresourcedefinition.apiextensions.k8s.io/envoyfilters.networking.istio.io created
customresourcedefinition.apiextensions.k8s.io/gateways.networking.istio.io created
customresourcedefinition.apiextensions.k8s.io/serviceentries.networking.istio.io created
customresourcedefinition.apiextensions.k8s.io/sidecars.networking.istio.io created
customresourcedefinition.apiextensions.k8s.io/virtualservices.networking.istio.io created
customresourcedefinition.apiextensions.k8s.io/attributemanifests.config.istio.io created
customresourcedefinition.apiextensions.k8s.io/handlers.config.istio.io created
customresourcedefinition.apiextensions.k8s.io/instances.config.istio.io created
customresourcedefinition.apiextensions.k8s.io/rules.config.istio.io created
customresourcedefinition.apiextensions.k8s.io/clusterrbacconfigs.rbac.istio.io created
customresourcedefinition.apiextensions.k8s.io/rbacconfigs.rbac.istio.io created
customresourcedefinition.apiextensions.k8s.io/serviceroles.rbac.istio.io created
customresourcedefinition.apiextensions.k8s.io/servicerolebindings.rbac.istio.io created
customresourcedefinition.apiextensions.k8s.io/authorizationpolicies.security.istio.io created
customresourcedefinition.apiextensions.k8s.io/peerauthentications.security.istio.io created
customresourcedefinition.apiextensions.k8s.io/requestauthentications.security.istio.io created
customresourcedefinition.apiextensions.k8s.io/adapters.config.istio.io created
customresourcedefinition.apiextensions.k8s.io/templates.config.istio.io created
```

Now after 2 minutes we need to deploy the 2-istio-minikube.yaml
```bash
kubectl apply -f 2-istio-minikube.yaml
configmap/istio-grafana-configuration-dashboards-citadel-dashboard created
configmap/istio-grafana-configuration-dashboards-galley-dashboard created
configmap/istio-grafana-configuration-dashboards-istio-mesh-dashboard created
configmap/istio-grafana-configuration-dashboards-istio-performance-dashboard created
configmap/istio-grafana-configuration-dashboards-istio-service-dashboard created
configmap/istio-grafana-configuration-dashboards-istio-workload-dashboard created
configmap/istio-grafana-configuration-dashboards-mixer-dashboard created
[...]
envoyfilter.networking.istio.io/metadata-exchange-1.5 created
envoyfilter.networking.istio.io/tcp-metadata-exchange-1.5 created
envoyfilter.networking.istio.io/stats-filter-1.5 created
envoyfilter.networking.istio.io/tcp-stats-filter-1.5 created
validatingwebhookconfiguration.admissionregistration.k8s.io/istiod-istio-system created
validatingwebhookconfiguration.admissionregistration.k8s.io/istio-galley created
```

After finish we'll have something like
```bash
kubectl get po -n istio-system
NAME                                   READY   STATUS    RESTARTS   AGE
grafana-5f6f8cbf75-5bw69               1/1     Running   0          107s
istio-egressgateway-8455c96498-q2rbm   1/1     Running   0          106s
istio-ingressgateway-c57d9d55-gnchn    1/1     Running   0          106s
istio-tracing-9dd6c4f7c-nxf67          1/1     Running   0          106s
istiod-79654b97c4-nv9r6                1/1     Running   0          105s
kiali-7fdddcb998-rc956                 1/1     Running   0          107s
prometheus-d78dbcffd-rn89q             2/2     Running   0          105s
```

Now we need to apply the labels
```bash
kubectl apply -f 4-label-default-namespace.yaml
Warning: kubectl apply should be used on resource created by either kubectl create --save-config or kubectl apply
namespace/default configured
```

Now we need to install the apps
```bash
kubectl apply -f 5-application-no-istio.yaml
deployment.apps/position-simulator created
deployment.apps/position-tracker created
deployment.apps/api-gateway created
deployment.apps/webapp created
deployment.apps/vehicle-telemetry created
deployment.apps/staff-service created
service/fleetman-webapp created
service/fleetman-position-tracker created
service/fleetman-api-gateway created
service/fleetman-vehicle-telemetry created
service/fleetman-staff-service created
```

Now let's check the applications
```bash
kubectl get po
NAME                                  READY   STATUS    RESTARTS   AGE
api-gateway-9b5bc4cd8-bc9w7           2/2     Running   0          70s
position-simulator-7c5f678bc9-jjqcx   2/2     Running   0          70s
position-tracker-f9cbfb57c-dgm5s      2/2     Running   0          70s
staff-service-865856c749-2vtbd        2/2     Running   0          70s
vehicle-telemetry-bc64d4bc9-vqxqt     2/2     Running   0          70s
webapp-7b599b8b46-bkqr6               2/2     Running   0          70s
```

Now we can get the minikube ip
```bash
minikube ip
10.0.0.25
```

We can access the application at http://10.0.0.25:30080

## Kiali Deeper Dive

Telemetry requisites
- You need a (Envoy Sidecar) Proxy running in each pod you want to monitor
- The control plane needs to be running (ie Mixer/Telemetry)
- You do NOT need any specific Istio yaml configuration! (NO need for VirtualServices, Gateways, etc)

We can access kiali on http://10.0.0.25:30081
- admin
- admin

Listing the virtualservices or vs
```bash
kubectl get virtualservices
No resources found in default namespace.
```

Listing the destinationrules or dr
```bash
kubectl get destinationrules
No resources found in default namespace.
```

Stopping the route to a service
- select a service
- right click
  - show details
- on actions
  - Suspend traffic
  - Create

Getting the virtual services
```bash
 kubectl get virtualservices
NAME                     GATEWAYS   HOSTS                                                AGE
fleetman-staff-service              [fleetman-staff-service.default.svc.cluster.local]   39s
```

Getting the destinationrules
```bash
kubectl get destinationrules
NAME                     HOST                                               AGE
fleetman-staff-service   fleetman-staff-service.default.svc.cluster.local   3m1s
```

Jaeger UI
- http://10.0.0.25:30082/

# Why you need to "Propagate Headers"
- https://istio.io/latest/docs/tasks/observability/distributed-tracing/overview/
- https://github.com/OpenFeign/feign

## References
- https://opentracing.io/
- https://kiali.io/
- https://istio.io/

## Traffic Management
**Canary Release**
- Deploy a new version of a software component (for us, new image)
- But only make that new image "live" for a percentage of the time
- Most of the time, the old (definitely working) version is the one being used

Calling from the API
- curl http://10.150.10.24:30080/api/vehicles/driver/City%20Truck

while true; do curl http://10.150.10.24:30080/api/vehicles/driver/City%20Truck; echo; sleep 0.5; done

Describing virtualservice
```bash
kubectl get vs fleetman-staff-service -o yaml
```

## What is a VirtualService?
- A VirtualService enables us to configure custom routing rules to the service mesh

## What Does a VirtualService fit in?

## Points to Note
- Despite the name, VirtualService and Service aren't really related
- VirtualServices don't replace Services - they do different jobs
- You only need a VirtualService if you need one (!)


## Load Balancing
**Question** Is it ossible to use the weighted destination rules to make a single user "stick" to a canary?
Quick answer: no, not using weightings

All of this is at the time of recording (late 2019); there is a feature request pending for this


Using load balancing with header definition
```bash
curl --header "myval: 192" http://10.150.10.24:30080/api/vehicles/driver/City%20Truck
```

## What is ConsistentHashing Useful for??

Example: you do some heavy processing for a user, you want to cache that in the pod's memory so the next time the same client requests, they get a cache hit. **Don't ever rely on this to implement a "session"**

## Gateways
- https://istio.io/latest/docs/tasks/traffic-management/ingress/ingress-control/

## Canary Requirement
- We have a new experimental front end, tagged "6-experimental"
- Requirement: deploy this as a 10% canary release
- This should be easy...


The proxies run after a container makes a request

checking the ingressgateway
```bash
kubectl get po -n istio-system
NAME                                   READY   STATUS    RESTARTS   AGE
grafana-5f6f8cbf75-5bw69               1/1     Running   0          46h
istio-egressgateway-8455c96498-q2rbm   1/1     Running   0          46h
istio-ingressgateway-c57d9d55-gnchn    1/1     Running   0          46h
istio-tracing-9dd6c4f7c-nxf67          1/1     Running   0          46h
istiod-79654b97c4-nv9r6                1/1     Running   0          46h
kiali-7fdddcb998-rc956                 1/1     Running   0          46h
prometheus-d78dbcffd-rn89q             2/2     Running   0          46h
```

Getting the labels to configure the ingress
```bash
kubectl get po -n istio-system --show-labels
NAME                                   READY   STATUS    RESTARTS   AGE   LABELS
grafana-5f6f8cbf75-5bw69               1/1     Running   0          46h   app=grafana,chart=grafana,heritage=Tiller,pod-template-hash=5f6f8cbf75,release=istio-system
istio-egressgateway-8455c96498-q2rbm   1/1     Running   0          46h   app=istio-egressgateway,chart=gateways,heritage=Tiller,istio=egressgateway,pod-template-hash=8455c96498,release=istio,service.istio.io/canonical-name=istio-egressgateway,service.istio.io/canonical-revision=1.5
istio-ingressgateway-c57d9d55-gnchn    1/1     Running   0          46h   app=istio-ingressgateway,chart=gateways,heritage=Tiller,istio=ingressgateway,pod-template-hash=c57d9d55,release=istio,service.istio.io/canonical-name=istio-ingressgateway,service.istio.io/canonical-revision=1.5
istio-tracing-9dd6c4f7c-nxf67          1/1     Running   0          46h   app=jaeger,pod-template-hash=9dd6c4f7c,release=istio
istiod-79654b97c4-nv9r6                1/1     Running   0          46h   app=istiod,istio=pilot,pod-template-hash=79654b97c4
kiali-7fdddcb998-rc956                 1/1     Running   0          46h   app=kiali,pod-template-hash=7fdddcb998,release=istio
prometheus-d78dbcffd-rn89q             2/2     Running   0          46h   app=prometheus,pod-template-hash=d78dbcffd,release=istio
```

https://istio.io/latest/docs/reference/config/networking/virtual-service/

## Dark Releases

Describing the virtualservice
```yaml
kubectl describe vs fleetman-webapp
Name:         fleetman-webapp
Namespace:    default
Labels:       <none>
Annotations:  API Version:  networking.istio.io/v1beta1
Kind:         VirtualService
Metadata:
  Creation Timestamp:  2020-08-26T12:33:20Z
  Generation:          6
  Managed Fields:
    API Version:  networking.istio.io/v1alpha3
    Fields Type:  FieldsV1
    fieldsV1:
      f:metadata:
        f:annotations:
          .:
          f:kubectl.kubernetes.io/last-applied-configuration:
      f:spec:
        .:
        f:gateways:
        f:hosts:
        f:http:
    Manager:         kubectl
    Operation:       Update
    Time:            2020-08-26T14:17:45Z
  Resource Version:  148879
  Self Link:         /apis/networking.istio.io/v1beta1/namespaces/default/virtualservices/fleetman-webapp
  UID:               6b84a5b5-e8a8-4e3f-9568-243834a51183
Spec:
  Gateways:
    ingress-gateway-configuration
  Hosts:
    *
  Http:
    Match:
      Headers:
        My - Header:
          Exact:  canary
    Route:
      Destination:
        Host:    fleetman-webapp.default.svc.cluster.local
        Subset:  experimental
    Route:
      Destination:
        Host:    fleetman-webapp.default.svc.cluster.local
        Subset:  original
Events:          <none>
```

Checking the header
```bash
curl -H "my-header:canary" -s 10.150.10.24:30380/canary | grep -i title
  <title>Fleet Management Istio Premium Enterprise Edition</title>
```

without the canary header
```bash
curl -H "my-header:no-canary" -s 10.150.10.24:30380/canary | grep -i title
  <title>Fleet Management</title>
```

## Dark Release Demo
- Restore your cluster to that the ":6-placeholder" version of staff management is live
- Can you "Dark Release" the new and untested :6 version? (The photos)
- There is a catch - see if you can spot it!

## Fault Injection
- https://principlesofchaos.org/
- https://en.wikipedia.org/wiki/Chaos_engineering
- http://days-of-chaos.com/

## Circuit Breaking
- https://istio.io/latest/docs/concepts/traffic-management/
- https://istio.io/latest/docs/reference/config/networking/destination-rule/#OutlierDetection
- https://github.com/fortio/fortio
- https://github.com/Netflix/Hystrix
- https://github.com/resilience4j/resilience4j
- https://en.wikipedia.org/wiki/Cascading_failure
- https://landing.google.com/sre/sre-book/chapters/introduction/
- https://landing.google.com/sre/sre-book/chapters/addressing-cascading-failures/#:~:text=A%20cascading%20failure%20is%20a,portions%20of%20the%20system%20fail.

## Problems with "Classic" Circuit Breaking
- You need to build the circuit breaker into your application (ie every Microservice)
- You are relying on your programming language/platform having one

```bash
while true; do curl 10.150.10.24:30380/api/vehicles/driver/City%20Truck; echo; done
```

## Mutual TLS (mTLS)
- This is a few lines of yaml that is probably a massive win for many projects...
- youtu.be/gEzCKNA-nCg

## Assumption
- Even if you think that pod <-> pod traffic isn't going to leave a building... ASSUME IT IS

## mTLS What Istio Can Do
- Enforce a policy that BLOCKS all non TLS traffic
- Automatically upgrade all proxy-proxy communication to use mTLS

## "STRICT" mTLS
- https://istio.io/latest/docs/tasks/security/authentication/authn-policy/
- https://istio.io/latest/docs/tasks/security/authentication/authn-policy/#globally-enabling-istio-mutual-tls-in-strict-mode

## Customizing and Installing Istio with Istioctl
- https://istio.io/latest/docs/reference/commands/istioctl/
- https://istio.io/latest/docs/setup/install/istioctl/
```bash
curl -L https://istio.io/downloadIstio | sh -
```

Clean the minikube
```bash
minikube delete
```

Starting a new minikube
```bash
minikube start --driver=none --memory 4096
```

Checking the pods
```bash
kubectl get po --all-namespaces
NAMESPACE     NAME                                            READY   STATUS    RESTARTS   AGE
kube-system   coredns-66bff467f8-9ckqn                        0/1     Running   0          9s
kube-system   etcd-homolog-ansible-vm-01                      1/1     Running   0          14s
kube-system   kube-apiserver-homolog-ansible-vm-01            1/1     Running   0          14s
kube-system   kube-controller-manager-homolog-ansible-vm-01   1/1     Running   0          14s
kube-system   kube-proxy-nf4nm                                1/1     Running   0          8s
kube-system   kube-scheduler-homolog-ansible-vm-01            1/1     Running   0          14s
kube-system   storage-provisioner                             1/1     Running   0          13s
```

Install Istio
```bash
istioctl install --set profile=demo
Detected that your cluster does not support third party JWT authentication. Falling back to less secure first party JWT. See https://istio.io/docs/ops/best-practices/security/#configure-third-party-service-account-tokens for details.
✔ Istio core installed
✔ Istiod installed
✔ Egress gateways installed
✔ Ingress gateways installed
✔ Installation complete
```

We can install with the default profile
```bash
istioctl install --set profile=default
Detected that your cluster does not support third party JWT authentication. Falling back to less secure first party JWT. See https://istio.io/docs/ops/best-practices/security/#configure-third-party-service-account-tokens for details.
✔ Istio core installed
✔ Istiod installed
✔ Ingress gateways installed
✔ Installation complete
```

We can install with default profile and enable kiali
```bash
istioctl install --set profile=default --set addonComponents.kiali.enabled=true
Detected that your cluster does not support third party JWT authentication. Falling back to less secure first party JWT. See https://istio.io/docs/ops/best-practices/security/#configure-third-party-service-account-tokens for details.
! addonComponents.kiali.enabled is deprecated; use the samples/addons/ deployments instead
✔ Istio core installed
✔ Istiod installed
✔ Ingress gateways installed
✔ Addons installed
✔ Installation complete
```

We can install with kiali and grafana, but take a look that we need to usa samples/addons
```bash
istioctl install --set profile=default --set values.grafana.enabled=true --set values.kiali.enabled=true
Detected that your cluster does not support third party JWT authentication. Falling back to less secure first party JWT. See https://istio.io/docs/ops/best-practices/security/#configure-third-party-service-account-tokens for details.
! values.grafana.enabled is deprecated; use the samples/addons/ deployments instead
! values.kiali.enabled is deprecated; use the samples/addons/ deployments instead
! addonComponents.grafana.enabled is deprecated; use the samples/addons/ deployments instead
! addonComponents.kiali.enabled is deprecated; use the samples/addons/ deployments instead
✔ Istio core installed
✔ Istiod installed
✔ Ingress gateways installed
✔ Addons installed
✔ Installation complete
```

Checking the pods
```bash
kubectl get po -n istio-system
NAME                                    READY   STATUS    RESTARTS   AGE
istio-egressgateway-695f5944d8-hz8b6    1/1     Running   0          2m35s
istio-ingressgateway-5c697d4cd7-qj6sl   1/1     Running   0          2m35s
istiod-59747cbfdd-vhdwx                 1/1     Running   0          2m59s
```

Adding the label to inject Envoy
```bash
kubectl label namespace default istio-injection=enabled
```

Profiles
- https://istio.io/latest/docs/reference/commands/istioctl/
- https://istio.io/latest/docs/setup/install/istioctl/
- https://istio.io/latest/docs/setup/getting-started/#download
- https://istio.io/latest/docs/setup/additional-setup/config-profiles/
- https://istio.io/latest/docs/reference/config/istio.operator.v1alpha1/#IstioOperatorSpec
- https://istio.io/latest/docs/reference/config/istio.operator.v1alpha1/#ExternalComponentSpec

IstioOperator
- https://istio.io/latest/docs/reference/config/istio.operator.v1alpha1/


Listing the profiles
```bash
istioctl profile list
Istio configuration profiles:
    default
    demo
    empty
    minimal
    preview
    remote
```

Dump the profile
```yaml
istioctl profile dump default
apiVersion: install.istio.io/v1alpha1
kind: IstioOperator
spec:
  addonComponents:
    istiocoredns:
      enabled: false
  components:
    base:
      enabled: true
    cni:
      enabled: false
    egressGateways:
[...]
```


Istio 1.7


Minikube

Clean the environment
```bash
minikube delete
🔄  Uninstalling Kubernetes v1.18.3 using kubeadm ...
🔥  Deleting "minikube" in none ...
💀  Removed all traces of the "minikube" cluster.
```

Starting
```bash
minikube start --driver=none --memory 4096
😄  minikube v1.12.3 on Ubuntu 18.04
✨  Using the none driver based on user configuration
❗  The 'none' driver does not respect the --memory flag
👍  Starting control plane node minikube in cluster minikube
🤹  Running on localhost (CPUs=4, Memory=16013MB, Disk=29598MB) ...
ℹ️  OS release is Ubuntu 18.04.4 LTS
🐳  Preparing Kubernetes v1.18.3 on Docker 19.03.12 ...
    ▪ kubelet.resolv-conf=/run/systemd/resolve/resolv.conf
🤹  Configuring local host environment ...

❗  The 'none' driver is designed for experts who need to integrate with an existing VM
💡  Most users should use the newer 'docker' driver instead, which does not require root!
📘  For more information, see: https://minikube.sigs.k8s.io/docs/reference/drivers/none/

❗  kubectl and minikube configuration will be stored in /root
❗  To use kubectl or minikube commands as your own user, you may need to relocate them. For example, to overwrite your own settings, run:

    ▪ sudo mv /root/.kube /root/.minikube $HOME
    ▪ sudo chown -R $USER $HOME/.kube $HOME/.minikube

💡  This can also be done automatically by setting the env var CHANGE_MINIKUBE_NONE_USER=true
🔎  Verifying Kubernetes components...
🌟  Enabled addons: default-storageclass, storage-provisioner
🏄  Done! kubectl is now configured to use "minikube"
```

Installing Istio
```bash
cd /usr/src && curl -L https://istio.io/downloadIstio | sh -
```

Creating the link
```bash
ln -s /usr/src/istio-course/istio-1.7.0/bin/istioctl /usr/local/bin/istioctl
```

Installing the Istio
```bash
istioctl install --set profile=default
Detected that your cluster does not support third party JWT authentication. Falling back to less secure first party JWT. See https://istio.io/docs/ops/best-practices/security/#configure-third-party-service-account-tokens for details.
✔ Istio core installed
✔ Istiod installed
✔ Ingress gateways installed
✔ Installation complete
```



Installing Prometheus
```bash
kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.7/samples/addons/prometheus.yaml
```

Installing Grafana
```bash
kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.7/samples/addons/grafana.yaml
```

Installing Kiali
```bash
kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.7/samples/addons/kiali.yaml
```

Installing Jeager
```bash
kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.7/samples/addons/jaeger.yaml
```

Exposing Grafana
```yaml
vim 01-grafana.yaml
apiVersion: networking.istio.io/v1alpha3
kind: Gateway
metadata:
  name: grafana-gateway
  namespace: istio-system
spec:
  selector:
    istio: ingressgateway
  servers:
  - port:
      number: 80
      name: http-grafana
      protocol: HTTP
    hosts:
    - "grafana.dqs.local"
---
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: grafana-vs
  namespace: istio-system
spec:
  hosts:
  - "grafana.dqs.local"
  gateways:
  - grafana-gateway
  http:
  - route:
    - destination:
        host: grafana
        port:
          number: 3000
---
apiVersion: networking.istio.io/v1alpha3
kind: DestinationRule
metadata:
  name: grafana
  namespace: istio-system
spec:
  host: grafana
  trafficPolicy:
    tls:
      mode: DISABLE
---
```

Applying the configuration
```bash
kubectl apply -f 01-grafana.yaml
gateway.networking.istio.io/grafana-gateway created
virtualservice.networking.istio.io/grafana-vs created
destinationrule.networking.istio.io/grafana created
```

Exposing Kiali
```yaml
vim 02-kiali.yaml
apiVersion: networking.istio.io/v1alpha3
kind: Gateway
metadata:
  name: kiali-gateway
  namespace: istio-system
spec:
  selector:
    istio: ingressgateway
  servers:
  - port:
      number: 80
      name: http-kiali
      protocol: HTTP
    hosts:
    - "kiali.dqs.local"
---
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: kiali-vs
  namespace: istio-system
spec:
  hosts:
  - "kiali.dqs.local"
  gateways:
  - kiali-gateway
  http:
  - route:
    - destination:
        host: kiali
        port:
          number: 20001
---
apiVersion: networking.istio.io/v1alpha3
kind: DestinationRule
metadata:
  name: kiali
  namespace: istio-system
spec:
  host: kiali
  trafficPolicy:
    tls:
      mode: DISABLE
---
```

Applying the configuratioin
```bash
kubectl apply -f 02-kiali.yaml
gateway.networking.istio.io/kiali-gateway created
virtualservice.networking.istio.io/kiali-vs created
destinationrule.networking.istio.io/kiali created
```

Exposing prometheus
```yaml
vim 03-prometheus.yaml
apiVersion: networking.istio.io/v1alpha3
kind: Gateway
metadata:
  name: prometheus-gateway
  namespace: istio-system
spec:
  selector:
    istio: ingressgateway
  servers:
  - port:
      number: 80
      name: http-prom
      protocol: HTTP
    hosts:
    - "prometheus.dqs.local"
---
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: prometheus-vs
  namespace: istio-system
spec:
  hosts:
  - "prometheus.dqs.local"
  gateways:
  - prometheus-gateway
  http:
  - route:
    - destination:
        host: prometheus
        port:
          number: 9090
---
apiVersion: networking.istio.io/v1alpha3
kind: DestinationRule
metadata:
  name: prometheus
  namespace: istio-system
spec:
  host: prometheus
  trafficPolicy:
    tls:
      mode: DISABLE
---
```

Applying the configuration
```bash
kubectl apply -f 03-prometheus.yaml
gateway.networking.istio.io/prometheus-gateway created
virtualservice.networking.istio.io/prometheus-vs created
destinationrule.networking.istio.io/prometheus created
```

Exposing tracing
```yaml
vim 04-tracing.yaml
apiVersion: networking.istio.io/v1alpha3
kind: Gateway
metadata:
  name: tracing-gateway
  namespace: istio-system
spec:
  selector:
    istio: ingressgateway
  servers:
  - port:
      number: 80
      name: http-tracing
      protocol: HTTP
    hosts:
    - "tracing.dqs.local"
---
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: tracing-vs
  namespace: istio-system
spec:
  hosts:
  - "tracing.dqs.local"
  gateways:
  - tracing-gateway
  http:
  - route:
    - destination:
        host: tracing
        port:
          number: 80
---
apiVersion: networking.istio.io/v1alpha3
kind: DestinationRule
metadata:
  name: tracing
  namespace: istio-system
spec:
  host: tracing
  trafficPolicy:
    tls:
      mode: DISABLE
---
```

Applying the configuration
```bash
kubectl apply -f 04-tracing.yaml
gateway.networking.istio.io/tracing-gateway created
virtualservice.networking.istio.io/tracing-vs created
destinationrule.networking.istio.io/tracing created
```

Visit the telemetry addons via your browser.

- Kiali: http://kiali.dqs.local
- Prometheus: http://prometheus.dqs.local
- Grafana: http://grafana.dqs.local
- Tracing: http://tracing.dqs.local

Cleanup
Remove all related Gateways:
```bash
kubectl -n istio-system delete gateway grafana-gateway kiali-gateway prometheus-gateway tracing-gateway
```

Remove all related Virtual Services:
```bash
kubectl -n istio-system delete virtualservice grafana-vs kiali-vs prometheus-vs tracing-vs
```

Checking the differente between profiles
```bash
istioctl profile diff default demo
```

Exporting the default profile
```bash
istioctl profile dump default > default.yaml
```

- https://istio.io/latest/docs/setup/install/istioctl/

Dumping the profile default
```bash
istioctl profile dump default > default.yaml
```

Converting the file into k8s
```bash
istioctl manifest generate -f default.yaml > 01-istio-init.yaml
```

Now we can apply the files
```bash
kubectl apply -f 01-istio-init.yaml
```

## Using
- https://www.udemy.com/course/istio-hands-on-for-kubernetes/learn/lecture/16783056#overview
- http://10.150.10.24:30380/vehicle/Village%20Truck
- http://10.150.10.24:30081/kiali/
- http://10.150.10.24:30082/jaeger/search
- http://10.150.10.24:30083/?orgId=1
- https://istio.io/latest/search/?q=virtual%20service
- https://istio.io/latest/docs/reference/config/networking/virtual-service/#HTTPMatchRequest

