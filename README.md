# Ldap. Task3. Report.
Student: [Evgeniy_Krupen](https://upsa.epam.com/workload/employeeView.do?employeeId=4060741400038655484#emplTab=general)

***1.For Configure CentOS to authenticate users via LDAP by ssh keys***

**$ yum install openssh-ldap nss-pam-ldapd**

**$ vi /etc/openldap/slapd.conf**

***on server:***

**$ ssh-keygen**

add rsa pub in user by phpldapadmin

**$ yum install openssh-ldap nss-pam-ldapd**

**$ vi /etc/ssh/sshd_config**

 # RSAAuthentication yes

PubkeyAuthentication yes

 #AuthorizedKeysFile     .ssh/authorized_keys

AuthorizedKeysCommand /usr/libexec/openssh/ssh-ldap-wrapper

AuthorizedKeysCommandRunAs nobody


**$ vi /usr/libexec/openssh/ssh-ldap-wrapper**

 #!/bin/sh
exec /usr/libexec/openssh/ssh-ldap-helper -d -e -v -s "$1"

check 755 on /usr/libexec/openssh/ssh-ldap-wrapper

**$ sh /usr/libexec/openssh/ssh-ldap-wrapper userinldap2**

![](https://github.com/evgeniy-krupen/ldap/blob/task3/task3/screenshots/l1.png)

check it on client

![](https://github.com/evgeniy-krupen/ldap/blob/task3/task3/screenshots/l2-on_client.png)

connection without password (only ssh key), screenshot below

![](https://github.com/evgeniy-krupen/ldap/blob/task3/task3/screenshots/l3-from_server.png)

***2. Groups wheel and sudoers***

![](https://github.com/evgeniy-krupen/ldap/blob/task3/task3/screenshots/l4-wheel-group.png)

![](https://github.com/evgeniy-krupen/ldap/blob/task3/task3/screenshots/l4-wheel_with_member.png)

on client:

$ echo '%wheel  ALL=(ALL)       ALL' > /etc/sudoers.d/wheel
$ chmod 0440 /etc/sudoers.d/wheel

$ vi /etc/openldap/slapd.conf

include /usr/share/doc/sudo-1.8.6p3/schema.OpenLDAP


$ slaptest -f /etc/openldap/slapd.conf

![](https://github.com/evgeniy-krupen/ldap/blob/task3/task3/screenshots/l5-before_add_wheel_in_sudoersd.png)

$ service slapd restart

![](https://github.com/evgeniy-krupen/ldap/blob/task3/task3/screenshots/l5-after_add_wheel_in_sudoersd.png)

$ ldapsearch -D "cn=admin,dc=mnt,dc=lab" -W -b "dc=mnt,dc=lab" -s sub "ou=sudoers"

![](https://github.com/evgeniy-krupen/ldap/blob/task3/task3/screenshots/l6-create_sudoers.png)

![](https://github.com/evgeniy-krupen/ldap/blob/task3/task3/screenshots/l7-create_sudoers_role.png)

$ ldapsearch -D "cn=admin,dc=mnt,dc=lab" -W -b "dc=mnt,dc=lab" -s sub "objectClass=sudoRole"

![](https://github.com/evgeniy-krupen/ldap/blob/task3/task3/screenshots/l7-check_sudoers_role.png)

on client:

$ vi /etc/sudo-ldap.conf

binddn cn=admin,dc=mnt,dc=lab

bindpw Epam_2011

uri ldap://192.168.25.115

sudoers_base ou=sudoers,dc=mnt,dc=lab


$ chmod 0400 /etc/sudo-ldap.conf

$ vi /etc/nsswitch.conf

sudoers:    files ldap

![](https://github.com/evgeniy-krupen/ldap/blob/task3/task3/screenshots/l8-sudoers.png)



