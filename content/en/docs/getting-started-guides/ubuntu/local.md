---
title: Local Kubernetes development with LXD
content_template: templates/task
---

{{% capture overview %}}
Running Kubernetes locally has obvious development advantages, such as lower cost and faster iteration than constantly deploying and tearing down clusters on a public cloud. Ideally, a Kubernetes developer can spawn all necessary nodes inside local containers and test new configurations as they are committed. This page will show you how to deploy a cluster to LXD containers on a local machine.
{{% /capture %}}

The purpose of using [LXD](https://linuxcontainers.org/lxd/) on a local machine is to emulate the same deployment that a user would use in a cloud or bare metal. Each node is treated as a machine, with the same characteristics as production. Each node is a separate container, which runs Docker containers and `kubectl` inside (see [Cluster Intro](/docs/tutorials/kubernetes-basics/cluster-intro/) for more info).

{{% capture prerequisites %}}
Install [conjure-up](http://conjure-up.io/), a tool for deploying big software.
Add the current user to the `lxd` user group.
    
```
sudo snap install conjure-up --classic
sudo usermod -a -G lxd $(whoami)
```

If you have never run or configured lxd before, you will need to run the following
command to set up the default storage pool and the network bridge required:

```
sudo lxd init
```

If a bridge named `lxdbr0` already exists, you can tell the init config tool to use it.

Note: If conjure-up asks you to "Setup an IPv6 subnet" with LXD, answer NO. IPv6 with Juju/LXD is currently unsupported.
If you already have a bridge configured, e.g. `lxdbr0`, [disable IPv6 on the bridge](https://docs.conjure-up.io/stable/en/troubleshoot#common-problems), otherwise you won't be able to choose it.
{{% /capture %}}

{{% capture steps %}}
## Deploying Kubernetes

Start the deployment with:

    conjure-up kubernetes

For this walkthrough we are going to create a new controller - select the `localhost` Cloud type:

![Select Cloud](/images/docs/ubuntu/00-select-cloud.png)

Deploy the applications:

![Deploy Applications](/images/docs/ubuntu/01-deploy.png)

Wait for Juju bootstrap to finish:

![Bootstrap](/images/docs/ubuntu/02-bootstrap.png)

Wait for our Applications to be fully deployed:

![Waiting](/images/docs/ubuntu/03-waiting.png)

Run the final post-processing steps to automatically configure your Kubernetes environment:

![Postprocessing](/images/docs/ubuntu/04-postprocessing.png)

Review the final summary screen:

![Final Summary](/images/docs/ubuntu/05-final-summary.png)

## Accessing the Cluster 

You can access your Kubernetes cluster by running the following:
    
    
    kubectl --kubeconfig=~/.kube/config
    

Or if you've already run this once it'll create a new config file as shown in the summary screen.
    
    
    kubectl --kubeconfig=~/.kube/config.conjure-up
    
{{% /capture %}}



