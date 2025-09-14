# Streamchik

## Project Description
You work at the online store “Streamchik”, which sells computer games worldwide. From open sources you have historical data on game sales, user and critic reviews, genres, and platforms (for example, Xbox or PlayStation). Your task is to identify patterns that determine a game’s success. This will make it possible to bet on potentially popular products and plan advertising campaigns.

## Data inspection
After importing the dataset, I checked the first few rows using .head() and .describe() to confirm the structure. The dataset has the following columns:

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
- Checked the list of platforms present in the dataset, and removed the ones released before 2010 due to them being too outdated (legacy).
- Found the games in dataset that were missing their release year, and filled it out manually using public domain data.
- Created a new column 'Sales_Total' which is a combination of sales across all regions.
- Dropped games released before 2010 to reduce noise from outdated data and ensure analysis focuses on more recent, comparable information.
- Since critic scores were 1-100 but user scores were 1-10, multiplied User_Score column to be in line with Critic_Score format.
- Confirmed that Critic_Score is in numeric format.
- There is no need to remove the missing data of critic and user scores. Part of the games were released too recently to have scores, and overall modifying the missing scores can alter the end data.

After cleaning, the dataset looked like this:

|    | Name                                      | Platform   |   Year_of_Release | Genre        |   NA_sales |   EU_sales |   JP_sales |   Other_sales |   Critic_Score |   User_Score | Rating   |   Sales_Total |
|---:|:------------------------------------------|:-----------|------------------:|:-------------|-----------:|-----------:|-----------:|--------------:|---------------:|-------------:|:---------|--------------:|
| 31 | Call of Duty: Black Ops 3                 | PS4        |              2015 | Shooter      |       6.03 |       5.86 |       0.36 |          2.38 |            nan |          nan | nan      |         14.63 |
| 33 | Pokemon X/Pokemon Y                       | 3DS        |              2013 | Role-Playing |       5.28 |       4.19 |       4.35 |          0.78 |            nan |          nan | nan      |         14.6  |
| 40 | Mario Kart 7                              | 3DS        |              2011 | Racing       |       5.03 |       4.02 |       2.69 |          0.91 |             85 |           82 | E        |         12.65 |
| 42 | Grand Theft Auto V                        | PS4        |              2014 | Action       |       3.96 |       6.31 |       0.38 |          1.97 |             97 |           83 | M        |         12.62 |
| 47 | Pokemon Omega Ruby/Pokemon Alpha Sapphire | 3DS        |              2014 | Role-Playing |       4.35 |       3.49 |       3.1  |          0.74 |            nan |          nan | nan      |         11.68 |

## Exploratory Data Analysis (EDA)

### 1. Games released per genre per year (2010–2016)

**Steps**
1) Created year_genre_count dataset containing 'Year_of_Release' and 'Genre' columns.
2) Transformed it into a pivot table with index = 'Year_of_Release', columns = 'Genre'.
3) Created a line plot using the pivot table

**Result**
<img width="3413" height="1629" alt="genre_releases_per_year" src="https://github.com/user-attachments/assets/b40857d4-6086-484c-a449-39e56fe2c3ae" />

**Findings**
- Far more action games released, even though last year action games are in a down-trend.
- Small uptrend seen on Shooter and Adventure games last year.
- Least amount of released games are in Puzzle genre.

### 2. Number of sales per platform

**Steps**
1) Created sales_yearly dataset containing 'Year_of_Release', 'Platform' and 'Sales_Total' columns.
2) Transformed it into a pivot table with index = 'Year_of_Release', columns = 'Platform' and values = 'Sales_Total'.
3) Created a line plot using the pivot table

**Result**
<img width="3413" height="1629" alt="sales_per_platform_per_year" src="https://github.com/user-attachments/assets/aed81942-7b5e-4920-906a-83440de268c9" />

