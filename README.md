[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/VuODydzp)

The dataset contains detailed player data from FIFA. Here is more information on the dataset: https://www.kaggle.com/datasets/stefanoleone992/fifa-22-complete-player-dataset/data

Task 1: 
Code Break down: 
The first block sets up a local PySpark session for the "FifaProject" and configures it to connect to PostgreSQL using the JDBC driver. It specifies memory allocations for the driver and executor, and loads the PostgreSQL JDBC driver from a local path. A success message is printed once the Spark session is successfully started.

Available in the files is a SQL script thatcreates a players_data table in the fifa schema to store detailed player information, including attributes like player ID, name, position, overall rating, club details, and skill statistics. It also ensures that the table has the necessary columns to capture various player features and relationships, and assigns the table ownership to the fifaproject user. I used pgAdmin 4 and postgres in order to run this properly.

The second block reads CSV files containing male and female player data from 2015 to 2022 and processes them using PySpark. It handles missing values by replacing empty strings with None and casts certain columns to appropriate data types such as DoubleType, IntegerType, and BooleanType. The processed data is then written to a PostgreSQL database, with a success message printed for each year and gender. If any errors occur during data processing, an exception is caught, and an error message is printed. Additionally, retrieves and displays a sample of the stored data from the PostgreSQL database in order to confrim that the data table is as intended. 

  Benifits of PostgresSQL DataBase:
  The FIFA dataset contains structured information about players, such as their names, positions, ratings, and financial data, making it ideal for a relational database like PostgreSQL. Using PostgreSQL enables efficient querying, data integrity, and ACID compliance, essential for consistent data updates and analyses. While NoSQL databases are better for handling massive, semi-structured data across distributed environments, PostgreSQL's relational structure is advantageous for this dataset’s clearly defined schema and high consistency requirements. Additionally, PostgreSQL is optimized for handling relationships between structured data, which would require more complex and less efficient solutions in a NoSQL environment. However, NoSQL solutions offer scalability and flexibility, benefiting projects of a larger scale. PostgreSQL Databases prioritize performance over strict data consistency.

Task 2:
Code Break Down

This code contains three functions for conducting analytics on the FIFA dataset, which is stored in a PostgreSQL database. The first function, top_clubs_by_contracts, retrieves the top Y clubs with the highest number of male players whose contracts are valid until year Z or beyond for a given year X. It uses PySpark to filter and group the data based on contract validity, and then orders the clubs by player count. The second function, clubs_by_avg_age, finds the top X clubs with the highest or lowest average player age for a specified year Y, ensuring that ties at the Xth rank are handled appropriately. It filters the data for male players and calculates the average age per club, sorting the results in ascending or descending order based on the user's preference using the argument in the function which can either be "highest" or "lowest". The third function, most_popular_nationality, identifies the most popular nationality for each year by grouping the data by nationality and year, counting occurrences, and selecting the nationality with the highest count for each year. It joins the results with the maximum count for each year to ensure the correct nationality is selected. All functions utilize Spark to perform data processing on large datasets and interact with the PostgreSQL database to retrieve the required information. The results are displayed using the show() method to present the analysis to the user.
  
Task 3:
Code Break Down: 
Data Cleaning and Engineering
The code begins by reading the FIFA player data from a PostgreSQL database using PySpark, with the connection details specified through the JDBC options. It filters the dataset to only include male players (sex == 'Male') and drops the dob (date of birth) column due to inconsitancies in the DataType. The total number of male player rows is counted in order to ensure that all the data is being processed, and columns with dots in their names are renamed to replace the dots with underscores to avoid errors based on formating. A threshold is set to drop columns where more than 10% of the data is null, and the names of dropped columns are stored. Afterward, rows with any remaining null values are dropped using na.drop(). The function extract_base_value() is defined to extract numerical values from position-related columns (e.g., ls, st, rs, etc.) using a regular expression, which is applied to these columns. The remaining columns are categorized into numerical, categorical, binary, and nominal columns based on data types and the number of unique values. Binary columns are transformed to 0 or 1 using when(). For nominal columns, StringIndexer is used to assign integer indices, followed by OneHotEncoder to create one-hot encoded columns, all of which are processed through a Pipeline. The pipeline is fitted and applied to transform the dataset. The code then performs a correlation analysis on the numerical columns using Pandas to compute the correlation matrix, and features with correlation above a threshold of 0.95 are dropped. Additional irrelevant columns are removed, and the remaining features are assembled into a feature vector using VectorAssembler. Finally, the features are standardized using StandardScaler in an effort to reduce computation time for certain regression models. 

After the data engineering is complete we are now ready to apply the ML models to the data 
The first two models are regression models using Spark, a Random Forest Regressor (RF) and Linear Regression (LR) to predict the overall player value based on their skillsets. The dataset is split into training (80%) and test (20%) subsets, and both models are initialized, trained using the training data, and evaluated using Root Mean Squared Error (RMSE) for performance, with lower RMSE indicating better model fit. A custom accuracy metric is also used, considering predictions within 5% of the actual values as correct. Random Forest was chosen for its ability to capture complex, non-linear relationships between features and target variables, making it suitable for datasets like this one, where interactions between features are likely intricate. Linear Regression, on the other hand, serves as a simpler baseline model that assumes linear relationships, offering interpretability and computational efficiency. The code compares the two models to evaluate both their predictive power and computational efficiency, with Random Forest expected to perform better on complex tasks due to its ability to model non-linearity, while Linear Regression is used for its simplicity and as a point of comparison. I chose Root Mean Squared Error (RMSE) because it provides a clear measure of how far off the model's predictions are from the actual values, with lower values indicating better accuracy. RMSE is particularly useful for regression tasks as it penalizes larger errors more heavily, helping to identify models that make significant prediction mistakes.

