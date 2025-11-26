# Wafer Quality Cost Model 📉
> **Quantifying wafer quality risk → Price adjustment guidelines**

## 🚀 Overview
**Wafer Quality Cost Model**은 반도체 웨이퍼의 품질 지표를 통계적 비용 모델로 변환하여, 데이터 기반의 단가 협상 가이드라인을 제시하는 프로젝트입니다.

단순한 불량률 계산을 넘어, **품질(Quality)의 변화가 실제 비용(Cost)에 미치는 영향을 정량화**함으로써 합리적인 **단가 결정(Price Decision)**을 지원합니다.

> **Core Logic:** Quality Metrics ($Q$) $\rightarrow$ Expected Cost ($C$) $\rightarrow$ Price Adjustment ($\Delta P$)

---

## 🔧 Key Features
사용자가 4가지 핵심 품질 특성치만 입력하면, 아래 항목들이 자동으로 계산됩니다.

1.  **Defect Probability:** 결함 수 기반 불량 확률 ($\lambda, \alpha$)
2.  **Flatness Probability:** 평탄도 기반 불량 확률 ($\mu, \sigma$)
3.  **Lot Pass Rate:** 샘플링 검사 통과 확률
4.  **Expected Quality Cost:** 품질 리스크 비용 (기회비용, 폐기, 불량출하, 검사비)
5.  **$\Delta$Price%:** 단가 인상/감액 권고안

---

## 📊 Statistical Models

### 1. Defect Model (Negative Binomial)
웨이퍼 표면의 결함 분포를 모델링합니다.
* **Metric:** $\lambda$ (Defect Density), $\alpha$ (Clustering Factor)
* **Result:** $P(\text{defect fail}) \approx 0.47\%$

### 2. Flatness Model (Normal Distribution)
웨이퍼의 평탄도(TTV)를 정규분포로 가정하여 분석합니다.
* **Metric:** $\mu = 2.11 \mu m$, $\sigma = 0.78 \mu m$ (USL = $3.5 \mu m$)
* **Result:** $P(\text{flatness fail}) \approx 3.79\%$

### 3. Lot Sampling Plan
* **Lot Size:** 25 wafers
* **Sample Size:** 5 wafers (AQL Standard)
* **Model:** Surfscan Error Model applied
* **Result:** Lot Pass Probability $\approx 76.88\%$

---

## 💸 Expected Quality Cost ($E[\text{Cost}]$)
품질 리스크 비용을 네 가지 카테고리로 세분화하여 산출합니다.

| Category | Description |
| :--- | :--- |
| **Opportunity Cost** | 양품을 불량으로 오판정하여 발생하는 폐기 비용 |
| **Scrap Cost** | 실제 불량품 폐기 비용 |
| **Test-escape Cost** | 불량품이 출하되어 발생하는 클레임/배상 비용 |
| **Inspection Cost** | 샘플 검사 운영 비용 |

> **Baseline $E[\text{Cost}]$:** \$28,174.88 per lot

---

## 🧮 Price Adjustment Logic
품질 비용 변화에 따른 합리적 단가 조정율을 계산합니다.

$$\Delta \text{Price}\% = k \times \frac{E[\text{Cost}_i] - E[\text{Cost}_0]}{E[\text{Cost}_0]}$$

* $E[\text{Cost}_0]$: 기준 품질 비용
* $E[\text{Cost}_i]$: 변경된 품질 비용
* $k$: 협상 탄력 계수 (Tier 기반)

### Negotiation Tiers
| Tier | Description | $k$ (Coefficient) |
| :---: | :--- | :---: |
| **1** | Strategic Partner | 0.3 |
| **2** | Standard Supplier | 0.5 |
| **3** | Regional / Specialty | 0.7 |

---

## 📈 Insights & Heatmap Guide


* **Defect Sensitivity:** $\lambda$ 값이 비용 상승의 가장 핵심적인 변수입니다.
* **Flatness Trade-off:** $\mu$와 $\sigma$는 상호 보완 관계이나, $\mu$ 개선이 비용 절감에 더 효과적입니다.
* **Guardrail:** $\lambda \ge 0.06$ 구간은 리스크가 과도하여 **협상 불가(Non-negotiable)** 영역으로 간주합니다.

---

## 🖥️ LLM Price Calculator
자연어 입력을 통해 품질 값을 전달하면, 리스크 및 단가 권고안을 즉시 도출합니다.
> Example: "If defect density increases to 0.05, how much should the price drop?"

---

## 📁 Project Structure
```bash
wafer-cost-model
 ┣ 📂 src          # Core logic & calculation modules
 ┣ 📂 data         # Simulation data & parameters
 ┣ 📂 notebooks    # Analysis & Visualization (Jupyter)
 ┗ README.md      # Project Documentation
```

---
* 단가 계산기 사이트

    https://wafer-app-hupbzlqxpqdabwyckcuqrj.streamlit.app/