**Findings**
- PS4 and Xbox One are in the uptrend, which is expected as they are the newest platforms.
- PS4 is far in the lead doubling the sale numbers of Xbox One.
- Rest of platforms in downtrend.

### 3. Best Platform/Genre combinations (Critic Scores)

**Steps**
1) Created avg_critic_score_by_platform dataset containing 'Platform', 'Genre' and 'Critic_Score' columns.
2) Grouped by 'Platform' and 'Genre' while filling out the combinations with mean of each combination.
3) Created a pivot table.
4) Created a heatmap.

**Result**
<img width="3302" height="1860" alt="average_critic_score_by_genre_and_platform" src="https://github.com/user-attachments/assets/81154cac-dfe1-4c1a-91e1-7b5e84620b64" />

**Findings**
- Highest score is the Puzzle/PS4 and Racing/WiiU combinations.
- Lowest score is the Adventure/WiiU combination.

### 4. Best Platform/Genre combinations (User Scores)

**Steps**
1) Created avg_user_score_by_platform dataset containing 'Platform', 'Genre' and 'User_Score' columns.
2) Grouped by 'Platform' and 'Genre' while filling out the combinations with mean of each combination.
3) Created a pivot table.
4) Created a heatmap.

**Result**
<img width="3302" height="1860" alt="average_user_score_per_genre_and_platform" src="https://github.com/user-attachments/assets/fa892537-e34e-4d2c-abb3-1490543ac62f" />

**Findings**
- Highest score is the Racing/WiiU combination.
- Lowest score is the Strategy/PSV combination.

### 5. Comparing critic and user scores based on platforms

**Steps**
1) Created comp_scores dataset containing 'Platform', 'Critic_Score' and 'User_Score' columns.
2) Converted table into long format using .melt().
3) Created boxplot chart.

**Result**
<img width="1701" height="1351" alt="critic_vs_user_scores" src="https://github.com/user-attachments/assets/d5377af5-308c-4e06-94ef-23d8ab0bfb27" />

**Findings**
- Critic and user scores have similar distributions across platforms, though users tend to be more varied and sometimes harsher (e.g., PC).

### 6. Correlation between critic/user scores and sales by platform.

**Steps**
1) Created a filtered dataset for each separate platform.
2) Removed empty values.
3) Created scatter matrix.

**Result**
<img width="2274" height="2314" alt="correlation_wiiu" src="https://github.com/user-attachments/assets/76475b21-3940-40d1-ac92-5a169bd4944e" />
<img width="2301" height="2323" alt="correlation_psv" src="https://github.com/user-attachments/assets/6582cc0f-4c7d-4212-ba3d-3ea26b6e0f37" />
<img width="2274" height="2314" alt="correlation_ps4" src="https://github.com/user-attachments/assets/e1808aea-f7f0-4309-89c2-6a3726c64c4f" />
<img width="2274" height="2314" alt="correlation_pc" src="https://github.com/user-attachments/assets/17c55c15-17d0-40a9-a5a1-24ad225830c4" />
<img width="2274" height="2314" alt="correlation_3ds" src="https://github.com/user-attachments/assets/5ef3c6d1-d5f7-43d0-98db-1ea7563cb404" />
<img width="2274" height="2314" alt="correlation_xone" src="https://github.com/user-attachments/assets/b4f702f9-a521-446a-9613-7fdfd8402668" />

**Findings**
- Overall, there is a moderate positive correlation between critic and user scores.
- It is stronger on some platforms, for example on 3DS, PSV, and WiiU critics and players tend to agree more.
- Correlation between critic scores and sales is relatively low across platforms.
- Mostly minimal correlation between user scores and sales. The only notable exception is WiiU (≈ 0.39).

### 7. Best combinations of Platform/Genre according to the total sales number overall and per region.

**Steps**
1) Created a filtered dataset for each region and overall.
2) Grouped by 'Platform' and 'Genre'
3) Created a pivot table with index = 'Genre', columns = 'Platform' and values = 'Sales_Total'.

