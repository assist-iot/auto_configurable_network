![image-3](https://user-images.githubusercontent.com/100677511/170439941-58810f43-b437-41e5-9976-899b60cf1e5e.png)


# Auto-configurable network (ACN) enabler

Note: Final version

## Introduction
---------------

The objective of this enabler is to provide optimal network performance KPIs by reconfiguration of the flows routing. It will work jointly with the SDN controller enabler to configure virtual network according to policy rules with AI support.

## Features
-----------

This enabler provides functionalities for optimising network configuration leveraging the SDN Controller of an ASSIST-IoT ecosystem. It assumes generation of the policies and enforces them using the northbound APIs of the SDN Controllers. Polices can be set manually or automatically (using different algorithms like AI solutions) to improve the performance and quality of selected KPIs of the network (e.g., network load distribution, data transfer losses and latency). 

## Place in architecture
------------------------

The ACN enabler is located in the Smart Network and Control plane as SDN elements of the ASSIST-IoT architecture. In particular, it belongs to the building block related to SDN networks, which is related to network configraion functionalities.

![place-in-architecture](./images/place-in-architecture.png)
Figure.1. ASSIST-IoT architecture.

This enabler considers two components: 
1. Policy engine, in charge of the creation of polices and their execution in the SDN network for optimising the network traffic and the creation of routing paths. It obtains network information through the SDN controller, and data traffic statistics via monitoring module.
2. Monitoring module, responsible for collecting network traffic statistics. 

![place-in-architecture2](./images/architecture.png)

Figure.2. Auto-configurable newtork enabler architecture.


## User guide
-------------

In the following table are presented the endpoint ready to use:

### Communication interfaces

Currently, this enabler is envisioned to work automatically, without interacting with users. Any configuration parameter needed (e.g., SDN Controller address) will be passed to the enabler at instantiation time. In further releases, a feature related to enable manual activation/deactivation of the policies will be assessed and, if needed, implemented following the endpoint indicated below.

| **Method** | **Endpoint**                   | **Description**              | **Payload (if need)**                                                   |
|------------|--------------------------------|------------------------------|-------------------------------------------------------------------------|
| POST       | /enabled/{true/false}          | Enables/Disables the enabler | na                                                                      |


                                                                        
## Installation
---------------

The installation of ACN in k8 environment can be done using Assist-IoT helm charts from project repository:

```sh
git clone https://github.com/assist-iot/auto_configurable_network.git
cd deployment/helm/ 
helm install acn ./auto_configurable_network
```


## Expected scenarios
---------------------

The usage of the enabler is related to the strategy of the performance/quality parameters goal optimisation. Three strategies (currently under development) are intended to be implemented, aiming at optimising traffic load optimisation, data transfer losses and latency in the network (RTT). 
A flow diagram and related steps of the main use case is presented below, consisting in the policy-based adaptation of the network, also considering the gathering of needed information:


![scenario](./images/use-case.png)

STEP 1: The policy engine requires data from the network. The monitoring module has to collect them previously, communicating with agents present in network nodes. This will be a continuous operation once the enabler is on.

STEP 2: The policy engine requests the selected parameters for a given purpose (optimise the load traffic, data losses or latency) from the monitoring module.

STEP 3-4: After data reception, the policy module generates the rules and sends them to the SDN controller.

STEP 5: SDN controller deploys the rules in the SDN network.

STEP 6-7: Confirmation messages are sent back to the policy engine.

The policy engine will work in a standalone fashion, triggering itself regularly or based on the threshold over defined KPIs. In the future, the addition of an endpoint to manually enabling and disabling it will be evaluated.


## Developer guide
------------------

For developers command to start and stop the optimisation enabler is avaialable.

## Version control and release
------------------------------

Version 1.0.0. First release.
