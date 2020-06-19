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
Successfully packaged chart and saved it to: /home/rasla/git/helm-charts/tsdb-0.2.0.tgz
$ mv tsdb-0.2.0.tgz docs/

$ helm repo index docs --url https://alterosmart.github.io/helm-charts

$ git add -i
$ git commit -m "helm: tsdb-0.2.0"
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
as/tsdb 0.2.0          0.7.0        The TimeSeries DataBase for AlteroUniversal (Cl...
```

### Configure & install CHART
```console
$ wget https://github.com/AlteroSMART/helm-charts/raw/master/config-ksvd-v1.example.yaml -O config-ksvd.local.yaml
##  Edit  config-ksvd.local.yaml

$ kubectl create namespace ksvd
namespace/ksvd created
$ helm install au-tsdb au/tsdb  -f config-ksvd.local.yaml  -n ksvd  --version 0.2.0

$ helm ls -A
NAME    NAMESPACE   REVISION   UPDATED                                  STATUS     CHART        APP VERSION
au-tsdb ksvd        1          2020-06-19 15:18:55.33723807 +0500 +05   deployed   tsdb-0.2.0   0.7.0      

$ kubectl get po -A | egrep "READY|tsdb"
NAMESPACE   NAME                     READY   STATUS    RESTARTS   AGE
ksvd        au-tsdb-clickhouse-0     1/1     Running   0          1m32s
```

### Uninstall CHART
```console
$ helm uninstall -n ksvd au-tsdb
release "au-tsdb" uninstalled
```
