**STAT 212:** Randomized Complete Block Designs

At this point in the course, we make a distinct change of focus. Up to
now, we've concentrated on analyzing data coming from various
**treatment** designs. We've considered multiple flavors of one-way
designs (unstructured, control vs others, regression, other structure)
and factorials (2 factors, 3 factors, could have more than 3). In all
cases though, we've been using the same **experimental** design: the
completely randomized design (CRD). Now, we change our focus to other
experimental designs. The treatment designs will be those we've seen
before, and we'll continue to analyze treatment effects using methods
we've already discussed.

With a shift to experimental designs, we'll be considering

This also means

We're going to start with the simplest experimental design (besides the
CRD): the **randomized complete block design** (RCBD).

Consider an experiment in which we are interested in comparing six
different lab activities for teaching the central limit theorem. Based
on a power analysis, we believe four replications per treatment is
sufficient, and so we need a total of 24 lab teams. We've got two
options:

-   
-   

Which do you pick? Why? What are pros and cons of each?

Suppose you decide to use teams in different classes (or you don't have
a choice). How will you assign treatments to teams?

Suppose we allocate treatments to teams completely at random (CRD), and
by chance four out of six teams in one class are assigned to
treatment 1. Would you be okay with this?

One of the main problems with the CRD is a possible 'conditional' bias.
That is, treatment assignment is not balanced relative to any systematic
variation/gradient. In this class, the gradient is

When this happens, treatment effect is confounded with gradient. Is any
effect we observe really due to treatment, or is it due to the effect of
the class? The other problem with the CRD is variance inflation. Suppose
there is a gradient among the experimental units, with response
increasing as you go up a gradient:

Even if all the same treatment is applied throughout, the variance among
the experimental units (the residuals) will be composed of two
quantities:

This means it will appear larger than it actually is. The most common
solution to these problems of confounding and variance inflation is
**blocking**.

**Idea of Blocking:**

Blocking allows us to reconcile two somewhat opposing aims of
experimental design.

-   
-   

In summary, the general idea of blocking is to organize experimental
units into groups that are as uniform as possible. We want to

Blocks usually represent naturally occurring differences not related to
treatments. If we block 'correctly' then the RCBD accounts for block
variation, and allows us to pull it out and isolate the usual random
error due to experimental units. If we block 'incorrectly' then we get a
weaker experiment.

**How Do We Block?**

There are two basic steps in blocking an experiment:

1.  Organize the experimental units into subsets (blocks) according to
    gradient

2.  Restrict the randomization so that each treatment is assigned to one
    or fewer (zero) experimental units in each block.

The simplest block design is the **randomized complete block design**
(RCBD). In this design

In the RCBD, we carry out the two steps referenced on the previous page
as

1.  Divide the experimental units into blocks of homogeneous units.

2.  Randomly assign treatments to units within blocks, using a separate
    randomization for each block. Every treatment will appear in every
    block.

Again, we carry out step 1 with the goal

The model for the RCBD helps point out some considerations for choosing
blocks. The model (assuming a one-way treatment design) is:

The ANOVA table looks like

**Example:** An experiment was carried out to evaluate the effect of
elevated CO$_2$ on rice grain yield. Four blocks of 2 rice paddies each
(each block owned by a different farmer, who used different fertilizer
regimes and management practices over the years) are available for the
experiment. In each paddy there is a 12 m diameter circular plot. In one
plot in each block there is a ring of tubing around the plot emitting
CO$_2$ at a rate of 300 ppm above ambient level. In the other plot, no
CO$_2$ is emitted. The grain yield is measured at 3 locations in each
plot at the end of the season.

What is the experimental unit here?

What does the assumption of no block $\times$ treatment interaction mean
in this example?

We can check it with an interaction plot. Here are the means for each
plot

::: center
   Block   Ambient CO$_2$   Elevated CO$_2$
  ------- ---------------- -----------------
     1          6.21             6.41
     2          6.25             6.42
     3          6.10             6.26
     4          6.14             6.30
:::

![image](block.png){width="70%"}

If you had been presented with this data in STAT 102 (or STAT 318), how
would you have analyzed it?

