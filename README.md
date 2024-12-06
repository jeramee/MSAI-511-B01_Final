# MSAI-511-B01_Final

## **AI-Driven Display Graphics, Marketing Insights, and Stock Trading Simulations**

This repository explores the use of **Artificial Intelligence (AI)** in marketing through **dynamic display graphics** and highlights **Voila** as a free and accessible tool for creating interactive web applications. The stock trading simulation serves as an example of how Voila's capabilities can empower **under-served groups** by providing a **low barrier of entry** to advanced technology.

---

## **Research Paper: AI in Marketing**

The accompanying research paper examines AI's role in transforming marketing through interactive visualizations and dynamic web applications. The study emphasizes Voila's potential as a **disruptive and equitable solution** for businesses and creators.

---

### **Research Paper Structure**

1. **Abstract**: Overview of the topic, objectives, key findings, and Voila's accessibility.
2. **Introduction**: Importance of AI in marketing and Voila's role as a disruptor.
3. **Problem Statement**: Challenges in creating engaging and equitable marketing tools.
4. **Research Analysis**: Literature review on AI in marketing and accessibility.
5. **General Findings**: Summary of insights.
6. **Strengths of AI Disruption**: Benefits of AI-powered marketing tools.
7. **Weaknesses of AI Disruption**: Challenges and ethical considerations.
8. **Opportunities in AI Adoption**: Innovation potential for underserved groups.
9. **Threats of AI Adoption**: Risks such as exclusion or over-reliance on technology.
10. **Problem Solving with AI**: Application of findings to solve real-world marketing challenges.
11. **Further Research**: Suggestions for future exploration.
12. **Conclusion**: Summary of findings and implications.
13. **References**: Peer-reviewed sources in APA format.

---

## **Project: Stock Trading Simulation as a Marketing Graphics Example**

This project uses **Voila** and **Jupyter Notebooks** to simulate stock trading while showcasing the potential for **dynamic visualizations in marketing applications**.

---

### **Features**

- Fetch live stock data using the **Yahoo Finance API**.
- Implement a moving average crossover trading strategy.
- Provide interactive widgets for profit simulation.
- Forecast trends with linear regression.
- Deploy the app as a **web-based solution** using Voila.

---

### **File Structure**

```plaintext
stock_ai_project/
├── notebooks/
│   └── stock_simulation.ipynb   # Jupyter Notebook showcasing the project
├── data/                        # Folder for storing downloaded datasets
├── environment.yml              # Conda environment configuration
├── Dockerfile                   # Docker setup for reproducibility
├── requirements.txt             # Pip requirements
├── README.md                    # Project documentation
└── scripts/                     # Helper scripts
    └── utils.py                 # Functions for data fetching and visualization
```

# **Setup Instructions**

## **Using Conda**

### Create the Conda Environment

```bash
conda create -n stock_ai python=3.9 -y
conda activate stock_ai
```

## **Install Dependencies**

```bash
conda install numpy pandas matplotlib scikit-learn ipywidgets -y
pip install yfinance voila
```

## Enable Widgets

```
jupyter nbextension enable --py widgetsnbextension
```

## Export the Environment

```
conda env export > environment.yml
```

## Using Docker (Optional)

Create Dockerfile

```
FROM python:3.9-slim

RUN apt-get update && apt-get install -y \
    gcc \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app
COPY . .

RUN pip install -r requirements.txt
RUN jupyter nbextension enable --py widgetsnbextension

EXPOSE 8888
CMD ["voila", "notebooks/stock_simulation.ipynb", "--no-browser", "--port=8888", "--strip-source"]
```

## Build and Run Docker Image
### Build the Docker Image

```
docker build -t stock_ai .
```
### Run the Docker Container

```
docker run -p 8888:8888 stock_ai
```

## Running the Application
### Launch Jupyter Notebook

```
jupyter notebook
```
- Open the notebooks/stock_simulation.ipynb file.

Deploy as a Web App
```
voila notebooks/stock_simulation.ipynb
```
- Access the app in your browser.

## Sample Code
### Fetch Stock Data

```
import yfinance as yf

def fetch_stock_data(ticker, start, end):
    data = yf.download(ticker, start=start, end=end)
    data['SMA_50'] = data['Close'].rolling(window=50).mean()
    data['SMA_200'] = data['Close'].rolling(window=200).mean()
    return data
```

### Trading Strategy

```
def trading_strategy(data):
    data['Signal'] = 0
    data.loc[data['SMA_50'] > data['SMA_200'], 'Signal'] = 1  # Buy Signal
    data.loc[data['SMA_50'] <= data['SMA_200'], 'Signal'] = -1  # Sell Signal
    return data
```

### Interactive Profit Simulation

```
from ipywidgets import interact, FloatSlider
import matplotlib.pyplot as plt

def simulate_profit(data, buy_adjust, sell_adjust):
    buy_price = data['Close'].iloc[0] * (1 - buy_adjust / 100)
    sell_price = data['Close'].iloc[-1] * (1 + sell_adjust / 100)
    profit = sell_price - buy_price

    plt.figure(figsize=(12, 6))
    plt.plot(data.index, data['Close'], label='Close Price')
    plt.axhline(y=buy_price, color='green', linestyle='--', label=f'Buy @ {buy_price:.2f}')
    plt.axhline(y=sell_price, color='red', linestyle='--', label=f'Sell @ {sell_price:.2f}')
    plt.title(f'Profit Simulation: ${profit:.2f}')
    plt.legend()
    plt.show()

interact(simulate_profit,
         data=fixed(data),
         buy_adjust=FloatSlider(min=0, max=50, step=1, value=5),
         sell_adjust=FloatSlider(min=0, max=50, step=1, value=5));
```

## **Why Use Voila?**

- **Free and Open-Source**: Removes financial barriers, making it ideal for under-served groups.
- **Low Technical Overhead**: Simplifies deployment without additional backend requirements.
- **Dynamic Interactivity**: Enables engaging, real-time data visualizations.

