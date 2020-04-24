# HappinessProject

Using the information from the World Happiness Reports (2015-2019), our group seeks to identify what factors, if any, can make an impact on happiness.


## Initial Data Gathering:

 The dataset found on Kaggle allowed us to explore this question more in-depth. The six main factors the report explores are as follows: economic production, social support, life expectancy, freedom, absence of corruption, and generosity. The happiness scores and rankings use data from the Gallup World Poll based on answers to the main life evaluation question asked in the poll. This method, known as the Cantril ladder, asks respondents to think of a ladder with the best possible life for them being a ten and the worst possible life being a zero and to rate their own current lives on that scale. The scores are nationally representative.

## Database and App Summary:

D. Luongo located the raw data on Kaggle, which consisted of five (5) csv files corresponding to the results of the World Gallup Poll happiness ranking from 2015-2019. She then downloaded the csv files locally and read them into Python to clean the data and to store it into Pandas dataframes for each year. She imported these dataframes as tables into a local SQL database, using Python. Next, she created an app on Heroku (opposite-of-sadness) and set up a cloud-based Postgres database for it, also on Heroku. After creating the Postgres database, new tables had to be created, matching the tables in the local SQL database. Then, the data were exported from the local database tables and imported into the Heroku Postgres database tables.


Nick and D found additional data sources to include population, continent, and GDP per Capita in USD. This data was added to the original csv files and the tables were re-created through the process described above. In order to create one table with all of the data spanning 2015-2019, D. then created a View in the Postgres Database on the Heroku server. The view was created by using the “union all” code in Postgres. This joined every column in each table to create one complete view of the data for use in visualizations needing longitudinal data.

 
Next, she established the file structure for Heroku to launch the app correctly. This consisted of the following files: initdb.py, Procfile, requirements.txt, run.sh, runtime.txt, and __init__.py. After those were created and filed in the correct structure, she created the app.py file. In this file, the Flask app and database were both set up. The database was read in through the operating system environment within Heroku, pointing to the Heroku Postgres database. Then database models were created for each table, displaying each country’s name and their scores in the eight categories. After the database models were created, API calls were created for each Postgres table, and the results were jsonified so that they could be read by the code created for each visualization.


## Global Happiness - World Map:

Peter and Robel, using the World Happiness csv files and the polygon coordinate data in the geojson, created a map to visualize happiness rankings across the globe. They did extensive research online and tried different ways to combine the World Happiness Report data with countries’ coordinate data. They used python and pandas to combine the coordinate data with the csv files and convert to the geojson format. They used geojson data to produce the world map with a choropleth layer, pop-up markers, and a legend.


Robel started by creating an array of cities and their locations (long/lat) and Happiness Score. He created a blank array to store countryMarkers. CountryMarkers will show the country name and Happiness Score. A loop looped through the countries array, created a new marker, and pushed it to the countryMarkers array. He added all the countryMarkers to one new layer group so layers can be referenced as a group and not individually. He then used the baseMaps variable to set the light layer, then overlayMaps variable to add countryMarker as a layer. He used myMap variable to create a map and center on Finland – the happiest country in world – and set light basemap and countries overlay as default layer. Lastly, he passed the map layers into layer control and added the layer control to the map. The point of countryMarker is to show that Nordic countries are the happiest in the world. Peter took over with ETL our data and creating a geojson file to make the choropleth layer.


## Measuring Happiness - Select Your Country:

HTML Logic allows user to select country from a dropdown of surveyed countries then calls, JS code to pull the rank and factors over the past 5 years. The data is shown in two different plotly charts: the line graph shows the change in rank for each year; the bar chart shows the factor values for each year.
Top Ten Happiest Countries:
The page was designed using HTML, CSS, and a few lines of Java Script written directly in the HTML file and displays the information on the website as an interactive carousel.

The HTML code organizes the information for the top 10 countries using a slideshow container: an image of the country’s flag, the country’s rank, and a small blurb about the country sourced from Wikipedia report on the 2020 World Happiness Report and searches on the top 10 countries. W3 HTML slideshow tutorial was used to design this slideshow. CSS styling was used to format the content container, controls, and indicators for the slide show. The controls are the navigation arrows (next and prev) used to advance the slides. The indicators are dots at the bottom of the slideshow that indicate where the viewer is in the slide deck. The indicator may also be used to select slides.

The JavaScript code is very minimal but provides the “do something” logic that advances or switches the slide based on the viewers use of the control and indicator arrow/dots to navigate the slides.


## A Look at the Factors - Scatter Visualization:
Demetria charted the correlation between the Happiness Score and each of the 6 happiness factors used in the 2019 World Happiness Report.

She read in current year (2019) data file from the postgresql database using flask/sqlalchemy. Next, used the map method to create new arrays for each of the factors, including country and happiness rank.Then, used d3.plotly to create a scatter plot (create 6 plots for each factor) to display the correlation between the happiness score for each country and the factor value for each country. The happiness score determines the position on the horizontal axis. The factor value determines the position on the vertical axis. Plot markers represent the country and are color coded based on the happiness rank for each country. A legend displays the color code for the happiness ranking with the highest rank (darkest color) at the bottom rising to the lowest rank (lightest color) at the top.

