# Kubernetes notes - Tipes and notes on writing manifest files

## Learning k8s API

Usefull links:

- [The beginners guide to creating Kubernetes manifests - blog article](https://prefetch.net/blog/2019/10/16/the-beginners-guide-to-creating-kubernetes-manifests/)

- [Understanding the Kubernetes manifest - Medium article](https://medium.com/@sujithar37/understanding-the-kubernetes-manifest-97f44acc2cb9)

Manifest file contains four requierd fiels:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  ...
spec:
  ...
```

where:

- apiVersion - is API group which is used to create resource
- kind - is a type of resource
- metadata - is a section used to uniquely identify resource inside cluster
- spec - section used to describe how to create and manage resource

### apiVersion

Available API can be seen with `kubectl`:

```bash
kubectl api-versions
```

### kind

Available resources can be seen with `kubectl`:

```bash
kubectl api-resources
```

### `kubectl explain` command

`api-versions` and `api-resources` can be used to "explain" resource

```bash
kubectl explain --api-version=apps/v1 deployments
```

output:

```bash
KIND:     Deployment
VERSION:  apps/v1

DESCRIPTION:
     Deployment enables declarative updates for Pods and ReplicaSets.

FIELDS:
    ...
```

Name of fields can be appended to get further description

```bash
kubectl explain --api-version=apps/v1 deployments.spec
```

output:

```bash
KIND:     Deployment
VERSION:  apps/v1

RESOURCE: spec <Object>

DESCRIPTION:
     Specification of the desired behavior of the Deployment.

     DeploymentSpec is the specification of the desired behavior of the
     Deployment.

FIELDS:
    ...
```

And so on and on

```bash
kubectl explain --api-version=apps/v1 deployments.spec.template.spec.containers
```

will give description of container resource:

```bash
KIND:     Deployment
VERSION:  apps/v1

RESOURCE: containers <[]Object>

DESCRIPTION:
     List of containers belonging to the pod. Containers cannot currently be
     added or removed. There must be at least one container in a Pod. Cannot be
     updated.

     A single application container that you want to run within a pod.

FIELDS:
    ...
```

`--recursive` flag will give hierarchical structure of resource

```bash
kubectl explain --api-version=apps/v1 deployments.spec.template.spec.containers --recursive
```

output

```bash
...
RESOURCE: containers <[]Object>
...

FIELDS:
   args	<[]string>
   command	<[]string>
   env	<[]Object>
      name	<string>
      value	<string>
      valueFrom	<Object>
         configMapKeyRef	<Object>
            key	<string>
            name	<string>
            optional	<boolean>
         fieldRef	<Object>

```

### Creating basic manifest with `kubectl create`

To avoid writing manifests from scratch `kubectl create` can be used

`kubectl create deployment 'deployment-name' --image='image-name' -o yaml --dry-run=client`

will output to stout basic manifest in yaml format.