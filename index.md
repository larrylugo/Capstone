

Data Science Capstone - Slide deck
========================================================
width: 1440
height: 900
author: Larry Lugo, M.Sc.
date: November, 2015
font-family: 'Helvetica'
transition: rotate
autosize: true


# <small>Using Sentiment Analysis and Collaborative Filtering methods to predict the review's rating based on its text alone from Yelp 2015 data set</small>


1. Introduction and background
========================================================

* Collaborative Filtering has been mostly focusing on the information resource of explicit user numerical ratings feedbacks.

* The ever-growing availability of textual user reviews has become an important information resource.

* This information rich resource of textual reviews have clearly exhibited brand-new approaches to solving many of the important problems that have been perplexing the research community for years, such as the explanation of recommendation and the automatic generation of user or item profiles. 

* Where a wealth of explicit product attributes/features and user attitudes/sentiments are expressed therein.

* The fundamental importance of textual reviews has gained wide recognition, mainly because of the difficulty in formatting, structuring and analyzing the free-texts. 

* In this Data Science Capstone project, Sentiment Analysis of textual reviews and Collaborative Filtering were combined under a mixed approach to predict the review's rating on Yelp 2015 data set.


2. Methods and data
========================================================


* Yelp offers a rich set of data for research and educational purposes
* One of the questions in this year's challenge is "How well can you guess a review's rating from its text alone?"
* To answer this question the following method was applied: (i) Based on the Yelp´s 5-star rating system, a negative feeling was considered when the review was 2 or less stars. Then, (ii) Text mining methods and a CART (Classification and regression trees) model were applied using R to predict negative or positive (3 or more stars) sentiment in the review.
* The review file has 1.569.264 registers. Due to hardware limitations, it was impossible to process the whole information
* A random sample of 25% (392.316 reviews.) was taken to ensure an accurate representation of the total data set .
* A Text Mining was done: (i) creating the corpus using "text" written by users; (ii) cleaning the data through convert text to lower case, remove punctuation and stop words. (iii) Stem the corpus. (iv) Create a Document Term Matrix. (v) Delete sparse terms with 5% or less of frequency. (vi) Matrix was converted to data frame to build a CART model
* "Negative" was created: TRUE if user gives 2 or less stars. FALSE is 3 or more stars are given.


```r
# SAMPLING: Take a 25% sample = 392,316 records
projData25 <- projData[sample(1:nrow(projData), 392316, replace=FALSE),]
str(projData25)
```

```
'data.frame':	392316 obs. of  3 variables:
 $ stars   : int  3 1 5 4 1 5 5 4 5 3 ...
 $ text    : chr  "I had my very first Red velvet cake when I was 16. It was a gift from a friend. It was my first experience with Barbs Bakery. I"| __truncated__ "They don't take reservations and the hostess was rude and also inept. She seated a party of 2 at a 6 top table which meant the "| __truncated__ "Perfect!  My car looked better than the day I bought it and I bought it new.  I had a stain on the seat from a red sharpie from"| __truncated__ "It's McDonald's, expect the expected. This location isn't open late. Keep going down to the one next to AZ Mills for late nite "| __truncated__ ...
 $ negative: Factor w/ 2 levels "FALSE","TRUE": 1 2 1 1 2 1 1 1 1 1 ...
```

3. Results
========================================================





```r
predictCART = predict(sparse5CART, newdata=testsparse5, type="class")
table(testsparse5$Negative, predictCART)
```

```
       predictCART
        FALSE  TRUE
  FALSE 93791  1476
  TRUE  19923  2505
```

```r
printcp(sparse5CART)
```

```

Classification tree:
rpart(formula = Negative ~ ., data = trainsparse5, method = "class")

Variables actually used in tree construction:
[1] great love  told 

Root node error: 52332/274621 = 0.19056

n= 274621 

        CP nsplit rel error  xerror      xstd
1 0.018268      0   1.00000 1.00000 0.0039329
2 0.011160      2   0.96346 0.96346 0.0038769
3 0.010000      3   0.95230 0.95645 0.0038659
```

<small>The CART model has an accuracy of 81.65%. Three variables were significative: great, love and told, with a Root node Error of 19.06%.</small>

4. Results (continue) and Discussion
========================================================


```r
prp(sparse5CART)
```

<img src="index-figure/unnamed-chunk-6-1.png" title="plot of chunk unnamed-chunk-6" alt="plot of chunk unnamed-chunk-6" style="display: block; margin: auto;" />


5.Discussion

<small>Using 25% of data Review, near four hundred thousand opinions, it was posible to develop a CART model to predict the user´s sentiment regarding their experience in different kind of business. 

The model had an accuracy of 81.75% and a Root node error of  19.204%.  The Relative Error stabilizes with 3 variables, so model was adjusted with this number of variables, being words told, great and love the most significant.

Sampling is really effective when there are hardware, time or economic limitations that makes imposible to analyze the whole data in huge files. To avoid this kind of limitations, technology like Hadoop or other server based technology could be used.

This study demonstrated that it is possible to predict favorable or unfavorable sentiment in user reviews based only on text written by them. To increase the accuracy of the model, it is recommended to incorporate other variables such as gender, location, hours of sevice, among others, but having adequate computing power according to study complexity.</small>