The x and y-axis ranges were standardized across the plots to provide a “at a glance” view of how each factor related to the happiness score on the same scale. The default for the ranges was the low and high values of the happiness score and factor value for each factor plot, which “at a glance” skewed the comparative view of the relationships of happiness across plots

Note: javascript plotly does not display the legend the same as the python version of plotly. It does not appear to allow the capability to name legends that are auto-generated from a data series (at least not for the novice). Also, the values displayed in the legend do not correspond to the ranking value, but rather some other generated value based on the rank value… Demetria was not sure what the listed number was. The python plotly version appeared to use the column value from the database in the legend.

To visualize the plots on the website, she used HTML bootstrap layout container and card formatting. Each plot is displayed with a bootstrap card (a styled content container - bordered box with formatting options like header, footer, title, content). The bootstrap layout container was used for margin-setting and wrapping.

## A Deep Dive into GDP:

Nick started making this visualization by referencing Plotly.D3 documentation to read our CSV data files by year, then creating an array of countries, their life expectancies, GDP per capita, and Happiness Scores. After creating containers for what will be the bubble sizes and the x and y axes, he called Plotly to assign the read traces into the plotting function. A for loop is used to go through each row, get the right trace, and append the data. He created a single trace here, to which the frames will pass data for the different years. Then he created a frame for each year, which are effectively just traces, except they do not need to contain the full trace definition (for example, appearance). The frames just need the parts of the traces that change (in this case, the data). Hovertemplate lets us call values from the CSV to be conveniently displayed as you hover over each country’s bubbles.

Next, he created the slider steps, one for each frame. The slider executes a plotly.js API command (Plotly.animate). He animated to one of the named frames created in the loop made earlier. He used updatemenus to create a play button and a pause button. The play button works by passing ‘null’, which indicates that Plotly should animate all frames. The pause button works by passing ‘[null]’, which interrupts any current running animations with a new list of frames. Finally, he added the slider and used ‘pad’ to position it nicely next to the buttons, and then create the plot. From there, the bubble chart plays using our happiness data represented dynamically through the Plotly animation slider, uses color-coding to differentiate between countries, and can be examined closely over the last five years. 


## Machine Learning Decision Tree Prediction Model:

### Input Data World Happiness 2019 Score range - Carl Jung's 5 Factors of Happiness:
* Good Physical Health
* Good persona and intimate relationship (marriage family friends)
* The faculty to perceive beauty in art and nature
* Reasonable stands of living/satisfactory work
* Philosophic or religious point of view capable for coping successfully with the vicissitudes of life

Responses: 
    * Yes No It’s Complicated
    * Value: Yes = 10 No = 1 Complicated = 5
    * Input values are not scientific, but assigned to team

### Jupiter Notebook Decision Tree
* Modified decision tree logic from class exercise to read in input file (tweaked file to ensure not overfitting model
* Defined the X,y values and fit the model which created a decision tree map
* Then serialized the model to a file called model.pkl in order later use with new unknown data to predict user happiness.

### HTML – User Interaction
* Created form for user to enter country from 2019 surveyed countries. 
* Selection triggers JS to get country’s score and return value of 0 or 1 to the form if the rank is <5 and >4 respectively. 
* The user completes the rest of the form and submits.


### FLASK– User Form Submission
The form submission/post triggers flask to run the prediction model using the user input data and returns the prediction to a new results.html page 

### HTML – Results
The results page renders the model prediction. Built a fun animated background for this page. Bird animation is pure CSS and HTML.
1. Inserted bird.svg from web
2. Put the bird image in a container
3. Used css @keyframes and transform to animate the bird motion over a background image   

### ML Happiness Decision Tree Map Analysis:
The decision tree uses input file happiness decisions to calculate the odds for user happiness. Happiness is ranked from 0-4

Happiness rankings:
* 4 = Very Happy
* 3 = Happy
* 2 = Somewhat Happy
* 1 = Not too Happy
* 0 = Not Happy

Aspects of the decision tree: 

Score
• Rank <= 0.5 means that every country with a score of 0.5 or lower will follow the True arrow (to the left), and the rest will follow the False arrow (to the right).
• gini = 0.72 refers to the quality of the split, and is always a number between 0.0 and 0.72, where 0.0 would mean all of the samples got the same result, and 0.72 would mean that the split is done exactly in the middle.
• samples =268 means that there are 268 scenarios left at this point in the decision which is all of them since this is the first step.
• value = [86 96 51, 30, 5] means that of these 268 scenarios:
- 86 will get a "NOT HAPPY"
- 96 will get a "NOT TOO HAPPY"
- 51 will get a “SOMEWHAT HAPPY”
- 30 will get a “HAPPY”
- 5 will get a “VERY HAPPY”

There are multiple steps in this model and each step can be explained similarly as above. View the decisiontree.png to see each decision step

Note of caution: As with other decision trees, if not overfitted, the model will give different results if ran enough times, even if fed the same data. This occurs because the tree model does not give a 100% certain answer. Answers are based on the probability of an outcome.
