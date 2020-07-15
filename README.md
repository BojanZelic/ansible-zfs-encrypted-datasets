bojanzelic.zfs_datasets
=========

This playbook and role allows you to define a list of encrypted ZFS datasets.

I used this on my proxmox setup, but it should be available 
to use in all ZFS environments.

Encryption works by loading the encryption key to zfs. The playbook then tries to mount
the dataset. 

Because it's encrypted... when you reboot the server, you'll need to run the 
playbook to mount the dataset again.

Installing
------------

Clone this repo into your roles directory:

```
git clone git@github.com:BojanZelic/ansible-zfs-encrypted-datasets.git roles/bojanzelic.zfs_datasets
```

Alternatively

```
ansible-galaxy install bojanzelic.zfs_datasets 
```

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

Pass in the encryption key as an environmental variable `ZFS_KEY`

Example Playbook
----------------

```
- hosts: all
  roles:
    - role: bojanzelic.zfs_datasets
      zfs_key: "{{ lookup('env','ZFS_KEY') }}"
      zfs_datasources:
        rpool:
          state: present
        rpool/backups:
          encrypted: true
          extra_zfs_properties:
            sharenfs: rw=@192.168.1.1/24
        rpool/documents:
          encrypted: true
        rpool/personal_media:
          state: present
        rpool/media:
          state: present
```

License
-------

GNU General Public License v3.0

Author Information
------------------

Contact me at https://bojan.zelic.io
