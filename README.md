# GridGuard AI

**An LLM-Augmented Digital Twin Framework for Resilient Nuclear-Renewable Micro Energy Grids**

Cognitive Innovation Competition 2026 | Challenge 2: AI for Resilient Interconnected Infrastructures
Ontario Tech University, Faculty of Engineering & Applied Science

**Team CI Innovation:** Omar Islam, Justin Tran, Shahin Shunbuli, Adam Salih

---

## Overview

GridGuard AI is a framework for improving the resilience of interconnected nuclear-renewable micro energy grids. It combines a real-time **digital twin** of the grid with a three-part AI layer (LSTM anomaly detection, a DQN reinforcement learning agent, and an LLM semantic engine) that detects problems early, responds automatically, and explains its reasoning to human operators in plain English.

The physical model mirrors Ontario's energy transition: a campus-scale micro grid with a Small Modular Reactor (SMR) analog, solar, wind, and battery storage.

## Why It Matters

Ontario is adding SMRs, solar, wind, and EV load at the same time. Conventional SCADA/EMS platforms struggle with the interdependencies of these hybrid grids under extreme weather, equipment failures, and demand surges. GridGuard AI aims to provide intelligent, autonomous, and *interpretable* resilience management.

## System Architecture

| Layer | Component | Role |
|-------|-----------|------|
| 1 | **Digital Twin** | Simulates the physical grid every 15 minutes (pandapower + SimPy) |
| 2 | **AI Intelligence** | LSTM anomaly detection, DQN reconfiguration agent (<1 s response) |
| 3 | **LLM Semantic Engine** | Claude API produces root-cause analysis, ranked actions, and risk assessment (3–5 s) |
| UI | **Dashboard** | Live telemetry, alerts, AI recommendations, and resilience metrics |

**Dual-speed design:** the RL agent provides a fast automated response, while the LLM provides interpretable reasoning for operators. All recommendations are surfaced to operators with override capability (human-in-the-loop).

### Simulated Grid (5 nodes)

| Node | Component | Specification |
|------|-----------|---------------|
| 1 | SMR analog | 500 kW baseload |
| 2 | Solar PV | 250 kW peak (NREL irradiance data, Oshawa) |
| 3 | Wind turbine | 150 kW rated (Toronto Pearson wind data) |
| 4 | Battery (BESS) | 200 kWh, 100 kW charge/discharge |
| 5 | Load bus | Residential/commercial blend, critical and non-critical loads |

## Resilience Framework

Resilience is defined across four capacities: **Absorb, Adapt, Recover, Predict**, and quantified with four metrics:

| Metric | Description | Target |
|--------|-------------|--------|
| **RTI**: Recovery Time Index | Time to restore 95% of critical load | < 30 min |
| **ECI**: Energy Continuity Index | Energy delivered ÷ energy demanded | > 0.85 |
| **LSR**: Load Satisfaction Rate | % of total load met per timestep | > 90% |
| **VSM**: Voltage Stability Margin | Minimum per-unit bus voltage | > 0.92 p.u. |

These feed a composite **Resilience Loss Index (RLI)**, where lower is better (target < 0.15).

## Test Scenarios

1. **S1 – Ice Storm:** solar drops to ~0, wind curtailed, heating demand +40%
2. **S2 – Equipment Failure:** BESS inverter fault plus SMR derated to 70%
3. **S3 – EV Demand Surge:** 50 EVs fast-charging, demand +60% on a low-wind evening

## Projected Results

> Results below are the **expected/projected** figures from the technical report (10 Monte Carlo runs per scenario), comparing the no-AI baseline against full GridGuard AI.

| Scenario | System | RTI (h) | ECI | LSR (%) | VSM (p.u.) | RLI |
|----------|--------|---------|-----|---------|------------|-----|
| S1: Ice Storm | Baseline | 3.8 | 0.61 | 72.4 | 0.88 | 0.31 |
| | **GridGuard AI** | **1.1** | **0.91** | **94.7** | **0.95** | **0.08** |
| S2: Equipment Fault | Baseline | 4.2 | 0.73 | 81.1 | 0.91 | 0.22 |
| | **GridGuard AI** | **1.9** | **0.88** | **93.2** | **0.94** | **0.11** |
| S3: EV Surge | Baseline | N/A | 0.79 | 87.4 | 0.90 | 0.19 |
| | **GridGuard AI** | N/A | **0.97** | **99.2** | **0.97** | **0.03** |

## Live Dashboard Demo

An interactive dashboard demonstrating the system is included in this repo:

```
gridguard_ai_live_dashboard.html
```

**To run it:** download the file and open it in any modern web browser.

**Demo flow:**
1. Select a scenario (Ice Storm, Equipment Failure, or EV Surge)
2. The digital twin injects the disruption and telemetry updates live
3. The LSTM detector raises an alert within 1–2 timesteps
4. The RL agent executes automated reconfiguration actions
5. The LLM engine writes a situation report with recommended operator actions
6. Resilience metrics (RTI, ECI, LSR, VSM) update and the recovery curve is plotted

## Technology Stack

| Component | Technology |
|-----------|------------|
| Digital twin | Python, pandapower, SimPy |
| LSTM detector | PyTorch, scikit-learn |
| DQN agent | Stable-Baselines3, Gym |
| LLM engine | Anthropic Claude API |
| Dashboard | React, Recharts, Tailwind CSS |
| Backend | FastAPI |
| Storage | SQLite / JSON |

## Planned Repository Structure

```
gridguard-ai/
├── README.md
├── gridguard_ai_live_dashboard.html
├── docs/
│   ├── CI_Innovation_GridGuard_AI_Technical_Report.pdf
│   └── GridGuard_AI_Presentation.pdf
├── simulation/     # digital twin core (grid_twin.py, scenarios.py, metrics.py)
├── ai/             # lstm_anomaly.py, dqn_agent.py, llm_engine.py
├── api/            # FastAPI backend
├── frontend/       # React dashboard
└── data/           # datasets, grid parameters, scenario configs
```

## Limitations & Future Work

- Simplified impedance models; future work could use full EMTP simulation
- API-based LLM latency (3–5 s); on-device small language models could reduce this
- Scope limited to the energy grid; future extension to water and communications infrastructure
- Physical validation on Ontario Tech's campus microgrid is a key next step

## Ethics & Governance

GridGuard AI keeps a human in the loop: all RL and LLM recommendations are presented to operators before execution, with override capability, in line with emerging AI governance for critical infrastructure.

## Documentation

- 📄 [Technical Report](docs/CI_Innovation_GridGuard_AI_Technical_Report.pdf)
- 📊 [Presentation Slides](docs/GridGuard_AI_Presentation.pdf)

## Acknowledgements

Inspired by the research of Dr. Hossam Gaber at Ontario Tech University on resilient interconnected micro energy grids, nuclear-renewable hybrid systems, and LLM-based energy semantic networks.

## License

Add a license here (e.g., MIT) or remove this section.
