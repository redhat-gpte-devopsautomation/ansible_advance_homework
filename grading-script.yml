- hosts: localhost
  gather_facts: false
  tasks:
  - name: Create In-memory Inventory
    add_host: 
      name: workstation-{{OSP_GUID}}.rhpds.opentlc.com
      group: workstation
 
- hosts: workstation
  gather_facts: false
  tasks:
  - name: OpenStack servers
    os_server_facts:
      cloud: ospcloud
      server: 'frontend'
    register: openstack_info

  - name: Curl website
    uri:
       url: "http://{{item.public_v4}}"
    with_items:
       - "{{ openstack_info.ansible_facts.openstack_servers }}"
    register: webpage

  
- hosts: localhost
  gather_facts: false
  tasks:
  - name: Curl website
    uri:
     url: "http://frontend1.{{ THREE_TIER_GUID }}.example.opentlc.com"
     return_content: yes
    register: webpage
  - name: Fail if 'Ansible has done its job' is not in the page content
    fail:
    when: "'Ansible has done its job' not in webpage.results[0].content"

