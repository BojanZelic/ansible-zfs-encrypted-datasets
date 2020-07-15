# Ansible ZFS Encrypted Dataset Playbook

This playbook and role allows you to define a list of encrypted ZFS datasets.

I used this on my proxmox setup, but it should be available 
to use in all ZFS environments.

Clone this repo into your roles directory:

```
git clone git@github.com:BojanZelic/ansible-zfs-encrypted-datasets.git roles/zfs_datasets
```

Add it to your play's roles:
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

Encryption works by loading the encryption key to zfs. The playbook then tries to mount
the dataset. 

Because it's encrypted... when you reboot the server, you'll need to run the 
playbook to mount the dataset again.