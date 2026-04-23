# LineageOS 20 (Android 13) en Samsung Galaxy Tab A 10.1 2019 (SM-T515)

## ✅ Estado
Funciona correctamente con audio, WiFi y Bluetooth.

---

## 📋 Requisitos previos
- Bootloader desbloqueado
- PC con Ubuntu/Linux
- Cable USB
- Pendrive + adaptador USB OTG (obligatorio para el repartitioner)
- `adb` y `heimdall` instalados (`sudo apt install android-tools-adb heimdall-flash`)

---

## 📥 Archivos necesarios

| Archivo | Fuente |
|--------|--------|
| `twrp-3.6.0_11-2` para SM-T515 | XDA Forums |
| `vendor_t515.zip` (Lifebreathing project) | [XDA - BaLazS28235](https://xdaforums.com/t/developement-64bit-support-sm-t510-sm-t515-lifebreathing-project-for-sm-t510-5) → GDrive SM-T515 (Android 15/16) |
| `lineage-20.0-UNOFFICIAL-arm64_bgN.img.xz` | [Andy Yan's GSI - SourceForge](https://sourceforge.net/projects/andyyan-gsi/files/lineage-20-td/) |
| `multidisabler-samsung-3.1.zip` | [Eureka Releases - SourceForge](https://sourceforge.net/projects/eurekaroms/files/web_files/multidisabler-samsung-3.1.zip) |

---

## 🔧 Contenido de vendor_t515.zip

```
7884and7904universal_repartitioner_script.zip
vendor.img
boot.img
README.md
```

---

## 💻 Envío de archivos desde Ubuntu via ADB

### Instalar herramientas necesarias
```bash
sudo apt install android-tools-adb heimdall-flash lz4
```

### Verificar que la tablet es detectada en TWRP
```bash
adb devices
# Debe mostrar algo como: R52N30F23JR    recovery
```

### Enviar archivos al almacenamiento interno
```bash
adb push archivo.zip /sdcard/
adb push archivo.img /sdcard/
```

### Solución si /sdcard/ da "Permission denied"
Hacer Format Data en TWRP primero:
- Wipe → Format Data → escribe `yes`
- Luego reconectar y volver a intentar el adb push

### Solución si /sdcard/ da "No space left on device"
Usar el pendrive OTG en lugar del almacenamiento interno (ver sección de pendrive más abajo)

### Descomprimir archivos .xz en Ubuntu
```bash
xz -d archivo.img.xz
# Genera archivo.img listo para flashear
```

---

## 📌 Proceso de instalación

### 1. Instalar TWRP
Desde Download Mode usando heimdall:
```bash
sudo heimdall flash --RECOVERY twrp.img
```

### 2. Preparar archivos en el pendrive
Copia estos archivos al pendrive desde Ubuntu:
```bash
cp 7884and7904universal_repartitioner_script.zip /media/usuario/PENDRIVE/
cp vendor.img /media/usuario/PENDRIVE/
cp boot.img /media/usuario/PENDRIVE/
cp lineage-20-arm64-gapps.img /media/usuario/PENDRIVE/
sync
umount /media/usuario/PENDRIVE/
```

### 3. Descomprimir la ROM
```bash
xz -d lineage-20.0-UNOFFICIAL-arm64_bgN.img.xz
```

### 4. Flashear desde TWRP (con pendrive OTG conectado)

**Paso 1 - Repartitioner** (⚠️ borra todo):
- Install → `7884and7904universal_repartitioner_script.zip`

**Paso 2 - Wipe:**
- Wipe → Format Data → escribe `yes`
- Wipe → Advanced Wipe → System, Vendor, Data, Cache, Product

**Paso 3 - Flashear vendor:**
- Install → Install Image → `vendor.img` → partición **Vendor**

**Paso 4 - Flashear boot:**
- Install → Install Image → `boot.img` → partición **Boot**

**Paso 5 - Flashear ROM:**
- Install → Install Image → `lineage-20-arm64-gapps.img` → partición **System**

**Paso 6 - Reboot:**
- Reboot → System
- Esperar 10-15 minutos el primer arranque

---

## ✅ Qué funciona
- 🔊 Audio (altavoz interno)
- 📶 WiFi
- 🔵 Bluetooth
- 📱 Pantalla táctil
- 📷 Cámara
- 🔋 Batería
- ⚡ Carga
- 🏪 Google Play Store (GApps incluidos en la ROM)

## ❌ No probado / Bugs conocidos
- LTE/Red celular (no probado)
- MTP por USB (usar TWRP para transferir archivos)

---

## 🙏 Créditos
- **BaLazS28235** - Proyecto Lifebreathing (vendor arm64 para T515)
- **Andy Yan** - GSI LineageOS builds
- **Magendanz** - TWRP para SM-T510/T515
- **Comunidad XDA Developers**

---

## 🐛 Errores comunes y soluciones

| Error | Causa | Solución |
|-------|-------|----------|
| `Permission denied` en adb push | /sdcard/ sin formatear | Format Data en TWRP primero |
| `No space left on device` | Partición system llena | Usar pendrive OTG |
| `Error 7` al flashear ZIP | Script verifica modelo | Editar updater-script y eliminar verificación |
| `Error 20` al flashear GApps | GApps incompatibles con ROM | La ROM arm64_bgN ya incluye GApps |
| `failed to mount /system_root` | Multidisabler incompatible con GSI | Omitir multidisabler con este vendor |
| Bootloop después de flashear | Orden incorrecto de flasheo | Seguir orden exacto: repartitioner → vendor → boot → ROM |
| `unsupported AVB` en heimdall | vbmeta incorrecto | Extraer vbmeta.img.lz4 del TWRP tar y descomprimir con lz4 |
| Audio no funciona | Vendor incorrecto (T510 WiFi) | Usar vendor específico T515 del proyecto Lifebreathing |
| ADB no detecta dispositivo en sistema | Bug conocido de GSI | Activar USB debugging + cambiar modo USB a "Transferencia de archivos" |
| `ERROR: Failed to detect compatible download-mode device` | Tablet no en Download Mode | Vol Abajo + Vol Arriba + conectar USB |

---

## ⚠️ Disclaimer
Este proceso borra todos los datos del dispositivo y puede anular la garantía.
Hazlo bajo tu propio riesgo. No me hago responsable por dispositivos dañados.
