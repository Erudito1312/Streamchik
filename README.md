# Streamchik

## Project Description
You work at the online store “Streamchik”, which sells computer games worldwide. From open sources you have historical data on game sales, user and critic reviews, genres, and platforms (for example, Xbox or PlayStation). Your task is to identify patterns that determine a game’s success. This will make it possible to bet on potentially popular products and plan advertising campaigns.

## Data inspection
After importing the dataset, I checked the first few rows using .head() to confirm the structure. The dataset has the following columns:

- **Name** – games title 
- **Platform** – gaming platform (e.g., PC, PlayStation Vita, Xbox 360) 
- **Year_of_Release** – release date information
- **Genre** – game genre (e.g., Platform, Racing, Role-Playing)
- **NA_sales** - sales in North America (units in million)
- **EU_sales** - sales in Europe (units in million)
- **JP_sales** - sales in Japan (units in million)
- **Other_sales** - sales in other jurisdictions (units in million)
- **Critic_Score** - numerical average review score from game critics
- **User_Score** - numerical average review score from users
- **Rating** - ESRB age rating
 
## Sample of the dataset

|    | Name                     | Platform   |   Year_of_Release | Genre        |   NA_sales |   EU_sales |   JP_sales |   Other_sales |   Critic_Score |   User_Score | Rating   |
|---:|:-------------------------|:-----------|------------------:|:-------------|-----------:|-----------:|-----------:|--------------:|---------------:|-------------:|:---------|
|  0 | Wii Sports               | Wii        |              2006 | Sports       |      41.36 |      28.96 |       3.77 |          8.45 |             76 |          8   | E        |
|  1 | Super Mario Bros.        | NES        |              1985 | Platform     |      29.08 |       3.58 |       6.81 |          0.77 |            nan |        nan   | nan      |
|  2 | Mario Kart Wii           | Wii        |              2008 | Racing       |      15.68 |      12.76 |       3.79 |          3.29 |             82 |          8.3 | E        |
|  3 | Wii Sports Resort        | Wii        |              2009 | Sports       |      15.61 |      10.93 |       3.28 |          2.95 |             80 |          8   | E        |
|  4 | Pokemon Red/Pokemon Blue | GB         |              1996 | Role-Playing |      11.27 |       8.89 |      10.22 |          1    |            nan |        nan   | nan      |

## Data cleaning and modifying
- Since the task does not require details on sales per jurisdiction I decided to combine the sales into a single column 'Sales_Total'.
- Dropped games released before 2010 to reduce noise from outdated platforms and ensure analysis focuses on more recent, comparable data.
- Filled missing values in Critic_Score and User_Score columns with the mean of each respective column.

After cleaning, the dataset looked like this:

|    | Name                           | Platform   |   Year_of_Release | Genre        |   Critic_Score |   User_Score | Rating   |   Sales_Total |
|---:|:-------------------------------|:-----------|------------------:|:-------------|---------------:|-------------:|:---------|--------------:|
| 14 | Kinect Adventures!             | X360       |              2010 | Misc         |             61 |          6.3 | E        |         21.82 |
| 16 | Grand Theft Auto V             | PS3        |              2013 | Action       |             97 |          8.2 | M        |         21.05 |
| 23 | Grand Theft Auto V             | X360       |              2013 | Action       |             97 |          8.1 | M        |         16.27 |
| 27 | Pokemon Black/Pokemon White    | DS         |              2010 | Role-Playing |             70 |          7   | nan      |         15.13 |
| 29 | Call of Duty: Modern Warfare 3 | X360       |              2011 | Shooter      |             88 |          3.4 | M        |         14.73 |

## Exploratory Data Analysis (EDA)

### 1. Games released per genre per year (2010–2016)

**Steps**
1) Created a copy of the dataset.
2) Transformed it into a pivot table with index = 'Year_of_Release', columns = 'Genre'.
3) Created a line plot using the pivot table

**Result**
<img width="3413" height="1629" alt="genre_releases_per_year" src="https://github.com/user-attachments/assets/9c755f0e-4b0e-4127-8546-487944067b3d" />

**Findings**
- Far more action games are being released, even though last year action games are in a down-trend.
- Small uptrend seen on Shooter games last year.
- Least amount of released games are in Puzzle genre.

### 2. Best Platform/Genre combinations (Critic Scores)

**Steps**
1) Created a copy of the dataset.
2) Grouping by 'Platform' and 'Genre' while filling out the combinations with mean of each combination.
3) Creating a pivot table.
4) Due to lack of data on some Platform/Genre combinations NaN cells were filled with 0.

