# ansible-my-playbook-collection

create option use
- name: Output new root password
      debug: msg="New root password is {{mysql_new_root_pass.stdout}}"

    - name: Create my.cnf
      template: src=templates/mysql/my.cnf dest=/root/.my.cnf

It’s important to note that each time the playbook runs, a new root password will be generated for the server.While it’s not a bad thing to rotate root passwords frequently,this may not be the behavior that you are seeking. To disable this behavior, you can tell Ansible not to run certain commands if a specific file exists. Ansible has a special creates option that determines if a file exists before executing a module:

    - name: Generate new root password
      command: openssl rand -hex 7 creates=/root/.my.cnf
      register: mysql_new_root_pass

# If /root/.my.cnf doesn't exist and the command is run

    - debug: msg="New root password is {{ mysql_new_root_pass.stdout }}"
      when: mysql_new_root_pass.changed

# If /root/.my.cnf exists and the command is not run
    - debug: msg="No change to root password"
      when: not mysql_new_root_pass.changed
