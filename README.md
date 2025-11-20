# OpenShift Appliance Builder Example
The purpose of this repo is to demonstrate how to build an openshift-appliance using a local registry cache.  

# Overview

## Files
The following files are required to get this to work.

### podman-bin.sh
This file lets the mirror config be passed along to the second podman run in podman.  You don't need to modify this file.

### registries.conf
This file sets the repo mirrors.  At a minimum you need to add entries for `quay.io` and `registry.redhat.io`.  Depending on what operators are being bundled, you many need to add additional entires for those repos.

### appliance-confg.yaml
A very simple `appliance-config.yaml` is included for demo purposes.  You will need to update the `pullSecret` parameter to include authentication for your local mirror repo.  The sample file has an example.  You also need to include `quay.io` as the process uses `oc image extract` which ignore the mirror settings.  `oc image extract` is only used for the initial image ~1 GB.

## Run It.

This example assumes you've cloned this repo to your home directory.
```
cd ~
git clone https://github.com/themoosman/openshift-appliance-builder-proxy.git
```

Create the Image
```
sudo podman run --rm -it --pull newer --privileged --net=host \
-v /home/kmoos/openshift-appliance-builder-proxy/podman-bin.sh:/root/.local/bin/podman \
-v /home/kmoos/openshift-appliance-builder-proxy/data:/assets:Z \
-v /home/kmoos/openshift-appliance-builder-proxy/registries.conf:/etc/containers/registries.conf:ro \
quay.apps.moos.red/registry_redhat_proxy/assisted/agent-preinstall-image-builder-rhel9:1.0.1 \
build --log-level=debug
```

# Sources
* [OpenShift Appliance GitHub](https://github.com/openshift/appliance/blob/master/docs/user-guide.md#generate-a-cluster-configuration-image) 
* [ocp-appliance-gui](https://github.com/kenmoini/ocp-appliance-gui/)