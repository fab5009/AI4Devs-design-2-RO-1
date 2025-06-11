## **Historias de Usuario Clave para el ATS**

A continuación se presentan 5 historias de usuario fundamentales derivadas de las funcionalidades principales descritas en el PRD del sistema ATS. Cada una está diseñada para capturar una necesidad específica del usuario y entregar valor medible.

### **HU-01: Creación de Ofertas de Empleo Asistida por IA**

* **Como** Reclutador,  
* **Quiero** utilizar un asistente de IA para que me sugiera y ayude a redactar descripciones de trabajo y requisitos,  
* **Para** optimizar la oferta, atraer candidatos más calificados y reducir el tiempo que dedico a tareas de redacción.

Descripción:  
El sistema debe proporcionar una interfaz para crear una nueva oferta de empleo. Durante este proceso, una herramienta de IA analizará el título del puesto y el sector, y sugerirá activamente descripciones de responsabilidades, requisitos de experiencia, habilidades y rangos salariales competitivos basados en datos del mercado y del historial de contrataciones exitosas de la empresa.  
**Criterios de Aceptación:**

* **Dado que** estoy en el formulario de "Crear Nueva Oferta",  
* **Cuando** ingreso un título de puesto como "Desarrollador Senior de Backend",  
* **Entonces** el sistema debe mostrar en menos de 5 segundos una sección de "Sugerencias de IA" con al menos 3 opciones de descripción de puesto y una lista de 5 a 10 habilidades recomendadas.  
* **Dado que** he seleccionado una sugerencia de la IA,  
* **Cuando** hago clic en el botón "Usar esta descripción",  
* **Entonces** el texto sugerido debe poblar automáticamente el campo de "Descripción del Puesto" en el formulario.  
* **Dado que** el sistema ha sugerido un rango salarial,  
* **Cuando** acepto la sugerencia,  
* **Entonces** los campos "Salario Mínimo" y "Salario Máximo" se deben actualizar con los valores propuestos.

**Notas Adicionales:**

* La IA debe ser entrenada con datos de mercado actualizados y con el historial de la propia empresa para mejorar la relevancia.  
* Debe existir la opción para que el usuario edite libremente las sugerencias de la IA antes de guardarlas.  
* Es crucial medir la tasa de adopción de las sugerencias de IA para evaluar su utilidad.

**Tareas:**

1. **Diseño UX/UI:**  
   * Diseñar el wizard o formulario de creación de ofertas.  
   * Diseñar la interfaz del asistente de IA (cómo se muestran las sugerencias).  
2. **Backend:**  
   * Desarrollar el endpoint de la API para crear/editar ofertas (POST /jobs, PUT /jobs/{id}).  
   * Integrar con el servicio o modelo de IA generativa para obtener las sugerencias.  
   * Crear la lógica para guardar la oferta en la base de datos (tabla JOB\_POSITION).  
3. **Frontend:**  
   * Implementar el formulario de creación de ofertas en React/Next.js.  
   * Realizar la llamada a la API de IA al detectar cambios en el campo de título.  
   * Renderizar las sugerencias de IA en la interfaz.  
   * Implementar la lógica para poblar los campos del formulario con las sugerencias.  
4. **Testing:**  
   * Crear pruebas unitarias para la lógica de negocio.  
   * Realizar pruebas de integración entre el frontend, backend y el servicio de IA.  
   * Realizar pruebas de usabilidad con reclutadores reales.

### **HU-02: Filtrado y Ranking Automático de Candidatos**

* **Como** Hiring Manager,  
* **Quiero** que el sistema analice y clasifique automáticamente a los nuevos candidatos que aplican a mi oferta,  
* **Para** poder enfocar mi tiempo de revisión en los perfiles más prometedores y no perder talento valioso.

Descripción:  
Una vez que una oferta está publicada y comienza a recibir aplicaciones, el sistema debe procesar automáticamente cada CV. Utilizando IA, debe extraer información clave (experiencia, habilidades, educación) y compararla con los requisitos del puesto para generar un "score de compatibilidad". Los candidatos deben aparecer en una lista priorizada según este score.  
**Criterios de Aceptación:**

* **Dado que** un nuevo candidato ha aplicado a la oferta "Analista de Datos",  
* **Cuando** accedo a la vista de "Candidatos" de esa oferta,  
* **Entonces** el nuevo candidato debe aparecer en la lista con un score de compatibilidad visible (ej. "92% de compatibilidad").  
* **Dado que** estoy viendo la lista de candidatos,  
* **Cuando** hago clic en el score de un candidato,  
* **Entonces** el sistema debe mostrar un desglose de por qué se asignó ese puntaje (ej. "Cumple con 5/6 habilidades clave", "3 años de experiencia de los 5 requeridos").