**Result**
<img width="3302" height="1860" alt="total_sales_by_genre_and_platform" src="https://github.com/user-attachments/assets/608a2c73-5ba9-4762-8fe8-914def6d8167" />
<img width="3302" height="1860" alt="na_sales_by_genre_and_platform" src="https://github.com/user-attachments/assets/ab3b412e-a2be-4c1e-8ac4-71c904444ae9" />
<img width="3302" height="1860" alt="eu_sales_by_genre_and_platform" src="https://github.com/user-attachments/assets/cd357f24-2537-402c-a6c2-30b12b0fe7ae" />
<img width="3302" height="1860" alt="jp_sales_by_genre_and_platform" src="https://github.com/user-attachments/assets/15868ecf-0f74-41a2-bc30-239bfe1e49a2" />
<img width="3302" height="1860" alt="oth_sales_by_genre_and_platform" src="https://github.com/user-attachments/assets/603ad746-f7fa-4562-82a5-d679d50259d4" />

**Findings**

Overall:

- Best sales overall are seen in Action/PS4, over 96 million units.
- Second is Shooter/PS4, over 88 million units.
- Lowest number is Puzzle/PS4, with only 20,000 units sold.

North America:

- Highest sales are Shooter/XOne, over 36 million units.
- Action/PS4 and Shooter/PS4 each sold over 32 million units.
- Lowest sales are Simulation/PSV and Fighting/PC, 10,000 units each.

Europe:

- Highest sales are Action/PS4, over 42 million units.
- Shooter/PS4 is second with 39 million units.
- Lowest sales are Simulation/PSV, less than 10,000 sales.

Japan:

- Highest sales are Role-Playing/3DS, over 41 million units.
- Action/3DS is second with 22 million units.
- Rest of combinations showed relatively low sales.

Other regions:

- Highest sales are Action/PS4, over 14 million units.
- Shooter/PS4 is second with 13 million units.
- Rest of combinations showed relatively low sales.

### 8. Key Findings

1) Genre trends:
   - Action genre dominated in term of releases, even though last year shows a decline.
   - Shooter and Adventure games are showing small growth in the final year.
   - Puzzle was the least released genre.
     
2) Platform/sales trends:
   - PS4 led the sales, roughly double the number of Xbox One sales over the same period.
   - Rest of the platforms are in decline.
  
3) Best Platform/Genre Combinations:
   - By Critic Scores: Puzzle/PS4 and Racing/WiiU were the highest-rated. Adventure/WiiU scored the lowest.
   - By User Scores: Racing/WiiU was the highest-rated. Strategy/PSV scored the lowest.
   - Global Sales: Action/PS4 and Shooter/PS4 were best-sellers.
   - North America: Shooter/XOne topped the charts.
   - Europe: Action/PS4 led.
   - Japan: Role-Playing/3DS dominated by a wide margin.
   - Other regions: Action/PS4 again was strongest.
  
4) Critic vs. User Scores:
   - Distributions are broadly similar across platforms, but users show more variation and can be harsher (especially on PC).

5) Correlation Analysis:
   - Critic/User Scores: Moderate positive correlation overall. Stronger on 3DS, PSV, and WiiU.
   - Critic Scores/Sales: Generally low correlation — high reviews do not guarantee high sales.
   - User Scores/Sales: Minimal correlation overall, with WiiU as an exception, where user sentiment had more influence.

## Data Limitations
- There is no information on the source of critics or users scores.
- Sales are given in millions of units, but not explicitly documented in the dataset.
- Data goes as far as to 2016 which is overall outdated.

## Conclusion
- Critics and users mostly agree on game scores, even though players are more divided.
- Sales are weakly connected to scores, suggesting factors like marketing, platform popularity, and brand recognition drive success more than reviews.
- Platform differences matter: Japan favors RPG titles, North America favors shooters, while Europe and global sales are led by Action games.

Data: data/games.csv
