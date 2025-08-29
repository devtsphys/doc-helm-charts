# Helm Charts Complete Reference Card

## Chart Structure

```
mychart/
├── Chart.yaml          # Chart metadata
├── values.yaml         # Default configuration values
├── charts/             # Chart dependencies
├── templates/          # Kubernetes manifest templates
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── configmap.yaml
│   ├── _helpers.tpl    # Template helpers
│   └── NOTES.txt       # Post-install notes
└── .helmignore         # Files to ignore during packaging
```

## Chart.yaml Structure

```yaml
apiVersion: v2
name: myapp
description: A Helm chart for Kubernetes
type: application
version: 0.1.0
appVersion: "1.16.0"
maintainers:
  - name: John Doe
    email: john@example.com
dependencies:
  - name: postgresql
    version: "11.6.12"
    repository: https://charts.bitnami.com/bitnami
keywords:
  - web
  - frontend
home: https://example.com
sources:
  - https://github.com/example/myapp
```

## Template Functions Reference

### Built-in Functions

|Function    |Description          |Example                                     |
|------------|---------------------|--------------------------------------------|
|`default`   |Provide default value|`{{ .Values.image.tag | default "latest" }}`|
|`quote`     |Wrap in quotes       |`{{ .Values.name | quote }}`                |
|`squote`    |Wrap in single quotes|`{{ .Values.name | squote }}`               |
|`upper`     |Convert to uppercase |`{{ .Values.env | upper }}`                 |
|`lower`     |Convert to lowercase |`{{ .Values.name | lower }}`                |
|`title`     |Title case           |`{{ .Values.name | title }}`                |
|`trim`      |Remove whitespace    |`{{ .Values.input | trim }}`                |
|`trimSuffix`|Remove suffix        |`{{ .Values.name | trimSuffix "-dev" }}`    |
|`trimPrefix`|Remove prefix        |`{{ .Values.name | trimPrefix "app-" }}`    |
|`replace`   |Replace string       |`{{ .Values.name | replace "old" "new" }}`  |
|`contains`  |Check if contains    |`{{ if contains "prod" .Values.env }}`      |
|`hasPrefix` |Check prefix         |`{{ if hasPrefix "app-" .Values.name }}`    |
|`hasSuffix` |Check suffix         |`{{ if hasSuffix "-dev" .Values.name }}`    |
|`indent`    |Add indentation      |`{{ .Values.config | indent 4 }}`           |
|`nindent`   |Newline + indent     |`{{ .Values.config | nindent 2 }}`          |
|`toYaml`    |Convert to YAML      |`{{ .Values.labels | toYaml }}`             |
|`toJson`    |Convert to JSON      |`{{ .Values.data | toJson }}`               |
|`fromYaml`  |Parse YAML           |`{{ .Values.yamlString | fromYaml }}`       |
|`fromJson`  |Parse JSON           |`{{ .Values.jsonString | fromJson }}`       |

### Sprig Functions

