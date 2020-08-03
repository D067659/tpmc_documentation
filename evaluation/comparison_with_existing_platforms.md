# Evaluation of Existing Platforms - Benchmarking against MTP Platform
## MTP Platform
Communication to other online services is key, especially in the field of automation. Chaining multiple communication responses and use the extracted data for other communicaiton requests is essential in the field of autonomous automation, meaning that no human-being is involved anymore.
This is what the created MTP Platform is all about. In more technical detail and to be more precise, the MTP Platform (referenced as MTPP) created an API-agnostic layer of communicaiton with other platform services. API-agnostic means, that the implementation of the chained communication is designed in a way, that the APIs themselves are exchangeable. Thus, the MTPP focuses heavily on a vast degree of great extensibility. Users are allowed to edit existing APIs, exchange them completely or use them in several use-cases, which also can be defined easily.

To validate any outcome, it is mandatory to evaluate the results to existing players on the market, serving a comparable service as the newly created one.
The same applies for creating *integration-communication* platforms. Several similar services were found and are compared based on several aspects in the following part. This comparison is explicitly focused on the end-user perspective and not on a technical one. This decision was made to provide the reader a better understanding on the capabilities and use-cases of the platforms, which is a more valuable insights compared to a simply code-based evaluation or the pure focus on technical ways of solving a problem.

## Comparison to SAP Cloud Platform Integration
SAP as the market leader of enterprise systems changed their strategy to become a cloud company. Therefore, they have an answer when it comes to cloud integration. The **SAP Cloud Platform Integration** (referenced as SCI) is the SAP solution for manage intregation between systems *(like secured enterprise systems)* or API-based services *(like a REST API, publicly accessible through the internet)*. Comparing the small introduction to both the MTPP and SCI, the high-level use-case of the platforms is rather different. Even though it is reasonable to have a look at the core concept behind SCI since the idea of connecting multiple objects is comparable to the MTPP concept.
A key aspect of both platforms is the *Modelling* of the desired functionality. Through the focus on extensibility and API-agnostic behavior, MTPP uses a simple dropdown with input field layout. The SCI in turn uses a variance of BPMN modelling language for their Integration Flows (an Integration Flow or IFlow is the most granular component for modelling a use-case in the SCI). While the flexibility is provided in MTPP out-of-the-box, one of the multiple pre-defined components in SCI is the Script module. The Groovy Script based template allows users to modify, analyze or extract the response content which was returned from a previous communcation. This forces the user to proactively develop the chaining API calls on his or her own. In the following, an example Integration Flow is illustrated.

![IFlow-Example](/resources/images/IFlow-Example.png)

It is mentionable that the SCI allows *OR* and *AND* conditions, just as the MTPP does, too.
One of the major advantages of SCI compared to MTPP is the out-of-the-box availability of hundreds of components. As mentioned earlier, the MTPP is about flexibility. It can be inferred that this leads to a lower amount of predefined delivered components. SAP, in turn, focuses more on the core components to be available, like the support of different protocols or advanced exception handling. The foucs on the business software is clearly noticeable and the strategy of the capability of the SCI is build around this emphasis. An advantage of this approach is the reduced failure possiblity. The main challenge of SAP is merely to reduce the possibility of failure in the mentioned Script component, where developers can apply custom logic. In MTPP, the developer enjoys total freedom of creating new APIs, modify existing ones (but with primary key restrictions) or build complete new use-cases which also can be used (and chained) in other use-cases.
A major disadvantage comes up while discussing this advantage: As SCI is only a single service among a whole service portfolio, built around SAP's cloud solution, the SAP Cloud Platform, it focuses and almost restricts the use-cases on SAP products or SAP APIs. While it is possible to connect to public APIs, the majority of available components focuses on business processes instead of end-user requirements.

To sum the comparison of SCI and MTPP up, both services follow a similar approach of connecting and chaining different events in a communication process. While MTPP focuses more on the capability of extensibility, the SCI is built on best-practise-components to suppor their business cloud strategy. MTPP has their benefits in the mentioned flexibility, which adresses end-user needs as they can decide on their own which APIs are integrated with an own degree of custom logic. While the SCI has a great portfolio of different services, its more business oriented and has a deviating focus of the target processes. Serving the SCI as an end-user product comes with a restriction in terms of limited capability, but not with a restriction in terms of technical limitation, as API-based communication is supported as well.

## Comparison to NodeRed (this is only a sketched Draft version and subject to change if s.o. has better insights!)
Keeping the naming convention of a *Flow* for clustering a activity, NodeRed is another integration platform which can be compared to MTPP. Having a look at the creation of a flow reveals its difference to the SCI and also to MTPP:

![NodeRedFlow-Example](/resources/images/NodeRedFlow-Example.png)

The approach of NodeRed is heavily based on a very technical layer. A real business, following the idea of creating value to the end-users is neglected on this platform. Focusing on exactly the technical integration allows the platform to scale towards huge capabilities in terms of extensibility. Both SCI and MTPP focusing on end-user tasks, their use-cases and concerns. NodeRed, instead, focusing on the properties of a technical integration flow. Another screenshot demonstrates this, as it depicts the properties of a randomly chosen entity:

