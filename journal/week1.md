# Week 1 — App Containerization

## Required Homework

### Containerize Backend and Frontend applications separately with DockerFile

I created the Dockerfile's for both [Backend](https://github.com/aki23gup/aws-bootcamp-cruddur-2023/blob/main/backend-flask/Dockerfile) and [Frontend](https://github.com/aki23gup/aws-bootcamp-cruddur-2023/blob/main/frontend-react-js/Dockerfile) applications. Once created, the docker run/build commands succesfully launched the containers. 

### Orchestrate containerized Backend and Frontend applications together with Docker Compose

Since backend and frontend are seperate containers, we need a way to orchestrate between the two to have a functioning application. This is where Docker Compose steps in as it allows defining and running multi-container applications. 

I created the [Docker-Compose](https://github.com/aki23gup/aws-bootcamp-cruddur-2023/blob/main/docker-compose.yml) file, where the URL of both the containers are defined for it to run appropriately. Now, the two containers are orchestrated and work in tandem once the 'Docker Compose' cli command is run in the Gitpod environment.

### Develop the Notification Endpoint feature 
#### Flask Endpoint for Notifications
Wrote the backend endpoint for the Notifications page by adding [This](https://github.com/aki23gup/aws-bootcamp-cruddur-2023/commit/e987cfcbe60bd3e66a95e5f64a34f925d4583cd9) to the app.py file

#### React Page for Notifications

Implemented frontend for the Notifications page by adding [This](https://github.com/aki23gup/aws-bootcamp-cruddur-2023/commit/f2c787b82ee490d25f5a631182180b84dd1c55ca) to the app.js file

### Creating DynamoDB and Postgres Containers
