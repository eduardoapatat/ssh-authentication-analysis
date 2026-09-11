# SSH Authentication Analysis

## Objetivo

El objetivo de este mini lab es analizar los eventos de autenticación SSH en un entorno Linux controlado (kali-linux).

Este laboratorio consiste en generar intentos de inicio de sesión fallidos y exitosos mediante SSH, para posteriormente investigar los eventos registrados por el sistema.

## Entorno

- Kali Linux
- OpenSSH Server 
- Cliente SSH
- journalctl
- Usuario de laboratorio: labuser
- Origen de las conexiones: localhost (127.0.0.1)

## Objetivo ha investigar

- Intentos de autenticación fallidos mediante logs
- Inicios de sesión exitosos
- Usuario utilizado
- IP de origen
- Puerto de origen
- Cantidad de intentos
- Cómo identificar una misma conexión SSH