# Tasty Bite: Precision Agriculture Platform

## week 1

### Project Administration

**Project Guide:** [Insert Guide Name]

* **Research Area:** [Insert Area]
* **Specializations:** Java SE, Swing, JDBC, MySQL

**Team Members**
 Name | Enrollment | Email | Mobile |
| :--- | :--- | :--- | :--- |
| Ravina | 24E1ARCSF30P131 | itsravinasheoran@gmail.com | 9414690286 |
| Shrashti Jain | 24E1ARCSF40P154 | sristhijain05@gmail.com | 8306912157 |
| Yashvardhan Singhal | 24E1ARCSM40P188 | imyash058@gmail.com | 8949119364 |
| Vinay Choudhary | 24E1ARCSM30P184 | vinaychoudharykarwar@gmail.com | 7976136409 |
| Venkatesh Kumawat | 24E1ARCSM30P181 | lakshyakumawat11@gmail.com | 7296957338 |


### Abstract

This project introduces Tasty Bite, a comprehensive Java desktop application engineered to streamline the restaurant ordering process through a dedicated dual-role architecture. Built utilizing Java with Swing for the graphical user interface, JDBC for secure database connectivity, and MySQL for persistent storage, the platform provides a structured environment for end-to-end food service operations.

The system operates by dividing functionality between customer and managerial access flows. For customers, Tasty Bite delivers an intuitive interface to select a restaurant, seamlessly browse menus, configure a shopping cart, place an order, and monitor live order status. Concurrently, the platform equips restaurant managers with a powerful centralized dashboard capable of dynamically building and updating menus. Managers can process incoming active orders through strict lifecycle stages—moving from Pending to Preparing, and finally to Completed or Cancelled—while maintaining access to a filterable order history.

To ensure transactional security and structural integrity, the application implements hashed passwords for all user sessions and strictly pairs every frontend interface screen with a matching backend module. Ultimately, Tasty Bite converges these functionalities into a unified, responsive system that bridges the gap between customer convenience and real-time operational oversight for restaurant management.

## week 2

### User Roles

Smart Krishi enforces strict Role-Based Access Control (RBAC) utilizing Spring Security 6 and stateless JSON Web Tokens (JWT) for secure authentication[cite: 9, 15].

* **Registered Farmer (ROLE_FARMER):** The primary user class. Can manage multi-plot farm records, maximize crop yields, and access personalized recommendations, calculators, and market locators[cite: 13].
* **Platform Administrator (ROLE_ADMIN):** System controllers and agricultural officers with elevated privileges to monitor platform usage, curate verified commodity prices, and post government welfare advisories[cite: 13].
* **Public / Unauthenticated User:** Farmers exploring the platform prior to registration. Permitted to access public crop recommendation tools, live weather, fertilizer advisor, and mandi prices[cite: 13].

### SRS PDF

