---
title: "Reproducible Research, Project 1"
author: "Ali Orlov"
date: "July 23, 2017"
output: html_document
---




###Loading the data and removing NAs

We are downloading the dataset from its url and unzipping the file to "step_data.csv".

<div class="chunk" id="unnamed-chunk-18"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">url</span> <span class="hl kwb">&lt;-</span> <span class="hl str">&quot;https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip&quot;</span>
<span class="hl std">destfile</span> <span class="hl kwb">&lt;-</span> <span class="hl str">&quot;step_data.zip&quot;</span>
<span class="hl kwd">download.file</span><span class="hl std">(url, destfile)</span>
<span class="hl kwd">unzip</span><span class="hl std">(destfile)</span>
<span class="hl std">activity</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">read.csv</span><span class="hl std">(</span><span class="hl str">&quot;activity.csv&quot;</span><span class="hl std">,</span> <span class="hl kwc">sep</span> <span class="hl std">=</span> <span class="hl str">&quot;,&quot;</span><span class="hl std">)</span>
</pre></div>
</div></div>

The variable names and the structure of the file are given by

<div class="chunk" id="unnamed-chunk-19"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">names</span><span class="hl std">(activity)</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] &quot;steps&quot;    &quot;date&quot;     &quot;interval&quot;
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">str</span><span class="hl std">(activity)</span>
</pre></div>
<div class="output"><pre class="knitr r">## 'data.frame':	17568 obs. of  3 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : Factor w/ 61 levels &quot;2012-10-01&quot;,&quot;2012-10-02&quot;,..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">head</span><span class="hl std">(activity[</span><span class="hl kwd">which</span><span class="hl std">(</span><span class="hl opt">!</span><span class="hl kwd">is.na</span><span class="hl std">(activity</span><span class="hl opt">$</span><span class="hl std">steps)), ])</span> <span class="hl com"># data set with NA rows removed</span>
</pre></div>
<div class="output"><pre class="knitr r">##     steps       date interval
## 289     0 2012-10-02        0
## 290     0 2012-10-02        5
## 291     0 2012-10-02       10
## 292     0 2012-10-02       15
## 293     0 2012-10-02       20
## 294     0 2012-10-02       25
</pre></div>
</div></div>

The file is ready for analysis without further necessary processing.

###Analysing the data

Mean of "total number of step taken per day" over all days
Group the number of steps by date and intervals. Find the total number of steps per day over all days. Note that some of the days such as 2012-10-01 have no steps data. We remove such rows for this part.

<div class="chunk" id="unnamed-chunk-20"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">(reshape2)</span>
<span class="hl std">activity_melt</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">melt</span><span class="hl std">(activity[</span><span class="hl kwd">which</span><span class="hl std">(</span><span class="hl opt">!</span><span class="hl kwd">is.na</span><span class="hl std">(activity</span><span class="hl opt">$</span><span class="hl std">steps)), ],</span> <span class="hl kwc">id.vars</span> <span class="hl std">=</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;date&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;interval&quot;</span><span class="hl std">))</span>
<span class="hl kwd">head</span><span class="hl std">(activity_melt)</span>
</pre></div>
<div class="output"><pre class="knitr r">##         date interval variable value
## 1 2012-10-02        0    steps     0
## 2 2012-10-02        5    steps     0
## 3 2012-10-02       10    steps     0
## 4 2012-10-02       15    steps     0
## 5 2012-10-02       20    steps     0
## 6 2012-10-02       25    steps     0
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl std">steps_sum</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">dcast</span><span class="hl std">(activity_melt, date</span> <span class="hl opt">~</span> <span class="hl std">variable, sum)</span>
<span class="hl kwd">head</span><span class="hl std">(steps_sum)</span>
</pre></div>
<div class="output"><pre class="knitr r">##         date steps
## 1 2012-10-02   126
## 2 2012-10-03 11352
## 3 2012-10-04 12116
## 4 2012-10-05 13294
## 5 2012-10-06 15420
## 6 2012-10-07 11015
</pre></div>
</div></div>
Then we can find the mean of 'total number of steps per day'.