|Category    |Function       |Description         |Example                                         |
|------------|---------------|--------------------|------------------------------------------------|
|**String**  |`abbrev`       |Abbreviate string   |`{{ "kubernetes" | abbrev 5 }}`                 |
|            |`abbrevboth`   |Abbreviate both ends|`{{ "kubernetes" | abbrevboth 2 3 }}`           |
|            |`plural`       |Pluralize           |`{{ "box" | plural }}`                          |
|            |`wrap`         |Word wrap           |`{{ .Values.description | wrap 80 }}`           |
|            |`wrapWith`     |Wrap with string    |`{{ "hello" | wrapWith "**" }}`                 |
|**Numeric** |`add`          |Addition            |`{{ add 1 2 }}`                                 |
|            |`sub`          |Subtraction         |`{{ sub 5 2 }}`                                 |
|            |`mul`          |Multiplication      |`{{ mul 3 4 }}`                                 |
|            |`div`          |Division            |`{{ div 10 2 }}`                                |
|            |`mod`          |Modulo              |`{{ mod 10 3 }}`                                |
|            |`max`          |Maximum             |`{{ max 1 2 3 }}`                               |
|            |`min`          |Minimum             |`{{ min 1 2 3 }}`                               |
|            |`round`        |Round number        |`{{ 3.14159 | round }}`                         |
|            |`floor`        |Floor               |`{{ 3.9 | floor }}`                             |
|            |`ceil`         |Ceiling             |`{{ 3.1 | ceil }}`                              |
|**Date**    |`now`          |Current time        |`{{ now }}`                                     |
|            |`date`         |Format date         |`{{ now | date "2006-01-02" }}`                 |
|            |`dateInZone`   |Date in timezone    |`{{ now | dateInZone "2006-01-02" "UTC" }}`     |
|            |`duration`     |Parse duration      |`{{ duration "1h30m" }}`                        |
|            |`durationRound`|Round duration      |`{{ duration "1h30m45s" | durationRound "1h" }}`|
|**Encoding**|`b64enc`       |Base64 encode       |`{{ "hello" | b64enc }}`                        |
|            |`b64dec`       |Base64 decode       |`{{ "aGVsbG8=" | b64dec }}`                     |
|            |`sha256sum`    |SHA256 hash         |`{{ "password" | sha256sum }}`                  |
|            |`sha1sum`      |SHA1 hash           |`{{ "password" | sha1sum }}`                    |
|**Lists**   |`list`         |Create list         |`{{ list "a" "b" "c" }}`                        |
|            |`append`       |Append to list      |`{{ list "a" "b" | append "c" }}`               |
|            |`prepend`      |Prepend to list     |`{{ list "b" "c" | prepend "a" }}`              |
|            |`first`        |First element       |`{{ list "a" "b" "c" | first }}`                |
|            |`last`         |Last element        |`{{ list "a" "b" "c" | last }}`                 |
|            |`rest`         |All but first       |`{{ list "a" "b" "c" | rest }}`                 |
|            |`initial`      |All but last        |`{{ list "a" "b" "c" | initial }}`              |
|            |`reverse`      |Reverse list        |`{{ list "a" "b" "c" | reverse }}`              |
|            |`uniq`         |Unique elements     |`{{ list "a" "b" "a" | uniq }}`                 |
|            |`compact`      |Remove empty        |`{{ list "a" "" "b" | compact }}`               |
|            |`join`         |Join with separator |`{{ list "a" "b" "c" | join "," }}`             |
|            |`sortAlpha`    |Sort alphabetically |`{{ list "c" "a" "b" | sortAlpha }}`            |

### Kubernetes Helm Functions

|Function  |Description           |Example                                      |
|----------|----------------------|---------------------------------------------|
|`include` |Include named template|`{{ include "myapp.labels" . }}`             |
|`required`|Require value         |`{{ required "Name required" .Values.name }}`|
|`tpl`     |Render template string|`{{ tpl .Values.template . }}`               |
|`toYaml`  |Convert to YAML       |`{{ .Values.labels | toYaml | nindent 4 }}`  |
|`fromYaml`|Parse YAML string     |`{{ .Values.yamlConfig | fromYaml }}`        |
|`toJson`  |Convert to JSON       |`{{ .Values.data | toJson }}`                |
|`fromJson`|Parse JSON string     |`{{ .Values.jsonConfig | fromJson }}`        |

## Built-in Objects

