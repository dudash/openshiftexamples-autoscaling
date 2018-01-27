# OpenShift Examples - Autoscaling
OpenShift can manually or automatically the scale application pods up and down based on the container metrics such as cpu and memory consumption.  This repo contains an intentionally simple example of automatically scaling (autoscaling) a webapp frontend with OpenShift.  
![Screenshot](./.screens/screenshot.png?raw=true)

## How to run this?
There is a template for creating all the components of this example. Use the CLI (after logging into an OpenShift server):
 > oc new-app -f https://raw.githubusercontent.com/dudash/candv/master/oc_templates/candv_instant_template.yaml

*Another option to run this via the web console is:*

Create a new project, select `Import YAML/JSON` and then upload the raw file from here: `./autoscale_instant_template.yml` into the 

# Why Autoscale?
Two of the biggest reasons to leverage this capability in OpenShift:
1) Provide better uptime for your apps when user demand spikes on your apps and services
2) Reduce unecessary compute, scale down during periods of inactivity and up when needed

## License
Under the terms of the MIT.
