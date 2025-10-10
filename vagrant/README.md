# Vagrant
Vagrant VMs are **ONLY** used to test cases which cannot be tested in
containers (kernel, firmware, proc, systemd, networking, etc) these should
**never** be default test cases. Vagrant is only used to manage
[VirtualBox](../virtualbox/README.md) VMs.

[Setup](setup.md)

[Testing](testing.md)

[Manual VM](manual_vm.md)

[Troubleshooting](troubleshooting.md)
