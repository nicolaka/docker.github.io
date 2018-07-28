---
title: Install the Kubernetes CLI
description: Learn how to install kubectl, the Kubernetes command-line tool, on Docker Universal Control Plane.
keywords: ucp, cli, administration, kubectl, Kubernetes
---

Docker EE installs Kubernetes automatically when you install UCP, and the
web UI enables deploying Kubernetes workloads and monitoring pods. You can
also interact with the Kubernetes deployment by using the Kubernetes
command-line tool, which is named kubectl, from inside or outside the cluster.


To use kubectl, install the binary on any node in the cluster (manager or worker nodes). To access
the UCP cluster with kubectl, install the UCP client bundle.

> Kubernetes on Docker for Mac 
>
> Docker for Mac 17.12 CE Edge provides a standalone Kubernetes server that
> runs on your Mac, with kubectl installed by default. This installation is
> separate from the Kubernetes deployment on a UCP cluster.
> Learn how to [deploy to Kubernetes on Docker for Mac](/docker-for-mac/kubernetes.md).
{: .important}

## Install the UCP client bundle

To access the Kubernetes API server that UCP exposes, you need the private and
public key pair that authorizes your requests to UCP. Follow the instructions
in [CLI-based access](cli.md#download-client-certificates-by-using-the-rest-api)
to install the client bundle.

> UCP client bundle is required
>
> If you run a kubectl command without the client bundle, you'll get an
> error like this:
> ```
> The connection to the server localhost:8080 was refused - did you specify the right host or port?
> ```
{: .warning}

## Install the kubectl binary

Install the version of kubectl on the node where you want
to control Kubernetes. You can install kubectl on nodes within your cluster or outside your cluster as long as you can access UCP. Learn how to [install and set up kubectl](https://v1-8.docs.kubernetes.io/docs/tasks/tools/install-kubectl/). Before you install kubectl, you need to make sure you're installing the correct version that matches the version of Kubernetes provisioned in Docker Enterprise. You can check the version of Kubernetes using the following command (assuming you have loaded the UCP client bundle)

```bash
$ docker version --format '{{range .Server.Components}}{{if eq .Name "Kubernetes"}}{{$ver:= split .Details.gitVersion "-"}}{{index $ver 0}}{{end}}{{end}}'
v1.8.11
```

Another method to get the Kuberenetes version is from the UCP portal by going to https://<UCP_URL>/manage/about/kubernetes and getting the `gitVersion` value.

## Confirm the connection to UCP

To confirm that kubectl is communicating with UCP, run:

```bash
kubectl config current-context
```

If the UCP client bundle is installed correctly, you'll see something like
this: 

```
ucp_54.70.245.225:6443_admin
```

## Inspect Kubernetes resources

When the kubectl executable is in place and the UCP client bundle is
installed, you can run kubectl commands against the UCP cluster, like you
would on any Kubernetes deployment.

For example, to see all resources in the default namespace, run:

```bash
kubectl get all
```

If you haven't deployed any Kubernetes workloads or created any Kubernetes
objects, you'll see something like this:

```
NAME             TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
svc/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   5d
```

## Where to go next

- [Deploy a workload to a Kubernetes cluster](../kubernetes.md)
- [Deploy to Kubernetes on Docker for Mac](/docker-for-mac/kubernetes.md)

