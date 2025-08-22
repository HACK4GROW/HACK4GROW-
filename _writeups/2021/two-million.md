---
layout: writeup
category: Test-CTF
chall_description: https://i.imgur.com/TuImagen.png
points: 100
solves: 42
date: 2025-08-19
comments: true
tags: crypto, easy
---

# Two-millions-Hack The Box - Ejemplo

TwoMillion es un lanzamiento especial de HackTheBox para celebrar los 2.000.000 de miembros. Se lanzó directamente a los retirados, así que no se otorgan puntos ni sangre, solo para correr. Presenta un sitio web que se asemeja a la plataforma original de HackTheBox, incluyendo el desafío del código de invitación original que debía resolverse para registrarse. Una vez registrado, enumeraré la API para encontrar un endpoint que me permita convertirme en administrador y luego encontraré una inyección de comandos en otro endpoint de administrador.



## Reconocimiento
Iniciamos un escaneo con Nmap 
```bash
nmap -p- -sS --min-rate 5000 --open -vvv 10.10.11.221 -oG allPorts         
```

### Explicacion de parametros
*   -p-  pedimos que escanee todos los puertos desde el 1 al 65535
*   -sS  para enviar peticiones sin que complete el la conexion(three-way handshake”) solo envia un syn ack y lo corta con un RST asì podemos evitar dejar tantos logs y pasar mas desapercibidos
*   --min-rate 5000 para decirle que envie un minimo de 5000 paquetes por segundo
*   --open para que nos valla mostrando puertos abiertos sobre la marcha
*   -vvv triple verbose para que nos valla mostrando en tiempo real todo el progreso del escaneo y no esperar a que termine
*   -oG allPorts para que nos exporte el escaneo en un formato de nmap asì no debemos hacerlo de nuevo

### Resultado

Descubrimos que los puertos 22 y 80 estàn expuestos
22/tcp open  ssh     syn-ack ttl 63
80/tcp open  http    syn-ack ttl 63

### Exploramos que version es del servicio expuesto
```bash
nmap -sCV -p22,80 10.10.11.221 -vvv -oN targeted         
```
*  -sCV es una fusion de -sC -sV para detectar servicios y versiones.
*  -oN para que guarde el resultado del escaneo en un formato normal como si apareciera en la terminal

vemos que las versiones son las siguientes:
OpenSSH 8.9p1 Ubuntu 3ubuntu0.1
nginx

Vemos que la ip redirige a 2million.htb
lo sumamos al /etc/hosts para tener visibilidad

### Fuerza bruta para descubrir subdominios

```bash
ffuf -u http://10.10.11.221 -H "Host: FUZZ.2million.htb" \                                                      37s  system root@kali
-w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt \
-mc all -ac -t 50
```
No pudimos encontrar nada asì que vamos directo a la web
