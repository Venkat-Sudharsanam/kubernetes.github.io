---
title: "Running your first containers in Kubernetes"
---
Ok, you've run one of the [getting started guides](/{{page.version}}/docs/getting-started-guides/) and you have
successfully turned up a Kubernetes cluster.  Now what?  This guide will help you get oriented
to Kubernetes and running your first containers on the cluster.

### Running a container (simple version)

From this point onwards, it is assumed that `kubectl` is on your path from one of the getting started guides.

The [`kubectl run`](kubectl/kubectl_run) line below will create two [nginx](https://registry.hub.docker.com/_/nginx/) [pods](pods) listening on port 80. It will also create a [replication controller](replication-controller) named `my-nginx` to ensure that there are always two pods running.

{% highlight bash %}

kubectl run my-nginx --image=nginx --replicas=2 --port=80

{% endhighlight %}

Once the pods are created, you can list them to see what is up and running:

{% highlight bash %}

kubectl get pods

{% endhighlight %}

You can also see the replication controller that was created:

{% highlight bash %}

kubectl get rc

{% endhighlight %}

To stop the two replicated containers, stop the replication controller:

{% highlight bash %}

kubectl stop rc my-nginx

{% endhighlight %}

### Exposing your pods to the internet.

On some platforms (for example Google Compute Engine) the kubectl command can integrate with your cloud provider to add a [public IP address](services.html#external-services) for the pods,
to do this run:

{% highlight bash %}

kubectl expose rc my-nginx --port=80 --type=LoadBalancer

{% endhighlight %}

This should print the service that has been created, and map an external IP address to the service. Where to find this external IP address will depend on the environment you run in.  For instance, for Google Compute Engine the external IP address is listed as part of the newly created service and can be retrieved by running

{% highlight bash %}

kubectl get services

{% endhighlight %}

In order to access your nginx landing page, you also have to make sure that traffic from external IPs is allowed. Do this by opening a firewall to allow traffic on port 80.

### Next: Configuration files

Most people will eventually want to use declarative configuration files for creating/modifying their applications.  A [simplified introduction](simple-yaml)
is given in a different document.


