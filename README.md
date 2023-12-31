# Apps_on_Google_PlayStore.
This project involves analysis of all the apps on Google PlayStore using SQL as the major tools for analysis.

## TITLE OF PROJECT
Google Playstore Apps

## PROJECT OVERVIEW
The analysis of Google Play Store apps offered a profound glimpse into the intricate landscape of app distribution, user preferences, and market dynamics. Delving into a diverse array of categories, the assessment spanned app counts, reviews, pricing strategies, content ratings, and installations. Key insights emerged, highlighting Family as the most populated category, boasting 1943 apps, followed closely by Games and Tools. Notably, popular apps like Facebook, WhatsApp, Instagram, and Messenger garnered the highest reviews, reflecting user engagement and preferences. Moreover, the majority of apps were free, indicating user inclination towards cost-free applications, while cheaper apps tended to attract higher installations. Understanding content ratings revealed a dominance of apps categorized as "Everyone," emphasizing inclusivity. Trends in app sizes across different genres and top categories by installations provided valuable insights into user behavior and preferences. The analysis culminated in actionable recommendations focusing on tailored category-specific strategies, pricing optimizations, and user-centric app development. This comprehensive analysis equips developers and stakeholders with vital insights to refine strategies, enhance user experiences, and make informed decisions in the dynamic app market landscape.

## DATA SOURCE
Kaggle

## TOOLS
- Excel
- SQL

## DATA CLEANING
Data cleaning was first done on Excel before the file was exported to SQL for proper analysis. The sizes of the apps had to be modified into numbers for proper analysis and also inconsistencies were eliminated for proper analysis.

## TYPE OF ANALYSIS
The analysis of the Google Play Store apps encompassed several types of data analysis methodologies:

1. **Descriptive Analysis:** Utilized to summarize and describe the dataset, including app counts per category, reviews, pricing distributions, content ratings, and installations.

2. **Trend Analysis:** Identified trends across various categories, such as app sizes, pricing patterns, content ratings, and user preferences based on installations and reviews.

3. **Correlation Analysis:** Explored relationships and correlations between different app attributes, such as app pricing versus installations, content ratings versus app categories, and app sizes versus categories.

4. **Categorical Analysis:** Evaluated categorical data including app categories, content ratings, and pricing to understand distributions and prevalence within each category.

5. **Statistical Analysis:** Leveraged statistics to highlight patterns, outliers, and averages within the dataset, such as average app ratings, the number of apps in each category, and average installations.

6. **Comparative Analysis:** Conducted comparisons across various categories, including installations, pricing, and content ratings, to identify top-performing categories and significant trends.

7. **User Behavior Analysis:** Examined user behavior patterns by analyzing installations, reviews, and content ratings to discern user preferences and behaviors across different app categories.



## DATA ANALYSIS
By employing these diverse analytical approaches, the analysis comprehensively dissected the Google Play Store app dataset, offering nuanced insights into app distribution, user behaviors, pricing strategies, and category-specific trends.
- **App Distribution by Category:** Family emerged as the category with the highest number of apps (1943), followed by Games (1121), Tools (843), down to Beauty with the fewest apps (53).
- **Most Reviewed Apps:** Notable apps such as Facebook, WhatsApp, Instagram, Messenger, and Clash of Clans received the highest number of reviews.
- **App Pricing and Installations:** The majority of apps (9593) were found to be free, while cheaper apps tended to have higher installations compared to higher-priced ones.
- **Content Ratings and Categories:** The "Everyone" content rating constituted the highest number of apps (8383), followed by Teens (1146), Mature 17+ (446), Adults Only 18+ (3), and Unrated (2).
- **App Size and Categories:** Apps in the Tool genre exhibited the largest sizes, followed by Entertainment, Education, Business, and Medical categories.
- **Top Categories by Installs:** Dominance of specific apps in their respective categories like Facebook, Gmail, Google Chrome, and YouTube in terms of installations.

