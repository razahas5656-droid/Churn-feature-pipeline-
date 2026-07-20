# Churn-feature-pipeline-
Telecom Churn Feature Pipeline

M-Tech Internship Program 2026 — Week 3 Task
Name: Muhammad Hasnain Raza | Reg No: Mtech-DS26034
Level: Advanced | GUI Tool: Tkinter | Concept: Churn Feature Engineering


What It Does

This app builds a leakage-safe feature set for churn prediction out of raw
telecom customer data (tenure, 6 months of usage history, 6 months of complaint
history). It engineers behavioral features — tenure buckets, usage decline, and
complaint velocity — encodes categorical fields, scales numeric fields, and
visualizes which factors most strongly drive churn, all inside a Tkinter GUI.


Why "Leakage-Safe"?

Data leakage happens when information that wouldn't actually be available at
prediction time (usually anything derived from or correlated with the future
outcome) accidentally ends up in a feature. This pipeline avoids that by:



Never using the Churn column to build any feature. It is only carried along separately as the label.

Only using the 6-month history window (usage_m1..m6, complaints_m1..m6) that would have been known before the churn outcome happened.

Fitting the StandardScaler on the training split only, then applying it to the test split — fitting on the full dataset (train+test together) would leak the test set's distribution into training.


Files


generate_raw_data.py — creates raw_telecom_data.csv, a synthetic raw dataset with 6 months of usage + complaint history per customer (replace with real telecom data if you have it)

feature_pipeline.py — the core leakage-safe feature engineering logic (engineer_features(), scale_features())

pipeline_app.py — the Tkinter GUI: load data → run pipeline → preview engineered table → view churn-driver charts

raw_telecom_data.csv — raw input dataset

engineered_features.csv — output of the pipeline


Engineered Features

Feature	What it captures
tenure_bucket	Customer lifecycle stage (0-6m, 6-12m, 12-24m, 24m+)
avg_usage_recent / avg_usage_earlier	Usage in last 3 vs previous 3 months
usage_decline_pct	% drop in usage, recent vs earlier
usage_trend_slope	Linear trend slope of usage over 6 months
complaint_velocity	Complaints per month, recent 3-month window
complaint_velocity_change	Change in complaint rate, recent vs earlier
Contract_enc, PaymentMethod_enc, tenure_bucket_enc	Label-encoded categoricals

How to Run


Install requirements:

pip install pandas numpy scikit-learn matplotlib

Generate the raw dataset (skip if you already have raw_telecom_data.csv):

python generate_raw_data.py

(Optional) Run the pipeline standalone from the command line:

python feature_pipeline.py

Launch the GUI:

python pipeline_app.py

In the app:

Confirm the file path (raw_telecom_data.csv) or Browse to another file
Click Run Feature Pipeline — engineered features appear in the table and are saved to engineered_features.csv
Click Show Churn Driver Charts to see:
Churn rate by tenure bucket
Usage decline % vs churn (boxplot)
Complaint velocity vs churn (boxplot)


Screenshot



How AI Was Used

I used Claude to help me understand what "leakage-safe" feature engineering
means and to design the tenure-bucket, usage-decline, and complaint-velocity
features. I reviewed the code myself, especially the part where the scaler
is fit only on the training split, to understand why fitting it on the full
dataset would be a leakage mistake.

