**STAT 212:** Power for the Completely Randomized Design

With multiple comparisons, we talked about Type I error and its
probability. We defined Type I error as rejecting the null hypothesis
when it is, in fact, true. If P(Type I error) = $\alpha$, then P(no Type
I error) = $1-\alpha$.

There is another kind of error -- Type II error. A Type II error occurs
when H$_0$ is not rejected, but H$_0$ is actually false. In earlier
courses, we summarized these two types of errors in a table:

Earlier in STAT 212, as well as other classes, we've said that we can
"set" the probability of a Type I error. Anytime we say we'll reject
H$_0$ if the p-value $< \alpha$, we're setting P(Type I error) =
$\alpha$. What about Type II error? We generally call the probability of
a Type II error $\beta$, P(Type II error) = $\beta$. The problem is we
can't "set" both $\beta$ and $\alpha$ without some other complications.

We are typically interested in the **power** of a test:

Let's explore this via simulation. Consider a two-sample $t$-test. In
Canvas, there is an R file called `simulation example.R`.

-   In the program, we're generating $n_1=20$ normal random variables
    with $\mu_1=10$, $\sigma^2=25$ and $n_2=20$ normal random variables
    with $\mu_2=10$, $\sigma^2=25$.

-   Carry out the t-test (code already included) and observe the
    p-value. Using $\alpha=0.10$, what is your decision?

-   Run the program 9 more times, so you have a total of 10 p-values.
    How many times out of 10 did you reject H$_0$?

    What does this estimate?

-   Edit the program to generate data with $\mu_1=10$ and $\mu_2=12$
    (still with $n_1=n_2=20$ and $\sigma^2=25$). Run the program 10
    times total. How many times did you reject H$_0$, using
    $\alpha=0.10$?

-   Edit the program to generate data with $\mu_1=10$ and $\mu_2=15$
    (still with $n_1=n_2=20$ and $\sigma^2=25$). Run the program 10
    times total. How many times did you reject H$_0$, using
    $\alpha=0.10$?

-   Edit the program to generate data with $\mu_1=10$ and $\mu_2=20$
    (still with $n_1=n_2=20$ and $\sigma^2=25$). Run the program 10
    times total. How many times did you reject H$_0$, using
    $\alpha=0.10$?

What do you observe as $\mu_1$ and $\mu_2$ get further apart?

Now let's try the following:

-   Edit the program to generate data with $\mu_1=10$ and $\mu_2=12$,
    but with $\sigma^2=1$ (still with $n_1=n_2=20$). Run the program 10
    times total. How many times did you reject H$_0$, using
    $\alpha=0.10$?

-   Edit the program to generate data with $\mu_1=10$ and $\mu_2=20$,
    but with $\sigma^2=625$ (still with $n_1=n_2=20$). Run the program
    10 times total. How many times did you reject H$_0$, using
    $\alpha=0.10$?

What do observe as $\sigma^2$ gets larger or smaller?

**Power** is the probability of rejecting H$_0$ when it is really false.
It is a function of several quantities:

-   
-   
-   
-   

Power analyses usually focus on calculating the sample size required to
achieve a particular power. What do you think would happen if instead of
using $n_1=n_2=20$ we used $n_1=n_2=10$?

What do you think would happen if instead of using $n_1=n_2=20$ we used
$n_1=n_2=40$?

What do you think would happen if instead of using $n_1=n_2=20$ we used
$n_1=10$ and $n_2=30$?

In some simple situations, we can SAS procs to do power calculations.
There are two: `PROC POWER` and `PROC GLMPOWER`. In some more
complicated situations we'll need to write our own code. We'll start
with `PROC POWER`. This proc will do power calculations for two sample
$t$ tests and ANOVA.

For a two-sample $t$ test, the basic code is:

        proc power;
          twosamplemeans test=diff
          alpha=
          stddev=
          meandiff=
          npergroup=
          power=               ;
        run;

We'll need to supply values for alpha, stddev, and meandiff. We can
either supply a value for npergroup and use `power=.` or supply a value
for power and use `npergroup=.`

We could also add the lines

-   `plot x=power min=0.5 max=0.95;` (for `ntotal=.`)

-   `x=n min= max= ;` (for `power=.`)

Let's try this, going back to our example with $\mu_1=10$, $\mu_2=12$,
and $\sigma^2=25$.

