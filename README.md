# OpenShift Examples - Autoscaling
OpenShift can manually or automatically the scale application pods up and down based on the container metrics such as cpu and memory consumption.  This repo contains an intentionally simple example of automatically scaling (autoscaling) a webapp frontend with OpenShift.  

:information_source: this example is based on OpenShift Container Platform version 3.7

![Screenshot](./.screens/screenshot.png?raw=true)


## How to run this?
There is a template for creating all the components of this example. Use the oc CLI tool:
 > oc new-app -f https://raw.githubusercontent.com/dudash/openshiftexamples-autoscaling/master/autoscale_instant_template.yaml

*Another option to run this via the web console is:*

Create a new project, select `Import YAML/JSON` and then upload the raw file from here: `./autoscale_instant_template.yml` into the 

*Don't have an OpenShift cluster?* that's OK download the CDK for free here: [https://developers.redhat.com/products/cdk/overview/][https://developers.redhat.com/products/cdk/overview/].


## Why autoscale?
Two of the biggest reasons to leverage this capability in OpenShift:
1) Provide better uptime for your apps when user demand spikes
2) Reduce unecessary compute, scale down during periods of inactivity and up when needed


## How does this work and how can I configure autoscaling?
In order the define autoscaling for an app, we first define how much cpu and memory an instance of the app should consume (both min and max).  This becomes the guideline for OpenShift to know when to scale the pod up or down.  These details are placed into what OpenShift calls a "deployment config" ([you can read more about that here][1]).

You can read more about the details of compute resource request and limits [here][4].  You can read about how Quality of Service (QoS) can be leveraged [here][3].  And read about how to further configure autoscaling [here][2].


## About the code / software architecture
I'm still working on it, but the parts planned are:

* Simple web front end to collect user counts and push to a datbase or backend API layer
* Backend service or database to receive user info and inputs from the webapps
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