<div class="chunk" id="unnamed-chunk-21"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">summary</span><span class="hl std">(steps_sum</span><span class="hl opt">$</span><span class="hl std">steps)</span>
</pre></div>
<div class="output"><pre class="knitr r">##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##      41    8841   10760   10770   13290   21190
</pre></div>
</div></div>

Histogram of the total number of steps taken each day.
<div class="chunk" id="unnamed-chunk-22"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">hist</span><span class="hl std">(steps_sum</span><span class="hl opt">$</span><span class="hl std">steps,</span> <span class="hl kwc">main</span> <span class="hl std">=</span> <span class="hl str">&quot;Histogram of total steps taken per day&quot;</span><span class="hl std">,</span>
     <span class="hl kwc">xlab</span> <span class="hl std">=</span> <span class="hl str">&quot;Total steps per day&quot;</span><span class="hl std">,</span> <span class="hl kwc">ylab</span> <span class="hl std">=</span> <span class="hl str">&quot;Number of days&quot;</span><span class="hl std">,</span>
     <span class="hl kwc">breaks</span> <span class="hl std">=</span> <span class="hl num">10</span><span class="hl std">,</span> <span class="hl kwc">col</span> <span class="hl std">=</span> <span class="hl str">&quot;steel blue&quot;</span><span class="hl std">)</span>
<span class="hl kwd">abline</span><span class="hl std">(</span><span class="hl kwc">v</span> <span class="hl std">=</span> <span class="hl kwd">mean</span><span class="hl std">(steps_sum</span><span class="hl opt">$</span><span class="hl std">steps),</span> <span class="hl kwc">lty</span> <span class="hl std">=</span> <span class="hl num">1</span><span class="hl std">,</span> <span class="hl kwc">lwd</span> <span class="hl std">=</span> <span class="hl num">2</span><span class="hl std">,</span> <span class="hl kwc">col</span> <span class="hl std">=</span> <span class="hl str">&quot;red&quot;</span><span class="hl std">)</span>
<span class="hl kwd">abline</span><span class="hl std">(</span><span class="hl kwc">v</span> <span class="hl std">=</span> <span class="hl kwd">median</span><span class="hl std">(steps_sum</span><span class="hl opt">$</span><span class="hl std">steps),</span> <span class="hl kwc">lty</span> <span class="hl std">=</span> <span class="hl num">2</span><span class="hl std">,</span> <span class="hl kwc">lwd</span> <span class="hl std">=</span> <span class="hl num">2</span><span class="hl std">,</span> <span class="hl kwc">col</span> <span class="hl std">=</span> <span class="hl str">&quot;black&quot;</span><span class="hl std">)</span>
<span class="hl kwd">legend</span><span class="hl std">(</span><span class="hl kwc">x</span> <span class="hl std">=</span> <span class="hl str">&quot;topright&quot;</span><span class="hl std">,</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;Mean&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;Median&quot;</span><span class="hl std">),</span> <span class="hl kwc">col</span> <span class="hl std">=</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;red&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;black&quot;</span><span class="hl std">),</span>       <span class="hl kwc">lty</span> <span class="hl std">=</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl num">1</span><span class="hl std">,</span> <span class="hl num">2</span><span class="hl std">),</span> <span class="hl kwc">lwd</span> <span class="hl std">=</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl num">2</span><span class="hl std">,</span> <span class="hl num">2</span><span class="hl std">))</span>
</pre></div>
<div class="rimage default"><img src="figure/unnamed-chunk-22-1.png" title="plot of chunk unnamed-chunk-22" alt="plot of chunk unnamed-chunk-22" class="plot" /></div>
</div></div>

Equivalent ggplot.

<div class="chunk" id="unnamed-chunk-23"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">(ggplot2)</span>
<span class="hl kwd">ggplot</span><span class="hl std">(steps_sum,</span> <span class="hl kwd">aes</span><span class="hl std">(steps))</span> <span class="hl opt">+</span> <span class="hl kwd">geom_histogram</span><span class="hl std">(</span><span class="hl kwc">bins</span> <span class="hl std">=</span> <span class="hl num">10</span><span class="hl std">)</span>
</pre></div>
<div class="rimage default"><img src="figure/unnamed-chunk-23-1.png" title="plot of chunk unnamed-chunk-23" alt="plot of chunk unnamed-chunk-23" class="plot" /></div>
</div></div>

