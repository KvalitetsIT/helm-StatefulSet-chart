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
