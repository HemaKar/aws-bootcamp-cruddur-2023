# Week 1 — App Containerization

## Why to create containers?
### Easy to move the application, makes it portable without having to worry about configurations to the environment you deploy. The individual running the app can run it as good as locally. 

## Docker file creation in backend. 
---------------------------------
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

```python3 -m flask run --host=0.0.0.0 --port=4567```
8. Hit refresh in the browser link. The backend data populates. 
