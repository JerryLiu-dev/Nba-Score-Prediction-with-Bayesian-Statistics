# Nba-Score-Prediction-with-Bayesian-Statistics

## Motivation
Sports betting has been the holy grail of statisticians and data scientists alike to wrangle with the sheer randomness and odds. As an NBA enthusiast myself, I thought I could apply my own knowledge on the subject, along with Bayesian statistics to produce predictions that could beat the odds. The over-under situation where our betting site issues a total score of a game and you just have to guess if it is 'over' or 'under' will be used in this experiment to see if we can predict the total score more accurate than a coin flip.

## Methodology
Firstly, we will be using data from this particular 2021-2022 season, as teams have drastically changed in coaching,staff, and roster in the offseason which effectively renders previous seasons irrelevant in constructing a prior distribution. Note that we are not using multiple regression here or any deep learning models that take in features like 3 point percentage, turnover percentage, and the like to predict the points scored because betting sites like DraftKings require us to place our prediction before the game, so we cannot utilize the granular features live in the game to fit and predict scores at each time step. What we will attempt is to build a prior distribution based off of 4 distinct qualities of a team coming into any given game, and update this distribution upon the likelihood of the given team's score on the most recent games in the current month.

Here are our assumptions that we will base our priors off of:

![alt text](https://github.com/JerryLiu-dev/Nba-Score-Prediction-with-Bayesian-Statistics/blob/main/images/home%20advantage.PNG)

![alt text](https://github.com/JerryLiu-dev/Nba-Score-Prediction-with-Bayesian-Statistics/blob/main/images/rest%20advantage.PNG)

So clearly we have some imbalance when it comes to where the game is played and how rested the players are. So we will leverage our EDA to cater for the combinations of 
playing at: Home/Rested, Home/Not Rested, Away/Rested, and Away/Not Rested.

We then get filter for each circumstance and team, and we get the means and variances given any game we take note of which prior to choose based on their circumstance and generate a posterior distribution with the formula: 

$$ prior \alpha likelihood $$

which generates $$N~(\mu_1, \sigma_1^2)$$ with 

![equation](https://latex.codecogs.com/svg.image?%20%5Cmu_1%20=%20%5Csigma_1%20*%20(%5Cmu_0/%5Csigma_0%5E2%20&plus;%20%5Cbar%7Bx%7D/%5Csigma%5E2/n)) 

and $$\mu_1 = \sigma_1 * (\mu_0/\sigma_0^2 + \bar{x}/\sigma^2/n).

With the posterior Normal distributions generated for each respective team, we can combine their distributions as we know the sum of Normal Distributions is just $$N~(\mu_x+\mu_y, \sigma_x^2 + \sigma_y^2)$$.

Finally with this Summed Normal we will find the CDF, or probability that we get less than a certain point, of the score that was allotted in the betting site. From there we should know whether over or under is better.

## Results
DraftKings did well,

![alt text](https://github.com/JerryLiu-dev/Nba-Score-Prediction-with-Bayesian-Statistics/blob/main/images/nbares.PNG)

Our distribution yielded around .4999 percent will be under, which we can roughly say is 50%. So we did not do better than a coin flip and will succumb to gambler's ruin if we do this in the long term and ultimately go bankrupt in a long streak of losses.