**Notas Adicionales:**

* El modelo de IA debe ser configurable para evitar sesgos (ej. por edad, género, etc.).  
* El "score" debe ser recalculado si los requisitos del puesto se modifican.

**Tareas:**

1. **Diseño UX/UI:**  
   * Diseñar el dashboard de candidatos para una oferta.  
   * Diseñar la visualización del score y el desglose de compatibilidad.  
2. **Backend:**  
   * Desarrollar un servicio de "parsing" de CVs (puede ser una integración externa).  
   * Desarrollar el endpoint para recibir aplicaciones (POST /applications).  
   * Implementar el algoritmo de scoring en el AI\_SCREENING.  
   * Guardar el score en la tabla APPLICATION.  
3. **Frontend:**  
   * Implementar la vista de la lista de candidatos.  
   * Mostrar el score y permitir la ordenación por este.  
   * Implementar el modal o vista de detalle del score.  
4. **Testing:**  
   * Probar el parser con diferentes formatos de CV (.pdf, .docx).  
   * Validar la precisión del algoritmo de scoring con un set de datos de prueba.

### **HU-03: Colaboración en Tiempo Real en el Perfil de un Candidato**

* **Como** miembro del equipo de contratación (reclutador, hiring manager, entrevistador),  
* **Quiero** añadir notas, calificar al candidato y ver los comentarios de mis compañeros en tiempo real en el perfil del candidato,  
* **Para** agilizar la toma de decisiones, centralizar el feedback y evitar la comunicación por canales externos como email o chat.

Descripción:  
Dentro de la página de perfil de un candidato, varios usuarios autorizados pueden ver la información del candidato simultáneamente. La página debe incluir un panel de colaboración donde se pueden dejar comentarios, usar @menciones para notificar a otros, y ver la actividad reciente de otros miembros del equipo sin necesidad de recargar la página.  
**Criterios de Aceptación:**

* **Dado que** el Reclutador A y el Hiring Manager B están viendo el perfil del mismo candidato,  
* **Cuando** el Reclutador A escribe un comentario "Me gustó su experiencia en el proyecto X" y lo publica,  
* **Entonces** el Hiring Manager B debe ver aparecer ese comentario en su pantalla en menos de 3 segundos, sin recargar la página.  
* **Dado que** estoy escribiendo un comentario en el perfil de un candidato,  
* **Cuando** escribo "@Ana" y el sistema me muestra a "Ana Pérez \- HR Specialist",  
* **Entonces** al publicar el comentario, Ana Pérez debe recibir una notificación dentro del sistema.

**Notas Adicionales:**

* Se requiere una solución técnica para la comunicación en tiempo real, como WebSockets o un servicio como Firebase Realtime Database.  
* Es fundamental gestionar los permisos para definir quién puede ver qué comentarios (ej. notas privadas vs. públicas).

**Tareas:**

1. **Arquitectura:**  
   * Definir e implementar la solución de tiempo real (ej. COLLAB\_SVC con WebSockets).  
2. **Diseño UX/UI:**  
   * Diseñar el panel de colaboración, incluyendo el feed de actividades y el campo para añadir comentarios.  
3. **Backend:**  
   * Desarrollar los endpoints de la API para gestionar notas (POST /applications/{id}/notes).  
   * Implementar la lógica en el COLLAB\_SVC para transmitir los nuevos comentarios a los clientes conectados.  
   * Implementar la lógica de notificaciones para las @menciones.  
4. **Frontend:**  
   * Establecer la conexión WebSocket en la página del perfil del candidato.  
   * Implementar la interfaz del panel de colaboración.  
   * Enviar y recibir eventos de nuevos comentarios a través del WebSocket.  
5. **Testing:**  
   * Realizar pruebas de carga para asegurar que el sistema de tiempo real soporta múltiples usuarios concurrentes.

### **HU-04: Programación de Entrevistas con Calendarios Sincronizados**

* **Como** Reclutador,  
* **Quiero** ver la disponibilidad combinada de todos los entrevistadores requeridos y del candidato en una única vista,  
* **Para** poder programar entrevistas rápidamente, evitando el cruce de correos para encontrar un horario compatible.

Descripción:  
El sistema debe permitir al reclutador seleccionar a los entrevistadores para una etapa de entrevista. Luego, debe conectarse a sus calendarios (Google, Outlook) y mostrar una vista unificada de los huecos disponibles. El reclutador puede seleccionar un hueco, y el sistema enviará las invitaciones a todos los participantes automáticamente.  
**Criterios de Aceptación:**

