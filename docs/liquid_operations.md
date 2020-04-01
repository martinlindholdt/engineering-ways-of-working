# Operational setup
_Every platform should follow the virtues of **Auditable**, **Low Risk** continuous integration and delivery, **Observable**, **Measureable** and **Alert and react** to change in health. In the Liquid platform the implementation of these are documented here._  

## Auditable 

### Hardware layer 

All changes to hardware infrastructure is done through UCS hardware profiles. 

While the implementation of hardware profiles are usually done through the controller unit of the UCS cluster we have the capability to inspect (and in theory apply) changes and settings through the APIs. For that we have a few Python based libraries: 

[Link missing]

### OpenStack layer 

Every virtual-infrastructural change is done through Terraform – a declarative provisioning tool. Therefore, the current state can at any time be evaluated by inspecting the definitions in code.  

This also allows all changes to be inspected, evaluated, and gated before implementation through pull requests.   

**Links** [links needed] 

* [Rancher Knet cluster terraform commits](https://gitlab.lqd.dk/liquid/rancher-clusters-terraform/commits/rancher-prod-1)
* [Rancher Public cluster terraform commits] 
* [Rancher Tools cluster terraform commits] 

### Kubernetes Layer 

The Kubernetes layer of liquid it based on Rancher. Once the VM-layer of infrastructure is terraformed, the nodes can be dynamically added through the Rancher System cluster. 

We have deliberately chosen not to use the Rancher auto-scaling options because we want the VM layer to be fully controlled and audible through Terraform code. 

Update and upgrade operations to Rancher clusters and Kubernetes versions are managed through Helm charts. 

Link: [Link to repo needed] 

### Change process integration 

Changes that impact the infrastructure of running production end-customer-services (no downtime, but stuff like patching, upgrading, updating ect.) is also registered in HPSM as a change in order to comply with the TDC ITIL changes processes. There is a _Standard Change_ in place, registered in HPSM and agreed with operations. [Std Change Coverage] (https://wiki.tdc.dk/pages/viewpage.action?pageId=300615995) 

For changes with downtime or backwards incompatible changes we have to paths: 

1. Go through regular change process. 
2. Build a new environment and ask applications to migrate within a min. 3 months timeframe. 

All changes can be tracked in HPSM under the assets "Liquid" or "Liquid (DEV/TEST)". 	

## Low risk 

_The Liquid platform release changes under a standard operating procedure. We differentiate between impact on **end customer services**, **development tools and services** and **platform services**._ 

*  _End customer services_: Nuuday customer experienced service levels and the production grade services the squads operate. 
*  _Development services_: Tools that developers depend on for building, deploying, monitoring, logging, and many other important functions. 
*  _Platform services_: Applications and functions that exist to keep the platform healthy, but not normally used by developers or customers. 

We put great effort in making almost every change a no-downtime event for _End customer services_ that has set up their availability configuration correctly (we can not be responsible for single-instance or misconfigured applications). 

1. Every change with no downtime for _End customer services_ is rolled out through:  
	* Fully automated deployment (Terraform, Helm, Ansible) 
	* Tested in stand-alone sandbox clusters, monitoring two things: 
		* Verification of rollout procedure 
		* Verification of workload impact 
	* Deployed one-node-at-a-time with rollback option 
2. If the change affect _development services_, the developers work may be interruption (e.g. upgrade of a tool), they are notified 24 hours in advance in the official Liquid Slack channel. 
3. The preferred change window is between 10:00 and 14:00, Mondays to Thursday. This is the time where the support is best because most people are available. 
3. Always documented as a “standard change” in HPSM to comply with processes  

In case of a service interruption (very few), the process follows the regular CAB process, creating change request and service windows outside of business hours and advance notifications.  

## Observable  
The Liquid platform constantly collects logs about all sorts of events. 

They are accessible from the very front page of the cluster GUI, but also collected and stored for 30 days in an ElasticSearch database where the team may query or search across all events, warning, errors that have happened with platform services or customer services. 

* [Knet cluster dashboard](https://rancher.lqd.dk/c/c-pfwv5/monitoring) 
* [Public cluster dashboard](https://rancher.lqd.dk/c/c-d7ch2/monitoring) 

Further we are implementing Datadog for Metrics, Logs analysis, dashboards, alerts and more. This is available to everybody in the team and will soon also be available to the entire organisation pr. request. 

* [link for Datadog dashboard to come] 

This makes us able to monitor and detect problems, but also assist teams that have questions or during SRTs.  

## Measurable  

Liquid is a platform that provides both primary hosting but also 7 managed tools and primary APIs for interacting with the applications. Therefore we have defined 2 tiers of SLIs. 

**Tier 1** is for the production workload clusters only:
Knet cluster, Public cluster, OpenStack TGL & SLT (VM hosting) and storage.  

**Tier 2**: For Tools, Kubernetes, GUI, Harbor and other managed services ect. 

The metrics may be best illustrated in this live dashboard: [https://status.knet.lqd.dk](https://status.knet.lqd.dk/)

### SLIs: 

* SLI is the 95th percentile response-time of Master API (Kubernetes) and entry point (ingress proxy), measured in milliseconds.  
* Definition of “unavailable” is SLI  > 1000 ms.  
* Definition of “uptime” is based on checks every 60 seconds and calculated as a percentage of not-unavailable over a week.  

### SLOs 

SLOs are defined as “uptime % > X” 

* Tier 1: SLO is 99.9% over a week. – meaning max 10 min downtime/week or a total of 10 slow responses in a week.  
* Tier 2: SLO is 99% over a week. Meaning 1h 40 minutes maximum downtime.
 
### Guaranteed uptime  
* Guaranteed uptime (if there exist such a thing) is 95% over a month on every service. 
 
## Alerting and support: 

The alerting setup is much more elaborate than the SLIs. There are hundreds of metrics collected and alarms set up for various combinations of patterns. Some of the more prominent are:  

* **ETCD** 
	* ETCD is unavailable 
	* A high number of leader changes within the etcd cluster are happening   (if the value is bigger than 3 it means etdc is not operating properly) 
	* Database usage close to the quota 800M 
	* Etcd member has no leader 
* Alerts for **k8s components**: API server, scheduler, controller manager: 
	* Controller Manager is unavailable 
	* Scheduler is unavailable 
* **Event** alerts: 
	* Get warning deployment failing event 
	* Ingress Controller Failed Config Reload 
* Alerts for each **node**:  
	* High cpu load 
	* High node memory utilization 
	* Node disk is running full within 24 hours 
* **SYSTEM** (kube-system) project level:  
	* Workload Alerts: 
	* Canal (network plugin) availability < 100%  
	* Cattle cluster agent availability < 100%  
	* Cattle node agent availability < 100% 
	* Core-DNS availability < 100%  
	* Ingress-NGINX deployment availability < 75%  

These alarms allow us to be warned before things go bad within our predictable and controllable space, and it ensures a fast response whenever unpredictable or external events happen.  
