# Checkpoint 4 — Integraciones La Fábrica

**Autor:** Matías Burdisso  
**Proyecto:** La Fábrica Coworking  
**Plataforma de automatización:** n8n

## Objetivo

Integrar el flujo de atención comercial de La Fábrica con correo electrónico, un CRM y mensajería interna, incorporando clasificación con IA y revisión humana antes de responder.

## Integraciones

- **Gmail:** trigger de correos entrantes y creación de borradores. El flujo prepara respuestas, pero no las envía automáticamente.
- **Google Gemini:** analiza el correo, clasifica la consulta y estructura los datos comerciales mediante un parser JSON.
- **HubSpot:** busca contactos por email antes de decidir si corresponde crear o actualizar un registro.
- **Slack:** publica una notificación en el canal `nuevo-canal` con los datos de la consulta.
- **Airtable:** se conserva la arquitectura previa de memoria conversacional del agente.

## Flujo de correo

1. `Gmail Trigger` detecta un correo entrante.
2. `If1` filtra respuestas automáticas, mensajes de ausencia y notificaciones de entrega.
3. `AI Agent` analiza el contenido y devuelve tipo de consulta, datos del contacto, resumen, necesidad y respuesta sugerida.
4. `If2` deja pasar las consultas clasificadas como comerciales.
5. `Edit Fields4` prepara los campos para las integraciones.
6. Gmail crea un borrador para revisión humana.
7. Slack envía una notificación comercial.
8. `Search contacts` consulta HubSpot por email y `If3` evalúa si existe un contacto.

## Validaciones realizadas

- Gmail Trigger: recepción de evento de prueba.
- Filtro anti-respuestas automáticas: se verificó el descarte de un correo `no-reply` y el paso de una consulta comercial.
- Agente IA y parser estructurado: clasificación y extracción de datos de una consulta comercial.
- Gmail: creación exitosa de un borrador con etiqueta `DRAFT`; no se envió el mensaje.
- Slack: envío exitoso de una notificación al canal `nuevo-canal`.
- HubSpot: búsqueda exitosa de un contacto existente por email.

## Consideraciones y limitaciones conocidas

- Durante las pruebas se detectó que el nombre incluido en el cuerpo del correo puede no coincidir con la identidad del remitente. Por ese motivo, las dos acciones `Create or update a contact` quedan **deshabilitadas en este archivo de entrega** para evitar modificar un contacto equivocado. La búsqueda en HubSpot permanece disponible.
- Antes de habilitar la escritura en HubSpot, se debe validar la correspondencia entre el email real del remitente y los datos personales extraídos del cuerpo del mensaje.
- La rama de HubSpot está planteada como búsqueda previa más bifurcación por existencia. Las dos acciones de escritura requieren una revisión final para separar correctamente actualización y creación.
- Las credenciales OAuth2 se configuran en la instancia de n8n y no se incluyen como secretos en este repositorio.
- El workflow conserva también el flujo conversacional y la memoria de Airtable desarrollados en etapas anteriores.

## Credenciales

Conexiones utilizadas en n8n:
- Gmail
- HubSpot
- Slack
- Airtable
- Google Gemini (administrado mediante el gateway de n8n)

**Para completar la evidencia de la consigna:** agregar una captura de la sección Credentials de n8n donde se vean las conexiones activas de Gmail, HubSpot y Slack. No mostrar tokens, claves privadas ni secretos.

## Archivos

- `checkpoint4_matias_burdisso.json`: exportación del workflow desde n8n.
- `README.md`: descripción de la arquitectura, integraciones, pruebas y limitaciones.

