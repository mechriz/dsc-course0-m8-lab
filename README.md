# Aviation Accident Safety Analysis
### Consulting Report for Aviation Insurer | NTSB Data 1983–2023

---

## Project Overview

This project analyses **88,889 NTSB aviation accident records (1948–2023)**, filtered to professional-build aircraft from **1983 onwards** (representing a 40-year active-service window). The goal is to identify aircraft makes/models with the lowest rates of:
- Fatal or serious passenger injuries per accident
- Total aircraft destruction per accident

Recommendations are provided separately for **small aircraft** (<20 passengers) and **large commercial aircraft** (≥20 passengers).

---

## Repository Structure

```
├── AviationData.csv                              # Raw NTSB data
├── USState_Codes.csv                             # US state lookup
├── Aviation_Accidents_Cleaning_COMPLETED.ipynb  # Data cleaning pipeline
├── Aviation_Accidents_Data_Analysis_COMPLETED.ipynb  # EDA & recommendations
├── aviation_cleaned.csv                          # Cleaned/derived dataset
└── README.md                                     # This file
```

---

## Methodology

### Data Cleaning
- **Date filter**: Events from 1983 onwards only
- **Aircraft type**: Removed helicopters, gliders, balloons, etc.; retained Airplane category
- **Amateur builds**: Excluded (professional/commercial focus)
- **Safety metrics derived**:
  - `Fatal_Serious_Frac` = (Fatal + Serious injuries) / Total aboard
  - `Destroyed` = Binary flag for total aircraft destruction
  - `Make_Model` = Unique make+model identifier (model names repeat across makes)
- **Make threshold**: Makes with <50 accident records excluded to ensure statistical reliability
- **Model threshold**: Models with <10 records excluded from model-level comparisons

---

## Key Findings

### Large Aircraft (≥20 passengers)

**Top makes by mean injury rate:**

| Make | n (accidents) | Mean Injury Fraction | Mean Destruction Rate |
|------|:---:|:---:|:---:|
| McDonnell Douglas | 176 | 7.9% | 17.2% |
| Embraer | 84 | 12.0% | 14.8% |
| Boeing | 794 | 12.5% | 19.8% |
| Airbus | 152 | 12.7% | 25.9% |

**Top recommended models:**

| Model | n | Mean Injury Frac | Destruction Rate |
|-------|:---:|:---:|:---:|
| Boeing 717-200 | 10 | 0.20% | 0% |
| Boeing 757-232 | 16 | 0.54% | 0% |
| Boeing 757-222 | 11 | 0.74% | 0% |
| McDonnell Douglas MD-88 | 12 | 0.75% | 0% |
| Embraer EMB-145LR | 13 | 5.3% | 0% |

> Note: All top-10 large models show 0% destruction rates, reflecting modern commercial aviation's strong structural standards.

---

### Small Aircraft (<20 passengers)

**Top makes by mean injury rate:**

| Make | n | Mean Injury Fraction | Mean Destruction Rate |
|------|:---:|:---:|:---:|
| WACO | 137 | 10.3% | 8.8% |
| Maule | 569 | 15.4% | 9.3% |
| Aviat Aircraft | 77 | 16.2% | 3.9% |
| Cessna | 594 | 37.6% | 31.7% |

**Top recommended models (statistically robust):**

| Model | n | Mean Injury Frac | Destruction Rate |
|-------|:---:|:---:|:---:|
| Schweizer SGS 2-33A | 45 | 3.3% | 4.4% |
| Piper PA-18A 150 | 12 | 0.0% | 0.0% |
| Maule MX-7-235 | 17 | 2.9% | 0.0% |
| Cessna C172 | 12 | 4.2% | 0.0% |
| Air Tractor AT 602 | 13 | 3.8% | 0.0% |

---

## Factor Analysis

### Factor 1: Weather Condition (VMC vs IMC)

![Weather Factor](fig7_weather.png)

| Condition | n | Mean Injury Fraction | Destruction Rate |
|-----------|:---:|:---:|:---:|
| VMC (Visual) | 56,313 | 22.7% | 17.7% |
| IMC (Instrument) | 4,935 | **66.7%** | **60.0%** |

**Key insight:** IMC conditions produce **3× higher injury rates** and **3.4× higher destruction rates** than VMC. This is the strongest single predictor of accident severity in the dataset. Insurers should heavily weight IMC-operation exposure in premium calculations.

---

### Factor 2: Number of Engines

![Engine Factor](fig8_engines.png)

| Engine Count | n | Mean Injury Fraction | Destruction Rate |
|:---:|:---:|:---:|:---:|
| 1 | 52,076 | 25.2% | 19.4% |
| 2 | 7,849 | **34.1%** | **32.8%** |
| 3 | 175 | 7.0% | 12.8% |
| 4 | 198 | 23.6% | 29.7% |

**Key insight:** Twin-engine general aviation aircraft show paradoxically higher injury and destruction rates than single-engine planes. This is likely a **selection/context bias** — twin-engine GA planes are often operated in more demanding cross-country IFR conditions, and asymmetric engine failure creates difficult loss-of-control scenarios. More engines ≠ inherently safer in GA context.

---

### Factor 3 (Supplemental): Phase of Flight

![Phase Factor](fig9_phase.png)

| Phase | Mean Injury Frac | Notes |
|-------|:---:|---|
| Taxi | 2.3% | Low speed, survivable |
| Landing | 4.1% | Low speed, high frequency |
| Takeoff | 23.1% | Moderate risk |
| Cruise | 35.4% | High altitude/energy |
| Maneuvering | **47.2%** | Loss-of-control risk |
| Climb | **42.9%** | Stall risk |

Landing and Taxi accidents account for a large share of total accidents but carry low fatality rates. Maneuvering phase is the single most dangerous phase per accident.

---

## Visualizations

| Figure | Description |
|--------|-------------|
| `fig1_makes_injury.png` | Top-15 makes by injury rate (small vs large) |
| `fig2_violin_small.png` | Injury distribution violin plot, small makes |
| `fig3_strip_large.png` | Injury distribution strip plot, large makes |
| `fig4_destruction_makes.png` | Top-15 makes by destruction rate |
| `fig5_large_models.png` | Top-20 large models by injury rate |
| `fig6_small_models.png` | Top-20 small models by injury rate |
| `fig7_weather.png` | Weather condition factor analysis |
| `fig8_engines.png` | Engine count factor analysis |
| `fig9_phase.png` | Phase of flight factor analysis |

---

## Tools & Libraries

- **Python 3.9** | pandas · numpy · matplotlib · seaborn
- **Jupyter Notebooks** — modular cleaning and analysis pipeline

