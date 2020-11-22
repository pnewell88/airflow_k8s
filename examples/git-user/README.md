# Adding a github sidecar to airflow
The goal here is to have the production Airflow automatically pick up new dags from the main branch of 
a shared dags repo.


### Deploying this on EKS or microk8s clusters

Step 1: add the stable airflow helm chart...
`helm3 repo add airflow-stable https://airflow-helm.github.io/charts`

Step 2: update helm
`helm3 repo update`

Step 3: install your chart

Examples given from inside this folder, else you'll need to map the whole path to this custom-values.yaml

```
helm3 install {release name} airflow-stable/airflow --namespace {ns} --values {path to values file}

example:
helm3 install airflow-prod airflow-stable/airflow --namespace airflow-prod --values custom-values.yaml
```

Give it a few minutes to spin everything up, then push some example dags to your repo and watch them
appear on the Airflow UI, available at `kubectl get svc -n airflow-prod` -> find the webserver service
and follow the url:port to the UI. Port forwarding might be necessary