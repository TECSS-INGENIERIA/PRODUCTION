Comandos MQTT para equipo HOTOIL
Este documento describe el tópico MQTT utilizado para enviar comandos hacia un equipo HOTOIL.
Formato del tópico
En firmware, el tópico se genera con:
```c
sprintf(_topic, "wellservice/%s/HOTOIL/%s/down/CMD", WRITER_Company, Writer_MacAddress);
```
La estructura resultante es:
```text
wellservice/<COMPANY>/HOTOIL/<MAC_ADDRESS>/down/CMD
```
Donde:
Campo	Descripción	Ejemplo
`COMPANY`	Identificador de la compañía o cliente	`TECSS`
`HOTOIL`	Tipo de equipo o servicio	`HOTOIL`
`MAC_ADDRESS`	Identificador único del equipo, normalmente la MAC sin separadores	`A1B2C3D4E5F6`
`down`	Indica comunicación descendente, desde servidor hacia equipo	`down`
`CMD`	Indica que el mensaje corresponde a un comando	`CMD`
Ejemplo documentado
Valores de ejemplo:
```c
WRITER_Company = "TECSS";
Writer_MacAddress = "A1B2C3D4E5F6";
```
Tópico generado:
```text
wellservice/TECSS/HOTOIL/A1B2C3D4E5F6/down/CMD
```
---
Comandos disponibles
Por ahora se documentan los siguientes comandos.
1. Reset del equipo
Este comando solicita al equipo realizar un reinicio.
Tópico
```text
wellservice/TECSS/HOTOIL/A1B2C3D4E5F6/down/CMD
```
Payload JSON
```json
{
  "reset": true
}
```
Descripción
Cuando el equipo recibe este mensaje, debe interpretar que se solicita un reset del sistema.
---
2. Actualización de firmware
Este comando solicita al equipo iniciar el proceso de actualización de firmware.
Tópico
```text
wellservice/TECSS/HOTOIL/A1B2C3D4E5F6/down/CMD
```
Payload JSON
```json
{
  "update": true
}
```
Descripción
Cuando el equipo recibe este mensaje, debe interpretar que se solicita una actualización de firmware.
---
Ejemplos de publicación MQTT
Reset
```bash
mosquitto_pub \
  -h <BROKER_MQTT> \
  -p 1883 \
  -t "wellservice/TECSS/HOTOIL/A1B2C3D4E5F6/down/CMD" \
  -m '{"reset": true}'
```
Actualización de firmware
```bash
mosquitto_pub \
  -h <BROKER_MQTT> \
  -p 1883 \
  -t "wellservice/TECSS/HOTOIL/A1B2C3D4E5F6/down/CMD" \
  -m '{"update": true}'
```
---
Notas importantes
El tópico debe respetar exactamente la estructura definida en el firmware.
`WRITER_Company` identifica la compañía o cliente.
`Writer_MacAddress` identifica el equipo destino.
El payload debe ser un JSON válido.
Los valores booleanos en JSON deben escribirse en minúscula: `true` o `false`.
Por ahora los comandos disponibles son:
`reset`
`update`
Se recomienda usar la MAC sin separadores para evitar diferencias de formato entre equipos, servidores y clientes MQTT.
---
Resumen rápido
Acción	Tópico	Payload
Reset	`wellservice/TECSS/HOTOIL/A1B2C3D4E5F6/down/CMD`	`{"reset": true}`
Actualización de firmware	`wellservice/TECSS/HOTOIL/A1B2C3D4E5F6/down/CMD`	`{"update": true}`
