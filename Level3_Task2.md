Task 2: Building Dashboards with Power BI 
Analysis Summary

Internship: Codveda Technology —Data Analytics Track Level: 3 (Advanced) Dataset: churn_for_powerbi.csv (2,666 telecom customer records — same source data as Task 1) Tool: Power BI Desktop

Objective

Build an interactive Power BI dashboard to explore telecom customer churn visually, allowing filtering by state, international plan status, and customer service call volume — surfacing the same churn drivers identified analytically in Task 1's classification model.

What Was Built

The dashboard, titled "Telecom Customer Churn Dashboard," consists of a single overview page with two headline KPI cards, four supporting charts, and three interactive filters.

Show Image

KPI Cards
Total Customers — a card showing the count of customers in the current filter context.
Churn Rate — a card showing the percentage of those customers who churned.
Charts
Chart	Type	What it shows
Churn Rate by State	Horizontal bar	Compares churn rate across individual states
Total Customers by Churn	Donut chart	Overall split between churned and retained customers
Total Customers by International Plan and Churn	Clustered column	Churn broken down by international plan subscription
Churn, Total Day Minutes and Total Day Charge	Scatter plot	Relationship between daytime usage, charge, and churn status
Churn Rate by Customer Service Calls	Column chart	Churn rate at each level of customer service call volume
Interactive Filters (Slicers)
International plan — checkbox slicer (Yes/No)
Customer service calls — range slider (0–9)
State — multi-select list, all 51 states available

All charts are cross-filtered: selecting a state or adjusting a slicer instantly updates every visual on the page, letting a viewer drill into specific customer segments without touching the underlying data.

Key Findings (From the Filtered View Shown)

The screenshot above reflects one specific filter combination applied during testing: states AK, AL, and AR only, International plan = No, and Customer service calls between 0 and 9. Under this filter:

Total customers in view: 137
Churn rate: 10.22% (14 churned out of 137)

Within this filtered subset:

AR has the highest churn rate of the three states shown (~19%), notably above AL and AK.
The Total Day Minutes vs. Total Day Charge scatter plot shows a clean, near-perfectly linear relationship — expected, since charge is calculated directly from minutes used. Churned customers (dark blue) cluster toward the higher end of both minutes and charge, reinforcing that heavy daytime users are more likely to churn.
Churn Rate by Customer Service Calls is the most striking chart: churn rate stays low (under 10%) for customers with 0–3 service calls, then rises sharply — jumping to roughly 50% at 4 calls and climbing further at 6+ calls, eventually approaching 100%. This visually confirms the strongest predictor identified by the Task 1 Random Forest model.
With the International plan filter locked to "No" in this view, the dashboard isolates churn behavior for non-plan holders specifically — useful for comparing against plan holders by simply toggling the slicer.
Dashboard vs. Model Findings

This dashboard was designed to visually validate the top features identified by the Task 1 classification model:

Task 1 Model Finding	Dashboard Confirmation
Total day charge/minutes = top predictor	Scatter plot shows churners concentrated at higher usage levels
Customer service calls = strong predictor	Bar chart shows churn rate rising sharply after 3–4 calls
International plan = notable predictor	Slicer allows direct before/after comparison of churn rate with plan toggled on/off

Both the model and the dashboard independently point to the same conclusion: customer service call frequency is the clearest early-warning signal for churn, followed by daytime usage intensity.

Business Takeaway

A retention team using this dashboard could immediately filter down to "customers with 4+ service calls" to generate a target list for proactive outreach — since the churn rate jumps dramatically at that threshold. Combined with the Task 1 model's precision (93%) for flagging likely churners, this dashboard turns a static prediction into an explorable, shareable tool that non-technical stakeholders can use on their own.

How to Reproduce
Open churn_for_powerbi.csv in Power BI Desktop.
Follow the build steps in TASK2_POWERBI_GUIDE.md (same repo) to recreate the KPI cards, four charts, and three slicers.
Publish to the Power BI Service and share the report link alongside this documentation.
