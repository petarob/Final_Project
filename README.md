# Final_Project

# 21 Years to Maturity: Has Foreign Aid Matured Poorer Countries: 21 Years to Maturity: Predicting GDP #

## Introduction ##

### Assumptions to test ###
(1) With billions being spent on foreign aid by combined wealthy countries, I expect an aided country’s GDP to increase
(2) Foreign aid should improve health indicators and employment levels
(3) With increased help, poorer countries would be able to jump start production capacity and subsequently consumption which will drive up CO2 output
(4) Poorer countries have smaller areas of agricultural land to feed their population, which is reason they need aid

### Data Sources ###
All data downloaded as separate CSV files from the World Bank website. Individual datasets were compiled from data primarily from the UN, UNESCO, ILO, FAO, WHO, and World Ban. Once I decided the topic and downloaded the CSVs, I had to combine the all seven data data files ready for ML. See website: https://data.worldbank.org/

## Data Cleaning and Preparation ##

Not all datasets had values for every country by year, so I found those years with mostly filled data. I identify countries (rows) with spotty data and located new countries had been formed over time. Much of the older European countries were stable by 1995. So I decided on the time period from 1995-2015 because there was pretty much a lot of data for most countries – the Modern Aid period.   deleted countries with no data from this time period and those that had two or more indicator datasets completely missing. These included North Korea, Iraq, Liberia, Monaco, Myanmar, Somalia, Afghanistan, Syria, Bermuda, Gibralter, Lichenstein, Montenegro, Nauru and New Calendonia. Given most of these countries have tiny populations so their missing values would not affect the overall model. 

Datasets had aggregated World Bank data. I manually deleted all aggregate data from each of the seven tables before joining them. 

Deleted all columns excluding 1995-2005. 

Each column in each dataset represented a specific year for each country with the whole dataset being for one variable. So I manually transposed each of the seven datasets into a master file, and then and manually stack countries by year. The resulting dataset had 9 columns with name of country, year, GDP, plus the six datapoints on aid, population, neonatal mortality, agriculture, CO2 emissions and employment. There were 180 countries with 21 years of data, so 3,780 rows, and 26,460 data points. 

### GDP Data ###
Eritrea Venezuela had a few missing values so I gave them dummy values,.

### Aid ###
Gaps in data. Addressed by adding zeroes to all countries that did not receive aid. 

### Population ###
Equatorial Guinea has 4 missing values for 1991-1995. I averaged out past 5 and made dummy data

### Neonatal Mortality ###
Some countries had zero data (Puerto Rico, Hong Kong, Greenland) so I used averages for the regions they were located in using World Bank aggregates. 

### Employment ###
Some countries had missing data so rolling year averages were created – Antigua and Barbuda, Dominica, Greenland, Grenada, Kiribati, Marshall Islands, Micronesia, Seychelles, St Kitts and Nevis, and Tuvalu. 

### Agriculture ###
Belgium had first four years of data missing so I used a rolling five year average as dummy data. Luxembourg, Sudan and Serbia also had some missing values that I filled with dummy data based on averages. 

### CO2 emissions ###
No data for any country was given for 2015 data so I used dummy data using averages for previous 5 years for each country. 

### Data Types ###
All formulas used to derive values were overwritten as values. Feedback from pandas was that the numbers were not uniform. In Excel, I changed all number to “Numerals” setting with various decimal points to suit the data. 

## Machine Learning and Results ##

### Residuals ###
I used pandas to import the combined dataset and it was scaled/normalized/split/trained/tested to develop a multiple linear regression model. I computed R2 which measures the strength of the relationship between the model and GDP. It compares the fit of the model with a horizontal straight line (the null hypothesis).

The residuals on the y-axis are the difference between the observed value of GDP and the predicted value of GDP. The x-axis shows the GDP. There are maybe two distinct lines in this graph – one clustered against the x-value, and one clustered around the y-values.  These patterns are not random, but in there are hardly any zeroes which there would be if the model was a good fit. So  I decided to take a look at each of my X factors to see if they could shed light on this weird looking model