Here is another plot showing the trend in total number of steps taken per day over two months.

<div class="chunk" id="unnamed-chunk-24"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">(lubridate)</span>
<span class="hl std">steps_sum</span><span class="hl opt">$</span><span class="hl std">date</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">as.Date</span><span class="hl std">(steps_sum</span><span class="hl opt">$</span><span class="hl std">date)</span>
<span class="hl kwd">ggplot</span><span class="hl std">(steps_sum,</span> <span class="hl kwd">aes</span><span class="hl std">(date, steps))</span> <span class="hl opt">+</span> <span class="hl kwd">geom_line</span><span class="hl std">()</span> <span class="hl opt">+</span>
        <span class="hl kwd">scale_x_date</span><span class="hl std">(</span><span class="hl kwc">date_labels</span> <span class="hl std">=</span> <span class="hl str">&quot;%b %d&quot;</span><span class="hl std">)</span> <span class="hl opt">+</span>
        <span class="hl kwd">ylab</span><span class="hl std">(</span><span class="hl str">&quot;Total number of steps&quot;</span><span class="hl std">)</span>
</pre></div>
<div class="rimage default"><img src="figure/unnamed-chunk-24-1.png" title="plot of chunk unnamed-chunk-24" alt="plot of chunk unnamed-chunk-24" class="plot" /></div>
</div></div>

###Average daily activity pattern

In this section, we make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken averaged across all days.

<div class="chunk" id="unnamed-chunk-25"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">stepsmeaninterval</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">dcast</span><span class="hl std">(activity_melt, interval</span> <span class="hl opt">~</span> <span class="hl std">variable, mean,</span> <span class="hl kwc">na.rm</span> <span class="hl std">=</span> <span class="hl num">TRUE</span><span class="hl std">)</span>
<span class="hl kwd">head</span><span class="hl std">(stepsmeaninterval)</span>
</pre></div>
<div class="output"><pre class="knitr r">##   interval     steps
## 1        0 1.7169811
## 2        5 0.3396226
## 3       10 0.1320755
## 4       15 0.1509434
## 5       20 0.0754717
## 6       25 2.0943396
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">plot</span><span class="hl std">(stepsmeaninterval</span><span class="hl opt">$</span><span class="hl std">interval, stepsmeaninterval</span><span class="hl opt">$</span><span class="hl std">steps,</span> <span class="hl kwc">ty</span> <span class="hl std">=</span> <span class="hl str">&quot;l&quot;</span><span class="hl std">,</span>
     <span class="hl kwc">xlab</span> <span class="hl std">=</span> <span class="hl str">&quot;time interval&quot;</span><span class="hl std">,</span> <span class="hl kwc">ylab</span> <span class="hl std">=</span> <span class="hl str">&quot;Average steps&quot;</span><span class="hl std">,</span> <span class="hl kwc">main</span> <span class="hl std">=</span> <span class="hl str">&quot;Average 
     steps taken over all days vs \n time interval&quot;</span><span class="hl std">)</span>
</pre></div>
<div class="rimage default"><img src="figure/unnamed-chunk-25-1.png" title="plot of chunk unnamed-chunk-25" alt="plot of chunk unnamed-chunk-25" class="plot" /></div>
</div></div>

The time interval during which the maximum number of steps is taken is

<div class="chunk" id="unnamed-chunk-26"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">maxsteps_interval</span> <span class="hl kwb">&lt;-</span> <span class="hl std">stepsmeaninterval</span><span class="hl opt">$</span><span class="hl std">interval[</span><span class="hl kwd">which.max</span><span class="hl std">(stepsmeaninterval</span><span class="hl opt">$</span><span class="hl std">steps)]</span>
<span class="hl std">maxsteps_interval</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] 835
</pre></div>
</div></div>

###Imputing missing values

First of all, let us get a sense for the missing values. Are there days with all time intervals reporting NA step values?

We can replace the missing data for a day by the time average over all other days.


