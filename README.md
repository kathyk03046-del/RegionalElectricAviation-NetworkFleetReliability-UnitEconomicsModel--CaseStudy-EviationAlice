# Regional Electric Aviation: End-to-End Fleet Intelligence & ROI Optimizer
Case Study: Eviation Alice Propulsion & Battery Telemetry

**Bridging 1Hz Physics Telemetry with Executive Financial Strategy for Electric Aviation.**

## Executive Summary
This project engineers a high-fidelity decision framework for the Eviation Alice (all-electric aircraft) fleet. By synthesizing 1Hz propulsion telemetry with strategic unit economics, I developed an analytical pipeline that predicts battery depletion with 96.1% accuracy and optimizes mission dispatching under an 8,000 kWh regional energy ceiling. The framework identifies the 750 lbs payload breakeven floor and quantifies a $1.01M annual profit lift achievable through algorithmic firmware optimization.

## Technical Architecture
1. High-Fidelity Physics Simulation **(The Digital Twin)**: Built in Python to model non-linear energy drain across three flight phases (Climb, Cruise, Landing), incorporating payload mass and thermal impedance.
2. Automated ETL & Signal Hygiene: Processed 442,297 rows of raw 1Hz data, implementing 98% time-domain **downsampling** to 1-minute intervals for analytical scalability.
3. Predictive Intelligence: Iterated from a linear baseline to a 2nd-degree **Polynomial ML model**, capturing complex thermal-loading interactions to define the fleet's safe operational envelope.
4. Operational ROI Engine: Exported ML weights into a standalone **Excel-based Solver** to maximize fleet EBITDA under specific energy and safety constraints using Linear Programming (Simplex LP).

## Key Findings
* The 750 lbs Profit Floor: Under a base-case scenario ($2.25/lb rate | $0.18/kWh cost), missions under 750 lbs are programmatically flagged as non-viable, resulting in a -20% EBITDA margin due to fixed operational drag.
* Firmware Version Proritization: Firmware v2.2 delivers a 2-4% EBITDA lift over v2.1. Scaling this optimization across a 10-aircraft fleet yields a projected $1.01M annual profit increase.
* Infrastructure Sensitivity: Linking results to my [AI-Grid_Audit] project, I identified that a $0.10/kWh hike in grid prices shifts the breakeven floor by +150 lbs, necessitating a pivot to high-density cargo during peak grid stress.


<img width="1488" height="972" alt="image" src="https://github.com/user-attachments/assets/619cbe7f-25d9-42e1-a201-4fcb752326cc" />


## Tech Stack & Skills
*   **Languages**: `Python`, `Pandas`, `Scikit-learn`, `Glob`
*   **Modeling**: `Polynomial Regression`, `MLOps`, `Object-Oriented Programming (OOP)
*   **Optimization**: `Operations Research (Linear Programming)`, Excel Solver`
*   **Visualization**: `Tableau`,`Matplotlib`

## Assumptions & Limitations:
1. Deterministic Phase Transitions: Flight phases (Climb/Cruise/Landing) are currently triggered by fixed timestamps. In a production environment, these should be driven by Dynamic Flight Envelope telemetry (e.g., real-time altitude rate and air-speed sensors).
2. Idealized Atmospheric Conditions: The simulation assumes a Zero-wind environment. In real-world operations, headwind/tailwind vectors significantly impact the "Energy-to-Destination" ratio, which is currently an unaccounted variance in this model.
3. Static Asset Health (SoH): All simulations assume a 100% State of Health (SoH). The model neglects the long-term impact of Battery Chemical Aging and internal resistance build-up, which would progressively shrink the operational range over time.
4. Piecewise Linearity: While the Polynomial model captures overall trends, the underlying physics engine "smooths over" the extreme non-linear thermal spikes that occur during high-current discharge events (e.g., initial rotation and climb-out).
  
### Logics
#### Enforcing Physical Boundary Locks and Phase-Load Modeling
```python
    # Segmented Power Draw based on Flight Phase
    if second < 450:       phase, phase_load = 'CLIMB', 0.025
    elif second < 3800:    phase, phase_load = 'CRUISE', 0.018
    else:                  phase, phase_load = 'LANDING', 0.008

    # Integration of Payload and Thermal surcharge
    drain = phase_load + payload_factor + temp_factor
    current_soc -= drain
    
    # Physics Guardrail: Enforcing a non-negative energy boundary
    if current_soc <= 0:
        current_soc = 0
        break
```

#### OOP Fleet Management Interface
```python
class AliceFleetManager:    
    def __init__(self, trained_model):
        self.model = trained_model
        self.safety_limit = 20.0  # FAA VFR Mandate

    def run_safety_audit(self, payload, temp, planned_min=60):
        # Using the Polynomial Pipeline
        pred_rate = self.model.predict(input_df)[0]
        final_soc = 100 - (pred_rate * planned_min)
        
        return {
            "status": "CLEARED" if final_soc >= self.safety_limit else "REJECTED",
            "projected_soc": round(final_soc, 2),
            "safety_margin": round(final_soc - self.safety_limit, 2)
        }
```

## Decision Support Assets
### **1. The Safety Envelope (Python Audit)**
#### Finding: to identify exactly when a mission profile violates the 20% SoC safety mandate.
<img width="997" height="488" alt="image" src="https://github.com/user-attachments/assets/f322dff3-731f-403b-b04c-d211753e7076" />

### **2. The Profitability Frontier (Tableau Simulator)**
#### Interaction: Change cargo rate & grid rate to demonstrates the fleet's financial sensitivity to infrastructure volatility.
<img width="1133" height="150" alt="image" src="https://github.com/user-attachments/assets/888897a7-f954-4b54-984e-6ecc390d84f4" />

### **3. The Mission ROI Optimizer (Excel Tool)**
#### Delivery: A tool for non-technical stakeholders to perform mission-level due diligence.
<img width="614" height="550" alt="image" src="https://github.com/user-attachments/assets/1a777f18-5ed9-4315-87bf-e6dab7ad8c98" />

### **4. The Fleet ROI Optimizer (Excel Tool)**
#### Delivery: A tool for non-technical stakeholders to maximize Fleet EBITDA by prioritizing missions with the highest Profit-per-kWh efficiency.
<img width="997" height="298" alt="image" src="https://github.com/user-attachments/assets/805e2080-9bce-41b6-b94e-6b9dd763d3c5" />


## Improvement/Next steps:
**Segmented High-Fidelity Modeling:** Transition from a universal regression model to Phase-Specific ML Models (Climb-specific vs. Cruise-specific). This would allow for even higher precision in the high-stress take-off phase, where energy consumption is most volatile.

**Stochastic Environmental Integration:** Incorporate real-time Wind-Vector Telemetry to quantify the impact of drag on safety reserves, moving the model toward a "Weather-Informed" dispatching system.

**Financial Integrity Loop:** Link Python-detected SoC anomalies directly to the Excel NPV model to calculate the Total Cost of Ownership (TCO) impact of premature battery degradation caused by frequent high-load missions.

**Hardware-in-the-Loop (HIL):** Scale the current AliceFleetManager class to accept real-time sensor streams, transforming this static audit tool into a live Fleet Telemetry Monitoring System.
