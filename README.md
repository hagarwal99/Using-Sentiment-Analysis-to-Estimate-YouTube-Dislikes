# Using Sentiment Analysis to Estimate YouTube Dislikes

## 1 Introduction
On December 13, 2021, YouTube removed the visibility of dislikes on all videos in order to eliminate “dislike attacks”, in which viewers would target a creator and dislike their videos for reasons that were not relevant to the actual content of the video. After the likes were hidden, YouTube viewers were very upset because they used the number of dislikes on a video to gauge its usefulness and relevance to the title of the video. Especially for tutorials, people rely on the statistics on the video to inform them on whether the video can actually solve the problem it claims. With the dislike counts hidden, users are forced to perform an analysis of their own, by comparing the like count with the total view count to determine whether the ratio is high enough to consider watching. However, sometimes previous viewers don’t interact with the video, either positively or negatively, so a good video may not have enough likes to reflect that. Alternatively, users can read the top 10-20 comments to understand the overall sentiment of the viewers. However, this is time-consuming, and creators on YouTube are able to remove comments, and if a creator finds a negative comment is gaining too many likes, they may delete the comment to avoid driving away potential viewers, or exclusively pin positive comments. With both of these options, viewers do not have a reliable way to determine whether a video will give them the information that they desire.

To solve this issue, we have created multiple sentiment-analysis-based regression models to perform a comparative study and understand which models perform better than others. Our models use the comments from a video, in addition to other data, and outputs an estimate for the number of dislikes that the video has. With this solution, we hope to be able to help viewers make informed decisions about the videos that they choose to spend their time watching. Given that videos, especially tutorials, can be several hours long, users may feel that they have wasted their valuable time on a video if they don’t get the outcome that they expected. By providing an estimate of the number of dislikes, users can use that information to decide whether they want to watch the video or not.

## 2 Method
For this comparative study, we used a Kaggle dataset that was collected before YouTube started hiding the dislike counts. It included data for nearly forty-thousand videos with information like the number of views, likes, dislikes, subscribers, comments, tags, and more. However, the comments from the dataset were collected using YouTube’s API, which only returns random comments from the video, with no option to get the most-liked comments instead. Comments that received the most likes obviously resonated with a majority of the viewers, so we felt that it was important to get these comments to gain an accurate understanding of previous viewers’ sentiments. To do this, we scraped the YouTube webpage for each of the video IDs in the dataset to get twenty of the most relevant comments on the page. Ideally, we would have wanted to get more, but due to limitations that YouTube placed on web scraping, we were constrained to 20.

After creating this dataset, we began creating our models. We decided to test three different sentiment analysis models, VADER, pre-trained BERT, and a custom-trained BERT to create a sentiment score rating for each comment. For VADER and pre-trained BERT, getting the sentiment analysis score was part of the package, so there was no additional work necessary for this part. However, in the case of the custom-BERT model, we used an annotated IMDB movie reviews dataset found on Kaggle, which contained mappings of movie reviews to sentiments of either “Positive” or “Negative.” This data was passed into BERT, and the output was passed into a fully connected feed-forward neural network. Finally, the output layer of this neural network would output two numbers: the probability of the comment having a positive sentiment and the probability of the comment having a negative sentiment.

We used the average of the sentiment score ratings from these three sentiment analysis models as a feature in our regression models, along with other video data from the dataset, such as the number of views, likes, dislikes, subscribers, comments, etc. We compared the performance of four different regression models: Linear Regression, SVM, Random Forest, and XGBoost to see which worked better for our application and to perform a comparative study.
We also considered another feature for the regression models, where we took the sentiment
 
## 3 Experimental Setup
To run our models, we use portions of the Kaggle dataset to both train and test our model since this is one of the few datasets that includes the dislike information that we need.
In order to evaluate each of our models, we decided to use Mean Absolute Error (MAE) as our metric. We chose this metric because we wanted to disregard the direction of error in predictions.

## 4 Results
We received our best results using VADER with Random Forest, without the number of comment likes considered in the sentiment rating. We believe that VADER performed the best because of the social media conversation training that it is built on, which makes it perform better than BERT or custom BERT on almost every regression model. This model was also able to outperform similar previous work done, which had reached an MAE of around 2,800].

We observed that our custom BERT model demonstrated weaker performance out of all the three sentiment analysis models. However, we think the disparity is in the language that is used on IMDB versus on YouTube. Reviewers on IMDB tend to use fuller sentences and descriptions of the
movies, whereas YouTube commenters tend to use incomplete sentences, slang, and emojis. We believe that if we were to use a corpus with language similar to YouTube, we could have received better results from our custom BERT model.

Another explanation for the difference in our models could be the variety in polarity ranges that each of these models use. Whereas BERT assigns a polarity between zero and one, VADER uses a scale between negative one and one. This difference means that VADER can express twice the detail in its polarity score, providing more precise values to the regression model.

We were surprised that there were a few cases where certain models actually performed better without the likes on comments being multiplied into the sentiment score rating for each comment. We believed that considering the relative ranking of each comment in relation to each other would certainly help reduce the MAE by helping to prioritize comments that had resonated with more viewers.

## 5 Conclusion
We presented a comparative study of 24 different models to identify which sentiment analysis models and regression models work best at predicting the number of dislikes on a YouTube video, and found a few models that were able to outperform previous work in this field.
We hope that this work results in further research being conducted on this topic.

In the future, we would like to turn this model into an application, such as a Chrome extension, that can give YouTube viewers an estimate of the number of dislikes that a video has, so viewers can make educated decisions on what content they spend their valuable time watching.

## 6 References
- [Southern2022] [1] Southern, Matt G. 2022. “YouTube CEO Defends Removal of Dislike Counts.” Search Engine Journal.
- [Suciu2021] [2] Suciu, Peter. 2021. “YouTube Removed ’Dislikes’ Button – It Could Impact ’How to’ and ’Crafts’ Videos.” Forbes Magazine.
- [Anarios2022] [3] Anarios. 2022. “Return-YouTube- Dislike: Chrome Extension to Return YouTube Dislikes.” GitHub.
- [Mitchell2019] [4] J, Mitchell. 2019. “Trending YouTube Video Statistics.” Kaggle.
- [Srivastava] [5] Srivastava, Aryan, et al. “Student The- ses.” Brown.
- [Nikolaev2021] [6] Nikolaev, Dmitry. 2021. “YouTube Dislikes Dataset Collection.” Kaggle.
- [Pandey2019] [7] Pandey, Parul. 2019. “Simplifying Sentiment Analysis Using Vader in Python (on Social Media Text).” Medium.
- [Ardeshirifar2022] [8] Ardeshirifar, Ramtin. 2022. “Predicting YouTube Dislikes Using Machine Learning.” Medium.
