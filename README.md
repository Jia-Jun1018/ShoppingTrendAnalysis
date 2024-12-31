# Portfolio
# Shopping Trend Analysis Using SQL and Tableau
# SQL Data Transforming
--1. Average Purchase Amount by Age Group
SELECT 
    CASE 
        WHEN Age BETWEEN 18 AND 19 THEN '18-19'
        WHEN Age BETWEEN 20 AND 29 THEN '20-29'
        WHEN Age BETWEEN 30 AND 39 THEN '30-39'
        WHEN Age BETWEEN 40 AND 49 THEN '40-49'
        WHEN Age BETWEEN 50 AND 59 THEN '50-59'
        WHEN Age BETWEEN 60 AND 70 THEN '60-70'
        ELSE 'Out of Range'
    END AS Age_Group,
    COUNT(Customer_ID) AS Total_Customers,
    AVG(Purchase_Amount_USD) AS Avg_Purchase_Amount,
    SUM(Purchase_Amount_USD) AS Total_Spend
FROM shopping_trends
GROUP BY 
    CASE 
        WHEN Age BETWEEN 18 AND 19 THEN '18-19'
        WHEN Age BETWEEN 20 AND 29 THEN '20-29'
        WHEN Age BETWEEN 30 AND 39 THEN '30-39'
        WHEN Age BETWEEN 40 AND 49 THEN '40-49'
        WHEN Age BETWEEN 50 AND 59 THEN '50-59'
        WHEN Age BETWEEN 60 AND 970 THEN '60-70'
        ELSE 'Out of Range'
    END
ORDER BY Age_Group;

--2. Purchases by Subscription Status
SELECT Subscription_status,
	SUM(CASE WHEN subscription_status = 'Yes' THEN 1 Else 0 END) AS Subscribed_Customers,
	SUM(CASE WHEN subscription_status = 'No' THEN 1 Else 0 END) AS Not_Subscribed_Customers,
	SUM(purchase_amount_USD) AS Purcase_Amount
FROM shopping_trends
GROUP BY Subscription_Status;

--3. Preferred Payment Method Distribution
SELECT 
   Preferred_Payment_Method,
    COUNT(Preferred_Payment_Method) AS Total_number,
    RANK() OVER (ORDER BY COUNT(Preferred_Payment_method) DESC) AS Rank
FROM shopping_trends
GROUP BY Preferred_Payment_Method
ORDER BY Rank;

--4. Top 5 Purchased Categories by Season
SELECT Season, Category, COUNT(Category) AS Total_number
FROM shopping_trends
GROUP BY Season, Category
ORDER BY Season, Total_number DESC;

--5. Average Review Rating by Category
SELECT Category, CAST(AVG(review_rating) AS decimal (10,2)) AS Average_Rating
FROM shopping_trends
GROUP BY Category;

--6. Purchase Amount by Location
SELECT Location, COUNT(Customer_id) AS Total_customer, SUM(purchase_amount_usd) AS Total_Purchase_Amount
FROM shopping_trends
GROUP BY location
ORDER BY Total_Purchase_Amount DESC;

--7. Shipping Type Usage by Region
SELECT Location, Shipping_Type, COUNT(Customer_ID) AS Total_number
FROM shopping_trends
GROUP BY location, Shipping_Type
ORDER BY Location, Total_number DESC;

--8. Percentage of Purchases with Discounts Applied
SELECT CAST(COUNT(CASE WHEN Discount_applied = 'Yes' THEN 1 END) *100.0/ COUNT(customer_id) as decimal (10,0)) AS Percentage_of_Discounts_Applied,
	CAST(COUNT(CASE WHEN Discount_applied = 'No' THEN 1 END) *100.0/ COUNT(customer_id)as decimal (10,0)) AS Percentage_of_No_Discounts_Applied
FROM shopping_trends;

--9. Revenue Impact of Promo Code Usage
SELECT 
    SUM(CASE WHEN Promo_Code_Used = 'Yes' THEN Purchase_Amount_USD ELSE 0 END) AS Revenue_With_Promo_Code,
    SUM(CASE WHEN Promo_Code_Used = 'No' THEN Purchase_Amount_USD ELSE 0 END) AS Revenue_Without_Promo_Code
FROM shopping_trends;

--10. Identify Loyal Customer
SELECT Age, Gender, COUNT(Gender) AS Total_number
FROM shopping_trends
WHERE Previous_Purchases > 10 AND Frequency_of_Purchases IN('Bi-Weekly', 'Fortnightly', 'Monthly', 'Weekly')
GROUP BY age, gender
ORDER BY Gender DESC, Total_number DESC;

--11. Average Previous Purchases by Review Rating
SELECT CAST(Review_Rating as decimal (10,1)) AS Review_Rating,
    AVG(Previous_Purchases) AS Avg_Previous_Purchases
FROM shopping_trends
GROUP BY Review_Rating
ORDER BY Review_Rating;

--12. Seasonal Average Purchase Amount by Category
SELECT Season, Category,
    AVG(Purchase_Amount_USD) AS Avg_Purchase_Amount
FROM shopping_trends
GROUP BY Season, Category
ORDER BY Season, Category;

![image](https://github.com/user-attachments/assets/df814b5a-b662-48f6-bfcb-67577a3b6377)
