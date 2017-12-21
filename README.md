# Kubeflow

The Kubeflow project is dedicated to making Machine Learning on Kubernetes easy, portable and scalable. Our goal is **not** to recreate other services, but to provide a straightforward way for spinning up best of breed OSS solutions. Contained in this repository are manifests for creating:

* A JupyterHub to create & manage interactive Jupyter notebooks
* A Tensorflow Training Controller that can be configured to use CPUs or GPUs, and adjusted to the size of a cluster with a single setting
* A TF Serving container

This document details the steps needed to run the Kubeflow project in any environment in which Kubernetes runs.

## The Kubeflow Mission

Our goal is to help folks use ML more easily, by letting Kubernetes to do what it's great at:
- Easy, repeatable, portable deployments on a diverse infrastructure (laptop <-> ML rig <-> training cluster <-> production cluster)
- Deploying and managing loosely-coupled microservices
- Scaling based on demand

Because ML practitioners use so many different types of tools, it is a key goal that you can customize the stack to whatever your requirements (within reason), and let the system take care of the "boring stuff." While we have started with a narrow set of technologies, we are working with many different projects to include additional tooling.

Ultimately, we want to have a set of simple manifests that give you an easy to use ML stack _anywhere_ Kubernetes is already running and can self configure based on the cluster it deploys into.

## Setup

This documentation assumes you have a Kubernetes cluster already available. For specific Kubernetes installations, additional configuration may be necessary.

### Minikube

[Minikube](https://github.com/kubernetes/minikube) is a tool that makes it easy to run Kubernetes locally. Minikube runs a
single-node Kubernetes cluster inside a VM on your laptop for users looking to try out Kubernetes or develop with it day-to-day.
The below steps apply to a minikube cluster - the latest version as of writing this documentation is 0.23.0. You must also have
kubectl configured to access minikube.

### Google Kubernetes Engine

[Google Kubernetes Engine](https://cloud.google.com/kubernetes-engine/) is a managed environment for deploying Kubernetes applications powered by Google Cloud.
If you're using Google Kubernetes Engine, prior to creating the manifests, you must grant your own user the requisite RBAC role to create/edit other RBAC roles.

```commandline
kubectl create clusterrolebinding default-admin --clusterrole=cluster-admin --user=user@gmail.com
```
## Quick Start

Ensure you have ksonnet version [0.8.0](https://github.com/ksonnet/ksonnet/releases) or later.

In order to quickly set up all components, execute the following commands,

```commandline
# Initialize a ksonnet APP
APP_NAME=my-kubeflow
ks init ${APP_NAME}
cd ${APP_NAME}

# Install Kubeflow components
ks registry add kubeflow github.com/google/kubeflow/tree/master/kubeflow
ks pkg install kubeflow/core
ks pkg install kubeflow/tf-serving
ks pkg install kubeflow/tf-job

# Deploy Kubeflow
ks generate core kubeflow-core --name=kubeflow-core --namespace=${NAMESPACE}
ks apply default kubeflow-core
```


The above command sets up JupyterHub and a custom resource for running TensorFlow training jobs. Furthermore, the ksonnet packages
provide prototypes that can be used to configure TensorFlow jobs and deploy TensorFlow models. 
Used together, these make it easy for a user go from training to serving using Tensorflow with minimal
effort in a portable fashion between different environments. 

For more detailed instructions about how to use Kubeflow please refer to the [user guide](user_guide.md)

If you'd like to try out Kubeflow, we've partnerned with Katacoda to offer a browser based walkthrough. You can try that [here](https://www.katacoda.com/kubeflow).

## Get involved

* [Slack Channel](https://join.slack.com/t/kubeflow/shared_invite/enQtMjgyMzMxNDgyMTQ5LWUwMTIxNmZlZTk2NGU0MmFiNDE4YWJiMzFiOGNkZGZjZmRlNTExNmUwMmQ2NzMwYzk5YzQxOWQyODBlZGY2OTg)
* [Twitter](http://twitter.com/kubeflow)
* [Mailing List](https://groups.google.com/forum/#!forum/kubeubeflow-discuss)
