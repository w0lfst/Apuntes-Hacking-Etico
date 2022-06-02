# Directorio web

Utilizaremos el script **http-enum** para analizar los directorios disponibles de una pagina web con un diccionario de rutas. Gracias a esto podriamos encontrar una via potencial de ataque.

```bash
nmap --script http-enum -p "ports" "IP-Attack" -oN "nombre-archivo" (webScan)
```

* `--script` > Seleccionamos los scripts disponibles de nmap. Podemos verlos en la ruta /usr/share/nmap/scripts.
* `-p` > En este caso al enumerar un servicio web http ponemos el puerto 80.
* `-oN` > Formato de archivo Nmap.
