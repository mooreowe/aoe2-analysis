# Introduction

Age of Empires II is a popular RTS (real-time strategy) game, currently developed by Microsoft under World's Edge studios and the Forgotten Empires team. The ranked mode of the game pits you (and optionally your team) against another team, based on elo. Everyone picks a civilization before the match begins, which has unique bonuses and access to different units, with the goal of building an empire to defeat your opponent. <br>
In 2v2 games, players pick civilizations according to the map and in order to cover each others' weaknesses. This project aims to answer this question: **Is the winner of a 2v2 game already decided by just the civilizations, map, and elo of the players?** In other words, are matches usually "civ wins" where one team has the clear advantage, and are elo disparities strong enough to sway the game before any play has begun? <br>

### Data
First, a big thank you to jerbot from [aoestats.io](https://aoestats.io/) for creating a community API endpoint for access to the data. This analysis is only possible because of this! <br>
<br>
The initial dataset has data from the last 5 weeks of games, from 5/11/25 to 6/14/25. In total, there were _ games, with a total of _ player instances. The data is separated into one dataframe of information on games, and one with information on specific players. <br>
The dataset has many columns (you can read a full description [here](https://aoestats.io/)). From the match dataframe, I will use the following columns: <br>
- `map` - string: contains the map the game was played on.
- `game_id` - string: internal id of the game - used to merge with the player dataframe.
- `avg_elo` - float: average elo of players in the match.
- `team_0_elo` - float: average elo of one team (team 0) in the match.
- `team_1_elo` - float: average elo of the other team (team 1) in the match.
- `raw_match_type` - integer: contains an integer referring what mode the game was (1v1, 2v2, etc.)
- `duration` - timestamp: contains duration of the game.  
From the player dataframe, I will use the following: <br> <br>
- `winner` - boolean: if the player won.
- `game_id` - string: used to merge with match dataframe.
- `team` - int: which team the player was on.
- `civ` - string: which civilization the player was playing.
- `old_rating` - float: player elo before the match was played.
All columns of the dataframes and more information can be found [here](https://aoestats.io/api-info/). <br>

# Data Cleaning and Exploratory Data Analysis

### Cleaning

This data was actually fairly clean. To start, I filtered to only contain 2v2 games, since that's the question I'm trying to answer. <br> 
Then, I filtered out any games that lasted less than 2 minutes, since occasionally the servers or someones internet crash at the very beginning of the game, in which case the data is meaningless because the winner is random. <br>
Finally, there were a few games that were marked as 2v2 games but had 3 players, which there is no gamemode with that amount. So, I filtered those out. <br>
All told, this filtering reduced the number of 2v2 games from 169028 to 160234. All but 3 of those filtered out games were less than 2 minutes long likely due to disconnect. <br>
Then, I created three new columns. Two of them contained the civilization pairs for either team, sorted alphabetically. The other column was just a true/false for which team won the game. So, the final cleaned data looked like this: <br> <br>

| map    |   game_id |   avg_elo |   team_0_elo |   team_1_elo |   raw_match_type | duration               | team0_civs         | team1_civs           |   winning_team |
|:-------|----------:|----------:|-------------:|-------------:|-----------------:|:-----------------------|:-------------------|:---------------------|---------------:|
| arabia | 398269888 |   1102.75 |       1084.5 |       1121   |                7 | 0 days 00:21:08.200000 | celts cumans       | armenians mongols    |              1 |
| nomad  | 398270650 |    961.5  |        954   |        969   |                7 | 0 days 00:32:02.700000 | koreans vikings    | dravidians saracens  |              0 |
| arabia | 398269639 |   1199    |       1189.5 |       1208.5 |                7 | 0 days 00:40:51.400000 | ethiopians wei     | magyars mongols      |              0 |
| arena  | 398269620 |   1051.5  |       1044.5 |       1058.5 |                7 | 0 days 00:47:25.800000 | franks khitans     | bohemians jurchens   |              1 |
| arabia | 398268971 |   1055.5  |       1021.5 |       1089.5 |                7 | 0 days 00:31:59.300000 | ethiopians teutons | italians lithuanians |              0 |

<br> <br>

The following bar chart shows the distribution of civilization picks across all 2v2 games in the dataset. Some civilizations have much lower pickrates than others, and a few stand out as very popular. <br>

<iframe
src="assets/civ_picks.html"
width="400"
height="600"
frameborder="0"
></iframe>
