# Airbnb-Activity-Investigation
Predicting the price for an Airbnb Host in Berlin sing XGboost Linear Regression
This is a solution to the [problem statement](https://www.kaggle.com/brittabettendorf/berlin-airbnb-data) provide by CodeAsylums for the Hackfest'19 IIT (ISM) Dhanbad.

# Context
Airbnb has successfully disrupted the traditional hospitality industry as more and more travelers decide to use Airbnb as their primary accommodation provider. Since its beginning in 2008, Airbnb has seen an enormous growth, with the number of rentals listed on its website growing exponentially each year. In Germany, no city is more popular than Berlin. That implies that Berlin is one of the hottest markets for Airbnb in Europe, with over 22,552 listings as of November 2018.
1. Can you predict the price for each Berlin neighborhood using listing descriptions?
2. What are the busiest times of the year to visit Berlin? By how much do prices spike?
3. Can you uncover trends in reviews of Airbnb visitors to Berlin?

# Apporach for solution

Initially, all the required libraries were imported and then the data set given was loaded. After exploring the dataset crucial fields that will help us in predicting prices were selected and all the other columns were dropped.
As we can see from the data that data was with a lot of junk, so cleaning the data was a must. All the Nan values in "cleaning_fees" were filled up with $0.00 and same for the "security_deposit". After that "$" symbols were removed from all the fields that contained it. Exploring the price fields it contained some values which were very high as 9000$ and average values range around 67$ so we decided to drop the values which were more than 400$ or whose price were 0$. Square feet and Space fields contained a lot of null values so we dropped that field and got the corresponding values of space from Description. Following things were extracted from the description.
1. all double-digit or three-digit numbers
2. that is followed by one of the two characters "s" or "m" (covering "sqm", "square meters", "m2" etc.) and
3. may or may not be connected by white space.

We have dropped the entries which contain a NULL value for bathrooms, bedrooms. With the help of Geopy library, we have calculated the distance of the hotel from the centre of Berlin with the help of longitude and latitude provided.  After getting the distance we have dropped the longitude and latitude fields.  We have extracted the size from the description, although half of our records still don't have a size. That means we have a problem! Dropping these records isn't an option as we would lose too much valuable information. Simply replacing it with the mean or median makes no sense. That leaves the third option: predict the missing value with a Machine Learning Algorithm. To not make it too complicated, we'll only use numerical features. Next, we have to split our data into</br>

a) a training set where we have sizes and <br/>
b) a test set where we don't.<br/><br/>
After predicting the sizes we again concatenated our data into the old data with the missing value of sizes.  Since the data contained a wide range of size so to make our data come in similar range we have dropped the values of sizes which were greater than 300 or equals to 0. We are interested in what amenities hosts offer their guests, and in order to enrich our prediction, whether we can determine what some of the more special and/or rare amenities might that make a property more desirable.  For that, we kept the top 30 amenities.  we have added columns with amenities that are somewhat unique and not offered by all hosts such as a laptop-friendly workspace, a TV, kid-friendly accommodation , smoker friendly and being greeted by the host.  After that, we have dropped the unnecessary field values and converted the categorial features. After that splitting the data into train test split we have applied scaling.  Now to apply XGboost regression we have applied grid search for different parameters. We have obtained following on grid search. 
{'colsample_bytree': 0.6, 'gamma': 0.22, 'learning_rate': 0.05, 'max_depth': 9, 'n_estimators': 250}

After applying XGboost Regression we have finally obtained an accuracy of 78% and RMSE of 22%.

# Dataset

Data set can be downloaded from [here](https://www.kaggle.com/brittabettendorf/berlin-airbnb-data)
