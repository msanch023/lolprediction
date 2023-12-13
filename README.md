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

This model uses a series of unqiue quantitative, nominal, and ordinal features to 
really highlight what a League of Legends game consists of.

### Quantitative Features: 
- `golddiffat15`: Represents the gold difference between the two teams at the 15-minute mark.
- `visionscore`: Signifies the vision score, reflecting the extent of vision control. 
Higher is better.
- `damagetochampions`: Indicates the total damage dealt to champions during the 
match by the whole team.
- `damagemitigatedperminute`: Represents the damage mitigated per minute by 
anything that shields, reduces, or in any way mitigates incoming damage.



















