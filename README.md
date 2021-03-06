# Live, Love, Like: The Science of Popularity
_Machine Learning 1 - Assignment 3: Using kNN classifier on actual data_

Facebook, as one of the most prominent social networking sites around the globe, has put itself at the core in many individuals' lifestyle. In one way or another, it has positioned itself in its consumers' lives regardless of age, gender, or even location. Notably in the Philippines, where in some cities, there exists more users than the area's population, Facebook has given numerous individuals a platform of sharing their stories. And with each story comes an opportunity for members of one's circle to validate and like the person's status – whether that's relatable status, a viral video, or a funny meme – the more likes a post generates, the more popular it becomes.
 
Here, we explore the features that could lead to a post generating more likes. What notable variables, if any at all, actually enable a post to receive more likes given a certain set of publicly available features?

**LT7 Members:**
* Amorado, Adriane Mikko A.
* Marcaida, Cymon P.
* Navarro, Tal Miguel A.
* Rance, Louise Hezekiah H.
* Quinto, Joanna G.


## Highlights of the Results
* Using the k-Nearest Neighbor algorithm to predict popularity, the optimal no. of neighbors is 13, with a mean accuracy of 62.32%, with less than or equal to 3 (median value) likes considered as unpopular while posts with greater than 3 likes as popular. 
* For this EDA, the researcher is trying to identify the gender of the user who posted something in Facebook using `average_user_likes`, `friend_count`, `popular`. Using $k$NN classification with a maximum of 30 neighbors and 25 loops, it yielded the lowest accuracy difference of $0.0006$ at an accuracy of around $0.970$, When increased the neighbors to 70 and decreased the loops to 5, it yielded the lowest accuracy difference at around $0.75$ accuracy.
* Using all the features (except the ones that are directly derived from the likes of the post, e.g., `score`, `message_score`, etc.) yielded a maximum of 83.89% accuracy at 23 neighbors. However, by using only the features related to the user (e.g., `age`, `average_user_likes`, `friend_count`, etc.) the $k$NN model can achieve a max accuracy of 84.60% at 39 neighbors.

## Data discussion

### About the Dataset
The dataset includes 14,090 instances of Facebook statuses including 9 features. The data was gathered using each of one of the authors' group member’s Facebook tokens and included data from that individual and all of their friends. The features that were collected include the number of friends, age, and gender of the user who posted the status, the time of the status (month and times of day), the time since the last status (hours), and the “score” of the Facebook status itself. To find the score of a Facebook status, a dictionary was built by the authors with individual scores of keywords from statuses in their entire dataset. The individual score was calculated by averaging the number of likes that a status with that word receives. Once the individual scores for each word is calculated and stored in a dictionary, the status is scored by averaging those values for each word in the status. 

An additional column was added to bin the number of likes *(**0** for 0 to 10 likes, **1** for 11 to 100 likes, and **2** for 101 to 1000 likes)* and that was the feature that the group wanted to predict, which is included in the training and validation set. The training set included 75% of the total data instances and was used to train our model while the testing set contained 3,522 instances used to evaluate the model’s results.


### Acknowledgements
The authors and contributors for the data for this project were Kevin Chen, Basil Huang, and Brittany Lee of Northwestern University for EECS 349: Machine Learning with Professor Doug Downey. They can be contacted regarding this project at:
* kevinchen2016@u.northwestern.edu
* basilhuang2014@u.northwestern.edu
* brittanylee2015@u.northwestern.edu

### Features

The dataset has `nine (9)` features as shown below: 

- **`friend_count:`** This number could help indicate the range of values that a user’s status could receive, since this would be roughly the maximum number of likes.
- **`age:`** The user's age. The user’s group of friends may consist mostly of other user’s of the same age, and people of this age may have certain interests.
- **`gender:`** Statuses about certain topics may be viewed differently if posted by one gender.
- **`Time the status was posted (month and time of day):`** Posting statuses about time-sensitive events will garner more likes if posted in a timely manner. Additionally, posting statuses when more people are online will also increase visibility.
    - **`month:`** Month when the post was posted on their facebook profile/page.  
    - **`time_of_day:`**
        - post_midnight: 12AM - 4AM
        - early_morning: 4AM - 8AM
        - morning: 8AM - 11AM
        - noon: 11AM - 2PM
        - afternoon: 2PM - 5PM
        - evening: 5PM - 9PM
        - night: 9PM - 12AM
- **`time_since_last (hours):`** People may be less inclined to like the status of someone who spams.
- **`message_score`** - A score assigned based on the words used in the status.
- **`average_user_likes:`** If a person get a high number of likes on every status, their statuses will get like more in general.
- **`score:`** Score of the status based off of the word score dictionary. Certain words and topics can contribute more likes to a status than others. To calculate the score, we remove common words, such as “the”, “was”, “am”, etc, and assign each of the remaining words a score of (the natural log of the number of likes in this status) / (number of remaining words). The score for each word is updated and averaged in the word score dictionary. We take the natural log so that statuses that have the same number of likes within an order of magnitude will be classified similarly.

### Data processing

The following steps were done to prepare the data for out models:

- Dataset was created from the combination of 2 csv files: `fb_likes1.csv` and `fb_likes2.csv`. The column names were created from `fb_cols.csv`.
- For our $k$NN models, we created the number of likes column based on the exponential of the score column
- An additional column was added to bin the number of likes *(**0** for 0 to 10 likes, **1** for 11 to 100 likes, and **2** for 101 to 1000 likes)*
- We also converted the `time_of_day` into integers: 0 for `early_morning`, 1 for `morning`, 2 for  `afternoon`, 3 for `evening`, 4 for `night` and 5 for `post_midnight`. There was no data for noon.
- Gender was converted to 0 for `male`, 1 for `female`.
- Some of the data contained question marks `?` we converted them to `NaN` and later dropped them for the models.
