---
title: "Analysis of 20,000 Chess Games"
author: "Davon Person"
date: "09/20/2019"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
## install and load the necessary packages

```

*Note*: If you try to Knit this document at this time, you *will* get an error because there is code in this document that has to be edited (by you!) before it will be able to successfully knit!

## Final Project

This exercise has been generated to practice everything you have learned in this course set.

### GitHub Setup

To get started, you'll want to go to GitHub and start a new repository:

- Call this repository `final_project`. 
- Add a short description
- Check the box to "Initialize this repository with a README. 
- Click `Create Repository`

Once the repository has been created, Click on `Clone or download` and copy the "Clone with HTTPS" link provided. You'll use this to clone your repo in RStudio Cloud.

**Note**: If you're stuck on this, these steps were covered in detail in an earlier course: [Version Control](https://leanpub.com/universities/courses/jhu/version-control). Refer to the materials in this course if you're stuck on this part of the project.

### RStudio Cloud Setup

Go to RStudio Coud and create a new project based on Github. As discussed previously, you'll want all your data science projects to be organized from the very beginning. Let's do that now!

First, use `cd` to get yourself into the directory of your GitHub Project.  

Once in the correct directory, use `mkdir` in the terminal to create folders with the following structure:

- data/
  - raw_data/
  - tidy_data/
- code/
  - raw_code/
  - final_code/
- figures/
  - exploratory_figures/
  - explanatory_figures/
- products/
  - writing/

Upload the data file into the tidy_data folder and this .Rmd file into the main project directory.

Once the .Rmd document is in the correct folder, you'll want to **change the author of this document** to your name at the top of the .Rmd document (in the YAML). Save this change before moving to the next step.

**Note**: If you're stuck on this, these steps were covered in detail in an earlier course: [Organizing Data Science Projects](https://leanpub.com/universities/courses/jhu/cbds-organizing). Refer to the materials in this course if you're stuck on this part of the project.

### Pushing to GitHub

You'll want to save changes to your project regularly by pushing them to GitHub. Now that you've got your file structure set up and have added a code file (.Rmd), it's a good time to stage, commit, and push these changes to GitHub. Do so now, and then take a long on GitHub to see the changes on their website!

**Note**: If you're stuck on this, these steps were covered in detail in an earlier course: [Version Control](https://leanpub.com/universities/courses/jhu/version-control). Refer to the materials in this course if you're stuck on this part of the project.


### The data

This is a set of just over 20,000 games collected from a selection of users on the site Lichess.org, and how to collect more. The data is downloaded from the website Kaggle.com and is public domain. The author of the data is [Mitchell J.](https://www.kaggle.com/datasnaek) This variables in the data are:

* Game ID
* Rated (T/F)
* Start Time
* End Time
* Number of Turns
* Game Status
* Winner
* Time Increment
* White Player ID
* White Player Rating
* Black Player ID
* Black Player Rating
* All Moves in Standard Chess Notation
* Opening Eco (Standardised Code for any given opening, list [here](https://www.365chess.com/eco.php))
* Opening Name
* Opening Ply (Number of moves in the opening phase)

Let's get to the project. Make sure you have loaded the necessary packages for this project. `tidyverse`, `skimr`, `janitor`, and `gridExtra` are suggested but you can edit any other package that you use there.

Import the data into R using the `readr` package and then use the package `janitor` to clean the column names. Write your code in the code chunk below.

```{r}
chess <- read_csv("data/tidy_data/games.csv") %>%   
 clean_names()
```



How many observations are in the data? Write your code in the chunk below.

```{r}
dim(chess)
```


Although there are so many played games in the data, in a majority of them the platers resign so the game doesn't end. For the purpose of this project, we don't need those observations. Remove them from the data and save the new dataframe to an object called `finished_games`.

```{r}
finished_games <- chess %>% 
  filter(victory_status != "resign")

