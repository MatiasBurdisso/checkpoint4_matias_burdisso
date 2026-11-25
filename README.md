# Checkpoint 4 — Integraciones La Fábrica

**Autor:** Matías Burdisso  
**Proyecto:** La Fábrica Coworking  
**Plataforma:** n8n

## Objetivo

Integrar el flujo de atención comercial con correo electrónico, CRM y mensajería interna, incorporando clasificación mediante IA y revisión humana antes de responder.

## Integraciones

- **Gmail:** trigger de correos entrantes y creación de borradores. No se envían respuestas automáticamente.
- **Google Gemini:** clasificación del correo y extracción estructurada de datos comerciales mediante JSON.
- **HubSpot:** búsqueda de contactos por email antes de decidir si corresponde crear o actualizar un registro.
- **Slack:** notificación de nuevas consultas comerciales en el canal `nuevo-canal`.
- **Airtable:** persistencia de la memoria conversacional del agente, desarrollada en módulos anteriores.

## Flujo de correo

1. `Gmail Trigger` detecta un correo entrante.
2. `If1` filtra respuestas automáticas, mensajes de ausencia y notificaciones de entrega.
3. `AI Agent` clasifica el correo y extrae datos, resumen, necesidad y respuesta sugerida.
4. `If2` permite continuar a las consultas clasificadas como comerciales.
5. `Edit Fields4` prepara los datos para las integraciones.
6. Gmail crea un borrador para revisión humana.
7. Slack publica una notificación comercial.
8. `Search contacts` consulta HubSpot por email y `If3` evalúa si existe el contacto.

## Pruebas realizadas

- Recepción de un evento mediante Gmail Trigger.
- Descarte de un correo `no-reply` y paso de una consulta comercial.
- Clasificación y extracción de datos mediante Gemini y Structured Output Parser.
- Creación exitosa de un borrador Gmail con etiqueta `DRAFT`; el correo no se envió.
- Envío exitoso de una notificación a Slack.
- Búsqueda exitosa de un contacto existente en HubSpot.

## Consideraciones y limitaciones conocidas

Durante las pruebas se detectó que el nombre indicado en el cuerpo del correo puede no coincidir con la identidad del remitente. Por seguridad, las dos acciones `Create or update a contact` están **deshabilitadas en el JSON de entrega** para evitar modificar un contacto equivocado. La búsqueda en HubSpot y la bifurcación por existencia permanecen configuradas.

Antes de habilitar la escritura en HubSpot, se debe validar que el email real del remitente y los datos personales extraídos correspondan a la misma persona. También debe revisarse la separación final entre creación y actualización.

Las credenciales OAuth2 se configuran en la instancia de n8n y no se incluyen como secretos en este repositorio.

## Credenciales y evidencia

Conexiones utilizadas en n8n: Gmail, HubSpot, Slack, Airtable y Google Gemini mediante el gateway de n8n.

**Evidencia pendiente:** agregar una captura de la sección Credentials de n8n donde se vean activas las conexiones de Gmail, HubSpot y Slack. No mostrar tokens, claves privadas ni secretos.

## Archivos del repositorio

- `checkpoint4_matias_burdisso_entrega (1).json` — exportación del workflow.
- `README.md` — documentación del proyecto.