The next code block performs hyperparameter tuning for two regression models—Random Forest Regressor (RF) and Linear Regression (LR) using cross-validation to optimize their performance in predicting the overall value of players. The hyperparameters for RF, such as maxDepth and numTrees, are tuned across several values, controlling the complexity of the model and helping to prevent overfitting. For LR, the regParam and elasticNetParam are tuned to manage regularization, with regParam preventing overfitting by penalizing large coefficients and elasticNetParam adjusting the mix between L1 and L2 regularization. Cross-validation with 3 folds is used to evaluate each hyperparameter combination. The RMSE (Root Mean Squared Error) is calculated to assess the model's performance on different data splits, ensuring generalizability. After tuning, the models are tested on unseen data, and their RMSE is evaluated to check for overfitting. The code also calculates accuracy, defined as the percentage of predictions within 5% of the actual values, offering a more intuitive metric for model performance. The results of hyperparameter tuning are visualized by plotting the RMSE values for each combination, allowing for a comparison between the two models. This approach ensures that both Random Forest and Linear Regression models are optimally tuned and evaluated for accurate player value predictions.

We ran the two regression models, and the results show that the Random Forest model outperforms Linear Regression in terms of both RMSE and accuracy, achieving a train RMSE of 1.61 and a test RMSE of 1.60, with an accuracy of approximately 94% for both train and test sets. In comparison, Linear Regression yields higher RMSE values (train RMSE of 1.84 and test RMSE of 1.82), with an accuracy of around 92%. These results demonstrate that the Random Forest model is better suited for predicting player overall values, likely due to its ability to capture complex, non-linear relationships. (Note these values may differ slighlty from the video as they were taken earlier)

The next ML model implements a machine learning pipeline in PyTorch, where the dataset is first converted into PyTorch tensors for features (scaled skillset data) and labels (overall player ratings) after being loaded into a Pandas DataFrame. The data is then split into training (80%) and testing (20%) sets, and an MLP (Multi-Layer Perceptron) model is defined with a hidden layer using ReLU activation and an output layer to predict player value. The model is trained using Mean Squared Error (MSE) loss and the Adam optimizer over 20 epochs with mini-batches of size 64. After training, the model is evaluated on the test data by calculating Root Mean Squared Error (RMSE) and accuracy, the latter defined as the percentage of predictions within 5% of the actual values. This provides both a performance measure (how close the predictions are to the true values) and a practical evaluation (how often predictions fall within a 5% margin). The model's ability to capture non-linear relationships between features and target values is assessed through these metrics, offering insight into its predictive performance for player ratings. Following this, a hyperparameter optimization process is implemented to fine-tune the MLP model. The dataset is split into training and testing sets, and the model is optimized by experimenting with combinations of learning rates, batch sizes, and hidden layer dimensions. The optimization uses a grid search through all hyperparameter combinations, tracks the training loss for each, and identifies the configuration with the lowest loss as the best model. After training, the model's performance on the test set is again evaluated using RMSE and accuracy. A plot is generated to visualize the training loss across different configurations, showing the impact of hyperparameters on the training process. The final model, with the best hyperparameters, is selected for evaluation, ensuring the model is fine-tuned for optimal performance and generalizes well to the test set. The MLP model achieves a Test RMSE of 1.15 and a Test Accuracy of 98.02%, indicating that while the model performs well, there is still some room for improvement compared to the CNN model. The training loss decreases steadily over the epochs, suggesting that the model is learning effectively, but it does not achieve the same level of precision or accuracy as the CNN model on the test set. (Note: Plots will generate if you run the code)(Note these values may differ slighlty from the video as they were taken earlier)

Finally I implemented a Convolutional Neural Network (CNN) in PyTorch. The dataset is first loaded into a Pandas DataFrame, then converted into PyTorch tensors for both features (scaled skillset data) and labels (overall player ratings). The data is split into training (80%) and testing (20%) sets like all the other pervious models, and a CNN model is defined with two convolutional layers, followed by fully connected layers to output the predicted player value. The model is trained using Mean Squared Error (MSE) loss and the Adam optimizer over 20 epochs with different combinations of batch sizes and convolutional layer outputs. A grid search is conducted across the hyperparameters—batch size, number of output channels for the convolutional layers—evaluating the model's performance based on the training loss for each configuration. The configuration with the lowest loss is selected as the best model. Training loss for each configuration is visualized through a plot, showing the impact of different hyperparameters on model performance. After training, the model’s performance is evaluated on both the training and test sets using RMSE and accuracy, with accuracy defined as the percentage of predictions that fall within 10% of the actual values. The final model, with the best hyperparameters, is evaluated on the test set to assess its generalization ability. The CNN model demonstrates impressive performance, achieving a Test RMSE of 1.27 and a Test Accuracy of 99.97%, indicating that the model's predictions are extremely close to the actual player values. The model also performs well on the training set, with a Train RMSE of 0.99 and a Train Accuracy of 99.97%, suggesting that it has successfully learned the underlying patterns in the data and generalizes well to new data. (Note: Plots will generate if you run the code)(Note these values may differ slighlty from the video as they were taken earlier)