### Overall R2 Score ###
The computed R2 scores were 70% for training score, and 75% for the test score. This means that the model explains about 75% of the predicted GDP figure for each year for each country. 

### R2 of the X factors ###
Coefficient weights were computed and in order of scale the results for each X factor are:
•	CO2 = 1.07218098
•	Aid = 0.0172525 
•	Agriculture = 0.01649658 
•	Employment = (minus) -0.00138013 
•	Neonatal Mortality =  (minus) -0.04575203
•	Population = (minus)  -0.39591146 

The coefficients of CO2 was a clear winner being perfectly correlated to GDP. In fact it was an overfit. Aid as it turned out might help explain 17% of GDP, as did Agriculture.  Not huge factors. And I got negative R2 values for employment, neonatal mortality and at minus 40% population. R2 was negative here because the multiple regression model does not follow the trend of the data, so it fits worse than a horizontal line. 

## Step 2: Aided vs Non-Aided Countries ##

Because the model was such a poor fit for all countries, I labelled the original dataset and applied the model to two types of countries – those that got aid and those that didn’t. 

### Residuals ###
I used matplotlib in Pandas to draw a residuals graph of the model as it applied to countries that got aid. This is pretty random looking graph and shows the model to be a poor fit. The R2 number of minus 95% was not kidding. The residuals graph plots multi-X values that shows the difference between the true values of GDP and the predicted values of GDP. There were very few zero values that would have shown a close-fitting model. Both graphs have distinct features from each other. Countries receiving Aid are clustered around negative 0.25 on x-axis, and for countries not receiving Aid, the residuals are clustered around negative 0.5 on the y-axis. 

### R2Result ###
For No-Aid countries R2 was 77% and for countries that did get aid R2 was (minus) -95%. Clearly this model only somewhat for countries that did not receive Aid. None of the six X-factors combined even come close to predicting GDP in countries that receive Aid. 

### Visualizations in Tableau ###
Views created:
•	CO2 plotted against actual and predicted values of GDP in Aided countries.
•	Population plotted against GDP for countries that received aid from those who didn’t received aid. Totally different looking. No straight lines there. 
•	Predicted GDP vs Actual GDP for all countries in their 21st year of aid (2015 only)
•	Aid vs GDP for countries that received Aid. 

# Summary #

Taking into account variables of aid, population, neonatal mortality, agriculture, CO2 emissions and employment by year for180 countries, a machine learning model of multiple linear regression failed to predict accurately GDP for countries receiving aid. The model only served to predict at 75% accuracy rate GDP in countries that do not use foreign aid. 

The only factor that was correlated with GDP was CO2 emissions which was perfectly correlated. 

Foreign Aid is hard. If there were easy to predict futures using these six factors, then poverty would have been solved a long time ago. It may be that the quantum of foreign aid over time might be too small to have much of an effect on GDP. 

## Data Explained and Sources ##

## Foreign Aid ##
Net official development assistance and official aid received  (current US$) 1960-2017 
Description: Net official development assistance (ODA) consists of disbursements of loans made on concessional terms (net of repayments of principal) and grants by official agencies of the members of the Development Assistance Committee (DAC), by multilateral institutions, and by non-DAC countries to promote economic development and welfare in countries and territories in the DAC list of ODA recipients. It includes loans with a grant element of at least 25 percent (calculated at a rate of discount of 10 percent). Net official aid refers to aid flows (net of repayments) from official donors to countries and territories in part II of the DAC list of recipients: more advanced countries of Central and Eastern Europe, the countries of the former Soviet Union, and certain advanced developing countries and territories. Official aid is provided under terms and conditions similar to those for ODA. Part II of the DAC List was abolished in 2005. The collection of data on official aid and other resource flows 
to Part II countries ended with 2004 data. Data are in current U.S. dollars.
Dataset: World Bank DT.ODA.ALLD.CD
Source: Development Assistance Committee of the Organisation for Economic Co-operation and Development, Geographical Distribution of Financial Flows to Developing Countries, Development Co-operation Report, and International Development Statistics database. 
Data are available online at: www.oecd.org/dac/stats/idsonline.