<div class="chunk" id="unnamed-chunk-27"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">activity2</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">split</span><span class="hl std">(activity, activity</span><span class="hl opt">$</span><span class="hl std">interval)</span>
<span class="hl std">activity2</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">lapply</span><span class="hl std">(activity2,</span> <span class="hl kwa">function</span><span class="hl std">(</span><span class="hl kwc">x</span><span class="hl std">) {</span>
        <span class="hl std">x</span><span class="hl opt">$</span><span class="hl std">steps[</span><span class="hl kwd">which</span><span class="hl std">(</span><span class="hl kwd">is.na</span><span class="hl std">(x</span><span class="hl opt">$</span><span class="hl std">steps))]</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">mean</span><span class="hl std">(x</span><span class="hl opt">$</span><span class="hl std">steps,</span> <span class="hl kwc">na.rm</span> <span class="hl std">=</span> <span class="hl num">TRUE</span><span class="hl std">)</span>
        <span class="hl kwd">return</span><span class="hl std">(x)</span>
<span class="hl std">})</span>
<span class="hl std">activity2</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">do.call</span><span class="hl std">(</span><span class="hl str">&quot;rbind&quot;</span><span class="hl std">, activity2)</span>
<span class="hl kwd">row.names</span><span class="hl std">(activity2)</span> <span class="hl kwb">&lt;-</span> <span class="hl kwa">NULL</span>

<span class="hl std">activity2</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">split</span><span class="hl std">(activity2, activity2</span><span class="hl opt">$</span><span class="hl std">date)</span>
<span class="hl std">df</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">lapply</span><span class="hl std">(activity2,</span> <span class="hl kwa">function</span><span class="hl std">(</span><span class="hl kwc">x</span><span class="hl std">) {</span>
        <span class="hl std">x</span><span class="hl opt">$</span><span class="hl std">steps[</span><span class="hl kwd">which</span><span class="hl std">(</span><span class="hl kwd">is.na</span><span class="hl std">(x</span><span class="hl opt">$</span><span class="hl std">steps))]</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">mean</span><span class="hl std">(x</span><span class="hl opt">$</span><span class="hl std">steps,</span> <span class="hl kwc">na.rm</span> <span class="hl std">=</span> <span class="hl num">TRUE</span><span class="hl std">)</span>
        <span class="hl kwd">return</span><span class="hl std">(x)</span>
<span class="hl std">})</span>
<span class="hl std">activity2</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">do.call</span><span class="hl std">(</span><span class="hl str">&quot;rbind&quot;</span><span class="hl std">, activity2)</span>
<span class="hl kwd">row.names</span><span class="hl std">(activity2)</span> <span class="hl kwb">&lt;-</span> <span class="hl kwa">NULL</span>
<span class="hl kwd">head</span><span class="hl std">(activity2)</span>
</pre></div>
<div class="output"><pre class="knitr r">##       steps       date interval
## 1 1.7169811 2012-10-01        0
## 2 0.3396226 2012-10-01        5
## 3 0.1320755 2012-10-01       10
## 4 0.1509434 2012-10-01       15
## 5 0.0754717 2012-10-01       20
## 6 2.0943396 2012-10-01       25
</pre></div>
</div></div>

Assuming that the time intervals form a disjoint partitioning of 24 hrs, i.e. 1 day is found to be erroneous. The time interval for each day corresponds to approximately 40 hours, which refutes the intervals being disjoint.


<div class="chunk" id="unnamed-chunk-28"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">(reshape2)</span>
<span class="hl std">activity_melt2</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">melt</span><span class="hl std">(activity2,</span> <span class="hl kwc">id.vars</span> <span class="hl std">=</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;date&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;interval&quot;</span><span class="hl std">))</span>
<span class="hl std">steps_sum</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">dcast</span><span class="hl std">(activity_melt2, date</span> <span class="hl opt">~</span> <span class="hl std">variable, sum,</span> <span class="hl kwc">na.rm</span> <span class="hl std">=</span> <span class="hl num">TRUE</span><span class="hl std">)</span>
<span class="hl kwd">head</span><span class="hl std">(steps_sum)</span>
</pre></div>
<div class="output"><pre class="knitr r">##         date    steps
## 1 2012-10-01 10766.19
## 2 2012-10-02   126.00
## 3 2012-10-03 11352.00
## 4 2012-10-04 12116.00
## 5 2012-10-05 13294.00
## 6 2012-10-06 15420.00
</pre></div>
</div></div>

