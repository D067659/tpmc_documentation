# User Service

The User Service's only responsibility is to handle the authenticate of the user at the frontend with the other micro services namely Master Service and Service Provider. Processwise the Frontend is logging into the User Service and receives a token which it can use for the other services. The other services validate the token at the user service and receive the user's id to retrieve information linked to the user.

## Endpoints
To provide this service two endpoints exist:

`/login/`

The login endpoint receives a post request with the username and password. It returns a token. The Frontend will make the login request and hand the tokens to the other services.

`/api/auth/`

The auth endpoint receives a get request without content but the token authentication in the header. If the token was valid it returns a successful reponse and the user id of the token. This way the service can verify that the user is authenticated directly. The user id can be used as a foreign key to retrieve user specific data.

## Implementation
The login service is provided by Django Restframework's (DRF) Basic Authentication and User model. The authentication via the auth endpoint is done by the DRF's TokenAuthentication where the user id is retrieved from the user atteched to the DRF request. The users are saved with hashed passwords in a dedicated postgreSQL database.
