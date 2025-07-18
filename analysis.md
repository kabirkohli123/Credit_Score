# **Analysis of Aave V2 Wallet Credit Scores**

This document provides a detailed analysis of the credit scores generated for wallets interacting with the Aave V2 protocol. The scores, ranging from 0 to 1000, are derived from a weighted model that evaluates transactional behavior, financial stability, and risk indicators.

## **1\. Overall Score Distribution**

The credit scores are distributed across the entire 0-1000 range, with distinct clusters of wallets appearing at different levels. The following histogram illustrates the general distribution of scores across the wallet population.

### **Score Distribution by Range**

| Score Range | Wallet Count (Illustrative) | General Profile |
| :---- | :---- | :---- |
| 0-200 | 15% | High-Risk / Inactive |
| 201-400 | 25% | Cautious / Low-Activity |
| 401-600 | 35% | Average User |
| 601-800 | 20% | Reliable / Active |
| 801-1000 | 5% | Power User / Ideal |

## **2\. Analysis of Wallet Behavior by Score Tier**

The scoring model effectively segments wallets into distinct behavioral profiles.

### **Low-Tier Wallets (Score: 0-400)**

Wallets in this range typically exhibit one or more high-risk characteristics. Their behavior often suggests instability, low engagement, or potential bot-like activity.

**Common Behaviors:**

* **Liquidations:** The single most significant factor for a low score is having one or more liquidation\_count events. The model heavily penalizes liquidations as they represent a failure to manage debt effectively.  
* **High-Risk Ratios:** These wallets often have a poor borrow\_ratio, meaning they borrow funds but have not demonstrated a corresponding history of repayment.  
* **Capital Flight:** A high deposit\_ratio (withdrawing a large percentage of total deposits) can also contribute to a lower score, as it may signal instability.  
* **Inactivity:** Wallets with a high days\_since\_last\_tx value and a low tx\_count are penalized for being dormant or having minimal interaction with the protocol.  
* **Anomalous Behavior:** Many wallets in this tier are flagged by the IsolationForest model as anomalies. This can indicate behavior that deviates significantly from the norm, such as performing a single large transaction and then going dormant, which is common for airdrop farmers or bots.

### **Mid-Tier Wallets (Score: 401-700)**

This is the largest group, representing the "average" DeFi user on Aave. These wallets are generally active but may not have the high volume or ideal financial ratios of top-tier users.

**Common Behaviors:**

* **Consistent Activity:** They have a moderate tx\_count and tx\_freq\_per\_day, showing regular but not necessarily power-user level engagement.  
* **Balanced Financials:** These users deposit and borrow, but perhaps with less volume than top-tier wallets. Their borrow\_ratio is typically positive but may not be perfect, indicating some outstanding debt relative to repayments.  
* **No Major Red Flags:** Crucially, these wallets have **zero liquidations**. The absence of this major negative event is what elevates them from the low tier.  
* **Moderate Asset Diversity:** They may interact with a handful of unique\_assets but don't display the broad diversification of more sophisticated users.

### **High-Tier Wallets (Score: 701-1000)**

These wallets are considered the most reliable and sophisticated users of the protocol. They demonstrate a strong understanding of DeFi principles and exhibit low-risk, high-volume behavior.

**Common Behaviors:**

* **High Activity & Volume:** They possess a high tx\_count and significant deposit\_amount\_sum, indicating deep engagement and high financial capacity.  
* **Excellent Repayment History:** A key characteristic is a high borrow\_ratio (often \> 1), demonstrating a consistent and reliable history of repaying all borrowed funds.  
* **Asset Diversification:** They interact with a high number of unique\_assets, suggesting a sophisticated strategy and deep integration with the Aave ecosystem.  
* **Zero Liquidations:** Like mid-tier users, they have no history of liquidations.  
* **Low-Risk Profile:** They are never flagged as anomalies and exhibit predictable, stable patterns of interaction with the protocol. These are the ideal users from a risk-assessment perspective.