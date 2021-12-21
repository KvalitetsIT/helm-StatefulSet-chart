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
