# RHEL: Problema con autenticación
## Referencias
[Red Hat Customer Portal](https://access.redhat.com/solutions/3380341)

## Problema
Error al autenticarse que genera el siguiente log en `/var/log/secure`:
```bash
[sssd[ldap_child[59627]]]: Failed to initialize credentials using keytab [MEMORY:/etc/krb5.keytab]: Preauthentication failed. Unable to create GSSAPI-encrypted LDAP connection.
```

## Solución
### RHEL 6
```bash
sudo -i
service sssd stop && \
rm -frv /var/lib/sss/{​​​​db,mc}​​​​/* && \
rm -fr /etc/krb5.keytab && \
puppet agent --test --verbose
```
> No es necesario volver a iniciar SSSD, se encarga puppet.

### RHEL 8
```bash
sudo -i
systemctl stop sssd && \
rm -frv /var/lib/sss/{​​​​db,mc}​​​​/* && \
rm -fr /etc/krb5.keytab && \
puppet agent --test --verbose
```

Log:
```bash
Stopping sssd:                                             [  OK  ]
Info: Using configured environment 'KT_Inditex_Produccion_itx_generic_puppet_48'
Info: Retrieving pluginfacts
Info: Retrieving plugin
Info: Retrieving locales
Info: Loading facts
Info: Caching catalog for axinsgaezeudb22.central.inditex.grp
Info: Applying configuration version '1603103605'
Notice: /Stage[main]/Rhelbase::Ad/Exec[JOIN AD]/returns: executed successfully (corrective)
Notice: /Stage[main]/Rhelbase::Sssd/Service[sssd]/ensure: ensure changed 'stopped' to 'running' (corrective)
Info: /Stage[main]/Rhelbase::Sssd/Service[sssd]: Unscheduling refresh on Service[sssd]
Notice: Applied catalog in 30.32 seconds
```
