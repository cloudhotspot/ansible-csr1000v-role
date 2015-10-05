# ansible-csr1000v-role

An Ansible role for automating deployment of the Cisco Cloud Services Router (CSR) 1000V on VMWare Fusion.  

The role creates and configures an OVF runtime environment that the CSR 1000V can use to provision configuration settings upon deployment.

##Requirements

- Mac OS X
- <a href="http://www.vmware.com/products/fusion" target="_blank">VMWare Fusion</a> 7.x or higher
- <a href="https://www.vmware.com/support/developer/ovf/" target="_blank">VMWare OVF Tools</a> 4.1 or higher (VMWare account may be required)
- <a href="https://software.cisco.com/download/release.html?mdfid=284364978&softwareid=282046477&release=3.14.1S&relind=AVAILABLE&rellifecycle=ED&reltype=latest" target="_blank">Cisco CSR 1000v OVA Image</a> (CCO login required)

##Role Variables

You must specify the following variables in your playbook:

```yaml
# Location of the Cisco CSR 1000V OVA image 
csr_ova_source: "/path/to/ova/source"

# Root folder where the Cisco CSR 1000V virtual machine that will be created
csr_vm_root: "/path/to/root"
```

The CSR 1000V virtual machine will be deployed to the following location:

`{{ csr_vm_root }}/{{ csr_vm_name }}.vmwarevm/`

For example, if `csr_vm_root` is **/Users/alice/guests** and `csr_vm_name` is **csr01** the virtual machine will be deployed to **/Users/alice/guests/csr01.vmwarevm**.

If the virtual machine already exists, by default the role will fail.  To overwrite the existing virtual machine, the following variable must be set (to any value):

`csr_vm_overwrite: yes`

###Default Role Variables

```yaml
# Name of the Cisco CSR 1000V virtual machine that will be created
csr_vm_name: "csr01"

# Last Octet of IP address assigned to CSR 1000V management interface.  This value should be between 3 and 127.
csr_vm_mgmt_ip_octet: "120"

# Management Interface - 0 = Ethernet0/GigabitEthernet1, 1 = Ethernet1/GigabitEthernet2, 2 = Ethernet2/GigabitEthernet2
csr_vm_mgmt_interface: 2

# Keep the DHCP reservation used for provisioning
csr_vm_persist_dhcp_reservation: yes

# CSR 1000V configuration variables
csr_name: csr01
csr_admin_username: admin
csr_admin_password: Pass1234
csr_domain_name: cloudhotspot.co

# Set to 'True' or 'False'
csr_enable_scp: False

# Set to 'ax' or 'appx'
csr_license_level: appx
```

Dependencies
------------

This role relies on the Ansible Galaxy <a href="https://github.com/yaegashi/ansible-role-blockinfile" target="_blank">yaegashi.blockinfile module</a>.  Installing this role will automatically install this module.

Example Playbook
----------------

This playbook is designed to run locally on local OS X host so you should configure any play that uses this role with `hosts: localhost` and `connection: local`:

    - hosts: localhost
      connection: local
      roles:
         - { role: mixja.csr1000v, csr_vm_overwrite: true, csr_ova_source: /path/to/ova/source, csr_ova_root: /path/to/vm/root  }

A sample playbook is provided at <a href="https://github.com/cloudhotspot/ansible-csr1000v-playbook">https://github.com/cloudhotspot/ansible-csr1000v-playbook</a>

Please also take note the following issue - https://github.com/cloudhotspot/ansible-csr1000v-role/issues/2

License
-------

BSD

Author Information
------------------

Created by Justin Menga - see http://pseudo.co.de
