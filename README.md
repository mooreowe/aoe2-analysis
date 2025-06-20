## Introduction

Age of Empires II is a popular RTS (real-time strategy) game, currently developed by Microsoft under World's Edge studios and the Forgotten Empires team. The ranked mode of the game pits you (and optionally your team) against another team, based on elo. Everyone picks a civilization before the match begins, which has unique bonuses and access to different units, with the goal of building an empire to defeat your opponent. \
In 2v2 games, players typically pick civilizations according to the map and in order to cover each others' weaknesses. This project aims to answer this question: **Is the winner of a 2v2 game already decided by just the civilizations, map, and elo of the players?** In other words, are matches usually "civ wins" where one team has the clear advantage, and are elo disparities strong enough to sway the game before any play has begun? \

# Data
First, a big thank you to jerbot from [aoestats.io](https://aoestats.io/) for creating a community API endpoint for access to the data. This analysis is only possible because of this! \ \
The initial dataset has data from the last 5 weeks of games, from 5/11/25 to 6/14/25. In total, there were _ games, with a total of _ player instances. The data is separated into one dataframe of information on games, and one with information on specific players. \\
The dataset has many columns (you can read a full description [here](https://aoestats.io/)). From the match dataframe, I will use the following columns: \
- `map` - string: contains the map the game was played on.
- `game_id` - string: internal id of the game - used to merge with the player dataframe.
- `avg_elo` - float: average elo of players in the match.
- `team_0_elo` - float: average elo of one team (team 0) in the match.
- `team_1_elo` - float: average elo of the other team (team 1) in the match.
- `raw_match_type` - integer: contains an integer referring what mode the game was (1v1, 2v2, etc.)
- `duration` - timestamp: contains duration of the game. \
From the player dataframe, I will use the following: \
- `winner` - boolean: if the player won.
- `game_id` - string: used to merge with match dataframe.
- `team` - int: which team the player was on.
- `civ` - string: which civilization the player was playing.
- `old_rating` - float: player elo before the match was played.
All columns of the dataframes and more information can be found [here](https://aoestats.io/api-info/). \

## Data Cleaning and Exploratory Data Analysis
