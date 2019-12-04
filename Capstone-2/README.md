#
# Springboard Data Science Capstone Project 2 – Recommendation System for Video Games

[Slide Deck](https://github.com/dgokalga/Springboard-Data-Science/blob/master/Capstone-2/Capstone2_Slides.pdf)

[Blog Post](https://github.com/dgokalga/Springboard-Data-Science/blob/master/Capstone-2/Capstone2_blog.pdf)

[Project Report](https://github.com/dgokalga/Springboard-Data-Science/blob/master/Capstone-2/Capstone2_Final_Report.pdf)

## **Data Problem**

Currently, approximately 2.2 billion gamers worldwide exist (a third of the world’s population!), with that number expected to rise to 2.7 by 2021. Often then not, a common issue that arises for gamers, with and without a rich gaming history, is deciding what to play next. Perhaps we want to enjoy games most similar to our previously played games, but how can we comprise a list of games to achieve that. Maybe be can look at other users’ histories with games and decipher whether we would also enjoy games played by these other users.  
Just like with TV and movies, games exhibit qualities and concepts that databases have taken note of and stored, as well as, other user’s ratings and reviews for these games, which can be utilized to decide which game to play next. Recommendation systems opt to alleviate this issue of deciding which games to play, by providing a list of games that the user will most likely enjoy based on previously played games and the history of similar users. The recommendation system is built on two main components: content based filtering and collaborative based filtering.  

The idea behind content-based filtering is that the system will recommend games, who content is most like the target game. For example, if I enjoy the game Halo, I would be recommended Halo 2, because it shares the same characters, genre, concepts, etc., or I would be recommended Destiny and Destiny 2, because they share the same developer, Bungie. This filtering method relies on the NLP technique, TF-IDF, in which keywords and properties of the game are taken into consideration. 

The idea behind collaborative-based filtering is that the system will predict ratings for unrated games based on the taste of similar users. For example, if I enjoy the game Halo (rating it 5 star), and you enjoy the games Halo and Mario Party (rating them a 5 star), I will most likely enjoy Mario Party. The filtering method will rely of model-based approaches, such as clustering algorithms and matrix factorization algorithms, to predict ratings for unrated games.  

## **Potential Clients**

My target audiences are:

Avid Gamers who have trouble determining which games to play next, or find enjoyment with games similar to the ones that they enjoy.

## **Dataset Description**

  24023 game reviews were extracted from the API provided by the largest gaming database online, [GiantBomb](https://www.giantbomb.com/). 

  User ratings, on a scale of 0-5, are given from a total of 6561 users to 4223 games, ranging from July 2008 to September 2019. Database also provided game features: genres, themes, concepts, platforms, developers, publishers, developers, short and long game description. Luckily, an additional feature, “similar-games”, provides a list of games that GiantBomb deemed similar to the target game. This feature will be used to evaluate how the content-based filtering system recommends games.  

## **Wrangling Steps**

  1. All of the game feature names were nested within lists, which included API links and unique feature ids. Only the names were extracted from these lists, as there can be more than one feature name for each game, as these will be involved in the TF-IDF implementation in finding similarities between game content. Names with spaces between their words, such as the genre “First Person Shooter”, were connected with a dash to prevent splitting of descriptive words: “First-Person-Shooter”. HTML tags and stopwords (commonly used words) were removed from the user review features and game descriptions to keep the unique and informative words that describe each game. All descriptive feature values were converted to lower-case, to prevent duplicates of different case lettering. Any ‘s’ or ‘ies’ were removed from word endings, in order to convert plural cases to singular, and lessen the duplicates of plural and singular case letterings There were 1417 reviews missing a value for the ‘similar games’ feature, approximately 5% of the data, which were omitted. 

  2. While all game reviews were given a score from 0 to 5 (whole number), two reviews were given a score of 0.5, for which many of the content and user features were given the value ‘test’, and as a result, removed from the dataset. All other missing values were replaced with an empty space and ready to be merged to create a ‘bag of words’ instance, which will be used in the TF-IDF implementation. To ensure that the evaluator feature, ‘similar games’, list only included games from the reviews, games that were identified to not be part of the list of total unique games (games which cannot be recommended) were removed from each “similar games” list. This process was repeated until each review record’s ‘similar games’ list had at least one game that can be found in the list of total unique games in the dataset. After the removal process, approximately 1500 review records had no games within their ‘similar games’ feature, and were omitted from the final dataset. After the wrangling process, the final dataset contains 22866 reviews from 3592 users on 4220 games.

## **Data Exploration Insights**

To summarize the findings from the exploratory data analysis of the game review data:

  Ratings and Reviews: The dataset is heavily skewed towards positively rated games (many 4’s and 5’s). Many users have only rated one game, and many games have only been reviewed upon one time.  

  Genres, Themes, and Concept: Majority of games reviewed upon were of genres: Action and Action-Adventure, themes: Fantasy and Sci-Fi, concepts: Achievements and Polygonal-3D. Highly associated popular genres include: Shooter with Action,  popular themes: Post-Apocalyptic with Sci-Fi. The percentage of games, in the similar games list, that are of the same genre, theme, or concept to the target game is relatively high, compared to the others features, indicating these features will be highly considered when recommending games evaluated by the similar games list. This does not mean that the other features, in general, should not be used to recommend games.  

  Characters and Franchises: The percentage of games, in the similar games list, that are of the same character or franchise is extremely low, indicating that these features should not be included in recommending game.by the content-based system. Once again, this does not mean these features, in general, should not be included, but for the purposes of evaluating the system with the similar-games list, they may not be. 

  Platforms, Publishers, and Developers: Strong association between most consoles and PC exists, as most games are available digitally on PC, and games overlap between Playstation and Xbox (few exclusives). The percentage of games, in the similar games list, that are of the same platform as the target game is relatively high, indicating that it will be an important feature to include when recommending games. However, the percentage of games, in the similar games list, that are of the same publisher, or developer, is extremely low. 

  Term Frequency: Looking at the frequency of terms, the popular terms all pertain to general gaming language, such as: ‘game’, ‘player’, ‘character’, ‘xbox 360’, etc. Action terms dominate: ‘weapon’, ‘enemies’, ‘first person shooter’, ‘real time combat’, etc. 

  Details on the implementation of the data analysis and visualization can be found in [this](https://github.com/dgokalga/Springboard-Data-Science/blob/master/Capstone-2/Capstone2_EDA.ipynb) IPython notebook.

## **Predictive Modeling Insights**

  To summarize the findings from the predictive modeling for the implementation of the recommender system:

  Content based filtering: The optimal combination (bag of words) of features, when evaluated by the similar games list, were genres, themes, concepts, platforms, developer, and short game summary. While this produced the best metrics, the similar games list seems to have unusual game choices, that doesn’t seem similar to the target game at hand. I would recommend the user to do some prior research on the games recommended and determine whether they deem those games similar to their favorite games, before deciding to play them, because the target variable similar games list has limits and flaws. 

  Collaborative based filtering: Out of all CF algorithms ran on dataset, SVDpp and SVD produced the lowest error, RMSE and MAE, indicating they are the optimal algorithms to run for this dataset. According to the residual plot, the system overpredicts as a result of the uneven distribution of the ratings (heavily skewed to positive ratings). A balanced dataset with negative and positive reviews would prevent this issue and produce better ratings predictions than this skewed dataset. In this case, content-based filtering is the preferable method to recommending the games.

  Details on the implementation of the modeling and analysis can be found in [this](https://github.com/dgokalga/Springboard-Data-Science/blob/master/Capstone-2/Capstone2_Modeling.ipynb) IPython notebook.

## **Future Work**

  For the content-based filtering method, while the game features provide substantial information in describing each of the 4223 unique games, the recommendations can always expand with added new games and their game information. A new evaluator list for the recommendations should also be determined, possibly through surveying gamers to determine what they feel are the most similar games to the games they have played, and strengthening how accurate the system will recommend games. Multiple surveys of similar games should provide enough evidence to determine a consensus list of similar games that can be used to evaluate the system. 

  For the collaborative-based filtering method, the dataset lacks negatively rated games, causing the system to overpredict ratings, so adding more reviews for these games may help alleviate this issue, but if not, will increase the amount of reviews for each game to make better rating predictions. Another idea that could be implemented is a hybrid system that combines collaborative based filtering with content based filtering, and add sentiment analysis, which would use the user review feature to recommend games tailored towards the individual querying the system. 
