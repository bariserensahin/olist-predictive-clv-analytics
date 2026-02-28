# Olist's ECommerce: Probabilistic Customer Lifetime Value and Customer Segmentation

**Author:** Barış Eren Şahin

**Dataset:** https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce

## *Aha!* Moment — Moving Away From Churn

Usually, people begin with a churn model. I had that in mind too, but while looking through the data, I found something amazing:

Of all the customers of Olist, 96.96% only shopped with them once!

This fact alone completely destroys the assumptions behind using a churn (binary) model (Yes / No) because when 96.96% of your customers only buy once and leave, a churn model has no hope of telling you who will leave.

So I only focused on financially driven CLV with the goal being to find the 3% of customers who provide Olist with all of its revenue (because they are the only ones who will keep Olist alive).  

## How
Step 1 — Data Engineering (Only Essential Data)

Kaggle (Olist) has 9 tables. So I only used the essential tables from the Kaggle dataset for my building project: Orders, Payments, and Customers.

I used Pandas to join and group all 3 tables via INNER JOIN and GROUP BY SQL commands.

Then I performed some data clean up (i.e., delete all canceled orders and transactions = $0.00) which resulted in a clean transaction based master table that would work for all of my models. 

Step 2 — Probabilistic Modeling (Mathematical Analysis)

I used 2 primary models from the Lifetimes library for my model work ... 

BG/NBD — To develop a probability about how many times a customer will purchase within the next 6 months of being a customer.

## Does It Work? Validated:

I didn't just call up the library and hope it would come through. I did a couple of major validations:

- Assumption Check: The Gamma-Gamma Model assumes that there are no associations between frequency of purchase and total amount of purchase. I calculated the association to be -0.0006 which is almost zero. The statistical suppositions were held up perfectly.

- Back-testing (Time-Based Validation): In probabilistic analyses one does not do a simple random split of the data as in random 80/20 split. I held out the last 6 months as a "Holdout" period and trained the model on the previous 6 month time period. Upon comparison of the model's predictions against actual results, the two sets of actual and predicted data matched almost exactly -- no over-fitted models there.

## Business Insights: Value to the Business?

I segmented customers in 4 ways based upon their CLV score. There were a few key insights:

- VIPs are where the money is. Although I only have a small number of VIPs, they are expected to contribute $77.9% of all revenue generated over the next 6 months.

- The Strategy is to not waste money sending out coupon promotions to all customers. Therefore, the company should budget accordingly so that VIPs receive most of the promotion attention, while the majority of the remaining promotional budget is spent acquiring "high-value" customers.

## Limitations & What's Next:

When working with real data I found that all models have limitations.

The filter for repeat buyers confirms that only those customers will receive the gamma-gamma calculations since the results are irrelevant to the 97% of single purchase customers as they didn't complete two purchases.

While the probabilistic model (RFM) provides good predictions of post-purchase behavior, the model does not consider all features associated with the order or all influences such as whether an order is delayed or if there are good reviews for the product. Therefore, at this point I am going to treat customer orders as a time series to build either a deep learning model (RNN/LSTM) or a hybrid XGBoost model to attempt to identify additional useful behavioural data.
