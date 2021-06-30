# shelm - my Helm chart 

## This Helm chart is built from the helm template
```
helm create shelm
```
This will generate a template project.

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

#create the index.yaml
helm repo index --url https://sonamsamdupkhangsar.github.io/sonam-helm-chart/ .

helm install authserver sonam/mychart --set image.repository=sonamsamdupkhangsar/openid_connect_app


install locally
 helm install kecha ../../github/sonam-helm-chart -f values.yaml   

 dry run to generate yaml
  helm install kecha ../../github/sonam-helm-chart --version 0.1.3 -f values.yaml --dry-run 