Histogram of the total number of steps taken each day with the imputed missing values.

<div class="chunk" id="unnamed-chunk-29"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">hist</span><span class="hl std">(steps_sum</span><span class="hl opt">$</span><span class="hl std">steps,</span> <span class="hl kwc">main</span> <span class="hl std">=</span> <span class="hl str">&quot;Histogram of total steps taken per day&quot;</span><span class="hl std">,</span>
     <span class="hl kwc">xlab</span> <span class="hl std">=</span> <span class="hl str">&quot;Total steps per day&quot;</span><span class="hl std">,</span> <span class="hl kwc">ylab</span> <span class="hl std">=</span> <span class="hl str">&quot;Number of days&quot;</span><span class="hl std">,</span>
     <span class="hl kwc">breaks</span> <span class="hl std">=</span> <span class="hl num">10</span><span class="hl std">,</span> <span class="hl kwc">col</span> <span class="hl std">=</span> <span class="hl str">&quot;steel blue&quot;</span><span class="hl std">)</span>
<span class="hl kwd">abline</span><span class="hl std">(</span><span class="hl kwc">v</span> <span class="hl std">=</span> <span class="hl kwd">mean</span><span class="hl std">(steps_sum</span><span class="hl opt">$</span><span class="hl std">steps),</span> <span class="hl kwc">lty</span> <span class="hl std">=</span> <span class="hl num">1</span><span class="hl std">,</span> <span class="hl kwc">lwd</span> <span class="hl std">=</span> <span class="hl num">2</span><span class="hl std">,</span> <span class="hl kwc">col</span> <span class="hl std">=</span> <span class="hl str">&quot;red&quot;</span><span class="hl std">)</span>
<span class="hl kwd">abline</span><span class="hl std">(</span><span class="hl kwc">v</span> <span class="hl std">=</span> <span class="hl kwd">median</span><span class="hl std">(steps_sum</span><span class="hl opt">$</span><span class="hl std">steps),</span> <span class="hl kwc">lty</span> <span class="hl std">=</span> <span class="hl num">2</span><span class="hl std">,</span> <span class="hl kwc">lwd</span> <span class="hl std">=</span> <span class="hl num">2</span><span class="hl std">,</span> <span class="hl kwc">col</span> <span class="hl std">=</span> <span class="hl str">&quot;black&quot;</span><span class="hl std">)</span>
<span class="hl kwd">legend</span><span class="hl std">(</span><span class="hl kwc">x</span> <span class="hl std">=</span> <span class="hl str">&quot;topright&quot;</span><span class="hl std">,</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;Mean&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;Median&quot;</span><span class="hl std">),</span> <span class="hl kwc">col</span> <span class="hl std">=</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;red&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;black&quot;</span><span class="hl std">),</span> <span class="hl kwc">lty</span> <span class="hl std">=</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl num">2</span><span class="hl std">,</span> <span class="hl num">1</span><span class="hl std">),</span> <span class="hl kwc">lwd</span> <span class="hl std">=</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl num">2</span><span class="hl std">,</span> <span class="hl num">2</span><span class="hl std">))</span>
</pre></div>
<div class="rimage default"><img src="figure/unnamed-chunk-29-1.png" title="plot of chunk unnamed-chunk-29" alt="plot of chunk unnamed-chunk-29" class="plot" /></div>
</div></div>

Number of rows with NA values

<div class="chunk" id="unnamed-chunk-30"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">sum</span><span class="hl std">(</span><span class="hl kwd">is.na</span><span class="hl std">(activity</span><span class="hl opt">$</span><span class="hl std">steps))</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] 2304
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">sum</span><span class="hl std">(</span><span class="hl kwd">is.na</span><span class="hl std">(activity</span><span class="hl opt">$</span><span class="hl std">steps))</span><span class="hl opt">*</span><span class="hl num">100</span><span class="hl opt">/</span><span class="hl kwd">nrow</span><span class="hl std">(activity)</span> <span class="hl com"># Percentage of rows with missing values</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] 13.11475
</pre></div>
</div></div>

