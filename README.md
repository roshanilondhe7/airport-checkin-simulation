# ✈️ Airport Check-In & Security Operations — Discrete-Event Simulation

**Module:** BBM115 — Simulation for Managerial Decision Making  
**Programme:** MSc Business Analytics, Aston University  
**Tool:** Simul8 (Educational Licence) + Microsoft Excel  

---

## 📋 Project Overview

This project simulates a hypothetical airport check-in and security system using **Discrete-Event Simulation (DES)** to evaluate operational performance and test improvement scenarios.

The central question: *does the as-is system meet the manager's target of an average Regular Check-in queue wait time of ≤15 minutes — and if not, can a Fast Track lane fix it?*

The short answers: **No** and **Yes, but with a catch.**

---

## 🏗️ System Description

Passengers arrive exponentially (mean inter-arrival = 60 seconds) and are routed probabilistically:

- **25%** → Self Check-in (5 kiosks, Triangular: 10/15/20 min)
- **75%** → Regular Check-in (8 agents / 10 counters, Triangular: 10/12/15 min)

All passengers then proceed to:

- **Security Screening** (10 X-ray machines, Exponential mean = 10 min)

### Simulation Parameters

| Process | Distribution | Parameters | Resources |
|---|---|---|---|
| Passenger Arrivals | Exponential | Mean = 60 seconds | — |
| Self Check-in Service | Triangular | Min=10, Mode=15, Max=20 min | 5 Kiosks |
| Manual Check-in Service | Triangular | Min=10, Mode=12, Max=15 min | 8 Agents / 10 Counters |
| Security Screening | Exponential | Mean = 10 min | 10 X-ray Machines |
| Routing Split | Fixed probability | 25% Self / 75% Manual | — |

---

## 🔬 Methodology

### Warm-Up Period (Welch's Method)
The system is modelled as a **non-terminating steady-state simulation**. Ten pilot replications were run, and the moving average (window w=5) of time-in-system was plotted at 60-minute intervals. The series stabilised at **t = 420 minutes**, which was set as the warm-up period and excluded from all output statistics.

### Replications
Each scenario was run for **10 replications × 1 month (43,200 minutes)**, with warm-up excluded from reported KPIs.

### Statistical Comparison
Scenarios were compared using **95% Paired Confidence Intervals** (t-distribution, 9 degrees of freedom), matching replications across scenarios to reduce between-replication variance.

---

## 📊 Results

### Scenario 1 — As-Is System

| KPI | Range (10 Reps) | Target |
|---|---|---|
| Regular Check-in Queue Time (min) | 1,454 – 2,048 | ≤15 min ❌ |
| Self Check-in Queue Time (min) | 2.2 – 3.3 | Within target ✅ |
| Security Queue Time (min) | 3.0 – 3.9 | Within target ✅ |
| Avg. Time in System (min) | 1,292 – 1,508 | Dominated by check-in delay |
| Regular Check-in Avg. Use | 8.00 | Resource cap hit — 100% utilisation |

The Regular Check-in agents were fully saturated across all replications — a textbook recipe for indefinitely growing queues (Gross & Harris, 1998).

---

### Scenario 2 — Fast Track Lane Added

A conditional Fast Track lane (2 dedicated agents, Triangular: 5/7/10 min) activates when Regular Check-in queue wait exceeds 15 minutes.

| KPI | Scenario 1 | Scenario 2 | Change |
|---|---|---|---|
| Regular Check-in Queue (min) | 1,454 – 2,048 | 3.00 – 3.05 | **99.8% reduction** |
| Self Check-in Queue (min) | 2.2 – 3.3 | 3.27 | Negligible change |
| Security Queue (min) | 3.0 – 3.9 | 98.82 – 98.96 | ⚠️ New bottleneck created |
| Avg. Time in System (min) | 1,292 – 1,508 | 107.89 | **91.7% reduction** |
| Regular Check-in Avg. Use | 8.00 | 6.65 | Congestion reduced |

---

## 💡 Key Insight — Theory of Constraints

The Fast Track lane achieved a **99.8% reduction** in Regular Check-in queue time (95% Paired CI: [965.40, 1063.08] minutes). However, it also caused **Security Queue time to increase from ~4 to ~99 minutes**.

Why? The check-in bottleneck was unintentionally acting as a **flow regulator**, limiting the rate at which passengers reached security. Removing it flooded the X-ray machines.

This is a classic **Theory of Constraints** (Goldratt, 1990) effect: eliminating one bottleneck does not improve overall system performance — it relocates the constraint downstream.

> *"You can't optimise queues in isolation."*

---

## 📈 Statistical Validation

**CI 1 — Regular Check-in Queue Time**
```
d̄ = 1014.24 min  |  SD = 68.34  |  n = 10
95% CI: [965.40, 1063.08]  →  statistically significant improvement ✅
```

**CI 2 — Security Queue Time**
```
d̄ = -94.84 min  |  95% CI: entirely negative
→  Security deterioration is statistically significant ⚠️
```

---

## 📁 Repository Structure

```
├── README.md                         # This file
├── report/
│   └── BBM115_Simulation_Report.pdf  # Full simulation study report
├── figures/
│   ├── figure1_process_flow.png      # Process Flow Diagram (As-Is)
│   ├── figure2_activity_cycle.png    # Activity Cycle Diagram (As-Is)
│   ├── figure3_simul8_scenario1.png  # Simul8 canvas — Scenario 1
│   └── figure6_simul8_scenario2.png  # Simul8 canvas — Scenario 2
└── analysis/
    └── paired_ci_calculations.xlsx   # Excel workings for CI calculations
```

---

## 📚 References

- Banks et al. (2010) *Discrete-Event System Simulation*, 5th edn. Prentice Hall.
- Goldratt, E.M. (1990) *Theory of Constraints*. North River Press.
- Gross, D. and Harris, C.M. (1998) *Fundamentals of Queueing Theory*, 3rd edn. Wiley.
- IATA (2019) *Airport Development Reference Manual*, 10th edn.
- Law, A.M. (2015) *Simulation Modeling and Analysis*, 5th edn. McGraw-Hill.
- Robinson, S. (2014) *Simulation: The Practice of Model Development and Use*, 2nd edn. Palgrave Macmillan.

---

*MSc Business Analytics — Aston University, Birmingham UK*
