# Master Service
The Master Service in the only contact point with the Frontend, besides the User Service for authentication. Its main responsibility is to compute a given routine, validate the outcome and send it back to the Frontend.
Since this is only a high-level described goal, it is reasonable to have a look at all the exposed endpoints of the Master Service in order to gain a holistic overview of the capabilities of the Master Service:

`/api/routine/`

`/api/functions`

`/api/apis/`

`/api/available_components` 

## Functionality explained by Endpoints
### /API/ROUTINE/
The routine endpoint is used by the Frontend after finishing the creation of a routine. It receives a JSON object of a routine. The most important parts of a routine are:
* The GUID of a routine
* Several components which describe a order and a logic for executing specific actions, like checking the weather (1. component) at a given time (2. component)
* A routine name for a better UI representation
* The user id belonging to the user who triggered the execution of a routine.