Differences in activity patterns: Weekdays vs Weekends
Create a new column describing if the date is a weekday or weekend.

<div class="chunk" id="unnamed-chunk-31"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">(lubridate)</span>
<span class="hl std">weekends</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">which</span><span class="hl std">(</span><span class="hl kwd">weekdays</span><span class="hl std">(</span><span class="hl kwd">as.Date</span><span class="hl std">(activity2</span><span class="hl opt">$</span><span class="hl std">date))</span> <span class="hl opt">==</span> <span class="hl str">&quot;Saturday&quot;</span> <span class="hl opt">|</span>
              <span class="hl kwd">weekdays</span><span class="hl std">(</span><span class="hl kwd">as.Date</span><span class="hl std">(activity2</span><span class="hl opt">$</span><span class="hl std">date))</span> <span class="hl opt">==</span> <span class="hl str">&quot;Sunday&quot;</span><span class="hl std">)</span>
<span class="hl std">weekdays</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">which</span><span class="hl std">(</span><span class="hl kwd">weekdays</span><span class="hl std">(</span><span class="hl kwd">as.Date</span><span class="hl std">(activity2</span><span class="hl opt">$</span><span class="hl std">date))</span> <span class="hl opt">!=</span> <span class="hl str">&quot;Saturday&quot;</span> <span class="hl opt">&amp;</span>
              <span class="hl kwd">weekdays</span><span class="hl std">(</span><span class="hl kwd">as.Date</span><span class="hl std">(activity2</span><span class="hl opt">$</span><span class="hl std">date))</span> <span class="hl opt">!=</span> <span class="hl str">&quot;Sunday&quot;</span><span class="hl std">)</span>
<span class="hl std">temp</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl kwd">rep</span><span class="hl std">(</span><span class="hl str">&quot;a&quot;</span><span class="hl std">,</span> <span class="hl kwd">length</span><span class="hl std">(activity2)))</span>
<span class="hl std">temp[weekends]</span> <span class="hl kwb">&lt;-</span> <span class="hl str">&quot;weekend&quot;</span>
<span class="hl std">temp[weekdays]</span> <span class="hl kwb">&lt;-</span> <span class="hl str">&quot;weekday&quot;</span>
<span class="hl kwd">length</span><span class="hl std">(temp)</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] 17568
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">names</span><span class="hl std">(temp)</span> <span class="hl kwb">&lt;-</span> <span class="hl str">&quot;day&quot;</span>
<span class="hl std">activity2</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">cbind</span><span class="hl std">(activity2, temp)</span>
<span class="hl kwd">names</span><span class="hl std">(activity2)[</span><span class="hl num">4</span><span class="hl std">]</span> <span class="hl kwb">&lt;-</span> <span class="hl str">&quot;day&quot;</span>
</pre></div>
</div></div>

Steps taken over each interval averaged across weekday days and weekend days.

<div class="chunk" id="unnamed-chunk-32"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">activity2split</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">split</span><span class="hl std">(activity2, activity2</span><span class="hl opt">$</span><span class="hl std">day)</span>
<span class="hl std">stepsmean_interval</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">lapply</span><span class="hl std">(activity2split,</span> <span class="hl kwa">function</span><span class="hl std">(</span><span class="hl kwc">x</span><span class="hl std">) {</span>
        <span class="hl std">temp</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">aggregate</span><span class="hl std">(x</span><span class="hl opt">$</span><span class="hl std">steps,</span> <span class="hl kwd">list</span><span class="hl std">(x</span><span class="hl opt">$</span><span class="hl std">interval), mean)</span>
        <span class="hl kwd">names</span><span class="hl std">(temp)</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;interval&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;steps&quot;</span><span class="hl std">)</span>
        <span class="hl kwd">return</span><span class="hl std">(temp)</span>
<span class="hl std">})</span>
</pre></div>
</div></div>

### Unsplit stepsmean_interval

