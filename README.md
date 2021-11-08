# moleculer helm package

This package will help you simply deploy yor moleculer app on GKE.

This chart bootstraps an [moleculer](https://moleculer.services/) app deployment on a [GKE](https://cloud.google.com/kubernetes-engine) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- GKE cluster v1.19+
- [Global static IP name](https://cloud.google.com/compute/docs/ip-addresses/reserve-static-external-ip-address#reserve_new_static)
- DNS point to the IP address that created before, you can create a DNS record if you host your domain name in GCloud using the [following steps](https://cloud.google.com/sdk/gcloud/reference/dns/record-sets/create)
- Your app image URL that you want to deploy 

## Get Repo Info

```console
helm repo add moleculer https://mhm0ud.github.io/helm-charts
helm repo update
```

## Install Chart

**Important:** only helm3 is supported

```console
helm install [RELEASE_NAME] helm-charts/moleculer
```

The command deploys moleculer on the Kubernetes cluster in the default configuration.

_See [configuration](#configuration) below._

_See [helm install](https://helm.sh/docs/helm/helm_install/) for command documentation._

## Uninstall Chart

```console
helm uninstall [RELEASE_NAME]
```

This removes all the Kubernetes components associated with the chart and deletes the release.

_See [helm uninstall](https://helm.sh/docs/helm/helm_uninstall/) for command documentation._

## Upgrading Chart

```console
helm upgrade [RELEASE_NAME] [CHART] --install
```

_See [helm upgrade](https://helm.sh/docs/helm/helm_upgrade/) for command documentation._

## Configuration

See [Customizing the Chart Before Installing](https://helm.sh/docs/intro/using_helm/#customizing-the-chart-before-installing). To see all configurable options with detailed comments, visit the chart's [values.yaml](./values.yaml), or run these configuration commands:

```console
helm show values helm-charts/moleculer
```
### Container Name 

You'll need to set the following value:

`container.name`

### Static IP Name Key

GlobalStaticIPNameKey tells the Ingress controller to use a specific GCE static ip for its forwarding rules.

You'll need to set the following value:

`ingress.annotations."kubernetes\.io/ingress\.global-static-ip-name"="STATIC_IP_NAME"`

and it's will annotate the ingress as shown in the [gce-ingress](https://github.com/kubernetes/ingress-gce/blob/e1e23f6b97faadc1a33bdde5ecfed10ceedd535b/pkg/annotations/ingress.go):

```yaml
kind: 'Ingress'
metadata:
  name: 'ingress'
  labels:
    name: ingress
  annotations:
    kubernetes.io/ingress.global-static-ip-name: 'STATIC_IP_NAME'
```

### Domain Name

Hosts tells the Ingress controller to use a specific DNS ip for its forwarding rules.

You'll need to set the following value:

`ingress.hosts[0].host="api.example.com"`

### Image Repository URL

This will define your application image repo URL.

You'll need to set the following value:

`image.repository`

P.S. `image.repository="mhm0ud/moleculer"`

### Image Tag

You'll need to set the following value:

`image.tag`

P.S. `image.tag="latest"`

### Service Name

You shaould include which services you need to deploy in this conatiner.

P.S. you have four services (api, service1, service2, service3), then you'll need to set the following value:

`container.services="{api,service1,service2,service3}"`

### CRON Job Configuration

If you have a cron and you need to run it, you need to specify it in this value:

`cron.enabled`

P.S. You have one cron job, you need to set the following values:

`cron.enabled=true`
`cron.list[0].name=cron1`
`cron.list[0].schedule="*/5 * * * *"`
`cron.list[0].command=products.updateAll`

**This feature is disabled by default since 0.1.8**

### Secrets

If you need to add secrets to your cluster, you'll need to set the following value:

`environment.secret.[YOUR_SECRET_NAME]`

P.S. `environment.secret.MONGO_URI=MONGO_URL`

### Environment Variables 

If you need to add environment variables to your cluster, you'll need to set the following value:

`environment.env.[YOUR_ENVIRONMENT_VARIABLES_NAME]`

P.S. `environment.env.NAMESPACE="NAMESPACE"`