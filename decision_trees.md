# Decision Trees

## Exercise

Paris airport wants to predict flight delays. They have collected a sample of flight conditions in the following table:

 | wind | season | company | day | status
---|--------|-------|--------|--------|--------
1 | windy | summer | AirFrance | week_day | on_time
2 | windy | winter | EasyJet | week_day | delayed
3 | windy | automn | EasyJet | week_day |delayed
4 | no_wind | summer | Corsair | week-end | delayed
5 | windy | winter  | AirFrance | week-end | delayed
6 | no_wind | summer | Corsair | week_day | delayed
7 | no_wind | spring |  AirFrance| week_day | on_time
8 | windy | automn  | EasyJet | week-end | delayed
9 | no_wind | winter | Corsair | week-end | delayed
10 | no_wind  | spring | Corsair | week_day | delayed
11 | windy | automn | EasyJet | week_day | delayed
12 | windy | spring | Corsair | week_day | on_time
13 | windy | summer | AirFrance |  week_day | on_time
14 | no_wind | automn | AirFrance | week_day | on_time
15 | windy | winter | AirFrance |week_day  | delayed
16 | no_wind | automn | Corsair | week_day | delayed
17 | windy | summer | Corsair | week_day | delayed
18 | windy | spring | Corsair | week-end | delayed
19 | no_wind | winter | EasyJet | week_day | on_time
20 | no_wind | spring | EasyJet | week-end | on_time

You are in charge of building a decision tree to predict whether or not a flight will be delayed.

**Question**
> For each attribute, compute the state frequancy table according to the following example : 



wind | delayed | on time
---|--------|-------|
 no wind | 5 | 4 
 windy | 8 | 3 

One of the possible criterion to build a decision network is the information gain. The information gain of an attribute A is the difference between the entropy of all the samples `M` and the sum of the entropies of the sets obtained by separating the samples according to the variable A. 


$$IG(M,A)  =  Ent(M) - \sum_{i\in A}\frac{|M_i|}{|M|}Ent(M_i)$$

For a discrete random variable with $s$ modalities of probability $p_s$, the entropy is computed as : 

$$Ent(M) = - \sum_s p_s \log(p_s)$$

This gives : 
$$
IG(M,A)  =  Ent(M) + \sum_{i\in A}\frac{|M_i|}{|M|} \sum_s \frac{|M_i^s|}{|M_i|}  \log( \frac{|M_i^s|}{|M_i|})
$$
and since $\sum_{i\in A}|M_i| = |M|$,
$$
IG(M,A)  =  Ent(M) + \sum_{i\in A}\frac{1}{|M|} \bigg[\big(\sum_s |M_i^s|  \log(|M_i^s| )) - |M_i| \log(|M_i|) \bigg]\\
   $$

We want the attribute which maximises $IG(M,A)$, but since  $Ent(M)$ is constant, we only need to maximise 
$$\hat{IG}(M,A)  =   \sum_{i\in A}\bigg[\big(\sum_s |M_i^s|  \log(|M_i^s| )) - |M_i| \log(|M_i|) \bigg]\\$$
