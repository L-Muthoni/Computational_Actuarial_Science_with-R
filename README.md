# Computational_Actuarial_Science_with-R

## Answers to 7.8.3 and 7.8.4 of Computational Actuarial Science with R
## Below are the answers to Chapter 7, Life Insurance, of the book Computational Actuarial Science with R under the Pricing and Reserving subtopics. 

r=getOption("repos")

r["CRAN"]= "http://cas.uqam.ca/pub/"

options(repos=r)

install.packages("CASdatasets",  type="source")

library(CASdatasets)

?CASdatasets

load("~/.RData")

install.packages("lifecontingencies")

install.packages("LifeTables")

library(lifecontingencies)

library(LifeTables)

library(actuaryr)

data(soa08Act)

data("demoIta")

### Creating the table 

SIM92=new("actuarialtable",x=demoIta$X[0:110],lx=demoIta$SIM92[0:110],interest=0.03)

### How much should a 25-year-old policyholder pay to receive $100,000 when he turns 65, assuming he is alive?
  
Exn(SIM92,x=25,n=65-25)

#### [1] 0.2490894

### How much should a 25-year-old policyholder pay to receive $100,000 when he turns 65 or when he dies?

AExn(SIM92,x=25,n=65-25)

#### [1] 0.3309626


Exn(SIM92,x=25,n=65-25)+Axn(SIM92,x=25,n=65-25)

#### [1] 0.3309626

### On the SOA actuarial table, calculate the APV whole life insurance for a policyholder aged 30 with benefit payable at the end of month of death at a 4% interest rate,

Axn(soa08Act, x=30,i=0.04,k=12)

#### [1] 0.2004297

### For the APV calculated in Exercise 7.16, determine the level benefit premium assuming it will be paid until the policyholder's death

Axn(soa08Act, x=30,i=0.04,k=12)/axn(soa08Act, x=30,k=0.04)

#### [1] 0.006495519

### Calculate the quarterly premium that a policyholder aged 50 will pay until the year of death to insure a face value of $100,000. The face value will be paid at the end of the quarter of death.

P=100000*Axn(soa08Act,50,k=4)/axn(soa08Act,x=50,k=4)

### Compute the 15-year benefit premium for 35-year term insurance with benefit payable at the end of month of death. Assume the interest rate is 3% and the premium is to be paid at the beginning of each month.

APV=100000*Axn(soa08Act, x=30,n=35,i=0.03, k=12) #Get the APV

Pa=APV/axn(soa08Act, x=30,n=15,i=0.03,k=12) #annualized benefit premium (payable monthly)

Pm=Pa/12 #montly benefit premium

Pm

#### [1] 73.10392

### Calculate the reserve at time 4 for life insurance on a policyholder aged 60 paid with five yearly premiums as long as the policyholder is alive from year.

P=Axn(soa08Act, 60)/axn(soa08Act, 60,5)

V4=Axn(soa08Act, 60+4) - P*axn(soa08Act, 60+4,5-4)

V4

#### [1] 0.3401786

### For a 20 year term life insurance on a (75) year policiholder calculate V(5)

U=Axn(soa08Act, 75,20)

P=U/axn(soa08Act, 75,20)

V=Axn(soa08Act, x=75+5,n=20-5)-P*axn(soa08Act, 75+5,20-5)

V

#### [1] 0.1708723

### Calculate the benefit reserve at time 10 for whole life insurance on 60 payable at the end of year of death with benefit premiums paid quarterly.

U=Axn(soa08Act,x=60)

P=U/axn(soa08Act, x=60,k=4)

V=Axn(soa08Act,x=60+10)-P*axn(soa08Act, x=60+10,k=4)

V

#### [1] 0.2341824

### Calculate the benefit reserve for a 20-year endowment on a policyholder aged 60 at time 10 assuming level benefit premiums to be paid for 15 years

U=AExn(soa08Act, 60,20)

P=U/axn(soa08Act, 60,15)

V=AExn(soa08Act, 60+10,20-10)-P*axn(soa08Act, 60+10,15-10)

V

#### [1] 0.4346281
