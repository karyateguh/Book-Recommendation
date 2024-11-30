# Book-Recommendation

Original file is located at
    https://colab.research.google.com/drive/1KRTgroTtBwX_Vpt8e49886pSHsCI1MxQ

# Project Domain

Books have always been a gateway to new knowledge and self-discovery, and now, thanks to technology, finding the right books has become easier than ever. Book recommendation systems are key to helping readers discover books that match their personal interests, making it easier to navigate the vast world of literature. These systems use smart algorithms to understand what readers like and suggest books they are most likely to enjoy.

According to a 2021 report by Grand View Research, the market for book recommendation systems has been growing rapidly due to the rising demand for personalized reading experiences. Platforms like Goodreads, Amazon, and Scribd use different methods to recommend books, such as collaborative filtering (based on what other users liked) and content-based filtering (focused on book genres or themes). This personalization not only makes readers happy but also helps boost book sales.

In Indonesia, where promoting literacy is a top priority, these recommendation systems can play a big role in encouraging people to read more. According to a report by the Ministry of Education and Culture in 2023, programs like the Gerakan Literasi Nasional (National Literacy Movement) aim to improve reading habits across the country. However, one of the biggest challenges is that many people in rural areas still struggle with limited access to books. Digital platforms with recommendation systems could help solve this by offering personalized book suggestions tailored to local needs and preferences.

Studies have shown that recommendation systems really do help people read more. A 2022 study in the Journal of Educational Technology found that when book suggestions are personalized, readers, especially younger ones, are more likely to read frequently. By analyzing factors like a reader's favorite genres, past books, and user reviews, these systems can guide them toward books they’ll love.

Popular platforms like Goodreads use a method called collaborative filtering, where they look at what other readers with similar tastes have liked and suggest books based on that. For example, if someone enjoys Atomic Habits by James Clear, the system might recommend books like Deep Work by Cal Newport or Grit by Angela Duckworth. Amazon’s system takes it a step further by combining user data with content-based insights, making recommendations even more accurate and relevant.

Although these systems have made discovering books easier, they still face some challenges. One issue is bias in the algorithms, which can sometimes lead to repetitive recommendations. Another problem is that niche books, especially from underrepresented authors, don’t always get the attention they deserve. To improve this, it’s important to diversify the data these systems use and create more inclusive models. Additionally, local platforms, especially in countries like Indonesia, could do a better job by offering recommendations that include books in regional languages or from local authors.

In the end, book recommendation systems are changing the way we discover and enjoy books, offering personalized experiences that encourage us to read more. With ongoing improvements in artificial intelligence and machine learning, these systems will only get better, helping readers connect with books in deeper, more meaningful ways.

# Business Understanding

Problem Statements:

1. Which models can predict the best
2. How is ten recommendations

Goals:

1. To find out which model can predict the best
2. To find out the price rice in the future


Solutions

Designing a recommendation system is like creating a personal librarian for each user—one that knows their preferences and introduces them to books they'll love. In this project, we evaluated four different algorithms: SVD, SVD++, NMF, and CoClustering, comparing their strengths and weaknesses to select the most suitable model for a book recommendation system. The evaluations were conducted based on metrics like RMSE, precision, and recall, which are widely used in recommendation system research (Koren et al., 2009; Ricci et al., 2011).

# Data Understanding

* User-ID : Id pengguna
* Age : umur pengguna
* ISBN : kode unik buku
* Title : Judul Buku
* Author : Nama penulis buku
* Year : Tahun terbit 
* Publisher  : Nama penerbit. 

Terdapat 3 file dalam dataset:
* Book, dengan 271379 baris
* User, dengan 278859 baris
* Rating, dengan 1149780 baris

Berikut beberapa baris yang kosong:
* Masing-masing 2 baris pada kolom author dan publisher
* 110232 baris pada kolom Age


# EDA

## Rating Diistribution



## Age Distribution



## Top 10 Most Rated Books



## Top 10 User Giving Most Ratings



## Book Publication Trend



## Correlation Between Rating And User Age



## Top 10 Publishers Pusblish The Books


# Data Cleaning
Berikut bebrapa teknik pembersihan data:

1. Menghapus Duplikat
2. Menangani Missing Values

