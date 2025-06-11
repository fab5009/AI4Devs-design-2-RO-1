## **Planificación Detallada: HU-01 Creación de Ofertas con IA**

Este documento desglosa la historia de usuario HU-01 en tareas técnicas, simula el proceso de toma de decisiones y estima el esfuerzo requerido para su implementación.

### **1\. Desglose en Tickets de Trabajo**

Aquí dividimos la User Story en tickets específicos para cada equipo.

**Epopeya:** HU-01: Creación de Ofertas de Empleo Asistida por IA

| ID Ticket | Título | Equipo | Descripción Breve |
| :---- | :---- | :---- | :---- |
| **Ticket-BE-01** | API para CRUD de Ofertas de Empleo | Backend | Crear y gestionar los endpoints de la API REST para crear, leer, actualizar y eliminar ofertas de empleo en la base de datos. |
| **Ticket-BE-02** | Integración con Servicio de Sugerencias IA | Backend | Desarrollar un servicio o fachada que se comunique con el motor de IA para solicitar y recibir sugerencias, manejando la autenticación y el formato de datos. |
| **Ticket-FE-01** | UI del Formulario de Creación de Ofertas | Frontend | Implementar el componente de React para el formulario/wizard de creación de ofertas, incluyendo todos los campos, validaciones y la estructura visual. |
| **Ticket-FE-02** | Lógica de Interacción con Asistente de IA | Frontend | Desarrollar la lógica para llamar al backend, recibir las sugerencias de la IA y mostrarlas de forma dinámica y no intrusiva al usuario. |
| **Ticket-AI-01** | Endpoint de Modelo IA para Sugerencias | AI/ML | Exponer un endpoint del modelo de Machine Learning que, dado un título de puesto, genere y devuelva descripciones, requisitos y rangos salariales. |
| **Ticket-DO-01** | Despliegue del Servicio de IA | DevOps | Configurar la infraestructura, el pipeline de CI/CD y el monitoreo para el nuevo microservicio de IA, asegurando su escalabilidad y disponibilidad. |

### **2\. Simulación de Reunión de Planificación (Role-Play)**

**Participantes:**

* **Valeria:** Product Manager (Líder de la reunión)  
* **David:** Tech Lead (Arquitectura y visión técnica general)  
* **Laura:** Senior Frontend Dev (Experiencia de usuario)  
* **Carlos:** Senior Backend Dev (APIs y base de datos)  
* **Sofía:** ML Engineer (Modelo de IA)

**(La reunión comienza)**

**Valeria (PM):** Buenos días a todos. Gracias por unirse. Hoy vamos a desglosar la primera historia de nuestro backlog: "Creación de Ofertas Asistida por IA". Como saben, esta es una funcionalidad clave. La idea es que cuando un reclutador escriba "Desarrollador Senior", le ayudemos activamente a construir la mejor oferta posible.

**David (Tech Lead):** Perfecto. Arquitectónicamente, esto implica un nuevo microservicio para la IA, que Sofía está liderando, y que el backend de Carlos consumirá para luego servirlo al frontend de Laura. Mi principal pregunta es sobre la experiencia de usuario y la performance: ¿las sugerencias aparecerán en tiempo real mientras el usuario escribe, o a través de un botón de "Sugerir"?

**Laura (Frontend):** Buena pregunta, David. Una sugerencia en tiempo real podría sentirse "mágica" y muy proactiva. Sin embargo, si hay latencia, podría ser frustrante. Imagina escribir y que las sugerencias tarden 2 segundos en aparecer, moviendo el layout. Un botón de "Generar sugerencias" es más predecible y le da el control al usuario.

**Carlos (Backend):** Desde el backend, un botón es más sencillo y económico. Evitamos un bombardeo de llamadas a la API (y al servicio de IA) con cada letra que se teclea (on-keystroke). Podríamos implementar un debounce, pero aun así son muchas llamadas. Un clic es una sola llamada, una sola transacción.

**Sofía (ML Engineer):** Y desde el punto de vista del modelo, una llamada por clic es mucho más eficiente. Nuestro modelo generativo tiene una latencia de unos 400-800ms. Si sumamos la latencia de red, podríamos estar por encima del segundo. Hacerlo en tiempo real requeriría una optimización considerable del modelo, quizás sacrificando algo de calidad en la respuesta por velocidad.

**Valeria (PM):** Entiendo. Parece que tenemos una decisión técnica importante que tomar que impacta directamente en la experiencia de usuario, el costo y la complejidad. Propongo que usemos la técnica de los **6 Sombreros para Pensar** para analizar esto y tomar la mejor decisión para el MVP.

### **3\. Análisis con 6 Sombreros para Pensar: ¿Sugerencias en Tiempo Real vs. Botón?**

**Valeria (Sombrero Azul \- Proceso):** Ok equipo, vamos a organizar la discusión. Empecemos con los hechos puros.

* **David (Sombrero Blanco \- Hechos):**  
  1. La latencia del modelo de IA es de \~400-800ms.  
  2. La latencia total (red \+ API Gateway \+ Backend \+ IA) será probablemente \>1 segundo.  
  3. Una implementación en tiempo real (on-keystroke) generaría múltiples llamadas a la API por segundo por usuario activo.  
  4. Una implementación con botón genera una sola llamada.  
  5. Cada llamada a la API de IA tiene un costo asociado.  
