# FM_ML_Model
Machine Learning models for Football Manager

# Attributes influence on average rating for outfield players in various domestic leagues

## 1. Introduction

In this experiment we will try to find if there is any correlation between selected attributes (including visible, hidden and things like Height/Weight/foot proficiency) and average rating for outfield players, regardless of their position, in various domestic leagues.    Average rating was selected arbitrary as players indicator of performance. It has its pros and cons, for example can vary a lot depends on players position, but overall, it was selected due to its versatility and status being probably the most vital statistic regarding players performance rating.

The test will include checking the correlation of the attributes and checking if a regression model will be able to find any significant factors that influence players average rating.

## 2. Game setup and testing environment

The experiment should measure players performance in domestic league since it is the most common and important competition that player will face while also having very high opponents level stability in oppose to cup games (including continental competition) and international fixtures. Since in regular case the average rating for a player includes cups and international games, and due to improving simulation processing time, it was decided to prepare a very specific database for the simulation.

Database prepared for the simulation (using built-in pre-game editor) got removed all the cup, international and friendly competition from the entire game. The database includes domestic leagues only.

The game was intended to be loaded with all leagues included in the default database that have similar season calendar (autumn -> summer) and based in Europe. This was done due to cohesion of the data extraction date. Every single season had data extracted at the end of the May so after most of the leagues were finished or almost finished. As a result, the game was loaded with 70 leagues from 25 countries listed below:


