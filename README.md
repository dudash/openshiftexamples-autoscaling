# OpenShift Examples - Autoscaling
OpenShift can manually or automatically scale application pods up and down based on container metrics such as cpu and memory consumption.  This repo contains an intentionally simple example of automatically scaling (autoscaling) a webapp frontend with OpenShift.

Here's what it looks like:

![Screenshot](./.screens/screenshot.png?raw=true)

*(:information_source: This example is based on OpenShift Container Platform version 3.7)*


## How to run this?
First off, you need access to an OpenShift cluster.  Don't have an OpenShift cluster?  That's OK, download the CDK for free here: [https://developers.redhat.com/products/cdk/overview/][5].

There is a template for creating all the components of this example. Use the oc CLI tool:
 > `oc new-project autoscaledemo `
 > `oc new-app -f https://raw.githubusercontent.com/dudash/openshiftexamples-autoscaling/master/autoscale_instant_template.yaml`

*If you don't like the CLI, another option is to create and project and import the template via the web console:*
 > Create a new project, select `Import YAML/JSON` and then upload the raw file from here: `./autoscale_instant_template.yml` into the 

Now to showcase the autoscaling - let's simulate a large user load on the frontend webapp using Apache Benchmark.  If you have `ab` installed just run it against the frontend URL.  Or you can use OpenShfit to pull a [container image containing `ab`][6] and run it as self-terminating like this:
 > `oc run web-load --rm --attach --image=jordi/ab -- ab -n 50000 -c 10 http://URL_GOES_HERE`


## Why autoscale?
Two of the biggest reasons to leverage this capability in OpenShift:
1) Provide better uptime for your apps when user demand spikes
2) Reduce unecessary compute, scale down during periods of inactivity and up when needed


## How does this work and how can I configure autoscaling?
In order the define autoscaling for an app, we first define how much cpu and memory an instance of the app should consume (both min and max).  This becomes the guideline for OpenShift to know when to scale the pod up or down.  These details are placed into what OpenShift calls a "deployment config" ([you can read more about that here][1]).

In this example if you want to tweak a few things to in the example the following are the most common asks:

Change the min/max number of replicated containers of the web frontend:
 > `oc autoscale dc/webapp --min 1 --max 5 --cpu-percent=60`

Change the request and limit values for the web frontend deployment:
 > `oc set resources dc/webapp --requests=cpu=200m,memory=256Mi --limits=cpu=400m,memory=512Mi`

You can read more about the details of compute resource request and limits [here][4].  You can read about how Quality of Service (QoS) can be leveraged [here][3].  And read about how to further configure autoscaling [here][2].


## About the code / software architecture
I'm still working on it, but the parts planned are:

* Simple web front end to collect user counts and push to a datbase or backend API layer
* Backend service, database, or cache to receive data for/from the web frontend
* OpenShift template (to create/configure everything easily)
* Key OpenShift components that enable this example
	* container building (source to image)
	* container CPU / memory monitoring
	* container replication and service layer
	* software defined networking and load balancing router


## License
Under the terms of the MIT.


[1]: https://docs.openshift.com/container-platform/3.7/architecture/core_concepts/deployments.html#deployments-and-deployment-configurations
[2]: https://docs.openshift.com/container-platform/3.7/dev_guide/pod_autoscaling.html
[3]: https://docs.openshift.com/container-platform/3.7/dev_guide/compute_resources.html#quality-of-service-tiers
[4]: https://docs.openshift.com/container-platform/3.7/dev_guide/compute_resources.html#dev-cpu-requests
[5]: https://developers.redhat.com/products/cdk/overview/
[6]: https://hub.docker.com/r/jordi/ab/