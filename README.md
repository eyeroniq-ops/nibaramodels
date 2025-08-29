# Panel administrativo para cursos y workshops (Netlify CMS)

Este repo incluye un panel listo para usar con Decap/Netlify CMS para gestionar:
- Cursos y horarios
- Alumnos y perfiles
- Inscripciones
- Pagos (ingresos)
- Gastos
- Instructores

Los datos se guardan como archivos JSON en `content/` y los adjuntos en `static/uploads/`.

## Estructura
- `admin/` UI del CMS y `config.yml` con colecciones y relaciones.
- `content/` datos versionados por Git (Netlify guarda cambios vía Git Gateway).
- `index.html` página mínima con enlace al panel.
- `netlify.toml` configuración de publicación.

## Despliegue en Netlify
1) Conecta el repo a Netlify y despliega (Build command vacío y Publish dir `.`).
2) Activa Identity (Settings → Identity → Enable Identity) y Git Gateway (Settings → Identity → Services → Enable Git Gateway).
3) En Identity → Registration, elige “Invite only” o “Open” según tu preferencia.
4) Invita a los administradores (Identity → Invite users). Inicia sesión en `/admin/` con Identity.

Notas:
- El backend está configurado con `git-gateway` en la rama `main`. Si tu repo usa `master`, cambia `admin/config.yml`.
- `publish_mode: editorial_workflow` permite crear borradores y publicar con control.

## Colecciones y lógica
- Cursos: tipo (curso/workshop), modalidad (recurrente/fecha fija), horarios o fechas, precio, cupo, instructores.
- Instructores: datos básicos y foto.
- Alumnos: perfil completo y campos de contacto.
- Inscripciones: relaciona un Alumno con un Curso y el horario elegido; estados (pendiente/confirmada/cancelada).
- Pagos: relaciona una Inscripción y opcionalmente un Alumno; método, estado y comprobante.
- Gastos: categorías, proveedor, centro de costo opcional vinculado a un Curso.

Relaciones:
- Inscripciones → Alumno y Curso (relation widget).
- Pagos → Inscripción (obligatorio) y Alumno (opcional).
- Gastos → Curso (opcional) para asociar costos por curso.
- Cursos → Instructores (múltiple).

Validaciones y coherencia:
- Campos numéricos con mínimos, fechas con `datetime` sin hora cuando aplica.
- Usa el campo `id` como clave estable para relaciones; si lo omites el CMS genera un slug, mantenlo estable.
- Para cursos recurrentes usa `horarios`; para fecha fija usa `fechas`.

## Desarrollo local (opcional)
Puedes usar `npx decap-server` para el `local_backend` (si tienes Node y red). Abre `http://localhost:8080/admin/`.

## Personalización
- Cambia opciones de monedas/categorías en `content/ajustes/parametros.json`.
- Agrega vistas públicas según necesidad; este repo prioriza funcionalidad del panel.

## Seguridad
- Mantén el panel detrás de Netlify Identity. Usa “Invite only” para control de acceso.

