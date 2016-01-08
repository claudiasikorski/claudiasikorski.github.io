---
layout: post
title: Combining Plots R Tutorial - Risk Factors for Coronary Artery Disease
tags:
- intro
- beginner
- r
- git
- linux
- coronary artery disease
categories:
- Git
- Linux
---


# Background

During my work term at Hamilton General Hospital as a Research Assistant in Cardiology, I realised that I may be interested in a career in health informatics. With this in mind, I decided to learn the open source programming language called R as it is a great program for graphics and statistical computing. While the learning curve is huge and very time consumming(especially for me given that I had no previous coding experience), I am happy I put the time in to learn it! As a Research Assisant for the Cardiolinguistics Research Study of Dr. Catherine Kreatsoulas, I interviewd patients with suspected CAD (coronary artery disease) each day about their pain symptoms to determine if symptoms are described differently based on gender. This is just one of the objectives of the study, you can read more about the study [here](http://claudiasikorski.github.io/pdfs/cardiolinguisticsprotocol.pdf). 

Each day I would ask patients about their risk factors for CAD, and so for my first mini project I decided to look at the risk factors for CAD by gender and by province in Canada. I was curious about the percentage of the population that had these risk factors and how they differed by gender. In the end I made this figure to create a simple way of looking at the various risk factors... 

![](/pdfs/cad-project.png)



First let us load the required packages we will be using to construct this figure.

```{r}
library(reshape2)
library(ggplot2)
library(plyr)
library(grid)
library(gridExtra)
library(devtools)
library(easyGgplot2) 
```

Please note, easyGgplot2 is not a stadard package on CRAN yet.  Please install is maually using the instructions [here](https://github.com/kassambara/easyGgplot2).


Now let us load up the data.  This data is from Staistics Canada from the following links for [smoking](http://www.statcan.gc.ca/tables-tableaux/sum-som/l01/cst01/health74b-eng.htm), [second hand smoke exposure](http://www.statcan.gc.ca/tables-tableaux/sum-som/l01/cst01/health96b-eng.htm), [BMI](http://www.statcan.gc.ca/tables-tableaux/sum-som/l01/cst01/health82b-eng.htm), [diabetes](http://www.statcan.gc.ca/tables-tableaux/sum-som/l01/cst01/health54b-eng.htm), [high blood pressure](http://www.statcan.gc.ca/tables-tableaux/sum-som/l01/cst01/health70a-eng.htm), [heavy drinking](http://www.statcan.gc.ca/tables-tableaux/sum-som/l01/cst01/health80b-eng.htm), [fruit and vegetable consumption, 5 times or more per day](http://www.statcan.gc.ca/tables-tableaux/sum-som/l01/cst01/health90b-eng.htm), [physical activity](http://www.statcan.gc.ca/tables-tableaux/sum-som/l01/cst01/health78b-eng.htm)(defined as having any physical activity during leisure time), and [perceived life stress, as quite a lot](http://www.statcan.gc.ca/tables-tableaux/sum-som/l01/cst01/health107b-eng.htm). 

In the end, I combined all the data into one .csv for the percentage the population with these factors from 2014(with the exception of heaving drinking as the only available data was from 2012). If a value was missing, I would use the value from the previous year. 

```{r}
options(stringsAsFactors = FALSE)
riskf <- read.csv("http://claudiasikorski.github.io/datasets/cvd/cad-project.csv")

``` 

Next, I used subset to extract all the risk factors for males and females. 

```{r}
subsetM <- subset(riskf, riskf$Province == "Males")
subsetF <- subset(riskf, riskf$Province == "Females")

#The provice name is every third row since it is province, males, females
provinces = riskf$Province[c(TRUE,rep(FALSE,2) )] 
```

Now let's make dataframes such that we can easily access the data for specific risk factors. I created a function that would do this in order to avoid the same manual process for each factor (yay for functions in R!)

```{r}
df.maker <- function(...) {
  df <- data.frame(cbind(...))
  colnames(df)[1] <- "Female"
  colnames(df)[2] <- "Male"
  rownames(df) <- provinces
  # create list of names
  gender=c("Female","Male")
  # combine them into a data frame
  df =cbind(df,provinces)
    return(melt(df))
}

smoking <- df.maker(subsetF$Smoking, subsetM$Smoking)
second_smoking <- df.maker(subsetF$Second.hand.smoke.exposure, subsetM$Second.hand.smoke.exposure)
bmi <- df.maker(subsetF$BMI.overweight.or.obese, subsetM$BMI.overweight.or.obese)
diabetes <- df.maker(subsetF$Diabetes, subsetM$Diabetes)
blood_pressure <- df.maker(subsetF$High.Blood.Pressure, subsetM$High.Blood.Pressure)
heavy_drinking <- df.maker(subsetF$Heavy.Drinking, subsetM$Heavy.Drinking)
fruit_veg <- df.maker(subsetF$Fruit.and.Vegetable.Consumption, subsetM$Fruit.and.Vegetable.Consumption)
active <- df.maker(subsetF$Physically.Active, subsetM$Physically.Active)
stress <- df.maker(subsetF$Perceived.Life.Stress, subsetM$Perceived.Life.Stress)

```
Now it's time to plot the data, if I wanted to only plot the data for smoking, I would do the following:

```{r}
plot1 <- ggplot(smoking, aes(provinces, value )) +  geom_bar(aes(fill = variable), position = "dodge", stat="identity") + scale_fill_discrete(name="Gender") + xlab("Provinces and Territories") + ylab("Percentage(%)") + ggtitle("Smoking") + theme_bw() + theme(panel.border = element_blank()) + theme(plot.title = element_text(size=13),  axis.title.x = element_text(size=11), axis.title.y = element_text(size=11)) + geom_hline(yintercept= 18.1) + theme(legend.position = (c(1.5,0.8)), legend.direction = "horizontal"


```

Looking at how to plot one graph, it is clear that I can write a generic function for easy plotting. I defined my arguments as risks(this is the varible I will replace with correct risk factor), riskf(where the data is coming from), t for titles, and mean(from Stats Canada links)

```{r , fig.width=16, fig.height=16}
#listing titles for plot function to use
title_list <- c("Smoking in Canada 2014", "Exposure to Second Hand Smoke in Canada 2014","BMI(overweight or obese) in Canada 2014", 
                "Diabetes in Canada 2014", "High Blood Pressure in Canada 2014", "Heavy Drinking in Canada 2012", 
                "Fruit and Vegetable Consumption in Canada 2014", "Active Lifestyle in Canada 2014", "Percieved Stress in Canada 2014")
#listing means for plot function to use
means_list <- c(18.1, 3.9, 54, 6.7, 17.7, 17.4, 39.5, 53.7, 23.0)

)

plot_function <- function(risks, riskf, t, mean){
  
      plot <- ggplot(risks, aes(provinces, value )) +  geom_bar(aes(fill = variable), position = "dodge", stat="identity") + xlab("Provinces and Territories") + ylab("Percentage(%)")  + labs(title = t) + theme_bw() + theme(legend.position="none") + theme(panel.border = element_blank()) + theme(plot.title = element_text(size=13),
  axis.title.x = element_text(size=11), axis.title.y = element_text(size=11)) + geom_hline(yintercept= mean)
return(plot)
}

````
Now that I have this function, plotting is easy! Notice that the means are represnted by a horizontal line (used geom_hline(yintercept= mean to do this)

```{r}

p1 <- plot_function(smoking, riskf, title_list[1], means_list[1])
p2 <- plot_function(second_smoking, riskf, title_list[2], means_list[2])
p3 <- plot_function(bmi, riskf, title_list[3], means_list[3])
p4 <- plot_function(diabetes, riskf, title_list[4], means_list[4])
p5 <- plot_function(blood_pressure, riskf, title_list[5], means_list[5])
p6 <- plot_function(heavy_drinking, riskf, title_list[6], means_list[6])
p7 <- plot_function(fruit_veg, riskf, title_list[7], means_list[7])
p8 <- plot_function(active, riskf, title_list[8], means_list[8])
p9 <- plot_function(stress, riskf, title_list[9], means_list[9])

```

I did not include a legend in the plot function, completed by the adding + theme(legend.position="none") as I want a common legend for the plot. In order to do this, I will extract the legend from plot1 (the example done for smoking above). I learnt how to do this by reading the [following](https://github.com/hadley/ggplot2/wiki/Share-a-legend-between-two-ggplot2-graphs)

```{r}
#extract legend
g_legend<-function(a.gplot){
  tmp <- ggplot_gtable(ggplot_build(a.gplot))
  leg <- which(sapply(tmp$grobs, function(x) x$name) == "guide-box")
  legend <- tmp$grobs[[leg]]
  return(legend)}

legend <- g_legend(plot1)

```

The last step is to arrange the nine plots together, I decided to display them as a 3x3 figure. 

```{r}

p10 <- grid.arrange(p1, p2, p3, p4, p5, p6, p7, p8, p9, ncol=3, legend)

```
# Next steps

Now that I created this figure it's easy for me to look at the difference between men and women by province for the risk factors for CAD. In my next blog post, I will discuss these results!