# Sistema de Inventario — Instalación paso a paso

Java 21 + Swing + SQLite + Maven.

> **Credenciales por defecto (demo):** usuario `admin` · contraseña `admin`

> **IMPORTANTE:** el sistema se inicia **SOLO** con `EJECUTAR_SISTEMA.cmd`, haciendo doble clic desde la carpeta `SistemaInventario_T1`. Otras formas de abrirlo suelen fallar (ver sección 4).

---

## 1. Requisitos

| Programa | Versión | Cómo comprobarlo (en CMD) |
|---|---|---|
| JDK | 21 | `java -version` |
| Maven | 3.8+ | `mvn -version` |
| Python *(opcional, solo para resetear la clave)* | 3.x | `python --version` |

Si algún comando dice "no se reconoce", instálalo y agrega su carpeta `bin` al PATH de Windows. Cierra y abre CMD después.

---

## 2. Descomprimir sin que Windows bloquee los archivos

Windows marca como "no confiables" los archivos descargados de internet o de Drive. Eso activa **SmartScreen** o **Smart App Control** y bloquea el `.cmd`. Para evitarlo, haz esto **antes** de descomprimir:

1. Clic derecho en el `.zip` descargado → **Propiedades**.
2. Abajo, marca **Desbloquear** (si aparece) → **Aplicar** → **Aceptar**.
3. Recién ahora descomprime (clic derecho → *Extraer todo*).

**Alternativa con PowerShell**, si ya descomprimiste. Ábrelo en la carpeta del proyecto y ejecuta:

```powershell
Get-ChildItem -Recurse | Unblock-File
```

---

## 3. Estructura correcta

```
Proyectos_ORGANIZADO/
└── 01_PROYECTO_ACTIVO/
    └── SistemaInventario_T1/          <-- TRABAJA SIEMPRE DESDE AQUÍ
        ├── EJECUTAR_SISTEMA.cmd       <-- EL ÚNICO ARRANQUE QUE SE USA
        ├── pom.xml
        ├── src/
        └── database/
            └── inventario.db          <-- ÚNICA base de datos que usa la app
```

### Regla de oro de la base de datos

La app solo lee `SistemaInventario_T1/database/inventario.db`. La ruta es **relativa a la carpeta desde donde se ejecuta**.

- `Proyectos_ORGANIZADO/database/inventario.db` (la carpeta de la **raíz**) **NO la usa la app**. Cambiar usuarios ahí no sirve de nada.
- Si ejecutas desde otra carpeta, la app abre o crea otra BD distinta y el login falla.

---

## 4. Por qué solo funciona con `EJECUTAR_SISTEMA.cmd`

El `.cmd` hace tres cosas necesarias:

1. Se posiciona en su propia carpeta (`cd /d "%~dp0"`). Sin esto, `database/inventario.db` no se encuentra.
2. Crea un respaldo inicial de la BD en `database/respaldos/`.
3. Compila (`mvn clean compile`) y arranca (`mvn exec:java`).

Si abres el proyecto de otra forma (doble clic al `.jar`, `mvn` desde otra carpeta, o un IDE mal configurado), el programa puede no encontrar la BD y el login falla aunque la clave sea correcta.

**Si usas un IDE** (NetBeans, IntelliJ, VS Code): configura el *working directory* (directorio de trabajo) como `SistemaInventario_T1`.

---

## 5. Si Windows bloquea el `.cmd`

Al ser un script, Windows puede mostrar uno de estos avisos.

### a) "Windows protegió su PC" (SmartScreen)
1. Clic en **Más información**.
2. Clic en **Ejecutar de todas formas**.

### b) El `.cmd` se cierra solo o dice "Una directiva de control de aplicaciones bloqueó este archivo" (Smart App Control)
Primero prueba la sección 2 (desbloquear el zip). Si sigue bloqueado y es **tu propio equipo**:

1. Abre **Seguridad de Windows**.
2. Ve a **Control de aplicaciones y navegador**.
3. Entra a **Configuración de Smart App Control**.
4. Selecciona **Desactivado**.

> **Cuidado:** en la mayoría de versiones de Windows 11, una vez desactivado Smart App Control no se puede volver a activar sin reinstalar o restablecer Windows. Si no quieres desactivarlo, usa una de estas alternativas:
> - Ejecuta los comandos manualmente en CMD (ver sección 6, opción B).
> - Usa otro equipo donde no esté activo.

