nGinx
=========

An ansible galaxy role for nGinx

Requirements
------------

Tested on ansible version 2.5.0 and only support CentOS6.

Role Variables
--------------

Variables to this role can be learned from `vars/main.yml`.

Available variables have `nginx_` as a prefix. For example, if you want to turn off tcp_nodelay, you can write `nginx_tcp_nodelay: "off"`.

When you have site configuration files, put those files into `ansible-role-for-nginx/files/`. This ansible-role-for-nignx will set up configuration files accordingly.

Example Playbook
----------------
```yaml
---
- hosts: all
  become: true
  roles:
    - role: ansible-role-for-nginx
      nginx_user: nginx
      nginx_worker_connection: 1024
      nginx_tcp_nodelay: "off"
      nginx_sites:
	- mysiteA
	- mysiteB
	- mysiteC
	...

```
```console
% cat /path-to/ansible-role-for-nginx/files/mysiteA
server {
    server_name localhost;
    listen 443;
    charset utf-8;
    ssl on;
    ssl_certificate /path/to/ssl/selfcert.pem;
    ssl_certificate_key /path/to/ssl/selfcert.key;

    location / {
	proxy_set_header Upgrade $http_upgrade;
	proxy_set_header Connection $http_connection;
	proxy_next_upstream error timeout invalid_header http_500;
	proxy_pass  http://localhost:9000;
    }
}
```

Install
----------------
```console
% ansible-galaxy install git+https://github.com/ablce9/ansible-role-for-nginx.git
```
