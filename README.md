# IOS App Store Project Repository

<!-- toc -->

- [Project Charter & Backlog](#project-charter--backlog)
- [Repo structure](#repo-structure)
- [Documentation](#documentation)
- [Running the application](#running-the-application)
  * [1. Set up environment](#1-set-up-environment)
    + [With `virtualenv` and `pip`](#with-virtualenv-and-pip)
    + [With `conda`](#with-conda)
  * [2. Configure Flask app](#2-configure-flask-app)
  * [3. Initialize the database](#3-initialize-the-database)
  * [4. Run the application](#4-run-the-application)
- [Testing](#testing)

<!-- tocstop -->

## Project Charter & Backlog

### Project Charter

 - **Vision**: The ever-changing mobile landscape is a challenging space to navigate. While the number of mobile apps has been increasing over the past few years, the competition has became more and more intense. Therefore, it is crucial for software developers to improve their competitive advantage and understand the existing strategy to drive growth. This project will allow the users to predict the their apps' potential ratings and popularity based on its current features and therefore help developers to improve their apps.
 - **Mission**: Enable users to input the features of their IOS apps and predict number of reviews and rating that specific app will receive and thus its popularity based on its features.
 - **Success criteria**: 
   - Model performance: R square. The final model is expected to have a R squared 20% higher than the initial simple linear regression model.
   - Business sense: Ask the user to rate the app (thumbs up or thumbs down) and calculate the ratio. Potentially increase users' revenue. 

### Project Planning

 - **Develop Themes:** Help target users (software developers) to predict number of of reviews and ratings of their apps and therefore gauge potential popularity of their apps. Users can improve and make adjustment to their current version based on the results and potentially increase revenue.
 - **Epics**
	-  **Epic 1:** Exploratory Data Analysis to explore the relationship between variables and determine which subset of variable to use. (user input related)
		-	**Story 1:** Merge two data sets
		-	**Story 2:** Data cleaning, check NA values and duplicates. 
		-	**Story 3:** Distribution of each variables, check multicollinearity and outliners 
	- **Epic 2:** Feature Engineering
		-	**Story 1:** Variable transformation 
		-	**Story 2:** Split the data set to training and test set 
	- **Epic 3:** Model Building to predict the number of reviews and ratings using a set of attributes.
		-	**Story 1:** Run a simple linear regression model to use its R square as the naive baseline. Check variable importance and residual plot to determine what advanced model to use. 
		-	**Story 2:** Run Neural Network, Random Forest Model to predict the number of reviews and ratings. Making sure the model hit the performance metrics. 
		-	**Story 3:** Text analysis or sentiment analysis of app descriptions to see the correlation between certain words and popularity of the apps.
	-  **Epic 4:** Build Pipeline
		 -	**Story 1:** Set up the environment 
		 -	**Story 2:** Move algorithm from local to AWS 
		 -	**Story 3:** Run and save the trained model 
		 -	**Story 4:** Set up the data pipeline 
	- **Epic 5:** Web App Development to build interactive features that allow users to enter their data to get predictions.
		-	**Story 1:** Writing the backend using Flask app. 
		-	**Story 2:** Web page design, basic layout 
		-	**Story 3:** Create interactive page allow the user to enter the attributes of their apps, attributes should be corresponds to the parameters used in the models. (8 points)
		-	**Story 4:** For certain attributes, enable users to click on instead of simple text entry for diverse user experience. 
		-	**Story 5:** Predict and show the number of reviews and ratings for the specific file on the same page in 20 seconds. 
		-	**Story 6:** Aesthetic design, color palette, font, format details.
	 - **Epic 6:** Running the application
		 -	**Story 1:** Test run the app to evaluate the performance.
- **Backlog**
	- Epic 1 story 1 (1 point) --planned in 2 weeks
	- Epic 1 story 2 (1 point) --planned in 2 weeks
	- Epic 1 story 3 (1 point) --planned in 2 weeks
	- Epic 2 story 1 (1 point) --planned in 2 weeks
	- Epic 2 story 2 (1 point) --planned in 2 weeks
	- Epic 3 story 1 (2 points) --planned in 2 weeks
	- Epic 3 story 2 (4 points) --planned in 2 weeks
	- Epic 4 story 1 (1 point)
	- Epic 4 story 3 (2 points)
	- Epic 5 story 2 (1 point)
	- Epic 5 story 3 (8 points)
	- Epic 5 story 4 (2 points)
	- Epic 5 story 5 (2 points)
	- Epic 5 story 5 (4 points)
 - **Icebox**
	 1. Text analysis or sentiment analysis of app descriptions to see the correlation between certain words and popularity of the apps.
	 2. Set up the data pipeline 
	 3. Move algorithm from local to AWS
	 4. Writing the backend using Flask app. 
	 5. Running the application

## Repo structure 

```
├── README.md                         <- You are here
│
├── app
│   ├── static/                       <- CSS, JS files that remain static 
│   ├── templates/                    <- HTML (or other code) that is templated and changes based on a set of inputs
│   ├── models.py                     <- Creates the data model for the database connected to the Flask app 
│   ├── __init__.py                   <- Initializes the Flask app and database connection
│
├── config                            <- Directory for yaml configuration files for model training, scoring, etc
│   ├── logging/                      <- Configuration files for python loggers
│
├── data                              <- Folder that contains data used or generated. Only the external/ and sample/ subdirectories are tracked by git. 
│   ├── archive/                      <- Place to put archive data is no longer usabled. Not synced with git. 
│   ├── external/                     <- External data sources, will be synced with git
│   ├── sample/                       <- Sample data used for code development and testing, will be synced with git
│
├── docs                              <- A default Sphinx project; see sphinx-doc.org for details.
│
├── figures                           <- Generated graphics and figures to be used in reporting.
│
├── models                            <- Trained model objects (TMOs), model predictions, and/or model summaries
│   ├── archive                       <- No longer current models. This directory is included in the .gitignore and is not tracked by git
│
├── notebooks
│   ├── develop                       <- Current notebooks being used in development.
│   ├── deliver                       <- Notebooks shared with others. 
│   ├── archive                       <- Develop notebooks no longer being used.
│   ├── template.ipynb                <- Template notebook for analysis with useful imports and helper functions. 
│
├── src                               <- Source data for the project 
│   ├── archive/                      <- No longer current scripts.
│   ├── helpers/                      <- Helper scripts used in main src files 
│   ├── sql/                          <- SQL source code
│   ├── add_songs.py                  <- Script for creating a (temporary) MySQL database and adding songs to it 
│   ├── ingest_data.py                <- Script for ingesting data from different sources 
│   ├── generate_features.py          <- Script for cleaning and transforming data and generating features used for use in training and scoring.
│   ├── train_model.py                <- Script for training machine learning model(s)
│   ├── score_model.py                <- Script for scoring new predictions using a trained model.
│   ├── postprocess.py                <- Script for postprocessing predictions and model results
│   ├── evaluate_model.py             <- Script for evaluating model performance 
│
├── test                              <- Files necessary for running model tests (see documentation below) 

├── run.py                            <- Simplifies the execution of one or more of the src scripts 
├── app.py                            <- Flask wrapper for running the model 
├── config.py                         <- Configuration file for Flask app
├── requirements.txt                  <- Python package dependencies 
```
This project structure was partially influenced by the [Cookiecutter Data Science project](https://drivendata.github.io/cookiecutter-data-science/).

## Documentation
 
* Open up `docs/build/html/index.html` to see Sphinx documentation docs. 
* See `docs/README.md` for keeping docs up to date with additions to the repository.

## Running the application 
### 1. Set up environment 

The `requirements.txt` file contains the packages required to run the model code. An environment can be set up in two ways. See bottom of README for exploratory data analysis environment setup. 

#### With `virtualenv`

```bash
pip install virtualenv

virtualenv pennylane

source pennylane/bin/activate

pip install -r requirements.txt

```
#### With `conda`

```bash
conda create -n pennylane python=3.7
conda activate pennylane
pip install -r requirements.txt

```

### 2. Configure Flask app 

`config.py` holds the configurations for the Flask app. It includes the following configurations:

```python
DEBUG = True  # Keep True for debugging, change to False when moving to production 
LOGGING_CONFIG = "config/logging/local.conf"  # Path to file that configures Python logger
PORT = 3002  # What port to expose app on 
SQLALCHEMY_DATABASE_URI = 'sqlite:////tmp/tracks.db'  # URI for database that contains tracks

```


### 3. Initialize the database 

To create the database in the location configured in `config.py` with one initial song, run: 

`python run.py create --artist=<ARTIST> --title=<TITLE> --album=<ALBUM>`

To add additional songs:

`python run.py ingest --artist=<ARTIST> --title=<TITLE> --album=<ALBUM>`


### 4. Run the application 
 
 ```bash
 python app.py 
 ```

### 5. Interact with the application 

Go to [http://127.0.0.1:3000/]( http://127.0.0.1:3000/) to interact with the current version of hte app. 

## Testing 

Run `pytest` from the command line in the main project repository. 


Tests exist in `test/test_helpers.py`
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzgxMjg2MDQ2LC0xNTQ5MzY2MjkyLC0xOT
QzMTg4NDU1LC0xOTM2NDg0MDgwLDEwMTA0NDIzODcsNjA1Mzk2
NjI3LDE3MTY4ODI5NjMsMTQwOTM4MjAzNiwxNTY1MTgxNjU1LC
0xMDI2MzAzNDczLDE1Njg5MTI2ODQsMTEwNzI3OTkxMywtNDAw
NjEzMzU2LDEwNTA0MjE2MzgsMjMwNzc0OTk1LC0xODUzMzg3ND
c4LC00MDIxMTY2NSwtMjA2MzYzMDYxM119
-->