![NodeRedFlow-Properties](/resources/images/NodeRedFlow-Properties.png)

No business-related information is given, but a server, a entity ID or the way of manipulating the payload. This is not what MTPP stands for. Besides the rigorous end-to-end focus, MTPP still answers the demand of developers to extend the platform using the MTPP Developer Suite. But even non-technical affine users are easily able to use the platform on their own with a proper intent to use the platform in a sophisticated manner.
Evaluation of Existing Platforms - Benchmarking against MTP Platform

## If this than that (IFTTT)
The features of IFTTT have a great similarity to the features of the MTPP. The concepts of Condition, Operation and Routine are recoginzable although with some differences such as limited complexity and less customizable conditions. The user interface for discovering services and defining basic routines is designed simple and very useable.
The main concept is that provider of application services have to submit their integration to the IFTTT platform and pay for the availability.
Application services cannot be integrated by users and the service's API has to be modified according to the IFTTT standards.
The integration has to be driven by the application provider which is promised to increase their "customer engagement" and "expand functionality" by integrating with more than 600 brands [[2]].

### Components
**Triggers** –
Triggers are components that that are trigger when some kind of event occurs. A Trigger can be specified with additional filters called Trigger Fields so that the Trigger is limited to only some events. The trigger can standardwise return some content of the event that caused the trigger but the user can also specify additional information called query is returned. Triggers can be compared to *Condition* in the MTP Platform. IFTTT has an additional feature that allows Triggers to return extra information when it is triggered. Though the MTP Platform allows completely custom definition of conditions by using the output of another component as a field in the condition.[[1]]

**Actions** – Actions perform actions or retrieve data at one of the services. The user can specify the fields of the performed action. Likewise the Action is comparable to MTPP's concept of *Operation*.[[1]]
### Service Configuration
IFTTT will match each Trigger or Action with one API that the application provides. IFTTT provides reqiurements that the application's API have to fulfill which means that the endpoints have to be created specifically for IFTTT. IFTTT offers tools and tutorials of how to build a layer that translates between existing APIs and the IFTTT specific API. Additonally IFTTT offers a testing tool to verify the configuration is correct [[1]]. It is important to note that the MTPP supports most of the already existing external APIs and does not need a specific API endpoints.

Although IFTTT supports complex combinations of different components (called routine in MTPP) only paying organizations are allowed to create those and publish them for users. Creating these routines also requires developer tools. User are only allowed to combine one trigger without queries with one action in the simple user interface. However, IFTTT provides the functionality and platform for partners to publish predefined routines easily for the whole community. [[3]]

### Availability
Like the MTPP, IFTTT also pays attention to the availability of the services, however its implementation is different. IFTTT does not have multiple APIs for one service so that if one is unavailable the other can be used interchangeably.
First, IFTTT uses a status endpoint to check if the services are available which is polled periodically. If a service is unavailable too often IFTTT automatically deactivates the corresponding routines [[1]].
IFTTT requires the providers of the API to return multiple last event so that in case one event was missed it can be handled later. Further it supports webhook call-back requests for instant triggering of events. It supports webhook call-back request and polling of multiple recent events for triggers[[4]].

## Conclusion
### Comparison
After looking at different competing platforms we want to give a general resume on how the MTPP compares to these platforms.

All platforms allow some kind of routine representation but allows different degress of complexity and different representation. SCI and NodeRed allow more complex routines which are displayed similar to a process model. MTPP and IFTTT only allow simple routines which are represented as a textual list. MTPP's and IFTTT's Routine model consist of only two simple types of components, condition and operation. The other two platforms have a larger of number of complex components. In the end SCI and NodeRed allow more complex routines but are also more complicated to define by users.

All platforms come with predefined operations that integrate other webservices into the plattform. MTPP, NodeRed and SCI allow custom integration of new webservices which is not allow with IFTTT. In IFTTT providers of a webservices must request and prepare the integration to the platform but receive some support.
MTPP is the only platform that allows the built-in definition of alternative APIs for operations so that unavailable APIs can be replaced.

### Features List

1. Create Routines of Conditions and Operations
2. Automatic Routine starting based on Timers
3. Definition of Operation
4. Definition of multiple APIs for Operations that cover most types of API calls
5. Pre-defined Conditions and Operations including compatible APIs
6. User sign up service

### Features yet to be implemented
1. Graphical user interface for routine creation
2. More complex Routine model
3. Testing and validation tool for the definition of APIs and Routines
4. Constantly polling triggers
5. Push events by API providers
6. Handling of unavailable operations through recent event lists or waiting

[1]: https://platform.ifttt.com/docs
[2]: https://platform.ifttt.com/blog/product_overview
[3]: https://platform.ifttt.com/docs/connections
[4]: https://platform.ifttt.com/docs/api_reference
