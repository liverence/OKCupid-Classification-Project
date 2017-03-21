# OKCupid-Classification-Project

This project uses a large, anonymized dataset of users' data from OKCupid, collected by Emil Kirkegaard, a student researcher at the University of Aarhus. The original datafile, user_data_public.csv is 968.6 MB, containing approximately 70,000 rows (1 row for each user) and >2500 columns of user data scraped from OKCupid profiles. [Unfortunately, the dataset is too large to post to github, and is also no longer available from the researcher's website (http://emilkirkegaard.dk/en/?p=5942), but can be obtained by contacting him directly.]

The data columns contain both concrete biographical data (e.g., each user's self-reported age, gender label, sexual orientation, geographical location, education level, employment field, etc.) as well as the answers provided by each user to thousands of subjective personality and dating questions (e.g., "Which would you rather master, art or science?", or "In your ideal sexual encounter, do you take control, or do they?"). These questions, used by OKCupid algorithms to "match" users based on hypothetical compatibility, provide a potentially fascinating trove of data about human personality and relationship psychology.

My overarching goal in this data science project was to explore the dating question data from the standpoint of the concrete biographical data. For example, which questions yielded the most divergent responses for (heterosexual) men vs. women, for users under age 40 vs. age 40 and above, etc. Furthermore, I wanted to explore the performance of various machine learning classifiers in the context of this extremely large, sparse, and messy dataset. How well could a classifier, trained on the question data, do at guessing a user's gender, age, or even geographical location, and which classifers do best?

After loading in the dataset, I first did some data cleaning (the dataset contains many NA values, and also question data are encoded as actual text responses, like "I prefer they take control", so I convert these to integers) and calculate the number of questions each user answered (which ranges anywhere from 0 to >2200).

Unsurprisingly, the models don't do well at learning from or classifying users who have not answered very many questions, so I exclude from further analysis users who didn't answer at least 500, leaving me with ~29,000 users. I plot some histograms of these results.

Next, for each of 7 psychologically-interesting experimental categories, I group users into one of 3 classes. For example, for gender, I group users into either "Heterosexual male", "Heterosexual female", or "Other" based on their profile data (not out of any animus towards the "Others", but simply because I thought heterosexual male vs. female might make for a particularly rich comparison). For each category, I create a new datacolumn that assigns each user a 1 (class 1), 2 (class 2), or 0 (other); classifiers for each category will be trained to classify 1 vs. 2, excluding 0s. The complete list of experimental categories is:
1. Gender: Hetero Female vs. Hetero Male
2. Age: Under 40 vs. 40 and over
3. Education level: High (Graduate degree) vs. Low (2-year college degree or less)
4. Job: High income vs. Low income
5. Body type: Ideal (e.g., athletic, thin) vs. Non Ideal (overweight)^
6. Drug use: Uses drugs often or sometimes vs. Never uses drugs
7. Location: New York vs. California

^Note: It was not my intention to offend anyone with this categorization. There is a rich body of psychological work on how body type influences self-esteem, and I suspected that users who view themselves as having "ideal" body types might have different attitudes than users with different body types.

I also included 2 control categories, at which classifiers should be at chance if they are working appropriately:
1. Random: 1 vs. 2 randomly assigned to each user (inherently meaningless)
2. Astrological sign: Virgo vs. Aquarius (My divorced parents' astrological symbols. I am told that, astrologically, these groups are so different as to be romantically incompatible, but I suspect this is also a meaningless category and should also produce chance classification performance!)

For each category, I plot a pie chart illustrating counts and proportions of users in each of the 3 classes, and also calculate for each of the ~2200 questions a "divergence factor" (from 0-1, with 0 indicating that the 2 classes answered in exactly the same way, and 1 indicating perfect divergence) and plot the top 5 most divergent questions as bar charts. This analysis yields some particularly fascinating insights, producing results that often match our intuitions but are sometimes also quite surprising.

Finally, for each category I train and test four different classifiers (Naive Bayes, SVM, Ridge Classifier, and Perceptron) on a 50/50 split of the data, and plot the results at the bottom. Thankfully, the 2 control categories produced chance performance for all 4 classifiers. Interestingly, all 4 classifiers are able to achieve above-chance performance for every category, though Naive Bayes performs far worse than the other three. While it may be unsurprising that men vs. women, or younger vs. older users, give such divergent responses that a classifier can distinguish them, this exercise finds that classifiers can even be trained to distinguish New York vs. California based users (which seems quite surprising, given how similar demographically these two states are)!



