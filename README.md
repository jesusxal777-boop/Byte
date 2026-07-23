# 🚀 Sistema de Actualizaciones - Byte Android

Esta estructura permite que la aplicación Byte Android se actualice automáticamente usando GitHub como servidor de actualizaciones.

---

## 📁 Estructura del Proyecto

```
Byte/
├── update/
│   ├── version.json          # Información de versión y actualización
│   └── changelog.md          # Historial de cambios
├── releases/
│   └── Byte.apk             # APK de la aplicación (lugar de almacenamiento)
└── README.md                # Este archivo
```

---

## 📍 Dónde Colocar el APK

1. **Ubicación:** Carpeta `releases/`
2. **Nombre del archivo:** `Byte.apk`
3. **Pasos:**
   - Compila tu aplicación Android generando el APK
   - Coloca el APK en la carpeta `releases/`
   - Haz commit y push a la rama correspondiente (idealmente `main`)
   - GitHub alojará automáticamente el archivo

**URL RAW del APK:**
```
https://raw.githubusercontent.com/jesusxal777-boop/Byte/main/releases/Byte.apk
```

---

## 🔄 Cómo Cambiar la Versión

### Archivo: `update/version.json`

Edita los siguientes campos:

```json
{
  "versionCode": 2,              // Incrementa este número (1, 2, 3...)
  "versionName": "1.1.0",        // Formato: MAYOR.MENOR.PATCH
  "downloadUrl": "https://raw.githubusercontent.com/jesusxal777-boop/Byte/main/releases/Byte.apk",
  "forceUpdate": 0,              // 0 = actualización opcional, 1 = forzada
  "updateNotes": "Descripción breve de los cambios"
}
```

### Ejemplo de Versionamiento (Semantic Versioning)

- **1.0.0** → 1.1.0 (nueva funcionalidad menor)
- **1.1.0** → 1.1.1 (bug fix)
- **1.1.1** → 2.0.0 (cambio importante/incompatible)

---

## ⚠️ Cómo Activar Actualización Obligatoria

La actualización obligatoria se controla con el campo `forceUpdate` en `version.json`:

### Actualización Opcional (default)
```json
{
  "forceUpdate": 0
}
```
El usuario verá una notificación y puede ignorarla.

### Actualización Obligatoria
```json
{
  "forceUpdate": 1
}
```
La aplicación forzará al usuario a actualizar. No podrá continuar sin hacerlo.

### Ejemplo de Caso de Uso
- **1.0.0 → 1.0.1 (bug crítico)** → `forceUpdate: 1`
- **1.0.1 → 1.1.0 (nueva feature)** → `forceUpdate: 0`
- **2.0.0 (cambio importante)** → `forceUpdate: 1`

---

## 📝 Cómo Escribir el Changelog

### Archivo: `update/changelog.md`

Sigue este formato para cada nueva versión:

```markdown
## [X.Y.Z] - YYYY-MM-DD

### ✨ Características
- Nueva funcionalidad agregada
- Mejora en la interfaz

### 🔧 Correcciones
- Se corrigió el bug de crashes
- Se mejoró el rendimiento

### ⚠️ Cambios Importantes (opcional)
- API modificada
- Requisitos actualizados

### 📝 Notas
- Información adicional relevante
```

### Ejemplo Real
```markdown
## [1.1.0] - 2024-02-15

### ✨ Características
- Nuevo sistema de notificaciones
- Tema oscuro implementado
- Opción de idiomas agregada

### 🔧 Correcciones
- Se corrigió el crash al cambiar de pantalla
- Mejor optimización de batería

### 📝 Notas
- La primera vez que abras el tema oscuro, puede tardar unos segundos
```

---

## 🔗 URL para tu Aplicación Android

Tu aplicación debe leer `version.json` desde esta URL RAW:

```
https://raw.githubusercontent.com/jesusxal777-boop/Byte/main/update/version.json
```

### Ejemplo de Llamada HTTP (Pseudocódigo)
```java
// Pseudo-código - Adáptalo a tu implementación
String versionUrl = "https://raw.githubusercontent.com/jesusxal777-boop/Byte/main/update/version.json";
JSON versionInfo = fetchJSON(versionUrl);

int latestVersionCode = versionInfo.getInt("versionCode");
String latestVersionName = versionInfo.getString("versionName");
String downloadUrl = versionInfo.getString("downloadUrl");
int forceUpdate = versionInfo.getInt("forceUpdate");
String updateNotes = versionInfo.getString("updateNotes");

// Comparar con la versión actual de la app
if (latestVersionCode > BuildConfig.VERSION_CODE) {
    // Nueva versión disponible
    if (forceUpdate == 1) {
        // Mostrar diálogo de actualización obligatoria
    } else {
        // Mostrar notificación de actualización opcional
    }
}
```

---

## 📋 Flujo de Trabajo Recomendado

### 1. Preparar nueva versión
```bash
# Actualizar version.json
# - Incrementar versionCode
# - Cambiar versionName
# - Escribir updateNotes

# Actualizar changelog.md
# - Agregar nueva entrada con fecha
# - Documentar cambios
```

### 2. Compilar APK
```bash
# Generar release APK desde Android Studio
# Colocarlo en releases/Byte.apk
```

### 3. Hacer commit y push
```bash
git add update/version.json update/changelog.md releases/Byte.apk
git commit -m "release: Versión 1.1.0 de Byte"
git push origin main
```

### 4. Verificar
- Accede a la URL RAW para confirmar que `version.json` se ve correctamente
- Prueba descargar el APK desde la URL

---

## 🛠️ Notas Importantes

✅ **GitHub sirve perfectamente como servidor de actualizaciones**
- URLs RAW son públicas y accesibles
- Actualizaciones en tiempo real
- No requiere servidor adicional

⚠️ **Consideraciones de seguridad**
- Verifica la integridad del APK en tu aplicación
- Considera firmar los APKs con certificados
- Implementa validación de checksums si es crítico

📱 **Pruebas**
- Prueba en un dispositivo/emulador la lectura de `version.json`
- Verifica que las descargas funcionan correctamente
- Comprueba que el flujo de actualizaciones funciona

---

## 📞 Soporte

Para más información sobre versionamiento en Android:
- [Android Versioning Guide](https://developer.android.com/studio/publish/versioning)
- [Semantic Versioning](https://semver.org/)

---

**¡Listo para usar! 🎉**