<div class="chunk" id="unnamed-chunk-33"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">stepsmean_interval</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">do.call</span><span class="hl std">(</span><span class="hl str">&quot;rbind&quot;</span><span class="hl std">, stepsmean_interval)</span>
<span class="hl std">weekdays</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">grep</span><span class="hl std">(</span><span class="hl str">&quot;weekday&quot;</span> <span class="hl std">,</span><span class="hl kwd">row.names</span><span class="hl std">(stepsmean_interval))</span>
<span class="hl std">weekends</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">grep</span><span class="hl std">(</span><span class="hl str">&quot;weekend&quot;</span> <span class="hl std">,</span><span class="hl kwd">row.names</span><span class="hl std">(stepsmean_interval))</span>
<span class="hl std">temp</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl kwd">rep</span><span class="hl std">(</span><span class="hl str">&quot;a&quot;</span><span class="hl std">,</span> <span class="hl kwd">length</span><span class="hl std">(stepsmean_interval</span><span class="hl opt">$</span><span class="hl std">steps)))</span>
<span class="hl std">temp[weekdays]</span> <span class="hl kwb">&lt;-</span> <span class="hl str">&quot;weekdays&quot;</span>
<span class="hl std">temp[weekends]</span> <span class="hl kwb">&lt;-</span> <span class="hl str">&quot;weekends&quot;</span>
<span class="hl std">stepsmean_interval</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">cbind</span><span class="hl std">(stepsmean_interval, temp)</span>
<span class="hl kwd">row.names</span><span class="hl std">(stepsmean_interval)</span> <span class="hl kwb">&lt;-</span> <span class="hl kwa">NULL</span>
<span class="hl kwd">names</span><span class="hl std">(stepsmean_interval)[</span><span class="hl num">3</span><span class="hl std">]</span> <span class="hl kwb">&lt;-</span> <span class="hl str">&quot;day&quot;</span>
<span class="hl kwd">head</span><span class="hl std">(stepsmean_interval)</span>
</pre></div>
<div class="output"><pre class="knitr r">##   interval      steps      day
## 1        0 2.25115304 weekdays
## 2        5 0.44528302 weekdays
## 3       10 0.17316562 weekdays
## 4       15 0.19790356 weekdays
## 5       20 0.09895178 weekdays
## 6       25 1.59035639 weekdays
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">tail</span><span class="hl std">(stepsmean_interval)</span>
</pre></div>
<div class="output"><pre class="knitr r">##     interval       steps      day
## 571     2330  1.38797170 weekends
## 572     2335 11.58726415 weekends
## 573     2340  6.28773585 weekends
## 574     2345  1.70518868 weekends
## 575     2350  0.02830189 weekends
## 576     2355  0.13443396 weekends
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">(ggplot2)</span>
<span class="hl kwd">ggplot</span><span class="hl std">(stepsmean_interval,</span> <span class="hl kwd">aes</span><span class="hl std">(interval, steps))</span> <span class="hl opt">+</span> <span class="hl kwd">geom_line</span><span class="hl std">()</span> <span class="hl opt">+</span> <span class="hl kwd">facet_grid</span><span class="hl std">(day</span> <span class="hl opt">~</span> <span class="hl std">.)</span>
</pre></div>
<div class="rimage default"><img src="figure/unnamed-chunk-33-1.png" title="plot of chunk unnamed-chunk-33" alt="plot of chunk unnamed-chunk-33" class="plot" /></div>
</div></div>

The mean number of steps taken over the weekdays and weekends.

<div class="chunk" id="unnamed-chunk-34"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">stepsdatamelt</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">melt</span><span class="hl std">(stepsmean_interval,</span> <span class="hl kwc">id.vars</span> <span class="hl std">=</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;interval&quot;</span><span class="hl std">,</span>
                                                      <span class="hl str">&quot;day&quot;</span><span class="hl std">))</span>
<span class="hl kwd">dcast</span><span class="hl std">(stepsdatamelt, day</span> <span class="hl opt">~</span> <span class="hl std">variable, mean)</span> <span class="hl com"># Average steps</span>
</pre></div>
<div class="output"><pre class="knitr r">##        day    steps
## 1 weekdays 35.61058
## 2 weekends 42.36640
</pre></div>
</div></div>

