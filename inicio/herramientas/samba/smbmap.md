# 🗺 SMBMap

Esta herramienta nos permite enumerar las unidades compartidas de **SAMBA** en todo un dominio.

* Unidades compartidas.
* Permisos de las unidades.
* Contenidos compartidos.
* Funcionalidad de carga/descarga.
* Coincidencia de patrones de descarga automatica de nombres de archivo.
* Comandos remotos.

***

### Listar archivos SMB:

```bash
smbmap -H "IP-attack"
```

* `-H` Indicamos la opcion de host.

***

### Listar archivos con usuario null:

```bash
smbmap -H "IP-attack" -u 'null'
```

* `-H` Indicamos la opcion de host.
* `-u` Indicamos el usuario. En este caso, es null ya que no tenemos ninguno.
