# Project Architecture & Workflow: Dynamic Parking Pricing System
This project proposes a smart dynamic pricing engine for parking lots using three different strategies: a momentum-based model, a demand-based model, and a real-time streaming model using Pathway. The architecture is designed to be modular, flexible, and responsive to real-time and historical data.

# Central Input: Parking Lot Data (CSV)
All models start from a common data source, which is a CSV file containing real-time and historical parking lot data. Each row typically contains fields such as:
1. Timestamp
2. SystemCodeNumber
3. Occupancy
4. Capacity
5. QueueLength
6. TrafficConditionNearby
7. IsSpecialDay
8. VehicleType
This CSV acts as the raw input pipeline feeding into the three models.

# Model 1: Momentum-Based Baseline
Goal: Provide a baseline model using simple logic and occupancy trends.

# Workflow:
1. Preprocessing: Uses Pandas and NumPy to clean and format the dataset.
2. Occupancy Delta: Computes the change in occupancy normalized by capacity for each parking lot.
3. Momentum: Applies a rolling sum of the last 3 occupancy deltas to capture trends.
4. Pricing Logic: Increases or decreases price proportionally to the momentum using a linear as well as non-linear rule-based formula.
# Output: The generated price is appended as Price_Momentum.
This model is designed to be simple, interpretable, and fast.

# Model 2: Demand-Based Prediction
Goal: Use multiple weighted factors to build a smarter, adaptive pricing mechanism.

# Workflow:
1. Feature Computation: Extracts key demand influencers like queue length, traffic, vehicle type, hour, special day, and momentum.
2. Weighted Demand: Each factor contributes to a raw demand score, which is further weighted by time (hour of the day).
3. Nonlinear Activation: Applies a log or tanh activation to normalize demand and allow for smarter price scaling.
4. Final Price: Calculates final price using:
# price = base_price * (1 + λ × demand_factor)
5. Ensures clamping between $5–$20.
6. Applies smoothing (cap of $3) to avoid sharp price jumps.
# Output: Added as Price_DemandModel.
This model is more realistic and sensitive to contextual variations.

# Pathway Model: Real-Time Streaming Price Calculation
Goal: Use Pathway’s streaming engine to stream dynamic parking price in real-time based on live updates.

# Workflow:
1. Stream Input: The CSV is read using pathway.io.csv.read() in streaming mode.
2. Feature Engineering: Extracts Hour from timestamps and computes all necessary features inline.
3. UDF Logic: Applies a @pw.udf to implement the demand-aware logic dynamically, including:
Occupancy, queue length, traffic, hour, vehicle type, and special day.
# Output: Directs result to console using pw.debug.compute_and_print() or similar for live monitoring.
This model is production-grade, reactive, and suitable for real-world smart parking systems.

# SUMMARY
This modular architecture demonstrates a progressive evolution from baseline model to intelligent, real-time pricing systems. It empowers city planners, smart parking services, and ML practitioners to choose the right balance of interpretability, accuracy, and deployment-readiness.