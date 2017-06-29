if interaction between treatment and mediator is absent then controlled direct effect and natural direct effect are identical




CDE

how much the outcome would change if the mediator was fixed at m and treatment moved from 0 to 1

NDE

how much the outcome would change when treatment moves from 0 to 1 if the mediator took whatever form when when treatment = 0

NIE

how much the outcome would change if treatment set at 0 but mediator changes from level it would have had for treatment = 0 to treatment = 1


CDE - interesting for policy where the mediator is a target for intervention
NDE/NIE - interesting from a pathway decomposition perspective

TE = NDE + NIE



n <- 1000
bxa <- 0.4
byx <- 0.4
varx <- 1
a <- rnorm(n)
x <- a * bxa + rnorm(n, sd=sqrt(varx))
y <- byx * x
vare <- seq(0, 4, length.out=100)
res <- matrix(0, length(vare), 2)
for(i in 1:length(vare))
{
	xm <- x + rnorm(n, sd=sqrt(vare[i]))
	res[i,1] <- coefficients(lm(xm ~ a))[2]
	res[i,2] <- coefficients(lm(y ~ xm))[2]
}

pdf("~/Desktop/me1.pf")
plot(res[,1] ~ vare, xlab="Variance of error", ylab="Effect of exposure on mediator")
dev.off()

pdf("~/Desktop/me2.pf")
plot(res[,2] ~ vare, xlab="Variance of error", ylab="Effect of mediator on outcome")
dev.off()

