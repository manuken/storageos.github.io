## LIO Init:Error

### Issue:

StorageOS pods not starting with `Init:Error`
```bash
{{ page.cmd }} -n storageos get pod
NAME              READY     STATUS              RESTARTS   AGE
storageos-2kwqx   0/1       Init:Err             0          6s
storageos-cffcr   0/1       Init:Err             0          6s
storageos-d4f69   0/1       Init:Err             0          6s
storageos-nhq7m   0/1       Init:Err             0          6s
```

### Reason:
This indicates that since the Linux open source SCSI drivers are not enabled,
StorageOS cannot start.  The StorageOS DaemonSet enables the required kernel
modules from the host system. If you are seeing these errors it is because that
container couldn't load the modules.

### Assert
Check the logs of the init container.

```bash
{{ page.cmd }} -n storageos logs $ANY_STORAGEOS_POD -c enable-lio
```

In case of failure, it will show the following output, indicating which kernel
modules couldn't be loaded or that they are not properly configured:

```bash
Checking configfs
configfs mounted on sys/kernel/config
Module target_core_mod is not running
executing modprobe -b target_core_mod
Module tcm_loop is not running
executing modprobe -b tcm_loop
modprobe: FATAL: Module tcm_loop not found.
```

### Solution:
Install the required kernel modules (usually found in the
`linux-image-extra-$(uname -r)` package of your distribution) on your nodes
following this [prerequisites page]({%link
_docs/prerequisites/systemconfiguration.md %}) and delete StorageOS pods,
allowing the DaemonSet to create the pods again.

