# Tasty Bite: Java Desktop Restaurant Application

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
