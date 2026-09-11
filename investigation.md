# Investigation

## Metodología

Para revisar los eventos generados por el servicio SSH se utilizó el siguiente comando:

`sudo journalctl -u ssh --since "15 minutes ago"`

Se aprendió que `journalctl` permite consultar los registros almacenados por `systemd`.

En este comando:

- `sudo` ejecuta el comando con privilegios elevados, lo que permite acceder a todos los registros necesarios.
- `journalctl` muestra los eventos registrados en el journal del sistema.
- `-u ssh` filtra los registros para mostrar únicamente los relacionados con la unidad o servicio SSH.
- `--since "15 minutes ago"` limita los resultados a los eventos ocurridos durante los últimos 15 minutos.

Este filtro permite reducir la cantidad de información mostrada y centrarse solamente en los eventos SSH generados durante el laboratorio.

## Caso 1 - Intentos fallidos de autenticación SSH

### Resumen

Se detectó una conexión SSH hacia el usuario `labuser` desde `127.0.0.1`.

### Datos principales

- Usuario: `labuser`
- IP origen: `127.0.0.1`
- Puerto origen: `52812`
- Servicio: SSH
- Resultado: autenticación fallida
- Intentos fallidos: 3

### Evidencia

Se observaron tres eventos:

`Failed password for labuser from 127.0.0.1 port 52812 ssh2`

Después apareció:

`Connection closed by authenticating user labuser ... [preauth]`

La evidencia completa puede revisarse en:

- [failed-login.txt](evidence/failed-login.txt)
- [case-1.png](screenshots/case-1.png)

### Interpretación

Los tres intentos pertenecen a la misma conexión SSH porque comparten:

- la misma IP de origen
- el mismo puerto de origen
- el mismo usuario
- el mismo proceso `sshd-session[41325]`

La conexión se cerró antes de que el usuario lograra autenticarse.

### Conclusión

No hubo acceso exitoso durante esta conexión.

---

## Caso 2 - Autenticación SSH exitosa

### Resumen

Se observó una conexión SSH hacia el usuario `labuser` desde `127.0.0.1` en la que se proporcionaron credenciales válidas.

### Datos principales

- Usuario: `labuser`
- IP origen: `127.0.0.1`
- Puerto origen: `50388`
- Servicio: SSH
- Resultado: autenticación exitosa

### Evidencia

Se observó el evento:

`Accepted password for labuser from 127.0.0.1 port 50388 ssh2`

Posteriormente se registró:

`session opened for user labuser`

Esto indica que las credenciales fueron aceptadas y se abrió una sesión SSH para el usuario.

La evidencia completa puede revisarse en:

- [successful-login.txt](evidence/successful-login.txt)
- [case-2.png](screenshots/case-2.png)

### Conclusión

A diferencia del caso anterior, en esta conexión la autenticación fue exitosa y se abrió una sesión para `labuser`.

---

> [!NOTE]
> Durante el laboratorio hubo un corte de luz y el servicio `SSH` tuvo que iniciarse nuevamente con:
>
> `sudo systemctl start ssh`