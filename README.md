install.packages("markovchain", dependencies = FALSE)  #installing the necessary packages for running the code
install.packages("Rcpp", dependencies = FALSE) #installing the necessary packages for running the code
install.packages("RcppParallel", dependencies = FALSE) #installing the necessary packages for running the code
install.packages("matlab", dependencies = FALSE) #installing the necessary packages for running the code
install.packages("expm", dependencies = FALSE) #installing the necessary packages for running the code
library(markovchain)
library(igraph)

library(haven)
markovchain_dataset <- read.dta("c:/nreg24.dta") #importing a stata dataset to R
###The dataset contains the transitions between CST categories for the individuals that did not regress until the 24 months of follow up

nhmcFit <- markovchainListFit(nreg24[,2:8])
nhmcFit$estimate[[1]] ###Transition matrix

singleMc<-markovchainFit(data=nreg24[,2:8]) 
singleMc[["estimate"]]
mclistFit<-markovchainListFit(data=nreg24[,2:8],name="myMcList")

CSTstates<-c("1","3","4")
CSTTransition <- matrix(c(1, 0, 0, 0.2857143, 0.4285714, 0.2857143, 0, 0.40000000, 0.60),byrow=TRUE, nrow=3, dimnames=list(CSTstates, CSTstates))
CSTTransition ## Transition Probabilities
mcCST <- new("markovchain", states = CSTstates, byrow = T, transitionMatrix = CSTTransition, name = "CST")
mcCST
plot(mcCST)
