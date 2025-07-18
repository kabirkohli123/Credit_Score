# Aave V2 Wallet Credit Scoring System

![Credit Score Distribution](https://i.imgur.com/8a1bC2d.png)

This project provides a robust system for assigning a credit score (from 0 to 1000) to DeFi wallets based on their historical transaction behavior on the Aave V2 protocol. The model analyzes raw transaction data to engineer features that reflect a wallet's financial reliability, activity patterns, and risk profile.

The primary goal is to create a transparent, interpretable, and extensible credit scoring mechanism tailored for the DeFi ecosystem.

---

## 🏛️ Architecture & Processing Flow

The system follows a sequential pipeline, transforming raw JSON transaction data into a final credit score. Each step is designed to build a comprehensive behavioral profile of a wallet.

### Processing Pipeline

**1. Data Loading & Preparation**
   - **Input:** `user-wallet-transactions.json` containing an array of transaction records.
   - **Process:** The raw JSON data is loaded into a pandas DataFrame. Nested fields (like `assetSymbol` and `amount`) are extracted from the `actionData` object.
   - **Cleaning:** The `timestamp` is converted into a timezone-aware `datetime` object (UTC) to ensure consistency in all time-based calculations.

**2. Feature Engineering**
   - This is the core of the model, where raw data is transformed into meaningful indicators of behavior.
   - **Transaction Frequency & Recency:**
     - `tx_count`: Total number of transactions. A measure of overall activity.
     - `tx_freq_per_day`: Transactions per day. Differentiates between consistently active wallets and those with sporadic bursts of activity.
     - `days_since_last_tx`: Measures wallet dormancy. A lower value indicates a more recently active wallet.
   - **Asset Diversity:**
     - `unique_assets`: The number of unique assets a wallet has interacted with. Higher diversity can indicate a more sophisticated and engaged user.
   - **Financial Behavior & Ratios:**
     - **Volume:** `deposit_amount_sum`, `borrow_amount_sum`, etc., are calculated to gauge the financial scale of the wallet.
     - **`borrow_ratio` (`repay_sum / borrow_sum`):** A crucial indicator of creditworthiness. A ratio close to or above 1 signifies that the user consistently repays their debts.
     - **`deposit_ratio` (`withdraw_sum / deposit_sum`):** Measures capital flight. A high ratio indicates that the user withdraws a large portion of what they deposit, potentially signaling lower stability.
   - **Risk Indicators:**
     - `liquidation_count`: The number of times a wallet has been liquidated. This is the strongest indicator of high-risk behavior and is heavily penalized.

**3. Scoring Methodology: A Transparent Weighted Model**
   - **Rationale:** Instead of a "black box" machine learning model (like a deep neural network), we use a **weighted scoring model**. This approach is chosen for its **transparency, interpretability, and auditability**. In a financial context, it's crucial to understand *why* a wallet receives a certain score.
   - **Normalization:** All engineered features are scaled to a common range (0 to 1) using `MinMaxScaler`. This ensures that features with large values (like deposit amounts) don't disproportionately influence the score compared to features with small values (like liquidation counts).
   - **Weighted Sum:** A `raw_score` is calculated by multiplying each normalized feature by its assigned weight and summing the results. Weights are assigned based on the feature's importance in determining creditworthiness (e.g., `liquidation_count` has a large negative weight, while `borrow_ratio` has a large positive weight).

**4. Anomaly Detection with Isolation Forest**
   - **Purpose:** To identify wallets with unusual or outlier behavior that might not be captured by the weighted model alone. This can help flag potential bots or airdrop farmers.
   - **Method:** An `IsolationForest` model is trained on the feature set. This is an unsupervised algorithm that is efficient at detecting anomalies by "isolating" observations.
   - **Penalty:** Wallets flagged as anomalies have their final credit score reduced by 50%, acting as a penalty for potentially risky or non-standard behavior.

**5. Final Score Generation**
   - The `raw_score` is scaled to the final 0-1000 range.
   - The anomaly penalty is applied.
   - The final scores are saved to `aave_credit_scores.csv` and a distribution histogram is generated for visualization.

---

## 🚀 How to Run

1.  **Prerequisites:**
    * Python 3.x
    * Jupyter Notebook or JupyterLab
    * Required libraries: `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `seaborn`. Install them with:
        ```bash
        pip install pandas numpy scikit-learn matplotlib seaborn jupyterlab
        ```

2.  **Setup:**
    * Clone this repository.
    * Place your `user-wallet-transactions.json` file in the root directory.

3.  **Execution:**
    * Launch JupyterLab:
        ```bash
        jupyter lab
        ```
    * Open the `.ipynb` notebook file.
    * Run the cells sequentially from top to bottom.

4.  **Output:**
    * A CSV file named `aave_credit_scores.csv` will be generated with the wallet addresses and their credit scores.
    * A plot showing the distribution of credit scores will be displayed in the notebook.

---

## 🔧 Extensibility & Future Work

This model serves as a strong foundation. It can be extended in several ways:

* **Additional Features:** Incorporate Aave-specific metrics like Health Factor, or on-chain data like wallet age and gas fees paid.
* **Cross-Protocol Analysis:** Integrate data from other DeFi protocols (e.g., Compound, Uniswap) to build a more holistic, chain-agnostic credit score.
* **Advanced Modeling:** While the weighted model is great for transparency, a more complex model (e.g., Gradient Boosting) could be used to capture non-linear relationships between features, potentially for the anomaly detection component.
* **Time-Series Analysis:** Analyze how a wallet's behavior and score change over time to predict future creditworthiness.

---

## 📄 License

This project is licensed under the MIT License. See the `LICENSE` file for details.
