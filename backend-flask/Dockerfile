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