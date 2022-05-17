# Steps to create an OS build using osbuild-composer

## Create image from blueprint

For details on blueprint reference, see [here](https://www.osbuild.org/guides/blueprint-reference/blueprint-reference.html)

- to save the blueprint [example blueprint](see https://github.com/nvasudevan/osbuild-composer-examples/blob/main/blueprints/composer-f36.toml):
```
sudo composer-cli blueprints push <blueprint toml>
```

- to check if dependancies resolve:
```
sudo composer-cli blueprints depsolve <blueprint name>
```

- start image create as `qcow2`, this adds to the queue
```
sudo composer-cli compose start <blueprint name> qcow2
```

- to check the status of the composition:
```
sudo composer-cli compose status
```

- to check if there were any errors during image create process:
```
sudo composer-cli compose log <UUID of the image>
```

- to check what packages an image contains:
```
sudo composer-cli compose info <UUID of the image>
```

- to see metadata in json format:
```
sudo composer-cli compose metadata <UUID of the image>
```

- to download the image by UUID:
```
sudo composer-cli compose image <UUID of the image>
```

## VM spin up

- to spin up a VM from the `qcow2` image:
```
sudo qemu-kvm --name test-vm -m 1024 -hda <qcow2 image file>
```

- to spin up a VM from the `vhd` image:
```
sudo qemu-kvm --name test-vm -m 1024 -drive format=raw,file=<vhd file>
```

## Thirdparty repos support

To add packages from thirdparty repos such as rpmfusion-free, follow 
instructions from [here](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html-single/composing_a_customized_rhel_system_image/index#image-builder-default-system-repositories_managing-repositories)

For e.g., for fedora 36, using `sudo`:
```
mkdir -p /etc/osbuild-composer/repositories
cp /usr/share/osbuild-composer/repositories/fedora-36.json /etc/osbuild-composer/repositories/fedora-36.json
```
Modify the `x86_64` block to add an entry for `rpmfusion-free`.
Then copy the metalink and gpg keys from `/etc/yum.repos.d/rpmfusion-free.repo`.
The gpg key has to be entered as one line, use the other entries in the 
fedora-36.json file as reference.

## Other useful commands

- to check what types are supported:
```
composer-cli compose types
```

- to list blueprints:
```
composer-cli blueprints list
```
