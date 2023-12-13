# lolprediction

## By Marco Sanchez

This is a website containing the report for a project done in DSC 80 at UCSD 
where I proposed a prediction problem around the game League of legends 
and trained a baseline and final model to make predictions. I also performed 
a fairness analysis at the end of the project to see if the final model performed 
better or worse for certain groups.

## Introduction

The data set I used was taken from [Oracle's Elixer](https://oracleselixir.com/tools/downloads) and contains information on
professional league of legends matches from 2022. This data set contains
149401 rows and 56 columns. There are 12 rows per game with 10 of the
rows containing information about the players and the last 2 containing
information about the teams in the match.

## Framing the problem

The prediction problem I posed revolves around being able to predict 
whether a team won the game in League of Legends. This question will be answered by using several 
in-game metrics collected after the game since I want my model to be able 
to make these predictions with data that represents the whole scope of the 
game that was played. This is a binary classification problem since 
I am trying to classify the outcome of the games into either win or loss which is 
represented by a 1 for win and 0 for loss in 
the data. The metric I will use to evaluate the model will be accuracy because it 
works the best in a binary classification case like this where I just want to see the 
ratio of correct to the total number of predictions made.


## Baseline Model

The predictive model I used was the Random Tree Classifier because of its effectiveness 
in prediction tasks. I think that the way it minimizes over fitting while providing a 
natural way of deducing the importance of different features when making predictions 
made it a good fit for this project. 

The baseline model uses two very general features which can be used to deduce the 
overall gold difference around 15 minutes or the middle point of the game. The team
that has a gold lead at this point usually goes on to win the game.


### Quantitative Features:
- `golddiffat15`: Represents the gold difference between the two teams at the 15-minute mark.

### Ordinal Features:
- `firsttothreetowers`: Represents the team which was able to destroy 3 towers first. 
This nets the team a very large chunk of gold.

### Response Variable:
- `Result`: Represents whether a team won or lost the game.


### Encoding: 
I used `StandardScaler()` on `golddiffat15` in order to ensure a set of normalized 
values that wont infuence the model in the bad way since it might have different scale 
compared to other features. 
For `firsttothreetowers` I used `OneHotEncoder()` to ensure the model would be able 
to understand what kind of categorical variable it was working with.


### Performance:

By using accuracy as my metric to assess the performance of my model, I was able to find 
that the model had around a 70% accuracy on predicting whether a team won or lost the game 
using just two features. While this accuracy might seem high, League of Legends provides an 
abundance of features I could have used in order to achieve a much higher accuracy. 
I used this baseline model to gauge the effect that these two feature would have 
and to get a general idea of how many more features I might want to add in the 
final model. This is a fine baseline but I know I can do much better.


## Final Model

I decided to stick with the Random Tree Classifier for my final model and instead 
opted to add more features for it to work with. For this model I added 6 more features 
in an attempt to really capture the essence of a game in League of Legends game while 
using interesting features instead of just looking at features that would be a dead giveaway 
such as the difference in gold or kills at the end of the game. With the new features 
I picked out, to the naked eye either team could have won the game but I want the model 
to be able to use the slight differences to make accurate predictions based on "unusual" 
data.


### Quantitative Features: 
- `golddiffat15`: Represents the gold difference between the two teams at the 15-minute mark.
- **(New)**`visionscore`: Signifies the vision score, reflecting the extent of vision control. 
Higher is better.
- **(New)**`damagetochampions`: Indicates the total damage dealt to champions during the 
match by the whole team.
- **(New)**`damagemitigatedperminute`: Represents the damage mitigated per minute by 
anything that shields, reduces, or in any way mitigates incoming damage.

### Ordinal Features:
- `firsttothreetowers`: Represents the team which was able to destroy 3 towers first. 
This nets the team a very large chunk of gold.

### Nominal Features:
- **(New)**`dragons`: Represents the number of dragons secured by a team.
- **(New)**`barons`: Represents the number of barons secured by a team.
- **(New)**`side`: Denotes the team's side, with values 'Blue' or 'Red'.

### Response Variable:
- `Result`: Represents whether a team won or lost the game.

### Hyperparameter Tuning: 

I ended up keeping it simple and used to `GridSearchCV()` to tune my hyper parameters. 
These were the best performing hyperparameters: 
 - `classifier__max_depth`: 10
 - `classifier__n_estimators`: 100
 
 
### Performance:

The same model with added features ended up performing much better than the baseline 
model with an accuracy of around 90%. In a game like League of Legends which has 
hundred of variables per game I think that a 90% accuracy while only using 8 interesting 
features is quite good.

<iframe src="assets/cm.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/roc.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/damagemitigatedperminute.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/barons.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/damagetochampions.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/dragons.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/firsttothreetowers.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/golddiffat15.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/side.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/visionscore.html" width=800 height=600 frameBorder=0></iframe>



## Fairness Analysis

### Groups:

Group X: `blue side`

Group Y: `red side`

There are several advantages and disadvantages between each side in League of Legends. 
For example blue side gets first pick when drafting champions, it is safer for the 
blue side to do baron since the pit is facing them, and arguably blue side has a better 
camera angle. 
In contrast red side has a safer time doing dragons for the same reason blue side 
has a safer time doing barons and red also gets to draft the last champion which 
means they can make counter picks. 

Here is a diagram of the map explaining the pits of baron and dragon. Natural 
advantages and disadvantages are created since the map is not mirrored. If you 
look closely the pits of baron and dragon face a certain direction which means 
that the opposing side has to get over a wall or walk around to get access. This 
might sound simple but is in fact a huge factor in whether a team gets the objective 
or not.

![Neutral Objectives](/assets/Neutral-objectives-in-the-LOL-map.png)
Scientific Figure on ResearchGate. Available from: https://www.researchgate.net/figure/Neutral-objectives-in-the-LOL-map_fig3_367962142 [accessed 13 Dec, 2023]

### Evaluation Metric:

The evaluation metric I used was precision so that I could see the accuracy of 
positive predictions.

### Hypothesis Testing:

**Hypotheses**

`Null`: The model is fair and there is no difference in precision between the two groups.
`Alternative`: The model is not fair and the precision for one group is different from the other.

**Factors**

`Observed Test Statistic`: The difference in precision between the two groups.

`Significance Level`: α = 0.05

**Results**

`Test Statistic`: -0.0242
`P-Value`: 0.0120

<iframe src="assets/fairness.html" width=800 height=600 frameBorder=0></iframe>

In conclusion, there is statistical evidence pointing to an unfairness between the two groups. 
With a p-value lower than the significance level, there is evidence to reject the null 
hypothesis. 











































