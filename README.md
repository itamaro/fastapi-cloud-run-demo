# fastapi-cloud-run-demo

Education project - learning to build & deploy a FastAPI app using Google Cloud Run

FastAPI demo app based on https://github.com/tiangolo/uvicorn-gunicorn-fastapi-docker

Google Cloud Run build & deploy based on https://cloud.google.com/run/docs/quickstarts/build-and-deploy/python

## Build & Run Locally

```
docker build -t my-fastapi-app .
docker run -it --rm -v $PWD/app -p 5000:80 my-fastapi-app /start-reload.sh
```

Access builtin API docs on http://localhost:5000/docs

Hit API using curl:

```
>curl -X 'GET' 'http://localhost:5000/' -H 'accept: application/json'
{"message":"Hello world! From FastAPI running on Uvicorn with Gunicorn. Using Python 3.9"}
```

## Build & Deploy On Google Cloud Run

```
export PROJECT_ID="$( gcloud config get-value project )"
gcloud builds submit --tag gcr.io/$PROJECT_ID/my-fastapi-app
gcloud run deploy fastapi-demo-app --image=gcr.io/$PROJECT_ID/my-fastapi-app --platform=managed --allow-unauthenticated
```

Access bulitin API docs on https://fastapi-demo-app-mrbe4zqmdq-uw.a.run.app/docs

Hit API using curl:

```
>curl -X 'GET' 'https://fastapi-demo-app-mrbe4zqmdq-uw.a.run.app/' -H 'accept: application/json'
{"message":"Hello world! From FastAPI running on Uvicorn with Gunicorn. Using Python 3.9"}
```

## Require Auth

```
export PROJECT_ID="$( gcloud config get-value project )"
gcloud builds submit --tag gcr.io/$PROJECT_ID/my-fastapi-app
gcloud run deploy fastapi-auth-demo-app --image=gcr.io/$PROJECT_ID/my-fastapi-app --platform=managed --no-allow-unauthenticated
```

Now curl gets 403:

```
>curl -i 'https://fastapi-auth-demo-app-mrbe4zqmdq-uw.a.run.app/' -H "Content-Type: application/json"
HTTP/2 403
date: Sun, 25 Apr 2021 18:16:40 GMT
content-type: text/html; charset=UTF-8
server: Google Frontend
content-length: 295
...
```

If we add ID token as bearer authorization header then it works again:

```
>curl 'https://fastapi-auth-demo-app-mrbe4zqmdq-uw.a.run.app/' -H "Content-Type: application/json" -H "Authorization: Bearer $(gcloud auth print-identity-token)"
{"message":"Hello world! From FastAPI running on Uvicorn with Gunicorn. Using Python 3.9"}
```
