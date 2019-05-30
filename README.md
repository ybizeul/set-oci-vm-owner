Role Name
=========

Copies `owner` custom attribute in vCenter to `owner` tag in NetApp OnCommand Insight

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

`vcenter_hostname` or `VMWARE_HOST` environment : Host name or IP of vCenter server

`vcenter_username` or `VMWARE_USER` environment : Username to use when connecting to vCenter Server. Can be a read-only user

`vcenter_password` or `VMWARE_PASSWORD` environment : Password to use when connecting to vCenter Server

`vcenter_datacenter` : Name of Datacenter as defined in vCenter Server top level object.

`oci_host`  : Host name or IP address of OCI server

`oci_username` : Username to use when connecting to OCI server

`oci_password` : Password to use when connecting to OCI server. To get a lit of annotation, you can use the following URL in your web browser : https://ociserver/rest/v1/assets/annotations

`oci_annotation_id` : ID for the annotation to use when updating VirtualMachine object in OCI.

`oci_query_id` :ID for the query that returns all the VirtualMachine objects that needs to be updated with owner annotation. To get a list of queries, you can use tge following URL in your web browser : https://ociserver/rest/v1/queries


Example Playbook
----------------

Here is an example that shows how to pass OCI credentials.

vCenter credentials usually comes from the inventory file.



    # Content of inventory.yaml
    all:
      children:
        vcenters:
          hosts:
            vcenter:
              ansible_connection: local
              vcenter_datacenter: Datacenter
              vcenter_hostname: myVcenterServer
              vcenter_username: ansible@vsphere.local
              vcenter_password: secretPassword

    # Content of set-oci-vm-owner.yaml
    - hosts: vcenters
      vars:
        oci_username: admin
        oci_password: secretPassword
        oci_host: ociServer
        oci_query_id: 67
        oci_annotation_id: 5012
      module_defaults:
        vmware_vm_facts:
          validate_certs: no
        vmware_guest_facts:
          validate_certs: no
        uri:
          validate_certs: no
      roles:
        - lab-set-oci-vm-owner

    $ ansible-playbook -i inventory.yaml set-oci-vm-owner.yaml 



Author Information
------------------

yann@tynsoe.org