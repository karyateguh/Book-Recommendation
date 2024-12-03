# Book-Recommendation

# Project Domain 

In a world where millions of books are published and countless readers seek their next favorite story, finding the perfect match between a book and a reader has become increasingly challenging. The advent of digital platforms has expanded access to books, but with this access comes a new problem: information overload. Readers are often overwhelmed by the sheer volume of options, making it difficult to discover books that truly resonate with their tastes.

This is where recommendation systems play a crucial role. By analyzing user behavior and preferences, these systems can suggest books that align with individual interests, offering a personalized reading experience. Such systems not only improve user satisfaction but also have a significant impact on the publishing and retail industries, driving sales and reader engagement.

The Book Recommendation System project aims to address this challenge by leveraging data from the Book-Crossing dataset, a widely studied resource in recommender system research. The dataset offers a wealth of information, including user demographics, book metadata, and user ratings, making it an excellent foundation for building and evaluating recommendation algorithms.

**Importance of the Project** 

Developing an effective recommendation system is essential for both users and businesses:

For Readers: A personalized recommendation system acts like a digital librarian, helping users discover books that match their unique preferences. This enhances the reading experience, reduces search effort, and introduces users to books they might not have found otherwise.

For Businesses: Book retailers, publishers, and libraries can benefit from improved customer retention and sales through tailored recommendations. By understanding user preferences, businesses can optimize inventory, highlight popular titles, and promote lesser-known works to the right audience.

For Research: The project provides an opportunity to evaluate and refine various recommendation algorithms, contributing to the broader field of recommender systems. Insights from this research can be applied to other domains, such as movie, music, or product recommendations.

**Supporting Research**

The significance of recommendation systems is well-documented in academic literature. For instance:

Ricci et al. (2015) highlighted the importance of collaborative filtering techniques in delivering accurate and scalable recommendations.

Koren, Bell, and Volinsky (2009) emphasized the value of matrix factorization methods, such as SVD and SVD++, for improving prediction accuracy in sparse datasets.

George and Merugu (2005) introduced co-clustering methods as a robust approach for handling the "cold start" problem in sparse data environments.

These studies underscore the relevance of developing sophisticated algorithms tailored to the unique challenges of recommendation systems, such as sparse data, long-tail distributions, and balancing precision and recall.

This project aims to overcome these challenges by systematically cleaning, preprocessing, and evaluating the data using state-of-the-art recommendation algorithms, ultimately delivering a system that balances accuracy and user satisfaction.

By bridging the gap between readers and books, this project aspires to transform the way people discover literature, enriching their lives one recommendation at a time.

# Business Understanding

Problem Statements:

1. Which recommendation model offers the most accurate predictions, and why?
2. How relevant are the top ten recommendations provided by the best-performing model?

Goals:

1. To identify the most accurate recommendation model and understand its relevance to users.
2. To evaluate the top ten recommendations from the best model in terms of user relevance and engagement.



Solutions

Designing a recommendation system is like creating a personal librarian for each user—one that knows their preferences and introduces them to books they'll love. In this project, we evaluated four different algorithms: SVD, SVD++, NMF, and CoClustering, comparing their strengths and weaknesses to select the most suitable model for a book recommendation system. The evaluations were conducted based on metrics like RMSE, precision, and recall, which are widely used in recommendation system research (Koren et al., 2009; Ricci et al., 2011).

# Data Understanding

The Book-Crossing dataset, curated by Cai-Nicolas Ziegler and colleagues, offers a fascinating glimpse into the world of readers, books, and their ratings. It has been widely utilized in research on recommender systems, reflecting its richness and potential for insights. Let’s take a journey through the details of this dataset.


The dataset is organized into three interconnected files:

* Books

Total entries: 271,379

Columns:

ISBN (unique identifier for each book)