::: center
   Block   Ambient CO$_2$   Elevated CO$_2$  
  ------- ---------------- ----------------- --
     1          6.21             6.41        
     2          6.25             6.42        
     3          6.10             6.26        
     4          6.14             6.30        
:::

Remember that the RCBD is an **experimental** design, not a treatment
design. It can be used with any treatment design. So, we might see RCBD
layouts that look like:

::: center
   Block 1   Block 2   Block 3
  --------- --------- ---------
   Control    Trt2      Trt4
    Trt2     Control    Trt3
    Trt3      Trt4      Trt2
    Trt4      Trt3     Control
:::

::: center
   Block 1   Block 2   Block 3
  --------- --------- ---------
     20        60        80
     60        40        20
     80        80        40
     40        20        60
:::

::: center
   Block 1   Block 2   Block 3
  --------- --------- ---------
   A1 & B1   A1 & B1   A2 & B2
   A2 & B1   A2 & B1   A1 & B1
   A2 & B2   A1 & B2   A1 & B2
   A1 & B2   A2 & B2   A2 & B2
:::

When you write a report, both the treatment design and the experimental
design need to be described in the methods section.

**Tips for Choosing Blocks:**

-   We want to maximize differences between blocks and minimize
    differences within blocks

-   Block size should not be excessively large

-   Keep in the mind the no block $\times$ treatment interaction
    assumption

**Common Criteria for Blocking:**

-   gradients that occur in the field, in greenhouses, in growth
    chambers

-   weight groups in animal experimentation, litters, cage positions in
    a room

-   occasion (day, month, year)

-   location (barn, different fields, different rooms, different states)

-   subjects (each subject serves as their own control)

**Model and Analysis**

Let $y_{ij}$ be

Earlier we stated the model

We do have another choice to make. We can consider the block effect to
either be a **fixed effect** or a **random effect**.

Our choice will have implications in the ANOVA table (in the Expected
Mean Squares) and in the standard errors of the cell means.

