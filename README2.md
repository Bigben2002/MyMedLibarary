flowchart TB
    %% Client
    USER[사용자]
    APP[모바일 앱\n(Flutter / React Native)]

    %% Gateway
    NGINX[Nginx\nReverse Proxy]

    %% Core Backend
    SPRING[Spring Boot\nCore Backend]
    DB[(PostgreSQL\nDatabase)]

    %% AI Service
    FASTAPI[FastAPI\nAI Identification Service]
    OCR[OCR / Image Analysis\n(OpenCV, OCR)]

    %% Automation
    N8N[n8n\nWorkflow Automation]

    %% Flow
    USER --> APP
    APP -->|HTTPS| NGINX

    NGINX -->|/api/*| SPRING
    NGINX -->|/ai/*| FASTAPI

    SPRING --> DB
    FASTAPI --> OCR

    %% Automation Triggers
    SPRING -->|Event / API| N8N
    N8N -->|Scheduled / Triggered| SPRING

    %% Return Flow
    SPRING --> NGINX
    FASTAPI --> NGINX
    NGINX --> APP

