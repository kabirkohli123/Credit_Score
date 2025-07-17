# Credit_Score


This project implements a **rule-based credit scoring system** for wallets that interact with the Aave V2 protocol. It uses real transaction-level data (deposit, borrow, repay, redeem, and liquidation actions) to assign a **credit score between 0 and 1000** to each wallet.

---

## 📌 Objective

To assess the creditworthiness of wallets based on their historical transaction behaviors in Aave V2 using a transparent scoring logic. This can be useful for identifying reliable vs. risky users in decentralized finance.

---

## 🧠 Scoring Logic

Each wallet is scored using weighted features derived from 5 key transaction types:

| Action             | Feature Used                          | Contribution         |
|-------------------|----------------------------------------|----------------------|
| `deposit`          | Total deposit USD                      | ➕ Positive           |
| `borrow`           | Total borrow USD                       | ➖ Slight Risk        |
| `repay`            | Repay/Borrow ratio, Total repaid USD   | ➕ Very Positive      |
| `redeemunderlying` | Total redeemed USD                     | ➕ Positive           |
| `liquidationcall`  | Number of times liquidated             | ➖ Negative           |

---

## 📊 Credit Score Formula

All features are normalized to `[0, 1]`, and the final score is calculated as:

```python
final_score = (
    0.3 * deposit_score +
    0.25 * repay_score +
    0.15 * redeem_score +
    0.15 * (1 - borrow_risk_score) +
    0.15 * (1 - liquidation_score)
)
````

The final score is then scaled to `[0, 1000]`.

---

## 🐍 How It Works (Jupyter Notebook Flow)

1. **Read JSON Data** – Parse Aave V2 transaction logs from `sample_data.json`.
2. **Aggregate by Wallet** – Compute total USD values per action per wallet.
3. **Feature Engineering** – Derive `repay_ratio`, normalize each feature.
4. **Score Calculation** – Use weighted formula to assign credit scores.
5. **Export Results** – Save wallet scores as CSV.

---

## 📁 Output

The notebook outputs a file:
**`wallet_credit_scores_full.csv`**
with the following columns:

* `wallet`
* `credit_score` (0-1000)
* `deposit_usd`
* `borrow_usd`
* `repay_usd`
* `liquidations`

---

## ⚙️ Future Work

* ✅ Use ML models instead of rule-based logic.
* ✅ Include temporal patterns (recency of transactions).
* ✅ Analyze token types (stablecoins vs volatile assets).
* ✅ Add visualizations (histograms, risk clusters, etc.).

---

## 📄 Requirements

* Python 3.8+
* pandas

Install dependencies with:

```bash
pip install pandas
```

---

## 🔒 Disclaimer

This is an educational prototype and should **not be used for real financial decisions** without further validation and peer review.

---

## 👨‍💻 Author

Kabir Kohli
Project Mentor: ChatGPT
License: MIT

