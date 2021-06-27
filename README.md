# shelm - my Helm chart 

## This Helm chart is built from the helm template
```
helm create shelm
```
This will generate a template project.

## add chart 
```helm repo add sonam https://sonamsamdupkhangsar.github.io/mychart/```

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
