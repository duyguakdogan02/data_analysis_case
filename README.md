# 🎮 Vertigo Games – Data Analyst Case Study

This repository contains my complete solution to the Vertigo Games Data Analyst Case.  


# 🧩 Task 1 – A/B Test Simulation

We simulate 30 days of performance for Variant A and Variant B using:

- Daily installs
- Purchase ratio
- eCPM
- Ad impressions per DAU
- Retention (D1, D3, D7, D14)
- Average order value (AOV)

I modeled DAU, retention-based decay, daily revenue and cumulative revenue for both variants.

## ✔️ **My SQL Model**

Full SQL code could be found in Word doc. 

---

## 🔍 **Findings (Task 1)**

### **a) Which variant has the most DAU after Day 15?**  
**→ Variant A**  
Because the cumulative‑retention model used in the SQL causes Variant A’s higher early‑stage retention (D1–D7) to compound faster, producing higher DAU by Day 15.

### **b) Which variant earns more total money by Day 15?**  
**→ Variant A**  
Variant A generates more revenue because its estimated DAU is higher and it has higher ad impressions per DAU, which outweighs Variant B’s slightly higher purchase ratio.

### **c) Which variant earns more total money by Day 30?**  
**→ Variant B**  
With the SQL model’s cumulative retention summation, Variant A’s DAU advantage continues to grow over time, so total revenue remains higher through Day 30.

### **d) Add a 10-day sale from Day 15 (purchase ratio +1%) — who wins?**  
**→ Variant B becomes the winner.**  
Both variants get the same uplift in purchase ratio, but Variant A already has consistently higher DAU in the model, so the sale increases A’s revenue more.

---

# 🧩 Task 2 – Exploratory Data Analysis

For Task 2, I generated country-level traffic, created user segments based on match engagement, and analyzed monetization behaviors.

### ✔️ **SQL: Country Traffic Table**

```sql
create table `games_v2.country_traffic` as
select  
  event_date,
  country,
  platform,
  sum(iap_revenue) as iap_revenue,
  sum(ad_revenue) as ad_revenue,
  sum(iap_revenue + ad_revenue) as gmv,
  sum(total_session_count) as session_count,
  count(distinct user_id) as user_count
from `games_v2.Vertigo`
where event_date between '2024-02-15' and '2024-03-15'
group by all;

✔️ Player Segmentation

I segmented paying users based on how many matches they played before making a purchase.

Segments:

    0-match purchasers

    1–3 match purchasers

    4–9 match purchasers

    10+ match purchasers

case
    when match_end_count = 0 then 'purchased_after_no_match'
    when match_end_count >=1 and match_end_count <4  then 'purchased_after_1-3_match'
    when match_end_count >=4 and match_end_count <10 then 'purchased_after_4-9_match'
    when match_end_count >=10 then 'purchased_after_10+_match'
end as segment

📊 Visualizations & Insights

Dashboard:
https://lookerstudio.google.com/reporting/ffed6080-f0a6-4ba8-9457-242119d78e63