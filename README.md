# Wallet Credit Score

This project analyzes transaction data from Aave V2 to assign a credit score (from 0 to 1000) to each wallet. Higher scores indicate more reliable and trustworthy behavior.

<img width="460" height="251" alt="image" src="https://github.com/user-attachments/assets/6061c967-4dd7-4e60-aa8c-407164faa3ef" />


---

## How It Works

The model looks at several key aspects of a wallet's transaction history to determine its score:

* **Transaction Activity:** How often does the wallet make transactions? Is it active or dormant?
* **Financial Behavior:** How much has the wallet deposited versus how much it has borrowed?
* **Repayment History:** Does the wallet reliably pay back its loans? A high repayment rate is a strong positive signal.
* **Liquidations:** Has the wallet ever been liquidated? Liquidations are heavily penalized as they indicate high-risk behavior.
* **Anomalies:** The model also looks for unusual, bot-like behavior and reduces the score accordingly.

These factors are combined into a single, easy-to-understand credit score.

---

## 🚀 How to Run

1.  **Get the Code & Dependencies:**
    * Clone this repository.
    * Install the required Python libraries:
        ```bash
        pip install pandas numpy scikit-learn matplotlib seaborn jupyterlab
        ```

2.  **Add Data:**
    * Place your `user-wallet-transactions.json` file in the same directory as the notebook.

3.  **Run the Notebook:**
    * Launch Jupyter Lab (`jupyter lab`).
    * Open the `.ipynb` file and run all the cells.

### Output

The script will generate:
* A CSV file named `aave_credit_scores.csv` with the final scores.
* A graph showing the distribution of the wallet scores.

---

## 🔧 Future Ideas

* Add data from other DeFi protocols like Compound or Uniswap.
* Track how wallet scores change over time.
* Incorporate other on-chain data like wallet age.
