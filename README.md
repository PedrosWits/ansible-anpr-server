Ansible Role: anpr-server
=========

Setup a SQL server database containing Number Plate Data, generated by networks of ANPR cameras.

Requirements
------------

None.

Role Variables
--------------

Available variables with corresponding default values:

```
destination: "/home/{{ ansible_ssh_user }}/anpr-server"

volumes:
  - {blkid: '/dev/vdb', mount_dir: 'dbfiles', fstype: 'ext4'}
  - {blkid: '/dev/vdc', mount_dir: 'tempdb', fstype: 'ext4'}
  - {blkid: '', mount_dir: 'bakfile', fstype: 'ext4'}

max_ram: 7983 #GB

sql_password: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          30666434643766303462353638623735666166623239363530373237343762643234613463383732
          3630393036623931666238613866353231643031336534380a303536306439313838363730656637
          38653065343239343736366638343062373562333735613930646336366539333330666133393431
          3834626363643261330a646435663532633165613836613431363765643037613265333866363033
          3335

attach: True
```

Volume blkids on ubuntu are given by order of attach hence when attaching volumes to the instance, follow the order above or modify these variables. This can  work differently for other OS'es. Thus this isn't an optimal, scalable or robust solution, but works for now. Will only try to mount volumes for which a blkid is defined.

`max_ram` defines the maximum ram to be used by sql-server.

The password to be used by sql server should be provided via ansible vault.

Dependencies
------------

None

Example Playbook
----------------

    - hosts: sql_server
      roles:
         - role: pedroswits.anpr_server
           max_ram: 2048
           attach: False

License
-------

MIT

Author Information
------------------
This role was created in 2018 by Pedro Pinto da Silva to help setup the computing resources necessary to reproduce his PhD.
