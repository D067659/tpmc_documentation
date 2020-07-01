# Documentation for the Team Project - Cluj Mannheim
Documentation - Team Project Cluj Mannheim

We gonna discuss whether we add the documentation files (e.g. Overleaf files, powerpoints, ...) or just shift the existing docu to be this readme file (using markup).

General idea how the docu should look like (style of writing, granularity: https://github.com/corona-warn-app)


## Evaluation of Existing Platforms - Benachmarking against MTP Platform
### MTP Platform
Communication to other online services is key, especially in the field of automation. Chaining multiple communication responses and use the extracted data for other communicaiton requests is essential in the field of autonomous automation, meaning that no human-being is involved anymore.
This is what the created MTP Platform is all about. In more technical detail and to be more precise, the MTP Platform (referenced as MTPP) created an API-agnostic layer of communicaiton with other platform services. API-agnostic means, that the implementation of the chained communication is designed in a way, that the APIs themselves are exchangeable. Thus, the MTPP focuses heavily on a vast degree of great extensibility. Users are allowed to edit existing APIs, exchange them completely or use them in several use-cases, which also can be defined easily.


### Comparison to SAP Cloud Platform Integration
To validate any outcome, it is mandatory to evaluate the results to existing players on the market, serving a comparable service as the newly created one.
The same applies for creating *integration-communication* platforms. Several similar services were found and are compared based on several aspects in the following part.
SAP as the market leader of enterprise systems changed their strategy to become a cloud company. Therefore, they have an answer when it comes to cloud integration. The **SAP Cloud Platform Integration** (referenced as SCI) is the SAP solution for manage intregation between systems *(like secured enterprise systems)* or API-based services *(like a REST API, publicly accessible through the internet)*. Comparing the small introduction to both the MTPP and SCI, the high-level use-case of the platforms is rather different. Even though it is reasonable to have a look at the core concept behind SCI since the idea of connecting multiple objects is comparable to the MTPP concept.
