# Project3_Amazon-fine-foods-reviews

Project part of the module INM432, Big Data, City University, London.

# 1. Motivation

Amazon Fine Food Reviews dataset is used, which contains 568,454 food reviews that 
Amazon.com users left between October 1999 and October 2012. This dataset was selected because it offers a wide range of 
venues that cuould be explored: from building product recommendation models to sentiment analysis. 
This dataset has been widely used by professionals and aspiring data scientists, making it largely compelling. 
The vast length of the dataset, makes it a viable 'playground' for big data model building.

Goal is to predict whether review was helpful or unhelpful to other buyers on Amazon, 
and find words which are distinctive for helpful and unhelpful reviews. In order to do so, new metric called 
'Helpfulness_perct' was created, which is a percentage of helpfulness of the review, 
derived from HelpfulnessNumerator (number of people who voted the review to be helpful) and 
HelpfulnessDenominator (number of people who 'reviewed' the review). Helpfulness_perct was converted into two binary classes:  
1 for  helpful reviews (reviews with Helpfulness_perct>=75%) 0 for unhelpful reviews (reviews with Helpfulness_perct<=25%).

# 2. Workflow

1. Data preprocessing

Data type initialization, outlier treatment, balancing (due to 87% of classes being labeled positive), etc.

2. 1st analysis pipeline

Single training/validation (80%/20%) split, and single combination of parameters. Text tokenized in sngle words, word frequency calculated,
transformation to IDF vectors, logistic regression used for predictions.

3. 2nd pipeline

10-fold cross validation , with parameter grid, consisting of 27 possible parameter combinations. Text tokenized in sngle words, stopwords
removed, word frequency calculated, transformation to IDF vectors, logistic regression used for predictions.

4. Evaluation

Models were evaluated, using ROC-AUC, precision, recall, f1 scores. Finally, most significant words were found.
They were defined by the total sum of IDF.TF values, adjusted for stopwords and common words.

# 3. Conclusion

First pipeline returned satisfactory results, achieving 72% accuracy. It is well above 57% , 
which is the proportion of positive reviews, which was an initial threshold we wanted to surpass. 
Both, precision and recall, were higher for helpful class, indicating algorithms power in differentiating positive class. 
It is a little surprising, as we expected, that oversampling, will increase the discriminatory power (via duplicaton) of ''unhelpful''
features. Second pipeline, consisted of 10fold cross-validated logistic regression model, had an extra 
preprocessing step - stopword removal and parameter grids for logistic regression regularization parameter, 
maximum number of iterations, and size of vocabulary (feature) vector. 
It returned an accuracy of over 80%, with ROC-AUC score of over 90%, which we consider as being quite high. 
Precision and recall remained higher for positive class (84%; 82%), however difference was decreased, 
as could be seen from harmonic mean (f1 scores): helpful reviews (78% -> 83%); unhelpful (51% -> 67%).

We defined important words, as having high TF.IDF values. It is a combined weight, consisting of term frequency measure within review (TF) and term specificity along the corpus (IDF). 
Terms at the top of the lists, were unintended spaces, and line break symbols prevalent along the whole corpus. 
Most important verb, was ''like'' on both lists as well. ''Bags'', was the first noun on the unhelpful reviews list, 
while ''Dog'' on helpful. Possibly, indicating our shared compassion for humans' best animal friends. 
There were a lot shared words, and a lot of, not very distinctive ones. However, we had an interesting result from deducing 
important words on testing data only. ''Yahushua'', was on top25 significant ''unhelpful" words, 
after digging deeper we found it is a translation for name Jesus in Hebrew. 
There was only one review, with this term in the whole corpus. 
Review, was just a long religious rant, which was very unique, seen by a lot of people, and marked as helpful by very few. 
(very low helpfulness perct) It is interesting, that model had picked up on a term occurring in a single unhelpful review
(which of course was quadrupled). Though at least in this instance, we believe it is justifiable,  
as food reviews section is not an appropriate venue for expressing religious beliefs.

