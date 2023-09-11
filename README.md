A place to list all my SELinux debugging commands and handy tips. Help me SELinux, you're my only hope.

## Building

### Local Interfaces:
- Copy custom policy interface to locatation so it is available to build against with `audit2allow -R`:
`install -Dp -m 0664 -o root -g root myapp.if /usr/share/selinux/devel/include/myapplications/myapp.if`

### Compiling:
`make -f /usr/share/selinux/devel/Makefile myapp.pp`

### Installing a policy:
`semodule -i myapp.pp` 

### Restoring contexts (recursively switch):
`restorecon -RvF`




## Permission Handling:

### Search SELinux if a source domain can read a file of the target type:
`sesearch -A -s myapp_t -t etc_t -c file -p read`

### Show attributes assigned to type:
`seinfo -xt myapp_var_log_t`

### Semanage port handling:
`semanage port -a -t myapp_port_t -p tcp 3000`

### Show process domain:
`ps -efZ | grep myapp`

### Show what context a file should have in a location:
`matchpathcon /path/to/something`



## Audit Log:

### Searching the audit log: 
`ausearch -m avc,user_avc,selinux_err -ts recent | less`
- Searching the audit log with audit2allow and match against interfaces with switch -R:
`ausearch -m avc,user_avc,selinux_err -ts recent | auditallow -R`

### Enabling full PATH audit logs:
- Will remain until reboot:
`auditctl -w /etc/shadow -p w`

- Permanently (CentOS 9 Stream):
`echo "-w /etc/shadow -p w" >> /etc/audit/rules.d/audit.rules`
`service auditd restart`

### Using audit to monitor actions:
- List all current rules in use: 
`auditctl -l`
- Add a directory to monitor for write permissions:
`auditctl -w /opt/mydir -p w`
- Remove a rule (capital switch):
`auditctl -W <exact rule>`



## File Locations:
- SELinux stored file contexts:
`/etc/selinux/targeted/contexts/files/file_contexts`



## Commonly Used Tools and Packages:
```
setools-console
policycoreutils
```
