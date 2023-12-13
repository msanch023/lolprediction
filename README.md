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

The prediction problem I will be I posed revolves around being able to predict 
whether a team won the game in League of Legends. This will be achieved by using several 
in-game metrics collected after the game. This is a binary classification problem since 
I am trying to classify the outcome of the games into either win or loss. Therefore, the 
response variable will be the result which is represented by a 1 for win and 0 for loss in 
the data.