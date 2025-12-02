# Wafer Quality Cost Model

> **Quantifying Wafer Quality Risk for Data-Driven Price Adjustment**

## Project Overview

**Wafer Quality Cost Model**은 반도체 웨이퍼의 물리적 품질 지표를 통계적 비용 모델로 변환하여, 데이터 기반의 단가 협상 가이드라인을 제시하는 프로젝트입니다.

기존의 단순 불량률 산출 방식을 넘어, **공정 품질(Quality)의 변동이 실제 비즈니스 비용(Cost)에 미치는 영향을 정량화**함으로써, 공급사와 수요사 간의 합리적인 \*\*단가 의사결정(Price Decision)\*\*을 지원합니다.

> **Core Logic:** Quality Metrics ($Q$) $\rightarrow$ Expected Cost ($C$) $\rightarrow$ Price Adjustment ($\Delta P$)

## Core Functionalities

사용자가 4가지 핵심 품질 특성치를 입력하면, 시스템은 내부 알고리즘을 통해 다음 항목들을 산출합니다.

1.  **Defect Probability:** 결함 밀도 및 클러스터링 기반 불량 확률 ($\lambda, \alpha$)
2.  **Flatness Probability:** 웨이퍼 평탄도(TTV) 기반 불량 확률 ($\mu, \sigma$)
3.  **Lot Pass Rate:** 국제 표준(AQL) 샘플링 검사 통과 확률 시뮬레이션
4.  **Expected Quality Cost:** 품질 리스크에 따른 기대 비용 (기회비용, 폐기, 불량출하, 검사비)
5.  **Price Adjustment ($\Delta P$):** 비용 변동에 따른 단가 인상/인하 권고안 도출

## Statistical Framework

### 1\. Defect Model (Negative Binomial Distribution)

웨이퍼 표면의 결함 분포를 음이항 분포(Negative Binomial)로 모델링하여 공정의 산포를 반영합니다.

  * **Parameters:** $\lambda$ (Defect Density), $\alpha$ (Clustering Factor)
  * **Output:** $P(\text{defect fail})$ Estimation

### 2\. Flatness Model (Normal Distribution)

웨이퍼의 평탄도(TTV, Total Thickness Variation)를 정규분포로 가정하여 USL(Upper Specification Limit) 초과 확률을 분석합니다.

  * **Parameters:** $\mu$ (Mean TTV), $\sigma$ (Standard Deviation)
  * **Constraint:** USL = $3.5 \mu m$
  * **Output:** $P(\text{flatness fail})$ Estimation

### 3\. Lot Sampling Plan

제조 현장의 리스크를 반영하기 위해 Lot 단위 시뮬레이션을 수행합니다.

  * **Lot Size:** 25 wafers
  * **Sample Size:** 5 wafers (Based on AQL Standard)
  * **Methodology:** Surfscan Error Model 적용
  * **Output:** Lot Pass Probability Calculation

## Expected Quality Cost Analysis ($E[\text{Cost}]$)

발생 가능한 품질 리스크 비용을 네 가지 카테고리로 세분화하여 산출합니다.

| Category | Description |
| :--- | :--- |
| **Opportunity Cost** | 양품을 불량으로 오판정(False Positive)하여 발생하는 기회비용 |
| **Scrap Cost** | 실제 불량품 폐기로 인한 손실 비용 |
| **Test-escape Cost** | 불량품이 출하(False Negative)되어 발생하는 배상 및 클레임 비용 |
| **Inspection Cost** | 샘플링 검사 운영 및 장비 가동 비용 |

> **Baseline Metric:** Calculated $E[\text{Cost}]$ per Lot

## Price Adjustment Logic

품질 비용의 증감분을 반영하여 합리적인 단가 조정율을 제안합니다.

$$\Delta \text{Price}\% = k \times \frac{E[\text{Cost}_i] - E[\text{Cost}_0]}{E[\text{Cost}_0]}$$

  * $E[\text{Cost}_0]$: 기준(Baseline) 품질 비용
  * $E[\text{Cost}_i]$: 변경된 공정 조건 하의 품질 비용
  * $k$: 협상 탄력 계수 (Negotiation Coefficient)

### Negotiation Strategy Tiers

공급사와의 관계 및 시장 지위에 따라 탄력 계수($k$)를 차등 적용합니다.

| Tier | Classification | Coefficient ($k$) |
| :---: | :--- | :---: |
| **1** | Strategic Partner | 0.3 |
| **2** | Standard Supplier | 0.5 |
| **3** | Regional / Specialty | 0.7 |

## Key Insights & Guidelines

시뮬레이션 결과에 기반한 주요 분석 포인트는 다음과 같습니다.

  * **Sensitivity Analysis:** 비용 상승에 가장 큰 영향을 미치는 변수는 결함 밀도($\lambda$)로 식별되었습니다.
  * **Quality Trade-off:** $\mu$와 $\sigma$는 상호 보완적이나, 평균값($\mu$)의 개선이 산포($\sigma$) 감소보다 비용 절감 효과가 큽니다.
  * **Risk Threshold:** $\lambda \ge 0.06$ 구간은 품질 비용이 기하급수적으로 증가하여 **협상 불가(Non-negotiable)** 영역으로 정의합니다.

## LLM-based Price Calculator

자연어 처리(NLP) 모듈을 탑재하여, 대화형 인터페이스를 통해 시나리오별 리스크 분석 및 단가 권고안을 즉시 제공합니다.

> **Query Example:** "If defect density increases to 0.05, how much should the price drop?"

## Project Structure

```bash
wafer-cost-model
├── src              # Core logic & calculation modules
├── data             # Simulation data & parameters
├── notebooks        # Analysis & Visualization (Jupyter)
└── README.md        # Project Documentation
```

## Live Demo

웹 기반의 단가 계산기 애플리케이션을 통해 모델을 직접 테스트할 수 있습니다.

  * **Access URL:** [Wafer Cost Model App](https://wafer-app-hupbzlqxpqdabwyckcuqrj.streamlit.app/)

