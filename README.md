# sonam-helm-chart
<img src="https://helm.sh/img/helm.svg" width="100"/>  

This Helm chart will deploy app using a Nginx Ingress controller on a Kubernetes cluster.

Please see the sample-values.yaml file included in this chart for a sample.

This chart supports creating environment variables
and secret environment variables using GITHUB secrets as envrionment variables such as following example:
``` 
--set "secretenvs[0].name=PACT_BROKER_BASIC_AUTH_USERNAME" --set "secretenvs[0].value=${{ secrets.PACT_BROKER_BASIC_AUTH_USERNAME }}" \
            --set "secretenvs[1].name=PACT_BROKER_BASIC_AUTH_PASSWORD" --set "secretenvs[1].value=${{ secrets.PACT_BROKER_BASIC_AUTH_PASSWORD }}" \
```

This chart can also support creation of environment variables for a postgres deployment.

For users, add the following chart to your environment:

```helm repo add sonam https://sonamsamdupkhangsar.github.io/sonam-helm-chart/```

To deploy:

``` helm install kecha sonam/mychart -f values.yaml```
 

## 0.1.15 version supports Oauth2-proxy
This 0.1.15 now supports OAuth2 proxy setup to secure applications.  See `values-echo-oauth-backend.yaml` for how to 
use this feature.


## The following instructions are for local development and debugging of this Helm chart purposes only.

To remove previous Helm charts under the name 'sonam' `helm repo remove sonam`

The following instruction for updating this Helm chart.  Run the following:
```
helm package .
helm repo index --url https://sonamsamdupkhangsar.github.io/sonam-helm-chart/ .
```

install locally
```helm install kecha ../../github/sonam-helm-chart -f values.yaml```   

 dry run to generate yaml
```helm install kecha ../../github/sonam-helm-chart --version 0.1.3 -f values.yaml --dry-run ```

another way to generate yamls for debug
 ```helm template -n stage kechaapp ../../github/sonam-helm-chart --version 0.1.16 -f values.yaml --debug```

 
 ```
helm upgrade --install --timeout 5m0s \
            --set "image.repository=jmalloc/echo-server" \
            --set "image.tag=latest" \
            --set "project=echo" \            
            echo \
            sonam/mychart -f values-echo-oauth-backend.yaml --version 0.1.16 --namespace=backend --dry-run
```