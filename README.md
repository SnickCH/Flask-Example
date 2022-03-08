# Flask Docker Tutorial

## Purpose
This Docker Flask tutorial is intended to show students on how to start. I will use this in my own class rooms. Feel free to use it at your own risk.

## What will be done

1. First you have to define the requirement.txt and the app.py. For this example you will get both and only need to copy the content.
2. We will create a docker file. For this example you only have to copy the content
3. We will cerate a local docker-image with the runtime for flask with your defined requirements
4. We will startup the container and mount on container creation your app.py into the container

At point 4. we could just exchange the app.py content and restart the container. So we can reuse the container, as long as the requirements are met. Because it is planned to do some changes there (as soon as the tutorial will be updated) we will use it like this. It also gives a good example on howto mount files into a container. For a productive app it would probably make more sense to coppy the app.py into the image - but this depends on the context.


## Docker
### Folder structure

|Folder| File  | Description   |
| ------------ | ------------ | ------------ |
|Main |   | This is the Mainfolder where we have our files   |
|Main |requirements.txt   |The Requirements that will be installed into the container image that are needed to run the Flask app. This is App specific  |
|Main |dockerfile  |The information needed for the docker build command, to build the container with all needed application. This is Python / Flask specific.|
|Main |app.py  |This is your app (in this case: hello-world)|

### Docker File

```Bash
#We will start from the official python:3 image
FROM python:3
#We define the directory where everything else will be run
WORKDIR /app
# We copy the requirements.txt so we can use this for the pip installation
COPY ./requirements.txt /app/requirements.txt
#We run pip to install all requirements that were defined in requirements.txt 
RUN pip install --no-cache-dir -r requirements.txt
#The entrypoint - we want to use phyton
ENTRYPOINT [ "python" ]
#The standard command that will be executed on container startup. We will start the "app.py" application
CMD [ "app.py" ]
```
Attention: 
The file "requirements.txt" must be in the same directory as where the build command will be run. If the file is not present, the build of the image won't work.

### Docker Build

We create now the local image "flask-tutorial"
```Bash
docker build -t flask-tutorial:latest .
```

We can check the image details with the following command
```Bash
docker images flask-tutorial
docker image inspect flask-tutorial
```

###  Docker run
We start now the container with the application

```Bash
docker run --name=flask --volume /path/to/the/app/app.py:/app/app.py:ro -d -p 5000:5000 flask-tutorial
```

If you need to stop and delete your container
```Bash
docker stop flask && docker rm flask
```


###  Docker test our application

We can now access the application in a browser (Firefox, Chrome etc.)
```Bash
http://host-ip:5000
```

## Needed files
## Requirement.txt File
This file is needed for the docker build command and defines the requirements that will be installed (into the container).

```Bash
Flask>=1.0.0
```

## App.py File (Hello World)
This is our hello world app.

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Flask: Hello World from Docker'

@app.route('/api')
def rest_hello_world():
    return '{"id":1,"message":"Flask: Hello World from Docker"}'

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')
```

