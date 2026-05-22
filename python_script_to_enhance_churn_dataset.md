import pandas as pd
import numpy as np
from datetime import timedelta
import random

# Load dataset

df = pd.read_csv("Telco-Customer-Churn.csv")

# Generate JoinDate

start_date = pd.to_datetime("2019-01-01")
end_date = pd.to_datetime("2024-12-31")

df['JoinDate'] = pd.to_datetime(
np.random.randint(
start_date.value // 10**9,
end_date.value // 10**9,
len(df)
),
unit='s'
)

# Subscription Age

today = pd.to_datetime("2025-01-01")
df['SubscriptionAgeMonths'] = (
(today - df['JoinDate']).dt.days // 30
)

# LastActivityDate

df['LastActivityDate'] = df['JoinDate'] + pd.to_timedelta(
np.random.randint(30, 1800, len(df)),
unit='D'
)

# ChurnDate

df['ChurnDate'] = np.where(
df['Churn'] == 'Yes',
df['LastActivityDate'] + pd.to_timedelta(
np.random.randint(1, 60, len(df)),
unit='D'
),
pd.NaT
)

# Satisfaction Metrics

satisfaction = []
tickets = []
complaints = []
nps = []

for churn in df['Churn']:
if churn == 'Yes':
satisfaction.append(random.randint(1,5))
tickets.append(random.randint(5,15))
complaints.append(random.randint(2,5))
nps.append(random.randint(-100,20))
else:
satisfaction.append(random.randint(6,10))
tickets.append(random.randint(0,5))
complaints.append(random.randint(0,2))
nps.append(random.randint(20,100))

df['SatisfactionScore'] = satisfaction
df['SupportTickets'] = tickets
df['ComplaintCount'] = complaints
df['NPSScore'] = nps

# Customer Segment

conditions = [
(df['MonthlyCharges'] < 50),
(df['MonthlyCharges'] >= 50) & (df['MonthlyCharges'] <= 90),
(df['MonthlyCharges'] > 90)
]

segments = ['Basic', 'Standard', 'Premium']

df['CustomerSegment'] = np.select(conditions, segments)

# Save enhanced dataset

df.to_csv("Enhanced_Telco_Churn.csv", index=False)

print("Enhanced dataset created successfully!")
