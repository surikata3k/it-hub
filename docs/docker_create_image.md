# Create a Docker Image Using a Dockerfile
### Create dockerfile
```
FROM python:alpine3.17
WORKDIR /app
COPY . /app
RUN pip install -r requirements.txt
ENTRYPOINT ["python", "app.py"]
```

### Build the image
```
docker build . -t hello-python:1.0
```

### List images
```
docker images
```

### Run
```
docker run -d -p 9998:9999 hello-python:1.0
```


# Create a Docker Image from a Container
### 1. Create a container of the base image
```
docker run -itd --name python-base python:alpine3.17
```

### 2. Copy the required files into the container
```
docker cp app.py python-base:/
docker cp requirements.txt python-base:/
```

### 3. Install the required dependencies
```
docker exec -it python-base pip install -r requirements.txt
```

### 4. Create the image of the container
```
docker commit --change='ENTRYPOINT ["python","app.py"]' python-base python-hello:1.0
```

### 5. Run
```
docker run -itd -p 9998:9999 python-hello:1.0
```


# Files

### Content of requirements.txt
```
flask
```

### Content of app.py
```
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello World Python Program.'

if __name__ == "__main__":
   app.run(host="0.0.0.0", port=9999)
```

