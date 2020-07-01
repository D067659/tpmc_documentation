# Documentation for the Team Project - Cluj Mannheim
> Documentation - Team Project Cluj Mannheim
> 
> We gonna discuss whether we add the documentation files (e.g. Overleaf files, powerpoints, ...) or just shift the existing docu to be this readme file (using >  markup).
> 
> General idea how the docu should look like (style of writing, granularity: https://github.com/corona-warn-app)


## Evaluation of Existing Platforms - Benachmarking against MTP Platform
### MTP Platform
Communication to other online services is key, especially in the field of automation. Chaining multiple communication responses and use the extracted data for other communicaiton requests is essential in the field of autonomous automation, meaning that no human-being is involved anymore.
This is what the created MTP Platform is all about. In more technical detail and to be more precise, the MTP Platform (referenced as MTPP) created an API-agnostic layer of communicaiton with other platform services. API-agnostic means, that the implementation of the chained communication is designed in a way, that the APIs themselves are exchangeable. Thus, the MTPP focuses heavily on a vast degree of great extensibility. Users are allowed to edit existing APIs, exchange them completely or use them in several use-cases, which also can be defined easily.


### Comparison to SAP Cloud Platform Integration
To validate any outcome, it is mandatory to evaluate the results to existing players on the market, serving a comparable service as the newly created one.
The same applies for creating *integration-communication* platforms. Several similar services were found and are compared based on several aspects in the following part.
SAP as the market leader of enterprise systems changed their strategy to become a cloud company. Therefore, they have an answer when it comes to cloud integration. The **SAP Cloud Platform Integration** (referenced as SCI) is the SAP solution for manage intregation between systems *(like secured enterprise systems)* or API-based services *(like a REST API, publicly accessible through the internet)*. Comparing the small introduction to both the MTPP and SCI, the high-level use-case of the platforms is rather different. Even though it is reasonable to have a look at the core concept behind SCI since the idea of connecting multiple objects is comparable to the MTPP concept.
A key aspect of both platforms is the *Modelling* of the desired functionality. Through the focus on extensibility and API-agnostic behavior, MTPP uses a simple dropdown with input field layout. The SCI in turn uses a variance of BPMN modelling language for their Integration Flows (an Integration Flow or IFlow is the most granular component for modelling a use-case in the SCI). While the flexibility is provided in MTPP out-of-the-box, one of the multiple pre-defined components in SCI is the Script module. The Groovy Script based template allows users to modify, analyze or extract the response content which was returned from a previous communcation. This forces the user to proactively develop the chaining API calls on his or her own. In the following, an example Integration Flow is illustrated.

![IFlow-Example](/resources/images/IFlow-Example.png)

It is mentionable that the SCI allows *OR* and *AND* conditions, just as the MTPP does, too.
One of the major advantages of SCI compared to MTPP is the out-of-the-box availability of hundreds of components. As mentioned earlier, the MTPP is about flexibility. It can be inferred that this leads to a lower amount of predefined delivered components. SAP, in turn, focuses more on the core components to be available, like the support of different protocols or advanced exception handling. The foucs on the business software is clearly noticeable and the strategy of the capability of the SCI is build around this emphasis. An advantage of this approach is the reduced failure possiblity. The main challenge of SAP is merely to reduce the possibility of failure in the mentioned Script component, where developers can apply custom logic. In MTPP, the developer enjoys total freedom of creating new APIs, modify existing ones (but with primary key restrictions) or build complete new use-cases which also can be used (and chained) in other use-cases. 
A major disadvantage comes up while discussing this advantage: As SCI is only a single service among a whole service portfolio, built around SAP's cloud solution, the SAP Cloud Platform, it focuses and almost restricts the use-cases on SAP products or SAP APIs. While it is absolutely possible to connect to public APIs, the majority of available components focuses on business processes instead of end-user requirements. 