```sql
--How many categories of apps are on Google Playstore and what are they?
SELECT  DISTINCT category,
		COUNT(category) AS NumberOfCategory
FROM googleplaystores
GROUP BY category
ORDER BY NumberOfCategory DESC;

--What are the 10 best apps based on ratings?
SELECT TOP 10 * 
FROM googleplaystores
ORDER BY Rating DESC;

--What are the 10 best apps based on their number of reviews?
SELECT TOP 10 * 
FROM googleplaystores
ORDER BY Reviews DESC;

--What is the ratio of free apps to the paid apps?
SELECT COUNT(Type) AS Freeapps,
		(SELECT COUNT(Type)  
		FROM googleplaystores 
		WHERE Type = 'Paid') AS Paidapps
FROM googleplaystores
WHERE Type = 'Free';

--How many apps are there for each content rating available on Google Playstore?
SELECT DISTINCT [Content Rating],
		COUNT(app) AS rating_count
FROM googleplaystores
GROUP BY [Content Rating]
ORDER BY rating_count DESC;

--Are Google Playstore apps downloaded based on suitability or price?
SELECT COUNT(installs) AS Numberofinstalls, 
		type
FROM googleplaystores
GROUP BY type;

--What is the distribution of app prices?
SELECT price, COUNT(app) AS price_count
FROM googleplaystores
GROUP BY price
ORDER BY price_count DESC;

--Which categories have the highest average price?
SELECT category,AVG(price) AS avg_price
FROM googleplaystores
GROUP BY category
ORDER BY avg_price DESC;

--What are the most common genre among apps?
SELECT genres, COUNT(app) AS size_count
FROM googleplaystores
GROUP BY genres
ORDER BY size_count DESC;

--What is the size distribution of apps?
SELECT [size(kb)], COUNT(app) AS size_count
FROM googleplaystores
GROUP BY [size(kb)]
ORDER BY size_count DESC;

--What are the top-installed apps in each category?
SELECT  category,App, MAX(installs) as max_installs
FROM googleplaystores
GROUP BY category,App
ORDER BY max_installs DESC 

--What are the top-rated apps that are also free to download?
SELECT app,rating
FROM googleplaystores
WHERE type = 'Free' AND rating = 5.0
ORDER BY rating DESC;

--What are the top rated apps for each content rating?
SELECT [Content Rating],app, MAX(rating) AS max_rating
FROM googleplaystores
WHERE rating = 5.0 
GROUP BY [Content Rating],app
ORDER BY max_rating DESC

--Which Android versions are most commonly used among apps?
SELECT [Android Ver], COUNT(app) AS version_count
FROM googleplaystores
GROUP BY [Android Ver]
ORDER BY version_count DESC;

--Which categories have the highest number of reviews on average?
SELECT category, AVG(Reviews) AS average_reviews
FROM googleplaystores
GROUP BY category
ORDER BY average_reviews DESC;

--What is the average size of apps in each category?
SELECT category,AVG([size(kb)]) AS avg_size
FROM googleplaystores
GROUP BY category
ORDER BY avg_size DESC;

--What is the corellation between the number of installs and ratings?
SELECT rating,AVG(installs) AS avg_install
FROM googleplaystores
GROUP BY rating
ORDER BY rating DESC,avg_install DESC;

--What is the average size of apps in each category?
SELECT category, AVG([size(KB)]) AS avg_size_kb
FROM googleplaystores
GROUP BY category
ORDER BY avg_size_kb;

-- How many installs do paid apps with a rating above 4.0 have?
SELECT SUM(installs) AS total_installs
FROM googleplaystores
WHERE type = 'Paid' AND rating > 4.0;

--Which categories have apps with prices greater than $50?
SELECT category, app, price
FROM googleplaystores
WHERE price > 50;

--How many apps have a size less than 10MB and a rating above 4.0?
SELECT COUNT(*) AS count_small_size_high_rating
FROM googleplaystores
WHERE [Size(kb)] < 10000 AND rating > 4.0;


--Which apps belong to the 'Education' category and have a content rating of 'Everyone'?
SELECT app, [Content Rating]
FROM googleplaystores
WHERE category = 'Education' AND [Content Rating] = 'Everyone';


--What is the highest price of paid apps in each category?
SELECT category, MAX(price) AS max_price
FROM googleplaystores
WHERE type = 'Paid'
GROUP BY category 
ORDER BY max_price DESC;


--How many apps in each category have a size greater than 50MB and support Android version 9.0 and above?
SELECT category, COUNT(*) AS count_apps
FROM googleplaystores
WHERE [Size(kb)] > 50000 AND [android ver] >= '9.0'
GROUP BY category;

--How many reviews do apps with a price greater than $100 have?
SELECT SUM(reviews) AS total_reviews
FROM googleplaystores
WHERE price > 100;

--What is the average rating of apps that belong to multiple genres?
SELECT AVG(rating) AS avg_rating
FROM googleplaystores
WHERE genres LIKE '%,%';


--What are the apps that were last updated before '2022-01-01'?
SELECT app, [last updated]
FROM googleplaystores
WHERE [Last Updated] NOT LIKE '%2022';


--How many apps have 'Teen' content rating and were updated in 2021?
SELECT COUNT(*) AS count_teen_updated_2021
FROM googleplaystores
WHERE [Content Rating] = 'Teen' AND [Last Updated] LIKE '%2021';


--How many apps were last updated in the year 2017 and have a content rating of 'Mature 17+'?
SELECT COUNT(*) AS count_apps
FROM googleplaystores
WHERE [Content Rating] = 'Mature 17+' AND [Last Updated] LIKE '%2017';

SELECT AVG(installs) AS avg_installs
FROM googleplaystores
WHERE category = 'Health & Fitness';

--What are the top 5 categories with the highest average rating and more than 100 apps? (Using Common Table Expression and Subquery)
WITH CategoryRating AS (
    SELECT category, AVG(rating) AS avg_rating, COUNT(*) AS app_count
    FROM googleplaystores
    GROUP BY category
)
SELECT TOP 5 category, avg_rating
FROM CategoryRating
WHERE app_count > 100
ORDER BY avg_rating DESC

--How many apps in each category have a 'Varies with device' size entry? (Using LIKE statement)
SELECT category, COUNT(*) AS count_varies_with_device
FROM googleplaystores
WHERE size LIKE 'Varies with device'
GROUP BY category;

--What is the total number of installs for apps with a content rating of 'Everyone' in each category? (Using SUM and CASE statement)
SELECT category, 
       SUM(CASE WHEN [content rating] = 'Everyone' THEN installs ELSE 0 END) AS total_installs_everyone
FROM googleplaystores
GROUP BY category
ORDER BY total_installs_everyone DESC;


--What is the percentage distribution of apps across different content ratings? (Using CASE statement)
SELECT [Content rating], 
       COUNT(*) AS count,
       ROUND(COUNT(*) * 100.0 / (SELECT COUNT(*) FROM googleplaystores), 2) AS percentage
FROM googleplaystores
GROUP BY [Content rating];
```

