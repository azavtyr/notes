When I was trying to power on the VM, I met an error telling me that Windows can't find Microsoft Software Licensing Terms.

What's happened is that when you first create a VM, VMware adds floppy drive, which Windows thinks contains activation file (or something like it), which it doesn't, so the installation will fail all the time.

To solve this problem,

1. Start the VM
2. Wait until Windows starts to load
3. Shut the VM down
4. Remove floppy disk from the config