Title (book's name)

Author (author's name)

Year (year of publication)

Publisher (publisher's name)

Data tidbit: While the dataset is largely complete, there are two missing entries in both the Author and Publisher columns. The Year column contains no missing values, ensuring clarity for temporal analysis.

* Users

Total entries: 278,859

Columns:

User-ID (unique identifier for each user)

Age (user’s age)

Insights: Over 168,000 users have provided their age, but approximately 110,232 rows are missing this information, presenting challenges for demographic analysis.

* Ratings

Total entries: 1,149,780

Columns:

User-ID

ISBN

Rating (user’s rating for a book)

Highlights: Every row is complete, offering a robust foundation for studying user preferences and trends. 



**Sparse Participation**

Among the 278,859 users, only 99,053 have rated at least one book. The engagement decreases significantly as rating frequency increases:
43,385 users rated at least 2 books.
A mere 12,306 users rated 10 or more books.

**Book Ratings**

The dataset contains 271,379 books, but only 270,171 have been rated at least once. Delving deeper, the numbers drop:
124,513 books received at least 2 ratings.
A modest 17,480 books earned 10 or more ratings.
These figures reflect the well-known "long tail" phenomenon in book ratings, where a small number of books garner the majority of attention while many remain relatively obscure.

**Structural Integrity**

The dataset boasts an impressive level of cleanliness:

*Duplicate rows?*
One. Whether in the Books, Users, or Ratings datasets, every row is unique.

*Encoding adjustments have been made (from ISO-8859-1 to UTF-8)*, ensuring accessibility and consistency.


*A Data Detective’s Challenge: Missing and Ambiguous Information*

While most columns are complete, gaps in the Age field (from the Users dataset) and sparse ratings per user/book pose analytical hurdles. Moreover, the absence of a standardized location field (intentionally removed) may limit geographic insights.


The dataset is also can be downloaded from [kaggle](https://www.kaggle.com/datasets/somnambwl/bookcrossing-dataset)

**For further details**, refer to the dataset's original publication:

Cai-Nicolas Ziegler, Sean M. McNee, Joseph A. Konstan, Georg Lausen. Proceedings of the 14th International World Wide Web Conference (WWW '05), May 10-14, 2005, Chiba, Japan.


# EDA

## Count unique users and books in the ratings dataset 

Number of unique users: 105283

Number of unique books: 340556

## Rating Diistribution

![Distribution of Rating](https://raw.githubusercontent.com/karyateguh/Book-Recommendation/master/image/1.%20Distribution%20of%20Rating.png)


## Top 10 Most Rated Books

![Top 10 Most Rated Books](https://raw.githubusercontent.com/karyateguh/Book-Recommendation/master/image/3.%20Top%2010%20Most%20Rated%20Books.png)


## Top 10 User Giving Most Ratings

![Top 10 User Giving the Most Rating](https://raw.githubusercontent.com/karyateguh/Book-Recommendation/master/image/4.%20Top%2010%20User%20Giving%20the%20most%20rating.png)


## Book Publication Trend

![Book Publication Trend](https://raw.githubusercontent.com/karyateguh/Book-Recommendation/master/image/5.%20Book%20Publication%20Trend.png)




## Top 10 Publishers Pusblish The Books
![Top 10 Publishers Publish The Books](https://raw.githubusercontent.com/karyateguh/Book-Recommendation/master/image/Top%2010%20Publshers%20Publish%20The%20Books.png)

# Data Cleaning
Here's a breakdown of the cleaning techniques, explained with care and purpose. 

## 1. Removing Duplicate Entries

Duplicate entries can distort analysis by over-representing certain data points. For example:

In the Books dataset, duplicates in the ISBN column would imply multiple records for the same book, leading to inconsistencies in matching ratings.
In the Users dataset, repeated User-ID entries could inflate the number of unique users, skewing demographic analysis.
In the Ratings dataset, repeated combinations of User-ID and ISBN might suggest duplicate ratings for the same book by the same user, misleading recommendation algorithms.
Removing duplicates ensures that every book, user, and rating is represented only once, providing a cleaner foundation for analysis. According to data integrity principles, removing duplicates helps maintain the accuracy of insights derived from the data . 

## 2. Ensuring Consistency Between Datasets

Datasets often reference one another, creating dependencies that must be preserved for reliable analysis. Here:

Ratings include both ISBN (to identify books) and User-ID (to identify users).
By filtering the ratings dataset to include only entries where the ISBN exists in the Books dataset and the User-ID exists in the Users dataset, we ensure that all references in the ratings data are valid.
For instance, if a rating references a book no longer present in the books dataset, that rating becomes meaningless. Ensuring consistency between datasets is a standard practice to avoid orphan records and maintain relational integrity . 

# 3. Dropping Duplicates Again in Ratings

Although duplicates based on User-ID and ISBN were already addressed, performing a final drop_duplicates operation ensures no stray duplicates exist due to prior operations or merging inconsistencies. This final check ensures a pristine dataset, free of redundancy.

# Preprocessing

Before building a recommender system, preprocessing ensures that the data is clean, relevant, and ready for modeling. The steps outlined in the provided code take us through filtering interactions and preparing the dataset for a collaborative filtering model using the Surprise library. Let’s unpack each part of the process.

## 1. Filtering Interactions

* What’s happening here?

Thresholds are set: Users must have rated at least 5 books (min_user_interactions = 5), and books must have received at least 5 ratings (min_book_interactions = 5).

Counting interactions:

user_counts calculates the number of ratings each user has given.

book_counts calculates the number of ratings each book has received.

Filtering the dataset: The ratings dataset is filtered to include only users and books meeting the minimum interaction criteria.

* Why this step matters:
Sparse datasets can negatively affect recommendation models because users with very few interactions provide limited information about their preferences, making it harder for the model to generalize effectively. Similarly, books with minimal ratings do not offer enough data to infer their general appeal. Removing these sparse interactions is a widely accepted best practice in recommender systems, as discussed by Ricci, Rokach, and Shapira (2015) in Recommender Systems Handbook.

## 2. Preparing the Dataset for Surprise

* What’s happening here?

Reader object: Defines the rating scale, which in this case ranges from 0 to 10. This ensures the model interprets ratings correctly during training.

Dataset.load_from_df: Converts the filtered dataset into a format compatible with the Surprise library, which is a popular toolkit for building recommendation algorithms.

* Why this step matters:
  
Collaborative filtering algorithms require data in a specific format, typically consisting of user-item-rating triples. Using the Surprise library streamlines the preprocessing phase, ensuring the dataset is ready for further operations like splitting and training. Hug (2020), in the Surprise library documentation, highlights the library's ability to handle sparse datasets and simplify collaborative filtering implementation.

## 3. Split Dataset

* What’s happening here?

Splitting the data: The dataset is divided into a training set (80%) and a test set (20%).

random_state: Ensures reproducibility, so results remain consistent across runs.

* Why this step matters:

This step is critical for evaluating the model’s performance on unseen data. Using a separate test set mimics real-world scenarios where the model must make predictions for unknown interactions. Koren, Bell, and Volinsky (2009) emphasize the importance of train-test splits in preventing overfitting and ensuring that the model generalizes well to new data.

# Modelling

# 1. SVD (Singular Value Decomposition)
SVD is a classic and well-regarded model in recommendation systems. Its primary strength lies in its ability to handle sparse datasets, which are common in user-item matrices where most items remain unrated by users.

* Strengths:

Excellent rating prediction accuracy (lowest RMSE).
High recall indicates broad coverage of relevant recommendations.
Relatively straightforward implementation and fast training times.

* Weaknesses:

Precision was limited at 0.20, meaning only 20% of the recommended books were truly relevant.
Could struggle with incorporating implicit feedback like browsing or clicks.
SVD's performance aligns with findings in literature, where it is often noted for its scalability and predictive power (Koren et al., 2009).

* Top 10 Recommendation

| ISBN         | Title                                                          | Author             |
|--------------|----------------------------------------------------------------|--------------------|
| 0060256672   | Where the Sidewalk Ends : Poems and Drawings                   | Shel Silverstein   |
| 0439136350   | Harry Potter and the Prisoner of Azkaban (Book 3)              | J. K. Rowling      |
| 0439064864   | Harry Potter and the Chamber of Secrets (Book 2)               | J. K. Rowling      |
| 0894808249   | All I Need to Know I Learned from My Cat                       | Suzy Becker        |
| 0553274325   | Johnny Got His Gun                                             | Dalton Trumbo      |
| 0060248025   | Falling Up                                                     | Shel Silverstein   |
| 8478886451   | Harry Potter y el cáliz de fuego                               | J. K. Rowling      |
| 0786808551   | The Arctic Incident (Artemis Fowl, Book 2)                    | Eoin Colfer        |
| 0439425220   | Harry Potter and the Chamber of Secrets Postcard Edition       | J. K. Rowling      |
| 3522128001   | Die unendliche Geschichte: Von A bis Z                         | Michael Ende       |


# 2. SVD++
Building on SVD, SVD++ adds implicit feedback into the mix, such as a user’s interactions with items beyond ratings. While theoretically promising, SVD++ shows slightly weaker prediction accuracy. 

Captures both explicit ratings and implicit feedback, providing a fuller picture of user preferences.
Performs well in terms of recall, ensuring that most relevant books are recommended.

* Weaknesses:

Higher computational cost due to its complexity.
Marginally worse RMSE than SVD, showing a small decline in predictive performance.
Although SVD++ shows promise for datasets with rich implicit feedback, its added complexity may not justify the performance gains in this case (Koren, 2008).

* Top 10 Recommendation

| ISBN         | Title                                                          | Author                |
|--------------|----------------------------------------------------------------|-----------------------|
| 0060256672   | Where the Sidewalk Ends : Poems and Drawings                   | Shel Silverstein      |
| 0894808249   | All I Need to Know I Learned from My Cat                       | Suzy Becker           |
| 0060248025   | Falling Up                                                     | Shel Silverstein      |
| 0553282182   | Alicia: My Story                                               | Alicia Appleman-Jurman|
| 0380973642   | Smoke & Mirrors: Short Fictions and Illusions                  | Neil Gaiman           |
| 0394823370   | The Lorax                                                      | Dr. Seuss             |
| 0962959170   | The Teenage Liberation Handbook: How to Quit School and Get a Life | Grace Llewellyn    |
| 0762409533   | TOLKIEN MAGNETIC POSTCARDS(tm) 12 Full-color Magnet Postcards | Brothers Hildebrandt  |
| 0316578398   | The Art of Raising a Puppy                                     | New Skete Monks       |
| 3522128001   | Die unendliche Geschichte: Von A bis Z                         | Michael Ende          |



# 3. NMF (Non-Negative Matrix Factorization)
NMF uses a different mathematical approach by restricting values to be non-negative, which often makes it more interpretable. However, it struggled with the dataset and achieving lower accuracy. While its recall and precision scores matched the other models, NMF did not show any standout strengths in this scenario.

* Strengths:

Provides interpretable factorization results, which can help understand user and item features (Lee & Seung, 1999).
Simpler implementation compared to more complex models like SVD++.

* Weaknesses:

Poor rating prediction accuracy (highest RMSE).
Fails to leverage implicit feedback effectively.
NMF’s interpretability is valuable, but it is not enough to compensate for its weaker predictive performance in this context.

* Top 10 Recommendation

| ISBN         | Title                                                          | Author               |
|--------------|----------------------------------------------------------------|----------------------|
| 0195153448   | Classical Mythology                                            | Mark P. O. Morford   |
| 0002005018   | Clara Callan                                                   | Richard Bruce Wright |
| 0060973129   | Decision in Normandy                                           | Carlo D'Este         |
| 0374157065   | Flu: The Story of the Great Influenza Pandemic                 | Gina Bari Kolata     |
| 0393045218   | The Mummies of Urumchi                                         | E. J. W. Barber      |
| 0399135782   | The Kitchen God's Wife                                         | Amy Tan              |
| 0425176428   | What If?: The World's Foremost Military Historians' Answers to Questions about Life | Robert Cowley |
| 0671870432   | PLEADING GUILTY                                               | Scott Turow           |
| 0679425608   | Under the Black Flag: The Romance and the Real Story of Pirates | David Cordingly      |
| 074322678X   | Where You'll Find Me: And Other Stories                        | Ann Beattie          |


# 4. CoClustering
CoClustering takes a different approach by grouping users and items into clusters. Surprisingly, it performed well. CoClustering strikes a balance between accuracy and computational efficiency.

* Strengths:

Strong recall, similar to SVD, with a competitive RMSE.
Efficient clustering approach, which can handle sparsity well.

* Weaknesses:

Limited precision, suggesting it struggles to prioritize the most relevant books.
Can be less flexible than other models when scaling or incorporating side information.
CoClustering is often highlighted in literature for its ability to handle sparse matrices effectively (George & Merugu, 2005).

* Top 10 Recommendation

| ISBN         | Title                                                          | Author               |
|--------------|----------------------------------------------------------------|----------------------|
| 0195153448   | Classical Mythology                                            | Mark P. O. Morford   |
| 0002005018   | Clara Callan                                                   | Richard Bruce Wright |
| 0060973129   | Decision in Normandy                                           | Carlo D'Este         |
| 0374157065   | Flu: The Story of the Great Influenza Pandemic                 | Gina Bari Kolata     |
| 0393045218   | The Mummies of Urumchi                                         | E. J. W. Barber      |
| 0399135782   | The Kitchen God's Wife                                         | Amy Tan              |
| 0425176428   | What If?: The World's Foremost Military Historians' Answers to Questions about Life | Robert Cowley |
| 0671870432   | PLEADING GUILTY                                               | Scott Turow           |
| 0679425608   | Under the Black Flag: The Romance and the Real Story of Pirates | David Cordingly      |
| 074322678X   | Where You'll Find Me: And Other Stories                        | Ann Beattie          |


# Evaluation and Metrics Used in the Book Recommendation System

To evaluate the performance of the recommendation models (SVD, SVD++, NMF, and CoClustering), several standard metrics were used. These metrics are common in the field of recommender systems and help assess how well the models predict ratings and suggest relevant items for users. Below are the key evaluation metrics used in this project:

## 1. Root Mean Square Error (RMSE)
RMSE is one of the most widely used metrics for evaluating the accuracy of rating predictions. It measures the square root of the average squared differences between predicted ratings and actual ratings. A lower RMSE indicates better predictive accuracy, as it reflects smaller deviations between the predicted and actual ratings.

## Root Mean Squared Error (RMSE)

Formula:

$$
\text{RMSE} = \sqrt{\frac{1}{N} \sum_{i=1}^{N} \left( r_{ui} - \hat{r}_{ui} \right)^2}
$$

Where:
- **N** is the total number of ratings in the test set.
- **r_{ui}** is the actual rating given by user **u** to item **i**.
- **\hat{r}_{ui}** is the predicted rating for user **u** on item **i**.


The RMSE is useful for quantifying how well the model's predictions match the true ratings. A lower RMSE indicates that the model's predictions are closer to the actual ratings, which implies better accuracy.

### Results:

| Model          | RMSE   |
|----------------|--------|
| SVD            | 3.5311 |
| CoClustering   | 3.5987 |
| SVD++          | 3.7281 |
| NMF            | 3.9527 |


## 2. Precision at K (Precision@K)

Precision@K measures the proportion of relevant items among the top \( K \) recommended items. It evaluates how well the model recommends relevant items (e.g., books) to the user. This metric helps determine the quality of the top \( K \) recommendations in terms of relevance.

### Formula:

$$
\text{Precision@K} = \frac{\text{Number of relevant items in top K}}{K}
$$

### Explanation:

- **Relevant items**: Items with ratings greater than or equal to a certain threshold (e.g., a rating of 7 or above).
- \( K \): The number of recommendations. In this case, \( K = 10 \).

Precision at K measures the proportion of relevant items among the top 
𝐾
K recommended items. It is used to evaluate how well the model recommends relevant items (books) to the user. This metric helps determine the quality of the top 
𝐾
K recommendations in terms of relevance.


In the context of this project, a precision of 0.20 means that only 20% of the top 10 recommended books were relevant to the user. 


## 3. Recall at K (Recall@K) 

Recall@K measures the proportion of relevant items that are included in the top \( K \) recommended items. It evaluates how well the model covers all the relevant items for a user, regardless of whether they appear in the top \( K \) recommendations or not.

### Formula:

$$
\text{Recall@K} = \frac{\text{Number of relevant items in top K}}{\text{Total number of relevant items}}
$$

### Explanation:

- **Relevant items**: Items with ratings greater than or equal to a certain threshold (e.g., a rating of 7 or above).
- \( K \): The number of top recommendations.
- **Total number of relevant items**: The total count of items rated as relevant by the user.

### Results:

| Model          | Recall@10 |
|----------------|-----------|
| SVD            | 0.91      |
| CoClustering   | 0.91      |
| SVD++          | 0.90      |
| NMF            | 0.90      |


## Explanation of Trade-Off Between Precision and Recall
While precision and recall are both important metrics, they often represent a trade-off. High precision means that the recommended books are highly relevant, but this can sometimes come at the cost of recall—meaning the model might miss relevant books. On the other hand, high recall means the model includes almost all relevant books in the recommendations, but it may also include less relevant items, lowering precision.

In this project, the precision was 0.20 across all models, which suggests that only 20% of the recommended books were relevant. However, the recall was quite high at 0.91, indicating that the models successfully included most of the relevant books in the recommendations.

# Conclusion
1. Based on the results, the SVD model delivers the most accurate predictions with the lowest RMSE of 3.5327. This suggests that the SVD model is better at minimizing prediction errors compared to the other models (NMF, CoClustering, and SVD++). While precision and recall are consistent across all models at 0.20 and 0.91 respectively for the top 10 recommendations, RMSE is a crucial indicator of overall prediction accuracy, where the lower value of SVD implies a better performance in terms of predictive accuracy.

2. The top 10 recommendations from the SVD model demonstrate 0.20 Precision@10 and 0.91 Recall@10. These metrics indicate that, although the relevance of recommendations (precision) is modest, the model is highly effective at recalling a broader range of relevant recommendations (recall). This shows that the SVD model is effective in retrieving relevant recommendations, despite the relatively low precision score. This implies that the SVD model is capable of suggesting a larger set of relevant items, which is valuable for user engagement.

**Summary of Model Evaluation:**
SVD stands out as the best-performing model in terms of RMSE, making it the most accurate for predicting user preferences.
Although all models have the same precision and recall for top 10 recommendations, SVD offers the best combination of accuracy (low RMSE) and relevant recommendations (high recall).
Thus, the SVD model is the most appropriate choice for the recommendation system, fulfilling the project goals of both predicting accurately and delivering relevant recommendations to users.




# References:



Aggarwal, C. C. (2016). Recommender Systems: The Textbook. Springer.

Bishop, C. M. (2006). Pattern Recognition and Machine Learning. Springer.

George, T., & Merugu, S. (2005). A scalable collaborative filtering framework based on co-clustering. Proceedings of the Fifth IEEE International Conference on Data Mining (ICDM’05).

Goodfellow, I., Bengio, Y., & Courville, A. (2016). Deep Learning. MIT Press.

Hug, N. (2020). Surprise: A Python Library for Recommender Systems. Retrieved from Surprise Library Documentation.

Koren, Y., Bell, R., & Volinsky, C. (2009). Matrix factorization techniques for recommender systems. Computer, 42(8), 30-37.

Lee, D. D., & Seung, H. S. (1999). Learning the parts of objects by non-negative matrix factorization. Nature, 401(6755), 788-791.

Ricci, F., Rokach, L., & Shapira, B. (2011). Introduction to recommender systems handbook. In Ricci, F., Rokach, L., & Shapira, B. (Eds.), Recommender Systems Handbook (pp. 1–35). Springer.

Ricci, F., Rokach, L., & Shapira, B. (2015). Recommender Systems Handbook. Springer.


