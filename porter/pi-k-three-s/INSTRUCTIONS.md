This bundle installs the lightweight Kubernetes distribution [k3s](https://github.com/rancher/k3s) onto a cluster of Raspberry Pi devices, and configures Tiller on the cluster so you can use [Helm](https://helm.sh/) to install Kubernetes applications.

## Prerequisites

In order to use this bundle, you must first prepare your Raspberry Pi device(s).

(Note - this bundle has been tested with Raspberry Pi 4)

Follow these steps to prepare each Pi device you want to use in the cluster:

### Flash SD card

Download and install [balanaEtcher](https://www.balena.io/etcher/) then use the program to flash the [Rasbpian Lite image](https://downloads.raspberrypi.org/raspbian_lite_latest) onto a mounted SD card.

### Configure SSH and wifi

Follow the [instructions here](https://www.raspberrypi.org/documentation/configuration/wireless/headless.md) to add files to boot partition of the SD card so that the Pi will automatically connect to your wifi network and enable SSH on first boot.

Insert the SD card into the Pi and power it on.

### Update Pi config

Connect to the Pi via SSH. By default, the username will be `pi` and the hostname will be `raspberrypi` so you can connect using:

```
ssh pi@raspberrypi
```

The default password will be `raspberry`.

Open the Raspberry Pi config UI using:

```
sudo raspbi-config
```
In the config UI, update the password to something more secure, and update the hostname (hint: if using multiple Pis in the cluster, give them identifiable names, e.g. `pi-master`, `pi-node-01`, etc.).

Reboot the Pi.

### Add SSH key

You will need to upload the public part of an SSH key to the Pi. Later, we will use the private key for authenticating with the Pi. Use the same SSH key for each Pi.

If you haven't got an SSH key generated, you can generate one using:

```
ssh-keygen
```

Upload the public key to the Pi using the following commands:
```
ssh pi@<hostname> mkdir ~/.ssh
scp <path-to-public-key> pi@<hostname>:~\.ssh\authorized_keys
```

## Running the bundle

Once the Raspberry Pi devices are setup according the instructions above, the bundle can be installed.

It's important to note that unless the Raspberry Pi devices are accessible via the Internet, then you will have to run this bundle locally on a device connected to the same network as the Pis, and use the Docker driver for Porter (rather than the Azure driver).

You will need to have Porter and Docker installed locally, then you can use the following commands:

```
porter creds generate --tag cnabquickstarts.azurecr.io/porter/pi-k-three-s/bundle:latest

porter install --tag cnabquickstarts.azurecr.io/porter/pi-k-three-s/bundle:latest --cred pi-k-three-s --param master_ipAddress=<master_ipAddress>,workers_ipAddress=<workers_ipAddress>
```

See [Parameters and Credentials](#parameters-and-credentials) below for more details on the parameters.