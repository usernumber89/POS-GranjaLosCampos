# 🐔 POS Granja Avícola - Migración a Firebase

## ✅ Cambios Realizados

La aplicación ha sido completamente migrada de **IndexedDB (almacenamiento local del navegador)** a **Firebase Realtime Database (base de datos en la nube)**.

### Ventajas de la migración

✨ **Nube**: Acceso desde cualquier dispositivo con internet  
🔄 **Sincronización**: Los datos se sincronizan en tiempo real  
📱 **Múltiples dispositivos**: Varios usuarios pueden acceder simultáneamente  
🔐 **Seguridad**: Respaldo automático y recuperación de desastres  
⚡ **Escalabilidad**: La base de datos crece automáticamente según necesidades  
🆓 **Costo**: Nivel gratuito generoso para pequeños negocios  

---

## 🚀 Configuración Inicial (IMPORTANTE)

### Paso 1: Crear cuenta en Firebase

1. Ve a [firebase.google.com](https://firebase.google.com)
2. Haz clic en "Ir a la consola"
3. Inicia sesión con tu cuenta Google (crea una si no tienes)

### Paso 2: Crear proyecto

1. Haz clic en "Crear un proyecto"
2. Nombre del proyecto: `pos-granja-avicola` (o el que prefieras)
3. Desactiva Google Analytics (opcional)
4. Haz clic en "Crear proyecto"

### Paso 3: Crear Realtime Database

1. En el menú izquierdo, ve a **Compilación** → **Realtime Database**
2. Haz clic en "Crear base de datos"
3. Selecciona ubicación: 
   - Para Latinoamérica: `sa-east-1` (São Paulo, Brasil)
   - O la región más cercana a ti
4. Modo de seguridad: Selecciona **"Modo de prueba"**
   - ⚠️ Esto permite lectura/escritura sin autenticación (para desarrollo)
   - Cambiar a "Modo bloqueado" cuando estés listo para producción

### Paso 4: Obtener credenciales de Firebase

1. Haz clic en el engranaje (⚙️) en la esquina superior izquierda → **Configuración del proyecto**
2. Ve a la pestaña **"General"**
3. En "Tus apps", busca tu aplicación web o haz clic en **"Agregar app"** → **Web**
4. Se mostrará un objeto con tus credenciales:

```javascript
{
  apiKey: "AIzaSyD...",
  authDomain: "pos-granja-avicola.firebaseapp.com",
  databaseURL: "https://pos-granja-avicola.firebaseio.com",
  projectId: "pos-granja-avicola",
  storageBucket: "pos-granja-avicola.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abc123def456"
}
```

### Paso 5: Actualizar index.html

Abre `index.html` y busca la sección:

```javascript
const firebaseConfig = {
    apiKey: "TU_API_KEY",
    authDomain: "tu-proyecto.firebaseapp.com",
    databaseURL: "https://tu-proyecto.firebaseio.com",
    projectId: "tu-proyecto",
    storageBucket: "tu-proyecto.appspot.com",
    messagingSenderId: "tu-sender-id",
    appId: "tu-app-id"
};
```

Reemplaza estos valores con los de tu proyecto (paso 4).

### Paso 6: Configurar reglas de seguridad

1. En Firebase Console, ve a **Realtime Database** → **Reglas**
2. Reemplaza todo el contenido con:

```json
{
  "rules": {
    ".read": true,
    ".write": true,
    "lotes": {
      ".indexOn": ["activo", "numero"]
    },
    "clientes": {
      ".indexOn": ["nombre"]
    },
    "proveedores": {
      ".indexOn": ["nombre", "tipo"]
    },
    "ventas": {
      ".indexOn": ["fecha"]
    },
    "salidas": {
      ".indexOn": ["fecha", "loteId", "proveedorId"]
    }
  }
}
```

3. Haz clic en **"Publicar"**

---

## 🔧 Funcionalidades Migradas

Toda la funcionalidad se mantiene igual, pero ahora usa Firebase:

### ✓ Gestión de Lotes
- Agregar, editar, eliminar lotes
- Rastrear disponibilidad de pollos
- Marcar como activo/inactivo

### ✓ Gestión de Clientes
- Registro y búsqueda de clientes
- Importar/exportar desde Excel
- IDs automáticos (CLI-0001, CLI-0002, etc.)

### ✓ Gestión de Proveedores
- Crear y eliminar proveedores
- Tipos: Alimento, Medicamentos, Equipos, Otros
- Importar/exportar desde Excel

### ✓ Registro de Ventas
- Carrito de compras
- Descuento automático de pollos del lote
- Generación de recibos
- Historial de ventas
- Exportar ventas a Excel

### ✓ Registro de Salidas (Gastos)
- Registrar gastos por lote o proveedor
- Categorización de gastos
- Historial con filtros

### ✓ Reportes
- Total de ventas
- Pollos vendidos
- Lotes activos
- Estadísticas generales

### ✓ Backup y Restauración
- Descargar backup en JSON
- Restaurar desde backup
- Limpiar base de datos (con confirmación)

---

## 📊 Estructura de la Base de Datos

```
Firebase Realtime Database
│
├── lotes/
│   ├── -LjK9l2Kj9...
│   │   ├── numero: "LOTE-001"
│   │   ├── cantidad: 500
│   │   ├── disponibles: 450
│   │   ├── pesoPromedio: 2.5
│   │   ├── fecha: "2026-01-29"
│   │   ├── notas: "..."
│   │   └── activo: true
│   │
│   └── -LjK9l2Kj9... (más lotes)
│
├── clientes/
│   ├── -LjK9l2Kj9...
│   │   ├── clienteId: "CLI-0001"
│   │   ├── nombre: "Juan García"
│   │   ├── telefono: "+503 7890-1234"
│   │   └── direccion: "..."
│
├── proveedores/
│   ├── -LjK9l2Kj9...
│   │   ├── nombre: "Empresa X"
│   │   ├── contacto: "..."
│   │   ├── tipo: "Alimento"
│   │   └── email: "..."
│
├── ventas/
│   ├── -LjK9l2Kj9...
│   │   ├── fecha: "2026-01-29T15:30:00.000Z"
│   │   ├── total: 125.50
│   │   └── items: [...]
│
└── salidas/
    ├── -LjK9l2Kj9...
    │   ├── fecha: "2026-01-29"
    │   ├── tipo: "Gasto"
    │   ├── monto: 50.00
    │   └── descripcion: "..."
```

---

## 🔐 Seguridad en Producción

⚠️ **Las reglas actuales permiten acceso sin autenticación**

Antes de usar en producción, considera:

### 1. Agregar autenticación (Recomendado)
```json
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}
```

### 2. Restringir por usuario
```json
{
  "rules": {
    "lotes": {
      ".read": "auth != null",
      ".write": "auth != null"
    }
  }
}
```

### 3. Implementar roles
```json
{
  "rules": {
    "lotes": {
      ".read": "root.child('usuarios').child(auth.uid).child('rol').val() != null",
      ".write": "root.child('usuarios').child(auth.uid).child('rol').val() === 'admin'"
    }
  }
}
```

---

## 💾 Migración de Datos Antiguos

Si tienes datos en IndexedDB y quieres migrarlos:

1. **Opción A: Exportar desde IndexedDB local**
   - En la aplicación antigua, ve a **Base de Datos** → **Exportar Backup**
   - Se descargará un archivo `backup_*.json`

2. **Opción B: Manual**
   - Agregar registros manualmente en Firebase Console
   - O usar importación por Excel

3. **Opción C: Migraciones automáticas**
   - Contactar para scripting personalizado

---

## 🐛 Solución de Problemas

### "Error de Conexión"
- Verifica que has reemplazado las credenciales correctamente
- Comprueba que la base de datos está creada en Firebase
- Abre la consola del navegador (F12) para más detalles

### "Permiso denegado"
- Asegúrate que las reglas están en "Modo de prueba"
- O configura correctamente los permisos si estás usando autenticación

### "Base de datos vacía"
- Verifica que estés usando la misma configuración de Firebase
- Comprueba en Firebase Console que los datos se guardaron

### Los cambios no se sincronizan
- Comprueba tu conexión a internet
- Abre las DevTools (F12) → Pestaña Console para ver errores
- Verifica que los IDs de registros son únicos

---

## 📱 Múltiples dispositivos

Ahora puedes:
- Abrir la aplicación en computadora, tablet y celular simultáneamente
- Los cambios se sincronizan automáticamente
- Cada dispositivo ve los datos actualizados en tiempo real

---

## 🎯 Próximos Pasos

1. ✅ Configura Firebase (pasos arriba)
2. ✅ Actualiza las credenciales en `index.html`
3. ✅ Prueba la aplicación
4. 📊 Ingresa tus primeros datos
5. 🔐 Cuando estés listo, aumenta la seguridad

---

## 📞 Soporte

Si tienes problemas:
1. Consulta los logs en la consola del navegador (F12)
2. Verifica Firebase Console → Reglas → Monitoreo
3. Verifica la estructura de datos en Firebase Console

---

**¡Tu aplicación está lista para usar Firebase! 🚀**
