# ansible-windows-execution-environment
Ansible Execution environment image for Windows with Kerberos Authentication 


## Add Domain user to Remote Management Users 

- Add the remote user to the group "Remote Management Users" (`example.com/builtin`)
- 
If the user is a member of the Administrators group on the remote host then you shouldn't have to touch the SDDL of the WinRM listener at all. This is only necessary if the user is a limited user and needs access.


## Install PowerShell and PowerCLI inside EE

## Step 1: Download the packages
- [Download](https://learn.microsoft.com/en-us/powershell/scripting/install/install-rhel?view=powershell-7.3) and store PowerShell RPM.
- [Download](https://developer.vmware.com/web/tool/13.0.0/vmware-powercli) and store PowerCLI zip file.

Extract PowerCLI and package as `tar.gz` (This is to avoid the container build issue with zip file extraction).

```shell
# extract the zip file
unzip VMware-PowerCLI-13.0.0-20829139.zip

# package the extracted directory as tar.gz
tar -zcvf powercli.tar.gz VMware-PowerCLI-13.0.0-20829139
```

## Step 2:

Prepare the `execution-environment.yml` with `append` options as follows.

```yaml
.
.
additional_build_steps:
  prepend:
    .
    .    
  append:
    .
    .
    - COPY powershell-7.3.2-1.rh.x86_64.rpm /tmp/powershell-7.3.2-1.rh.x86_64.rpm    
    - RUN rpm -i /tmp/powershell-7.3.2-1.rh.x86_64.rpm

    - ADD powercli.tar.gz /opt/microsoft/powershell/7/Modules
    - WORKDIR /opt/microsoft/powershell/7/Modules
    - RUN mv VMware-PowerCLI-13.0.0-20829139/* .
```


## References

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

- [Investigating kinit Authentication Failures](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/linux_domain_identity_authentication_and_policy_guide/trouble-gen-kinit-auth)