To estimate the difference between two treatment means
$(\mu_{i\cdot} - \mu_{i'\cdot})$, we use
$\overline y_{i\cdot} - \overline y_{i'\cdot}$. To figure out the
variance (or estimate of the variance), let's look at what
$\overline y_{i\cdot}$ is actually estimating:

Now let's explore the variance of this quantity.

Now let's consider $\overline y_{i\cdot} - \overline y_{i'\cdot}$:

and its variance

If we want to construct confidence intervals for treatment means or
differences, they'll have the form

where the standard error is

For example, we use MSE as our estimate of $\sigma^2$, so confidence
intervals for the difference between two means is

**RCBDs in SAS**

We can still use `PROC GLIMMIX` to fit the model if our experimental
design is the RCBD. The basic program for **fixed blocks** is

        proc glimmix data=dataset;
          class block trt;
          model y = block trt;
        run;

Note:

The basic program for **random blocks** is

        proc glimmix data=dataset;
          class block trt;
          model y = trt;
          random block;
        run;

Note:

**Example:** This experiment is looking at the emergence rate of soybean
seeds treated with four different chemical treatments and a control.

::: center
   Treatment Number  Treatment Name
  ------------------ ----------------
          1          Control
          2          Arasan
          3          Spergon
          4          Semesan
          5          Fermate
:::

**Experimental Layout:** The field is located on a slope, and blocks are
formed based on elevation. There are five plots at each elevation, and
five blocks.

**Treatment Design:**

100 seeds were planted in each plot, and the response is the number of
plants that emerge out of the 100.

**Model:**

**Analysis with Blocks Fixed:**

If we assume blocks are fixed, we use the code

    proc glimmix data=seeds;
      class block chem;
      model emerge=block chem;
    run;

which gives

                     Fit Statistics
        
        Pearson Chi-Square / DF         5.41
        
             Type III Tests of Fixed Effects
                      Num      Den
        Effect         DF       DF    F Value    Pr > F
        
        block           4       16       2.30    0.1032
        chem            4       16       3.87    0.0219
        

The follow-up analyses don't change from what we've done so far. In this
case, the treatment design is one-way treatment-versus-control, so
comparing all treatments to the control is appropriate and we can use
the Dunnett adjustment.

        lsmeans chem/diff=control('Control') adjust=dunnett;

                            Chem Least Squares Means
        
                               Standard
        Chem       Estimate       Error       DF    t Value    Pr > |t|
        
        Arasan      93.8000      1.0402       16      90.18      <.0001
        Control     89.2000      1.0402       16      85.75      <.0001
        Fermate     94.2000      1.0402       16      90.56      <.0001
        Semesan     93.4000      1.0402       16      89.79      <.0001
        Spergon     91.8000      1.0402       16      88.25      <.0001
        
        
        
        
                     Differences of Chem Least Squares Means
                  Adjustment for Multiple Comparisons: Dunnett
        
                                          Standard
        Chem       _Chem      Estimate       Error       DF    t Value    Pr > |t|     Adj P
        
        Arasan     Control      4.6000      1.4711       16       3.13      0.0065    0.0218
        Fermate    Control      5.0000      1.4711       16       3.40      0.0037    0.0125
        Semesan    Control      4.2000      1.4711       16       2.86      0.0115    0.0375
        Spergon    Control      2.6000      1.4711       16       1.77      0.0962    0.2680
        

**Analysis with Blocks Random:**

If we assume blocks are random, we use the code

        proc glimmix data=seeds;
          class block chem;
          model emerge=chem;
          random block;
          lsmeans chem/diff=control('Control') adjust=dunnett;
        run;

which gives

                  Fit Statistics
        
        Gener. Chi-Square / DF          5.41
        
          Covariance Parameter Estimates
                                Standard
        Cov Parm    Estimate       Error
        block         1.4100      1.8032
        Residual      5.4100      1.9127
        
               Type III Tests of Fixed Effects
                      Num      Den
        Effect         DF       DF    F Value    Pr > F
        Chem            4       16       3.87    0.0219
        
                        Chem Least Squares Means
                               Standard
        Chem       Estimate       Error       DF    t Value    Pr > |t| 
        Arasan      93.8000      1.1679       16      80.31      <.0001
        Control     89.2000      1.1679       16      76.38      <.0001
        Fermate     94.2000      1.1679       16      80.66      <.0001
        Semesan     93.4000      1.1679       16      79.97      <.0001
        Spergon     91.8000      1.1679       16      78.60      <.0001
        
                         Differences of Chem Least Squares Means
                    Adjustment for Multiple Comparisons: Dunnett-Hsu
                                          Standard
        Chem       _Chem      Estimate       Error       DF    t Value    Pr > |t|     Adj P    
        Arasan     Control      4.6000      1.4711       16       3.13      0.0065    0.0218
        Fermate    Control      5.0000      1.4711       16       3.40      0.0037    0.0125
        Semesan    Control      4.2000      1.4711       16       2.86      0.0115    0.0375
        Spergon    Control      2.6000      1.4711       16       1.77      0.0962    0.2680
        

The results are the same whether we used fixed blocks or random blocks.
This is because our data are **balanced**--we had the same number of
observations in each block, and all treatments appear in all blocks. If
our data had not been balanced, the results would be different.

Let's go back to the ANOVA table to see where the estimate of
$\sigma^2_R$ is coming from

We just said that the analysis based on the treatment design will not
change. Let's consider an example with a one-way regression/quantitative
factor levels treatment design.

**Example:** In this experiment, seven soil samples were taken from the
Canary Islands. Each soil sample was split into five subsamples and a
phosphate (Na$_2$,PO$_4$H) was added in amounts 0, 50, 100, 150, 200 ppm
to these five subsamples (note even spacing). At the end of the
experiment, the amount of exchangeable calcium was measured on each
subsample. The seven samples serve as blocks. There are five levels of
phosphate so

Let's first consider blocks fixed (it doesn't matter here, since the
experiment is balanced). First we have to decide the order of the
polynomial:

        proc glimmix data=soil;
          class block;
          model calcium=block phos phos*phos phos*phos*phos phos*phos*phos*phos/htype=1;
        run;

which gives output:

                     Fit Statistics
        Pearson Chi-Square / DF         0.13
        
                     Type I Tests of Fixed Effects
                                Num      Den
        Effect                   DF       DF    F Value    Pr > F
        
        block                     6       24     109.80    <.0001
        phos                      1       24       7.08    0.0137
        phos*phos                 1       24       1.62    0.2147
        phos*phos*phos            1       24       1.88    0.1829
        phos*phos*phos*phos       1       24       0.48    0.4940
        

Now that we've determined the appropriate polynomial, we can get the
fitted model:

        proc glimmix data=soil;
          class block;
          model calcium=block phos/htype=1 solution;
        run;

which gives

                                   Parameter Estimates
                                          Standard
        Effect       block    Estimate       Error       DF    t Value    Pr > |t|
        
        Intercept               1.9737      0.1854       27      10.65      <.0001
        block        1          1.4520      0.2312       27       6.28      <.0001
        block        2          2.2280      0.2312       27       9.64      <.0001
        block        3          0.8540      0.2312       27       3.69      0.0010
        block        4          0.4420      0.2312       27       1.91      0.0666
        block        5          5.0280      0.2312       27      21.75      <.0001
        block        6          0.9960      0.2312       27       4.31      0.0002
        block        7               0           .        .        .         .
        phos                  0.002283    0.000874       27       2.61      0.0145
        Scale                   0.1336     0.03637        .        .         .
        

So the fitted model is:

Do we really care about the effect Block 1 has on exchangeable calcium?
Probably not. The seven soil samples were likely selected at random, so
it really makes sense to treat blocks as random. Let's go back to the
most complicated possible model and start over.

        proc glimmix data=soil;
          class block;
          model calcium=phos phos*phos phos*phos*phos phos*phos*phos*phos/htype=1;
          random block;
        run;

          Covariance Parameter Estimates
                                Standard
        Cov Parm    Estimate       Error
        
        block         2.8049      1.6343
        Residual      0.1289     0.03721
        
        
                     Type I Tests of Fixed Effects
                                Num      Den
        Effect                   DF       DF    F Value    Pr > F
        
        phos                      1       24       7.08    0.0137
        phos*phos                 1       24       1.62    0.2147
        phos*phos*phos            1       24       1.88    0.1829
        phos*phos*phos*phos       1       24       0.48    0.4940
        

Now let's get the final model

        proc glimmix data=soil;
          class block;
          model calcium=phos/htype=1 solution;
          random block;
        run;

         Covariance Parameter Estimates
                                Standard
        Cov Parm    Estimate       Error
        
        block         2.8040      1.6343
        Residual      0.1336     0.03637
        
                          Solutions for Fixed Effects
                                 Standard
        Effect       Estimate       Error       DF    t Value    Pr > |t|
        
        Intercept      3.5451      0.6419        6       5.52      0.0015
        phos         0.002283    0.000874       27       2.61      0.0145
        
                Type I Tests of Fixed Effects
                      Num      Den
        Effect         DF       DF    F Value    Pr > F
        
        phos            1       27       6.83    0.0145
        

So, our fitted model is

Note that when we treated blocks as either fixed or random

-   We ended up picking the same model

-   The estimate for the slope was the same

Our goal was to describe the relationship between the treatment and the
response. The blocks just give us a way to eliminate excess variance.

**Did Blocking Work?**

When we treated blocks as fixed effects, we get a p-value associated
with block but it is completely meaningless because there is no valid
hypothesis test for evaluating the effect of block. However, we can
check the **efficiency** of the block design relative to a competing
design.

Suppose we have $t$ treatments and $rt$ experimental units available for
our experiment. We have two possible experimental designs:

-   
-   

The only difference between these is whether we group the experimental
units into blocks before randomly assigning the treatments. Efficiency
gives us a way to compare the variance of two competing designs--we want
to select the design that gives us the smaller variance of estimated
treatment differences.

-   CRD:

-   RCBD

So the choice between these two designs comes down to a comparison of
$\sigma^2_{CRD}$ and $\sigma^2_{RCBD}$. We can compare variances using a
ratio called the **relative efficiency**.

If RE $>$ 1

Once we've conducted an RCBD experiment we can look and see whether we
did the right thing when we used blocks.

Note there is a difference in the error degrees of freedom between the
CRD and RCBD which can have an impact. We can adjust for this difference
by calculating the **adjusted relative efficiency**:

The correction factor is always less than 1, and usually won't make much
difference. It can make a difference if the number of treatments and
reps is small.

**Example: Rice paddies** In this example, there were 4 blocks and two
treatments. We'll fit the model both with blocks and without.

-   RCBD:

-   CRD:

**Example: Seed Emergence** In this example, there were five blocks and
five treatments. Again, we'll fit the model both with blocks and
without.

-   RCBD:

-   CRD:

**Blocking and Power**

We've used several methods to determine power and sample size for CRDs:
`proc power`, `proc glmpower`, and tricking `proc glimmix` into
calculating it for us. We can use the third option to incorporate
blocking into our power calculations. These often require a pilot data
set so we can get some idea about how much variance we expect among the
blocks.

**Example:** Researchers are interested in exploring the effect of three
types of insecticide on the count of plant seedlings. They know there
will be variability due to location in the greenhouse, so they plan on
using an RCBD. They have pilot data from a previous study in the
greenhouse, with 4 samples from 6 greenhouse benches selected at random.
The data are in 'block power.sas.'

The researchers want to treat block as random, so we need estimates of
both $\sigma^2$ and $\sigma^2_R$. We can get estimates of both of these
based on our pilot data, as well as 75% confidence intervals for the
variances.

We can use the following code:

        proc glimmix noprofile data=seedlings;
          class plot;
          model count = ;
          random plot;
          covtest / cl(type=plr alpha=0.25);
        run;

                                  Covariance Parameter Estimates
                                               Profile Likelihood 75% Confidence Bounds
                                            ------- Lower ------    ------- Upper ------
                                Standard                    Pr >                    Pr >
        Cov Parm    Estimate       Error       Bound       Chisq       Bound       Chisq
        
        plot         44.5903     29.1831     22.5497      0.2500      104.00      0.2500
        Residual      6.1806      2.0602      4.3102      0.2500      9.3093      0.2500
        

Now we've got estimates for the variances, and can turn our attention to
the differences we want to be able to detect. The researchers are
interested in detecting differences of 1, 2, and 3 units. We're going to
start with 6 blocks, and see if that's enough power. They want
$\alpha=0.05$ and are aiming for a power of 0.80.

We can modify the previous code we used to trick `glimmix` as follows:

        *set up example data set with initial parameters;
        data seedlingsa;
          nblock=6;
          input insecticide mu;
          do block=1 to nblock;
            output;
          end;
        datalines;
        1 79
        2 82
        3 80
        ;
        
        proc glimmix data=seedlingsa;
          class block insecticide;
          model mu=insecticide;
          random block;
          parms(104)(9.3093)/hold=1,2;
          contrast '1 unit diff' insecticide 1 0 -1;
          contrast '2 unit diff' insecticide 0 1 -1;
          contrast '3 unit diff' insecticide 1 -1 0;
          lsmeans  insecticide/diff cl;
          ods output tests3=overallF1 contrasts=contrastF1;
        run;
        
        
        
        
        /* Power computation & Print step */
        data power1;
          set overallF1 contrastF1;
          alpha=0.05;
          ncparm=numDF*Fvalue;
          Critical_F=Finv(1-alpha,numDF,denDF,0);
          Power=1-probF(Critical_F,numDF,denDF,ncparm);
        
        proc print data=power1;
          var effect label numdf dendf alpha critical_F ncparm Power; 
          title 'power - RCBD';
        run;
        

which gives the following:

                                                 power - RCBD        
                                  Num                      Critical_
        Obs      Effect          Label         DF    DenDF    alpha        F         ncparm     Power
        1     insecticide                      2       10     0.05     4.10282     3.00774    0.24820
        2                    1 unit diff       1       10     0.05     4.96460     0.32226    0.08092
        3                    2 unit diff       1       10     0.05     4.96460     1.28903    0.17731
        4                    3 unit diff       1       10     0.05     4.96460     2.90033    0.33778
        

So it doesn't look like 6 blocks is enough. Even with a 3 unit
difference, the power is only 0.33778. Let's see what happens if we
increase the block size to

-   20 blocks:

-   40 blocks:

-   150 blocks:
