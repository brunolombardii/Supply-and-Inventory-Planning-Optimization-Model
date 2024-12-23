# Supply and Inventory Planning Optimization Model

This repository contains an optimization model developed in **Pyomo** for supply and inventory planning. The model minimizes total costs while satisfying demands and adhering to operational constraints. This repository contains the implementation of a supply chain optimization model using Pyomo. The model is designed to handle various constraints and objectives for efficient planning and decision-making within a supply chain network.

## 🚀 Project Purpose
The model addresses:
- Inventory planning.
- Optimization of logistical resource usage.
- Penalties for failing to meet minimum requirements.

## 📊 Mathematical Formulation

### Objective Function:
Minimize the total cost \( Z \), which includes:
1. **Storage Costs** for raw materials and finished goods.
2. **Production Costs**.
3. **Logistics Costs** (fixed truck usage costs).
4. **Penalties** for unmet minimum order quantities and negative inventory.
 
$$ Z = \sum_{i \in I, t \in T} \left( pr[i] \cdot \left( c + \frac{TIE}{52} \right) \cdot y[i, t] + t\text{cost} \cdot cu[t] + Pen[i] \cdot penalidad3[i, t] \right) + \sum_{p \in PT, t \in T} \left( \left( c + \frac{TIE}{52} \right) \cdot c\text{prod}[p] \cdot y\text{pt}[p, t] + PN \cdot penalidad\text{negativa}[p, t] + c\text{prod}[p] \cdot pro[p, t] + \text{backordercost}[p] \cdot \text{backorder}[p, t] \right)$$

### Constraints:

1. **Initial Inventory**:
   - Inputs:
     ```math
     y[i, 0] = I_0[i]
     ```
   - Finished products:
     ```math
     y_pt[p, 0] = PT_0[p]
     ```

2. **Inventory Balance**:
   - Finished products:
     ```math
     y_pt[p, t] = y_pt[p, t-1] - d[p, t-1] + pro[p, t-1]
     ```
   - Inputs:
     ```math
     y[i, t] = y[i, t-1] + q[i, t] - \sum_{p \in PT} cons[i, p] \cdot pro[p, t]
     ```

3. **Capacity Constraints**:
   - Production capacity:
     ```math
     pro[p, t] \leq k_max \cdot d[p, t]
     ```
   - Truck capacity:
     ```math
     \sum_{i \in I} z[i, t] = k \cdot cu[t]
     ```

4. **Pallet Multiples**:
   ```math
   q[i, t] = z[i, t] \cdot Pal[i]

5. **Penalty Constraints**

- Negative Inventory Penalty:
  ```math
  penalidad\_negativa[p, t] \geq -y\_pt[p, t]
  ```
Non-Negative Penalty:
  ```math
  penalidad\_negativa[p, t] \geq 0
  ```
Backorder Penalty:
  ```math
  y\_pt[p, t] + backorder[p, t] \geq ss[p] + l \cdot d[p, t]
  ```

Penalty for Unmet Minimum Order:
  ```math
   penalidad[i, t] \cdot 10^{10} \geq q\_min[i] - q[i, t]
   penalidad2[i, t] \cdot 10^{10} \geq q[i, t]
   penalidad3[i, t] \geq \frac{penalidad[i, t] + penalidad2[i, t]}{2} - 0.9

Final Inventory Constraints (Inputs):
  ```math
   y[i, \max(T)] \geq I\_0[i] \cdot nivel\_final\_minimo
  ```

Final Inventory Constraints (Finished Products):
   ```math
   y\_pt[p, \max(T)] \geq PT\_0[p] \cdot nivel\_final\_minimo
  ```

Ensure Non-Negative Inventory:
  ```math
  y[i, \max(T)] \geq 0
  y\_pt[p, \max(T)] \geq 0
  ```



## 📂 Repository Content
- **`supply-chain-optimization-pyomo.ipynb`** A Jupyter Notebook that contains the Pyomo model implementation. This notebook includes:
- Definition of the problem
- Data import and preprocessing
- Model formulation
- Solution and analysis of results
  
Excel Files with Parametric Setup:
These files include the input parameters for the optimization model, such as:
- Demand forecasts
- Supply constraints
- Cost matrices
- Facility capacities

Excel File with Solution:
This file contains the final results, showcasing the values of critical variables such as:
- Optimal shipment quantities
- Total cost of the optimized plan


## 🧩 Model Extensions

This model can be extended to handle more complex supply chain scenarios, such as:

1. Multi-Echelon Supply Chains:
  -  Add multiple levels of suppliers, warehouses, and distribution centers.
  - Model the flow of goods across all levels.
2. Stochastic Demand:
  - Introduce probabilistic scenarios to account for demand uncertainty.
  - Solve the model under different demand realizations and compute risk metrics.
3. Dynamic Pricing: Include price elasticity of demand to optimize pricing decisions alongside supply planning.
