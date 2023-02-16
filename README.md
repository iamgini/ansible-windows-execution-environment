# ansible-windows-execution-environment
Ansible Execution environment image for Windows with Kerberos Authentication 


- [How to create a new Execution Environment for Red Hat Ansible Automation Platform 2.x?](https://access.redhat.com/solutions/6654601)
- [User Authentication with Kerberos](https://docs.ansible.com/ansible-tower/latest/html/administration/kerberos_auth.html?extIdCarryOver=true&intcmp=701f20000012ngPAAQ&sc_cid=701f2000001OH7YAAW)


## Troubleshooting

```shell
# debug messages from the kinit process:
KRB5_TRACE=/dev/stdout kinit Administrator
```


[User Authentication with Kerberos](https://docs.ansible.com/ansible-tower/latest/html/administration/kerberos_auth.html)


```shell
[root@ip-172-31-26-180 ~]# kinit username
Password for username@WEBSITE.COM:
[root@ip-172-31-26-180 ~]#

Check if we got a valid ticket.

[root@ip-172-31-26-180 ~]# klist
Ticket cache: FILE:/tmp/krb5cc_0
Default principal: username@WEBSITE.COM

Valid starting     Expires            Service principal
01/25/16 11:42:56  01/25/16 21:42:53  krbtgt/WEBSITE.COM@WEBSITE.COM
  renew until 02/01/16 11:42:56
[root@ip-172-31-26-180 ~]#
```