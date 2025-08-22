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
###Explicacion de parametros
*   -p-  pedimos que escanee todos los puertos desde el 1 al 65535
*   -sS  para enviar peticiones sin que complete el la conexion(three-way handshake”) solo envia un syn ack y lo corta con un RST asì podemos evitar dejar tantos logs y pasar mas desapercibidos