* Untuk ratings, kita akan menghapus baris yang memiliki nilai rating kosong
* Untuk usia pengguna, kita bisa mengisi nilai yang hilang dengan rata-rata usia
* Untuk Tahun Publikasi Buku, kita akan mengisi nilai yang hilang dengan 0 atau median tahun


3. Memastikan Format yang Konsisten

* Mengonversi rating ke dalam format integer
* Mengonversi kolom ISBN dan User-ID menjadi tipe data string untuk konsistensi

4. Memastikan Rentang Rating yang Valid (Jika rating berada di luar 0-10, hapus)


# Model 1: Sistem Rekomendasi Collaborative Filtering (SVD)

# Preprocessing
1. Menyiapkan dataset
* Reader
Membuat objek pembaca yang menentukan rentang nilai penilaian (rating scale). Dalam hal ini, penilaian berada dalam rentang 0 hingga 10.

* Dataset.load_from_df
Mengonversi DataFrame pandas, yaitu ratings[['User-ID', 'ISBN', 'Rating']], menjadi format dataset yang sesuai dengan Surprise. Kolom yang digunakan:

User-ID: Identitas unik pengguna.
ISBN: Identitas unik buku.
Rating: Penilaian yang diberikan pengguna pada buku.
Fungsi ini menghasilkan dataset yang siap digunakan oleh algoritma Collaborative Filtering di Surprise.

2. Membagi data menjadi train dan test set dengan perbandingan 80:20




# 1. SVD (Singular Value Decomposition)
SVD is a classic and well-regarded model in recommendation systems. Its primary strength lies in its ability to handle sparse datasets, which are common in user-item matrices where most items remain unrated by users. In our evaluation, SVD achieved an RMSE of 3.5311, the lowest among all models, indicating strong accuracy in predicting ratings. It also performed well in recall, scoring 0.91, which means it successfully identified 91% of the relevant books for the user.

* Strengths:

Excellent rating prediction accuracy (lowest RMSE).
High recall indicates broad coverage of relevant recommendations.
Relatively straightforward implementation and fast training times.

* Weaknesses:

Precision was limited at 0.20, meaning only 20% of the recommended books were truly relevant.
Could struggle with incorporating implicit feedback like browsing or clicks.
SVD's performance aligns with findings in literature, where it is often noted for its scalability and predictive power (Koren et al., 2009).

# 2. SVD++
Building on SVD, SVD++ adds implicit feedback into the mix, such as a user’s interactions with items beyond ratings. While theoretically promising, SVD++ had a higher RMSE of 3.7281, which shows slightly weaker prediction accuracy. Its recall dropped marginally to 0.90, and its precision remained the same as SVD at 0.20.

* Strengths:

Captures both explicit ratings and implicit feedback, providing a fuller picture of user preferences.
Performs well in terms of recall, ensuring that most relevant books are recommended.

* Weaknesses:

Higher computational cost due to its complexity.
Marginally worse RMSE than SVD, showing a small decline in predictive performance.
Although SVD++ shows promise for datasets with rich implicit feedback, its added complexity may not justify the performance gains in this case (Koren, 2008).

# 3. NMF (Non-Negative Matrix Factorization)
NMF uses a different mathematical approach by restricting values to be non-negative, which often makes it more interpretable. However, it struggled with the dataset, achieving the highest RMSE of 3.9527, indicating lower accuracy. While its recall and precision scores matched the other models, NMF did not show any standout strengths in this scenario.

* Strengths:

Provides interpretable factorization results, which can help understand user and item features (Lee & Seung, 1999).
Simpler implementation compared to more complex models like SVD++.

* Weaknesses:

Poor rating prediction accuracy (highest RMSE).
Fails to leverage implicit feedback effectively.
NMF’s interpretability is valuable, but it is not enough to compensate for its weaker predictive performance in this context.

# 4. CoClustering
CoClustering takes a different approach by grouping users and items into clusters. Surprisingly, it performed well, achieving the second-lowest RMSE of 3.5987. Its recall was excellent at 0.91, matching SVD, and its precision was consistent at 0.20. CoClustering strikes a balance between accuracy and computational efficiency.

* Strengths:

Strong recall, similar to SVD, with a competitive RMSE.
Efficient clustering approach, which can handle sparsity well.

* Weaknesses:

