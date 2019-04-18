# TIME SERIES ANALYSIS IN R TO PREDICT THE THICKNESS OF OZONE LAYER FOR THE NEXT 5 YEARS
## TABLE OF CONTENTS

##### - INTRODUCTION 
##### - Load Data 
##### - Representation of time series 
##### - Generation of scatter plot - Pairs of conseq!{}uetive ozone layer relationship 
##### - Linear Model Calculation 
##### - Intercept and slope estimation using least square regression method 
##### - Residual Analysis - Linear trend 
##### - Checking The Normality of residuals 
##### - Quadratic model Calculation 
##### - Residual Analysis of Quadratic trend 
##### - Checking The Normality of residuals 
##### - Forecasting and Prediction 
##### - Forecasting using Quadratic Model 11
##### - Plotting the forecasted Time series values 
##### - CONCLUSION 

## INTRODUCTION
The ozone layer is the common term for the high concentration of ozone that is found in the stratosphere
around 15-30km above the earth’s surface. It covers the entire planet and protects life on earth by absorbing
harmful ultraviolet-B (UV-B) radiation from the sun. Ozone is a naturally occurring molecule. An ozone
molecule is made up of three oxygen atoms. It has the chemical formula O3. Depletion of the ozone layer
occurs globally, however,the severe depletion of the ozone layer over the Antarctic is often referred to as the
‘ozone hole’. Increased depletion has recently started occurring over the Arctic as well.
## Load Data
```
Library(TSA)
```
![](https://github.com/ksuraj93/Time-series-analysis-in-R-to-predict-the-thickness-of-ozone-layer/blob/master/pics/TSA.JPG)
 ```
 library(lmtest)
 ```
 ![](https://github.com/ksuraj93/Time-series-analysis-in-R-to-predict-the-thickness-of-ozone-layer/blob/master/pics/zoo.JPG)
 ```
 library(ggplot2)
library(forecast)
library(readr)
```
![](https://github.com/ksuraj93/Time-series-analysis-in-R-to-predict-the-thickness-of-ozone-layer/blob/master/pics/readr.JPG)
## Representation of time series
This time series dataset is designed in such a way that the mean function is known prior and the rest of the
functions form of trend can be determined. The time series trend mentioned here is a deterministic trend. A
linear function y =ax+b can be set or in some cases y=a+bx+cxˆ2. This will produce a straight line with
small fluctuations in the middle.
```
suraj = read.csv('data1.csv',col.names='Layer thickness', header = FALSE)
suraj = ts(as.vector(suraj), start = 1927,end=2016)
plot(suraj, type='o',
ylab='Layer thickness', xlab='Years',
main=' Ozone layer thickness, 1927 - 2016')
```
![](https://github.com/ksuraj93/Time-series-analysis-in-R-to-predict-the-thickness-of-ozone-layer/blob/master/pics/ozone%20layer%20thickness.JPG)
**ZLAG PLOTTING** - The changes in the ozone layer depletion is plotted. The difference with the previous
year s value is defined in the zlag. The variation in thickness of the ozone layer is plotted. The year 1952 had
the highest thickness whereas 1992 had very very less ozone layer thickness. The next year s data can be
predicted with the previous year s data.
Generation of scatter plot - Pairs of consequetive ozone layer relationship
```
plot(y=suraj,x=zlag(suraj),ylab='Ozone layer Thickness',
xlab='Previous Year Ozone layer Thickness')
```
![](https://github.com/ksuraj93/Time-series-analysis-in-R-to-predict-the-thickness-of-ozone-layer/blob/master/pics/previous%20yr%20thickness.JPG)
### Calculation of OZone layer thickness
```
y = suraj
x = zlag(y)
index = 2:length(x)
cor(y[index],x[index])
```
## [1] 0.8700381
The corelation between the previous year and the present year. The correlation is 0.87 High co relation has
been observed. Seasonality is not possible to observe. 
## Linear and Quadratic MODEL
### Linear Model Calculation
A linear trend model with the equation y=ax+b, where a is the intercept and b is the slope. This graph
shows the linear trend model. The thickness of the ozone layer decreases as time progresses. In the linear
trend model shown above, the thickness of teh ozone layer starts at the top left after which it falls below the
mean line.
The high variance is attributed with the increase in years. Apparently in the year 1954 the ozone layer is at
the highest thickness level. Following this, the ozone layer thickness is continously decreasing towards the
negative value from the year 2014 to 2016.
The calculated values of slope and intercept are b=-0.1100 and a=213.72, the significance level of the slope is
at 5%
## Intercept and slope estimation using least square regression method
```
model1 = lm(y~time(y))
plot(y, type='o',
ylab='Dobson Units', xlab='Years'main=' Change in Thickness of Ozone layer')
abline(model1, col ="blue")
grid()
```
![](https://github.com/ksuraj93/Time-series-analysis-in-R-to-predict-the-thickness-of-ozone-layer/blob/master/pics/change%20in%20thickness%20ozone.JPG)
```
summary(model1)
```
![](https://github.com/ksuraj93/Time-series-analysis-in-R-to-predict-the-thickness-of-ozone-layer/blob/master/pics/call%203.JPG)
## Residual Analysis - Linear trend
```
checkresiduals(rstudent(model1))
```
![](https://github.com/ksuraj93/Time-series-analysis-in-R-to-predict-the-thickness-of-ozone-layer/blob/master/pics/residual%20-%20linear%20trend%201.JPG)
**ACF** - corelation function for quadratic trends which indicate that some lines are crossing the significance
level. It can be concluded that the process is not an white noise process.
**HISTOGRAM** - used to check the normality of residuals. The frequency histogram of standardised residuals
is depicted here. In the above Histogram, the plot is not symmetric at Middle. It does not tail off at both the
high and low ends and we cannot say accurately that the histogram of residual values is normally distributed.
Checking The Normality of residuals
```
y1 = rstudent(model1)
qqnorm(y1)
qqline(y1, col = 2, lwd = 1, lty = 2)
```
![](https://github.com/ksuraj93/Time-series-analysis-in-R-to-predict-the-thickness-of-ozone-layer/blob/master/pics/normall%20qq%20plot%202.JPG)
Standardised residuals for the dataset. The staright line pattern represents a normally distributed stochastic
component in the above model. Comparing the linear and quadratic model, there is staright line with a few a
few outliers. Out of the both models the quadratic trend model seems to fit the dataset more accurately than
the linear trend model, the reason being there are less outliers in the quadratic model and there are more
outliers in the linear trend model. The less number of outliers is attributed with more number of points lying
on the line. The normality assumption can be tested with various hypothesis tests
```
shapiro.test(y1)
```
![](https://github.com/ksuraj93/Time-series-analysis-in-R-to-predict-the-thickness-of-ozone-layer/blob/master/pics/shapiro.JPG)
The values derived from the shapiro test indicate that the distribution of data are not different from a normal
distribution as the pvalue > 0.05 for both the trend models. We do not reject the hypothesis.
## Quadratic model Calculation
```
t = time(y)
t2 = t^2
model2 = lm(y ~ t+t2) # label the model as model1
#t = time(ozone.ts)
#t2 = t^2
#model.ozone.quad = lm(ozone.ts~ t + t2)
#summary(model.ozone.quad)
summary(model2)
```
![](https://github.com/ksuraj93/Time-series-analysis-in-R-to-predict-the-thickness-of-ozone-layer/blob/master/pics/call%203.JPG)
mentioned above as y=a+bx+cxˆ2 where “a” is the intercept, “b” is the linear trend, and “c” is the quadratic
trend in time a=-5.733e+03,b=5.924e+00 and C=-1.530e-03 .
It is evident that on seeing the p -values, the quadratic trend term is significant. It can be concluded that
after analysing both linear and quadratic trend, the Quadratic trend suits well for the “Ozone layer thickness
of the earth” dataset. The reason being it fits more points in the line than to that of the linear trend model.
There are Symmetric points on both sides of the Quadratic curve than the linear trend model curve. If
the R-squared is increased it will reduce the error std deviation by approx 10 percent . Since Quadratic
trend model has a higher std deviation it is the perfect fit. According to the p-values, quadratic trend is
found significant and the value of multiple R-squared is 0.73 which is better than the linear model #Fitted
quadratic trend curve claculation
```
plot(ts(fitted(model2)), ylim = c(min(c(fitted(model2),
as.vector(y))), max(c(fitted(model2),as.vector(y)))),
ylab='layer Thickness' ,
main = "Quadratic curve",xlab='Time(Years)' )
lines(as.vector(y),type="o")
```
![](https://github.com/ksuraj93/Time-series-analysis-in-R-to-predict-the-thickness-of-ozone-layer/blob/master/pics/quadratic%20curve%204.JPG)
## Residual Analysis of Quadratic trend
```
checkresiduals(rstudent(model2))
```
![](https://github.com/ksuraj93/Time-series-analysis-in-R-to-predict-the-thickness-of-ozone-layer/blob/master/pics/residuals%20acf%20histogram%205.JPG)
ACF - corelation function for quadratic trends which indicate that some lines are crossing the significance
level. It can be concluded that the process is not an white noise process.
HISTOGRAM - used to check the normality of residuals. The frequency histogram of standardised residuals
is depicted here. In the above Histogram, the plot is not symmetric at Middle. It does not tail off at both the
high and low ends and we cannot say accurately that the histogram of residual values is normally distributed.
## Checking The Normality of residuals
```
y1 = rstudent(model2)
qqnorm(y1)
qqline(y1, col = 2, lwd = 1, lty = 2)
```
![](https://github.com/ksuraj93/Time-series-analysis-in-R-to-predict-the-thickness-of-ozone-layer/blob/master/pics/normal%20qq%20plot%206.JPG)
```
shapiro.test(y1)
```
**Shapiro-Wilk normality test
 data: y1
 W = 0.98889, p-value = 0.6493**
## Forecasting and Prediction
### Forecasting using Quadratic Model
```
t=time(suraj)
h = 5
z = seq((as.vector(tail(t,1))+1), (as.vector(tail(t,1))+h))
pred = data.frame(t = z, t2 = z^2)
forecasts = predict(model2, pred, interval = "prediction")
knitr::kable(forecasts, caption="Prediction of 5 Year Values")
```
![](https://github.com/ksuraj93/Time-series-analysis-in-R-to-predict-the-thickness-of-ozone-layer/blob/master/pics/forecasted_1.JPG)
![](https://github.com/ksuraj93/Time-series-analysis-in-R-to-predict-the-thickness-of-ozone-layer/blob/master/pics/forecasted_2.JPG)
## Plotting the forecasted Time series values
```
plot(suraj, xlim = c(1927,2021), ylim = c(-18, 12),
ylab = "Ozone layer thickness", xlab='Time(Years)',
main='Prediction of 5 year values')
lines(ts(as.vector(forecasts[,1]), start = 2017), col="red", type="l")
lines(ts(as.vector(forecasts[,2]), start = 2017), col="blue", type="l")
lines(ts(as.vector(forecasts[,3]), start = 2017), col="blue", type="l")
legend(x="topleft", lty=1, pch=1, col=c("black","blue","red"),text.width = 10,
c("suraj","5% forecast limits", "Forecast"),
bty = 'n'
```
![](https://github.com/ksuraj93/Time-series-analysis-in-R-to-predict-the-thickness-of-ozone-layer/blob/master/pics/prediction%205%20yr%20value%20-%20last.JPG)
## CONCLUSION
We conclude that, based on the giveen dataset, the operations of modelling and estimating deterministic
trends in time series were performed. It can be found that, the ozone layer thickness can be estimated using
a Quadratic trend model and using a hypothesis tests. The hypothesis tests are QQ plot and Shiapro test.
For the given dataset the Quadratic trend model is the better at fitting the data than the linear model as the
r Squared values are higher. As the time passes by there is a gradual decrease in the ozone layer thickness.
### CONTACT :
#### SURAJ KANNAN - DATA SCIENTIST
#### EMAIL : mahi.suraj93@gmail.com
#### WEBSITE & E-PORTFOLIO : [click me](http://www.surajkannan.com/)
