# Informing Business Strategy With Hypothesis Testing


## The Objective

Test 4 different hypotheses using the data for Northwind, a food trading company, in order to create actionable recommendations to guide business strategy.

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

![PermTests](https://github.com/calbal91/project-northwind-trading/blob/master/Images/MCS1.jpg)

![MonteCarloSims](https://github.com/calbal91/project-northwind-trading/blob/master/Images/MCS2.jpg)

The advantage of using Monte Carlo simulations is that we don't need to worry about the assumptions (e.g. normally distributed data) that are required for parametric tests like T-Tests.


### Designing Hypothesis Tests

Rather than performing a single experiment for each point, we can perform a series of experiments sequentially. Let's take the shipping cost experiments as an example. Here, we want to work out which shipping company is cheapest on average in each region. Then this becomes 36 sequential experiments, where we test if shipping company X is significantly cheaper in market Y based on cost KPI, Z.

![Experiments](https://github.com/calbal91/project-northwind-trading/blob/master/Images/Experiments.jpg)

This allows us to perform a thorough exploration of each point that we highlighted on the original issue tree.

### Experiment 1: Customer quality by region

Do certain regions have significantly worse revenue per customer than others? This would guide the business on where it should focus growing out the customer base.

![Hypothesis1](https://github.com/calbal91/project-northwind-trading/blob/master/Images/Hypothesis1.png)

### Experiment 2: The effect of discounting

Do discounts actually result in a significant increase in the size of orders? If not, then sales staff should minimise discounting in order to protect revenues. We can view the effect of discounting by product type.

![Hypothesis2](https://github.com/calbal91/project-northwind-trading/blob/master/Images/Hypothesis2.png)

### Experiment 3: Increasing customer order frequency

There is obvious value in getting customers to order more frequently. However, could it be the case that customers that order infrequently do so because they are buying significantly more goods with each order (thus meaning that they don't actually need to order that frequently).

We should work this out before encouraging sales staff to expend energy on any order frequency initiatives.

![Hypothesis3](https://github.com/calbal91/project-northwind-trading/blob/master/Images/Hypothesis3.png)

### Experiment 4: The cost of shipping firms

Are certain shipping companies significantly cheaper than others in given regions? If so, then Northwind could reduce costs by substituting deliveries to them, away from more expensive firms.

![Hypothesis4](https://github.com/calbal91/project-northwind-trading/blob/master/Images/Hypothesis4.png)