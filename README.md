# Predicting Outcome of a League of Legends Game

# Introduction

This is a project for DSC 80 at UCSD where I am analyzing a League of Legends 2023-2024 season dataset. 
The dataset I am analyzing contains statistics of League of Legends games during the 2023-2024 competitive season. The dataset contains 40440 rows × 131 columns. The question we are trying to answer is "How much of a game of League of Legends can be determined before the match even starts and how much more can be determined based on in-game statistics?"
This is an important question because using this information we can determine better draft team compositions and what to focus on during a game of League of Legends in order to come out victorious. Important columns we will be using from our dataset are 'side', 'killsat15', 'gamelength', 'datacompleteness', 'champion', 'patch', 'teamname', 'teamdeaths', 'total cs', 'totalgold', 'wardsplaced', 'dpm', 'cspm', and 'monsterkills'. Most of these features are self-explanatory, except 'side', 'killsat15', 'total cs', 'dpm', 'cspm', and 'monsterkills', so I'll briefly explain. 'side' is either red or blue depending on which side of the map they player is on. 'killsat15' is the number of kills the player has at 15 minutes. 'total cs' is the total creep score the player has. 'dpm' is damage per minute and 'cspm' is cs per minute. Lastly, 'monsterkills' is the number of monster killed by the player which would be the jungle camps, dragons, and barons. 

# Data Cleaning and Exploratory Data Analysis

I cleaned the dataframe by only taking rows where the column 'datacompleteness' is equal to 'complete'. This ensured that I did not having missing values in the columns I needed for my anaylsis. 
(head of cleaned df here)

<iframe
  src="hist_killsat15.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

explanation

(two var plot)
<iframe
  src="violin_killsat15_side.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

(pivot table)
explanation

# Assessment of Missingness

I believe there are columns in my dataset that are NMAR. That would be the ban columns, which have missing values dependent on themselves. I believe this because, in League of Legends, you can choose to not ban, which could be the reason why there are missing values in NMAR. However, I feel like in pro play there should not be missed bans, so a column specifying the specific game mode could help make this column MAR since if it was blind pick and not draft, it would be MAR since there are no bans in blind pick. 
Based on my missingness permutation tests, I can see that missing values for 'killsat15' are dependent on 'datacompleteness' as the permutation test's p-value is 0.0. I did the same permutation test but with 'killsat15' and 'side', which had a p-value of 1.0 meaning that 'killsat15' being missing is not dependent on the side of the player with the missing 'killsat15'. 
(graphs I have to make)

# Hypothesis Testing

Null Hypothesis (H0): There is no significant difference in the mean 'killsat15' between the red and blue sides.
Alternative Hypothesis (H1): The mean 'killsat15' for the blue side is significantly higher than that of the red side.
The result is that we reject the null hypothesis in favor of the alternative hypothesis since the p-value is very close to zero and is lower than our chosen significance level of 0.05. 
These choices are good for answering my question since before the game even starts, we can determine that the blue side has an advantage for getting more kills by 15 minutes, which could sway the overall outcome of the match. 

# Framing a Prediction Problem

Prediction Problem: Predict how long a League of Legends game will last based on in-game stats and features before the game starts. I will be using a linear regression model to help me predict the game length. The metric I am going to use to help me determine the accuracy of my model is both the RMSE and R2 score. I am choosing RMSE because RMSE represents the square root of the average squared difference between predicted values and actual values. It is expressed in the same units as the target variable (seconds) making it easy to interpret in the context of my prediction task. There are downsides to RMSE, however, as it does not really tell me how well overall my model fits the data and the scale of the errors alone might not be very interpretable. Therefore, I am also using R-squared (R2) as it provides a measure of how well the regression model fits the observed data points. R2 takes into account both the variance explained by the model and the total variance in the data, giving a clearer indication of the proportion of variability in the dependent variable that is captured by the independent variables. This helps in assessing the overall effectiveness of the model in explaining the variability in game length based on my chosen features.

# Baseline Model
My baseline model will be a linear regression model trying to predict how long a game will last before it even happens by training on the features 'champion' and 'patch'. I believe those are significant factors when it comes to predicting how long a game will last since the champions in the game could really sway the pace/gameplay of a match and in different patches, a champion could be stronger or weaker compared to other patches. Both these features are categorical so I used One Hot Encoding to allow me to use it in my linear regression model. This baseline model aimed to predict the length of a game before it even starts, but the RMSE was around 5 minutes which is not bad. However, the R2 score is very low at 0.007, meaning that only a very small fraction of the variance in the game length can be explained by the independent variables included in the model. This indicates that the model does not effectively capture the relationships between 'champions' and 'patch' with 'gamelength'. Therefore, the model's predictive power in estimating game length based on these features is quite limited, meaning there are many improvements to make in my final model. 

# Final Model

Based on the results of my Baseline Model, I could tell that the features before a game starts is nowhere near enough to predict the length of a League of Legends game. This made me implement features based on the progression of the game and stats at the end of the game. These features include 'killsat15', 'teamdeaths', 'total cs', 'totalgold', 'wardsplaced', 'dpm', 'cspm', and 'monsterkills'. I chose these features since they are crucial information that can help determine how long a game lasts since on average these end-of-game stats /mid-game stats occur only at specific game lengths. I also incorporated the feature of 'teamname', since a teams overall skill should help determine the length of a game. These features I added improved my RMSE to around 2 minutes and 30 seconds and R2 to 0.78, which is a huge overall improvement. Overall, I could see that the best hyperparameters for predicting the length of a game are end-of-game statistics like 'total cs', 'wardsplaced', and 'monsterkills'. 

# Fairness Analysis

Choice of Group X and Group Y: Group X: 'Red' side team and Group Y: 'Blue' side team
Evaluation Metric: Root Mean Squared Error (RMSE)
Null and Alternative Hypotheses:
Null Hypothesis (H0): There is no difference in the RMSE between Group X (Red side) and Group Y (Blue side). In other words, the model performs equally well for both groups.
Alternative Hypothesis (H1): There is a difference in the RMSE between Group X (Red side) and Group Y (Blue side). This implies the model performs differently for the two groups.
Test Statistic: The difference in RMSE between Group X and Group Y.
Significance Level: 0.05
Results:
The observed difference in RMSE between Group X and Group Y: -6.437049253880531
P-value from permutation test: 0.749
Conclusion:
Given that the p-value is 0.749, which is much greater than the typical significance level of 0.05, we fail to reject the null hypothesis. This means we do not have sufficient evidence to conclude that there is a significant difference in model performance (in terms of RMSE) between the Red team and the Blue team. In other words, the model performs similarly for both groups, indicating no substantial fairness issue in this context. However, based on the RMSE, we can see that on average our model does predict Blue side games better, but again this is not significant enough to prove that our model is unfair. 









