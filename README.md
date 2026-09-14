# ADF Incremental Load Pipeline — Practice Project

A hands-on Azure Data Factory project built to practice incremental/upsert data loading, pipeline orchestration, and automated monitoring alerts.

## Overview

Ingests source data (CSV/API) into Azure SQL Database using a metadata-driven pipeline, applies upsert logic during transformation, and sends email notifications on pipeline success or failure.

## Architecture

```
Source (CSV / API)
      │
      ▼
Azure Data Factory Pipeline
  ├── Copy / Ingestion Activity
  ├── Mapping Data Flow
  │     ├── Schema mapping (source → sink)
  │     └── AlterRow → Upsert if 1>0
  ▼
Azure SQL Database (Serverless) — adf_db
      │
      ▼ (on success/failure)
Logic App (HTTP trigger)
  └── Sends dynamic HTML email alert
```

## Components

- **Azure Data Factory** — pipeline orchestration, Mapping Data Flow transformations
- **Azure SQL Database (Serverless tier)** — target/sink, with auto-pause enabled
- **Mapping Data Flow** — schema mapping + `AlterRow` transformation to upsert rows by key columns (insert-vs-update decision delegated to sink key matching)
- **Logic App** — HTTP-triggered workflow, dynamic JSON payload from the pipeline, sends a status email (success/failure) with a dynamically built HTML body

## Key Concepts Practiced

- Metadata-driven / incremental load pattern
- Upsert logic via `AlterRow` + sink key columns
- ADF expression syntax (`@expr` vs `@{expr}` string interpolation)
- Inline vs. Dataset objects in Data Flows
- Logic Apps HTTP trigger schema + dynamic content in actions
- Diagnosing Azure SQL serverless auto-pause/auto-resume behavior via Activity Log

## Notes

Built as a practice project to close an ADF skill gap, following the *"Azure Data Factory End-To-End Project With Azure DevOps | 2025 Zero To Pro Guide"* tutorial.