|Object                     |Description            |Example                                        |
|---------------------------|-----------------------|-----------------------------------------------|
|`.Release.Name`            |Release name           |`{{ .Release.Name }}-pod`                      |
|`.Release.Namespace`       |Target namespace       |`{{ .Release.Namespace }}`                     |
|`.Release.Service`         |Rendering service      |`{{ .Release.Service }}`                       |
|`.Release.Revision`        |Revision number        |`{{ .Release.Revision }}`                      |
|`.Release.IsUpgrade`       |Is this an upgrade?    |`{{ if .Release.IsUpgrade }}`                  |
|`.Release.IsInstall`       |Is this an install?    |`{{ if .Release.IsInstall }}`                  |
|`.Chart.Name`              |Chart name             |`{{ .Chart.Name }}`                            |
|`.Chart.Version`           |Chart version          |`{{ .Chart.Version }}`                         |
|`.Chart.AppVersion`        |App version            |`{{ .Chart.AppVersion }}`                      |
|`.Values`                  |Values from values.yaml|`{{ .Values.image.repository }}`               |
|`.Files`                   |Chart files access     |`{{ .Files.Get "config.conf" }}`               |
|`.Capabilities.APIVersions`|Available API versions |`{{ .Capabilities.APIVersions.Has "apps/v1" }}`|
|`.Capabilities.KubeVersion`|Kubernetes version     |`{{ .Capabilities.KubeVersion.Version }}`      |

## Template Actions

### Conditionals

```yaml
# If statement
{{ if .Values.enabled }}
enabled: true
{{ end }}

# If-else
{{ if eq .Values.env "production" }}
replicas: 3
{{ else }}
replicas: 1
{{ end }}

# Complex conditions
{{ if and .Values.persistence.enabled (not .Values.persistence.existingClaim) }}
  # Create PVC
{{ end }}
```

### Loops

```yaml
# Range over list
{{ range .Values.environments }}
- name: {{ . }}
{{ end }}

# Range with index
{{ range $index, $env := .Values.environments }}
- name: env-{{ $index }}
  value: {{ $env }}
{{ end }}

# Range over map
{{ range $key, $value := .Values.config }}
{{ $key }}: {{ $value | quote }}
{{ end }}
```

### Variables

```yaml
# Simple variable
{{ $name := .Values.name }}
name: {{ $name }}

# Variable in range
{{ range $key, $value := .Values.env }}
{{- $envName := printf "%s-%s" $.Release.Name $key }}
- name: {{ $envName }}
  value: {{ $value }}
{{ end }}
```

## Named Templates (_helpers.tpl)

```yaml
{{/*
Expand the name of the chart.
*/}}
{{- define "myapp.name" -}}
{{- default .Chart.Name .Values.nameOverride | trunc 63 | trimSuffix "-" }}
{{- end }}

{{/*
Create a default fully qualified app name.
*/}}
{{- define "myapp.fullname" -}}
{{- if .Values.fullnameOverride }}
{{- .Values.fullnameOverride | trunc 63 | trimSuffix "-" }}
{{- else }}
{{- $name := default .Chart.Name .Values.nameOverride }}
{{- if contains $name .Release.Name }}
{{- .Release.Name | trunc 63 | trimSuffix "-" }}
{{- else }}
{{- printf "%s-%s" .Release.Name $name | trunc 63 | trimSuffix "-" }}
{{- end }}
{{- end }}
{{- end }}

{{/*
Common labels
*/}}
{{- define "myapp.labels" -}}
helm.sh/chart: {{ include "myapp.chart" . }}
{{ include "myapp.selectorLabels" . }}
{{- if .Chart.AppVersion }}
app.kubernetes.io/version: {{ .Chart.AppVersion | quote }}
{{- end }}
app.kubernetes.io/managed-by: {{ .Release.Service }}
{{- end }}

{{/*
Selector labels
*/}}
{{- define "myapp.selectorLabels" -}}
app.kubernetes.io/name: {{ include "myapp.name" . }}
app.kubernetes.io/instance: {{ .Release.Name }}
{{- end }}
```

## Template Examples

### Deployment Template

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "myapp.fullname" . }}
  labels:
    {{- include "myapp.labels" . | nindent 4 }}
