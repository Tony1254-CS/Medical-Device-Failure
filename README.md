# Medical Device Failure Dataset — ICU Operational Records

## Overview

An operational dataset representing three representative ICU device 
categories: ventilators, infusion pumps, and patient monitors. The 
dataset integrates mechanical sensor readings with clinical enhancement 
criteria to capture devices that may appear operationally functional 
yet deliver clinically unsafe outputs.

This dual-failure definition — combining mechanical breakdown with 
clinically flagged degradation — addresses a critical blind spot in 
conventional maintenance datasets.

**Records:** 2,000  
**Features:** 29  
**Overall failure rate:** 3.35%  
**Device categories:** Ventilators (850), Infusion Pumps (750), 
Patient Monitors (400)

---

## Device Distribution and Failure Events

| Device Type | Total Records | Mechanical Failures | Clinical-Enhanced Failures | Total Failure Rate |
|---|---|---|---|---|
| Ventilators | 850 | 18 | 7 | 2.94% |
| Infusion Pumps | 750 | 22 | 5 | 3.60% |
| Patient Monitors | 400 | 12 | 3 | 3.75% |
| Overall | 2,000 | 52 | 15 | 3.35% |

---

## Feature Reference

### Core Sensor Features
| Column | Type | Description |
|---|---|---|
| UDI | int | Unique device identifier |
| Product ID | str | Device serial identifier |
| Type | str | Device category: L / M / H |
| Air temperature [K] | float | Ambient temperature in Kelvin |
| Process temperature [K] | float | Operating temperature in Kelvin |
| Rotational speed [rpm] | int | Motor rotational speed |
| Torque [Nm] | float | Drive torque in Newton-meters |
| Tool wear [min] | int | Cumulative component wear in minutes |
| Machine failure | int | Binary failure label (1 = failure) |

### Engineered Mechanical Indicators
| Column | Type | Description |
|---|---|---|
| thermal_gradient | float | Thermal stress differential; proxy for cooling efficiency |
| power_estimate | float | Motor strain index; composite of torque and speed |
| wear_rate_indicator | float | Degradation intensity ratio relative to operational speed |
| temp_stability | float | Temperature variance proxy |
| wear_acceleration | float | Rate of change in component wear |
| operating_hours_proxy | float | Normalized sequential operating time |
| operating_regime | int | Operational state: high-stress / normal / degraded |
| Type_encoded | int | Numeric device type encoding |

### Clinical Enhancement Features
Rule-based clinical flags applied to sensor readings to identify 
devices delivering clinically unsafe outputs despite appearing 
mechanically operational.

| Column | Type | Description |
|---|---|---|
| rule_tachycardia | bool | Elevated motor speed threshold flag |
| rule_tachypnea | bool | Elevated process rate flag |
| rule_temp | bool | Temperature exceedance flag |
| rule_wbc | bool | Wear-based criticality flag |
| clinical_flag | int | Count of active clinical rules per record |
| meets_clinical_rule | bool | True if any clinical rule is triggered |
| clinical_risk_score | bool | Composite clinical risk indicator |

### Temporal Features
| Column | Type | Description |
|---|---|---|
| seq_index | int | Sequential observation index |
| time_since_last_failure | str | Records elapsed since previous failure event |

---

## Usage Notes

- Primary target column: `Machine failure` (binary, 1 = failure)
- `Machine failure.1` and `id` are duplicates — drop before modeling
- `failure` mirrors `Machine failure` — used in clinical annotation 
  pipeline; verify consistency before use
- Clinical enhancement columns (`rule_*`, `clinical_flag`, 
  `meets_clinical_rule`) encode target-adjacent information — 
  exclude from feature sets when evaluating purely mechanical 
  signal to avoid leakage
- The dataset is made openly available to support reproducibility 
  of the associated research framework

---

## Associated Research

This dataset supports the following work:

*Trust Before Accuracy: A Forensic Validation Framework for 
Predictive Maintenance AI in Critical Care*

---

## License

MIT License