You can access the [Project SRS Click](#)

### Architecture & Core Modules

The Smart Krishi architecture operates as a modern distributed multi-tier client-server system, with a Spring Boot (Java 17) backend acting as the core application tier and a responsive HTML5/Bootstrap frontend[cite: 9, 12].

### Functional Modules

* **Module 1: Farmer Management & Land Profiling:** Manages account registration, secure login, profile maintenance, and multi-plot farm records[cite: 12].
* **Module 2: Precision Crop Recommendation Engine:** A multi-parameter engine that prescribes suitable crops based on chemical soil composition, environmental conditions, and soil texture[cite: 12].
* **Module 3: Live Meteorological Telemetry & Smart Irrigation:** Provides real-time, location-based atmospheric data and water management guidance using Open-Meteo without simulated values[cite: 12].
* **Module 4: Smart Fertilizer Advisor:** Calculates exact chemical fertilizer quantities and bag conversions aligned with ICAR standard practices[cite: 12].
* **Module 5: Crop Profit & ROI Calculator:** A farm economics calculator providing cost breakdowns, net profit, ROI, and break-even pricing[cite: 12].
* **Module 6: Crop Growth Calendar:** Generates a structured 6-stage phenological growth timeline with watering schedules and advisories[cite: 12].
* **Module 7: Crop Price Trend Prediction Engine:** Calculates short-to-medium term commodity price projections using statistical momentum and historical APMC data[cite: 12].
* **Module 8: Nearby APMC Mandi Market Prices:** A GPS-enabled Haversine mandi locator for discovering nearby markets and verified live commodity prices[cite: 12].
* **Module 9: System Administration & Governance:** Empowers administrators to audit profiles, post market rates, and publish government welfare schemes[cite: 12].

### Persistence Database Strategy

The system utilizes a dual-environment database strategy, routing data persistence based on the deployment target via Spring Data JPA and Hibernate[cite: 12].

**Local/Development (H2 In-Memory)**
* **Usage:** Used for local development, rapid prototyping, and automated testing[cite: 10, 14].
* **Structure:** Operates in MySQL compatibility mode (`MODE=MYSQL`) to ensure schema consistency[cite: 14].

**Production (MySQL 8.0+)**
* **Usage:** Cloud production database hosted securely, utilizing the InnoDB storage engine and utf8mb4 encoding[cite: 10, 14].
* **Integration:** Enforces strict relational schemas, foreign key constraints, and referential integrity across all 7 core tables[cite: 20].

### Non-Functional Requirements

* **Performance:** All backend calculation endpoints respond within 250 milliseconds. External API queries timeout gracefully after 5,000ms, and frontend page load completes under 1.5 seconds[cite: 19].
* **Security:** All secure endpoints require valid Bearer tokens, passwords are salted and hashed using BCrypt, and all database queries execute via parameterized statements to eliminate SQL Injection[cite: 19].
* **Fault Tolerance:** If the primary government AGMARKNET API fails, the platform seamlessly fails over to the verified Smart Krishi database repository without interrupting the user[cite: 19].
* **Safety & Agronomic Integrity:** The fertilizer advisor enforces strict upper thresholds to prevent soil toxicity, and the system strictly prohibits randomized or fake weather/mandi telemetry[cite: 19].

## week 3

### UML & System Architecture

* **Smart Krishi Entity-Relationship (ER) Design**
The system relies on a strictly relational database model with core entities mapped via JPA and Hibernate. The core architecture enforces relationships where `USERS` possess `FARMER_PROFILES`, and those profiles can contain multiple `FARMS`[cite: 20]. Additionally, users receive and store generated `CROP_RECOMMENDATIONS`[cite: 20].

```mermaid
erDiagram
    USERS ||--|| FARMER_PROFILES : "has"
    USERS ||--o{ CROP_RECOMMENDATIONS : "receives"
    FARMER_PROFILES ||--o{ FARMS : "contains"

    USERS {
        bigint id PK
        varchar name
        varchar email UK
        varchar role
    }
    FARMER_PROFILES {
        bigint id PK
        bigint user_id FK
        double land_area
        varchar primary_crop
    }
    FARMS {
        bigint id PK
        bigint farmer_profile_id FK
        double area_acres
        varchar water_source
    }
    CROP_RECOMMENDATIONS {
        bigint id PK
        varchar recommended_crop
        varchar suitable_season
    }
graph TD
    subgraph Client/Presentation Tier
        UI[Web Browsers - HTML5, CSS3, ES6+, Bootstrap 5.3]
    end

    subgraph Application/Service Tier - Spring Boot 3.3.4
        Auth[Farmer & Profile Svc]
        Crop[Crop Recommend Svc]
        Weather[Weather Svc]
        Mandi[Mandi & Geodesic Svc]
    end

    subgraph Persistence Tier
        DB[(MySQL 8.0+ / H2 In-Memory)]
    end

    subgraph External Web Services
        OM[Open-Meteo APIs]
        Gov[data.gov.in AGMARKNET API]
    end

    UI -->|HTTPS / REST JSON| Auth
    UI -->|HTTPS / REST JSON| Crop
    UI -->|HTTPS / REST JSON| Weather
    Auth -->|JDBC / JPA| DB
    Crop -->|JDBC / JPA| DB
    Weather -->|HTTP Requests| OM
    Mandi -->|HTTP Requests| Gov
## week 4: Database Design & UI Architecture

### 1. Database Architecture & Persistence Strategy

Smart Krishi operates on a strictly relational database architecture, utilizing an H2 In-Memory database (in MySQL compatibility mode) for local development and rapid prototyping, and a Cloud MySQL 8.0+ instance with the InnoDB storage engine for production deployments[cite: 9, 14].

**A. Core Relational Database Schema (Identity & Profiles)**

*   **`users`** Table: Core identity, authentication, and role-based access[cite: 21].

    *   `id` ( BIGINT AUTO_INCREMENT , PRIMARY KEY )[cite: 21]
    *   `name` ( VARCHAR(100) , NOT NULL )[cite: 21]
    *   `email` ( VARCHAR(120) , UNIQUE , NOT NULL )[cite: 21]
    *   `phone` ( VARCHAR(20) , NOT NULL )[cite: 21]
    *   `password` ( VARCHAR(255) , BCrypt Hash )[cite: 21]
    *   `role` ( VARCHAR(20) ) — ROLE_FARMER , ROLE_ADMIN[cite: 21]
    *   `created_at` ( TIMESTAMP )[cite: 21]

*   **`farmer_profiles`** Table: Agricultural profiling and demographic metadata[cite: 21].

    *   `id` ( BIGINT AUTO_INCREMENT , PRIMARY KEY )[cite: 21]
    *   `user_id` ( BIGINT , FOREIGN KEY -> users.id , ON DELETE CASCADE )[cite: 21]
    *   `village`, `district`, `state` ( VARCHAR(100) )[cite: 21]
    *   `land_area` ( DOUBLE )[cite: 21]
    *   `soil_type` ( VARCHAR(50) )[cite: 21]
    *   `primary_crop` ( VARCHAR(100) )[cite: 21]

*   **`farms`** Table: Multi-plot farm management records[cite: 15, 21].

    *   `id` ( BIGINT AUTO_INCREMENT , PRIMARY KEY )[cite: 21]
    *   `farmer_profile_id` ( BIGINT , FOREIGN KEY -> farmer_profiles.id , ON DELETE CASCADE )[cite: 22]
    *   `farm_name` ( VARCHAR(100) , NOT NULL )[cite: 22]
    *   `area_acres` ( DOUBLE , NOT NULL )[cite: 22]
    *   `water_source` ( VARCHAR(100) )[cite: 22]

**B. Analytical & External Data Schema (Decision Support)**

*   **`crop_recommendations`** Table: Historical logging of agronomic prescriptions[cite: 16, 22].

    *   `id` ( BIGINT AUTO_INCREMENT , PRIMARY KEY )[cite: 22]
    *   `user_id` ( BIGINT , FOREIGN KEY -> users.id , ON DELETE SET NULL )[cite: 23]
    *   `nitrogen`, `phosphorus`, `potassium`, `ph`, `temperature`, `humidity`, `rainfall` ( DOUBLE , NOT NULL )[cite: 23]
    *   `recommended_crop` ( VARCHAR(100) , NOT NULL )[cite: 23]
    *   `suitable_season` ( VARCHAR(50) )[cite: 23]

*   **`market_prices`** Table: Caching of APMC commodity rates[cite: 23].

    *   `id` ( BIGINT AUTO_INCREMENT , PRIMARY KEY )[cite: 23]
    *   `crop_name` ( VARCHAR(100) , NOT NULL )[cite: 23]
    *   `market_name`, `district`, `state` ( VARCHAR )[cite: 23]
    *   `min_price`, `max_price`, `modal_price` ( DOUBLE , NOT NULL )[cite: 23]
    *   `price_date` ( DATE , NOT NULL )[cite: 23]

*   **`advisories`** Table: Government welfare schemes and agricultural notices[cite: 24].

    *   `id` ( BIGINT AUTO_INCREMENT , PRIMARY KEY )[cite: 24]
    *   `title` ( VARCHAR(255) , NOT NULL )[cite: 24]
    *   `category` ( VARCHAR(50) , NOT NULL ) — e.g., SCHEME, FERTILIZER[cite: 24]
    *   `details` ( TEXT , NOT NULL )[cite: 24]

---

### 2. User Interface (UI) System

The Smart Krishi frontend is designed as a responsive web application communicating via asynchronous JSON REST APIs[cite: 13].

**A. Visual Design & Theming**
*   **Aesthetics:** The UI reflects an agricultural theme utilizing Agro Forest Green (`#2e7d32`) as the primary color, `#4caf50` as a secondary accent, `#f4fbf4` for backgrounds, and `#e0e0e0` for neutral borders[cite: 14].
*   **Typography & Icons:** Features clean, highly readable typography using Google Fonts Inter and vector iconography via Bootstrap Icons 1.11.3[cite: 14].
*   **Responsiveness:** Implements a fluid layout utilizing Bootstrap 5.3 grid classes (`col-12`, `col-md-6`, `col-lg-4`) to adapt seamlessly across screen widths from 320px (mobile) to 1920px (desktop)[cite: 14].

**B. Interactive Elements**
*   **Real-time Feedback:** The interface integrates real-time validation, responsive toast notifications (`UI.showToast()`), loading spinners during asynchronous fetch calls, and modal dialogues for configuration to enhance the farmer experience[cite: 14].
*   **Hardware Integration:** The browser utilizes device GPS/cellular location services (`navigator.geolocation.getCurrentPosition()`) to supply high-precision coordinates for weather telemetry and the geodesic mandi locator[cite: 14].

*(Note: You can add actual screenshots of your Smart Krishi frontend here under this section if you have them, similar to the DevStream mockups.)*
