# Credit Basket Analysis

The model is used to analyze the dynamics of a well diversified credit basket of names. More specifically, the tool is capable of predicting correlated loss levels and pool spreads over time. These forecasts can be used to further gauge the possibility of hitting predefined spread trigger thresholds.

A key motivation is the fact that certain trades have critical transaction triggers which reference these values. For example, Leveraged Super Senior (LSS) deals may trigger actions once the pool spread of an underlying CDX basket of 125 investment grade names reaches a specified threshold for a given loss level and remaining term.

In the model, names are allowed to migrate ratings on an individual basis according to a credit transition matrix. At the same time, spreads are simulated using mean reverting processes with time dependent parameters. For the purpose of spread evolution, names are grouped into classes of similar credit ratings. This classification simplifies the number of inputs required. Time dependent process parameters allows the inclusion of a spread term structure if desired.

Overall, this model captures the combined stochastic behaviour of both name migrations and spread forecasts. To ensure consistency, all dynamic processes are appropriately correlated with one another. Ultimately, at any simulated time the current basket loss level and pool spread can be used to check for trigger breaches. The portion of first trigger breaches relative to the number of simulated paths is an important measure. It represents an estimate of the probability of ever experiencing a spread trigger breach.

Calibration of this model may be a challenge given the number of correlated inputs and limited
historical credit data. In this report we essentially test submitted inputs. In particular, we rely on the provided credit transition matrix and spread mean reverting parameters. On the other hand, we do investigate shocked scenarios as one way of assessing the sensitivity of the output to the supplied inputs.


Predicting an aggregate basket spread is highly non-trivial owing to the dynamics of the situation. The contribution to the overall spread from an individual name with a fixed credit rating is a stochastic process. On top of this, the credit rating of a given name may change at any time, despite there being no changes to the spreads of other names with similar ratings. Furthermore, all movements are correlated. That is, spreads are correlated with one another, migrations are correlated with one another and spreads are correlated with migrations. 

In order to forecast the basket loss level and pool spread it is essential to consider the combined effect of both spread and credit rating movements together. The proposed approach captures systemic risk, through grouped credit spread evolution, as well as idiosyncratic risk, through specific name migrations.

As suggested above, the spread of each name is not modeled individually. Instead, the names
in the basket are classified according to their credit rating. In this context we consider 7 such classes: ‘AAA’, ‘AA’, ‘A’, ‘BBB’, ‘BB’, ‘B’ and ‘DEFAULT’. In terms of notation, it is useful to index the ratings such that ‘AAA’ corresponds to 1, ‘AA’ corresponds to 2, etc, and finally ‘DEFAULT’ corresponds to 7. With the exception of ‘DEFAULT’, each credit rating spread, say Sit , is modeled as a mean reverting processes in the form

where dWit is a standard Brownian motion process and log Sit is the natural logarithm of Sit . The parameters μi, ai and _i represent the speed of reversion, the mean reversion level and
the volatility respectively.

Equation can be solved to yield

Note that although the spread processes are defined using continuous time, spread levels are
simulated at m discrete time steps per year. That is, we sample the spread path at the times.

At the same time frequency, names are assumed to migrate. To formalize this concept, suppose that represents the annual single step credit rating transition matrix. That is, pij is the probability
of moving from credit rating i to credit rating j in 1 year. Since we are interested in transitions m times per year, S reflect the _t time single step credit rating transition matrix. Furthermore, let
T be the cumulative _t time single step credit rating transition matrix by setting


References:

https://finpricing.com/lib/FxForwardCurve.html

https://derivatives.hcommons.org/2020/09/02/product/
