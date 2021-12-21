## Adding Ingress
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
