## LIO Init:Error

### Issue:

StorageOS pods not starting with `Init:Error`
```bash
{{ page.cmd }} -n storageos get pod
# output of error
```

### Reason:
It indicates that since the Linux open source SCSI drivers are not enabled, StorageOS cannot start.
The StorageOS DaemonSet enables the required kernel modules from the host system. If you are seeing these errors is
because that container couldn't load the modules.

### Doublecheck
Check the logs of the init container.

```bash
{{ page.cmd }} -n storageos logs $ANY_STORAGEOS_POD -c enable-lio
```

In case of failure, it will show the following output, indicating that the kernel modules couldn't
be loaded or that they are not properly configured:

```bash
# INIT OUTPUT
```

### Solution:
Install the kernel modules (usually found in the linux-image-extra-$(uname -r) package of your distribution) in your nodes following this [prerequisites page]({%link
_docs/prerequisites/systemconfiguration.md %}) and delete StorageOS pods, so the DaemonSet creates the pods again.
