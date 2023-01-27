# Phase 1 Project 

The goal of this project is to analyze data providing information about movies to generate insights for Microsoft as to what types of films they should produce for their new film studio.


### The Data

The datasets provided for research are from IMDB, Box Office Mojo, Rotten Tomatoes, The Movie DB, and The Numbers. For this project, I examined the The Movie DB dataset to extract financial information for films and examined the IMDB dataset to extract general information for films such as genre, directors, and cast members.

### Importing Budget Dataset and Cleaning Data

To clean the data, I standardized strings containing dollar signs and commas to float values.
I also created a column named gross_per_dollar to measure the ratio between money spent and money earned on a film.
I created a separate column for release year and added this value to the end of the movie name to avoid confusion between identically named films.
Finally, I removed all films with a production budget less than $100,000 or a worldwide gross of exactly zero.


### Extracting IMDB Basic Information from imdb_basics and Merging with Budget Dataset

The "imdb_basics" dataset contains the "movie_id" code used to identify films within the IMDB database, primary and original titles, year of release, runtime, and genres of the film.

### Extracting IMDB Actor and Producer Information from imdb_principals

The "imdb_principals" database contains movie_id, the ordering of the person in the credit, the "person_id" used to identify the individual within the IMDB dataset, their category, job, and the name of any characters they may play.

First I extract all rows where the person's category is "actor" and then extract all rows for which the person's category is "producer". I drop all columns other than movie_id and person_id, which I rename to actor_id and producer_id respectfully.

### Extracting IMDB Director and Writer Information from imdb_directors and imdb_writers

The "imdb_directors" and "imdb_writers" datasets each only include columns for movie_id and person_id.

I rename person_id to director_id and actor_id respectfully and leave the datasets otherwise unmodified.

### Merging Budgets with the above Producer, Writer, Actor, and Director Datasets

Since all datasets share the movie_id column, they can be merged using this as a key. This returns a dataset with multiple rows per film in cases where the film has more than one producer, writer, actor, or director.

### Cleaning Merged Dataset

First I grouped the dataset such that each movie only has one row. This returns columns of lists, containing repeated values. Next I removed these repeated values and extracted column values from lists for instances where the list only had one value.

### Iterating over producers, directors, actors, and writers to create "success scores" represeting how many successful films that person has made

"Successful" defined here as being one of the top-250 grossing movies since 2000.
The code below returns a dictionary for each job category containing successful person_ids and the count of successful films each has worked on.

### Mapping the above "success score" for each individual to their person_ids in dataframe

I created columns summing the success scores by job category and created a final "success_score" column measuring the total number of successful people for each film.

### Plotting Relationships Between Success Score and ROI

The below graphs indicate that moderately successful producers and writers perform best. Very successful directors perform much better than their moderately successful counterparts. Successful actors do not necessarily perform better than their moderately successful counterparts, except for a bump in performance for the most successful actors. There is an overall positive relationship between net success score and ROI.

![download](https://user-images.githubusercontent.com/103140702/215185806-888e1f41-3b20-405f-9751-e811d7101408.png)

### Extracting Statistical Information By Genre

Here, I am extracting: correlation between production budget and worldwide gross, mean production budget, mean worldwide gross, worldwide gross standard deviation, mean success score for that genre, correlation between success score and worldwide gross, mean ROI, or "gross_per_dollar", and ROI standard deviation

I will be using these statistics to determine what genres to examine visually.

### Examining Genres Visually

From the above, I specifically want to examine Horror, Documentary, and Animation. I am also examining some other genres from mostly the upper median range, based on the correlation between budget and gross income, success scores, and ROI.

![download](https://user-images.githubusercontent.com/103140702/215186608-9b905d86-5edb-4424-8820-98d933b33986.png)

From the above graphs, I've decided that Microsoft should focus on animated films. Animated films are most likely, along with Comedy, to perform disproportionately well in the moderate budget range. Unlike Comedy, the starting y-value of the linear regression line is high, which means that they are less likely to flop. Below, I examine Horror and Documentary in greater detail to explore producing these films as a secondary focus.

Horror has many heavy outliers. What happens if we remove the top ten highest grossing horror films from the dataset? Mean ROI decreases from 8.16 dollars earned / dollars spent to 5.24 dollars earned / dollars spent, which is much closer to Documentary with its ROI of 4.41. Documentary has few data points, but this could be advantageous as it represents an undersaturated market.

### Microsoft should produce animation films and spend a small amount of money producing low budget documentaries, to try to capture the understaturated market.  Who should they hire to make these films?

First, I examine Animation. Hiring successful writers and actors does not have much impact on ROI.
There is a large correlation between ROI and hiring successful producers and directors. Microsoft should hire established producers and directors to create their animation films.

![download](https://user-images.githubusercontent.com/103140702/215186971-08b5f672-bc61-4ac0-9ab1-7773aad6da85.png)

Next, I examine Documentary films. This gives inconclusive results - since there are few datapoints, there appears to be no advantage from hiring within any specific category. From the above statistics chart, however, we can see that the correlation betwen success score and worldwide gross is fairly low.

### Final Recommendation

Microsoft should focus primarily on making low to moderate budget animation films, since these are less likely to flop and are capable of performing much better in relation to their cost than most other genres. For these low to moderate budget animation films, they should hire very successful producers and directors, and can hire any actors or writers they want. They should also devote a small amount of resources to creating low budget documentaries, since this is a underexploited market that frequently yields a high return on investment. For these documentaries, they do not need to hire established people.
