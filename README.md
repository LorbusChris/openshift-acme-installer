# openshift-acme-installer

An Ansible Playbook Bundle (APB) for deploying the [OpenShift ACME Controller](https://github.com/tnozicka/openshift-acme) to an OpenShift cluster.

## Requirements

An OpenShift cluster. (e.g. `oc cluster up`)

## Building the installer

```bash
apb build
```

## Installing OpenShift ACME

Before installing, you must be logged in with `cluster-admin` privileges
```bash
# login as a user with cluster-admin
oc login -u system:admin
```

You can docker run the ASB.  Assuming the installer was built with the default tag of `openshift-acme-installer`, the basic installation of ASB looks like:
```bash
docker run --rm --net=host -v $HOME/.kube:/opt/apb/.kube:z -u $UID openshift-acme-installer provision
```

You can pass in more variables to the command as you would when running an Ansible playbook, such as:

Running with verbose output:
```bash
docker run --rm --net=host -v $HOME/.kube:/opt/apb/.kube:z -u $UID openshift-acme-installer provision -v
```
Overriding default one ore more variables using `--extra-vars`:
```bash
docker run --rm --net=host -v $HOME/.kube:/opt/apb/.kube:z -u $UID openshift-acme-installer provision --extra-vars "acme_selfservicename=acme-controller"
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
# assuming the installer has been tagged as asb-installer
docker run --rm --net=host -v $HOME/.kube:/opt/apb/.kube:z -u $UID openshift-acme-installer deprovision
```
