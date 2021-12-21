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

As an alternative, you can use any of the preset configurations for pod affinity, pod anti-affinity, and node affinity. To do so, set the podAffinityPreset, podAntiAffinityPreset, or nodeAffinityPreset parameters.