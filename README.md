# Ansible-network_acls

Ansible role that configures access lists on network devices.  

The configuration of the role is done so that it shouldn't be necessary to modify the role for any configuration.
All settings can be configured by changing role parameters or by declaring new config as variable.

Please report any issues or send PR.

nxos specific: state=absent removes the ACE if it exists. state=delete_acl deletes the ACL if it exists. 

## Examples

```yaml
---

- name: Example of how to configure an acl 'monitoring' allowing 192.0.2.0/24 to 198.51.100.0/24 for http/https. It starts with a remark and it set to log. 
  hosts: sw1
  vars:
    access_lists:
      monitoring:
        10: 
          state: present
          action: remark
          remark: This is the first seq that describes the access   
        20: 
         state: present
         action: permit
         source: 192.0.2.0/24
         destination: 198.51.100.0/24
         dest_port1: 80      
         dest_port_op: eq
         log : enable
         proto: tcp
        30: 
         state: present
         action: permit
         source: 192.0.2.0/24
         destination: 198.51.100.0/24
         dest_port1: 443      
         dest_port_op: eq
         log : enable
         proto: tcp
  roles:
    - Ansible-network_acls

- name: Example of how to configure an acl using a range. Source/Destination port operands such as eq, neq, gt, lt, range can be used.  
  hosts: sw1
  vars:
    access_lists:
      app_access:  
        10: 
         state: present
         action: permit
         source: 192.0.2.0/24
         src_port_op: range 
         src_port1: 5000                    # source port, also first (lower) port when using range
         src_port2: 60000                    # source port, also second (end) port when using range
         destination: 198.51.100.0/24
         dest_port_op: range
         dest_port1: 2000      
         dest_port2: 2010     
         log : enable
         proto: udp
  roles:
    - Ansible-network_acls



```

## Role variables

```yaml
---
# Define the acls to be configured (see README for examples)
access_lists: { }
```


## License

Apache


## Author

Dan Murarasu