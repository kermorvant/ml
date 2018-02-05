# Decision Trees

## A simple example on a toy problem

### Exercise

Paris airport wants to predict flight delays. They have collected a sample of flight conditions in the following table:

 wind  | company | day | status
---|--------|-------|--------|--------|--------
windy|AirFrance|week_day|on_time
windy|EasyJet|week_day|delayed
no_wind|EasyJet|week_day|delayed
windy|AirFrance|week-end|delayed
no_wind|AirFrance|week_day|on_time
windy|EasyJet|week-end|delayed
windy|EasyJet|week_day|delayed
windy|AirFrance|week_day|on_time
no_wind|AirFrance|week_day|on_time
windy|AirFrance|week_day|delayed
no_wind|EasyJet|week_day|on_time
no_wind|EasyJet|week-end|on_time

You are in charge of building a decision tree to predict whether or not a flight will be delayed.

**Question**
>  Compute the state frequency table for the variable *wind* according to the following example for the variable *company*: 



day | delayed | on time
---|--------|-------|
 week-day | 4 | 5 
 week-end | 2 | 1 

One of the possible criterion to build a decision network is the information gain (IG). The information gain of an attribute A is the difference between the entropy of all the samples `M` and the sum of the entropies of the sets obtained by separating the samples according to the variable A. 


$$IG(M,A)  =  Ent(M) - \sum_{i\in A}\frac{|M_i|}{|M|}Ent(M_i)$$

For a discrete random variable with $s$ modalities of probability $p_s$, the entropy is computed as : 

$$Ent(M) = - \sum_s p_s \log(p_s)$$

This gives : 

$$IG(M,A)  =  Ent(M) + \sum_{i\in A}\frac{|M_i|}{|M|} \sum_s \frac{|M_i^s|}{|M_i|}  \log( \frac{|M_i^s|}{|M_i|})$$


We want the attribute which maximises `IG(M,A)`, but since  `Ent(M)` is constant, we only need to find the attribute which minimizes 

$$\sum_{i\in A}\frac{|M_i|}{|M|}Ent(M_i)$$

For the attribute *day*, we have 

$$\sum_{i\in A}\frac{|M_i|}{|M|}Ent(M_i) = \frac{9}{12}* \left( \frac{4}{9}log2(\frac{4}{9}) +\frac{5}{9}log2(\frac{5}{9})\right) + \frac{3}{12} * \left( \frac{2}{3}log2(\frac{2}{3}) +\frac{1}{3}log2(\frac{1}{3}) \right) $$

which is 0.97. For the attribute *company*, this formula gives 0.92.

> Compute the value for the attribute *wind*. 
> Which attribure  is used a for the first split ?
 
### Code

> Load the data 'flight.csv' corresponding to the data of the previous exercise.

In order to train a  decision trees on categorical variables, they must be encoded with integers. This encoding can be done with the [LabelEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.LabelEncoder.html) :

	from sklearn import preprocessing
	le = preprocessing.LabelEncoder()
	df['wind_label'] = le.fit_transform(df['wind'])
	
> * Create a new column for each attribute encoded with an integer label
> * Train a decision tree using the entropy criterion on all the data (use `DecisionTreeClassifier(criterion='entropy')`)
> * Check that the first splitting criterion chosen by the algorithm is the same a the one you found in the previous exercice.


