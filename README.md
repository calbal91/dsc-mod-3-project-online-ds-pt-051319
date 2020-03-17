# Informing Business Strategy With Hypothesis Testing


## The Objective
Test 4 hypotheses using the data for a fictional food trading company, in order to create actionable recommendations to guide business strategy.

## The Motivation
Using data to help guide business strategy is the bread and butter of any professional data scientist.

Hypothesis testing is especially important, since it allows us to see if an observed correlation is in fact a causal relationship; a particular marketing method genuinely leads to better conversion, certain salespeople are genuinely better at building customer loyalty, a new store layout genuinely produces higher sales, and so forth.

## The Technologies Used
* Pandas for Data Manipulation
* Matplotlib / Seaborn for Data Visualisation
* Numpy for creating the Monte Carlo simulation functions

## The Process Overview
The first step is deciding which hypotheses to test. This decision can be guided by a combination of two things:
* Inspecting the data that's actually available to us
* Working through an issue tree to understand the potential drivers of company performance

We would then need to create a function that will efficiently perform Monte Carlo Simulations, which will form the basis of our tests (we could try to use more traditional methods, such as T-Tests, however this rests on a number of assumptions - namely that our data is normally distributed - something that is difficult to guarantee).

### The Available Data
The data is provided to us in an SQL database with the following Schema.

![SQLSchema](https://github.com/calbal91/project-northwind-trading/blob/master/Images/Northwind_ERD_updated.png)

Looking into some headline measures, we see that revenues are growing...

![Revenue](https://github.com/calbal91/project-northwind-trading/blob/master/Images/NWRevenue.png)

... but that this is being driven mostly by an increase in unique customers...

![Customers](https://github.com/calbal91/project-northwind-trading/blob/master/Images/NWCustomers.png)

... rather than a deepening of customer relationships

![RevenuePerCustomer](https://github.com/calbal91/project-northwind-trading/blob/master/Images/NWRevPerCust.png)


### The Issue Tree
We can use an issue tree to explore the different drivers of company profits (rather than simply widening the customer base).

![IssueTree](https://github.com/calbal91/project-northwind-trading/blob/master/Images/IssueTree.png)

This allows us to pull out hypotheses that we know will have business relevance.


## The Hypothesis Testing

### On Monte Carlo Simulations

We use Monte Carlo Simulations (of Permutation Tests) to test our hypotheses.

![IssueTree](https://github.com/calbal91/project-northwind-trading/blob/master/Images/MCS1.png)

![IssueTree](https://github.com/calbal91/project-northwind-trading/blob/master/Images/MCS2.png)

The advantage of using Monte Carlo simulations is that we don't need to worry about the assumptions (e.g. normally distributed data) that are required for parametric tests like T-Tests.


### Designing Hypothesis Tests

Rather than 


### Hypothesis 1: 
We first address missing spurious data, dropping columns that are mostly NAN values.

We can create histograms of each feature to check for outliers. We can trim any properties from the dataset that are vastly unrepresentitive of the property market (e.g. houses selling for more than $1m, or with more than 7 bedrooms, etc.)

![Seattle](https://github.com/calbal91/project-king-county-housing/blob/master/Images/Distributions.png)

There may also be hidden outliers, which are unrepresentitive, but may not show up if we look at the features in isolation. For example, properties may be very expensive relative to their size, but may have a price that might not look silly in and of itself. We can thus create new features, such as price per square foot, which will help us further narrow down the dataset.

We can also visualise non-obvious categorical variables using scatter plots:

![Seattle](https://github.com/calbal91/project-king-county-housing/blob/master/Images/Scatters.png)

These will need to be one hot encoded ahead of modelling.

### Geography
One key visualisation that comes out of the EDA is a scatter plot of house latitude vs longitude. As expected, this produces a map.

![Seattle](https://github.com/calbal91/project-king-county-housing/blob/master/Images/ScatterMap.png)

As for the effect of geography on price, we can see that there appears to be some kind a north/south divide, however this is not universal (the very North of the map is mostly blue, lower value properties, having been mostly purple slightly further South). Thus, we can not assume a linear relationship between price, and either of longitude or latitude.

However, it is clear that expensive properties are clustered. It may therefore be worth demarkating 'areas' within the dataset, based on the zipcode data. These can then be used as categories in a multivariate linear regression. Zipcode data is categoried (A-I) as per the zipcode map on the King County website: https://www.kingcounty.gov/services/gis/Maps/vmc/Boundaries.aspx

* A - Seattle, Shoreline, Lake Forest Park
* B - Kirkland, Kenmore, Bothell, Redmond, Woodinville
* C - Bellevue, Mercer Island, Newcastle
* D - Renton, Kent
* E - Burien, Normandy Park, Des Moines, SeaTac, Tukwilla, Vashon Island
* F - Federal Way, Auburn, Algona, Milton, Pacific
* G - Sammamish, Issaquah, Carnation, Duvall
* H - Covington, Maple Valley, Black Diamond, Enumclaw
* I - Snoquaimie, North Bend

![Seattle](https://github.com/calbal91/project-king-county-housing/blob/master/Images/KCZipCodes.jpg)

## The Model Building

To settle on the final features, we one-hot encode the categories as required, then perform some log transformations on a few variables to make them more 'normal' looking.

![Seattle](https://github.com/calbal91/project-king-county-housing/blob/master/Images/LogScaled.png)

We then go through an iterative process of removing features that have high levels of collinearity. This is necessary, since it is hard to seperate the effects of two features with high levels of collinearity - bad for the quality of the model.

Having done this, Scikit Learn generates a linear model with an R^2 of about 0.67 - in other words, the model can explain about two-thirds of the variance we observe in the dependent variable. We can see the general accuracy of the model's predictive capacity:

![Seattle](https://github.com/calbal91/project-king-county-housing/blob/master/Images/ModelPredictions.png)


## The Model In Action

By extracting the coefficients from the trained model, we can visualise how it predicts a house value in the following way:

![Seattle](https://github.com/calbal91/project-king-county-housing/blob/master/Images/ModelOutline.jpg)


We can then use the model to help us solve potential business cases pertaining to the King County housing market:

### Example 1

![Seattle](https://github.com/calbal91/project-king-county-housing/blob/master/Images/ModelCase1.jpg)


### Example 2

![Seattle](https://github.com/calbal91/project-king-county-housing/blob/master/Images/ModelCase2.jpg)


### Example 3

![Seattle](https://github.com/calbal91/project-king-county-housing/blob/master/Images/ModelCase3.jpg)