# helm-charts-making-it-simple-to-package-and-deploy-apps-on-kubernetes

This will automatically deploy an app to kubernetes.

## Prerequisites
[Install Helm](https://github.com/kubernetes/helm#install)
`helm repo add itech https://itechops.github.io/k8s-charts/`

## Usage

Copy the generic values to a location of your filesystem for modification and customize
`cp generic-app/values.yaml updated-values.yaml`

Now simply run the helm command:
`helm install -f updated-values.yaml itech/generic-app --name your-appname --namespace your-namespace`

For LoadBalancers you have several options

1) `type: LoadBalancer`: This uses an amazon elb and must be accompanied with an Amazon Cert manager ARN.

* ```type: LoadBalancer
  acm: arn:aws:acm:....```

2) `type: ClusterIP`: This creates an internal load balancer. You are able to access this service by using the cluster ip or by the hostname (same as the app name)

3) `type: ""` Leaving the type as `""` will use our nginx-ingress load balancer. If this is filled out don't forget to fill in the details under `ingress` and create a new entry in route53

To disable the healthcheck and readinessProbe change `healthcheck: /` to `healthcheck: null`


### Ingress
The ingress format has changed as such:

```
ingress:
  hosts:
  - name: clevermeow.biz
    rules:
    - subdomain: mood. # <- don't forget the .
      path: /
    - subdomain: moo.  # <- don't forget the .
      path: /
  - name: itech.md
    rules:
    - subdomain: # <- naked domain
      path: /
    - subdomain: zoom. # <- zoom.itech.md
      path: /
```
This allows multiple domains to be specified. with multiple paths for each domain.

### Making changes

You can update the application by using the update command. Simply `helm get values <app you want to modify>`. Copy that file locally. Make the desired changes then run

`helm upgrade -f updated-values.yaml <app-name> itech/generic-app`

TODO:
Use config maps to store non-sensitive data
Allow multiple uri paths in ingress