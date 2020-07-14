# Right Lyrics

A very simple microservice architecture to be deployed in OpenShift.

## Overview

![preview](./documentation/images/preview.png)

## Topology

![topology](./documentation/images/topology.png)

### Components

* **Lyrics Page** (React.js + NGINX)
* **Lyrics Service** (Node.js + MongoDB)
* **Songs Service** (Spring Boot + PostgreSQL)
* **Hits Service** (Python + Redis)
* **Albums Service** (Quarkus + MySQL)
* **Operator** (Operator Framework using Ansible)

For more information, read the [documentation](documentation/README.md).

## Deploy in OpenShift

Create the CustomResourceDefinition (this requires cluster admin privileges):

```bash
oc create -f ./operator/deploy/crds/veicot_v1_rightlyrics_crd.yaml
```

Create the Right Lyrics project:

```bash
oc new-project right-lyrics
```

Deploy the Operator:

```bash
oc create -f ./operator/deploy/service_account.yaml -n right-lyrics
oc create -f ./operator/deploy/role.yaml -n right-lyrics
oc create -f ./operator/deploy/role_binding.yaml -n right-lyrics
oc create -f ./operator/deploy/operator.yaml -n right-lyrics
```

Deploy a CustomResource:

```yaml
apiVersion: veicot.io/v1
kind: RightLyrics
metadata:
  name: my-rightlyrics
spec:
  lyricsPageReplicas: 1
  lyricsServiceReplicas: 1
  songsServiceReplicas: 1
  hitsServiceReplicas: 1
```

The CustomResource can be created as follows:

```bash
oc create -f ./operator/deploy/crds/veicot_v1_rightlyrics_cr.yaml -n right-lyrics
```

Finally, the Operator will create the application based on the CustomResource.