spec:
  {{- if not .Values.autoscaling.enabled }}
  replicas: {{ .Values.replicaCount }}
  {{- end }}
  selector:
    matchLabels:
      {{- include "myapp.selectorLabels" . | nindent 6 }}
  template:
    metadata:
      {{- with .Values.podAnnotations }}
      annotations:
        {{- toYaml . | nindent 8 }}
      {{- end }}
      labels:
        {{- include "myapp.selectorLabels" . | nindent 8 }}
    spec:
      containers:
        - name: {{ .Chart.Name }}
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag | default .Chart.AppVersion }}"
          imagePullPolicy: {{ .Values.image.pullPolicy }}
          ports:
            - name: http
              containerPort: {{ .Values.service.targetPort }}
              protocol: TCP
          env:
            {{- range $key, $value := .Values.env }}
            - name: {{ $key }}
              value: {{ $value | quote }}
            {{- end }}
          {{- with .Values.resources }}
          resources:
            {{- toYaml . | nindent 12 }}
          {{- end }}
```

### Service Template

```yaml
apiVersion: v1
kind: Service
metadata:
  name: {{ include "myapp.fullname" . }}
  labels:
    {{- include "myapp.labels" . | nindent 4 }}
spec:
  type: {{ .Values.service.type }}
  ports:
    - port: {{ .Values.service.port }}
      targetPort: {{ .Values.service.targetPort }}
      protocol: TCP
      name: http
  selector:
    {{- include "myapp.selectorLabels" . | nindent 4 }}
```

## Common Patterns and Techniques

### Conditional Resource Creation

```yaml
{{- if .Values.ingress.enabled -}}
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: {{ include "myapp.fullname" . }}
  {{- with .Values.ingress.annotations }}
  annotations:
    {{- toYaml . | nindent 4 }}
  {{- end }}
spec:
  # ... ingress spec
{{- end }}
```

### Environment Variables from ConfigMap/Secret

```yaml
env:
  {{- if .Values.config }}
  {{- range $key, $value := .Values.config }}
  - name: {{ $key }}
    value: {{ $value | quote }}
  {{- end }}
  {{- end }}
  {{- if .Values.secrets }}
  {{- range $key, $secret := .Values.secrets }}
  - name: {{ $key }}
    valueFrom:
      secretKeyRef:
        name: {{ $secret.name }}
        key: {{ $secret.key }}
  {{- end }}
  {{- end }}
```

### Volume Mounts

```yaml
volumeMounts:
  {{- range .Values.persistence.volumes }}
  - name: {{ .name }}
    mountPath: {{ .mountPath }}
    {{- if .subPath }}
    subPath: {{ .subPath }}
    {{- end }}
  {{- end }}
volumes:
  {{- range .Values.persistence.volumes }}
  - name: {{ .name }}
    {{- if .persistentVolumeClaim }}
    persistentVolumeClaim:
      claimName: {{ .persistentVolumeClaim.claimName }}
    {{- else if .configMap }}
    configMap:
      name: {{ .configMap.name }}
    {{- else if .secret }}
    secret:
      secretName: {{ .secret.secretName }}
    {{- end }}
  {{- end }}
```

### Resource Limits and Requests

```yaml
{{- with .Values.resources }}
resources:
  {{- if .limits }}
  limits:
    {{- toYaml .limits | nindent 4 }}
  {{- end }}
  {{- if .requests }}
  requests:
    {{- toYaml .requests | nindent 4 }}
  {{- end }}
{{- end }}
```

## Flow Control

### With Blocks

```yaml
# With - sets scope
{{- with .Values.nodeSelector }}
nodeSelector:
  {{- toYaml . | nindent 2 }}
{{- end }}

# Multiple with conditions
{{- with .Values.affinity }}
affinity:
  {{- toYaml . | nindent 2 }}
{{- end }}
```

### Range Examples

```yaml
# Simple range
env:
{{- range .Values.env }}
  - name: {{ .name }}
    value: {{ .value | quote }}
{{- end }}