## RESULTS
Certainly! Here is a more detailed breakdown of the results from the Google Play Store app analysis:

1. **App Distribution by Category**:
   - Family category has the highest number of apps (1943), followed by Games (1121), and Tools (843), with Beauty having the fewest apps (53).

2. **Most Reviewed Apps**:
   - Apps with the most reviews are Facebook, WhatsApp, Instagram, Messenger, and Clash of Clans.

3. **Free and Paid Apps**:
   - 9593 apps are free, while 765 apps are paid on Google Play Store.

4. **Content Ratings**:
   - Everyone: 8383 apps
   - Teens: 1146 apps
   - Mature 17+: 446 apps
   - Adults Only 18+: 3 apps
   - Unrated: 2 apps

5. **Pricing and Installations**:
   - Users tend to install cheaper apps more frequently than higher-priced ones.
   - Finance, Lifestyle, and Medical categories have more priced apps, while Comics, House and Beauty categories don't have paid apps.

6. **App Size**:
   - Tool genre has the largest-sized apps, followed by Entertainment, Education, Business, and Medical.
   - Most apps are below 200MB.

7. **Top Installed Apps by Category**:
   - Facebook (Social), Gmail, and Google Chrome (Communication), Google (Tools), YouTube (Video Players), Subway Surfers (Games), etc.

8. **App Ratings**:
   - About 244 apps have a rating of 5.0.
   - Apps with a rating above 4.0 and are paid have total installs of over 6.7 million.

9. **Price Ranges and Reviews**:
   - Categories with prices over $50: Lifestyle, Family, Finance, Business, Medical, Events, and Productivity.
   - Apps priced over $100 have total reviews of about 9049.

10. **Content Ratings and App Sizes**:
   - Approximately 1873 apps have a size less than 10MB and a rating above 4.0.
   - Many apps in the Education category have a content rating for "Everyone."
   - Lifestyle category has the highest-priced app at $400, followed by Family and Finance at $399.99 each, and Medical at $200.

11. **Top Categories with High Average Ratings**:
   - Education, Entertainment, Game, Photography, and Shopping categories have the highest average ratings among categories with more than 100 apps.

12. **Installations Based on Content Rating and Category**:
   - Communication with "Everyone" content rating has over 22 billion plus installs, followed by Game with 18 billion plus installs, and Productivity with 12 billion plus installs.

This detailed analysis offers comprehensive insights into app distribution, user behavior, pricing strategies, content ratings, and app performance across different categories in the Google Play Store.



## RECOMMENDATIONS
- **Category-Specific Strategies:** Utilize insights from top-rated categories to refine app development strategies and enhance user engagement.
- **Pricing and Size Optimization:** Tailor pricing strategies and app sizes based on category-specific trends to optimize user experience and monetization.
- **User Behavior Analysis:** Continuously analyze user preferences and behaviors to adapt app features and content to user expectations and demands.
The analysis provided a comprehensive understanding of app market trends, user preferences, and category-specific dynamics within the Google Play Store. This information empowers app developers and stakeholders to make informed decisions for app development, marketing, and user engagement strategies.


