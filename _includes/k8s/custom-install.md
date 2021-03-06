## Custom Installations

There are a variety of flavours, versions and particularities in the container
orchestrator scope. Because of this, StorageOS installation procedures aim to
be flexible so they can fit different needs depending on the environment,
preferences or requirements. The StorageOS cluster operator simplifies the installation
by implementing automated install. You can review and adapt the StorageOS install
in the following examples. Feel free to extend and modify the publicly available
examples.

### Installation with Native Drivers (default)

The following github repository hosts installation examples.

{% if page.platform == "openshift" %}
```bash
git clone https://github.com/storageos/deploy.git storageos
cd storageos/openshift/deploy-storageos
```
{% else %}
```bash
git clone https://github.com/storageos/deploy.git storageos
cd storageos/k8s/deploy-storageos
```
{% endif %}

You can see various installation examples are available such as `standard`, 
`CSI (Container Storage Interface)` or `etcd-as-svc`. All of them have a
`deploy-storageos.sh` that serves as a wrapper to trigger the manifest
creation. Follow the according `README.md` for each one of them for more
details.

For advanced installations, it is recommended to refer to `etcd-as-svc` that
will guide the user to deploy an etcd cluster deployed by the official
Kubernetes [etcd operator](https://github.com/coreos/etcd-operator). Once an
external etcd cluster has been created, then use the StorageOS cluster operator
to finish the installation.