* **Laura (Sombrero Rojo \- Emociones/Intuición):**  
  * "Siento que la sugerencia en tiempo real, si fuera instantánea, sería impresionante y nos haría ver muy avanzados. Pero intuyo que la frustración de la lentitud superaría el asombro. Un botón se siente más seguro y confiable, aunque menos 'sexy'."  
* **Carlos (Sombrero Negro \- Riesgos/Precauciones):**  
  1. **Riesgo de Costo:** Con la opción en tiempo real, los costos de la API de IA podrían dispararse inesperadamente si muchos usuarios usan la función a la vez.  
  2. **Riesgo de Performance:** Una latencia alta puede causar que el UI "salte" o se congele, creando una pésima experiencia de usuario.  
  3. **Riesgo de Complejidad:** La lógica de debounce y manejo de estado en el frontend, y la gestión de carga en el backend, son significativamente más complejas para la opción en tiempo real.  
* **Sofía (Sombrero Amarillo \- Beneficios):**  
  1. **Beneficio del Botón:** Controlamos el costo y la carga de forma predecible. Garantizamos una experiencia de usuario consistente. La implementación es más rápida, lo que significa que entregamos valor antes.  
  2. **Beneficio Potencial (Tiempo Real):** Si logramos optimizarlo, el "wow factor" podría ser un gran diferenciador competitivo y mejorar la percepción de la marca como líder en innovación.  
* **David (Sombrero Verde \- Creatividad/Alternativas):**  
  1. ¿Y si combinamos? Podríamos tener un pequeño icono de "rayo" que se activa después de que el usuario deja de escribir por 2 segundos (un debounce largo), y al hacer clic en él se generan las sugerencias.  
  2. ¿O si las sugerencias no aparecen en el formulario principal, sino en una barra lateral que se actualiza de forma asíncrona para no interferir con la escritura?  
  3. ¿Podríamos precargar sugerencias para los 20 puestos más comunes y servirlas instantáneamente, y usar la llamada a la API solo para puestos menos frecuentes?  
* **Valeria (Sombrero Azul \- Conclusión y Decisión):**  
  * "Excelente discusión. Resumiendo: el **Sombrero Blanco** nos dice que la opción en tiempo real es lenta y costosa hoy. El **Sombrero Negro** confirma que esos son riesgos serios. Aunque el **Sombrero Amarillo** y el **Rojo** ven el potencial atractivo, la realidad técnica nos frena. Las ideas del **Sombrero Verde** son geniales para futuras iteraciones.  
  * **Decisión para el MVP:** Implementaremos la funcionalidad con un **botón explícito de "Generar Sugerencias con IA"**. Es la solución más segura, rápida de implementar y eficiente en costos. Nos permite entregar la funcionalidad principal sin riesgos de performance. En el backlog, crearemos una nueva historia para "Explorar mejoras de performance en las sugerencias de IA" para no perder las ideas creativas."

### **4\. Estimación del Esfuerzo**

Se realiza una sesión de Planning Poker para estimar en Story Points (SP) y luego se traduce a una estimación en horas/días.

| ID Ticket | Estimación (Story Points) | Estimación (Días-Persona) | Justificación |
| :---- | :---- | :---- | :---- |
| **Ticket-BE-01** | 3 SP | 2 Días | Tarea estándar de CRUD. Modelo de datos ya definido. |
| **Ticket-BE-02** | 5 SP | 3 Días | Requiere definir el contrato con el servicio de IA, manejar errores y timeouts. Mayor incertidumbre que un CRUD normal. |
| **Ticket-FE-01** | 5 SP | 3 Días | El formulario es un wizard con varios pasos y validaciones complejas. Requiere buen manejo de estado. |
| **Ticket-FE-02** | 3 SP | 2 Días | Lógica de llamada a la API y renderizado de resultados. Complejidad media, ya que la decisión del "botón" simplifica mucho el trabajo. |
| **Ticket-AI-01** | 8 SP | 5 Días | Es el núcleo de la funcionalidad. Requiere ajustar el modelo, definir el formato de la respuesta (JSON) y asegurar la calidad y baja latencia de las sugerencias. |
| **Ticket-DO-01** | 5 SP | 3 Días | Configurar un nuevo microservicio en Kubernetes/Cloud, incluyendo CI/CD, logging y alertas. Es una tarea conocida pero con muchos pasos. |
| **Total HU-01** | **29 SP** | **\~18 Días-Persona** | \- |

Resumen de la Estimación:  
La estimación total para completar la historia de usuario HU-01 es de 29 Story Points, lo que se traduce en aproximadamente 18 días-persona de trabajo. La mayor complejidad reside en el desarrollo del propio servicio de IA (Ticket-AI-01), que es una tarea de 8 SP. Los demás componentes tienen una complejidad media. Esta estimación considera el desarrollo, las pruebas unitarias y de integración, y el despliegue de todos los componentes necesarios para que la funcionalidad esté lista para producción.