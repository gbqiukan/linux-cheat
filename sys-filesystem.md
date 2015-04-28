# sys filesystem

## /sys/bus/pci/devices

## /sys/bus/pci_express/devices

<http://prefetch.net/articles/linuxpci.html>

PCI bus information.

`lspci` is a good way to extract human readable information from those directories.

    ls /sys/bus/pci/devices | head -n 4

gives something like:

    lrwxrwxrwx 1 root root 0 Apr 19 08:34 0000:00:00.0 -> ../../../devices/pci0000:00/0000:00:00.0
    lrwxrwxrwx 1 root root 0 Apr 19 08:34 0000:00:01.0 -> ../../../devices/pci0000:00/0000:00:01.0
    lrwxrwxrwx 1 root root 0 Apr 19 08:34 0000:00:02.0 -> ../../../devices/pci0000:00/0000:00:02.0
    lrwxrwxrwx 1 root root 0 Apr 19 08:34 0000:00:14.0 -> ../../../devices/pci0000:00/0000:00:14.0

Breaking down an ID of form `0000:01:02.3`

- `0000`: PCI domain (each domain can contain up to 256 PCI buses)
- `01`: the bus number the device is attached to
- `02`: the device number
- `3`: PCI device function

If we go then under one of the directories and `ls`:

    ls -l /sys/bus/pci/devices/0000:00:00.0

Output:

    total 0
    drwxr-xr-x 2 root root    0 Apr 19 06:35 power
    -rw-r--r-- 1 root root 4096 Apr 19 10:09 broken_parity_status
    -r--r--r-- 1 root root 4096 Apr 19 06:35 class
    -rw-r--r-- 1 root root  256 Apr 19 06:35 config
    -r--r--r-- 1 root root 4096 Apr 19 10:09 consistent_dma_mask_bits
    -rw-r--r-- 1 root root 4096 Apr 19 10:09 d3cold_allowed
    -r--r--r-- 1 root root 4096 Apr 19 06:35 device
    -r--r--r-- 1 root root 4096 Apr 19 10:09 dma_mask_bits
    -rw-r--r-- 1 root root 4096 Apr 19 10:09 enable
    -r--r--r-- 1 root root 4096 Apr 19 06:35 irq
    -r--r--r-- 1 root root 4096 Apr 19 10:09 local_cpulist
    -r--r--r-- 1 root root 4096 Apr 19 10:09 local_cpus
    -r--r--r-- 1 root root 4096 Apr 19 10:09 modalias
    -rw-r--r-- 1 root root 4096 Apr 19 10:09 msi_bus
    -r--r--r-- 1 root root 4096 Apr 19 10:09 numa_node
    --w--w---- 1 root root 4096 Apr 19 10:09 remove
    --w--w---- 1 root root 4096 Apr 19 10:09 rescan
    -r--r--r-- 1 root root 4096 Apr 19 06:35 resource
    lrwxrwxrwx 1 root root    0 Apr 19 06:36 subsystem -> ../../../bus/pci
    -r--r--r-- 1 root root 4096 Apr 19 10:09 subsystem_device
    -r--r--r-- 1 root root 4096 Apr 19 10:09 subsystem_vendor
    -rw-r--r-- 1 root root 4096 Apr 19 08:34 uevent
    -r--r--r-- 1 root root 4096 Apr 19 06:35 vendor

Some interesting files:

    cat class vendor device

Sample output:

    0x060000
    0x8086
    0x0154

Those IDs can be decoded with the `/usr/share/hwdata/pci.ids` file.

### /usr/share/hwdata/pci.ids

We can then find what PCI IDs correspond to with:

    less /usr/share/hwdata/pci.ids

which is provided by the hwdata package <https://git.fedorahosted.org/hosted/hwdata.git/>. If we search for `^8086` we find:

    8086 Intel Corporation
        0154  3rd Gen Core processor DRAM Controller

### TODO

- why do many devices are shown under PCI, but I think PCI was replaced by PCIe a long time ago.

### update-pciid

Updates

### Bibliography

<http://prefetch.net/articles/linuxpci.html>