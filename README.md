# krishi-management-system
A group project for managing agricultural activities and resources using Java, JDBC and MySQL.
Week 1: Project Administration & Abstract Definition

Project Administration

Project Guide: Er. Ram Babu Buri   
JPEG

Target Deployment: Netlify (Frontend) at [https://smart-krishi-digital-kishan.netlify.app/](https://smart-krishi-digital-kishan.netlify.app/) & Render (Dockerized Java Web Service)   
PDF

Repository: [https://github.com/amanraj2205/Java_Project](https://github.com/amanraj2205/Java_Project)

Team Members

Name	Role
Ravina	Team Leader & Tech Lead
Yashvardhan Singhal	Core Backend Developer
Team Member 3	Core Backend Developer
Team Member 4	Frontend Developer
Team Member 5	Frontend Developer & QA Lead
Abstract
This project introduces Smart Krishi, an integrated digital agriculture ecosystem tailored to the socioeconomic and agronomic conditions of Indian farmers. While traditional agricultural apps often rely on static or fabricated data, Smart Krishi operates as a precision decision-support platform delivering strictly verified, rule-based agronomic recommendations. The platform provides 8 functional modules including multi-parameter crop recommendation, real-time meteorological tracking via Open-Meteo, ICAR-compliant fertilizer calculations, and granular farm financial projections.   
PDF
+ 2

The system operates on a distributed client-server architecture orchestrated by a stateless Java 17 Spring Boot 3.3.4 backend. It utilizes MySQL 8.0+ for robust, relational persistence of farmer profiles, multi-plot records, and historical market data. The client-side interface, developed in pure HTML5, CSS3, ES6+, and Bootstrap 5.3, ensures fluid responsiveness across mobile and desktop devices without heavy frontend framework overhead.   
PDF
+ 4

Week 2: User Roles, Architecture & Core Modules

User Roles
Smart Krishi enforces stateless JWT Role-Based Access Control (RBAC) via Spring Security 6.   
PDF
+ 2

Registered Farmer (ROLE_FARMER): Can manage multi-plot farm records, view personalized crop/fertilizer recommendations, and access weather advisories.   
PDF

Platform Administrator (ROLE_ADMIN): System controllers who audit users, publish government welfare schemes, and manually seed mandi prices.   
PDF

Public / Unauthenticated User: Can access public tools like the profit calculator, price prediction, and mandi locators prior to registration.   
PDF

Architecture & Core Modules

Module 1: Farmer Management & Land Profiling: Manages user authentication, secure BCrypt password hashing, and multi-plot agricultural profiles.   
PDF
+ 1

Module 2 & 3: Precision Agronomy & Live Weather: Evaluates 8 soil/climatic parameters for crop recommendation and utilizes Open-Meteo for real-time evapotranspiration irrigation advisories.   
PDF

Module 4 & 5: Input Optimization & Economics: Calculates ICAR-standard nutrient deficits (converted to commercial bags) and computes net profit, ROI, and break-even pricing.   
PDF
+ 1

Module 6 & 7: Forecasting: Generates 6-stage phenological growth calendars and projects short-term APMC commodity prices using moving averages.   
PDF
+ 1

Module 8: Mandi Locator: Uses the Haversine formula and data.gov.in AGMARKNET APIs to find nearby operational APMC markets.   
PDF

Module 9: Administration: Centralized governance for platform telemetry and advisory publication.   
PDF

Database Strategy & NFRs

Database: MySQL 8.0+ with InnoDB storage engine for production; H2 In-Memory relational database operating in MySQL mode for development.   
PDF

Performance: Backend endpoints must respond within 250ms, and frontend page load must complete in <1.5s on a 4G connection.   
PDF

Safety: Strict adherence to ICAR thresholds to prevent soil toxicity, and zero fabricated telemetry data.   
PDF

Week 3: UML Design

[Smart Krishi Class Diagram] - Mapping FarmerController, CropRecommendationService, and MandiLocationRegistry.

[Smart Krishi Architecture] - Mapping the asynchronous JSON Fetch API calls between the Bootstrap frontend and Spring Boot service tier.

Week 4: Database Design & UI Mock-ups

1. Relational Database Schema
The system enforces strict referential integrity across 6 core tables.   
PDF
+ 1

users Table: id (PK), email (UK), password (BCrypt), role.   
PDF

farmer_profiles Table: id (PK), user_id (FK), land_area, soil_type, irrigation_type.   
PDF

farms Table: id (PK), farmer_profile_id (FK), plot_number, area_acres.   
PDF

crop_recommendations Table: Stores inputs (N, P, K, pH, temp) and prescribed crops.   
PDF

market_prices Table: crop_name, mandi, modal_price, trend.   
PDF

advisories Table: title, category, target_crop, details.   
PDF

2. User Interface Mock-ups
Designed an agricultural UI using Bootstrap 5.3 focusing on an Agro Forest Green theme (#2e7d32), Inter typography, and Bootstrap Icons.   
PDF

Week 5: Project Initialization & Identity Engine

Directory Structure

Plaintext
Smart_Krishi_Project/
│
├── frontend/                          # Static Frontend (HTML5, CSS3, ES6+, Bootstrap 5.3)
│   ├── index.html                     # Entry point
│   ├── css/style.css                  # Agro Forest Green Theme definitions
│   ├── js/
│   │   ├── api.js                     # Global Fetch API utilities & JWT injection
│   │   ├── auth.js                    # Login & Registration logic
│   │   └── dashboard.js               # Multi-plot rendering logic
│
└── backend/                           # Spring Boot 3.3.4 (Java 17)
    ├── src/main/java/com/krishi/
    │   ├── config/                    # SecurityConfig.java, JwtTokenFilter.java
    │   ├── controller/                # AuthController, FarmerController, MandiController
    │   ├── dto/                       # Data Transfer Objects
    │   ├── model/                     # JPA Entities (User, Farm, MarketPrice)
    │   ├── repository/                # Spring Data JPA Repositories
    │   └── service/                   # Business Logic & External API Clients
    └── src/test/java/com/krishi/      # JUnit 5 & MockMvc Integration Tests
Member 1 (Ravina): Identity, Profiling & Administration (Modules 1 & 9)

Scope: Engineered the entry point of the platform. Built the secure login system, JWT token generation, and the multi-plot farm management UI. Developed the Admin Dashboard for auditing farmers and posting government advisories.   
PDF
+ 2

Challenges: Ensuring JWTs remained completely stateless required custom Spring Security filter chains. Handled cascading deletes so that removing a user automatically cleared their associated farm plots without breaking foreign key constraints.   
PDF

Completion: A fully secured ROLE_FARMER dashboard, seamless BCrypt authentication, and a working administrative governance portal.   
PDF

Week 6: Agronomy & Telemetry Engines

Member 2 (Yashvardhan Singhal): Precision Crop & Weather Engines (Modules 2 & 3)

Scope: Developed the backend algorithms evaluating N, P, K, pH, rainfall, humidity, and temperature to prescribe crops. Integrated Open-Meteo external APIs via browser GPS geolocation to deliver real-time meteorological data and evapotranspiration models.   
PDF
+ 3

Challenges: The strict "Zero Fake Data" NFR meant handling API failures gracefully. Resolving city names to GPS coordinates required sequential chaining of the Open-Meteo Geocoding API before hitting the Forecast API.   
PDF
+ 1

Completion: A robust agronomic prescription endpoint and a highly accurate smart irrigation advisory system that explicitly outputs "PAUSE IRRIGATION" if rainfall > 5mm.   
PDF

Week 7: Financial & Fertilizer Calculators

Member 3: Input Optimization & Profit ROI Engine (Modules 4 & 5)

Scope: Built the Fertilizer Advisor that calculates exact elemental deficits and converts them into physical commercial bags (Urea, DAP, MOP, SSP). Developed the Profit/ROI engine to aggregate 7 production costs against gross revenue projections.   
PDF
+ 1

Challenges: Translating raw chemical deficits into standard Indian bag sizes (50kg for Urea/DAP, 25kg for Zinc) required complex rounding logic. Safely categorizing soil amendments for pH<6.0 (Lime) versus pH>8.0 (Gypsum) required strict boundary validations.   
PDF
+ 1

Completion: A mathematically verified ICAR-standard fertilizer calculator and a dynamic financial health classifier outputting exact break-even prices.   
PDF
+ 1

Week 8: Calendars & Mandi Locator

Member 4 & Member 5: Timelines, Trends & Geodesic Locator (Modules 6, 7 & 8)

Scope: Built the 6-stage phenological Crop Calendar and the statistical Price Prediction engine using moving averages and linear momentum. Engineered the Nearby Mandi Locator using the Haversine formula to calculate spherical distance, filtering results by user-selected radius (10km-100km), and querying live data.gov.in AGMARKNET rates.   
PDF
+ 2

Challenges: The AGMARKNET API frequently experienced downtime. Implemented a fallback mechanism where the system seamlessly switches from LIVE_GOV_API to VERIFIED_DATABASE_FALLBACK without interrupting the user.   
PDF
+ 1

Completion: A fully functional GPS-enabled market locator, deep-linked to Google Maps navigation, alongside predictive trend analysis.   
PDF

Summary of Module Distribution

Module Group	Primary Focus	Key Deliverable
IAM & Admin	Auth, Profiles, RBAC, Schemes	JWT Security & Governance Dashboard
Agronomy & Weather	Matrix Algorithms, External Telemetry	Open-Meteo Integration & Crop Prescriptions
Economics & Fertilizer	Math Formulas, Commercial Bag logic	ROI Calculator & ICAR Nutrient Engine
Timelines & Mandis	Haversine Formula, Data.gov API	GPS Market Locator & Growth Calendars
Week 9: System Integration & E2E Testing

During Week 9, the team shifted from module development to full system Integration & Testing, utilizing Spring Boot's JUnit 5 framework.   
PDF

Member 1 (Security Integration): Executed Context Load and Admin integration tests, verifying that unauthenticated users could not access /api/farmers/ or /api/admin/ routes.   
PDF
+ 1

Member 2 (API Latency & Fallbacks): Tested WeatherController performance, validating that Open-Meteo queries gracefully timed out after 5,000ms and threw appropriate ResourceNotFoundException (404) errors.   
PDF
+ 1

Member 3 (Agronomic Boundary Audits): Ran tests like testFertilizerAcidicAndAlkalineSoilWarnings to ensure the backend never recommended dangerous chemical loads that could cause soil toxicity.   
PDF
+ 1

Member 4 & 5 (Data Failover & Routing): Validated the AGMARKNET fallback logic (testNearbyMandiPricesJaipur), ensuring the UI transparently displayed the CACHED_GOV_API tag when the live connection failed.   
PDF
+ 1

Overall Testing Result: 16 core test cases executed with 0 failures and 0 errors.   
PDF

Week 10: Final System Evaluation, Deliverables & Project Submission

Project Deliverables & Artifacts

Deliverable	Description
Final Project Report	
Complete SRS v2.0 detailing the 9 modules, NFRs, and RTM. 
PDF
+ 1

Presentation Deck (PPT)	Slide deck covering architecture, Haversine formula usage, and failover states.
Video Demonstration	End-to-end walkthrough of farmer login, live weather fetch, and mandi navigation.
Production Deployment	
Cloud-ready application targeting Netlify (Frontend) and Render (Java Backend). 
PDF

Final Milestones Completed:

Eliminated the legacy Al disease detection dependency to permanently remove third-party computer vision API failures.   
PDF

Finalized Docker configuration for the Spring Boot executable .jar file.   
PDF

Successfully executed individual Demo & Viva evaluations to conclude the 10-week PBL timeline.   
JPEG
