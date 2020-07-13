# Service Provider and its underlying Mapping Concept

In short, the Service Provider is the part of our Prototype which is responsible for executing pre-defined functions using external APIs. Its responsibility is cleary decoupled from the Master Service which orchestrates the execution of a user routine (docu see [here](master_service.md)). In the context of the overall microservice architecture of our prototype, the Service Provider is accessed solely via the Master Service, mainly to execute functions, i.e. certain actions possibly having some input and returning some output. 

Since the Service Provider is so to say the implementation of our overall mapping concept for dynamic adding (i.e. during runtime) of new functions and external APIs to execute those functions. Therefore, in order to be able to understand the architecture of the Service Provider it is indispensable to understand the underlying mapping concept, i.e. the way how functions and APIs are represented, first. Thus, this documentation in a first step explains how our Mapping Concept works and afterwards focusses on the technical implementation of this concept by the Service Provider.

The structure of this documentation is as follows: 
* [Underlying Mapping Concept](#underlying-mapping-concept)
* [Technical Implementation by the Service Provider](#technical-implementation-by-the-service-provider)
  * [Overall Architecture](#overall-architecture)
  * [Capabilities offered by the individual Endpoints](#capabilities-offered-by-the-individual-endpoints)
  * [Function Execution Flow](#function-execution-flow)
* [Appendix 1 - Exemplary Fixtures of Function and API Specifications](#appendix-1---exemplary-fixtures-of-function-and-api-specifications)
* [Appendix 2 - Source Code for main part of Function Execution](#appendix-2---source-code-for-main-part-of-function-execution) 

## Underlying Mapping Concept





## Technical Implementation by the Service Provider
### Overall Architecture

### Capabilities offered by the individual Endpoints

### Function Execution Flow

## Appendix 1 - Exemplary Fixtures of Function and API Specifications

## Appendix 2 - Source Code for main part of Function Execution 



