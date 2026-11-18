# 🏏 Rajasthan Royals — SupeRR Selector Season 2
### **IPL 2027 Auction Strategy: Identifying the Most Reliable Players Under Pressure**

[![Live Dashboard](https://img.shields.io/badge/Live%20War%20Room-Dashboard-E11D74?style=for-the-badge&logo=googlechrome&logoColor=white)](https://princepatel2830.github.io/rr-warroom-dashboard/)
[![Case Deck PDF](https://img.shields.io/badge/Strategy%20Deck-12--Slide%20PDF-0C1631?style=for-the-badge&logo=adobeacrobatreader&logoColor=white)](https://princepatel2830.github.io/rr-warroom-dashboard/SupeRR_Selector_S2_Case_Deck.pdf)
[![Matches Audited](https://img.shields.io/badge/Deliveries%20Audited-200%2C999-teal?style=for-the-badge)](https://princepatel2830.github.io/rr-warroom-dashboard/)

---

## 📌 Executive Summary

This repository contains the complete analytical methodology, data pipeline, and interactive scouting dashboard for **Round 1 of Rajasthan Royals' SupeRR Selector Season 2**. 

By auditing **200,999 deliveries** across 867 matches (2022–2026), this project establishes a mathematically grounded framework to quantify player performance under true match leverage and shape Rajasthan Royals' squad ahead of the **IPL 2027 Auction**.

* 🚀 **Interactive Dashboard:** [https://princepatel2830.github.io/rr-warroom-dashboard/](https://princepatel2830.github.io/rr-warroom-dashboard/)
* 📄 **Complete 12-Slide Strategy Deck:** [SupeRR_Selector_S2_Case_Deck.pdf](https://princepatel2830.github.io/rr-warroom-dashboard/SupeRR_Selector_S2_Case_Deck.pdf)

---

## 🔬 Step-by-Step Methodology

### **Step 1: Auditing & Cleaning the War-Room Data (The 3 Hidden Traps)**
1. **The `extras1_runs` Schema Bug:** Discovered that the raw database column `extras1_runs` does *not* record extra runs, but rather an extras type code (1=No-ball, 2=Wide, etc.), causing a ~2.49 run/match drift. Cumulative totals were rebuilt ball-by-ball.
2. **Associate Deflation Filter:** Associate T20Is average 7.79 RPO compared to the IPL's 9.23 RPO (a 1.44 RPO gap). Rankings were isolated to **784 elite matches** (367 IPL + 417 Full-Member T20Is; weighted 1.00x vs 0.75x) to eliminate minnow stat-padding.
3. **Adjusting for +1.37 RPO Era Inflation:** Between IPL 2022 (8.44 RPO) and IPL 2026 (9.81 RPO), baseline scoring surged. Every delivery was normalized against an expected par conditioned on venue, innings, over (1–20), year, and opponent quality.

### **Step 2: Quantifying True Match Leverage (Pressure Index)**
Pressure is defined by win-loss leverage, not arbitrary over bands (chasing 18 off 24 balls with 9 wickets in hand is low pressure; losing 3 wickets in the Powerplay is high pressure).
* **Chase Win-Probability Model:** Trained a logistic regression model on 2022–2024 chases (52,736 balls) and validated forward on 2025–2026 (33,624 balls), achieving **0.9129 out-of-time AUC** and **0.120 Brier score**.
* **Pressure Index (PI):** Formulated from 1-ball swing $|WP(+4r) - WP(-1w)|$ (52%), required-rate gap (33%), and wickets lost (15%), with a `live_gate` to strip out hopeless garbage-time runs.
* **Empirical Validation (PI $\ge 45$):** Run rate drops from 9.05 to 8.55 RPO (-0.50 RPO), dot-ball % rises from 32.8% to 34.9% (+2.08 pp), and boundary % drops by -1.28 pp.

### **Step 3: The RUPS (Runs Under Pressure Score) Metric**
Simply computing $(HP - Ordinary)$ introduces a severe **baseline paradox**—it rewards players who perform poorly in easy overs while penalizing superstars who dominate all situations (e.g., Jasprit Bumrah's +29.3 ordinary baseline).

$$RUPS = 0.70 \times \text{Shrunk HP RV} + 0.15 \times \text{Capped Clutch Delta} + 0.15 \times \text{Match Consistency}$$

* **70% Pressure Run Value:** Runs above expected par per 100 balls, Bayesian-shrunk ($K=350$).
* **15% Capped Clutch Delta:** $(HP - Ord)$, shrunk ($K=220$) and capped at $+12 / -30$.
* **15% Match-Level Consistency:** Proportion of high-pressure matches beating par (shrunk to 55%).
* **Phase Wicket Valuation:** Innings 1 wickets priced via Ridge Regression: **Powerplay (9.50 runs)**, **Middle (6.53 runs)**, and **Death (3.50 runs)**.

### **Step 4: Statistical Rigor & Bootstrap Validation**
* Filtered across 5 multi-season sample gates ($\ge 500$ total balls, $\ge 220$ pressure balls, $\ge 22$ matches, $\ge 200$ IPL balls, active in 2025–26) $\rightarrow$ **57 Batters and 60 Bowlers qualified**.
* **1,000-Draw Match-Clustered Bootstrap:** All Top 20 picks maintain a **92.8%+ probability of positive pressure impact ($P > 0$)**.

---

## 🏆 Top 10 Picks for Rajasthan Royals

### **Top 10 Batters Under Pressure**
| Rk | Batter | 2026 Team | RUPS Score | HP Strike Rate | Pos Match % | Primary Pressure Edge |
|:---|:---|:---:|:---:|:---:|:---:|:---|
| **#1** | **Suryakumar Yadav** | MI | **18.22** | 166.6 (+25.5) | 70.6% | 360° middle-over destroyer (+51.0/100b in middle HP) |
| **#2** | **Yashasvi Jaiswal** | RR | **16.37** | 168.9 (+27.5) | 80.0% | Tour-best 80% consistency; Powerplay ambush |
| **#3** | **Phil Salt** | RCB | **16.17** | 166.1 (+28.1) | 71.4% | Relentless Powerplay strike rate |
| **#4** | **Abhishek Sharma** | SRH | **13.64** | 178.6 (+40.4) | 70.0% | Zero drop-off under intense pressure |
| **#5** | **Virat Kohli** | RCB | **13.33** | 150.6 (+9.4) | 69.0% | High-floor chase anchor (99.5% P>0) |
| **#6** | **Tim David** | RCB | **11.56** | 169.6 (+20.8) | 73.5% | Conquers death overs (190.8 death HP SR) |
| **#7** | **Prabhsimran Singh** | PBKS | **11.49** | 164.6 (+18.8) | 59.1% | +18.8 SR surge in tight games |
| **#8** | **Ryan Rickelton** | MI | **11.04** | 165.7 (+17.8) | 63.2% | Explosive left-hand top-order counter |
| **#9** | **Shreyas Iyer** | PBKS | **10.69** | 148.6 (+7.5) | 55.2% | +29.7/100b across 434 IPL crunch balls |
| **#10** | **Ishan Kishan** | SRH | **9.99** | 145.8 (+9.9) | 62.8% | High-volume pressure accumulator |

### **Top 10 Bowlers Under Pressure**
| Rk | Bowler | 2026 Team | RUPS Score | HP Economy | Pos Match % | Primary Pressure Edge |
|:---|:---|:---:|:---:|:---:|:---:|:---|
| **#1** | **Sunil Narine** | KKR | **21.72** | 6.37 (-2.27) | 69.7% | 6.37 HP Econ (unreal 5.10 in HP death overs) |
| **#2** | **Mitchell Santner** | MI | **17.29** | 6.80 (-1.51) | 66.7% | Dual T20I (+42.7) & IPL (+27.4) lockdown |
| **#3** | **Kuldeep Yadav** | DC | **15.62** | 7.05 (-1.59) | 63.2% | 100.0% positive bootstrap probability |
| **#4** | **Varun Chakravarthy** | KKR | **13.83** | 7.75 (-0.87) | 69.2% | 58 pressure wickets in 58 crunch matches |
| **#5** | **Krunal Pandya** | RCB | **13.10** | 6.93 (-1.50) | 60.0% | Quietly chokes middle overs (6.93 HP Econ) |
| **#6** | **Jasprit Bumrah** | MI | **12.87** | 7.12 (-2.09) | 62.2% | 7.58 RPO across 201 death HP balls (42.6% dot rate) |
| **#7** | **Axar Patel** | DC | **12.80** | 7.33 (-1.23) | 58.3% | Highest pressure workload (964 balls, 49 wickets) |
| **#8** | **Rashid Khan** | GT | **12.74** | 6.82 (-1.06) | 58.9% | 6.82 HP Econ across 68 crunch games |
| **#9** | **Jofra Archer** | RR | **11.79** | 8.24 (-0.96) | 60.7% | Raises level (+30.0 clutch delta over ordinary) |
| **#10** | **Patrick Cummins** | SRH | **11.73** | 7.52 (-1.32) | 69.0% | 69.0% crunch games (+30.8 IPL HP RV) |

---

## 🎯 Rajasthan Royals IPL 2027 Auction Playbook

1. **Lock Untouchable Core:** Retain **Yashasvi Jaiswal** (#2, 16.37 RUPS), **Jofra Archer** (#9, 11.79 RUPS), and teenage sensation **Vaibhav Sooryavanshi** (+19.43 RUPS, 222.6 HP SR across 186 pressure balls — just 34 balls shy of our sample gate!).
2. **Rebuild Overs 7–20 Batting:** Replace **Dhruv Jurel** (-8.60 RUPS) by targeting death hitter **#6 Tim David** (190.8 death SR) or middle anchor **#9 Shreyas Iyer** (backup: **#11 Nicholas Pooran**).
3. **Plug Bowling Leaks:** Release leaking pacers **Tushar Deshpande** (-12.68) and **Sam Curran** (-12.40); target **#2 Mitchell Santner** (6.80 HP Econ) or **#5 Krunal Pandya** (6.93 Econ) + death specialist **#11 Nathan Ellis**.

---

## 💻 Tech Stack
* **Analysis & Modeling:** Python, Pandas, NumPy, Scikit-learn, SciPy
* **Visualization:** Matplotlib, Seaborn, Chart.js, ReportLab
* **Hosting:** GitHub Pages (Instant, zero-latency static SPA)
