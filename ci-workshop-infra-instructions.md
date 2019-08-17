Infrastructure
===================

1. Create a gcp service account
2. Create a service account and grant write access to Storage buckets. Download the API key (JSON)
3. Run using Docker

    export GCLOUD_SERVICE_KEY_ENC=$(cat <PATH_TO_SECRET_JSON> | base64)
    export GCP_STORAGE_BUCKET=<GS_BUCKET_NAME> # exclude gs:// prefix

    docker pull arunma/mlflow-gcp

    export EXPERIMENT_NAME=<NAME_OF_THE_EXPERIMENT>

    docker run --name mlflow -it -P \
    -e GCLOUD_SERVICE_KEY_ENC=$GCLOUD_SERVICE_KEY_ENC \
    -e GCP_STORAGE_BUCKET=$GCP_STORAGE_BUCKET \
    -e EXPERIMENT_NAME=$EXPERIMENT_NAME \
    -p 5000:5000 \
    arunma/mlflow-gcp:latest

4. Run using GCP
    

    gcloud container clusters create my-cluster --region australia-southeast1 --num-nodes 1
    gcloud container clusters get-credentials my-cluster --region australia-southeast1

    export GCLOUD_SERVICE_KEY_ENC=$(cat <PATH_TO_SECRET_JSON> | base64)
    export GCP_STORAGE_BUCKET=<GS_BUCKET_NAME> # exclude gs:// prefix
    export EXPERIMENT_NAME=<NAME_OF_THE_EXPERIMENT>


    git clone https://github.com/arunma/mlflow-gcp.git
    go to the cloned library mlflow-gcp
    source ./populate_secret.sh

    This script just replaces the GCLOUD_SERVICE_KEY_ENC and GCP_STORAGE_BUCKET variables in mlflow-gcp-secret.yaml.template file with the values from the local environment variable.Also creates the Secret in Kubernetes


    Create Deployment and service

        kubectl create -f mlflow-gcp-deployment.yaml
        kubectl create -f mlflow-gcp-service.yaml

    To get the public IP of your kubernetes cluster, run  the following after 2 minutes or so
        kubectl get service 


5. Go to forked ci-workshop-app

6. Once MLFlow is provisioned, go to src/settings.py and:

7. Replace the mlflow tracking server URL

8. set SHOULD_USE_MLFLOW=True



