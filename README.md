# Sistema de Inventario — Instalación paso a paso

Java 21 + Swing + SQLite + Maven.

> **Credenciales por defecto (demo):** usuario `admin` · contraseña `admin`

---

## 1. Requisitos

| Programa | Versión | Cómo comprobarlo (en CMD) |
|---|---|---|
| JDK | 21 | `java -version` |
| Maven | 3.8+ | `mvn -version` |
| Python *(opcional, solo para resetear la clave)* | 3.x | `python --version` |

Si algún comando dice "no se reconoce", instálalo y agrega su carpeta `bin` al PATH de Windows. Cierra y abre CMD después.

---

## 2. Estructura correcta

Descomprime el ZIP completo. La carpeta del proyecto es **esta**:

```
Proyectos_ORGANIZADO/
└── 01_PROYECTO_ACTIVO/
    └── SistemaInventario_T1/          <-- TRABAJA SIEMPRE DESDE AQUÍ
        ├── EJECUTAR_SISTEMA.cmd
        ├── pom.xml
        ├── src/
        └── database/
            └── inventario.db          <-- ÚNICA base de datos que usa la app
```

### Regla de oro de la base de datos

La app solo lee `SistemaInventario_T1/database/inventario.db`. La ruta es relativa a la carpeta desde donde se ejecuta.

- `Proyectos_ORGANIZADO/database/inventario.db` (la carpeta de la **raíz**) **NO la usa la app**. Es solo un respaldo o ejemplo. Cambiar usuarios ahí no sirve de nada.
- Si ejecutas desde otra carpeta, la app crea o abre otra BD distinta y el login falla.

---

## 3. Instalar la base de datos con el usuario admin

1. Copia el archivo `inventario.db` entregado (el que ya trae `admin`).
2. Pégalo en `SistemaInventario_T1/database/`, reemplazando el existente.
3. **Guarda una copia limpia** de ese mismo archivo en otro lugar, por ejemplo `inventario_base_admin.db`. Es tu salvavidas si algo se rompe.

---

## 4. Ejecutar

**Opción A (recomendada):** doble clic en `EJECUTAR_SISTEMA.cmd`, dentro de `SistemaInventario_T1`.

**Opción B (manual):**

```cmd
cd Proyectos_ORGANIZADO\01_PROYECTO_ACTIVO\SistemaInventario_T1
mvn clean compile
mvn exec:java
```

Se abre la ventana de login. Ingresa `admin` / `admin`.

> La primera vez, el `.cmd` crea `database/respaldos/inventario_antes_migraciones.db`. Es un respaldo de la BD tal como estaba antes de arrancar.

---

## 5. Problemas comunes

| Síntoma | Causa | Solución |
|---|---|---|
| "Credenciales incorrectas" con `admin`/`admin` | Reemplazaste la BD equivocada (la de la raíz) o el usuario no existe | Revisa que el `inventario.db` esté en `SistemaInventario_T1/database/` |
| Después de 5 intentos fallidos no deja entrar ni con la clave correcta | La cuenta se bloquea **15 minutos** | Espera 15 min, o resetea (ver sección 6) |
| Solo existe el usuario `kelly` | BD original sin `admin`. Su clave no se puede recuperar | Instala la BD de la sección 3 o resetea (sección 6) |
| "No se encontro database\inventario.db" | Ejecutaste desde otra carpeta | Ejecuta desde `SistemaInventario_T1` |
| `mvn` / `java` "no se reconoce" | No están en el PATH | Instálalos y reabre CMD |
| Error de compilación | JDK distinto de 21 | Instala JDK 21 y revisa `java -version` |

---

## 6. Resetear la contraseña (si se pierde o se bloquea)

Las contraseñas **no se pueden recuperar**: se guardan cifradas (PBKDF2, 600 000 iteraciones). Solo se pueden reemplazar.

**Con Python:**

1. Cierra la app.
2. Copia `resetear_admin.py` a `SistemaInventario_T1`.
3. En CMD, dentro de esa carpeta:

```cmd
python resetear_admin.py
```

Esto deja `admin` / `admin` como ADMINISTRADOR y desbloquea todas las cuentas. Puedes repetirlo cuando quieras.

**Sin Python:** cierra la app y reemplaza `database/inventario.db` por tu copia limpia `inventario_base_admin.db`. Perderás los datos cargados desde que la guardaste.

---

## 7. Seguridad y notas

- `admin`/`admin` es solo para pruebas o demo. Si el sistema se usa en serio, cambia la clave.
- Al **crear** un administrador desde la app (`CrearAdministradorInicial`), la contraseña debe tener entre 12 y 128 caracteres. Al **iniciar sesión** se acepta cualquier longitud, por eso `admin` funciona.
- Usuario: 3 a 50 caracteres (letras, números y `. _ @ -`).
- Haz copias de `database/inventario.db` antes de probar cosas grandes.
- Verifica que el rol de administrador llegue al menú principal (ver `docs/ESTADO_DEL_PROYECTO.md`, hallazgo 1).

---

## 8. Checklist rápido

- [ ] JDK 21 y Maven instalados (`java -version`, `mvn -version`)
- [ ] ZIP descomprimido completo
- [ ] `inventario.db` copiado en `SistemaInventario_T1/database/`
- [ ] Copia limpia guardada aparte
- [ ] `EJECUTAR_SISTEMA.cmd` ejecutado desde `SistemaInventario_T1`
- [ ] Login con `admin` / `admin`
