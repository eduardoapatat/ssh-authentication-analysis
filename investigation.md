# Investigation

## Caso 1 - Intentos fallidos de autenticación SSH

### Resumen

Se detectó una conexión SSH hacia el usuario `labuser` desde `127.0.0.1`.

Luego se utilizó el siguiente comando:

`sudo journalctl -u ssh --since "15 minutes ago"`

Se aprendió que `journalctl` permite consultar los registros almacenados por `systemd`.

En este comando:

- `sudo` ejecuta el comando con privilegios elevados, lo que permite acceder a todos los registros necesarios.
- `journalctl` muestra los eventos registrados en el journal del sistema.
- `-u ssh` filtra los registros para mostrar únicamente los relacionados con la unidad o servicio SSH.
- `--since "15 minutes ago"` limita los resultados a los eventos ocurridos durante los últimos 15 minutos.

Este filtro permite reducir la cantidad de información mostrada y centrarse solamente en los eventos SSH generados durante el laboratorio.

### Datos principales

- Usuario: labuser
- IP origen: 127.0.0.1
- Puerto origen: 52812
- Servicio: SSH
- Resultado: autenticación fallida
- Intentos fallidos: 3

### Evidencia

Se observaron tres eventos:

`Failed password for labuser from 127.0.0.1 port 52812 ssh2`

Después apareció:

`Connection closed by authenticating user labuser ... [preauth]`

### Interpretación

Los tres intentos pertenecen a la misma conexión SSH porque comparten:

- la misma IP de origen
- el mismo puerto de origen
- el mismo usuario
- el mismo proceso `sshd-session` [41325]

La conexión se cerró antes de que el usuario lograra autenticarse.

### Conclusión

No hubo acceso exitoso durante esta conexión. 

La evidencia puede revisarse en [failed-login.txt](evidence/failed-login.txt) y la captura en [case-1.png](screenshots/case-1.png).