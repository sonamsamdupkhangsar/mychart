# sonam-helm-chart - my Helm chart 
This Helm chart will deploy app using a Nginx Ingress controller on a Kubernetes cluster.

This chart can also support creation of environment variables for a postgres deployment.

For users, add the following chart to your environment:

```helm repo add sonam https://sonamsamdupkhangsar.github.io/sonam-helm-chart/```

To deploy:

``` helm install kecha sonam/mychart -f values.yaml```
 

## The following instructions are for local development and debugging of this Helm chart purposes only.

To remove previous Helm charts under the name 'sonam' `helm repo remove sonam`

The following instruction for updating this Helm chart.  Run the following:
``
helm package .
helm repo index --url https://sonamsamdupkhangsar.github.io/sonam-helm-chart/ .
```

install locally
```helm install kecha ../../github/sonam-helm-chart -f values.yaml```   

 dry run to generate yaml
```helm install kecha ../../github/sonam-helm-chart --version 0.1.3 -f values.yaml --dry-run ```

another way to generate yamls for debug
 ```helm template -n stage kechaapp ../../github/sonam-helm-chart --version 0.1.4 -f values.yaml --debug```
