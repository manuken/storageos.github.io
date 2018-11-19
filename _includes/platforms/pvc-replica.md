```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-vol-1
  labels:
      storageos.com/replicas: "1"
  annotations:
    volume.beta.kubernetes.io/storage-class: fast
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```
