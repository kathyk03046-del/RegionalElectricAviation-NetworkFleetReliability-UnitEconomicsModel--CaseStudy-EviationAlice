# Regional Electric Aviation: Fleet Reliability & Unit Economics Model
**Case Study: Eviation Alice Propulsion & Battery Telemetry**

#Electric Aviation#Aircraft Physics#Fleet Economics

### Executive Summary
The regional electric aviation sector represents a transformative shift in logistics, yet it faces a critical misalignment between stringent certification standards, battery energy density limits, and economic feasibility.

Using the Eviation Alice (Cargo Variant) as a reference, I developed a physics-based simulation generating 450,000 rows of high-fidelity telemetry to map the aircraft's operational limits.Through this digital twin, I identified the FAA 20% energy reserve redline, establishing a hard 60-minute mission cap for safe operations. I then integrated these physical constraints with a financial engine to audit the profitability of each sortie, resulting in an automated Decision Support System (DSS) that balances flight safety with unit economics.

### North Star Metric: Profit per kWh
Unlike jet fuel, which burns away and makes the plane lighter, Alice carries 8,000 lbs of batteries from takeoff to landing - dead weight; every kWh is precious. I structured this project to answer three critical questions：
* **Safety Margin Sensitivity:** Does the official 58-min mission remain safe under a max. 2,500 lbs payload and 45°C extreme heat?
* **Anomaly Detection:** Can we isolate "Energy Burn-Rate" outliers within a 1Hz high-frequency telemetry stream to identify hidden inefficiencies?
* **Operational ROI:** What is the exact financial impact of enforcing the non-negotiable 20% FAA safety reserve by capping flight missions?

### Methodology:
I architected a four-stage modular pipeline to transform raw physical constraints into executive business strategy. 

* **Phase 1:** Developed a 1Hz deterministic simulation in Python to generate 450,000 rows of telemetry.
* **Phase 2:** Engineered a fault-tolerant pipeline using Pandas to aggregate raw data; 98% Time-Domain Downsampling
* **Phase 3:** Trained a Linear Regression model (R-squared=.68) and encapsulated it into an Object-Oriented (OOP) Fleet Manager class.
* **Phase 4:** Integrated ML coefficients into an Excel-based Decision Support System (DSS) using Goal Seek and Solver.
 
#### Limitations:
* Fixed-Time Transitions: Flight phases (Climb/Cruise etc.) are triggered by fixed timestamps rather than dynamic altitude or air-speed.
* Zero-wind environment: Ignoring the energy variance caused by real-world headwinds and tailwinds.
* 100% SoH Health and SoC: All aircraft are simulated with 100% SoH and SoC, eglecting the impact of battery chemical aging on overall range.
* Linearity Constraint: "Smooths over" the non-linear thermal spikes that occur during high-current takeoff and climb.

### Key Findings:
<img width="1414" height="872" alt="image" src="https://github.com/user-attachments/assets/d6c3c169-4234-43fc-b63b-d01e957edd91" />
<img width="1382" height="731" alt="image" src="https://github.com/user-attachments/assets/7847eea0-73a9-4dc7-a5c2-afb237e969d6" />


* **Insight 1:** Missions below 1,000 lbs result in a -37% EBITDA margin due to the fixed $1,200 ground operational drag.
  
* **Insight 2:** Energy costs inflate by 7% when ambient temperatures >40°C, driven by increased BMS cooling demand.
  
* **Insight 3:** A/B testing between firmware versions showed that v2.2 increased avg. net profit per sortie by $453 compared to the v2.1
  
* **Insight 4:** High-density missions deliver a return of $238 per 1kWh consumed, whereas light-load missions yield negative returns.

#### Tech Stack
* Python: Pandas, NumPy, Scikit-Learn, OOP, JOINS
* Excel: Power Query, Scenario Manager, Goal Seek
* Tableau: LOD, Calculated Fields, Dashboard
  
### Improvement/Next steps:
* Transition from universal linear regression to segmented ML models (Climb-specific vs. Cruise-specific) to improve prediction accuracy to > 0.8
* Incorporate real-time wind speed/direction data to quantify the impact of Headwinds on mission safety reserves.
* Link SoC anomalies detected in Python to the Excel, to calculate the NPV of early battery interventions.
  
