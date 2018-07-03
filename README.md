glusterfs_client
================

Ansible role which helps to install and configure GlusterFS client.

The configuration of the role is done in such way that it should not be
necessary to change the role for any kind of configuration. All can be
done either by changing role parameters or by declaring completely new
configuration as a variable. That makes this role absolutely
universal. See the examples below for more details.

Please report any issues or send PR.


Examples
--------

```yaml
---

- name: Example of how to only install GlusterFS client
  hosts: all
  roles:
    - glusterfs_client

- name: Example of how to install GlusterFS and mount volumes
  hosts: all
  vars:
    glusterfs_client_volumes:
      # Mount 192.168.169.10:/test1 to /mnt and make 192.168.169.11 and
      # 192.168.169.12 as the backupvolfile server
      - name: test1
        servers:
          # First server is used as the mount source
          - 192.168.169.10
          # Any further server is used as backupvolfile server
          - 192.168.169.11
          - 192.168.169.12
        ### Optional settings:
        # Mount point
        #dir: /data
        # Mount point directory permissions
        #dir_owner: root
        #dir_group: www-data
        #dir_mode: "0775"
        # Additional mount options
        #opts: log-level=WARNING,log-file=/var/log/gluster.log
  roles:
    - glusterfs_client
```


Role variables
--------------

```yaml
# GlusfterFS release
glusterfs_client_yumrepo_release: "{{
  '3.10'
    if (
      ansible_distribution == 'Ubuntu' and
      ansible_distribution_major_version | int < 16
    ) else
  '3.13'
    if (
      (
        ansible_os_family == 'RedHat' and
        ansible_distribution_major_version | int < 7
      ) or (
        ansible_distribution == 'Debian' and
        ansible_distribution_major_version | int < 9
      )
    ) else
  '4.1' }}"

# YUM repo URL
glusterfs_client_yumrepo_url: https://buildlogs.centos.org/centos/{{ ansible_distribution_major_version }}/storage/$basearch/gluster-{{ glusterfs_client_yumrepo_release }}/

# APT key
glusterfs_client_apt_key: "{{
  'https://download.gluster.org/pub/gluster/glusterfs/' ~ glusterfs_server_yumrepo_release ~ '/rsa.pub'
    if ansible_distribution == 'Debian'
    else
  '' }}"

# APT repo URL
glusterfs_client_apt_url: "{{
  'ppa:gluster/glusterfs-' ~ glusterfs_client_yumrepo_release
    if ansible_distribution == 'Ubuntu'
    else
  'deb https://download.gluster.org/pub/gluster/glusterfs/' ~ glusterfs_client_yumrepo_release ~ '/LATEST/Debian/' ~ ansible_distribution_release ~ '/amd64/apt ' ~ ansible_distribution_release ~  ' main' }}"

# Package to be installed (explicit version can be specified here)
glusterfs_client_pkg: glusterfs-client

# Location of the secure-access file
glusterfs_client_secure_access_file: /var/lib/glusterd/secure-access

# Whether to create the secure-access file or not
glusterfs_client_secure_access: no

# Default mount options for all volumes
glusterfs_client_volume_default_opts: defaults

# Default mount point attributes
glusterfs_client_dir: /mnt
glusterfs_client_dir_owner: root
glusterfs_client_dir_group: root
glusterfs_client_dir_mode: "0750"

# Volumes to mount (see README for examples)
glusterfs_client_volumes: []
```


Dependencies
------------

- [`glusterfs_server`](https://github.com/jtyr/ansible-glusterfs_server) (optional)
- [`ssl_cert`](https://github.com/jtyr/ansible-ssl_cert) (optional)


License
-------

MIT


Author
------

Jiri Tyr
