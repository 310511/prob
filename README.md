## Table of Contents

| # | File | Topic |
|---|------|-------|
| 01 | `01_basic_functions_data.R` | Basic R functions and data input |
| 02 | `02_summary_statistics_tables_graphs.R` | Summary statistics, tables, and graphs |
| 03 | `03_correlation_simple_regression.R` | Correlation and simple linear regression |
| 04 | `04_multiple_regression.R` | Multiple linear regression |
| 05 | `05_binomial_distribution.R` | Binomial distribution |
| 06 | `06_poisson_and_normal_distributions.R` | Poisson and normal distributions |
| 07 | `07_large_sample_z_tests.R` | Large-sample Z-tests (mean & proportion) |
| 08 | `08_two_sample_proportion_tests.R` | Two-sample mean and proportion Z-tests |
| 09 | `09_t_test_and_f_test.R` | t-test (independent, paired) and F-test |
| 10 | `10_chi_square_tests.R` | Chi-square goodness-of-fit and independence |
| 11 | `11_anova_designs.R` | One-way ANOVA, two-way ANOVA, RBD, Latin square |



```r
# Experiment 1: basic R functions and data input

1:10
x <- c(1, 2, 5)
X <- 10

a <- c(1, 3, 5, 7)
b <- c(2, 4, 6, 8)
a + b
a - b
5 * a
a * b
a / b

as.character(5.2)
paste("Baa", "Baa", "Black", "Sheep")

data_csv <- read.csv(file.choose())
```

---

## Experiment 02 — Summary Statistics, Tables, and Graphs

**File:** `02_summary_statistics_tables_graphs.R`

```r
# Experiment 2: summary statistics, tables, and graphs

empid <- 1:15
age <- c(30,37,45,32,50,60,35,32,34,43,32,30,43,50,60)
gender <- c(0,1,0,1,1,1,0,0,1,0,0,1,1,0,0)
status <- c(1,1,2,2,1,1,1,2,2,1,2,1,2,1,2)

empinfo <- data.frame(empid, age, gender, status)
empinfo$gender <- factor(empinfo$gender, labels = c("male", "female"))
empinfo$status <- factor(empinfo$status, labels = c("staff", "faculty"))

male <- subset(empinfo, gender == "male")
female <- subset(empinfo, gender == "female")

summary(empinfo)
summary(male)
summary(female)
summary(age)
mean(age)
median(age)
var(age)
sd(age)
range(age)

table(empinfo$gender)
table(empinfo$status)
table(empinfo$gender, empinfo$status)

plot(empinfo$age, type = "l", main = "Age of employees",
     xlab = "Employee ID", ylab = "Age in years")
pie(table(empinfo$gender))
barplot(table(empinfo$gender, empinfo$status), beside = TRUE)
boxplot(age ~ status, data = empinfo, col = c("red", "blue"))
```



```r
# Experiment 3: correlation and simple linear regression

data <- cars
summary(data)
var(data$speed)
var(data$dist)
cov(data$speed, data$dist)

cor(data$speed, data$dist)
cor.test(data$speed, data$dist, method = "pearson")
cor.test(data$speed, data$dist, method = "spearman")

plot(data$speed, data$dist, xlab = "Speed", ylab = "Stopping distance")

regression1 <- lm(speed ~ dist, data = data)
summary(regression1)
abline(regression1)

regression2 <- lm(dist ~ speed, data = data)
summary(regression2)

weight <- c(15,26,27,25,25.5,27,32,18,22,20,26,24)
bmi <- c(13.35,16.12,16.74,16,13.59,15.73,15.65,13.85,16.07,12.8,13.65,14.42)
cor(weight, bmi)
model1 <- lm(bmi ~ weight)
summary(model1)
```



```r
# Experiment 4: multiple linear regression

Y <- c(110,80,70,120,150,90,70,120)
X1 <- c(30,40,20,50,60,40,20,60)
X2 <- c(11,10,7,15,19,12,8,14)

RegModel <- lm(Y ~ X1 + X2)
summary(RegModel)
library(scatterplot3D)
graph = scatterplot3D(Y,X2,X1)
graph

graph&plane3D(RegModel)

```



