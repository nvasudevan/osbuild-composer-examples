# Steps to create an OS build using osbuild-composer

to check what types are supported:
composer-cli compose types

to list blueprints:
composer-cli blueprints list

to save the blueprint:
sudo composer-cli blueprints push <blueprint toml>

to check if dependancies resolve:
sudo composer-cli blueprints depsolve <blueprint name>

start image create as `qcow2`, this adds to the queue
composer-cli compose start <blueprint name> qcow2

to check the status of the composition:
sudo composer-cli compose status

to check if there were any errors during image create process:
sudo composer-cli compose log <UUID of the image>

to check what packages an image contains:
sudo composer-cli compose info <UUID of the image>

to see metadata in json format:
sudo composer-cli compose metadata <UUID of the image>

to download the image by UUID:
sudo composer-cli compose image <UUID of the image>

to spin up a VM from the `qcow2` image:
sudo qemu-kvm --name test-vm -m 1024 -hda <qcow2 image file>

to sping up a VM from the `vhd` image:
sudo qemu-kvm --name test-vm -m 1024 -drive format=raw,file=<vhd file>
