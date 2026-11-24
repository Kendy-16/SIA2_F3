# Proyectos — paquete organizado

Proyecto: **Sistema de Inventario** (Java 21 + Swing + SQLite). De 1 782 entradas con 3 copias del proyecto y mucha duplicación,
quedó un paquete con **un único proyecto activo** y todo lo demás ordenado y documentado.

```
Proyectos_ORGANIZADO/
├── 00_LEEME.md                       ← este archivo
├── 01_PROYECTO_ACTIVO/               ← la versión que compila y está más avanzada
│   └── SistemaInventario_T1/         (103 archivos · ejecutar con EJECUTAR_SISTEMA.cmd)
│       └── docs/ESTADO_DEL_PROYECTO.md   ← qué está listo, qué falta y hallazgos
├── 02_DOCUMENTACION/                 (52 archivos) informes de revisión, auditorías, estructura, informe de organización
├── 03_RESPALDOS/                     (87 archivos) bases de datos y código «antes de…»
├── 04_VERSIONES_ANTERIORES/          (26 archivos) solo lo que difiere: Login y Temporal
└── 05_PAQUETES_Y_PROTOTIPOS/         (5 archivos) ZIP sueltos y prototipo de Productos
```

## Lo esencial

* **Versión activa:** `SistemaInventario_T1` (la principal). Compila con **0 errores** (60 archivos, 27 238 líneas; verificado con `javac` 21, confirma con `mvn clean compile`). Código sin modificar.
* **Pendiente más importante:** el rol de usuario no llega al menú, así que nadie accede a las opciones de administrador
  (detalle y arreglo mínimo en `docs/ESTADO_DEL_PROYECTO.md`, hallazgo 1).
* **Rescatable de otras versiones:** historial de movimientos (`HistorialMovimientosView`, `MovimientoHistorial`) en `04_VERSIONES_ANTERIORES/Instalacion_Login`.
* **Eliminado:** 22 archivos vacíos creados por comandos de CMD, 3 carpetas `target/`, copias redundantes y ZIP contenedores
  (lista completa y motivo en `02_DOCUMENTACION/MANIFIESTO_DESCARTADOS.csv`).

## Cómo abrirlo

Abre `01_PROYECTO_ACTIVO/SistemaInventario_T1` en NetBeans, IntelliJ o VS Code (proyecto Maven), o en Windows ejecuta `EJECUTAR_SISTEMA.cmd`.
Requiere JDK 21 y Maven.
