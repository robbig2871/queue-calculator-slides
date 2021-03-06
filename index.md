---
title       : Comparing the Response Time of Three Simple Queueing Systems
subtitle    : A shiny queue calculator
author      : Tim Wise
job         : 
logo        : MM1_logo.png
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
url:
  lib: ./libraries
  assets: ./assets
widgets     : [mathjax, quiz, bootstrap]   # {mathjax, quiz, bootstrap}
mode        : standalone                # {selfcontained for github, standalone for rpubs, draft}
knit        : slidfy::knit2slides        
---&twocol

## A Quick Introduction to Queueing Theory

[Queueing theory](http://en.wikipedia.org/wiki/Queueing_theory) is the mathematical 
study of *customers* waiting in lines for *services*. 
A queueing system can be as simple as a single server,
like waiting to get cash from an ATM machine.
Or it can be a complex network of servers, 
like components flowing through a factory assembly line.

*** =left
The simplest queue is a single server with a waiting line and called
an [M/M/1 queue](http://en.wikipedia.org/wiki/M/M/1_queue). Customers
arrive in the queue at rate $\lambda$ and
the server handles them at rate $\mu$.

A generalization is an [M/M/c queue](http://en.wikipedia.org/wiki/M/M/c_queue)
that has a single line feeding
C servers. The next person in line is served by the next available 
server. It's like a load balancer.

We're interested in comparing the response times of the two queueing systems
for different values of $C$.

*** =right

<img class=center src=./assets/img/MM1-MMc.png>


---&twocol
## Metrics of M/M/1 Queue and an Example

*** =left

Typical configuration metrics of an M/M/1 queue are: 
- $\lambda$, arrival rate of customers
- $S$, service time for a single customer

from those we can compute:
- max server rate, or capacity, $\mu = 1 / S$ 
- server utilization,  $\rho = \lambda / \mu$
- customer response time, $R = 1 / (\mu - \lambda)$

A queueing system is in steady-state when the arrival rate is less than
the capacity of the server. For an M/M/1 queue, when 
$\lambda < \mu \equiv \rho < 1$.

*** =right


```r
lambda <- 50    # tx per sec
S <- 0.010      # sec per tx
mu <- 1 / S; mu # max tx per sec
```

```
## [1] 100
```

```r
# server utilization 
rho <- lambda / mu; percent(rho) 
```

```
## Error in eval(expr, envir, enclos): could not find function "percent"
```

```r
# tx response time sec
R <- 1 / (mu - lambda); R        
```

```
## [1] 0.02
```

---
## Consider Three Equivalent Queueing Systems
From [Developing Our Intuition About Queuing Network Models](http://www.cmg.org/publications/conference-proceedings/conference-proceedings2014/):

<img class=center src=./assets/img/3qnetworks.png>

Grocery Store | Bank Teller | Super Server
------------- | ----------- | ------------
A set of six parallel M/M/1 queues  | An M/M/c queue with 6 servers       | An M/M/1 queue that is $6x$ faster
each with an arrival rate $\lambda$ | with an arrival rate of $6\lambda$  | with an arrival rate of $6\lambda$ 
and each server works at rate $\mu$ | and each server works at rate $\mu$ | and the server works at rate $6\mu$

---&radio

## Which Queue Provides the Fastest Response Time?

### Question 1

We've seen that response time of a queue is a function of the
customer arrival rate and the service time. Which of the 3 queueing systems do 
you think provides the fastest response time under light load, 
i.e., slow customer arrival rate?

1. Grocery Store
2. Bank Teller
3. _Super Server_


*** .hint

Hint: Under light load there is no waiting time. Response time is just the
service time. Which system has the fastest server?

*** .explanation 

The Super Server is 6 times as fast as the other servers. 
So its service time $1/6$-th that of the other systems.
Under low customer arrival rate, there is no wait time and the response time
is just the service time. So the Super Server has the fastest response time
under low load.

*** post-question-text

### Question 2

Now, which system do you think provides better response time under very 
high load? To find out, go use our simple shiny app
[queue calculator](http://timwise.shinyapps.io/queue-calculator/).

