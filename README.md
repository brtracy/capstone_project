![IMDb logo](https://github.com/brtracy/capstone_project/blob/main/img/Imdb-logo.png?raw=true)

# Detecting Movie Spoilers with NLP

***
### The Background
Nobody likes a spoiled movie (or TV show or book for that matter). [Studies](https://www.statista.com/statistics/899009/reading-reviews-before-viewing-movie-united-states-by-age/) show that about 2/3 of U.S. adults utilize reviews to influence their decision to see a movie. When those reviews contain spoilers, they affect the anticipation and suspension of disbelief that are integral to a pleasureable movie-going experience. Users tend to avoid resources that do not censor spoiler content; they can knowingly seek out such content from other sources if they so wished.

### Business Problem
IMDb, the premier site for movie information, is no stranger to dealing with the effects of hosting spoiler content. Their current solution mainly relies on self-flagging of spoiler content by the user themselves, or for other users to report a review that contains spoilers. By employing automated flagging sytems to hide user reviews containing spoilers, IMDb can provide the users with a confidence that their browsing will not present them with spoiler content. We are tasked with developing a model to detect reviews containing spoilers so they can be censored.

### The Data
The dataset can be downloaded from [Kaggle](https://www.kaggle.com/datasets/rmisra/imdb-spoiler-dataset), which is also available [here](https://rishabhmisra.github.io/publications/). It contains 573,913 review records that correspond to 1572 movies. There is additional film and review metadata, like film release date, critic and user rating, review post date, movie genre, etc. Primarily we are interested in the user reviews text and the target, a feature called 'is_spoiler' which indicates if the review contains a spoiler or not. There is an imbalance to our target, about 73.6% of the reviews do not contain a spoiler.

Exploratory analaysis found that, despite massive differences in raw review count, the percentage of the reviews marked as spoilers is relatively homogenous when compared by genre. See the following bar graph that shows the raw review count (non-spoiler reviews in blue, spoiler in orange) along with a secondary grey bar that indicated percentage of reviews as spoiler.
![spoilers by genre](https://github.com/brtracy/capstone_project/blob/main/img/spoilers_by_genre.png?raw=true)

We also discovered that there are 'serial spoilers' in the userbase, that is to say users who almost exclusive post reviews that can be considered a spoiler. You can see in the following graph a representation of the top 20 reviewers by raw review count broken down by spoiler(orange) and non-spoiler(blue). While not the particular goal of the project, this information should help IMDb update their policy with respect to allowing these users to remain on their platform.
![spoilers by user](https://github.com/brtracy/capstone_project/blob/main/img/spoilers_by_user.png?raw=true)

### The Process
After removing non-English reviews, the review text was cleaned and lemmatized. A simple baseline model was developed, predicting that if the word 'spoiler' appeared in the review then the review was classified as a spoiler. You can see the results here:
![baseline model results](https://github.com/brtracy/capstone_project/blob/main/img/baseline.png?raw=true)

The baseline scores allowed us a comparative benchmark as we iterated through different vectorization encoding types/settings and modeled with Multinomial Naive Bayes, Logistic Regression, and Support-vector Machines. Evaluation of model performance was done using classification reports and confusion matrices resulting in selection of a best performing model.

### Conclusions
We found that bag of words modeling struggled in picking up on what makes a spoiler review a spoiler. Our best model utilized TF-IDF text encoding, adding in bi- and tri-grams with a minimum count of 8, trained using a Support-vector machine. We achieved an overall accuracy of 77% while maintaing recall of above 90% on non-spoiler reviews but only a 40% on spoiler reviews. 
![final model results](https://github.com/brtracy/capstone_project/blob/main/img/best%20model.png?raw=true)

Seeing improvement over iterations of the bag-of-words modeling we feel that turning to cutting edge tools like BERT modeling would see even greater success. Another interesting avenue is cosine similarity, comparing the review to the synopsis of the film being reviewed, which has a more narrow scope but could prove to be successful.

