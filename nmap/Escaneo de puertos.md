# Escanear puertos.
```nmap -p- --open -T5 -v -n "IP-attack" -oG "nombre-archivo" (allPorts)```
- `-p-` > Indicamos todos los puertos.
- `--open` > Filtrar por puertos abiertos.
- `-T5` > Velocidad de escaneo (maxima).
- `-v` > Mostrar en la terminal todo el proceso. Tambien se puede usar `-vvv` para informacion mas detallada (verbose). 
- `-n` > Desactivar la resolucion DNS para mayor rapidez.
- `-oG` > Exportar formato grepeable para despues utilizar ExtractPorts.

### Escaneo mas rapido.
```nmap -p- -sS --min-rate 5000 --open -vvv -n -Pn "IP-attack" -oG "nombre-archivo" (allPorts)```
- `-p-` > Indicamos todos los puertos.
- `--sS` > TCP sync port scan.
- `--min-rate` > No emitir paquetes mas lentos de "paquetes por segundo" (5000).
- `--open` > Filtrar por puertos abiertos. 
- `-v` > Mostrar en la terminal todo el proceso. Tambien se puede usar `-vvv` para informacion mas detallada (verbose). 
- `-n` > Desactivar la resolucion DNS para mayor rapidez.
- `-Pn` > No aplicar Host discovery (Descubrimiento de host). 
- `-oG` > Exportar formato grepeable para despues utilizar ExtractPorts.

### Escanear version y servicio que corren los puertos.

```nmap -sC -sV -p "ports" "IP-attack" -oN "nombre-archivo" (targeted)```
	
- `-sC` > Seleccion de script pedeterminado de nmap para escaneo. 
- `-sV` > Sondear puertos abiertos, para obtener información de servicio/versión.
- `-p` > Seleccionar puertos escaneados previamente.
- `-oN` > Exportar formato Nmap. 