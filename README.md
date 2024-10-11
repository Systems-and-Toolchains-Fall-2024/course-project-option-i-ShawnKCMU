[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/VuODydzp)

The dataset contains detailed player data from FIFA. Here is a description of some of the key columns:

    sofifa_id: Unique identifier for each player in the database.
    player_url: Link to the player’s profile on the SoFIFA website.
    short_name: Player’s commonly used short name.
    long_name: Full name of the player.
    player_positions: The positions on the field the player can play.
    overall: Overall rating of the player.
    potential: Potential rating the player could achieve.
    value_eur: Player’s market value in Euros.
    wage_eur: Player’s wage in Euros.
    age: Player’s age.
    positions: Ratings for specific positions (e.g., cb, rb, gk for center-back, right-back, and goalkeeper positions).
    player_face_url: URL for the player’s profile image.
    club_logo_url: URL of the player’s club logo.
    club_flag_url: URL of the flag representing the player's club country.
    nation_logo_url: URL of the logo for the player's national team.
    nation_flag_url: URL of the flag for the player's nationality.

  Here is more information on the dataset: https://www.kaggle.com/datasets/stefanoleone992/fifa-22-complete-player-dataset/data

  Benifits of PostgresSQL DataBase:
  The FIFA dataset contains structured information about players, such as their names, positions, ratings, and financial data, making it ideal for a relational database like PostgreSQL. Using PostgreSQL enables efficient querying, data integrity, and ACID compliance, essential for consistent data updates and analyses. While NoSQL databases are better for handling massive, semi-structured data across distributed environments, PostgreSQL's relational structure is advantageous for this dataset’s clearly defined schema and high consistency requirements. Additionally, PostgreSQL is optimized for handling relationships between structured data, which would require more complex and less efficient solutions in a NoSQL environment. However, NoSQL solutions offer scalability and flexibility, benefiting projects of a larger scale. PostgreSQL Databases prioritize performance over strict data consistency.

Sample Output for Task II: 
Bullet Point 1
+-------------------+------------+
|          club_name|player_count|
+-------------------+------------+
|   Deportes Iquique|          12|
|Patriotas Boyacá FC|          12|
|          Al Ain FC|          11|
|  Alianza Petrolera|          11|
|     Atlético Huila|          11|
+-------------------+------------+

Bullet Point 2
+--------------------+-------+
|           club_name|avg_age|
+--------------------+-------+
|           Fortaleza|   32.6|
|            Cruzeiro|   31.6|
|Club Athletico Pa...|   31.4|
|            Botafogo|   31.4|
|Associação Chapec...|   31.4|
+--------------------+-------+

Bullet Point 3
+----+----------------+-----+
|year|nationality_name|count|
+----+----------------+-----+
|2015|         England| 1627|
|2016|         England| 1519|
|2017|         England| 1627|
|2018|         England| 1633|
|2019|         England| 1625|
|2020|         England| 1670|
|2021|         England| 1685|
|2022|         England| 1719|
+----+----------------+-----+

