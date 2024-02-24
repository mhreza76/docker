# Simple Web Application

This is a simple web application using [Python Flask](http://flask.pocoo.org/). 
  
# Run by using Docker

```sh
docker build . -t simple-web-app
docker run simple-web-app
```

# Run Manually   

  Below are the steps required to get this working on a base linux system.
  
  - Install all required dependencies
  - Start Web Server 
## 1. Install all required dependencies
  
  Python and its dependencies

  ```sh
  apt-get update
  apt-get install -y python3 pip
  ```
   
## 2. Install and Configure Web Server

Install Python Flask dependency

    pip install flask


## 3. Start Web Server

Start web server

    FLASK_APP=app.py flask run --host=0.0.0.0
    
## 4. Test

Open a browser and go to URL

    http://<IP>:5000                            => Welcome
    http://<IP>:5000/how%20are%20you            => I am good, how about you?
