## Install StorageOS operator

Our cluster operator is a [Kubernetes native
application](https://kubernetes.io/docs/concepts/extend-kubernetes/extend-cluster/)
developed to deploy and configure StorageOS clusters, and assist with
maintenance operations. We recommend its use for standard installations. 

The StorageOS operator can be installed with Helm.

### Install

```bash
helm repo add storageos https://charts.storageos.com
helm install storageos/storageoscluster-operator --namespace storageos-operator
```

> The Helm chart can be found in the [Charts public
> repository](https://github.com/storageos/charts).

> The StorageOS Cluster Operator source code can be found in the
> [cluster-operator repository](https://github.com/storageos/cluster-operator).

The operator is a Kubernetes controller that watches the `StorageOSCluster`
CRD. Once the controller is ready, a StorageOS cluster definition can be
created. The operator will deploy a StorageOS cluster based on the
configuration specified in the cluster definition.

### Create a Secret

Before deploying a StorageOS cluster, create a Secret to define the StorageOS
API Username and Password in base64 encoding. 

```bash
kubectl create -f - <<END
apiVersion: v1
kind: Secret
metadata:
  name: "storageos-api"
  namespace: "default"
  labels:
    app: "storageos"
type: "kubernetes.io/storageos"
data:
  # echo -n '<secret>' | base64
  apiUsername: c3RvcmFnZW9z
  apiPassword: c3RvcmFnZW9z
END
```

This example contains a default password, for production installations, use a
unique, strong password.

> Make sure that the encoding of the credentials doesn't have special characters such as '\n'.

> You can define a base64 value by `echo -n "mystring" | base64`.

## Trigger a StorageOS installation

> This is a Cluster Definition example. 
```bash
kubectl create -f - <<END
apiVersion: "storageos.com/v1alpha1"
kind: "StorageOSCluster"
metadata:
  name: "example-storageos"
spec:
  secretRefName: "storageos-api" # Reference from the Secret created in the previous step
  secretRefNamespace: "default"  # Namespace of the Secret
  images:
    nodeContainer: "storageos/node:1.0.0-rc5" # StorageOS version
  resources:
    requests:
    memory: "128Mi"
END
```
> `spec` parameters available on the [Cluster Operator configuration]({%link _docs/reference/cluster-operator/configuration.md %}) page.

> You can find more examples such as deployments with CSI or deployments referencing a external etcd kv store.
store for StorageOS in the [Cluster Operator examples]({%link _docs/reference/cluster-operator/examples.md %}) page.