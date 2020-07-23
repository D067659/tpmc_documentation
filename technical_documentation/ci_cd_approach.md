# Continuous Integration and Continous Delivery
The project environment consists of multiple loosely coupled, independet microservices. This characteristic requires strict and disciplined orchestration of the whole context.
Therefore, the following automation mechanisms were introduced in terms of satisfying the CI/CD idea, to ease the deployment process and provide the latest features directly to the end-users:
* To ease the deployment and dependency management, all microservices are managed in an Docker container
* These docker images are linked to a Jenkins pipeline, which takes care of the continuous deployment on a third party server. Additionally, the
pipeline saves copies of the code on a regular basis in a backup GitHub repository
* The jenkins pipeline also validates the code quality by executing the automated tests, written once, for several use cases within each service
* The jenkins pipeline is triggered for running the deployment/build actions after each commit to the master branch
* To keep the code quality constantly at a maximum level, the direct commit to the master branch is restricted. It is necessary to create a Pull Request and have another developer review and actively approve the created code. All this is managed via GitHub directly.

Following these steps, a proper contribution towards the CI/CD philosophy is made. Moreover, it is a clean way of accomplishing and mastering the microservice architecture environment by automating as many aspects as possible.
