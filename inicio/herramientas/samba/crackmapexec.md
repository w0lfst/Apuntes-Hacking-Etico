# 🎆 CrackMapExec o CME

Esta herramienta nos ayuda a automatizar la evaluación de la seguridad de las grandes redes de Active Directory. En este caso, la utilizaremos para analizar el servicio de **SAMBA**.

***

* Analizar servidor SMB:

```bash
crackmapexec smb "IP-attack"
```

* Listar recursos compatirdos:

```bash
crackmapexec smb "IP-attack" --shares 2>/dev/null
```

> _**NOTA:**_ `2>/dev/null` se utiliza para quitar los errores que muestre el escaneo. Proximamente en [Linux](https://github.com/w0lfst/Apuntes/tree/main/Linux) se explicara detalladamente.
