#Registry-Nginx-LDAP

This will create a docker registry with a Nginx proxy.  Authentication is done via Nginx and LDAP to Active Directory or another LDAP server.

##Getting started
- Place the SSL key and SSL crt in the nginx/certs/ directory.
- Edit the nginx/config/nginx.conf file.
```
#!bash
ldap_server LDAP1 {
        url "ldaps://<LDAP SERVER>/DC=your,DC=domain,DC=com?sAMAccountName?sub?(objectClass=person)";
        binddn "<LDAP BIND USER NAME";
        binddn_passwd "<LDAP BIND USER PASSWORD>";
        bind_timeout 5s;
        request_timeout 5s;
        group_attribute member;
        group_attribute_is_dn on;
        require valid_user;
        satisfty all;
}
```
- Edit the server names in the HTTP and HTTPS sections.

```
#!bash
server {
        listen          80;
        server_name     <YOUR SERVER NAME>;
        return          301 https://$server_name$request_uri;
}
```
```
#!bash
server {
        listen          80;
        server_name     <YOUR SERVER NAME>;
...
}
```
- Build the docker container
```
#!bash
#from the registry-nginx-ldap directory
$ cd nginx && docker build --tag=ldap_nginx .
```
- Start the containers
```
#!bash
#from the registry-nginx-ldap directory
$ docker-compose up -d
```