# MyMedLibarary
```mermaid
erDiagram
    APP_USER ||--o{ USER_MEDICINE : "owns"
    DRUG_MASTER ||--o{ USER_MEDICINE : "is_based_on"
    CATEGORY ||--o{ USER_MEDICINE : "classified_as"

    USER_MEDICINE ||--o{ INTAKE_RULE : "has"
    USER_MEDICINE ||--o{ INTAKE_LOG : "records"

    DRUG_MASTER ||--o{ DRUG_INTERACTION : "drug_a"
    DRUG_MASTER ||--o{ DRUG_INTERACTION : "drug_b"

    APP_USER ||--o{ PHOTO_UPLOAD : "uploads"
    PHOTO_UPLOAD ||--o{ AI_IDENTIFICATION_CANDIDATE : "produces"
    DRUG_MASTER ||--o{ AI_IDENTIFICATION_CANDIDATE : "candidate"

    APP_USER ||--o{ USER_WARNING : "receives"
    USER_WARNING }o--|| DRUG_INTERACTION : "based_on"
    USER_WARNING }o--|| USER_MEDICINE : "medicine_a"
    USER_WARNING }o--|| USER_MEDICINE : "medicine_b"

    APP_USER ||--o{ ROUTINE_PLAN : "has"
    ROUTINE_PLAN ||--o{ ROUTINE_ITEM : "contains"
    ROUTINE_ITEM }o--|| USER_MEDICINE : "uses"


    APP_USER {
      bigint user_id PK
      varchar email UK
      varchar password_hash
      varchar name
      datetime created_at
    }

    CATEGORY {
      smallint category_id PK
      varchar category_name  "상비약/영양제/병원처방약"
    }

    DRUG_MASTER {
      bigint drug_id PK
      varchar drug_name
      varchar manufacturer
      varchar form_type        "정제/캡슐/시럽 등"
      varchar color
      varchar shape
      varchar imprint_code
      text    ingredients      "주요 성분(영양제 포함)"
      text    standard_dosage   "일반적 1일 기준"
      text    recommended_time  "권장 복용 시간"
      varchar source_ref        "공공DB 식별키(있으면)"
      datetime updated_at
    }

    USER_MEDICINE {
      bigint user_medicine_id PK
      bigint user_id FK
      bigint drug_id FK
      smallint category_id FK
      varchar nickname          "사용자가 부르는 이름(선택)"
      boolean is_active
      datetime added_at
    }

    INTAKE_RULE {
      bigint intake_rule_id PK
      bigint user_medicine_id FK
      varchar time_slot         "MORNING/LUNCH/DINNER/BED"
      time intake_time          "08:00 같은 실제 시간(선택)"
      smallint times_per_day    "1~n"
      varchar dose_unit         "정/캡슐/ml"
      decimal dose_amount
      text notes
    }

    INTAKE_LOG {
      bigint intake_log_id PK
      bigint user_medicine_id FK
      datetime taken_at
      boolean taken
      text memo
    }

    DRUG_INTERACTION {
      bigint interaction_id PK
      bigint drug_a_id FK
      bigint drug_b_id FK
      varchar severity          "HIGH/MEDIUM/LOW"
      text interaction_reason
      text guidance
      varchar source_ref
      datetime updated_at
    }

    USER_WARNING {
      bigint warning_id PK
      bigint user_id FK
      bigint interaction_id FK
      bigint user_medicine_a_id FK
      bigint user_medicine_b_id FK
      boolean is_read
      datetime created_at
    }

    PHOTO_UPLOAD {
      bigint photo_id PK
      bigint user_id FK
      varchar image_url
      varchar status            "UPLOADED/PROCESSING/DONE/FAILED"
      datetime created_at
    }

    AI_IDENTIFICATION_CANDIDATE {
      bigint candidate_id PK
      bigint photo_id FK
      bigint drug_id FK
      decimal confidence
      text evidence             "색/모양/각인 근거"
    }

    ROUTINE_PLAN {
      bigint routine_plan_id PK
      bigint user_id FK
      date plan_date
      varchar version_tag       "추천 알고리즘 버전"
      datetime created_at
    }

    ROUTINE_ITEM {
      bigint routine_item_id PK
      bigint routine_plan_id FK
      varchar time_slot         "MORNING/LUNCH/DINNER/BED"
      bigint user_medicine_id FK
      smallint order_no
    }
