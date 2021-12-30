# sonam-helm-chart - my Helm chart 
This Helm chart will deploy a Istio virtual service using a gateway.
It will also setup the Postgres secret file's username/password as envrionment variables.

When updating do the following
```
helm package .
helm repo index --url https://sonamsamdupkhangsar.github.io/sonam-helm-chart/ .
```
also remove the old helm chart

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

The following is a sample of deploying a app using ```values.yaml``` file
```
> helm install kecha sonam/mychart -f values.yaml            
NAME: kecha
LAST DEPLOYED: Wed Jul 21 15:52:39 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/name=mychart,app.kubernetes.io/instance=kecha" -o jsonpath="{.items[0].metadata.name}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl --namespace default port-forward $POD_NAME 8080:80
```


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


This Helm chart assumes a postgres-operator is used for deploying the Postgres database cluster.
I use this https://github.com/zalando/postgres-operator/blob/master/docs/quickstart.md#deployment-options for deploying a Postres operator.

To deploy postgres-operator:
```helm install postgres-operator ./charts/postgres-operator```

To deploy the postgres operator UI:
```helm install postgres-operator-ui ./charts/postgres-operator-ui```

To deploy a postgres db cluster:
```kubectl create -f manifests/minimal-postgres-manifest.yaml```

To see the postgresql cluster deployment:
```kubectl get postgresql```


To install istio:
```./istioctl install```

Personal docker registry:
To start a docker registry:
```docker run -d -p 5000:5000 --restart=always --name registry registry:2```
To stop the docker registry:
```docker container stop registry```

Check istio-demo project for gateway setup or use the following for setting up a gateway:
```
apiVersion: networking.istio.io/v1alpha3
kind: Gateway
metadata:
  name: gateway
spec:
  selector:
    istio: ingressgateway
  servers:
    - port:
        number: 80
        name: http
        protocol: HTTP
      hosts:
        - '*'
```

## port-forward k8 postgresql database master pod connection to local machine:

Get postgresql cluster name:

```
kubectl get postgresql
NAME                    TEAM    VERSION   PODS   VOLUME   CPU-REQUEST   MEMORY-REQUEST   AGE   STATUS
kecha-minimal-cluster   kecha   13        2      1Gi                                     38d   Running
```

Get master pod name of pgsql cluster PGMASTER variable:

```
export PGMASTER=$(kubectl get pods -o jsonpath={.items..metadata.name} -l application=spilo,cluster-name=kecha-minimal-cluster,spilo-role=master -n default)  
```

 Port-forward to port 6432 with the master pod  name:

```
kubectl port-forward $PGMASTER 6432:5432 -n default
Forwarding from 127.0.0.1:6432 -> 5432
Forwarding from [::1]:6432 -> 5432
```

###  Connecting to port-forwarded pgsql master-pod
I am using my generated secret file by the operator "sonam.kecha-minimal-cluster.credentials.postgresql.acid.zalan.do" to store the password in PGPASSWORD variable.

```
export PGPASSWORD=$(kubectl get secret sonam.kecha-minimal-cluster.credentials.postgresql.acid.zalan.do -o 'jsonpath={.data.password}' | base64 -d)
export PGSSLMODE=require
psql -U kecha -h localhost -p 6432
```

If you need to find the username too then just replace '.data.password' with '.data.username' field:
```
export PGUSER=$(kubectl get secret sonam.kecha-minimal-cluster.credentials.postgresql.acid.zalan.do -o 'jsonpath={.data.username}' | base64 -d)
``` 

If your username (sonam) and database (db=kecha) are different then specify them separately:
```
 psql -U sonam -d kecha -h localhost -p 6432 
 ```




