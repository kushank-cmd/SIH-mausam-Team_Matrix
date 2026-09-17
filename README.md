# SIH 2026 Project Repository

## 1. Project Information

*   **Project Title:** Mausam
*   **PS ID:** SIH26076
*   **PS Title:** Development of
personalized homepage for 'Mausam' mobile application
*   **Category:** Software
*   **Theme:** Smart Automation
*   **Team Name:** MatriX
*   **Team ID:** NSUT070

## 2. Problem Statement

Users suffer from alert fatigue and a lack of actionable, localized weather data tailored to their daily routines. Existing applications provide raw meteorological data but fail to explain what that data actually means for users—such as farmers, travelers, and daily commuters—in their specific, hyperlocal context.

## 3. Proposed Solution

MAUSAM provides a personalized, hyperlocal dashboard that dynamically prioritizes weather data (AQI, UV, rainfall, wind, humidity) based on user preferences and location using real-time IMD feeds. It features an AI-powered Weather Life Advisor that converts raw data into actionable, plain-language recommendations using NLP, alongside an AI Personalization Engine that learns user routines to auto-rearrange homepage cards.

## 4. Key Features

* **Adaptive Weather Homepage:** Personalized, hyperlocal dashboard prioritizing data based on preferences, GPS, and real-time IMD feeds.
* **Weather Life Advisor:** Converts raw weather data into actionable, plain-language recommendations using a rule-based advisory and NLP.
* **AI Personalization Engine:** Uses on-device ML to learn user routines and dynamically rank dashboard elements.
* **Activity & Readiness Score:** Generates personalized 0-100 composite scores for outdoor readiness via a weighted scoring model.
* **Routine-Aware Weather:** Maps forecasts to daily routines (commutes, school timings) via geofencing and time-based triggers.
* **Smart Alert Engine & Notifications:** Delivers predictive, context-aware push alerts with debounced notification logic to reduce alert fatigue.
* **Explainable & Inclusive Design:** Multilingual voice guidance using the Bhashini API and low-bandwidth, offline-first accessibility.

## 5. Technology Stack

* **Frontend:** Flutter, Dart, Rive, Riverpod, Shorebird, Dio
* **Backend / API:** Python, FastAPI, Nginx (Load Balancing)
* **Database:** Postgres, Redis, Milvus (Vector DB), TimescaleDB (Timeseries DB)
* **AI/ML:** vLLM, Prophet, MLflow, LangChain, HuggingFace
* **Deployment:** App (Play Store, Apple Store, Indus Appstore), API (AWS, Docker, GitHub Actions)
* **API Services:** Open-Meteo, Bhashini API, Mausam, Firebase Cloud Messaging, Twilio, Coastal, Mistral AI
* **Add-Ons:** Pydantic, PyAudio, Hive, permission_handler, cached_network_image

## 6. Architecture

See `docs/architecture.md` for detailed component specifications.

```text
[ CLIENT LAYER ] (Flutter / Dart Apps)
      |
      v (HTTPS / REST)
[ ROUTING LAYER ] (Nginx Proxy / Gateway)
      |
      v
[ APPLICATION LAYER ] (FastAPI Backend + Identity/Onboarding Engine)
      |
      +-------------------------------------------------+
      |                                                 |
      v                                                 v
[ AI / ML SERVICES ]                              [ DATA STORAGE ]
 - LangChain (Chatbot)                             - PostgreSQL (Profiles)
 - vLLM / Mistral AI (NLP)                         - TimescaleDB (Metrics)
 - Prophet (Forecasting)                           - Milvus (Vector DB)
 - MLflow (Monitoring)                             - Redis (Cache)
      |
      v
[ EXTERNAL INTEGRATIONS ]
 - Weather APIs (Open-Meteo, IMD, Coastal)
 - Bhashini API (Multilingual Voice)
      |
      v
[ OPERATIONS & DEPLOYMENT ]
 - AWS Docker CI/CD (GitHub Actions)
 - Firebase / Twilio (Push & SMS Alerts)