* **Dado que** he seleccionado a 3 entrevistadores para una entrevista,  
* **Cuando** accedo a la función "Programar Entrevista",  
* **Entonces** el sistema debe mostrar una vista de calendario con los horarios en los que los 3 entrevistadores están libres simultáneamente.  
* **Dado que** he seleccionado un horario y he confirmado la entrevista,  
* **Cuando** reviso los calendarios de los entrevistadores y del candidato,  
* **Entonces** todos deben tener un nuevo evento de calendario con los detalles de la entrevista (enlace de la videollamada, CV del candidato adjunto, etc.).

**Notas Adicionales:**

* La integración con los servicios de calendario es la parte más compleja y debe manejar la autenticación (OAuth 2.0) de forma segura.  
* Se debe considerar la opción de que los candidatos puedan proponer sus propios horarios.

**Tareas:**

1. **Infraestructura/Seguridad:**  
   * Configurar los proyectos de desarrollador en Google y Microsoft para obtener las credenciales de OAuth 2.0.  
2. **Diseño UX/UI:**  
   * Diseñar la interfaz de selección de entrevistadores.  
   * Diseñar la vista de calendario unificada.  
3. **Backend:**  
   * Implementar el flujo de autenticación OAuth 2.0 para que los usuarios conecten sus calendarios.  
   * Desarrollar el CALENDAR\_SVC para leer la disponibilidad (free/busy) de múltiples calendarios.  
   * Desarrollar el endpoint para crear el evento en los calendarios de los participantes.  
4. **Frontend:**  
   * Implementar la interfaz para que los usuarios conecten sus cuentas de calendario.  
   * Mostrar la vista de disponibilidad combinada.  
   * Permitir la selección de un horario y enviar la solicitud de programación.  
5. **Testing:**  
   * Probar exhaustivamente la integración con Google Calendar y Outlook Calendar.  
   * Verificar que las invitaciones se crean correctamente y con toda la información necesaria.

### **HU-05: Publicación de Ofertas en Múltiples Canales con un Clic**

* **Como** Reclutador,  
* **Quiero** publicar una oferta de empleo aprobada en el sitio de carreras de mi empresa, LinkedIn y Indeed simultáneamente con un solo clic,  
* **Para** maximizar la visibilidad de la oferta y ahorrar el tiempo de publicarla manualmente en cada plataforma.

Descripción:  
Una vez que una oferta de empleo ha sido creada y aprobada, el reclutador tendrá una opción para "Publicar". Al usar esta opción, el sistema presentará una lista de canales de publicación configurados (ej. Portal de Empleo, LinkedIn, etc.). El reclutador puede seleccionar los canales deseados y, con un clic, el sistema distribuirá la oferta a todas las plataformas seleccionadas usando sus respectivas APIs.  
**Criterios de Aceptación:**

* **Dado que** tengo una oferta de empleo en estado "Aprobada",  
* **Cuando** hago clic en "Publicar" y selecciono "LinkedIn" e "Indeed",  
* **Entonces** el sistema debe mostrar un mensaje de confirmación "Publicado exitosamente en 2 canales".  
* **Dado que** la oferta ha sido publicada,  
* **Cuando** reviso la página de empresa en LinkedIn y el portal de Indeed,  
* **Entonces** la nueva oferta de empleo debe estar visible públicamente en ambas plataformas en menos de 5 minutos.

**Notas Adicionales:**

* Requiere que la empresa configure y autorice las integraciones con cada job board.  
* El sistema debe ser capaz de manejar diferentes formatos y requisitos de cada plataforma.

**Tareas:**

1. **Investigación y Configuración:**  
   * Investigar las APIs de publicación de empleos de LinkedIn, Indeed, etc.  
   * Configurar las cuentas de desarrollador necesarias.  
2. **Diseño UX/UI:**  
   * Diseñar el modal o la página donde el reclutador selecciona los canales de publicación.  
3. **Backend:**  
   * Desarrollar el JOB\_BOARD\_INT para abstraer la lógica de cada integración.  
   * Crear los adaptadores específicos para la API de LinkedIn, Indeed, etc.  
   * Desarrollar el endpoint que inicie el proceso de publicación (POST /jobs/{id}/publish).  
4. **Frontend:**  
   * Implementar la interfaz de selección de canales.  
   * Realizar la llamada a la API para iniciar la publicación.  
   * Mostrar el feedback al usuario (éxito, fallo, progreso).  
5. **Testing:**  
   * Realizar pruebas de publicación "end-to-end" para cada canal integrado.  
   * Verificar que la información del puesto se muestra correctamente en cada plataforma externa.