# Range with variables
{{- range $index, $config := .Values.configs }}
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ $.Release.Name }}-config-{{ $index }}
data:
  {{- range $key, $value := $config }}
  {{ $key }}: {{ $value | quote }}
  {{- end }}
{{- end }}
```

## Advanced Techniques

### Template Inheritance

```yaml
# Base template
{{- define "myapp.deployment.base" -}}
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "myapp.fullname" . }}
spec:
  replicas: {{ .Values.replicaCount }}
  template:
    spec:
      containers:
        - name: {{ .Chart.Name }}
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
{{- end }}

# Extended template
{{- define "myapp.deployment.extended" -}}
{{ include "myapp.deployment.base" . }}
          env:
            - name: EXTENDED_VAR
              value: "true"
{{- end }}
```

### Validation and Required Values

```yaml
{{- if not .Values.image.repository }}
{{- fail "image.repository is required" }}
{{- end }}

# Or using required function
image: {{ required "image.repository is required" .Values.image.repository }}

# Validate enum values
{{- if not (has .Values.service.type (list "ClusterIP" "NodePort" "LoadBalancer")) }}
{{- fail "service.type must be ClusterIP, NodePort, or LoadBalancer" }}
{{- end }}
```

### Dynamic Resource Names

```yaml
{{- $resources := list "deployment" "service" "configmap" }}
{{- range $resource := $resources }}
---
{{ include (printf "myapp.%s" $resource) $ }}
{{- end }}
```

### Multi-line Strings

```yaml
# Literal style (preserves formatting)
config.yaml: |
  server:
    port: {{ .Values.port }}
    host: {{ .Values.host }}
  database:
    url: {{ .Values.database.url }}

# Folded style (folds newlines)
description: >
  This is a long description that will be
  folded into a single line with spaces
  replacing the newlines.

# Using nindent for proper indentation
data:
  config.yaml: |
    {{- .Values.config | nindent 4 }}
```

## Helm CLI Commands Reference

|Command          |Description             |Example                                                   |
|-----------------|------------------------|----------------------------------------------------------|
|`helm create`    |Create new chart        |`helm create mychart`                                     |
|`helm template`  |Render templates locally|`helm template myapp ./mychart`                           |
|`helm install`   |Install chart           |`helm install myapp ./mychart`                            |
|`helm upgrade`   |Upgrade release         |`helm upgrade myapp ./mychart`                            |
|`helm rollback`  |Rollback release        |`helm rollback myapp 1`                                   |
|`helm uninstall` |Uninstall release       |`helm uninstall myapp`                                    |
|`helm list`      |List releases           |`helm list -A`                                            |
|`helm get`       |Get release info        |`helm get values myapp`                                   |
|`helm status`    |Get release status      |`helm status myapp`                                       |
|`helm lint`      |Lint chart              |`helm lint ./mychart`                                     |
|`helm package`   |Package chart           |`helm package ./mychart`                                  |
|`helm dependency`|Manage dependencies     |`helm dependency update ./mychart`                        |
|`helm repo`      |Manage repositories     |`helm repo add bitnami https://charts.bitnami.com/bitnami`|

## Values.yaml Best Practices

```yaml
# Default values for myapp
replicaCount: 1

image:
  repository: nginx
  pullPolicy: IfNotPresent
  tag: ""  # Overrides the image tag whose default is the chart appVersion

imagePullSecrets: []
nameOverride: ""
fullnameOverride: ""

serviceAccount:
  create: true
  annotations: {}
  name: ""

podAnnotations: {}
podSecurityContext: {}
securityContext: {}

service:
  type: ClusterIP
  port: 80
  targetPort: 8080

ingress:
  enabled: false
  className: ""
  annotations: {}
  hosts:
    - host: chart-example.local
      paths:
        - path: /
          pathType: Prefix
  tls: []

resources:
  limits:
    cpu: 100m
    memory: 128Mi
  requests:
    cpu: 100m
    memory: 128Mi

autoscaling:
  enabled: false
  minReplicas: 1
  maxReplicas: 100
  targetCPUUtilizationPercentage: 80

nodeSelector: {}
tolerations: []
affinity: {}

# Custom application configuration
config:
  database:
    host: localhost
    port: 5432
  cache:
    redis: true
    ttl: 3600

# Environment variables
env:
  NODE_ENV: production
  LOG_LEVEL: info
```