![View of the leagues loaded](https://media.invisioncic.com/Msigames/monthly_2024_11/leagues.thumb.png.8aae5e8246a0515c39f980141728b66b.png)

All the leagues had set detail level to ‘All senior competitive matches’ to use ‘full match engine’ instead of ‘quick match engine’ to make results similar to the actual player experience.

## 3. Results

The game was processed 11 seasons into the game. For each season the data was collected as said at the end of the May. The data included players data and the domestic leagues ranking.

The players data included:

- information that cannot be removed from scouting view
- club
- division
- technical attributes
- mental attributes
- physical attributes
- left/right foot proficiency
- age
- weight
- height
- hidden attributes
- average rating

Leagues data included:

- league current position
- league previous position
- change
- league name
- nation
- reputation

The filter for the players included some specific criteria:

- players were no-GK (only outfield players)
- only players from the playable leagues
- players with more than 1000 minutes played during the season

The combined result was almost **200k observations** of players from the playable leagues.

### 3.1 Data preprocessing

The raw data from each season has to be pre-processed. Pre-processing was mostly cleaning the data and changing some of the information to the more model-friendly types – for example foot proficiency is visible in the game as description (from Very Weak to Very Strong) so the foot proficiency description was mapped into integer values.

Another issue that come up was that it would be very difficult for the model to find a relationship between players and their average rating across different leagues. Example – player from Vanarama National with low attributes might have the same average rating as other player with much higher attributes playing in English Premier League. For the model it could be potentially confusing – why the two players with vastly different attributes have similar output value. To tackle this issue the form of normalization of data was executed on the data. This included calculating the average attributes value for every league (actually by the league rank not the league per se) and then getting for every single player another set of attributes that was difference between player actual attributes values and the average for his league. Example – if the league average for Crossing is 10 and the player actual value for this attribute is 12 his ‘difference’ value would be +2.

This way we can get rid of directly including league or league rank information and focus on the players and their abilities in relation to their respective leagues.

### 3.2 Correlation matrix

The first step into analysis was making the correlation matrix for all the attributes included in the test. The result is following table:

![Correlation matrix](https://media.invisioncic.com/Msigames/monthly_2024_11/correlation_matrix.thumb.png.72b6cd6c344911975048363766443aca.png)

We can clearly see occurrence of some attributes groups, mostly depending on the players position. Example – high coefficients between Off The Ball and Dribbling, Finishing, First Touch. This indicates attributes that often show up together for offensive players.

In my opinion some of the most interesting correlations due to their appearance and/or value are:

- Weight with strength and balance
- Pace with acceleration and agility
- Age and consistency – for this one community already knew that there was relationship between those two attributes but the coefficient being as high as 0.59 points was quite surprising

### 3.3 Correlation coefficients for Average Rating

The most interesting part is a list of corelation coefficients for the Average Rating. To make reading easier they were sorted descending.


![Correlation coefficients for Average Rating](https://media.invisioncic.com/Msigames/monthly_2024_11/av_rat_coeff.thumb.png.7ad6de911b7e512659060a170c1cc43e.png)

If we exclude height and weight – since they are correlated with each other and height being correlated with jumping reach – we can see that two attributes with the highest correlation are Jumping Reach and Strength. Then we have Concentration and Anticipation next to Balance, Heading and only after those there is Pace. It’s quite surprising since Pace was for very long time considered one of the most important attributes for a player.

On the other side of the list, we can see Controversy and Dirtiness that are self-explanatory. Quite interesting is negative correlation of Left Foot proficiency. It might be related to larger sample size of right footed players than left footed. Also worth noticing is negative correlation of Flair on average rating. This is really surprising since Flair is associated with offensive players that tends to get higher ratings than defensive players.

### 3.4 Models output

3 types of models were built on the data. First one was simple linear regression, second one was linear regression LASSO type, and the third one was polynomial regression.

To choose specific variance of each model there was introduced metric of cross validated MSE (Meas Square Error) on relation to number of features included. For all three models the sub-models that were selected were ones with around 10 features included. The pick of that particular number was arbitrary due to the limited number of attributes that are usually included while scouting for the player.

For simple regression model selected model had exactly 10 features included. The results are listed by their coefficient below:

![Simple linear regression models results](https://i.imgur.com/djlBN51.png)

| # | Feature   | Coefficient |
|---|-----------|-------------|
| 0 | Jum_diff  | 0.020670    |
| 1 | Pac_diff  | 0.017367    |
| 6 | Acc_diff  | 0.014285    |
| 3 | Cnt_diff  | 0.012516    |
| 5 | Ant_diff  | 0.010305    |
| 7 | Bal_diff  | 0.010155    |
| 9 | Pas_diff  | 0.009462    |
| 2 | Vis_diff  | 0.006812    |
| 8 | Cons_diff | 0.005691    |
| 4 | Dri_diff  | 0.005664    |

As we can see the most important attribute in this model is Jumping Reach followed by Pace and Acceleration. Then we finally get mental attributes Concentration and Anticipation followed by another Physical attributes – Balance. The list ends with Passing, Vision, Consistency and Dribbling.

For linear regression LASSO type selected model had 13 features. The results are listed by their coefficient below:

![LASSO models results](https://i.imgur.com/xzThDrA.png)
![LASSO models CV MSE](https://i.imgur.com/qPlutx7.png)

| Attribute | Value    |
|-----------|----------|
| Jum_diff  | 0.012278 |
| Pac_diff  | 0.007023 |
| Cnt_diff  | 0.003997 |
| Str_diff  | 0.003354 |
| Vis_diff  | 0.002642 |
| Bal_diff  | 0.001789 |
| Cons_diff | 0.001230 |
| Dri_diff  | 0.001196 |
| Agi_diff  | 0.001118 |
| Pas_diff  | 0.001107 |
| Acc_diff  | 0.000835 |
| Pen_diff  | 0.000733 |
| Lon_diff  | 0.000395 |

Again the 2 most important attributes are Jumping Reach and Pace but there is change on the last step of the podium with Concentration going one place higher than in previous model. Interesting observation is low coefficient for Acceleration compared to the previous model. Also, Strength kind of replacing Balance is worth noticing – since Strength was not included at all in simple linear regression model.

For the polynomial regression selected model had 7 features, since models with higher number of features had very low coefficients on them. The results are listed by their coefficient below:

![Polynomial regression models results](https://i.imgur.com/FGfrpO0.png)

| # | Feature    | Coefficient |
|---|------------|-------------|
| 1 | Pac_diff   | 0.027069    |
| 0 | Jum_diff   | 0.022109    |
| 3 | Cnt_diff   | 0.013298    |
| 6 | Ant_diff   | 0.012319    |
| 2 | Vis_diff   | 0.011816    |
| 5 | Dri_diff   | 0.010321    |
| 4 | Jum_diff^2 | 0.002351    |

For polynomial model the Pace is king. It sometimes swaps places with Jumping Reach but when it finally appears in the model it’s usually higher or very, very close to Jumping Reach.

Again those 2 physical attributes are followed by Concentration and Anticipation from Mental attributes with Vision and Dribbling later on the list. Interesting observation is that the last feature is Jumping Reach to the power of 2. This shows the importance of Jumping Reach being higher than most of the other attributes.

## 4. Conclusion – is Pace still a King?

As we could see for the results the most important attributes across all the models are Pace and Jumping Reach occupying top 2 sometimes switching places across the models and sub-models. Keeping in mind that height is also correlated with high weight, balance and strength we have rough recipe for high rated players. Tall, rather heavy (thus strong and with good balance) and on top of that pacey (any particular player of that characteristic in mind?). The superiority of physical attributes follows the community observations, while also Jumping Reach being surprising one. Also worth noticing that from mental attributes the most important ones are Anticipation and Concentration. These 2 are generally very universal attributes that were already highly valued by the players.

Please keep in mind that these models are not perfect. They are only approximation of the data they were feed. There are obviously many concerns regarding the models and data quality. For example, there is high probability that the data is flooded with players with relatively low CA – remember that many lower leagues tend to have sub-divisions. The model will try to fit to most of the data and not for some certain leagues. It is also easier to be outstanding player in lower league than in one of the top 5 leagues. The ‘attribute difference’ approach is also debatable since we do not know if attributes impact is linear or not. Example – is impact of a player with 15 pace in a league with average pace of 10 the same as player with 20 pace in a league with average pace of 15? We don’t know that but from the data and the models we can get a sneak peek in what attributes gives the best increase in average rating. The rating itself can also be skewed towards certain positions – I remember some editions where forwards and centre-backs had on average higher ratings than for example midfielders who rarely got higher average ratings than 7.

I hope you all enjoyed the work. I wanted to also make such models for particular positions. The data is there but sadly I very little free time, but I’ll try to deliver it soon. The same goes with FM 23 edition. This experiment started a while ago, with the most current edition I had at the time but since I purchased FM24 I can say that I already run the simulation similar way as here. So, the data is already there I just have to extract it and prepare for another experiment. So, stay tuned and hope you’ll find it equally interesting as this one.
