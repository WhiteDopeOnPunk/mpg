# Analyzing Fuel Economy
Predicting MPG of a vehicle using a linear regression model. Success will be evaluated using r-squared and root mean square error scores over time as well as testing with some unseen, futuristic, and very different data compared to the training set.

If you're curious about what goes into fuel economy, this should provide some insight on how it all works.

# Contents
1. [Clean](clean.ipynb) - Quick look to find any missing values, data of wrong types. Make sure data is in ranges that make sense
2. [EDA](eda.ipynb) - Investigate outliers and other items of interest. Manufacture some new features
3. [Model](model.ipynb) - Model and make predictions. Introduce some outside data

# Libs
* pandas
* numpy
* seaborn
* os
* matplotlib
* Ipython
* sklearn

# The Data
Using https://archive-beta.ics.uci.edu/ml/datasets/auto+mpg
1. Title: Auto-Mpg Data

2. Sources:
   (a) Origin:  This dataset was taken from the StatLib library which is
                maintained at Carnegie Mellon University. The dataset was 
                used in the 1983 American Statistical Association Exposition.
   (c) Date: July 7, 1993

3. Past Usage:
    -  See 2b (above)
    -  Quinlan,R. (1993). Combining Instance-Based and Model-Based Learning.
       In Proceedings on the Tenth International Conference of Machine 
       Learning, 236-243, University of Massachusetts, Amherst. Morgan
       Kaufmann.

4. Relevant Information:

   This dataset is a slightly modified version of the dataset provided in
   the StatLib library.  In line with the use by Ross Quinlan (1993) in
   predicting the attribute "mpg", 8 of the original instances were removed 
   because they had unknown values for the "mpg" attribute.  The original 
   dataset is available in the file "auto-mpg.data-original".

   "The data concerns city-cycle fuel consumption in miles per gallon,
    to be predicted in terms of 3 multivalued discrete and 5 continuous
    attributes." (Quinlan, 1993)

5. Number of Instances: 398

6. Number of Attributes: 9 including the class attribute

7. Attribute Information:

    1. mpg:           continuous
    2. cylinders:     multi-valued discrete
    3. displacement:  continuous
    4. horsepower:    continuous
    5. weight:        continuous
    6. acceleration:  continuous
    7. model year:    multi-valued discrete
    8. origin:        multi-valued discrete
    9. car name:      string (unique for each instance)

8. Missing Attribute Values:  horsepower has 6 missing values

# Summary

## A bit on engines:

* A most basic description of an engine is that it's an air pump
* Horsepower = (Torque * RPM) / 5252
* Torque peak is where an engine is operating most efficiently as far as air flow, applied science in action. (Fluid dynamics, resonance)
* Operating above or below the torque peak reduces efficiency and efficiency == fuel economy
* Torque peaks normally occur below 5252rpm, and horsepower peaks above that, so long as the engine can actually rev that high. On a dyno sheet (measuring torque and horsepower vs rpm) you'll see the torque/horsepower lines cross at 5252rpm
* As an engine spins faster, the power output increases until combustion is so inefficient and it produces so little torque that spinning faster produces no more power, if it holds together that long

Basically an engine that makes lots of power at high rpm but relatively little low end torque (mazda rotary), is going to have poor fuel economy because it spends most of its time outside of its efficiency range during normal driving. In contrast, diesel engines typically turn lower rpms and create all kinds of torque down low. So not only do they start off making more torque but they are less likely to stray very far from torque peak during normal driving. This is also why horsepower numbers on a diesel appear low, because they can't rev as high. There's more to it than this but this should be enough to provide context.

# Features
From the original features I chose:
* mpg: target
* number of cylinders
* engine displacement (in cubic inches)
* Horsepower
* Total weight of vehicle

From these I then calculated:
* Efficiency: HP per cubic inch - as this increases so does MPG
* Load: cubic inches per lb of weight - metric of how hard the engine has to work compared to its size. Engines that work hard use more fuel and a small engine working really hard can use more fuel than a big engine not doing much
* Bore_size: cubic inches per cylinder - best attempt to describe cylinder bore diameter which gives insight on torque curve
* Grunt: bore size divided by efficiency - an attempt to describe the power curve of an engine, or more specifically the presence/absence of low rpm torque output

All of these features are continuous.

# Model

Into the model:
* Horsepower
* Displacement
* Number of cylinders
* Weight

Which then gets turned into:
* Horsepower
* Bore_size
* Grunt
* Load

Then sent through:
* Quantile Transformer
* Linear Regression