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


