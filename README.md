# openshift-acme-installer

An Ansible Playbook Bundle (APB) for deploying the [OpenShift ACME Controller](https://github.com/tnozicka/openshift-acme) to an OpenShift cluster.

## Requirements

An OpenShift cluster. (e.g. `oc cluster up`)

## Installing OpenShift ACME

```bash
# login as a user with cluster-admin
oc login -u system:admin
# provision openshift-acme
docker run --rm --net=host -v $HOME/.kube:/opt/apb/.kube:z -u $UID docker.io/lorbus/openshift-acme-installer provision -v
```

You can override default variables by using `--extra-vars`.
For example, to use the Let's Encrypt Live Endpoint (instead of the default Staging one) for production deployments, run:
```bash
docker run --rm --net=host -v $HOME/.kube:/opt/apb/.kube:z -u $UID docker.io/lorbus/openshift-acme-installer provision --extra-vars "acme_url=https://acme-v01.api.letsencrypt.org/directory"
```

#### Available variables
| Variable   | Default |
|---|---|
|acme_url| https://acme-staging.api.letsencrypt.org/directory |
|acme_loglevel | 8 |
|acme_selfservicename| acme-controller|
|docker_image | docker.io/tnozicka/openshift-acme|
|docker_image_tag | lastest|

## Uninstalling OpenShift ACME
```bash
# login as a user with cluster-admin
oc login -u system:admin
# deprovision openshift-acme
docker run --rm --net=host -v $HOME/.kube:/opt/apb/.kube:z -u $UID docker.io/lorbus/openshift-acme-installer deprovision
```
