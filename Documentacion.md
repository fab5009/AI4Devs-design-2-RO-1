Documentación Completa del Proyecto: Sistema de Seguimiento de Candidatos (ATS) con IAEste documento consolida toda la planificación y el diseño del producto para el nuevo sistema ATS, desde la visión inicial hasta el desglose técnico de la primera historia de usuario.Parte 1: Descripción y Diseño del Producto (Basado en PRD)Descripción del Software ATS - Valor Añadido y Ventajas CompetitivasNuestro sistema ATS (Applicant Tracking System) es una plataforma integral diseñada para revolucionar la gestión del talento humano mediante la automatización inteligente y la colaboración en tiempo real. El valor principal radica en transformar un proceso tradicionalmente fragmentado y manual en una experiencia fluida y basada en datos.Las ventajas competitivas principales incluyen la integración nativa de inteligencia artificial para el análisis predictivo de candidatos, un sistema de colaboración en tiempo real que permite a reclutadores y managers trabajar simultáneamente sobre los mismos procesos, y automatizaciones inteligentes que reducen el tiempo de contratación hasta en un 60%. Además, nuestro enfoque en la experiencia del usuario tanto para el equipo de HR como para los candidatos nos diferencia significativamente del mercado actual.Funciones Principales del SistemaEl sistema opera a través de siete funciones centrales que siguen el ciclo natural de contratación. La creación de empleos permite a los managers definir posiciones con asistencia de IA para optimizar las descripciones y requisitos. La publicación multiplica automáticamente las ofertas a través de múltiples canales incluyendo job boards, redes sociales y el sitio web corporativo.La recepción de aplicaciones centraliza todos los candidatos en una interfaz unificada con parsing automático de CVs y scoring inicial. La revisión de aplicaciones incorpora filtros inteligentes y rankings automáticos basados en machine learning. Las pruebas online se integran seamless con proveedores externos mientras mantienen el tracking interno.La programación de entrevistas utiliza calendarios inteligentes que se sincronizan con todos los stakeholders, y finalmente, la contratación de candidatos seleccionados incluye workflows de onboarding automatizados y generación de documentación legal.Descripción del Diagrama: Lean Canvas(El diagrama original era un SVG que representaba un Lean Canvas. A continuación, se describe su contenido en formato de texto).Problema: Procesos de contratación lentos y manuales; falta de colaboración; pérdida de candidatos de calidad; datos fragmentados.Solución: ATS con IA integrada; colaboración en tiempo real; automatizaciones inteligentes; analytics predictivos.Propuesta Única de Valor: El único ATS que combina IA predictiva con colaboración en tiempo real, reduciendo el tiempo de contratación en un 60% y mejorando la calidad de los candidatos.Ventaja Injusta: Algoritmos propios de matching; base de datos de patrones de éxito; red de integraciones exclusivas.Segmentos de Clientes: Empresas medianas (50-500 empleados); Startups en crecimiento; Agencias de reclutamiento.Métricas Clave: Time-to-hire; Tasa de conversión; Satisfacción de usuarios (NPS); ARR; Retención de clientes.Canales: Ventas directas B2B; Marketing digital; Partners e integradores; Eventos de HR.Estructura de Costos: Desarrollo de software; Infraestructura cloud; Equipo de ventas y marketing; Soporte al cliente; Licencias de IA.Fuentes de Ingreso: Suscripciones SaaS (varios planes); Fees de implementación; Servicios de consultoría; Módulos premium.Casos de Uso PrincipalesCaso de Uso 1: Gestión Completa de Proceso de ContrataciónDescripción: Abarca el journey completo desde que un hiring manager crea una posición con asistencia de IA, la publica, recibe y filtra candidatos, coordina entrevistas y finalmente contrata al candidato ideal.Actores: Hiring Manager.Sistema: Sistema ATS.Interacciones: El manager interactúa con el sistema para crear la oferta, publicarla, revisar y filtrar aplicaciones, programar entrevistas y generar la oferta final. El sistema se integra con sistemas externos como Job Boards, calendarios y plataformas de testing.Caso de Uso 2: Colaboración en Tiempo Real entre Reclutadores y ManagersDescripción: Se enfoca en la capacidad del sistema para facilitar la colaboración simultánea. Los reclutadores y managers pueden compartir notas, calificar candidatos y ver dashboards actualizados en tiempo real.Actores: Reclutador, Hiring Manager, Team Member.Sistema: Hub de Colaboración ATS (basado en WebSocket y Real-time DB).Interacciones: Los actores interactúan con el hub para compartir notas, calificar, discutir y construir consenso. El sistema provee notificaciones push, chat, video llamadas y comentarios en vivo.Caso de Uso 3: Automatización Inteligente con IADescripción: Se centra en las capacidades de automatización del sistema. La IA analiza CVs, sugiere candidatos ideales, automatiza la programación de entrevistas y genera reportes predictivos.Actores: HR Specialist, Sistema IA.Sistema: Motor de IA y Automatización.Interacciones: El HR Specialist utiliza el sistema, que a su vez emplea el motor de IA para realizar análisis de CVs, matching de candidatos, detección de sesgos y programación automática, basándose en datos históricos y del mercado.Modelo de Datos (Esquema ERD)erDiagram
    COMPANY {
        uuid id PK
        string name
        boolean is_active
    }
    DEPARTMENT {
        uuid id PK
        uuid company_id FK
        string name
    }
    USER {
        uuid id PK
        uuid company_id FK
        string email
        enum role
    }
    JOB_POSITION {
        uuid id PK
        uuid company_id FK
        string title
        enum status
    }
    JOB_STAGE {
        uuid id PK
        uuid job_position_id FK
        string name
        int order_position
    }
    CANDIDATE {
        uuid id PK
        string email
        string first_name
        string last_name
    }
    APPLICATION {
        uuid id PK
        uuid job_position_id FK
        uuid candidate_id FK
        uuid current_stage_id FK
        enum status
    }
    APPLICATION_STAGE_HISTORY {
        uuid id PK
        uuid application_id FK
        uuid stage_id FK
        datetime moved_at
    }
    INTERVIEW {
        uuid id PK
        uuid application_id FK
        datetime scheduled_at
        string meeting_link
    }
    ASSESSMENT {
        uuid id PK
        uuid job_position_id FK
        string name
        enum type
    }
    CANDIDATE_ASSESSMENT {
        uuid id PK
        uuid assessment_id FK
        uuid application_id FK
        enum status
        decimal score
    }
    COLLABORATION_NOTE {
        uuid id PK
        uuid application_id FK
        uuid author_id FK
        text content
    }
    
    COMPANY ||--o{ DEPARTMENT : has
    COMPANY ||--o{ USER : employs
    COMPANY ||--o{ JOB_POSITION : creates
    JOB_POSITION ||--o{ JOB_STAGE : has
    JOB_POSITION ||--o{ APPLICATION : receives
    CANDIDATE ||--o{ APPLICATION : submits
    APPLICATION ||--o{ INTERVIEW : scheduled_for
    APPLICATION ||--o{ COLLABORATION_NOTE : discussed_in
Diseño del Sistema a Alto Nivelgraph TB
    subgraph "Client Layer"
        WEB[Web Application]
        MOBILE[Mobile App]
    end
    
    subgraph "API Gateway & Load Balancer"
        GATEWAY[API Gateway]
    end
    
    subgraph "Core Application Services"
        USER_SVC[User Management]
        JOB_SVC[Job Management]
        APP_SVC[Application Tracking]
        COLLAB_SVC[Collaboration Service]
    end
    
    subgraph "AI & Automation Layer"
        AI_MATCHING[AI Candidate Matching]
        AI_SCREENING[Resume Screening AI]
        WORKFLOW[Workflow Automation Engine]
    end
    
    subgraph "Data Layer"
        PRIMARY_DB[(Database<br/>PostgreSQL)]
        CACHE[(Cache<br/>Redis)]
        SEARCH[(Search<br/>Elasticsearch)]
        BLOB[File Storage<br/>AWS S3]
    end

    WEB --> GATEWAY
    MOBILE --> GATEWAY
    GATEWAY --> USER_SVC
    GATEWAY --> JOB_SVC
    GATEWAY --> APP_SVC
    GATEWAY --> COLLAB_SVC
    JOB_SVC --> AI_MATCHING
    APP_SVC --> AI_SCREENING
    USER_SVC --> PRIMARY_DB
    JOB_SVC --> PRIMARY_DB
    APP_SVC --> SEARCH
Parte 2: Historias de UsuarioHU-01: Creación de Ofertas de Empleo Asistida por IAComo Reclutador,Quiero utilizar un asistente de IA para que me sugiera y ayude a redactar descripciones de trabajo y requisitos,Para optimizar la oferta, atraer candidatos más calificados y reducir el tiempo que dedico a tareas de redacción.HU-02: Filtrado y Ranking Automático de CandidatosComo Hiring Manager,Quiero que el sistema analice y clasifique automáticamente a los nuevos candidatos que aplican a mi oferta,Para poder enfocar mi tiempo de revisión en los perfiles más prometedores y no perder talento valioso.HU-03: Colaboración en Tiempo Real en el Perfil de un CandidatoComo miembro del equipo de contratación,Quiero añadir notas, calificar al candidato y ver los comentarios de mis compañeros en tiempo real,Para agilizar la toma de decisiones y centralizar el feedback.HU-04: Programación de Entrevistas con Calendarios SincronizadosComo Reclutador,Quiero ver la disponibilidad combinada de todos los entrevistadores y del candidato en una única vista,Para poder programar entrevistas rápidamente.HU-05: Publicación de Ofertas en Múltiples Canales con un ClicComo Reclutador,Quiero publicar una oferta de empleo aprobada en múltiples plataformas (LinkedIn, Indeed) con un solo clic,Para maximizar la visibilidad de la oferta y ahorrar tiempo.Parte 3: Product Backlog y Priorización1. Matriz de EisenhowerUrgenteNo UrgenteImportanteHacer Primero:• HU-01: Creación con IA• HU-02: Ranking con IA• HU-05: Publicación MulticanalPlanificar:• HU-03: Colaboración Real• HU-04: Programación EntrevistasNo ImportanteDelegar:(Ninguna)Eliminar:(Ninguna)2. Método MoSCoWCategoríaHistorias de UsuarioMust-Have• HU-01, HU-02, HU-05Should-Have• HU-03, HU-04Could-Have(Ninguna en esta fase)Won't-Have(Ninguna en esta fase)Conclusión: La prioridad máxima es HU-01: Creación de Ofertas de Empleo Asistida por IA.Parte 4: Planificación Detallada - HU-01Desglose en Tickets de TrabajoID TicketTítuloEquipoDescripción BreveTicket-BE-01API para CRUD de OfertasBackendEndpoints REST para crear, leer, actualizar y eliminar ofertas.Ticket-BE-02Integración con Servicio IABackendFachada para comunicarse con el motor de IA para obtener sugerencias.Ticket-FE-01UI del Formulario de CreaciónFrontendComponente React del wizard de creación de ofertas.Ticket-FE-02Lógica de Interacción con IAFrontendLógica para llamar al backend y mostrar las sugerencias de IA al usuario.Ticket-AI-01Endpoint de Modelo IAAI/MLExponer endpoint del modelo que genera las sugerencias.Ticket-DO-01Despliegue del Servicio de IADevOpsConfigurar infraestructura y CI/CD para el nuevo microservicio de IA.Decisión de Diseño Clave: ¿Sugerencias en Tiempo Real vs. Botón?Tras un análisis con la técnica de los 6 Sombreros para Pensar, el equipo concluyó:Hechos: La latencia del modelo de IA (>1s) y el costo asociado hacen que las sugerencias en tiempo real (on-keystroke) sean problemáticas.Riesgos: Altos costos, mala performance y complejidad técnica elevada para la opción en tiempo real.Decisión para el MVP: Se implementará la funcionalidad con un botón explícito "Generar Sugerencias con IA". Esta opción es más segura, económica, rápida de implementar y garantiza una experiencia de usuario consistente.Estimación del EsfuerzoID TicketStory Points (SP)Días-PersonaTicket-BE-013 SP2 DíasTicket-BE-025 SP3 DíasTicket-FE-015 SP3 DíasTicket-FE-023 SP2 DíasTicket-AI-018 SP5 DíasTicket-DO-015 SP3 DíasTotal HU-0129 SP~18 Días-Persona