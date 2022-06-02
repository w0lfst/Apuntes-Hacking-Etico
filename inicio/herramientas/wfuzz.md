# 🐙 Wfuzz

Ha sido creado para facilitar la tarea de evaluación de aplicaciones web y se basa en un concepto simple: reemplaza cualquier referencia a la palabra clave **FUZZ** por el valor de una carga útil determinada, por ejemplo, podriamos utilizar un diccionario de [SecLists](https://github.com/danielmiessler/SecLists).

Nos permite inyectar cualquier entrada en cualquier campo de una solicitud HTTP, lo que permite realizar complejos ataques de seguridad web en diferentes componentes de la aplicación web, como: parámetros, autenticación, formularios, directorios/archivos, encabezados, etc...

## Directorio

```bash
wfuzz -c -t 200 --hc=404 -w diccionario.txt http://tu-url/FUZZ
```

* `-c` > Ver resultado con un formato colorizado.
* `-t` > Especificar el número de conexiones simultáneas.
* `--hc` > Ocultar respuestas con codigo 404 (not found).
* `-w` > Ruta del diccionario. [Diccionario recomendado](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/directory-list-2.3-medium.txt)
* `FUZZ` > Indicamos donde queremos que realice el ataque.

## Subdominio

```bash
wfuzz -c -t 200 --hc=404 -w diccionario.txt -H "HOST: FUZZ.tu-url"
```

* `-c` > Ver resultado con un formato colorizado.
* `-t` > Especificar el número de conexiones simultáneas.
* `--hc` > Ocultar respuestas con codigo 404 (not found).
* `-w` > Ruta del diccionario. [Diccionario recomendado](https://github.com/danielmiessler/SecLists/blob/master/Discovery/DNS/subdomains-top1million-5000.txt)
* `-H` > Indicar cabeceras http.
* `FUZZ` > Indicamos donde queremos que realice el ataque.

[Manual de Wfuzz](https://manpages.debian.org/bullseye/wfuzz/wfuzz.1.en.html).

[Documentación oficial Wfuzz](https://manpages.debian.org/bullseye/wfuzz/wfuzz.1.en.html).
