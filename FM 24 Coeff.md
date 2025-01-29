# Introduction

Hello everyone. We're back again with a new experiment regarding most important attributes for each respective position in FM24. It's based on the same methodology as in these two topics regarding Goalkeepers in FM23 and Goalkeepers in FM24. The difference is this time we'll get most important attributes for all positions in the game and their coefficients.

#Game setup and testing environment

Game setup and testing environment is exactly the same as in the Goalkeepers in FM24 topic. Just this time we've selected only the players for each respective position.

# Results

Instead of using all 3 models, due to their similarities, I decided to use only polynomial regression to get 8 key attributes for each position. The number of attributes was selected arbitrary so the number is not too high, since most of attributes will have very minor coefficients, and not to small, so we have more attributes to compare our players.

The attributes that were excluded from the model are - Corners, Free Kick Taking, Penalty Taking, Long Throws. It was decided due to those attributes affecting the outcome. Penalty takers, no matter the position, are expected to get higher ratings due to their goalscoring potential. The same applies to other set pieces that allow players to get more goals or assist, hence higher ratings, no matter their respective position.

The 'side' positions, like ML and MR were merged since the side aspect should not affect player ratings.

The results are listed below:

|    | GK        | DLR         | DC        | WBLR        | DMC       | MLR         | MC        | AMLR        | AMC       | STC         |
| -- | --------- | ----------- | --------- | ----------- | --------- | ----------- | --------- | ----------- | --------- | ----------- |
|    | Attribute | Coefficient | Attribute | Coefficient | Attribute | Coefficient | Attribute | Coefficient | Attribute | Coefficient | Attribute | Coefficient | Attribute | Coefficient | Attribute | Coefficient | Attribute | Coefficient | Attribute | Coefficient |
| #1 | Agi       | 0,014640    | Pac       | 0,018991    | Jum       | 0,022608    | Pac       | 0,019196    | Acc       | 0,020572    | Pac | 0,022852 | Ant | 0,014011 | Pac | 0,023458 | Pac | 0,016763 | Jum | 0,024768 |
| #2 | Ref       | 0,012837    | Jum       | 0,014582    | Pac       | 0,015882    | Acc       | 0,018585    | Ant       | 0,013047    | Acc | 0,018679 | Acc | 0,012595 | Acc | 0,019640 | Acc | 0,016348 | Pac | 0,021030 |
| #3 | Aer       | 0,011812    | Acc       | 0,012670    | Acc       | 0,013536    | Jum       | 0,013761    | Sta       | 0,010352    | Dri | 0,016960 | Cmp | 0,012589 | Ant | 0,015160 | Cnt | 0,013697 | Acc | 0,015754 |
| #4 | Thr       | 0,007465    | Ant       | 0,012269    | Wor       | 0,010819    | Cmp       | 0,011762    | Jum       | 0,009796    | Tec | 0,013665 | Pac | 0,012156 | Cro | 0,014857 | Cmp | 0,012813 | Cnt | 0,014398 |
| #5 | Com       | 0,007436    | Cnt       | 0,009911    | Ant       | 0,010326    | Vis       | 0,010025    | Cmp       | 0,009470    | Jum | 0,010993 | Cro | 0,010100 | Dri | 0,013533 | Tec | 0,011647 | Dri | 0,012353 |
| #6 | Han       | 0,006255    | Dri       | 0,008246    | Pos_x     | 0,008864    | Cro       | 0,009596    | Pas       | 0,009114    | Vis | 0,010804 | Dri | 0,008134 | Jum | 0,013029 | Lon | 0,009914 | Vis | 0,011814 |
| #7 | Dec       | 0,005089    | Cmp       | 0,007720    | Pas       | 0,008826    | Wor       | 0,008166    | Lon       | 0,007952    | Cnt | 0,009576 | Jum | 0,007918 | Tec | 0,012662 | Jum | 0,009524 | Bal | 0,010338 |
| #8 | TRO       | \-0,003310  | Cro       | 0,006812    | Cnt       | 0,008358    | Det       | 0,005121    | Dri       | 0,007338    | Acc Agi | 0,003329 | Str | 0,006528 | Cmp | 0,012295 | Dri | 0,008679 | Jum^ | 0,002953 |

# How to use it?

This attributes coefficients regards difference between players actual attributes and their league average - but since we are comparing players that will potentially play in the same league we don't have to care about it. The results will be a little bit skewed towards attributes with higher coefficients but for our purpose it's acceptable.

Take any position - for example GK. Take given attribute value and multiply it by coefficient - for example 10 Agi * 0,014640 = 0,14640. Do the same for all other attributes listed. Sum the results. This is your player 'score'. You can use your existing player as a benchmark. Look for other players in your scouting view and calculate score for every one of them. Pick a player that has the highest score.

# Disclaimer

If there are 2 attributes listed, like for example 'Acc Agi' for the MLR multiply those by themselves and then use coefficient ( Acc * Agi * Coeff). Jum^ is the power of two (so Jum * Jum * Coeff).

Attributes names are corresponding with their English naming.

Pos_x is Positioning.

TRO is 'Rushing out (Tendency) - it's just how FM names that attribute when exporting.

# Example of usage

I've use this method recently to find a replacement for the goalkeeper that wanted to leave. I've found one that had little bit higher 'score' than my current goalkeeper so in theory should be proper replacement. This is the result.

Old goalkeeper that I had to sell at the beginning of the season:

![Replaced goalkeeper](https://media.invisioncic.com/Msigames/monthly_2025_01/image.thumb.png.02bd7c2eca5c20c2886387420bed6f55.png)

And his replacement results

![New goalkeeper](https://media.invisioncic.com/Msigames/monthly_2025_01/image.thumb.png.51492e3fcb1fd8d9fae6ab673e5ac32c.png)

Keep in mind that we had a recent promotion so previous goalkeeper rating of 6,91 was in 'weaker' environment while our current goalkeeper rating of 6,95 is in higher division.

 

I hope you had a good reading and will find this method useful for finding a proper replacements for your other players.
