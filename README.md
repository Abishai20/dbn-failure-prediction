# Exercise: Building a Dynamic Bayesian Network (DBN) for Failure Prediction Using Alarm Data

## Objective
Build a Dynamic Bayesian Network (DBN) that predicts whether a machine 
will transition to a Failure state in the next time window based on its 
current state and observed alarm behavior.

## Dataset
Sequential alarm observations collected from an industrial machine, 
organized into fixed time windows (10 minutes). Each time window 
represents the machine operating condition during that period 
(Running or Failure).

## Steps Completed

**Step 0 — Dataset Description**
Load and explore the alarm dataset to understand its structure and identify 
preprocessing requirements.

**Step 1 — Define Alarm Features**
For each alarm, two features are extracted per time window:
- Alarm Count: None (0), Low (1-2), Medium (3-5), High (>5)
- Alarm Duration: None (0), Short (1-30s), Medium (31-300s), Long (>300s)
Note: Duration thresholds were adjusted based on the actual data distribution.

**Step 2 — Define Prediction Target**
State ∈ {Running, Failure}
Given the current machine state and alarm characteristics, estimate the 
probability that the machine will be in Failure state during the next 
time window.

**Step 3 — Generate Training Transitions**
Generate consecutive window pairs (W_t, W_t+1) for DBN training. 
Missing windows are reconstructed as quiet Running periods to preserve 
the Markovian assumption.

**Step 4 — Define DBN Structure**
The DBN models temporal dependencies between consecutive windows:
- State_t → State_t+1
- A1_Count_t → State_t+1
- A1_Duration_t → State_t+1
- A2_Count_t → State_t+1
- A2_Duration_t → State_t+1

**Step 5 — Learn Conditional Probabilities**
DBN estimates probabilities of the form:
P(State_t+1 = Failure | State_t, A1_Count_t, A1_Duration_t, ...)

## Results
| Threshold | Accuracy | Precision | Recall | F1 |
|---|---|---|---|---|
| 0.2 | 81.4% | 60.5% | 59.0% | 59.7% |
| 0.5 | 77.8% | 62.5% | 12.8% | 21.3% |

Recommended threshold: 0.2 — optimized for recall since missing a real 
failure is more costly than a false alarm in predictive maintenance.

## Dependencies
