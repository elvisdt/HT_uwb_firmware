# HT UWB — firmware

Binarios publicados del firmware del dispositivo HT UWB (ESP32 en WeAct
CAN485, enlace BLE ↔ RS485 con el equipo UWB). La app **HT UWB LINK** busca
acá las actualizaciones (*Dispositivo → Actualizar firmware*).

Este repo no tiene código: cada versión es un **release** con dos archivos.

| Archivo | Qué es |
|---|---|
| `uwb_rs485.bin` | imagen de la app ESP32 que se instala por Bluetooth |
| `firmware.json` | versión, build, SHA-256 y tamaño del `.bin` |

La app lee siempre el último release:

```
https://github.com/elvisdt/HT_uwb_firmware/releases/latest/download/firmware.json
```

y solo ofrece la actualización si es más nueva que la del equipo. Antes de
instalar verifica el SHA-256 y que la imagen sea de este proyecto.

## Publicar una versión

En el repo del código (privado):

```bash
cd HT_uwb_rs485
# subir PROJECT_VER y FW_BUILD en CMakeLists.txt
idf.py build
tools/fw_manifest.py          # genera build/firmware.json y archiva la versión en dist/
```

En este repo: **Releases → Draft a new release**

- Tag: `v<versión>` (por ejemplo `v0.5.3`), creado al publicar
- Título: `Firmware <versión> (build <N>)`
- Adjuntar `build/uwb_rs485.bin` y `build/firmware.json` con esos nombres exactos
- Release label: **None** (un pre-release no cuenta como último)

Los `.bin` no se guardan en el historial de git: van solo en los releases.

El `.elf` (símbolos, sirve para leer un core dump) **no** va acá: se sube al
release del repo privado del código.