## Testing and Debugging

### Template Debugging

```bash
# Render templates without installing
helm template myapp ./mychart

# Render with specific values
helm template myapp ./mychart --set image.tag=v2.0.0

# Render with values file
helm template myapp ./mychart -f custom-values.yaml

# Debug mode (verbose output)
helm install myapp ./mychart --debug --dry-run

# Show computed values
helm get values myapp

# Lint chart
helm lint ./mychart

# Verify chart syntax
helm template myapp ./mychart --validate
```

### Common Debugging Patterns

```yaml
# Debug output in template
{{- if .Values.debug }}
# DEBUG: Release name is {{ .Release.Name }}
# DEBUG: Values: {{ .Values | toYaml | nindent 2 }}
{{- end }}

# Fail fast on missing required values
{{- required "A valid .Values.image.repository entry required!" .Values.image.repository }}

# Type checking
{{- if not (kindIs "string" .Values.name) }}
{{- fail ".Values.name must be a string" }}
{{- end }}
```

## Hook Examples

```yaml
# Pre-install hook
apiVersion: batch/v1
kind: Job
metadata:
  name: {{ include "myapp.fullname" . }}-pre-install
  annotations:
    "helm.sh/hook": pre-install
    "helm.sh/hook-weight": "-5"
    "helm.sh/hook-delete-policy": before-hook-creation,hook-succeeded
spec:
  template:
    spec:
      restartPolicy: Never
      containers:
        - name: pre-install
          image: busybox
          command: ['sh', '-c', 'echo Pre-install hook']
```

## Dependencies

### Chart.yaml Dependencies

```yaml
dependencies:
  - name: postgresql
    version: "11.6.12"
    repository: https://charts.bitnami.com/bitnami
    condition: postgresql.enabled
  - name: redis
    version: "16.4.0"
    repository: https://charts.bitnami.com/bitnami
    condition: redis.enabled
```

### Managing Dependencies

```bash
# Update dependencies
helm dependency update

# Build dependencies
helm dependency build

# List dependencies
helm dependency list
```

## Tips and Best Practices

1. **Always use named templates** for reusable components
1. **Validate required values** using `required` function
1. **Use `nindent` for proper YAML indentation**
1. **Quote string values** to prevent YAML parsing issues
1. **Use `with` blocks** to check if values exist before using them
1. **Add comprehensive NOTES.txt** for post-install instructions
1. **Use semantic versioning** for chart versions
1. **Document all values** in values.yaml with comments
1. **Test with different value combinations** using `--set` flags
1. **Use hooks** for initialization and cleanup tasks

## Common Gotchas

- **Indentation**: YAML is sensitive to indentation - use `nindent` consistently
- **Scope**: Remember that `range` and `with` change template scope - use `$` for root context
- **Quotes**: Always quote string values that might contain special characters
- **Empty checks**: Use `with` or `if` to check for empty values before rendering
- **Naming**: Ensure resource names are DNS-compliant (lowercase, no spaces)
- **Dependencies**: Remember to run `helm dependency update` after modifying dependencies

## Useful One-liners

```yaml
# Safe string rendering with default
{{ .Values.config.timeout | default 30 }}

# Conditional labels
{{ if .Values.extraLabels }}{{ .Values.extraLabels | toYaml | nindent 4 }}{{ end }}

# Environment-specific values
{{ if eq .Values.environment "production" }}{{ .Values.prodConfig | toYaml | nindent 2 }}{{ end }}

# Dynamic naming
{{ printf "%s-%s" (include "myapp.fullname" .) "worker" }}

# File inclusion
{{ .Files.Get "configs/app.conf" | indent 2 }}
```

This reference card covers the essential aspects of Helm chart development. Keep it handy for quick lookups while building and maintaining your Helm charts!