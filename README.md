# oc-mirror-demo

1. Navigate to the Downloads page of the OpenShift Cluster Manager Hybrid Cloud Console. Under the OpenShift disconnected installation tools section, click Download for OpenShift Client (oc) mirror plugin and save the file.
2. Extract the archive: \
`$ tar xvzf oc-mirror.tar.gz`
3. Install the oc-mirror CLI plugin by placing the file in your PATH and adjust the permission mode to make it executable.
4. Login to the Red Hat registry: \
`$ podman login registry.redhat.io`
5. Create an ImageSetConfiguration manifest:
`$ oc mirror init --registry quay.io/csarta/oc-mirror-metadata > imageset-config.yaml`
6. Add to the ImageSetConfiguration the images you want to make available for your registry. You can find some samples here: https://docs.openshift.com/container-platform/4.12/installing/disconnected_install/installing-mirroring-disconnected.html#oc-mirror-image-set-examples_installing-mirroring-disconnected
7. Generate the needed manifest to apply in your cluster:
`$ oc mirror --config=imageset-config.yaml file://mirror`
8. Apply the YAML files from the results directory to the cluster:
`$ oc apply -f ./oc-mirror-workspace/results-[...]/`
9. Verify that the ImageContentSourcePolicy and CatalogSource resources were successfully installed:
`$ oc get imagecontentsourcepolicy --all-namespaces`
`$ oc get catalogsource --all-namespaces`

Here's a sample of the generated folder with all the needed manifest, for reference:

```
csarta@fedora:~$ tree mirror/
mirror/
└── oc-mirror-workspace
    └── src
        ├── catalogs
        │   └── registry.redhat.io
        │       └── redhat
        │           └── redhat-operator-index
        │               └── v4.12
        │                   ├── bin
        │                   │   └── opm
        │                   ├── cacheLocation.txt
        │                   ├── include-config.gob
        │                   ├── index
        │                   │   └── index.json
        │                   └── layout
        │                       ├── blobs
        │                       │   └── sha256
        │                       │       ├── ab...
        │                       │       └── ..ab
        │                       ├── index.json
        │                       └── oci-layout
        ├── charts
        ├── publish
        ├── release-signatures
        │   └── signature-[...].json
        └── v2
```
I'm omitting all the files in the subfolder, as I just wrote this is just a sample.
