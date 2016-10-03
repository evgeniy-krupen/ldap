# Ldap. Task2. Report.
Student: [Evgeniy_Krupen](https://upsa.epam.com/workload/employeeView.do?employeeId=4060741400038655484#emplTab=general)

***1. I set up audit logging.***

- add to slapd.conf:


moduleload auditlog.la

overlay auditlog

auditlog /var/log/ldap/audit.log



- I created folder /var/log/ldap with 755 permisions and ldap owner

- I created users with phpldapadmin

![](https://github.com/evgeniy-krupen/ldap/blob/task2/task2/screenshots/l2.png)
![](https://github.com/evgeniy-krupen/ldap/blob/task2/task2/screenshots/l3.png)

***2. Configure Password Policy***

- slapd.conf

![](https://github.com/evgeniy-krupen/ldap/blob/task2/task2/screenshots/l4.png)

[ldap-policy.ldif](https://github.com/evgeniy-krupen/ldap/blob/task2/task2/config_files/ldap-policy.ldif) to ldap DB
![](https://github.com/evgeniy-krupen/ldap/blob/task2/task2/screenshots/l5.png)

- audit log

![](https://github.com/evgeniy-krupen/ldap/blob/task2/task2/screenshots/l6.png)

***3. Zabbix with ldap authentication***

[ldap-zabbix-groupofname.ldif](https://github.com/evgeniy-krupen/ldap/blob/task2/task2/config_files/ldap-zabbix-groupofname.ldif) to ldap DB

[ldap-users.ldif](https://github.com/evgeniy-krupen/ldap/blob/task2/task2/config_files/ldap-users.ldif) to ldap DB

$ ldapadd -x -D "cn=admin,dc=mnt,dc=lab" -W -f /etc/openldap/ldap-zabbix-groupofname.ldif

$ ldapadd -x -D "cn=admin,dc=mnt,dc=lab" -W -f /etc/openldap/ldap-users.ldif

![](https://github.com/evgeniy-krupen/ldap/blob/task2/task2/screenshots/l7.png)

![](https://github.com/evgeniy-krupen/ldap/blob/task2/task2/screenshots/l8.png)

$ ldapsearch -h localhost -D "cn=admin,dc=mnt,dc=lab" -W -b "dc=mnt,dc=lab" 'memberOf=cn=zabbix,ou=Services,dc=mnt,dc=lab'

![](https://github.com/evgeniy-krupen/ldap/blob/task2/task2/screenshots/l9.png)

- I created account user1 in zabbix and changed authentication method

![](https://github.com/evgeniy-krupen/ldap/blob/task2/task2/screenshots/l10.png)

***4. Linux authentication with ldap client***

[ldap-linux-group.ldif](https://github.com/evgeniy-krupen/ldap/blob/task2/task2/config_files/ldap-linux-group.ldif)

![](https://github.com/evgeniy-krupen/ldap/blob/task2/task2/screenshots/l11.png)

- I set up ldap client and configureted it

![](https://github.com/evgeniy-krupen/ldap/blob/task2/task2/screenshots/l12.png)

add line below in /etc/nslcd.conf

filter passwd (&(objectClass=posixAccount)(memberOf=cn=Linux,ou=Groups,dc=mnt,dc=lab))

$ authconfig --enableldap --enableldapauth --ldapserver=ldap://192.168.25.115/ --ldapbasedn=dc=mnt,dc=lab --disablefingerprint --kickstart --enablemkhomedir

![](https://github.com/evgeniy-krupen/ldap/blob/task2/task2/screenshots/l13.png)

![](https://github.com/evgeniy-krupen/ldap/blob/task2/task2/screenshots/l16.png)

![](https://github.com/evgeniy-krupen/ldap/blob/task2/task2/screenshots/l15.png)

Thank you for your time

