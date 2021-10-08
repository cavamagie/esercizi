---
- name: Configure VPN phase2.
  fortios_vpn_ipsec_phase2:
    vdom:  "{{ vdom }}"
    state: "present"
    #access_token: "<your_own_value>"
    vpn_ipsec_phase2:
      name: "NET-{{ item }}"
      add_route: "enable"
      auto_negotiate: "disable"
      comments: "{{ item }}"
      dhgrp: "19"
      dst_addr_type: "subnet"
      dst_port: "0"
      dst_subnet: "{{ item }}"       
      phase1name: "{{ name_phase1 }}"
      keepalive: "disable"
      keylife_type: "seconds"
      keylifeseconds: "3600"
      pfs: "enable"
      #proposal: "aes256-sha256"
      proposal: "null-sha512"
      protocol: "0"
      replay: "enable"
      src_addr_type: "subnet"
      src_port: "0"
      src_subnet: "{{ src_subnet_phase2 }}"
  loop:
   "{{ remote_address_phase2 }}"
