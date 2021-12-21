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
## Configuration
The following table, lists the configurable parameters.

Parameter | Description                                                                                                                                             | Example
--- |---------------------------------------------------------------------------------------------------------------------------------------------------------| ---
**[Basics](#adding-basics)** |
`fullnameOverride` | Name of the service                                                                                                                                     
`namespace` | Namespace to deploy into                                                                                                                                
`podSecurityContext` | podSecurityContext                                                                                                                                      
`podAnnotations` | Annotations for the pod fx prometheus                                                                                                                   | `prometheus.io/path: actuator prometheus` <br> `prometheus.io/scrape: "true"` <br>
`serviceAccountName` | serviceAccountName                                                                                                                                      
`imagePullSecrets` | imagePullSecrets                                                                                                                                        
`image.repository` | Name of the web-service image                                                                                                                           
`image.tag` | Web-service image tag                                                                                                                                   
`autoscaling.enabled` | Enable autoscaling                                                                                                                                      | `true` or `false`
`replicaCount` | Number of replicas - Not used if autoscaling is enabled                                                                                                 | `1`, `2`, `3` ..
`statefulsetStrategy` | Enables to set statefulset strategy                                                                                                                     | `Recreate`
`affinity`                          | Affinity for pod assignment.                                                                                                                            |
`podAffinityPreset`                  | Pod affinity preset. Ignored if `affinity` is set. Allowed values: ``, `soft` or `hard`                                                                 | `""`                         |
`podAntiAffinityPreset`              | Pod anti-affinity preset. Ignored if `affinity` is set. Allowed values: ``,`soft` or `hard`                                                             | `soft`                       |
`nodeAffinityPreset.type`            | Node affinity preset type. Ignored if `affinity` is set. Allowed values: `soft` or `hard`                                                               | `""`                         |
`nodeAffinityPreset.key`             | Node label key to match Ignored if `affinity` is set.                                                                                                   | `""`                         |
`nodeAffinityPreset.values`          | Node label values to match. Ignored if `affinity` is set.                                                                                               | `[]`
&nbsp; |
**[StatefulSet](#adding-statefulset)** |
`statefulset.enabled` | Enables the statefulset                                                                                                                                 | `true` or `false`
`statefulset.hostAliases` | hostAliases                                                                                                                                             |
`statefulset.containerPort` | Port on web-service                                                                                                                                     | `8080`
`statefulset.extraContainerPort` | Extra port on statefulset                                                                                                                               | `Port1: 8051`
`statefulset.readinessProbe` | Set values under this to config readiness probe                                                                                                         
`statefulset.livenessProbe` | Set values under this to config liveness probe                                                                                                          
`statefulset.commands` | List of cronjob commands                                                                                                                                | `- /bin/bash`
`statefulset.args` | List of arguments to the commands                                                                                                                       |
**StatefulSet - Environment variables** |
`statefulset.env` | Map of environment variables                                                                                                                            
`statefulset.env.{name}` | Name of the environment variables                                                                                                                       
`statefulset.env.{name}.value` | Value of the environment variable                                                                                                                       
`statefulset.env.{name}.type` | Optional: specify type of the environment variables if it should be read from a secret or configmap.<br> One of fieldPath, secretKeyRef, configMapKeyRef | `type: secretKeyRef`
`statefulset.env.{name}.fieldPath` | Optional: Value of fieldPath                                                                                                                            
`statefulset.env.{name}.name` | Optional: Name of the SecretKeyRef or ConfigMapKeyRef                                                                                                   
`statefulset.env.{name}.key` | Optional: Key for the SecretKeyRef or ConfigMapKeyRef                                                                                                   
`statefulset.envFrom` | Map of environment variables                                                                                                                            
`statefulset.envFrom.configMapRef` | List of ConfigMaps to read environment variables -from                                                                                                  | <code>configMapRef:<br>&nbsp;&nbsp;- my-configmap</code>
**StatefulSet - PVC** |
`statefulset.pvc.mountPath` | mountPath for the persistent volume                                                                                                                |
`statefulset.pvc.accessMode` | Accessmode of pvc. One of ReadWriteOnce, ReadOnlyMany or ReadWriteMany. Defaults to ReadWriteOnce                                                       |
`statefulset.pvc.request` | Storage request for pvc. Defaults to 2Gi                                                                                                                |
`statefulset.pvc.storageclass` | Storageclass for pvc                                                                                                                                    |
&nbsp; |
**[Ingress](#adding-ingress)** |
`ingress.enabled` | Set to false to disable ingress                                                                                                                         | `false`
`ingress.annotations` | Annotations for ingress                                                                                                                                 | `kubernetes.io/ingress.class: nginx`
`ingress.hosts` | Hosts served by the ingress                                                                                                                             | `- host: domain.dk`
`ingress.tls` | TLS config                                                                                                                                              
`extraIngress` | Map of extra ingress                                                                                                                                    
`extraIngress.{name}` | Place ingress values under the name value                                                                                                               
&nbsp; |
**[Service](#adding-service)** |
`service.enabled` | Set to false to disable service                                                                                                                         | `false`
`service.port` | Port on the service                                                                                                                                     | `8080`
`service.targetPort` | Target port                                                                                                                                             | `proxy-port`
`service.annotations` | Annotations for service                                                                                                                                 | `prometheus.io/path: /manage/actuator/appmetrics`
&nbsp; |
**[Sealed Secret](#adding-sealed-secret)** |
`sealedSecret.{name}` | Name of secret                                                                                                                                          |
`sealedSecret.{name}.type` | Type of the secret - Default Opaque                                                                                                                     | `kubernetes.io/tls`
`sealedSecret.{name}.encryptedData` | List of 'Key: Value' pair of the encrypted data                                                                                                         | `password: AgBOQOoh7RGqTBPPSG0Ctbf...`


# Mini guides
There are a few guides, but if it is still not what you're looking for, please direct your attention to the [configuration](#Configuration). Here you will find all the configuration values that can be set.
## Adding Basics
Parameter | Description | Example
--- | --- | ---
**Basics** |
`fullnameOverride` | Name of the service
`namespace` | Namespace to deploy into
`podSecurityContext` | podSecurityContext
`podAnnotations` | Annotations for the pod fx prometheus | `prometheus.io/path: actuator prometheus` <br> `prometheus.io/scrape: "true"` <br>
`serviceAccountName` | serviceAccountName
`imagePullSecrets` | imagePullSecrets
`image.repository` | Name of the web-service image
`image.tag` | Web-service image tag
`autoscaling.enabled` | Enable autoscaling | `true` or `false`
`replicaCount` | Number of replicas - Not used if autoscaling is enabled | `1`, `2`, `3` ..
`deploymentStrategy` | Enables to set deployment strategy | `Recreate`

> The `autoscaling.enabled` does not create autoscaling, but just makes sure that `replicaCount` is not used.

> see full configuration [here]( #Configuration)

First of all we need to specify the basics. We need to provide information about the image we wish to deploy.

For `deploymentStrategy`, Kubernetes offers two strategies; `Recreate` or `RollingUpdate` (default).
- `Recreate` : All existing Pods are killed before new ones are created
- `RollingUpdate` : The Deployment updates Pods in a rolling update fashion
> This chart does not support paramaters; `maxUnavailable` and `maxSurge` to control the rolling update process.

> Kubernetes Doc: [Deploymentstrategies](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#strategy) \
> You can also achieve other kinds of deploymentstrategies like; canary, blue/green etc [read here](https://blog.container-solutions.com/kubernetes-deployment-strategies)

```yaml
fullnameOverride: mywebapp
namespace: my-tenant-namespace
podSecurityContext:
  runAsUser: 1000
  runAsGroup: 3000
  fsGroup: 2000
imagePullSecrets:
  - name: someSecret
replicaCount: 4
autoscaling: false
deploymentStrategy: Recreate
image:
  repository: kvalitetsit/greatestImageOfAllTime
  tag: 1.1.0
podAnnotations:
   prometheus.io/path: /metrics
   prometheus.io/port: "9113"
   prometheus.io/scrape: "true"
```
## Adding Deployment

Parameter | Description | Example
--- | --- | ---
`statefulset.enabled` | Enables the statefulset | `true` or `false`
`statefulset.hostAliases` | hostAliases |
`statefulset.containerPort` | Port on web-service | `8080`
`statefulset.extraContainerPort` | Extra port on statefulset| `Port1: 8051`
`statefulset.configMapMountPaths` | Set value if config map needs to mount on statefulset | `/config`
`statefulset.extraVolumeMounts` | Extra volume mounts
`statefulset.readinessProbe` | Set values under this to config readiness probe
`statefulset.livenessProbe` | Set values under this to config liveness probe
`statefulset.commands` | List of cronjob commands | `- /bin/bash`
`statefulset.args` | List of arguments to the commands |
**Deployment - Environment variables** |
`statefulset.env` | Map of environment variables
`statefulset.env.{name}` | Name of the environment variables
`statefulset.env.{name}.value` | Value of the environment variable
`statefulset.env.{name}.type` | Optional: specify type of the environment variables if it should be read from a secret or configmap.<br> One of fieldPath, secretKeyRef, configMapKeyRef | `type: secretKeyRef`
`statefulset.env.{name}.fieldPath` | Optional: Value of fieldPath
`statefulset.env.{name}.name` | Optional: Name of the SecretKeyRef or ConfigMapKeyRef
`statefulset.env.{name}.key` | Optional: Key for the SecretKeyRef or ConfigMapKeyRef
`statefulset.envFrom` | Map of environment variables
`statefulset.envFrom.configMapRef` | List of ConfigMaps to read environment variables -from | <code>configMapRef:<br>&nbsp;&nbsp;- my-configmap</code>
**Deployment - Volume mounts** |
`statefulset.extraVolumeMounts` | Extra volume mounts
`statefulset.extraVolumeMounts.{name}` | Name of the extra volume mount. This must match the name of 'statefulset.extraVolumes.{name}'
`statefulset.extraVolumeMounts.{name}.mountPath` | Mountpath for the extra volume
`statefulset.extraVolumes` | Extra volumes for the mounts
`statefulset.extraVolumes.{name}` | Extra volumes for the mounts.<br>This must match the name of 'statefulset.extraVolumeMounts.{name}'<br>The value can be one of persistentVolumeClaim or configMap and the value for claimname _must_ match 'pvc.{name}' or a configmap respectively | <code>my-storage: &#124;-<br>&nbsp;&nbsp;persistentVolumeClaim:<br>&nbsp;&nbsp;&nbsp;&nbsp;claimName: my-application-storage</code>

> see full configuration [here]( #Configuration)

First of all we need to specify the basics. We need to provide env-variables, arguments for our container, and also containerports. The image we need to deploy, has already been specified in chapter before [the basics](#adding-basics).
```yaml
statefulset:
...
  enabled: true
  containerPort: 1313
  exstraContainerPort: 1314
  args: []
  env:
    someEnvVar1:
      value: goodValue1
    someEnvVar2:
      value: valueForSecrets
```
When that is done, we need to setup our probes. Kubernetes describes the probes as follows;
- *ReadinessProbe* The kubelet uses readiness probes to know **when a container is ready to start accepting traffic**. A Pod is considered ready when all of its containers are ready. One use of this signal is to control which Pods are used as backends for Services. When a Pod is not ready, it is removed from Service load balancers.
- *LivenessProbe* The kubelet uses liveness probes to know **when to restart a container**. For example, liveness probes could catch a deadlock, where an application is running, but unable to make progress. Restarting a container in such a state can help to make the application more available despite bugs.
- *StartupProbe* The kubelet uses startup probes to know **when a container application has started**. If such a probe is configured, it disables liveness and readiness checks until it succeeds, making sure those probes don't interfere with the application startup. This can be used to adopt liveness checks on slow starting containers, avoiding them getting killed by the kubelet before they are up and running.

> Read more [here](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/).

Now that we know what probes are, lets configure them!

> (Written 25 aug 2021) This servicechart does currently not support startupProbes

```yaml
statefulset:
...
  readinessProbe:
    httpGet:
      path: /
      port: 1313
    initialDelaySeconds: 5
    periodSeconds: 5
    successThreshold: 1
    timeoutSeconds: 1

  livenessProbe:
    httpGet:
      path: /
      port: 1313
```
The service-chart supports all the paramaters that the regular liveness -and readinessprobes supports.

### Volumes
Now lets mount some volumes to the container. configMapMountPath shows where the referenced volume should be mounted in the container. For instance, if you mount a volume to configMapMountPath: `/a/b/c`, the volume will be available to the container under the directory `/a/b/c`.

Mounting a volume will make all of the volume available under configMapMountPath. If you need to mount only part of the volume, such as a single file in a volume, you use configMapMountSubPath to specify the part that must be mounted. For instance, configMapMountPath: `/a/b/c`, configMapMountSubPath: `d` will make whatever `d` is in the mounted volume under directory `/a/b/c`

```yaml
statefulset:
...
  configMapMountPaths:
    nginx-conf:
      configMapMountPath: /config/
      configMapMountSubPath: nginx.conf

  # Extra
  extraVolumes:
    firstVolume: |
      persistentVolumeClaim:
        claimName: my-persistent-volume-claim
  extraVolumeMounts:
    firstVolume:
      mountPath: /path/in/container

```
## Set Pod affinity
This chart allows you to set custom Pod affinity using the affinity parameter. Find more information about Pod's affinity in the Kubernetes documentation.

As an alternative, you can use any of the preset configurations for pod affinity, pod anti-affinity, and node affinity. To do so, set the podAffinityPreset, podAntiAffinityPreset, or nodeAffinityPreset parameters.## Adding Ingress
Parameter | Description | Example
--- | --- | ---
**Ingress** |
`ingress.enabled` | Set to false to disable ingress | `false`
`ingress.annotations` | Annotations for ingress | `kubernetes.io/ingress.class: nginx`
`ingress.hosts` | Hosts served by the ingress | `- host: domain.dk`
`ingress.tls` | TLS config
`extraIngress` | Map of extra ingress
`extraIngress.{name}` | Place ingress values under the name value

> see full configuration [here]( #Configuration)

When you find yourself in a position where you wish do deploy an application, that can be reached from the internet, you will need to add an ingress to your app. This ingress will direct traffic from a domain into the service configured.

1. Enable ingress in Configuration
1. Add the appropriate annotations
1. Specify what domain/host that should point to the service
1. Add the tls-part in the example below, if TLS/SSL should be used
    > *Please note that this uses the certificate existing in the secret specified in secretName. (It will not create the certificate)*

```yaml
  ingress:
    enabled: true
    annotations: {}
      # kubernetes.io/ingress.class: nginx
      # kubernetes.io/tls-acme: "true"
    hosts:
      - host: mydomain.com
        paths:
          - path: /
    tls:
      - hosts:
          - mydomain.com
        secretName: mydomain.com
```
### Create SSL/TLS Certificate
 If you wish to create the certificate you need to add the following to a new file in the template-folder. (This requires Cert-manager to be installed)
```yaml
{{- if .Values.certificate.enabled -}}
apiVersion: cert-manager.io/v1alpha2
kind: Certificate
metadata:
  name: {{ .Values.certificate.name }}
  namespace: {{ .Values.namespace }}
spec:
  dnsNames:
  {{- range .Values.certificate.dnsNames }}
  - {{ .  }}
  {{- end }}
  issuerRef:
    group: cert-manager.io
    kind: {{ .Values.certificate.issuer.kind }}
    name: {{ .Values.certificate.issuer.name }}
  secretName: {{ .Values.certificate.name }}
  {{- end }}
```
When this is in a `.yaml` file in the template-folder, add the following to your values-file
```yaml
certificate:
  mydomain.com:
    namespace: infrastructure
    spec:
      secretName: mydomain.com
      issuerRef:
        name: letsencrypt-prod
        kind: ClusterIssuer
        group: cert-manager.io
      dnsNames:
        - mydomain.com
```
## Adding sealed secret
Parameter | Description | Example
--- | --- | ---
**Sealed Secret** |
`sealedSecret.{name}` | Name of secret |
`sealedSecret.{name}.type` | Type of the secret - Default Opaque | `kubernetes.io/tls`
`sealedSecret.{name}.encryptedData` | List of 'Key: Value' pair of the encrypted data | `password: AgBOQOoh7RGqTBPPSG0Ctbf...`

> see full configuration [here]( #Configuration)

With sealed secrets it is possible to store your secrets in github with no security-risk. Sounds like something for you? [Follow this guide!](https://doc.hosting.kitkube.dk/deployment/secrets/)
```yaml
sealedSecret:
  my-secret:
    type: Opaque
    encryptedData:
      password: myEncryptedSecretPassword #remember to encrypt value!
      otherPassword: myOtherEncryptedAndMaybeRedundantPassword #remember to encrypt value!
      thirdPassword: encryptedQuestionWhySoManyPasswords? #remember to encrypt value!
```
