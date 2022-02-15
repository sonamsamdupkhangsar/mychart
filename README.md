# sonam-helm-chart - my Helm chart 
This Helm chart will deploy a Istio virtual service using a gateway.
It will also setup the Postgres secret file's username/password as envrionment variables.

When updating do the following
 helm repo remove sonam

```
helm package .
helm repo index --url https://sonamsamdupkhangsar.github.io/sonam-helm-chart/ .
```
also remove the old helm chart
 helm repo remove sonam 
 
## This Helm chart is built from the helm template
```
helm create shelm
```
This will generate a template project.

Build the index.yaml:
``` helm repo index --url https://sonamsamdupkhangsar.github.io/sonam-helm-chart/ .```

Deploy chart using sonam-helm-chart 
``` helm install kecha sonam/mychart -f values.yaml```

## add chart 
```helm repo add sonam https://sonamsamdupkhangsar.github.io/sonam-helm-chart/```


## Deploy default app within chart
Helm deploy with auto generated name for package
```
helm install sonam/mychart --generate-name
```

Helm deploy with package name
```
helm install example1 sonam/mychart
```

install locally
```helm install kecha ../../github/sonam-helm-chart -f values.yaml```   

 dry run to generate yaml
```helm install kecha ../../github/sonam-helm-chart --version 0.1.3 -f values.yaml --dry-run ```

another way to generate yamls for debug
 ```helm template -n stage kechaapp ../../github/sonam-helm-chart --version 0.1.4 -f values.yaml --debug```




