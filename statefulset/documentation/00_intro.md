# Service Helm Chart
Deploys a generic service

## Automatic generated readme.md
The readme.md file is automatic generated from the documentation-folder using github actions.

## Installing
First add KvalitetsIT Helm repo to Helm
```console
$ helm repo add KvalitetsIT https://raw.githubusercontent.com/KvalitetsIT/helm-repo/master/
$ helm repo update
```

Create values.yaml file with the parameters specified  

Run Helm command:  
```console
$ helm install web-service KvalitetsIT/statefulset -f myValues.yaml --version 1.0.3
```
