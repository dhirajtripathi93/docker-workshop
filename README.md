# 🚀 AI-Powered Data Ingestion & Quality Audit Pipeline

## 📌 Executive Summary
A production-grade, containerized data ingestion pipeline designed to stream multi-million-row NYC TLC Taxi datasets into a PostgreSQL warehouse. This project integrates a pre-load AI validation framework utilizing **OpenAI** and **Anthropic** APIs to perform automated anomaly detection, generate executive audit reports, and output exploratory data visualizations.

## 🛠️ Technology Stack
- **Core Engineering:** Python 3.13, Pandas, SQLAlchemy, Psycopg
- **Infrastructure & Containerization:** Docker, Docker Compose, PostgreSQL, pgAdmin
- **AI & Data Quality:** OpenAI API, Anthropic API
- **Data Visualization:** Matplotlib, Seaborn
- **Dependency Management:** Astral UV

## ⚙️ Architecture & Features
- **High-Performance Ingestion:** Loads massive CSV payloads into PostgreSQL using memory-safe, chunked execution via SQLAlchemy.
- **LLM-Driven Anomaly Detection:** Extracts a 10,000-row sample to automatically audit bad records, identify statistical outliers, and flag logical data anomalies.
- **Automated Audit Reporting:** Programmatically generates a structured Markdown report (`AI_INSIGHTS.md`) and distribution scatter plots prior to full database commits.
- **Containerized Orchestration:** Services (`pgdatabase`, `pgadmin`, `taxi_ingest`) are seamlessly orchestrated over a Docker bridge network with persistent volume mapping.
- **Lightning-Fast Builds:** Replaced standard `venv` and `pip` with **Astral UV**, reducing Python dependency resolution and container build times to seconds.

## 🚀 Quick Start Guide

### 1. Prerequisites
- Docker & Docker Compose installed.
- OpenAI and Anthropic API keys.

### 2. Environment Setup
Create a `.env` file in the root directory to securely store your API keys and database credentials:
```env
OPENAI_API_KEY=your_openai_key_here
ANTHROPIC_API_KEY=your_anthropic_key_here
PG_USER=root
PG_PASSWORD=root
PG_DB=ny_taxi