## GDP ## 
GDP at purchaser's prices is the sum of gross value added by all resident producers in the economy plus any product taxes and minus any subsidies not included in the value of the products. It is calculated without making deductions for depreciation of fabricated assets or for depletion and degradation of natural resources. Data are in current U.S. dollars. Dollar figures for GDP are converted from domestic currencies using single year official exchange rates. For a few countries where the official exchange rate does not reflect the rate effectively applied to actual foreign exchange transactions, an alternative conversion factor is used.  Current US$
Dataset: World Bank NY.GDP.MKTP.CD Last updated: 10/16/2019
Data source: World Bank national accounts data, and OECD National Accounts data files.

## Neonatal Mortality ##
Mortality rate, neonatal (per 1,000 live births)
Neonatal mortality rate is the number of neonates dying before reaching 28 days of age, per 1,000 live births in a given year.
Dataset: SH.DYN.NMRT
Data source: Estimates Developed by the UN Inter-agency Group for Child Mortality Estimation (UNICEF, WHO, World Bank, UN DESA Population Division) at www.childmortality.org.

## CO2 emissions (kilo tonnes) ##
Carbon dioxide emissions are those stemming from the burning of fossil fuels and the manufacture of cement. They include carbon dioxide produced during consumption of solid, liquid, and gas fuels and gas flaring.
Dataset: World Bank EN.ATM.CO2E.PC
Data source: Carbon Dioxide Information Analysis Center, Environmental Sciences Division, Oak Ridge National Laboratory, Tennessee, United States.

## Employment Ratio ##
Employment to population ratio, 15 years or older, total (%) (modeled ILO estimate)
Description: Employment to population ratio is the proportion of a country's population that is employed. Employment is defined as persons of working age who, during a short reference period, were engaged in any activity to produce goods or provide services for pay or profit, whether at work during 
the reference period (i.e. who worked in a job for at least one hour) or not at work due to temporary absence from a job, or to working-time arrangements. Ages 15 and older are generally considered the working-age population.
Dataset: SL.EMP.TOTL.SP.ZS. Data retrieved in September 2019.
Data Source: International Labour Organization, ILOSTAT database. 

## Agriculture ##
Agricultural land (% of land area)
Description: Agricultural land refers to the share of land area that is arable, under permanent crops, and under permanent pastures. Arable land includes land defined by the FAO as land under temporary crops (double-cropped areas are counted once), temporary meadows for mowing or for pasture, land under market or kitchen gardens, and land temporarily fallow. Land abandoned as a result of shifting cultivation is excluded. Land under permanent crops is land cultivated with crops that occupy the land for long periods and need not be replanted after each harvest, such as cocoa, coffee, and rubber. This category includes land under flowering shrubs, fruit trees, nut trees, and vines, but excludes land under trees grown for wood or timber. Permanent pasture is land used for five or more years for forage, including natural and cultivated crops.
Dataset: AG.LND.AGRI.ZS
Data source: Food and Agriculture Organization, electronic files and web site.

## Population ##
Description: Total population is based on the de facto definition of population, which counts all residents regardless of legal status or citizenship. The values shown are midyear estimates.
Dataset: SP.POP.TOTL
Data source: (1) United Nations Population Division. World Population Prospects: 2019 Revision. (2) Census reports and other statistical publications from national statistical offices, (3) Eurostat: Demographic Statistics, (4) United Nations Statistical Division. Population and Vital Statistics Reprot (various years), (5) U.S. Census Bureau: International Database, and (6) Secretariat of the Pacific Community: Statistics and Demography Programme.

## Peta Roberts. October 2019. Berkeley University Extension: Data Analytics and Visualization ##
