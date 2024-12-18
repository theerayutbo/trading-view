This script is an implementation of the **Hurst Exponent** in Pine Script, designed for TradingView. The Hurst Exponent is a statistical indicator used to measure the predictability and fractality of time series data, often applied in financial markets. Here's a detailed description of the code:

---

### **Purpose**
- **Indicator Name:** "Hurst Exponent [QuantA]" (short title: "Hurst Exp [QA]").
- **Purpose:** To calculate and display the **Hurst Exponent** along with its smoothed version, providing insights into market behavior—whether it trends, reverts to the mean, or exhibits random walk behavior.
- **Overlay:** This indicator is displayed in a separate panel below the price chart.

---

### **Inputs**
1. **Lookback Period** (`lookback`):
   - Defines the number of bars to consider for calculating the Hurst Exponent.
   - Default: `1024`.

2. **Sample Sizes** (`hurt_len1` to `hurt_len8`):
   - Allows users to specify up to 8 different group lengths for rescaled range analysis. These sample sizes determine the scale at which the Hurst Exponent is evaluated.
   - Default values:
     - `hurt_len4`: 32
     - `hurt_len5`: 64
     - `hurt_len6`: 128
     - `hurt_len7`: 256
     - `hurt_len8`: 512
     - Other lengths default to `0` (disabled).

3. **Smoothing Options**:
   - `show_smooth`: Enables or disables the display of a smoothed Hurst Exponent using an EMA.
   - `smooth_len`: Sets the length of the smoothing EMA. Default: `10`.

4. **Calculation Start Date** (`calc_from`):
   - Specifies the date from which the calculations should begin.
   - Default: `January 1, 2000`.

---

### **Key Calculations**

1. **Price Normalization (`pnl`)**:
   - Computes the percentage change between consecutive closing prices.

2. **Rescaled Range Analysis** (`get_avg_rs` function):
   - This function calculates the average rescaled range (R/S) for a specified group length (`group_len`).
   - **Steps:**
     1. **Group Splitting:** Divides the data into groups based on the specified group length.
     2. **Mean Calculation:** Computes the mean of normalized price changes for each group.
     3. **Standard Deviation:** Calculates the standard deviation of the price changes in each group.
     4. **Cumulative Range:** Computes the cumulative range (`cum`) and its maximum and minimum values.
     5. **Rescaled Range:** Calculates the ratio of the cumulative range to the standard deviation for each group.
     6. **Average R/S:** Averages the rescaled ranges across all groups.

3. **Logarithmic Transformation**:
   - Converts the average R/S values and corresponding group lengths into their logarithmic forms for Hurst Exponent calculation.

4. **Hurst Exponent Calculation (`hurstexp`)**:
   - Uses linear regression to estimate the slope of the log-log plot of R/S versus group length.
   - The slope represents the Hurst Exponent, which ranges as follows:
     - **H > 0.5:** Trending market behavior (persistent).
     - **H = 0.5:** Random walk (neutral).
     - **H < 0.5:** Mean-reverting behavior (anti-persistent).

5. **Smoothing**:
   - Applies an exponential moving average (EMA) to the Hurst Exponent for a smoother output (`hurstexp_smooth`).

---

### **Outputs (Plot)**
1. **Hurst Exponent (`hurstexp`)**:
   - Plotted in green with a linewidth of 2.

2. **Smoothed Hurst Exponent (`hurstexp_smooth`)**:
   - Plotted in red if smoothing is enabled.

3. **Midline**:
   - A horizontal line at `0.5`, indicating the threshold between trending and mean-reverting behavior.

---

### **Usage**
- **Market Analysis:**
  - Use the Hurst Exponent to determine market behavior:
    - **H > 0.5:** Indicates persistent or trending behavior, suggesting the market is likely to continue its direction.
    - **H < 0.5:** Indicates mean-reverting behavior, suggesting potential reversals.
    - **H = 0.5:** Indicates random walk, implying no clear trend.
- **Timeframe Selection:**
  - The choice of `lookback` and `hurt_len` values should match the trader's timeframe and market structure.

---

### **Code Features**
- **Customizable Sample Sizes:** Allows fine-tuning of group lengths for rescaled range analysis.
- **Smoothing Options:** Adds flexibility in visualizing the indicator with or without smoothing.
- **Historical Start Date:** Enables focused analysis from a specific point in time.
- **Efficient Calculations:** Uses arrays and loop constructs to perform calculations for multiple sample sizes efficiently.

---

This script provides a robust implementation of the Hurst Exponent, making it a valuable tool for traders and analysts seeking to understand market dynamics and behavior.
