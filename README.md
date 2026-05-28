# Wood Supply Chain System Dynamics Simulation
## Bachelor Thesis — TH Rosenheim 

**Student:** Jon Gurakuqi | Matriculation No. 1014404
**Course of Study:** E-Commerce — Bachelor, 8th Semester
**First Examiner:** Prof. Dr. Holly Ott
**Second Examiner:** Prof. Dr. Sebastian Feger
**Collaboration:** Abdul (AnyLogic Implementation)
**Institution:** TH Rosenheim — Campus Chiemgau | 2026

---

## Overview

This repository contains the modelling work, data sources, and literature framework for a **System Dynamics (SD) simulation of the Bavarian wood supply chain**, with a focus on sawmill dynamics.

The thesis is structured as a two-part collaborative project:

| Role | Contributor | Deliverable |
|---|---|---|
| Literature review + model framework | Jon Gurakuqi | MFA, SD variable map, theoretical framework, scenario design |
| AnyLogic implementation | Abdul | Working SD simulation model in AnyLogic |

The simulation will be built in **AnyLogic** using stocks, flows, and feedback loops derived from the literature review and Material Flow Analysis (MFA) conducted in this thesis.

---

## Thesis Focus

**Central question:** How do disruptions propagate through the Bavarian wood supply chain — specifically at the sawmill stage — and how can a System Dynamics model simulate inventory accumulation, demand response, and recovery dynamics?

**Geographic scope:** Bavaria (Freistaat Bayern), Germany
**Supply chain focus:** Forest → Sawmill → Processed timber → Market
**Primary node:** Sawmills — capacity constraints, input disruption, output volatility

---

## Repository Structure

```
/
├── README.md                        ← This file
├── /mfa                             ← Material Flow Analysis
│   ├── mfa_bavaria_wood_flows.xlsx  ← Raw data tables (harvest, sawmill, market)
│   ├── mfa_flow_diagram.pdf         ← Visual MFA flow diagram
│   └── data_sources.md             ← Source documentation for all data
├── /literature                      ← Key papers and framework
│   ├── paper1_dellas_2022.pdf       ← AnyLogic SD pandemic tutorial (WSC)
│   ├── paper2_sinha_2020.pdf        ← SD supply chain disruption framework
│   ├── paper3_franco_2021.pdf       ← Lumber super disruption case (JSCM)
│   └── literature_notes.md         ← Summary and cross-paper synthesis
├── /sd_model_framework              ← SD model design (pre-AnyLogic)
│   ├── variable_map.md             ← Stocks, flows, feedback loops defined
│   ├── causal_loop_diagram.pdf     ← Hand-drawn or Vensim CLD
│   └── scenarios.md               ← Defined test scenarios
├── /presentations                   ← Presentation files
│   └── SD_Research_Presentation.pptx
└── /exposé
    └── Expose_BachelorThesis_Gurakuqi.docx
```

---

## Material Flow Analysis (MFA)

The MFA maps the physical flow of wood through the Bavarian supply chain and provides the empirical backbone for all SD model variables.

### Four-stage flow structure

```
[FOREST STOCK]
      |
      | Harvest Rate (Efm/year)
      ↓
[SAWMILL INPUT]
      |
      | Processing Rate (m³ sawn / year)
      ↓
[SAWN TIMBER INVENTORY]
      |
      | Demand Rate / Market Flow
      ↓
[MARKET OUTPUT]
    /   \
Domestic  Export
```

### Key data points (Bavaria)

| Variable | Value | Year | Source |
|---|---|---|---|
| Total timber harvest | 20.7 million Efm | 2024 | LWF Bayern |
| 10-year average harvest | 18.6 million Efm | 2014-2023 | LWF Bayern |
| Damage wood share | 52% of total harvest | 2024 | LWF Bayern |
| Bayern share of German harvest | ~33% | 2023 | Destatis |
| Softwood (spruce/pine) share | ~72% | 2023 | LWF Bayern |

---

## Data Sources

All data is publicly available. The following sources are used for the MFA and model calibration.

### Primary sources

| Source | Data | URL |
|---|---|---|
| **LWF Bayern** (Bayerische Landesanstalt für Wald und Forstwirtschaft) | Annual timber harvest by species, ownership, damage cause (1999-present) | lwf.bayern.de/forsttechnik-holz/holzmarkt |
| **GENESIS-Online Bayern** (Bayerisches Landesamt für Statistik) | Official Bavarian statistical database — Holzeinschlag tables, free download | statistik.bayern.de |
| **Destatis** (Statistisches Bundesamt) | Federal timber harvest by state, GENESIS-Online national database | destatis.de → Wald und Holz |
| **Bayerische Staatsforsten (BaySF)** | State forest harvest volumes, annual statistics | baysf.de → Publikationen → Statistikband |
| **LWF Holzmarktbericht** | Timber prices, market conditions, quarterly reports | lwf.bayern.de/forsttechnik-holz/holzmarkt |

### Secondary sources

| Source | Data | URL |
|---|---|---|
| **Cluster-Studie Forst und Holz Bayern** | Wood material flow structure — raw timber to product distribution | Bayerisches Staatsministerium (2021) |
| **BMEL Holzmarktbericht** | National timber prices, sawmill production, export volumes | bmel.de |
| **Eurostat** | EU-level sawnwood production and trade data | ec.europa.eu/eurostat |
| **ScienceDirect — Dynamic MFA Germany (1991-2020)** | Comprehensive wood flow analysis for calibration context | doi.org/10.1016/j.resconrec.2023.107034 |

