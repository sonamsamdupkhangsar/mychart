# shelm - my Helm chart 

## add chart 
```helm repo add sonam https://sonamsamdupkhangsar.github.io/mychart/```

## deploy default app within chart
Helm deploy with auto generated name for package
```helm install sonam/mychart --generate-name```

Helm deploy with package name
```helm install example1 sonam/mychart```

#create the index.yaml
helm repo index --url https://sonamsamdupkhangsar.github.io/mychart/ .