```r
# Experiment 5: Binomial distribution

n <- 4
p <- 0.5
x <- 0:n

dbinom(2, size = n, prob = p)
sum(dbinom(2:n, size = n, prob = p))
sum(dbinom(0:2, size = n, prob = p))

px <- dbinom(x, size = n, prob = p)
data.frame(x, probability = px)

expectation <- n * p
variance <- n * p * (1 - p)
expectation
variance

plot(x, px, type = "h", xlab = "Values of X",
     ylab = "Probability", main = "Binomial distribution")
```



```r
# Experiment 6: Poisson distribution

lambda <- 2
x <- 0:10

dpois(2, lambda = lambda)
sum(dpois(2:10, lambda = lambda))
sum(dpois(0:2, lambda = lambda))

px <- dpois(x, lambda = lambda)
data.frame(x, probability = px)
plot(x, px, type = "h", xlab = "Values of X",
     ylab = "Probability", main = "Poisson distribution")

# Experiment 6: Normal distribution

x <- seq(0, 40)
y <- dnorm(x, mean = 20, sd = 5)
plot(x, y, type = "l", xlab = "Time", ylab = "Density",
     main = "Normal distribution")

pnorm(15, mean = 20, sd = 5)
1 - pnorm(25, mean = 20, sd = 5)
pnorm(25, mean = 20, sd = 5) - pnorm(15, mean = 20, sd = 5)
```



```r
# Experiment 7: one-sample mean Z-test

xbar <- 14.6
mu0 <- 15.4
sigma <- 2.5
n <- 35
alpha <- 0.05

z <- (xbar - mu0) / (sigma / sqrt(n))
critical <- qnorm(1 - alpha / 2)
p_value <- 2 * pnorm(-abs(z))

z
c(-critical, critical)
p_value
if (p_value < alpha) "Reject H0" else "Do not reject H0"

# Experiment 7: one-sample proportion Z-test

n <- 640
deaths <- 63
sample_prop <- deaths / n
pop_prop <- 0.1726
q <- 1 - pop_prop
z_prop <- (sample_prop - pop_prop) / sqrt(pop_prop * q / n)
critical_prop <- qnorm(1 - alpha / 2)
p_prop <- 2 * pnorm(-abs(z_prop))

sample_prop
z_prop
c(-critical_prop, critical_prop)
p_prop
if (p_prop < alpha) "Reject H0" else "Do not reject H0"
```



```r
# Experiment 8: two-sample mean Z-test

xbar <- 20
ybar <- 15
sigma <- 4
n1 <- 500
n2 <- 400
alpha <- 0.05

z <- (xbar - ybar) / (sigma * sqrt(1/n1 + 1/n2))
critical <- qnorm(1 - alpha/2)
p_value <- 2 * pnorm(-abs(z))

z
c(-critical, critical)
p_value
if (p_value < alpha) "Reject H0" else "Do not reject H0"

# Experiment 8: two-sample proportion Z-test

x1 <- 45
n1 <- 100
x2 <- 35
n2 <- 100
p1 <- x1 / n1
p2 <- x2 / n2
p_pool <- (x1 + x2) / (n1 + n2)
z_prop <- (p1 - p2) / sqrt(p_pool * (1 - p_pool) * (1/n1 + 1/n2))
critical_prop <- qnorm(1 - alpha/2)
p_value_prop <- 2 * pnorm(-abs(z_prop))

z_prop
p_value_prop
if (p_value_prop < alpha) "Reject H0" else "Do not reject H0"
```



