# Google Play Store Apps Data Analysis
## Project Review
Exploring different app categories,  ratings and their review to identify popular categories and emerging trends, to identify the potential success of a new app based on weather it is paid or free  and which category have the most competition than others.  Market segmentation is based onn factors like app type (free vs. paid), ratings, installs, user reviews.  and contentratings.

### Data Sources
Google Playstore Apps is a dataset iwth all the apps informationn and it can be fount on kaggle googleplaystore.csv .

### tools
- SQL Lite
- Tableau
  
### Exploratory Data Analysis  
The EDA involved exploring the Google Play Store Apps to establish the relationship between, installation, ratings, reviews and content rating to discover the opportinuties of a new app being succcesful

1. Establishing Total Apps and categories available
2. Finding Category with the most apps
3. Which Apps with the highest number of Reviews?
4. Finding out which apps  have to top reviews and wether they are paid or free
5. Which are the top rated free apps
6. Apps with the highest rating
7. What are the average rating by category
8. Top installations by category
9. Content rating by category
10. What is the general Sentiments of the top apps
11. Sentiment Review by Category

### Data Analysis

~~~sql
SELECT count("DISTINCT App") AS TotalApps,count(DISTINCT "Category")TotalAppCategory
FROM googleplaystore

SELECT Category, count(*) AppstotalCount
FROM googleplaystore
GROUP BY Category
ORDER BY AppsTotalCount DESC
  
SELECT App, max(Reviews) AS HighestReviews
FROM googleplaystore
GROUP BY App
ORDER BY App DESC
LIMIT 20
    
SELECT Rating,App, max(Reviews)AS TotalReviews, Type
FROM googleplaystore
WHERE Rating >4
GROUP BY Reviews, Type
ORDER BY Reviews DESC
   
SELECT App, Category, Reviews, Rating
FROM googleplaystore
WHERE Type = "free" AND Rating != "NaN"
ORDER BY Rating DESC 
   
SELECT MAX(Reviews) AS MaxReviews, Category, MAX(Type) AS MaxType, App
FROM googleplaystore
WHERE Type IN ("Paid","Free")
GROUP BY Category
ORDER BY MaxReviews DESC;

SELECT Type, count(Type) TotalAPPs, Category,Rating
FROM googleplaystore
GROUP BY Type,Category,Rating
ORDER by Rating  DESC

SELECT Category, round(avg(CAST(rating AS FLOAT)),2) AS AverageRating
FROM googleplaystore GROUP BY Category
ORDER BY AverageRating DESC
 
SELECT Category, sum(CAST(replace(Installs,',','')AS INTEGER)) AS TotalInstalls
FROM googleplaystore
GROUP BY Category
ORDER BY TotalInstalls DESC
LIMIT 10
 
SELECT ContentRating,count(App) AS TotalAPP,round(avg(Rating),2) AS AverageRatings,Type, Category
FROM googleplaystore
GROUP BY ContentRating,Type
ORDER BY TotalAPP DESC,Type
LIMIT 10

SELECT  DISTINCT (gp.App),gp.Rating,gr.Sentiment,gp.Reviews,gp.Category,round(gr.Sentiment_Polarity,2),gp.Type
FROM googleplaystore AS gp
INNER JOIN googleplaystore_user_reviews AS gr
ON gp.App = gr.App
WHERE gr.Sentiment IN ("Positive","Neutral","Negative") 
GROUP BY gp.Reviews,gp.Type
ORDER BY gp.Reviews DESC,gr.Sentiment_Polarity DESC
LIMIT 1000

SELECT gp.Category,gr.Sentiment,count(*) AS TotalSentiment
FROM googleplaystore AS gp
INNER JOIN googleplaystore_user_reviews AS gr
ON gp.App = gr.App
WHERE gr.Sentiment IN ("Positive","Neutral","Negative")
GROUP BY gp.Category,gr.Sentiment
ORDER BY TotalSentiment DESC

### Results/Findings

### Recommandation

### Limitations
### References


