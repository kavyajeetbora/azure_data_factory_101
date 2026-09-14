# ADF End-to-End Data Pipeline — Practice Project

A hands-on Azure Data Factory project covering multi-source ingestion, incremental loading, Medallion-style transformation, automated alerting, and Git-based version control.

## Overview

Ingests data from three different source types into a Bronze layer, transforms it into a Silver layer using Mapping Data Flows, and sends email notifications on pipeline success or failure — all version-controlled via GitHub integration.

## Architecture

```
┌─ On-Prem Ingestion ───────────┐
│                                │
├─ SQL Incremental Ingestion ───┤──▶ Bronze Layer
│  (Azure SQL DB + Watermark)   │
│                                │
└─ API Ingestion (Open Source) ─┘
              │
              ▼
     Primary Orchestrator Pipeline
       (runs all 3 sub-pipelines)
              │
              ├──▶ Logic App → Email Alert (Success / Failure)
              │
              ▼
     Mapping Data Flow (ETL)
              │
              ▼
         Silver Layer
```

## Steps

1. **Ingestion pipelines** (built using Copy Data tool and related activities) for three source types:
   - **On-Prem Ingestion** — on-premises source to Azure via Integration Runtime
   - **SQL Incremental Ingestion** — provisioned an Azure SQL Database, ingested data using a **watermark-based incremental load** pattern
   - **API Ingestion** — pulled data from an open-source public API/URL
   - All three land data in the **Bronze layer**

2. **Primary orchestrator pipeline** — runs all three ingestion pipelines, then triggers a **Logic App** to send an email notification on success or failure

3. **ETL transformation (Bronze → Silver)** — built using **Mapping Data Flow**, including schema mapping and `AlterRow`-based upsert logic

4. **Git integration** — connected the Data Factory to **GitHub** for source control / version tracking of pipeline, dataset, and Data Flow definitions

## Components

- **Azure Data Factory** — pipeline orchestration, Copy Data activities, Mapping Data Flows
- **Azure SQL Database (Serverless)** — incremental-load source, watermark tracking
- **Logic App** — HTTP-triggered, dynamic JSON payload from the pipeline, sends a status email (success/failure) with dynamic HTML body
- **GitHub** — source control for ADF pipeline/dataset/Data Flow JSON definitions

## Key Concepts Practiced

- Watermark-based incremental loading
- Medallion architecture (Bronze → Silver)
- Upsert logic via `AlterRow` + sink key columns
- ADF expression syntax (`@expr` vs `@{expr}` string interpolation)
- Logic Apps HTTP trigger schema + dynamic content in actions
- ADF Git integration and collaboration-branch workflow
- Diagnosing Azure SQL serverless auto-pause/auto-resume behavior via Activity Log

## Cost Analysis on Azure

<img width="1007" height="357" alt="image" src="https://github.com/user-attachments/assets/1c05d766-9e40-48bb-a488-c83c2f2a7b1b" />



## Reference Tutorial

[![Watch the video](https://img.youtube.com/vi/Za_9XYwPbKM/0.jpg)](https://youtu.be/Za_9XYwPbKM)