We can also use `PROC POWER` for ANOVA and contrasts. This time, the
basic code is:

    proc power;
      onewayanova
      alpha=
      stddev=
      groupmeans=  |   |
      ntotal=
      power=
      contrast= (     );
    run;

Let's go back to the donuts data. In that example, the MSE was 100.90,
so we'll use $\sigma=10$ as a guess for future experiments. We observed
sample means of $\overline y_{1\cdot}=172$, $\overline y_{2\cdot}=185$,
$\overline y_{3\cdot}=176$, and $\overline y_{4\cdot}=162$. We can
certainly use these as guesses for future experiments. We'll consider
the contrast testing animal fats versus vegetable fats. We could also
look at the overall test.

We can also add plot statements here.

But, we don't actually have to have guesses for the treatment means. We
do have to have an idea of how large a difference we want to be able to
detect. With our example data, we had a animal fat mean of 178.5 and a
vegetable fat mean of 169. This is a difference of 9.5.

We could also the consider potential differences we might observe in
pairwise differences.

There are situations, however, where `PROC POWER` doesn't work. It can't
handle more than one factor, a treatment design we'll see in a couple of
weeks. And, it can't handle any model with a random effect other than
$e_{ij}$, a consequence of experimental designs we'll see shortly.

Let's go back to the donut scenario. In that case, the ANOVA table was

::: center
  Source of Variation   df                       SS        MS    Expected MS                                       F
  --------------------- ------------------------ --------- ----- ------------------------------------------------- ---------
  Fat Type              $t-1=4-1=3$              SSTrt     MST   $\sigma^2 + \frac{n}{t-1}\sum_{i=1}^4 \tau_i^2$   MST/MSE
  Error                 $t(n-1) = 4(6-1) = 20$   SSError   MSE   $\sigma^2$                                        
  Total                 $nt-1=(4)(6)-1=23$       SSTotal                                                           
:::

When we considered this ANOVA table back in Section 2, we observed

But, power is about detecting when the null hypothesis is NOT true. It
turns out that if H$_0$ is not true, the $F$ ratio follows a different
distribution:

The term SSHyp means

We can determine the power of an overall $F$ test or contrast under a
specified alternative by tricking `PROC GLIMMIX` into computing
$\lambda$ and then using it to compute power. There are three basic
steps.

1.  Generate data set where all $y_{ij}$ are set to the alternative
    value $\mu_i$ (the "true" $\mu_i$ MUST be specified by the
    researcher). A "true" value of $\sigma^2$ must also be specified.

2.  Run `PROC GLIMMIX` on these data, but fix $\sigma^2$ at the value
    specified by the researcher (don't let SAS estimate it). Use
    `GLIMMIX` to calculate the overall $F$ and $F$ values for any
    contrasts. This means the `contrast` statement must be used, not
    `estimate` or `lsmestimate`. The product of the numerator degrees of
    freedom and the resulting overall ANOVA and contrast $F$ values are
    the $\lambda$s.

3.  Use the results from (2) in the SAS $F$ probability functions to
    compute power.

Let's go back to the donuts.

1.  Step 1: Generate or simulate the data of "true" means.

        data donutpower;
          input trt mu;
          do eu=1 to 6; *6 batches per treatment;
            output;
          end;
        datalines;
        1 172
        2 185
        3 176
        4 162
        ;   

2.  Step 2: Run `GLIMMIX`, fixing the true value of $\sigma^2$ at 100.

            proc glimmix data=donutpower;
              class trt;
              model mu=trt;
              parms (100)/hold=1;
              contrast 'animal vs veg' trt 0.5 0.5 -0.5 -0.5;
            ods output tests3=b;
            ods output contrasts=c;
            run;
            
            proc print data=b;
            proc print data=c; run;

    This produces the output

    ![image](glimmix out.png){width="70%"}

3.  Step 3: Use the results from `GLIMMIX` to find the power.

            data powerval;
              set b c;
              do alpha=0.10, 0.05, 0.01;
                lambda=numdf*fvalue;
                fcrit=finv(1-alpha, numdf, dendf, 0);
                power=1-probf(fcrit, numdf, dendf,lambda);
                output;
              end;
            proc print; run;

    Which gives

    ![image](power out.jpg){width="90%"}
