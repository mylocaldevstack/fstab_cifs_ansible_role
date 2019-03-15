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

## License

Apache
