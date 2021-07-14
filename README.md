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

another way to generate yamls for debug
  helm template -n stage kechaapp ../../github/sonam-helm-chart --version 0.1.4 -f values.yaml --debug

### port-forward from a service
> kubectl get svc
kecha-mychart                  ClusterIP   10.110.171.228   <none>        80/TCP     5m28s

>kubectl port-forward service/kecha-mychart 8180:80

After applying the virtualservice, you can access the kecha service  curl -v http://localhost
istio-demo % curl -v http://localhost
*   Trying ::1...
* TCP_NODELAY set
* Connected to localhost (::1) port 80 (#0)
> GET / HTTP/1.1
> Host: localhost
> User-Agent: curl/7.64.1
> Accept: */*
> 
< HTTP/1.1 200 OK
< x-content-type-options: nosniff
< x-xss-protection: 1; mode=block
< cache-control: no-cache, no-store, max-age=0, must-revalidate
< pragma: no-cache
< expires: 0
< x-frame-options: DENY
< content-type: text/html;charset=UTF-8
< content-language: en
< date: Tue, 06 Jul 2021 04:41:48 GMT
< x-envoy-upstream-service-time: 11
< server: istio-envoy
< transfer-encoding: chunked
< 
