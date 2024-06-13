# League of Legends Analysis

#Introduction
This is a project for DSC 80 at UCSD where I am analyzing a League of Legends 2023-2024 season dataset. 
The dataset I am analyzing contains statistics of League of Legends games during the 2023-2024 competitive season. The dataset contains 40440 rows × 131 columns. The question we are trying to answer is "How much of a game of League of Legends can be determined before the match even starts and how much more can be determined based on in-game statistics?"
This is an important question because using this information we can determine better draft team compositions and what to focus on during a game of League of Legends in order to come out victorious. Important columns we will be using from our dataset are 'side', 'killsat15', 'gamelength', 'datacompleteness', 'champion', 'patch', 'teamname', 'teamdeaths', 'total cs', 'totalgold', 'wardsplaced', 'dpm', 'cspm', and 'monsterkills'. Most of these features are self-explanatory, except 'side', 'killsat15', 'datacompleteness', 'total cs', 'dpm', 'cspm', and 'monsterkills', so I'll briefly explain. 'side' is either red or blue depending on which side of the map they player is on. 'killsat15' is the number of kills the player has at 15 minutes. 'total cs' is the total creep score the player has. 'dpm' is damage per minute and 'cspm' is cs per minute. Lastly, 'monsterkills' is the number of monster killed by the player which would be the jungle camps, dragons, and barons. 

#Data Cleaning and Exploratory Data Analysis


