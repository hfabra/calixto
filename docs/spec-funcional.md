# Especificación funcional inicial

## 1. Objetivo

Construir una plataforma de exámenes académicos donde un docente pueda administrar grados y exámenes, compartirlos mediante un código y obtener reportes automáticos de resultados; mientras que el estudiante pueda presentar el examen desde un portal independiente usando dicho código.

---

## 2. Roles del sistema

### 2.1 Perfil docente
El docente puede:

- Crear, editar y desactivar grados.
- Crear, editar, publicar y cerrar exámenes.
- Asociar un examen a un grado existente.
- Compartir un examen con un grado específico mediante un código único.
- Configurar si el examen tiene o no límite de tiempo.
- Consultar resultados por examen, grado y estudiante.
- Rehabilitar a un estudiante para permitir un nuevo intento.
- Exportar reportes finales en PDF y Excel.

### 2.2 Perfil estudiante
El estudiante puede:

- Ingresar al portal de estudiantes.
- Digitar el código del examen compartido por el docente.
- Ver únicamente los exámenes habilitados para su grado.
- Presentar el examen una sola vez, salvo que el docente lo rehabilite.
- Consultar su resultado, si la política institucional lo permite.

---

## 3. Reglas funcionales principales

### RF-01. Asociación obligatoria con un grado
Todo examen debe estar vinculado a un grado previamente creado. No se puede publicar ni compartir un examen sin grado asociado.

### RF-02. Separación por perfiles
El sistema debe diferenciar claramente la experiencia del docente y la del estudiante mediante permisos, rutas y vistas independientes.

### RF-03. Creación y compartición mediante código
El docente debe poder:

1. Crear un examen.
2. Seleccionar el grado al que aplica.
3. Generar o visualizar un código único de acceso.
4. Compartir ese código con los estudiantes del grado correspondiente.

### RF-04. Acceso del estudiante por código
El estudiante debe entrar al módulo de estudiante e ingresar el código recibido. El sistema valida:

- que el código exista,
- que el examen esté activo,
- que pertenezca al grado del estudiante,
- que el estudiante tenga intento disponible.

### RF-05. Calificación automática
El sistema debe calificar automáticamente el examen al finalizarlo, usando una escala numérica de **1.0 a 5.0**.

#### Criterios mínimos de calificación
- Debe existir una fórmula de conversión desde respuestas correctas a nota final.
- La nota debe almacenarse con precisión decimal.
- La calificación debe quedar disponible para reportes y consultas posteriores.

### RF-06. Temporizador opcional
El docente podrá decidir si el examen:

- **tiene tiempo límite**, definiendo duración en minutos; o
- **no tiene tiempo límite**.

Si existe límite de tiempo, el sistema debe cerrar automáticamente el intento cuando expire.

### RF-07. Un solo intento por defecto
Cada estudiante dispone de un único intento por examen.

#### Excepción
El docente puede rehabilitar manualmente a un estudiante para otorgarle un nuevo intento. Esta acción debe quedar registrada en auditoría con:

- docente que autorizó,
- estudiante rehabilitado,
- examen afectado,
- fecha y hora.

### RF-08. Reportes finales
El sistema debe generar reportes finales de calificaciones por examen, incluyendo exportación a:

- **PDF**, para consulta o impresión.
- **Excel**, para análisis y consolidación.

#### Campos sugeridos del reporte
- Nombre del examen.
- Grado.
- Código del examen.
- Nombre del estudiante.
- Fecha de presentación.
- Número de intento.
- Estado (presentado, vencido, rehabilitado, pendiente).
- Puntaje obtenido.
- Calificación final en escala 1 a 5.

---

## 4. Flujos principales

### Flujo 1. Creación y publicación del examen por docente
1. El docente inicia sesión.
2. Crea o selecciona un grado.
3. Crea un examen.
4. Asocia el examen al grado.
5. Configura duración opcional.
6. Publica el examen.
7. El sistema genera un código de acceso.
8. El docente comparte el código con los estudiantes.

