# i have just created my ansible project script
---
- name: installation LEMP on amazon linux
  hosts: localhost
  become: yes
  vars:
   web_pkg: nginx
   db_pkg: mariadb105-server
   php_pkg:
    - php
    - php-fpm
   web_service: nginx
   db_service: mariadb105-server
   php_service: php-fpm
   php_file_path: /usr/share/nginx/html/index.php
  tasks:
    - name: install webserver
      ansible.builtin.apt:
        name: "{{web_pkg}}"
        state: present
    - name: install mariadb
      ansible.builtin.apt:
        name: "{{db_pkg}}"
        state: present
    - name: install php-fpm
      ansible.builtin.apt:
        name: "{{php_pkg:}}"
        state: present
# start services
    - name: start webserver
      ansible.builtin.systemd:
        name: "{{web_services}}"
        state: started
        enabled: true
    - name: start mariadb
      ansible.builtin.systemd:
        name: "{{db_services}}"
        state: started
        enabled: true
    - name: start php-fpm
      ansible.builtin.systemd:
        name: "{{php_service}}"
        state: started
        enabled: true
# deploy php file
    - name: deploy php
      ansible.builtin.copy:
        dest: "{{php_file_path}}"
        content: 
          <?php
          phpinfo();
          ?>
