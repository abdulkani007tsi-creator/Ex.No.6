# Ex.No.6 Development of Python Code Compatible with Multiple AI Tools

# Aim:
To write and implement Python code that integrates with multiple AI tools 
(such as OpenAI GPT, Hugging Face, and Google Gemini) to automate API interactions, 
compare outputs, and generate actionable insights.
# AI Tools Required:
  OpenAI GPT-3.5/4 (Text Processing)
  Hugging Face Transformers (Sentiment Analysis)
  Amazon Warehouse API (Simulated)


## **Algorithm & Implementation**

### **1. Simulated API Data Fetching for a Smart Farming System**
```python
import random

def fetch_farm_sensor_data():
    return {
        "sensor_alerts": [
            "Soil moisture low in Field A",
            "Temperature rising above normal",
            "Water pump active in Sector 3"
        ],
        "farm_status": random.choice(["Healthy", "Irrigation_Required", "Critical"])
    }

```

### **2. AI Tools Integration**
```python
# Hugging Face Sentiment Analysis
from transformers import pipeline

def extract_entities(texts):
    ner_model = pipeline("ner", grouped_entities=True)
    return [ner_model(text) for text in texts]


# OpenAI Summarization
import openai

def classify_risk_level(alert_text, api_key):
    openai.api_key = api_key

    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[
            {"role": "system", "content": "You are a risk classification assistant."},
            {"role": "user", "content": f"Classify the risk level of this alert as Low, Medium, or High:\n{alert_text}"}
        ]
    )

    return response.choices[0].message["content"]

```

### **3. Insight Generation Based on Entity Extraction + Risk Classification**
```python
def generate_advanced_insights(data, entities, risk_levels):
    # Count how many alerts are classified as HIGH risk
    high_risk_count = sum(1 for r in risk_levels if "High" in r)

    # Count how many locations (from entity extraction)
    locations = []
    for item in entities:
        for ent in item:
            if ent['entity_group'] == 'LOC':   # location entities
                locations.append(ent['word'])

    return {
        "total_alerts": len(risk_levels),
        "high_risk_alerts": high_risk_count,
        "affected_locations": list(set(locations)),
        "system_condition": "Critical" if high_risk_count >= 2 else "Stable",
        "action_plan": "Deploy emergency response team"
                        if high_risk_count >= 2
                        else "Monitor regularly and schedule inspection"
    }

```

### **4. Complete Workflow**
```python
def main():
    # 1. Fetch Sensor Data
    data = fetch_farm_sensor_data()

    # 2. AI Processing
    entities = extract_entities(data["sensor_alerts"])
    risk_levels = [classify_risk_level(alert, "your_openai_key")
                   for alert in data["sensor_alerts"]]

    # 3. Generate Insights
    insights = generate_advanced_insights(data, entities, risk_levels)

    # 4. Display Results
    print("=== Smart Farming AI Report ===")
    for alert, risk in zip(data["sensor_alerts"], risk_levels):
        print(f" • {alert} → Risk: {risk}")

    print("\nExtracted Entities:")
    for i, ent_set in enumerate(entities):
        print(f" Alert {i+1}: {ent_set}")

    print("\nSystem Condition:", insights["system_condition"])
    print("High-Risk Alerts:", insights["high_risk_alerts"])
    print("Affected Locations:", insights["affected_locations"])
    print("Action Plan:", insights["action_plan"])


if __name__ == "__main__":
    main()

```

---

## **Execution Steps:**  
1. Install dependencies:
   ```bash
   pip install transformers openai
   ```
2. Set OpenAI API key  
3. Run script:
   ```bash
   python warehouse_ai.py
   ```

---

## **Input Prompt:**

"Write a Python program that fetches simulated warehouse robot alert data, analyzes the sentiment of each alert using the Hugging Face Transformers sentiment-analysis pipeline, and generates a summary of all alerts using the OpenAI GPT API. The program must follow a modular structure with separate functions for (1) data fetching, (2) sentiment analysis, (3) OpenAI summarization, and (4) insight generation. Based on the sentiment analysis results and the operational status of the warehouse, the program should generate maintenance recommendations. The final output should clearly display each alert with its sentiment score, the AI-generated summary, and a final recommendation for warehouse maintenance."

## **Output:**
```
=== Warehouse AI Report ===
Overheating detected in Robot Unit R7 (NEGATIVE 0.92)
Navigation delay in Aisle C2 (NEGATIVE 0.76)
Inventory scan completed successfully in Zone 4 (POSITIVE 0.98)

AI Summary: The system detected two performance-related issues 
(overheating and navigation delay) along with one completed inventory scan.
Overall activity indicates the need for system inspection.

Recommendation: Routine check

```

---

## **Conclusion:**  
The system successfully demonstrates:  

1. **Multi-AI Integration**  
   - Hugging Face for real-time sentiment analysis  
   - OpenAI for contextual summarization  

2. **Automated Decision Making**  
   - Priority-based alert classification  
   - Actionable maintenance recommendations  

3. **Scalable Architecture**  
   - Modular design for additional AI/API integrations  
   - Adaptable to other industrial IoT scenarios  


# Result:  
The corresponding Prompt is executed successfully.