**Result**
<img width="3302" height="1860" alt="average critic score per platform" src="https://github.com/user-attachments/assets/eeba595f-33ef-4df4-a12f-01f33eca6b31" />

**Findings**
- Highest score is the Puzzle/PS4 combination.
- Lowest score is the Simulation/WiiU combination.
- Also critics scored highly games in Sports/PC, Role-Playing/PC, Role-Playing/XOne combinations.

### 3. Best Platform/Genre combinations (User Scores)

**Steps**
1) Created a copy of the dataset.
2) Since critic scores are double-digit, while user scores were single-digit, applied a lambda function to multiply user scores by 10 and make it look more similar to critic score format.
3) Grouping by 'Platform' and 'Genre' while filling out the combinations with mean of each combination.
4) Creating a pivot table.
5) Due to lack of data on some Platform/Genre combinations NaN cells were filled with 0.

**Result**
<img width="3302" height="1860" alt="average user score per platform" src="https://github.com/user-attachments/assets/5d834d72-4ca0-4c5f-a6a0-cf9c5d911084" />

**Findings**
- Highest score is the Puzzle/PS4 combination.
- Lowest score is the Simulation/WiiU combination.
- Also users scored highly games in Shooter/PS2, Racing/WiiU combinations.

### 4. Sales by Platform/Genre combinations

**Steps**
1) Created a copy of the dataset.
2) Dropped columns 'Name', 'Year_of_Release', 'Critic_Score', 'User_Score', 'Rating'.
3) Grouped by 'Platform' and 'Genre' while summarizing the resulted combinations.
4) Due to lack of data on some Platform/Genre combinations NaN cells were filled with 0.

**Result**
<img width="3325" height="1860" alt="Total sales by genre and platform" src="https://github.com/user-attachments/assets/89cfb7b4-3ff1-4fb5-9e80-a19934c2f5ca" />

**Findings**
- Highest sales are in Action/PS3 combination.
- Lowest sales are in Role-Playing/PS2 and Puzzle/PS4 both showing only 0.02.
- Overall high sales also noticed in Shooter/X360, Action/X360 and Shooter/PS3 combinations.

### 5. Most popular platforms overall

**Steps**
1) Created a copy of the dataset with only 'Platform' column.
2) Applied value_counts method.

**Result**
<img width="2538" height="1629" alt="Total games released by platform" src="https://github.com/user-attachments/assets/c83300e5-62ba-4171-bfcd-081c0c567715" />

**Findings**
- PS3 is in the lead, X360 is close second.
- PS2 has the least amount of games released.
- This data however may be not useful due to changes of platform popularity over time.

### 6. Platform popularity over time

**Steps**
1) Created a copy of the dataset.
2) Created a pivot table with index = 'Year_of_Release', columns = 'Platform' and values = 'count'.

**Result**
<img width="3413" height="1629" alt="Popularity of platforms over the years" src="https://github.com/user-attachments/assets/cee45ef2-937a-4fbc-b223-16e33bcfc6e4" />

**Findings**
- Platform gaining the most popularity is PS4 which makes sense with it being the newest one.
- Even though XOne was released practically at the same time as PS4, it is gaining popularity much slower than PS4.
- DS, Wii and PSP were in the downtrend throughout the whole review period.

### 7. Games released by genre in 2016

**Steps**
1) Created a copy of dataset.
2) Applied value_counts method.

**Result**
<img width="1420" height="1229" alt="Total games per genre released in 2016" src="https://github.com/user-attachments/assets/cdcdcbb6-bf93-4561-8306-2ba5a0d724cb" />

**Findings**
- 35% of games released in 2016 were of Action genre.
- Adventure and Role-Playing were 11% each.
- 10% of games were in Sports genre.
- Rest of genres were less than 10% in 2016 releases.

### 8. Key Findings

- Both critics and users have same platform/genre combinations for the highest and lowest scores. 
- Besides the highest/lowest scores however scores are different between critics and users.
- Even though lowest sales are in Puzzle/PS4 combination it does not stop it from being the highest scored combination by both critics and users. This may be due to the fact that according to EDA 1 puzzle has least amount of game releases, which may be the reason sales overall are low even with the high rating.
- Although Action games showed a decline in 2016, they still dominate in number of releases.

## Data Limitations
- There is no information on the source of critics or users scores.
- Sales are given in millions of units, but not explicitly documented in the dataset.
- Data goes as far as to 2016 which is overall outdated.

## Conclusion
The analysis highlights that Action games dominate in terms of volume and sales, while Puzzle games, though less common, achieve the highest review scores. Platform trends show the rise of PS4 and decline of older consoles such as Wii and DS. These insights can help predict future market directions and support decisions in marketing and game development.

Data: outputs/games.csv
