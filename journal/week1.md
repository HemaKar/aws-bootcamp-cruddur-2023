# Week 1 — App Containerization

## Why to create containers?
### Easy to move the application, makes it portable without having to worry about configurations to the environment you deploy. The individual running the app can run it as good as locally. 

## BACKEND INSTALLATION & DOCKER FILE creation. 
-----------------------------------------------------
Installed the extension for docker in gitpod and VSCode. 

Steps to Contairize backend. 

1. Add a dockerfile
```
FROM python:3.10-slim-buster

#Inside Container
#make a new folder inside container
WORKDIR /backend-flask

# Outside Container ----> Inside Container
#this contains the libraries want to install to run the app
COPY requirements.txt requirements.txt

#Inside Container
#Install the python lib used for the app
RUN pip3 install -r requirements.txt

# Outside Container -- >Inside Container
# . means everything in the current directory
# first period . / backend-flask (outside container)
# second period . /backend- flask (inside container)
COPY . .

#Set Environment variables
# these get set inside the container and remain there when the container is running
ENV FLASK_ENV=development

EXPOSE ${PORT}

# Command
# this is the command used to run flask in your application
# python3 -m flask run --host =0.0.0.0 (everything address, by default it runs on 27.0.0.0 but containers need to be 0.0.0.0 port=4567(flask))
CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=4567"]

```
2. As per application you create containers, in this case it is python.app
Run python
```
cd backend-flask
export FRONTEND_URL="*"
export BACKEND_URL="*"
python3 -m flask run --host=0.0.0.0 --port=4567
cd ..
```

3. Check port '4567' in ports tab to be opened. You unlock the port by clicking on the lock button. 
4. Try clicking on the link, it gave error 404. Server was running but the page was not found. 
5. To resolve create an endpoint by adding /api/activities/home

```curl -X GET http://localhost:4567/api/activities/home```

6. To run the application we need environment variables and configure the app by using the following code

```
export FRONTEND_URL="*"
export BACKEND_URL="*"
```
7. Run the following cmd to check if the python app runs

```
python3 -m flask run --host=0.0.0.0 --port=4567
```
8. Hit refresh in the browser link. The backend data populates. 
9. Ctrl + C (quit)
10. ## Build Container
To build a container you need to be in the root directory and run the following command 
```
docker build -t  backend-flask ./backend-flask
```
-t stands for tag. 
./ backend-flask means looks in current directory. 
By default docker gives "latest" tag if not explicitly set. 
11. The images in gitpod for the backend is listed in the left side corner and can be seen by clicking on the "fish" symbol.
12. The images can also be listed by using the command 
```
$ docker images
```
13. ## Run container
Run the following command 
```
docker run --rm -p 4567:4567 -it backend-flask
FRONTEND_URL="*" BACKEND_URL="*" docker run --rm -p 4567:4567 -it backend-flask
export FRONTEND_URL="*"
export BACKEND_URL="*"
docker run --rm -p 4567:4567 -it -e FRONTEND_URL='*' -e BACKEND_URL='*' backend-flask
docker run --rm -p 4567:4567 -it  -e FRONTEND_URL -e BACKEND_URL backend-flask
```
Make sure the port is running and active. IF not quit and run the commanc again. 
-d is used to run the command in background. 

```
$ docker ps
```
lists all the docker images.

14. Use commands ```compose up``` and ``` compose down``` to open or shutdown the containers. 

## FRONT_END Installations
----------------------------

1. Run NPM install using the following code

```
cd frontend-react-js
npm i

```
2. Create a dockerfile ``` frontend-react-js/Dockerfile ```and paste the following code 

```
FROM node:16.18

ENV PORT=3000

COPY . /frontend-react-js
WORKDIR /frontend-react-js
RUN npm install
EXPOSE ${PORT}
CMD ["npm", "start"]
```
3. Build Container

```
docker build -t frontend-react-js ./frontend-react-js
```

4. Run Container 

```
docker run -p 3000:3000 -d frontend-react-js
```

### Docker - compose
---------------------

Is used to run multiple containers
1. Create docker-compose.yml (right click ->create new file-> name "docker-compose.yml"
2. Run the following command 

```
version: "3.8"
services:
  backend-flask:
    environment:
      FRONTEND_URL: "https://3000-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
      BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
    build: ./backend-flask
    ports:
      - "4567:4567"
    volumes:
      - ./backend-flask:/backend-flask
  frontend-react-js:
    environment:
      REACT_APP_BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
    build: ./frontend-react-js
    ports:
      - "3000:3000"
    volumes:
      - ./frontend-react-js:/frontend-react-js

# the name flag is a hack to change the default prepend folder
# name when outputting the image names
networks: 
  internal-network:
    driver: bridge
    name: cruddur
    
```
    
The use of docker compose is to run multiple containers at the same time. In this case it is the frontend and backend containers. 

3. Port 3000 is for frontend. Make sure it is unlocked.
4. Backend port 4567 unlocked
5. Click the frontend link and the application was launched. 


### Frontend Notification
![Front end Notification1](https://github.com/HemaKar/aws-bootcamp-cruddur-2023/blob/main/journal/assets/Frontend_notification.PNG))

### Backend Notification


### Postgres and Dynamo DB local 
1. Run containers
2. Open docker-compose.yml file
3. In docker-compose.yml under "Services"
4. POSTGRES
```
  db:
    image: postgres:13-alpine
    restart: always
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
    ports:
      - '5432:5432'
    volumes: 
      - db:/var/lib/postgresql/data
```
5. DynamoDB local:
```
 dynamodb-local:
    # https://stackoverflow.com/questions/67533058/persist-local-dynamodb-data-in-volumes-lack-permission-unable-to-open-databa
    # We needed to add user:root to get this working.
    user: root
    command: "-jar DynamoDBLocal.jar -sharedDb -dbPath ./data"
    image: "amazon/dynamodb-local:latest"
    container_name: dynamodb-local
    ports:
      - "8000:8000"
    volumes:
      - "./docker/dynamodb:/home/dynamodblocal/data"
    working_dir: /home/dynamodblocal
```
6.Docker -> compose up

