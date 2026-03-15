# Smart-Grid-Battery-Dispatch
Utility-scale battery storage optimizer. Uses convex optimization to maximize energy arbitrage profit while respecting physical hardware constraints.

## Overview
This project uses **Linear Programming (LP)** to optimize the daily operation of a utility-scale battery storage system. By predicting day-ahead electricity prices, the algorithm calculates the exact hourly schedule for charging and discharging the battery to maximize financial arbitrage profit while adhering to the physical constraints of the hardware.

## The Mathematical Model
The optimization is formulated as a convex optimization problem using the `cvxpy` library. 

### Objective Function
The goal is to maximize total daily profit by engaging in energy arbitrage (buying low, selling high):

$$\max \sum_{t=1}^{T} \left( P_{t}^{dis} \cdot \pi_t - P_{t}^{cha} \cdot \pi_t \right)$$

**Where:**
* $P_{t}^{dis}$ = Power discharged to the grid at hour $t$ (MW)
* $P_{t}^{cha}$ = Power charged from the grid at hour $t$ (MW)
* $\pi_t$ = Electricity spot price at hour $t$ (€/MWh)

### Physical Constraints
A battery cannot charge infinitely, nor is it perfectly efficient. The mathematical solver must respect the physical dynamics of the system:

**1. Power Inverter Limits:**
$$0 \le P_{t}^{cha} \le P_{max}$$
$$0 \le P_{t}^{dis} \le P_{max}$$

**2. Energy Capacity & State of Charge (SoC) Dynamics:**
The energy stored in the battery at the next hour depends on the current hour, accounting for efficiency losses ($\eta$) during both charging and discharging:
$$E_{t+1} = E_t + (P_{t}^{cha} \cdot \eta) - \left( \frac{P_{t}^{dis}}{\eta} \right)$$

**3. Capacity Limits:**
$$0 \le E_t \le E_{capacity}$$

## Business Value
With the rapid expansion of renewable energy (wind and solar), electricity grid prices have become highly volatile. Energy companies and grid operators require mathematically rigorous dispatch algorithms to stabilize the grid and ensure battery assets generate the maximum possible return on investment. This model demonstrates how mathematical operations research directly translates to clean energy profitability.

## Technologies Used
* **Python**
* **CVXPY** (Convex optimization solver)
* **NumPy / Pandas** (Data structuring)

## How to Run
1. Clone this repository.
2. Install the required libraries: `pip install numpy pandas cvxpy`
3. Run the optimization engine: `python battery_dispatch_optimizer.py`
