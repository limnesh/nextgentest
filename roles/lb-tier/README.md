##LB Tier

Install and configure haproxy

##Requirements

None

##Role Variables

payload: haproxy

##Dependencies

None

##Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - name: setup load-balancer tier
      hosts: frontends
      gather_facts: false
      become: yes
      roles:
        - {name: lb-tier, tags: [lbs, haproxy]}