```

Now, how many observations are left. Write the code below:

```{r}
dim(finished_games)
```

Is there any missing values in your data. Write the code to check this in the code chunk below:

```{r}
any(is.na(finished_games))
```

Use ggplot to show a histogram of the number of turns in the finished games.

```{r}
finished_games %>%
ggplot(finished_games, aes(x=turns)+
geom_histogram()
    
```

What is the avergae and median of the number of turns? Write your code below. You can use the package `skimr`.

```{r}
skim(finished_games)
```


The first-move advantage in chess is the inherent advantage of the player (White) who makes the first move in chess. Chess players and theorists generally agree that White begins the game with some advantage. Since 1851, compiled statistics support this view; White consistently wins slightly more often than Black, usually scoring between 52 and 56 percent. White's winning percentage is about the same for tournament games between humans and games between computers.

Does your data support this hypothesis? Show this in a pie chart. In your pie chart, change the color of the `white` group to white, the color of the `black` group to black, and the color of the `draw` group to grey.

```{r}
pie(x=100,labels = finished_games$winner,names(c("white,black,draw")), edges = 300, radius = 0.8,clockwise = TRUE, init.angle = if(clockwise) 90 else 0,
      density = NULL, angle = 45, col = "black", border = "brown",
  lty = NULL, main = NULL,)
```


A chess opening or simply an opening refers to the initial moves of a chess game. The term can refer to the initial moves by either side, White or Black, but an opening by Black may also be known as a defense. There are dozens of different openings, and hundreds of variants. The Oxford Companion to Chess lists 1,327 named openings and variants. Chess openings have changed over time as well.

In your data, what are the most popular opening moves? Show a bar plot using ggplot that on the vertical axis you have the top 10 most popular openings and on the horizontal axis you have the frequency of each opening in your data. Sort the opening names based on the frequency in the data from high to low.



    ```{r}

  finished_games %>% group_by(opening_name) %>% 
  summarise(n=n())%>% 
  arrange(desc(n)) %>% 
  ggplot()+
  geom_bar(aes(x=(finished_games$opening_name),y=
                 top_n(opening_name))
    
```

```{r}
 finished_games %>% top_n(opening_ply)
    
```


In another bar plot, show what percentage of the games started with each of the top 10 opening strategies are won by white and what percentage are won by white. Which opening strategies would be the best for white to win? In your `geom_bar()` function, use the argument `position = "fill"` for having bars with the same height.



```{r}
ggplot(finished_games =finished_games$opening_name) + 
  geom_bar(mapping = aes(x =finished_games$opening_ply, stat = "identity", position = "fill"))

```


Depending on the opening games, games can have different lengths (in terms of the number of turns). Show another bar plot that shows the the average number of turns for different opening strategies. Games based on which opening strategy on average take the longest?

```{r}
ggplot(finished_games =finished_games) + 
  geom_bar(mapping = aes(x =finished_games$turns, stat = "identity", position = "fill"))
```



## Reshaping data

In the data, users are rated based on their previous games. The higher the rating the better the player is. In the data you currently have, each row is a game. Create a new data frame in which each row is a player and there are five columns: the color of the plater, their rating, number of turns, the opening ply, and whether they lost of won (won = 1 and lost = 0). Call this new data frame `players`. Note that you should first remove the games that end as draw.



```{r}
players <- finished_games[,c("winner",
                    "black_rating",
                    "white_rating",
                    "turns",
                    "opening_ply")]

```


How many rows are in the new data frame? Why is it more than the previous data frame?

```{r}
dim(players)
```

Using the data you just created, show two denisty functions (using geom_density) of ratings on the same graph: one for the winners and one for the losers ignoring the games that end as draw. Are the winners' ratings higher than the losers?

```{r}
players %>% gplot(,aes(x=winner)+
   geom_density(stat=density))
    
```


Filter your players data to only include those who won. Do you find a correlation between the plater's rating and the number of turns it takes them to win? Find the correlation coefficient and show this in a scatter plot. Do you see a high correlation?

```{r}
plot1<-ggplot(data=players,aes(x=turns,
                            y=white_rating))+  
  geom_point()+geom_smooth(method = "lm")

plot2<-ggplot(data=players,aes(x=turns,
                        y=black_rating))+  
  geom_point()+geom_smooth(method = "lm")

grid.arrange(plot1,plot2,ncol=2)

```


Read the lesson on inferential analysis. We are using linear regression to find what factors determine the probability of winning a game. Use the variable indicating whether the player wins or not as the dependent variables (Y) and rating, color of the player's piece, and the opening ply. Make sure to use the color as factor.

```{r}
fin<- lm(winner ~  rating , data = players)
```

Answer the following questions based on your regression result.

* Does the player's rating matter in their winning chances?NO
* Does whether the player starts a game or not affect their chances of winning the game?NO
* Are the opening ply affect the chances of winning a game?YES

## Final Steps

Congratulations! You have completed the project. There are a few final notes:

### Add Markdown Text to .Rmd

Before finalizing your project you'll want be sure there are **comments in your code chunks** and **text outside of your code chunks** to explain what you're doing in each code chunk. These explanations are incredibly helpful for someone who doesn't code or someone unfamiliar to your project.

**Note**: If you're stuck on this, these steps were covered in detail in an earlier course: [Introduction to R](https://leanpub.com/universities/courses/jhu/introduction-to-r). Refer to the R Markdown lesson in this course if you're stuck on this part (or the next part) of the project.


### Knit your R Markdown Document

Last but not least, you'll want to **Knit your .Rmd document into an HTML document**. If you get an error, take a look at what the error says and edit your .Rmd document. Then, try to Knit again! Troubleshooting these error messages will teach you a lot about coding in R.

### A Few Final Checks

A complete project should have:

- Completed code chunks throughout the .Rmd document (your RMarkdown document should Knit without any error)
- README.md text file explaining your project
- Comments in your code chunks
- Answered all questions throughout this exercise.

### Final `push` to GitHub

Now that you've finalized your project, you'll do one final **push to GitHub**. `add`, `commit`, and `push` your work to GitHub. Navigate to your GitHub repository, and answer the final question below! 

**Note**: If you're stuck on this, these steps were covered in detail in an earlier course: [Version Control](https://leanpub.com/universities/courses/jhu/version-control). Refer to the materials in this course if you're stuck on this part of the project.

At the end, submit the link to your github repository to us.

Submit the URL to your `final_project` GitHub repository below.

https://github.com/2400dayday/Finale_project.git