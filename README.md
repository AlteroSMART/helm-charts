# HELM-charts Repo
This is the HELM-charts repository.

## Create HELM-chart
I set up GitHub Pages to point to the `master branch /docs folder`.  
From there, I can create and publish docs like this:
```console
## Create chart-template
$ helm create tsdb
## Edit chart-files...
```

## Update HELM-repository
```console
## Make new chart-package
$ helm package tsdb
Successfully packaged chart and saved it to: /home/rasla/git/helm-charts/tsdb-0.2.5.tgz
$ mv tsdb-0.2.5.tgz docs/

$ helm repo index docs --url https://alterosmart.github.io/helm-charts

$ git add -i
$ git commit -m "helm: tsdb-0.2.5"
$ git push origin master
```

## Usage
### Configure HELM
```console
$ helm repo add au https://alterosmart.github.io/helm-charts
"au" has been added to your repositories
$ helm repo add stable https://kubernetes-charts.storage.googleapis.com/
"stable" has been added to your repositories

$ helm repo update
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "au" chart repository
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈ Happy Helming!⎈

$ helm search repo tsdb
NAME    CHART VERSION  APP VERSION  DESCRIPTION                                       
au/tsdb 0.2.5          0.7.0        The TimeSeries DataBase for AlteroUniversal (Cl...
```

### Configure & install CHART
```console
$ wget https://github.com/AlteroSMART/helm-charts/raw/master/config-ksvd-v1.example.yaml -O config-ksvd.local.yaml
##  Edit  config-ksvd.local.yaml

$ kubectl create namespace ksvd
namespace/ksvd created
$ helm install au-tsdb au/tsdb  -f config-ksvd.local.yaml  -n ksvd  --version 0.2.5

## au-secret.yaml - personal access to the `AlteroSMART Container Registry`
$ kubectl apply -f au-secret.yaml
$ helm install au au/ksvd  -f config-ksvd.local.yaml  -n ksvd

$ helm ls -A
NAME    NAMESPACE   REVISION   UPDATED                                  STATUS     CHART        APP VERSION
au      ksvd        1          2020-09-02 18:11:36.043607146 +0500 +05  deployed   ksvd-0.9.0   1.0.0      
au-tsdb ksvd        1          2020-09-02 18:10:59.528386484 +0500 +05  deployed   tsdb-0.2.5   0.7.0      

$ kubectl get po -A | egrep "READY|tsdb"
NAMESPACE  NAME                            READY   STATUS    RESTARTS   AGE
ksvd       au-tsdb-clickhouse-0            1/1     Running   0          11m
ksvd       au-tsdb-redis-8575c4c795-gv4ch  2/2     Running   0          10m
ksvd       au-tsdb-8554bd5776-sjccc        1/1     Running   0          10m
```

### Uninstall CHART
```console
$ helm uninstall -n ksvd au-tsdb
release "au-tsdb" uninstalled
```
