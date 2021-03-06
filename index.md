---
title       : My Shiny App
subtitle    : The Gaussian plot
author      : Asier
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## The idea
To plot a Gaussian function with the desired mean and sd values, together with two lines plotting the position of the first sd (not shown). 

```r
x   <- seq(-10,10,length=2000)
y   <- dnorm(x,mean=0, sd=2)                
plot(x,y, type="l", lwd=1, col='lightblue',main='Gaussian Function')                      
```

![plot of chunk unnamed-chunk-1](assets/fig/unnamed-chunk-1-1.png) 

--- .class #id 

## The Code:server.R


```r
shinyServer(
        function(input, output) {
                output$text1 <- renderText({input$text1})
                output$text2 <- renderText({input$sdin})
                output$newgauss <- renderPlot({
                        x   <- seq(-10,10,length=2000)
                        meanin<-input$text1
                        sdin<-input$sdin
                        meanin<-as.numeric(meanin)
                        sdin<-as.numeric(sdin)
                        y   <- dnorm(x,mean=meanin, sd=sdin)
                        plot(x,y, type="l", lwd=1, col='lightblue',main='Gaussian Function')
                        
                        lines(c(meanin+sdin, meanin+sdin), c(0, 200),col="red",lwd=5)
                        lines(c(meanin-sdin, meanin-sdin), c(0, 200),col="red",lwd=5)                                        
                })
                
        }
)
```

--- .class #id 

## The Code:ui.R


```r
        headerPanel("Gaussian plot"),
        sidebarPanel(
                textInput(inputId="text1", label = "Value of the mean"),
                sliderInput('sdin', 'Value of the standard deviation',value = 1, min = 0.01, max = 5, step = 0.05,)
        ),
        mainPanel(
                p('Value of the mean (between 10 and -10)'),
                textOutput('text1'),
                p('Value of the standard deviation'),
                textOutput('text2'),
                plotOutput('newgauss')
        )
))
```

-The two inputs correspond to a slider and a text input. The output returns the inserted values and the plot.

--- .class #id 

## Conclusion

-No explanation was provided in previous slides due to lack of space.

-The App requires two valid numeric values to plot the figure. 

-It plots a Gaussian curve with the desired value of mean (from -10 to 10) and sd. Together with two lines indicating the position of the first standard deviation. 

-The app allows to understand how the curve changes as we vary the different parameters.

-Enjoy and have a great day! ;)


--- .class #id 


