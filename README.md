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
      # Mount 192.168.169.10:/test1 to /mnt and make 192.168.169.11 as the
      # backupvolfile server
      - name: test1
        servers:
          # First server is used as the mount source
          - 192.168.169.10
          # Any further server is used as backupvolfile server
          - 192.168.169.11
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

- name: GlusterFS client installation
  hosts: ~gclient
  vars:
  roles:
    - glusterfs_client
```


Role variables
--------------

```yaml
# GlusfterFS release
glusterfs_client_yumrepo_release: "{{
  '3.13'
    if ansible_distribution_major_version | int == 6
    else
  '4.0' }}"

# YUM repo URL
glusterfs_client_yumrepo_url: https://buildlogs.centos.org/centos/{{ ansible_distribution_major_version }}/storage/$basearch/gluster-{{ glusterfs_client_yumrepo_release }}/

# Package to be installed (explicit version can be specified here)
glusterfs_client_pkg: glusterfs-client

# Default mount point attributes
glusterfs_client_dir: /mnt
glusterfs_client_dir_owner: root
glusterfs_client_dir_group: root
glusterfs_client_dir_mode: "0750"
```


Dependencies
------------

- [`glusterfs_server`](https://github.com/jtyr/ansible-glusterfs_server) (optional)


License
-------

MIT


Author
------

Jiri Tyr
