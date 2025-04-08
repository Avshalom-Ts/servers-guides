# Create the helm chart [YouTube Guid](https://www.youtube.com/watch?v=jUYNS90nq8U&ab_channel=DevOpsJourney).

To start creating the helm chart for app passt the commands.

```bash
helm create webapp
```

This command tell Helm to create folder `webapp1` and inside the nesesary files like so:

```text
└── webapp1
    ├── charts
    ├── Chart.yaml // This file is for the data for the chart.
    ├── templates 
    │   ├── deployment.yaml
    │   ├── _helpers.tpl
    │   ├── hpa.yaml
    │   ├── ingress.yaml
    │   ├── NOTES.txt
    │   ├── serviceaccount.yaml
    │   ├── service.yaml
    │   └── tests
    │       └── test-connection.yaml
    └── values.yaml // The default configuration for the heml chart(Can be Deleted/Emty)
```

## helm install

```bash
helm install mywebapp-releas webapp/
```

## Install the first one

```bash
helm install mywebapp-release webapp1/ --values webapp1/values.yaml
```

## Upgrade after templating

```bash
helm upgrade mywebapp-release webapp1/ --values mywebapp/values.yaml
```

## Accessing it

```bash
minikube tunnel
```

## Create dev/prod

```bash
k create namespace dev
k create namespace prod
helm install mywebapp-release-dev webapp1/ --values webapp1/values.yaml -f webapp1/values-dev.yaml -n dev
helm install mywebapp-release-prod webapp1/ --values webapp1/values.yaml -f webapp1/values-prod.yaml -n prod
helm ls --all-namespaces
```
