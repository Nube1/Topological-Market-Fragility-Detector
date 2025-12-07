# 📉 Topological Market Fragility Detector

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![TDA](https://img.shields.io/badge/Topic-Topological_Data_Analysis-purple)
![Finance](https://img.shields.io/badge/Domain-Quantitative_Finance-green)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

A quantitative finance project that applies **Topological Data Analysis (TDA)** and **Persistent Homology** to detect structural fragility and "liquidity voids" in financial Limit Order Books (LOB).

## 📖 Overview

In high-frequency trading, a "liquidity void" occurs when there is a significant gap in the order book. If a large market order hits this void, price slippage increases dramatically, potentially triggering a flash crash.

Traditional metrics (like volume or average spread) often fail to capture the *shape* of the liquidity. This project uses **Persistent Homology** to treat the Order Book as a point cloud. It mathematically proves that a "fragile" order book has a distinct topological signature compared to a "healthy" one.

### The Problem
*   **Real LOB data (Level 3)** is proprietary and expensive.
*   **Standard metrics** don't always visualize the "gaps" effectively.

### The Solution
*   **Fetch** real-time top-of-book prices (via `yfinance`) to anchor the simulation in reality.
*   **Simulate** the depth of the book:
    1.  **Healthy State:** Dense, Gaussian distribution of limit orders.
    2.  **Fragile State:** Programmatically induced liquidity voids (gaps).
*   **Compute** the Persistent Homology (using `ripser`) to generate Persistence Diagrams.
*   **Visualize** the difference, showing the "smoking gun" signature of a fragile market.

---

## 🛠️ Installation

1.  Clone the repository:
    ```bash
    git clone https://github.com/yourusername/topological-market-fragility.git
    cd topological-market-fragility
    ```

2.  Install the required dependencies:
    ```bash
    pip install numpy matplotlib pandas yfinance ripser persim
    ```

### Libraries Used
*   **`ripser`**: Lean C++ wrapper for computing persistent homology (incredibly fast).
*   **`persim`**: Tools for visualizing persistence diagrams.
*   **`yfinance`**: Fetches real-time anchor prices (SPY, AAPL, etc.).
*   **`matplotlib`**: Visualization of LOBs and diagrams.

---

## 🚀 Usage

Run the main script to generate the simulation and analysis:

```bash
python lob_topology.py
```

### What happens when you run it?
1.  The script pulls the latest price for **SPY** (S&P 500 ETF).
2.  It generates two datasets: a **Normal** book and a **Fragile** book (with a simulated liquidity gap).
3.  It calculates topological features (H0 homology - connected components).
4.  It opens a dashboard displaying the order distribution and the topological persistence diagrams.

---

## 📊 Interpreting the Results

The output will be a 2x2 Plot Grid. Here is how to read it:

### 1. The LOB Histograms (Top Row)
*   **Normal:** You will see a continuous bell curve of orders.
*   **Fragile:** You will see a physical gap in the histogram bars. This represents price levels where no limit orders exist.

### 2. The Persistence Diagrams (Bottom Row)
This is the core TDA analysis.
*   **X-Axis (Birth):** When a component (cluster of orders) forms.
*   **Y-Axis (Death):** When that component merges with another.
*   **The Diagonal:** Points near the diagonal represent noise or very dense data.
*   **The "Outliers" (The Signal):**
    *   In the **Fragile** diagram, look for **points far away from the diagonal** (high persistence).
    *   These points represent the "Void." It mathematically indicates that the algorithm had to expand the search radius significantly to bridge the gap between orders.

---

## 📁 Project Structure

```text
.
├── lob_topology.py     # Main execution script
├── README.md           # Documentation
└── requirements.txt    # (Optional) List of dependencies
```

---

## ⚠️ Disclaimer

This project uses **simulated depth** based on real top-of-book prices. While `yfinance` provides the current price, the "limit orders" below that price are generated synthetically for educational and demonstration purposes. In a live production environment, this algorithm should be fed real Level 2 or Level 3 market data.

---

## 📜 License

Distributed under the MIT License. See `LICENSE` for more information.

---

## 🤝 Contributing

1.  Fork the Project
2.  Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3.  Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4.  Push to the Branch (`git push origin feature/AmazingFeature`)
5.  Open a Pull Request
