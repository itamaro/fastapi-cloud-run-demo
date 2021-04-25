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