```r
# Experiment 9: independent two-sample t-test

sample1 <- c(19,17,15,21,16,18,16,14)
sample2 <- c(15,14,15,19,15,18,16,20)
alpha <- 0.05

result_t <- t.test(sample1, sample2)
result_t
if (result_t$p.value < alpha) "Reject H0" else "Do not reject H0"

result_t_eq <- t.test(sample1, sample2, var.equal = TRUE)
result_t_eq
if (result_t_eq$p.value < alpha) "Reject H0" else "Do not reject H0"

# Experiment 9: paired t-test

before <- c(10,12,14,13,15)
after  <- c(12,13,15,15,16)
result_paired <- t.test(before, after, paired = TRUE)
result_paired
if (result_paired$p.value < alpha) "Reject H0" else "Do not reject H0"

# Experiment 9: F-test

result_f <- var.test(sample1, sample2)
result_f
if (result_f$p.value < alpha) "Reject H0" else "Do not reject H0"
```



```r
# Experiment 10: Chi-square goodness-of-fit test

n <- 5
alpha <- 0.05
N <- 256
P <- 0.5
x <- 0:n
observed <- c(5,35,75,84,45,12)
expected <- dbinom(x, size = n, prob = P) * N

sum(observed)
sum(expected)
chi_stat <- sum((observed - expected)^2 / expected)
df <- length(x) - 1
critical <- qchisq(1 - alpha, df = df)
p_value <- pchisq(chi_stat, df = df, lower.tail = FALSE)

chi_stat
critical
p_value
if (p_value < alpha) "Reject H0" else "Do not reject H0"

# Experiment 10: Chi-square test of independence

observed_table <- matrix(c(69,51,81,20,35,44), ncol = 2, byrow = TRUE)
chisq.test(observed_table)
```



```r
# Experiment 11: One-way ANOVA / CRD

A <- c(36,37,42,38,47)
B <- c(46,39,35,37,43)
C <- c(35,42,37,43,38)
D <- c(45,36,39,35,32)
E <- c(41,39,37,35,38)

group <- data.frame(A, B, C, D, E)
long_data <- stack(group)
crd <- aov(values ~ ind, data = long_data)
summary(crd)
boxplot(group, ylab = "Response", main = "Compare groups")

# Experiment 11: Two-way ANOVA
# anova_model <- aov(Sales ~ States + Salesmen, data = data)
# summary(anova_model)

# Experiment 11: RBD
# aov(response ~ treatment + block, data = data)

# Experiment 11: Latin Square Design
# aov(response ~ row_factor + column_factor + treatment_factor, data = data)
```
# Experiment 11: Two-way ANOVA

StateA = c(6, 5, 3, 8)
StateB = c(8, 9, 6, 5)
StateC = c(10, 7, 8, 7)

Group = data.frame(cbind(StateA, StateB, StateC))
Group

Sales = c(t(as.matrix(Group)))
Sales

f = c("State A", "State B", "State C")
g = c("Salesman1", "Salesman2", "Salesman3", "Salesman4")

k = ncol(Group)
n = nrow(Group)

States = gl(k, 1, n*k, factor(f))
Salesmen = gl(n, k, n*k, factor(g))

data = data.frame(Sales, States, Salesmen)
data

anova = aov(Sales ~ States + Salesmen, data = data)
summary(anova)

# Experiment 11: Latin Square Design

manure = c(
  rep("manure1", 5),
  rep("manure2", 5),
  rep("manure3", 5),
  rep("manure4", 5),
  rep("manure5", 5)
)

cultivation = rep(
  c("cultP", "cultQ", "cultR", "cultS", "cultT"),
  each = 5
)

crop = c(
  "P","T","R","Q","S",
  "R","Q","P","S","T",
  "Q","R","S","T","P",
  "S","P","T","R","Q",
  "T","S","Q","P","R"
)

freq = c(
  42,45,41,56,47,
  47,54,46,52,49,
  55,52,57,49,45,
  51,44,47,50,54,
  44,50,48,43,46
)

data = data.frame(cultivation, manure, crop, freq)
data

matrix(data$crop, nrow = 5, ncol = 5)
matrix(data$freq, nrow = 5, ncol = 5)

data$manure = factor(data$manure)
data$cultivation = factor(data$cultivation)
data$crop = factor(data$crop)

fit = lm(freq ~ manure + cultivation + crop, data = data)
anova(fit)