Limited precision, suggesting it struggles to prioritize the most relevant books.
Can be less flexible than other models when scaling or incorporating side information.
CoClustering is often highlighted in literature for its ability to handle sparse matrices effectively (George & Merugu, 2005).


# Evaluation and Metrics Used in the Book Recommendation System

To evaluate the performance of the recommendation models (SVD, SVD++, NMF, and CoClustering), several standard metrics were used. These metrics are common in the field of recommender systems and help assess how well the models predict ratings and suggest relevant items for users. Below are the key evaluation metrics used in this project:

## 1. Root Mean Square Error (RMSE)
RMSE is one of the most widely used metrics for evaluating the accuracy of rating predictions. It measures the square root of the average squared differences between predicted ratings and actual ratings. A lower RMSE indicates better predictive accuracy, as it reflects smaller deviations between the predicted and actual ratings.

Formula:
𝑅
𝑀
𝑆
𝐸
=
1
𝑁
∑
𝑖
=
1
𝑁
(
𝑟
𝑢
𝑖
−
𝑟
^
𝑢
𝑖
)
2
RMSE= 
N
1
​
  
i=1
∑
N
​
 (r 
ui
​
 − 
r
^
  
ui
​
 ) 
2
 
​
 
Where:

𝑁
N is the total number of ratings in the test set.
𝑟
𝑢
𝑖
r 
ui
​
  is the actual rating given by user 
𝑢
u to item 
𝑖
i.
𝑟
^
𝑢
𝑖
r
^
  
ui
​
  is the predicted rating for user 
𝑢
u on item 
𝑖
i.
The RMSE is useful for quantifying how well the model's predictions match the true ratings. A lower RMSE indicates that the model's predictions are closer to the actual ratings, which implies better accuracy.


## 2. Precision at K (Precision@K)
Precision at K measures the proportion of relevant items among the top 
𝐾
K recommended items. It is used to evaluate how well the model recommends relevant items (books) to the user. This metric helps determine the quality of the top 
𝐾
K recommendations in terms of relevance.

Formula:
Precision@K
=
Number of relevant items in top K
𝐾
Precision@K= 
K
Number of relevant items in top K
​
 
Where:

The relevant items are those with ratings greater than or equal to a certain threshold (e.g., a rating of 7 or above).
𝐾
K is the number of recommendations (in this case, 
𝐾
=
10
K=10).
In the context of this project, a precision of 0.20 means that only 20% of the top 10 recommended books were relevant to the user. 


## 3. Recall at K (Recall@K)
Recall at K measures the proportion of relevant items that are included in the top 
𝐾
K recommended items. It helps evaluate how well the model covers all the relevant items for a user, regardless of whether they appear in the top 
𝐾
K recommendations or not.

Formula:
Recall@K
=
Number of relevant items in top K
Total number of relevant items
Recall@K= 
Total number of relevant items
Number of relevant items in top K
​
 
Where:

The relevant items are the items with a rating greater than or equal to the threshold (e.g., 7 or above).
𝐾
K is the number of top recommendations.
The denominator is the total number of relevant items available for the user.
A recall of 0.91 means that 91% of the relevant books for the user were included in the top 10 recommendations. 


## 4. Explanation of Trade-Off Between Precision and Recall
While precision and recall are both important metrics, they often represent a trade-off. High precision means that the recommended books are highly relevant, but this can sometimes come at the cost of recall—meaning the model might miss relevant books. On the other hand, high recall means the model includes almost all relevant books in the recommendations, but it may also include less relevant items, lowering precision.

In this project, the precision was 0.20 across all models, which suggests that only 20% of the recommended books were relevant. However, the recall was quite high at 0.91, indicating that the models successfully included most of the relevant books in the recommendations.

# References:

Koren, Y., Bell, R., & Volinsky, C. (2009). Matrix factorization techniques for recommender systems. Computer, 42(8), 30-37.
Ricci, F., Rokach, L., & Shapira, B. (2011). Introduction to recommender systems handbook. Springer.
Lee, D. D., & Seung, H. S. (1999). Learning the parts of objects by non-negative matrix factorization. Nature, 401(6755), 788-791.
George, T., & Merugu, S. (2005). A scalable collaborative filtering framework based on co-clustering. Proceedings of the Fifth IEEE International Conference on Data Mining (ICDM’05).