---

## SD Model Framework

The simulation model is built on three core SD concepts, grounded in the three key papers.

### Stocks (Level Variables)

| Stock | Description | Unit | Source |
|---|---|---|---|
| Forest Timber Stock | Standing timber volume available for harvest | Million m³ | LWF / BWI |
| Sawmill Input Inventory | Raw timber held at or en route to sawmills | Thousand Efm | LWF / BaySF |
| Sawn Timber Inventory (It) | Finished sawn timber awaiting sale | Thousand m³ | Cluster Studie |
| Backlog (Bt) | Unfulfilled market orders | Thousand m³ | Sinha et al. (2020) |

### Flows (Rate Variables)

| Flow | Description | Unit | Source |
|---|---|---|---|
| Harvest Rate | Timber extracted from forest per period | Efm / year | LWF |
| Processing Rate (∂Qt) | Sawmill throughput — raw input to sawn output | m³ / year | Cluster Studie |
| Demand Rate | Market orders placed per period | m³ / year | BMEL / Eurostat |
| Supply Rate (∂St) | Sawn timber shipped to market | m³ / year | Sinha et al. (2020) |
| Forest Growth Rate | Annual increment of forest stock | m³ / year | BWI4 / LWF |

### Feedback Loops

| Loop | Type | Description |
|---|---|---|
| Harvest → Inventory → Price → Harvest | Balancing (−) | High inventory signals lower prices, reduces harvest incentive |
| Damage event → Forced harvest → Inventory glut | Reinforcing (+) | Bark beetle / storm damage forces excess harvest, collapses price |
| Backlog → Production ramp-up → Lead time | Balancing (−) | Unfulfilled orders drive sawmill capacity increase, subject to delay |
| Sawmill capacity constraint | Balancing (−) | Maximum throughput limits production rate regardless of inventory |

### Force Majeure Factor (ff)

Following Sinha et al. (2020): ff takes a value between 0 (complete stoppage) and 1 (normal activity). Applied to model external disruption scenarios at the sawmill stage.

---

## Test Scenarios

To be confirmed with Prof. Dr. Ott after MFA review.

| Scenario | Shock Type | ff Value | Duration |
|---|---|---|---|
| Baseline | No disruption | 1.0 | Full run |
| Bark beetle outbreak | Supply shock — forced harvest surge | 0.6 - 0.8 | 2-3 years |
| COVID-19 demand collapse | Demand shock — construction halt | 0.4 - 0.6 | 6-12 months |
| Energy price shock | Cost increase — logistics disruption | 0.7 - 0.9 | 12-24 months |
| Sawmill capacity reduction | Processing constraint | 0.5 | 12 months |

---

## Key Literature

| Paper | Authors | Year | Key Contribution |
|---|---|---|---|
| A Tutorial on How to Set Up a System Dynamics Simulation on the Example of COVID-19 Pandemic | Dellas, Ismail, Ehm, Hartwick (Infineon) | 2022 | Step-by-step AnyLogic SD model — same software as Abdul's implementation |
| The Supply Chain Disruption Framework Post COVID-19: A System Dynamics Model | Sinha, Bagodi & Dey | 2020 | LARG framework, force majeure factor, inventory & backlog as level variables |
| Super Disruptions and Supply Chain Crises: The Case of Lumber | Franco | 2021 | Lumber/wood supply chain direct case — 4-criteria super disruption framework |
| Business Dynamics | Sterman, J.D. | 2000 | Foundational SD textbook — stocks, flows, feedback loops methodology |

---

## Theoretical Framework

Four theories ground the model design:

1. **Buffer Stock Theory** — Arrow, Harris & Marschak (1951): firms hold excess inventory as a hedge against supply and demand uncertainty. Applied to sawmill inventory accumulation under disruption.

2. **System Dynamics Methodology** — Sterman (2000): stocks, flows, and feedback loops model complex system behaviour over time.

3. **LARG Framework** — Cabral et al. (2012) via Sinha et al. (2020): Lean-Agile-Resilient-Green supply chain design — applied to identify vulnerability points in the Bavarian wood SC.

4. **Super Disruption Classification** — Ivanov (2021) via Franco (2021): distinguishes instantaneous from long-lasting supply chain crises across four criteria — impact, scope, recovery, timing.

---

## Meeting Log

| Date | Format | Attendees | Key Decisions |
|---|---|---|---|
| TBD | Online (Zoom) | Jon, Abdul, Prof. Ott | MFA review and scenario confirmation |
| TBD | On-site Rosenheim | Jon, Abdul, Prof. Ott | Model structure sign-off |

---

## Notes

- Simulation development is **Abdul's deliverable** — Jon defines the model framework, Abdul implements in AnyLogic
- MFA must be reviewed and approved by Prof. Dr. Ott **before** scenario testing begins
- All data is sourced from publicly available official statistics — no proprietary data required
- Model complexity principle: *"As simple as possible — but as complex as needed"* (Sterman, 2000 via Dellas et al., 2022)

---

*Last updated: May 2026 — Jon Gurakuqi, TH Rosenheim*