### Flujo 2. Presentación del examen por estudiante
1. El estudiante entra al portal estudiantil.
2. Ingresa el código del examen.
3. El sistema valida el acceso.
4. El estudiante presenta el examen.
5. El sistema calcula automáticamente la nota.
6. El intento queda cerrado.

### Flujo 3. Rehabilitación de intento
1. El docente consulta resultados.
2. Identifica a un estudiante que requiere nuevo acceso.
3. Rehabilita el intento del estudiante.
4. El sistema vuelve a habilitar exactamente un intento adicional.
5. La acción queda registrada.

### Flujo 4. Generación de reportes
1. El docente entra al módulo de reportes.
2. Filtra por examen, grado o rango de fechas.
3. El sistema genera el consolidado.
4. El docente descarga el archivo en PDF o Excel.

---

## 5. Reglas de negocio sugeridas

- **RN-01**: Un código de examen debe ser único por publicación.
- **RN-02**: Un examen solo puede ser visible para estudiantes del grado asociado.
- **RN-03**: Un estudiante no puede iniciar un segundo intento si ya consumió el disponible y no ha sido rehabilitado.
- **RN-04**: Si el tiempo del examen expira, el sistema debe enviar automáticamente las respuestas guardadas.
- **RN-05**: Toda rehabilitación debe quedar auditada.
- **RN-06**: Los reportes deben reflejar tanto intentos originales como intentos rehabilitados.

---

## 6. Entidades sugeridas

### Usuario
- id
- nombre
- correo
- rol (`DOCENTE` / `ESTUDIANTE`)
- estado

### Grado
- id
- nombre
- descripción
- estado
- docente_responsable_id

### Examen
- id
- título
- descripción
- grado_id
- código_acceso
- duración_minutos (nullable)
- tiene_límite_tiempo
- estado
- fecha_publicación

### Pregunta
- id
- examen_id
- enunciado
- tipo
- puntaje
- orden

### Opción / Respuesta esperada
- id
- pregunta_id
- contenido
- es_correcta

### Intento
- id
- examen_id
- estudiante_id
- número_intento
- fecha_inicio
- fecha_fin
- estado
- puntaje_total
- calificación_final
- rehabilitado_por_docente

### Auditoría de rehabilitación
- id
- intento_id o examen_id
- estudiante_id
- docente_id
- motivo
- fecha_hora

---

## 7. Criterios de aceptación iniciales

### CA-01
**Dado** un docente autenticado, **cuando** crea un examen, **entonces** debe seleccionar un grado existente antes de publicarlo.

### CA-02
**Dado** un estudiante autenticado, **cuando** ingresa un código válido, **entonces** solo podrá acceder al examen asignado a su grado.

### CA-03
**Dado** un examen finalizado, **cuando** el sistema procesa las respuestas, **entonces** debe generar automáticamente una calificación entre 1.0 y 5.0.

### CA-04
**Dado** un examen con tiempo límite, **cuando** el temporizador llega a cero, **entonces** el intento debe cerrarse automáticamente.

### CA-05
**Dado** un estudiante que ya agotó su intento, **cuando** un docente lo rehabilita, **entonces** el sistema debe permitir exactamente un nuevo intento.

### CA-06
**Dado** un examen con resultados registrados, **cuando** el docente exporta el consolidado, **entonces** debe obtener un archivo PDF y un archivo Excel con las calificaciones.

---

## 8. Recomendaciones técnicas para la implementación

- Separar claramente el portal docente del portal estudiantil.
- Modelar la relación `Examen -> Grado` como obligatoria.
- Usar códigos de acceso no predecibles.
- Registrar eventos de auditoría para rehabilitaciones y cierres automáticos por tiempo.
- Preparar una capa de exportación desacoplada para PDF y XLSX.
- Diseñar la nota final como decimal para soportar la escala de 1 a 5 sin pérdida de precisión.
