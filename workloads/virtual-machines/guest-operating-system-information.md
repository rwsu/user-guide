# Guest Operating System Information

Guest operating system identity for the Virtual Machine will be provided by the label `kubevirt.io/os` :

```text
metadata:
  name: myvm
  labels:
    kubevirt.io/os: win2k12r2
```

The `kubevirt.io/os` label is based on the short OS identifier from [libosinfo](https://libosinfo.org/) database. The following Short IDs are currently supported:

| Short ID | Name | Version | Family | ID |
| --- | --- | --- | --- | --- |
| **win2k12r2** | Microsoft Windows Server 2012 R2 | 6.3 | winnt | [http://microsoft.com/win/2k12r2](http://microsoft.com/win/2k12r2) |

## Use with presets

A Virtual Machine Preset representing an operating system with a `kubevirt.io/os` label could be applied on any given Virtual Machine that have and match the`kubevirt.io/os` label.

Default presets for the OS identifiers above are included in the current release.

### Windows Server 2012R2 `VirtualMachinePreset` Example

```yaml
apiVersion: kubevirt.io/v1alpha1
kind: VirtualMachinePreset
metadata:
  name: windows-server-2012r2
  selector:
    matchLabels:
      kubevirt.io/os: win2k12r2
spec:
  domain:
    cpu:
      cores: 2
    resources:
      requests:
        memory: 2G
    features:
      acpi: {}
      apic: {}
      hyperv:
        relaxed: {}
        vapic: {}
        spinlocks:
          spinlocks: 8191
    clock:
      utc: {}
      timer:
        hpet:
          present: false
        pit:
          tickPolicy: delay
        rtc:
          tickPolicy: catchup
        hyperv: {}
---
apiVersion: kubevirt.io/v1alpha1
kind: VirtualMachine
metadata:
  labels:
    kubevirt.io/os: win2k12r2  
  name: windows2012r2
spec:
  terminationGracePeriodSeconds: 0
  domain:
    firmware:
      uuid: 5d307ca9-b3ef-428c-8861-06e72d69f223
    devices:
      disks:
      - name: server2012r2
        volumeName: server2012r2
        disk:
          dev: vda
  volumes:
    - name: server2012r2
      persistentVolumeClaim:
        claimName: my-windows-image
```

Once the `VirtualMachinePreset` is applied to the `VirtualMachine`, the resulting resource would look like this:

```yaml
apiVersion: kubevirt.io/v1alpha1
kind: VirtualMachine
metadata:
  annotations:
    presets.virtualmachines.kubevirt.io/presets-applied: kubevirt.io/v1alpha1
    virtualmachinepreset.kubevirt.io/windows-server-2012r2: kubevirt.io/v1alpha1
  labels:
    kubevirt.io/os: win2k12r2  
  name: windows2012r2
spec:
  terminationGracePeriodSeconds: 0
  domain:
    cpu:
      cores: 2
    resources:
      requests:
        memory: 2G      
    features:
      acpi: {}
      apic: {}
      hyperv:
        relaxed: {}
        vapic: {}
        spinlocks:
          spinlocks: 8191
    clock:
      utc: {}
      timer:
        hpet:
          present: false
        pit:
          tickPolicy: delay
        rtc:
          tickPolicy: catchup
        hyperv: {}
    firmware:
      uuid: 5d307ca9-b3ef-428c-8861-06e72d69f223
    devices:
      disks:
      - name: server2012r2
        volumeName: server2012r2
        disk:
          dev: vda
  volumes:
    - name: server2012r2
      persistentVolumeClaim:
        claimName: my-windows-image
```

For more information see [Virtual Machine Presets](presets.md)

