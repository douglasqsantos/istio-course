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
minikube start --driver=none --memory 2048
```


Adding label to istio see the pods in a namespace
```bash
kubectl label namespace default istio-injection=enabled
```