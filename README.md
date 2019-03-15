# Ansible Role: fstab/cifs

An Ansible Role that enables the cifs volumes in your kubernetes cluster.
It is based on binary [fstab/cifs](https://github.com/fstab/cifs) created by Fabian Stäber.
It is compatible with Debian/Ubuntu node(s).

## Requirements

Role is self sustainable - it installs all needed prerequisites on the target node(s).

## Role Variables

Available variables are listed below along with their default values:

```yaml
k8s_volume_plugins_directory: /usr/libexec/kubernetes/kubelet-plugins/volume/exec
fstab_cifs_directory: fstab~cifs
fstab_cifs_download_url: https://raw.githubusercontent.com/fstab/cifs/master/cifs
```

## Example Playbook

```yaml
    - hosts: snipeit
      roles:
         - role: mylocaldevstack.fstab_cifs
```

if your cluster has volume plugins in other directory (because you're running your kubelet with `--volume-plugin-dir=/some/other/directory`):

```yaml
    - hosts: snipeit
      vars:
        k8s_volume_plugins_directory: /some/other/directory
      roles:
         - role: mylocaldevstack.fstab_cifs
```

##  How to use this plugin?
First, you have to create a secret containing username and password to your cifs share:

```yaml

apiVersion: v1
kind: Secret
metadata:
  name: cifs-secret
  namespace: default
type: fstab/cifs
data:
  username: ... #base64 encoded username
  password: ... #base64 encoded password

```

then you need to add a volume in your deployment:

```yaml
apiVersion: v1
...
spec:
  containers:
  - name: SOMENAME
    ...
    volumeMounts:
    - name: test          # reference to spec.volumes.name
      mountPath: /data    # mountpoint in the pod
  volumes:
  - name: test
    flexVolume:
      driver: "fstab/cifs"
      fsType: "cifs"
      secretRef:
        name: "cifs-secret"   # reference to deployed secret
      options:
        networkPath: "//HOSTAME/SHARENAME"
        mountOptions: "dir_mode=0755,file_mode=0755,noperm"
```

## License

Apache
