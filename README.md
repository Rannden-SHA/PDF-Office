# 🛠 PDF Office

Aplicación de escritorio en Python para automatizar todas las tareas repetitivas que se hacen a mano en la oficina con archivos PDF y documentos. **13 herramientas en una sola app**, con backup automático y deshacer.

---

## ✨ Funcionalidades

### 🏢 Renombrar y formatear

- **Renombrar facturas** — detecta el emisor de cada factura y la renombra. Tiene **modo revisión uno a uno con visor de PDF integrado**: ves la factura, los campos detectados aparecen resaltados (CIF en amarillo, empresa en verde, fecha en azul, frase identificativa en morado), puedes **arrastrar el cursor sobre el PDF para seleccionar texto y copiarlo** (Ctrl+C), abrir un panel lateral con el texto plano de la página o usar el botón ⧉. Para cada empresa el sistema guarda una **regla multi-señal** que combina:
  - **CIF / NIF / NIE** (con tolerancia al prefijo `ES` intracomunitario)
  - **Frases identificativas** (texto que aparece siempre, útil cuando el CIF está dentro de un logo)
  - **Dominios** de email/web del documento
  - **Autor/Creator/Producer** de los metadatos del PDF
  - **Patrones de nombre de archivo** (substring o glob)

  Cualquiera de esas señales hace que el sistema reconozca a la empresa, así que las facturas siempre acaban detectadas aunque el formato cambie. La prioridad es CIF > frase > autor > dominio > nombre. La base de datos siempre se actualiza al confirmar (sin duplicados): si la regla existe, se mergea; si es nueva, se crea.
- **Prefijo / sufijo masivo** — añade un mismo texto al principio o al final del nombre de todos los archivos.
- **Numeración secuencial** — añade `001`, `002`, ... con padding configurable, posición y texto base opcional.
- **Buscar y reemplazar** — en nombres de archivo, con soporte de **regex** y opción case-sensitive.
- **Normalizar nombres** — quita tildes, pasa a minúsculas, reemplaza espacios.

### 📂 Clasificar y mover

- **Clasificar por contenido** — define reglas tipo *"si el PDF contiene X → mover a carpeta Y"*. Las reglas se evalúan en orden.
- **Organizar por fecha** — mueve archivos a subcarpetas año/mes (`2026/05` o `2026/05_mayo`). Para PDFs intenta extraer la fecha de la factura; si no, usa la fecha de modificación.
- **Detectar duplicados** — encuentra archivos con contenido idéntico (hash SHA-256) y mueve los repetidos a una papelera local recuperable.

### 📄 Manipular PDFs

- **Unir / dividir PDFs** — combina varios PDFs en uno con el orden que indiques, o divide un PDF por rangos de páginas (`1-3, 4-6, ...`) o página a página.
- **Marca de agua** — añade texto a uno o varios PDFs con posiciones (diagonal, centro, esquina, pie), color y opacidad configurables. Conserva los originales.

### 📊 Datos

- **Extraer datos a Excel** — analiza facturas en lote y exporta CIF, empresa, fecha, número de factura y total a un `.xlsx` o `.csv` con cabeceras estilizadas.

### ⚙️ Sistema

- **Historial / Deshacer** — todas las operaciones se guardan persistentemente y se pueden revertir con un clic. Cada operación muestra archivo por archivo lo que cambió.
- **Ajustes** — gestión de la base de datos de empresas con import/export CSV; cambio de tema; acceso a la carpeta de datos.

Todas las acciones tienen **vista previa antes de aplicar** (con tabla de checkboxes para desmarcar fila por fila), **backup automático** y **deshacer**.

---

## ⌨️ Atajos del modo revisión uno a uno

Cuando abres una factura para revisar:

| Tecla        | Acción                                       |
|--------------|----------------------------------------------|
| `Enter`      | Aceptar el nombre propuesto y pasar al siguiente |
| `Esc`        | Saltar (no renombrar) y pasar al siguiente   |
| `Ctrl + Z`   | Volver al archivo anterior y reabrir su decisión |
| `Ctrl + rueda` | Zoom in/out sobre el PDF                   |
| `← / →` (toolbar) | Cambiar de página dentro del mismo PDF  |
| Drag con el ratón sobre el PDF | Seleccionar texto |
| `Ctrl + C` | Copiar el texto seleccionado en el visor |
| Botón **Aa Texto** | Abre panel lateral con el texto plano de la página, seleccionable y copiable |
| Botón **⤢** | Ajustar el PDF al ancho del visor |

El cursor empieza en el campo *Empresa* — puedes empezar a escribir directamente. Sobre cada factura, el badge superior indica claramente:

- 🟢 **Reconocida en BD** — la empresa ya existe y los identificadores coinciden.
- 🟡 **En BD — se actualizará** — empresa existente, pero estás añadiendo nuevos identificadores que se mergearán.
- 🔵 **Empresa nueva — se creará** — al aceptar se creará una regla nueva con los identificadores que dejes en el bloque "Cómo identificar a esta empresa".
