# AI-Based-Gas-Leakage-Severity-Prediction-
An AI-based system that predicts gas leakage severity using machine learning to support early detection, risk assessment, and safety monitoring.

import numpy as np
from sklearn.ensemble import RandomForestClassifier

# AI-Based Gas Leakage Severity Prediction System
# DEMO ONLY: Synthetic data and illustrative thresholds.
# Do not use for real gas safety decisions.

def calculate_severity(gas, temperature, humidity):
    # Illustrative scoring formula for generating demo labels only
    score = (
        gas
        + max(temperature - 30, 0) * 20
        + max(humidity - 70, 0) * 5
    )

    if score < 500:
        return "Low"
    elif score < 1000:
        return "Medium"
    else:
        return "High"


# Generate synthetic training data
np.random.seed(42)
samples = 2000

gas_values = np.random.uniform(0, 1500, samples)
temperatures = np.random.uniform(15, 60, samples)
humidities = np.random.uniform(20, 100, samples)

X = np.column_stack(
    (gas_values, temperatures, humidities)
)

y = np.array([
    calculate_severity(g, t, h)
    for g, t, h in X
])

# Train the machine learning model
model = RandomForestClassifier(
    n_estimators=100,
    random_state=42
)
model.fit(X, y)

print("=" * 45)
print(" AI GAS LEAKAGE SEVERITY PREDICTION")
print("=" * 45)

try:
    gas = float(input("Enter gas concentration (demo units): "))
    temperature = float(input("Enter temperature (°C): "))
    humidity = float(input("Enter humidity (%): "))

    if gas < 0 or not 0 <= temperature <= 100:
        raise ValueError("Invalid gas or temperature value.")

    if not 0 <= humidity <= 100:
        raise ValueError("Humidity must be between 0 and 100.")

    # Predict severity using the trained AI model
    prediction = model.predict(
        [[gas, temperature, humidity]]
    )[0]

    print("\n--- Prediction Result ---")
    print(f"Gas concentration: {gas}")
    print(f"Temperature: {temperature} °C")
    print(f"Humidity: {humidity}%")
    print(f"Predicted severity: {prediction}")

    if prediction == "Low":
        print("Demo classification: LOW")
    elif prediction == "Medium":
        print("Demo classification: MEDIUM")
    else:
        print("Demo classification: HIGH")

    print("\nNote: This is a software demonstration,")
    print("not a real gas detection or safety device.")

except ValueError as error:
    print("Invalid input:", error)