### c) El antivirus lo pone en cuarentena
Agrega la carpeta `SistemaInventario_T1` a las exclusiones del antivirus y restaura el archivo.

---

## 6. Ejecutar

**Opción A (recomendada):** doble clic en `EJECUTAR_SISTEMA.cmd`, dentro de `SistemaInventario_T1`.

**Opción B (manual, si el `.cmd` está bloqueado):** abre CMD y escribe:

```cmd
cd /d "RUTA\COMPLETA\Proyectos_ORGANIZADO\01_PROYECTO_ACTIVO\SistemaInventario_T1"
mvn clean compile
mvn exec:java
```

Se abre la ventana de login. Ingresa `admin` / `admin`.

---

## 7. Instalar la base de datos con el usuario admin

1. Copia el archivo `inventario.db` entregado (el que ya trae `admin`).
2. Pégalo en `SistemaInventario_T1/database/`, reemplazando el existente.
3. **Guarda una copia limpia** en otro lugar, por ejemplo `inventario_base_admin.db`. Es tu salvavidas.

---

## 8. Problemas comunes

| Síntoma | Causa | Solución |
|---|---|---|
| "Credenciales incorrectas" con `admin`/`admin` | BD equivocada (la de la raíz) o ejecutado desde otra carpeta | Revisa que el `inventario.db` esté en `SistemaInventario_T1/database/` y usa el `.cmd` |
| Después de 5 intentos fallidos no deja entrar | La cuenta se bloquea **15 minutos** | Espera, o resetea (sección 9) |
| Solo existe el usuario `kelly` | BD original sin `admin`. Su clave no se puede recuperar | Instala la BD de la sección 7 o resetea |
| "No se encontro database\inventario.db" | Ejecutaste desde otra carpeta | Ejecuta desde `SistemaInventario_T1` |
| El `.cmd` se cierra solo o no abre | Bloqueo de Windows | Sección 2 y sección 5 |
| `mvn` / `java` "no se reconoce" | No están en el PATH | Instálalos y reabre CMD |
| Error de compilación | JDK distinto de 21 | Instala JDK 21 y revisa `java -version` |
| "RUC invalido" con un RUC de 11 dígitos | El RUC se valida con prefijo (10, 15, 17 o 20) y dígito verificador | Usa un RUC real o uno válido, como `20100070970` |

---

## 9. Resetear la contraseña (si se pierde o se bloquea)

Las contraseñas **no se pueden recuperar**: se guardan cifradas (PBKDF2, 600 000 iteraciones). Solo se pueden reemplazar.

**Con Python:**

1. Cierra la app.
2. Copia `resetear_admin.py` a `SistemaInventario_T1`.
3. En CMD, dentro de esa carpeta:

```cmd
python resetear_admin.py
```

Esto deja `admin` / `admin` como ADMINISTRADOR y desbloquea todas las cuentas.

**Sin Python:** cierra la app y reemplaza `database/inventario.db` por tu copia limpia `inventario_base_admin.db`. Perderás los datos cargados desde que la guardaste.

---

## 10. Seguridad y notas

- `admin`/`admin` es solo para pruebas o demo. Si el sistema se usa en serio, cambia la clave.
- Al **crear** un administrador desde la app, la clave debe tener entre 12 y 128 caracteres. Al **iniciar sesión** se acepta cualquier longitud, por eso `admin` funciona.
- Usuario: 3 a 50 caracteres (letras, números y `. _ @ -`).
- El RUC debe ser peruano: 11 dígitos, prefijo 10/15/17/20 y dígito verificador válido.
- Haz copias de `database/inventario.db` antes de probar cosas grandes.

---

## 11. Checklist rápido

- [ ] JDK 21 y Maven instalados (`java -version`, `mvn -version`)
- [ ] ZIP **desbloqueado** (Propiedades → Desbloquear) y luego descomprimido completo
- [ ] `inventario.db` copiado en `SistemaInventario_T1/database/`
- [ ] Copia limpia guardada aparte
- [ ] `EJECUTAR_SISTEMA.cmd` ejecutado desde `SistemaInventario_T1` (o Opción B manual)
- [ ] Login con `admin` / `admin`
