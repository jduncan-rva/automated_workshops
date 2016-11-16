Role Name
=========

This role sets up a Satellite 6.2 server and creates a hammer script designed to configure a RHEL 7 SOE environment in a new organization.  The pulp target directory is moved to the data disk which is set to 30 GB.  The SOE includes 7Server, Satellite Tools, RH Common, Extras, and the 7Server Kickstart tree.  This SOE weighs in at ~23GB.

You'll need to create a Satellite 6 in the Portal and download the manifest file to the files/ directory.

To sync channels and create the orgs, log into the satellite as `cloud-user`  and run the two hammer scripts in tmp:
 `bash -x  /tmp/hammer_sync && bash -x /tmp/hammer_env`

Role Variables
--------------

Defaults:
Satellite Admin user / pass:
  `admin_user: admin`
  `admin_pass: redhat`

Inital organization and location:
  `initial_org: "DLT Solutions"`
  `initial_loc: HQ`

Satellite manifest zip file:
  `manifest_file: manifest_f0fc9a2f-f802-4f99-9bc8-6165f45afb7d.zip`

Used from other roles:
Common Defaults:
  `sub_poolid` Subscription Pool must have Satellite available.
KVM Hypervisor Defaults:
  `data_disk_size` Increase to handle the pulp directory

Dependencies
------------

No external dependencies

Example Playbook
----------------

- hosts: sat62
  vars:
    data_disk_size: '30G'
  roles:
  - common
  - kvm_hypervisor
  - satellite6

License
-------

Apache

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
