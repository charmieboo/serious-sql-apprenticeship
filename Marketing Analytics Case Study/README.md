# 🎬 Marketing Analytics Case Study
![market](https://user-images.githubusercontent.com/117857989/223360293-3dd96298-fac1-47ef-add9-9890e55156ea.png)


## 📌 1.0 Overview

### Business Task

We (Leadership team) have been asked to support the Customer Analytics team at DVD Rental Co who have been tasked with **generating the necessary data points** required to populate specific parts of this first-ever **customer email campaign**.

### Business Deliverables

The Marketing team have shared with us a draft of the email they wish to send to their customers.

<img width="573" alt="image" src="https://user-images.githubusercontent.com/81607668/129348831-c64205f9-46f4-4182-83eb-0cbd12ba7e61.png">

We have summarized the data points to 7 parts.

Data Points | Email Item | What information do we need to find? | Flag Out to Marketing Team | 
----------- | ----------- | ------------------------------------ | -------------- |
1 & 4       | Top 2 Categories | Identify top 2 categories for each customer based off their past rental history. | - |
2 & 5       | Individual Customer Insights | For 1st category (2), identify total films watched, average comparison and percentile. For 2nd category (5), identify total films watched and proportion of films watched in percentage. | - |
3 & 6       | Category Film Recommendations | Identify 3 most popular films for each customer's top 2 categories that customer has not watched. | If customer does not have any film recommendations for either category. |
7, 8 &      | Favourite Actor Recommendation | Identify favourite actor with 3 film recommendations that have not been recommended. | If customer does not have at least 1 recommendation. |

### ER Diagram (Entity-Relationship)
![erm](https://user-images.githubusercontent.com/117857989/223359915-0667507f-c739-42cf-bdb6-ff7e5217b5c6.png)

***

### PEAR Template
* Problem: The main initiative is to share insights about each customer’s viewing behaviour to demonstrate DVD Rental Co’s relentless focus on customer experience.
* Exploration
* Analysis
* Report
