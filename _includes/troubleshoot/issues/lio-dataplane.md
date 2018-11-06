## LIO not enabled

### Issue:

StorageOS node can't start and shows the following log entries.

```bash
time="2018-09-24T14:34:40Z" level=error msg="liocheck returned error" category=liocheck error="exit status 1" module=dataplane stderr="Sysfs root '/sys/kernel/config/target' is missing, is kernel configfs present and target_core_mod loaded? category=fslio level=warn\nRuntime error checking stage 'target_core_mod': SysFs root missing category=fslio level=warn\nliocheck: FAIL (lio_capable_system() returns failure) category=fslio level=fatal\n" stdout=
time="2018-09-24T14:34:40Z" level=error msg="failed to start dataplane services" error="system dependency check failed: exit status 1" module=command
```

### Reason:

This indicates that one or more kernel modules required for StorageOS are
not loaded.

### Doublecheck

The following kernel modules must be enabled in the host.

```bash
lsmod  | egrep "^tcm_loop|^target_core_mod|^target_core_file|^configfs"
```

### Solution:

Install the required kernel modules (usually found in the
`linux-image-extra-$(uname -r)` package of your distribution) on your nodes
following this [prerequisites page]({%link
_docs/prerequisites/systemconfiguration.md %}) and restart the container.
