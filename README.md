# Calixto

Calixto es una plataforma de evaluación académica con dos perfiles principales:

- **Docente**: crea grados, crea exámenes, comparte exámenes por código con un grado específico, configura tiempo y revisa resultados.
- **Estudiante**: entra al portal estudiantil, ingresa el código compartido por el docente y presenta el examen habilitado.

## Alcance funcional inicial

El alcance funcional inicial está documentado en `docs/spec-funcional.md`.

## Funcionalidades clave

1. Los exámenes siempre están asociados a un grado previamente creado.
2. Existen dos perfiles de usuario: docente y estudiante.
3. El docente puede crear grados y exámenes, y compartir un examen con un grado específico mediante un código.
4. El estudiante accede al portal estudiantil y entra al examen con el código compartido.
5. El sistema califica automáticamente en una escala de **1.0 a 5.0**.
6. El docente puede definir si un examen tiene límite de tiempo.
7. Cada examen tiene un solo intento por estudiante, pero el docente puede rehabilitar a un estudiante para un nuevo intento.
8. El sistema genera reportes finales en **PDF** y **Excel** con las calificaciones.

## Próximos pasos sugeridos

- Diseñar el modelo de datos.
- Definir flujos de autenticación por perfil.
- Implementar motor de evaluación automática.
- Generar exportaciones PDF/XLSX para reportes.
