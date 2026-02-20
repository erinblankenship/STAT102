**STAT 212:** Multifactor Experiments

So far, we've discussed factorial experiments with two factors, A and B,
both of which have 2 or more levels, $a$ and $b$. We've referred to this
as an $a \times b$ factorial treatment design. We're still considering
the completely randomized (CRD) experimental design. If we add a factor
C (with $c$ levels) to our $a \times b$ design, we have an
$a \times b \times c$ factorial design. If all three factors have $a$
levels, we can alternatively describe this as an $a^3$ factorial. The
total number of treatments is $a \times b \times c$.

**Example:** The following experiment was actually carried out in an
experimental design class at Arizona State University, and considered
some of the many different ways to bake brownies. The purpose of the
experiment was to determine how the pan material, brand of brownie mix,
and the stirring method affect the scrumptiousness of brownies. The
factors and levels were:

::: center
  Factor   Levels
  -------- --------
  A=       
  B=       
  C=       
:::

This is a $2 \times 2 \times 2 = 2^3$ experiment. There are 8 treatment
combinations:

::: center
  ----------- ---------- ---------- -----------
              Factors               
   Treatment  Pan        Stirring   Mix
       1      Glass      Spoon      Expensive
       2      Aluminum   Spoon      Expensive
       3      Glass      Mixer      Expensive
       4      Aluminum   Mixer      Expensive
       5      Glass      Spoon      Cheap
       6      Aluminum   Spoon      Cheap
       7      Glass      Mixer      Cheap
       8      Aluminum   Mixer      Cheap
  ----------- ---------- ---------- -----------
:::

The response variable was scrumptiousness, a subjective measured derived
from a questionnaire given to the subjects who sampled each batch of
brownies. The questionnaire included questions related to taste,
appearance, consistency, aroma, etc. Eight batches of each treatment
combination were rated, for a total of $8 \times 8 = 64$ experimental
units.

Before we look at the data, let's consider how we must adapt our
notation, model, and ANOVA table for an additional factor.

First, consider the cell mean $\mu_{ijk}$

Our response now have 4 subscripts: $y_{ijkl}$

The treatment effects model is:

And a sketch of the ANOVA table:

It gets a little more complicated to make a table of cell means

::: center
                   Stirring Method                               
  --------------- ----------------- ------------- -------------- -------------
        2-5             Spoon                         Mixer      
        2-5         Pan Material                   Pan Material  
   2-5 Mix Brand        Glass         Aluminum        Glass        Aluminum
     Expensive       $\mu_{111}$     $\mu_{211}$   $\mu_{121}$    $\mu_{221}$
       Cheap         $\mu_{112}$     $\mu_{212}$   $\mu_{122}$    $\mu_{222}$
:::

The data are in the file 'brownie.sas.' Let's put in the observed cell
means

::: center
                   Stirring Method                            
  --------------- ----------------- ---------- -------------- ----------
        2-5             Spoon                      Mixer      
        2-5         Pan Material                Pan Material  
   2-5 Mix Brand        Glass        Aluminum      Glass       Aluminum
     Expensive                                                
                                                              
       Cheap                                                  
                                                              
:::

We can also think about main effect marginal means

and treatment combination marginal means

We can again look at simple effects but this time, instead of looking at
the effect of one factor with the other held constant, simple effects
describe the effect of one factor with the other two held constant.

**Example:** We can consider the simple effect of pan material, given
stirring with a spoon and expensive mix:

We can use simple effects to determine if there is **conditional
interaction**. Conditional interaction means

This is denoted as $A \times B | C_k$ or $A \times B | C$. Determining
whether $A \times B | C_k$ is nonzero is the same as asking

We can also construct interaction plots to explore this. For the brownie
example, suppose we are interested in the interaction between pan
material and stirring method, given the cheap brand.

We can also examine the conditional interaction between pan material and
stirring method, given the expensive mix.

These two plots together can be examined to see if there is a three-way
interaction present. Three-way interaction occurs when

In this case, it does not appear there is significant three-way
interaction. What would three-way interaction look like?

We'll now explore the analysis of a three-way factorial treatment design
by looking first at the analysis of the brownie data, and then two
additional example data sets with increasingly complex interactions
among the three factors.

The basic steps in analyzing data arising from a three-factor design
are:

1.  Include all main effects, two-way, and three-way interaction terms
    in the model and test the three-way interaction.

2.  If the three-way interaction term is not significant, test the
    two-way interactions. If the three-way interaction is significant,
    test the two-way interactions at each level of the third factor
    (conditional interactions).

3.  Depending on the results of (1) and (2),

    -   Test main effects (for those main effects free of any
        interaction)

    -   Test simple effects of one factor conditional on the second but
        free of the third (when there are two-way interactions, but no
        three-way)

    -   Test simple effects of one factor conditional on combinations of
        the other two (when there is three-way interaction)

This can be conducted using a two-stage approach in SAS.

**Stage 1:** Run the basic analysis including all main effects, two-way
interactions, and three-way interaction to see what is significant.

        proc glimmix;
          class a b c;
          model y=a|b|c; *or model y=a b c a*b a*c b*c a*b*c;
        run;

**Stage 2:** Add needed two-way interaction and/or main effect
contrasts, `lsmeans`, `slice`, and `slicediff` statements to test
various hypotheses and estimate various differences, depending on which
thee-way or two-way interactions are significant (and therefore which
hypotheses are legitimate to test).

**Example: Brownies**

**Stage 1:** We'll start using the basic program

        proc glimmix data=brownies;
          class pan stir mix;
          model y=pan|stir|mix;
        run;

which gives

                  Type III Tests of Fixed Effects
        
                         Num      Den
        Effect            DF       DF    F Value    Pr > F
        
        pan                1       56      11.95    0.0010
        stir               1       56       2.99    0.0894
        pan*stir           1       56       0.01    0.9194
        mix                1       56       0.01    0.9194
        pan*mix            1       56       0.26    0.6132
        stir*mix           1       56       0.17    0.6858
        pan*stir*mix       1       56       0.04    0.8396

**Stage 2:** Based on the output above, we only need to further
investigate the (main) effects of pan material and (potentially)
stirring method. We can refit the model without the interactions and add
an `lsmeans` statements

        proc glimmix data=brownies;
          class pan stir mix;
          model y=pan stir mix;
          lsmeans pan stir/diff;
        run;

which gives


             Type III Tests of Fixed Effects
                  Num      Den
    Effect         DF       DF    F Value    Pr > F

    pan             1       60      12.70    0.0007
    stir            1       60       3.17    0.0798
    mix             1       60       0.01    0.9169


                     pan Least Squares Means
                            Standard
    pan         Estimate       Error       DF    t Value    Pr > |t|

    aluminum     12.6250      0.4217       60      29.94      <.0001
    glass        10.5000      0.4217       60      24.90      <.0001


                    Differences of pan Least Squares Means
                                        Standard
    pan         _pan        Estimate       Error       DF    t Value    Pr > |t|

    aluminum    glass         2.1250      0.5963       60       3.56      0.0007


                      stir Least Squares Means
                         Standard
    stir     Estimate       Error       DF    t Value    Pr > |t|

    mixer     12.0938      0.4217       60      28.68      <.0001
    spoon     11.0313      0.4217       60      26.16      <.0001

     
                 Differences of stir Least Squares Means
                                  Standard
    stir     _stir    Estimate       Error       DF    t Value    Pr > |t|

    mixer    spoon      1.0625      0.5963       60       1.78      0.0798

**Example: Data Set 2** The data are in the file 'multifactor
examples.sas'.

Let's first explore the cell means and sketch the interaction plots.

                              a*b*c Least Squares Means
        
                                   Standard
        a    b    c    Estimate       Error       DF    t Value    Pr > |t|
        
        1    1    1      221.83      4.8769       16      45.49      <.0001
        1    1    2      243.50      4.8769       16      49.93      <.0001
        1    2    1      239.77      4.8769       16      49.16      <.0001
        1    2    2      247.73      4.8769       16      50.80      <.0001
        2    1    1      254.87      4.8769       16      52.26      <.0001
        2    1    2      270.40      4.8769       16      55.44      <.0001
        2    2    1      232.07      4.8769       16      47.58      <.0001
        2    2    2      245.67      4.8769       16      50.37      <.0001

Let's plot B $\times$ C for each level of A.

So it looks like there is not a 3-way interaction.

**Stage 1:** We'll start with the basic program

        proc glimmix data=example2;
          class a b c;
          model y=a|b|c;
        run;

                   Fit Statistics
        
        Pearson Chi-Square / DF        71.35
        
        
             Type III Tests of Fixed Effects
                      Num      Den
        Effect         DF       DF    F Value    Pr > F
        
        a               1       16      13.23    0.0022
        b               1       16       3.38    0.0846
        a*b             1       16      25.53    0.0001
        c               1       16      18.15    0.0006
        a*c             1       16       0.00    0.9715
        b*c             1       16       1.28    0.2738
        a*b*c           1       16       0.73    0.4062

Based on this, we can look at the main effects of C and the simple
effects of A given B (averaged over C) and B given A (averaged over C).

**Stage 2:**

        proc glimmix data=example2;
          class a b c;
          model y=a b a*b c;
          lsmeans c/diff;
          lsmeans a*b/slicediff=a slicediff=b;
        run;

                     Fit Statistics
        Pearson Chi-Square / DF        67.65
        
                Type III Tests of Fixed Effects
                      Num      Den
        Effect         DF       DF    F Value    Pr > F
        
        a               1       19      13.95    0.0014
        b               1       19       3.57    0.0743
        a*b             1       19      26.93    <.0001
        c               1       19      19.14    0.0003
        
                    c Least Squares Means
                         Standard
        c    Estimate       Error       DF    t Value    Pr > |t|
        
        1      237.13      2.3743       19      99.87      <.0001
        2      251.83      2.3743       19     106.06      <.0001
        
               Differences of c Least Squares Means
                               Standard
        c    _c    Estimate       Error       DF    t Value    Pr > |t|
        
        1    2     -14.6917      3.3578       19      -4.38      0.0003
        
                       a*b Least Squares Means
                              Standard
        a    b    Estimate       Error       DF    t Value    Pr > |t|
        
        1    1      232.67      3.3578       19      69.29      <.0001
        1    2      243.75      3.3578       19      72.59      <.0001
        2    1      262.63      3.3578       19      78.22      <.0001
        2    2      238.87      3.3578       19      71.14      <.0001
        
              Simple Effect Comparisons of a*b Least Squares Means By a
        Simple
        Effect                           Standard
        Level     b    _b    Estimate       Error       DF    t Value    Pr > |t|
        
        a 1       1    2     -11.0833      4.7486       19      -2.33      0.0307
        a 2       1    2      23.7667      4.7486       19       5.00      <.0001
        
               Simple Effect Comparisons of a*b Least Squares Means By b
        Simple
        Effect                           Standard
        Level     a    _a    Estimate       Error       DF    t Value    Pr > |t|
        
        b 1       1    2     -29.9667      4.7486       19      -6.31      <.0001
        b 2       1    2       4.8833      4.7486       19       1.03      0.3167

We can also construct interaction plots to visualize the interaction
between A and B:

        proc glimmix data=example2;
          class a b c;
          model y=a b a*b c;
          lsmeans a*b/plots=meanplot(sliceby=b join);
        run;

![image](Example2.png){width="70%"}

Let's verify that this really is averaging over C.

**Example: Data Set 3** The data are in the file 'multifactor
examples.sas'.

Let's first explore the cell means and sketch the interaction plots.

                          a*b*c Least Squares Means
                               Standard
    a    b    c    Estimate       Error       DF    t Value    Pr > |t|

    1    1    1      223.13      6.2094       16      35.93      <.0001
    1    1    2      234.53      6.2094       16      37.77      <.0001
    1    2    1      247.77      6.2094       16      39.90      <.0001
    1    2    2      259.57      6.2094       16      41.80      <.0001
    2    1    1      245.67      6.2094       16      39.56      <.0001
    2    1    2      275.30      6.2094       16      44.34      <.0001
    2    2    1      265.33      6.2094       16      42.73      <.0001
    2    2    2      249.90      6.2094       16      40.25      <.0001

Let's plot B $\times$ C for each level of A.

So it looks like there is a 3-way interaction.

**Stage 1:** We'll start with the basic program

        proc glimmix data=example2;
        class a b c;
        model y=a|b|c;
        run;

                      Fit Statistics
            Pearson Chi-Square / DF       115.67
        
        
             Type III Tests of Fixed Effects
                      Num      Den
        Effect         DF       DF    F Value    Pr > F
        
        a               1       16      16.43    0.0009
        b               1       16       6.26    0.0236
        a*b             1       16       9.95    0.0061
        c               1       16       4.53    0.0491
        a*c             1       16       0.26    0.6153
        b*c             1       16       6.47    0.0217
        a*b*c           1       16       6.70    0.0198

So our suspicion of three-way interaction is verified. Let's explore
this a bit more using plots. We've seen the B $\times$ C for each level
of A. To illustrate how to use SAS to construct the interaction plots,
let's consider A $\times$ B for each level of C.

        proc glimmix data=example3;
          class a b c;
          model y=a|b|c;
          lsmeans a*b*c/plot=meanplot (sliceby=b plotby=c join);
        run;

![image](Example 31.png){height="35%"}

![image](Example 32.png){height="35%"}

So it looks like there is no interaction between A and B at C=1.

**Stage 2:** We can formally test the interaction between A and B at
C=1, and proceed based on the results. We'll use the same approach we
did in the last section:

1.  Write the linear combination you want to test or estimate in terms
    of the cell means, $\mu_{ij}$

2.  Convert means into model parameters

3.  Gather like terms

In this case, we're interested in
$\mu_{111} - \mu_{121} - \mu_{211} + \mu_{221}$.

This leads to the `contrast` statement

        contrast 'a*b at c=1' a*b 1 -1 -1 1 a*b*c 1 0 -1 0 -1 0 1 0;

                           Contrasts
                       Num      Den
        Label           DF       DF    F Value    Pr > F
        
        a*b at c=1       1       16       0.16    0.6945

As suspected, there is no A $\times$ B interaction at C=1. So, we can
examine the 'main' effects of A and B at C=1.

        proc glimmix data=example3;
          class a b c;
          model y=a|b|c;
          lsmeans a*c/slicediff=c;
          lsmeans b*c/slicediff=c;
        run;

                            a*c Least Squares Means
                              Standard
        a    c    Estimate       Error       DF    t Value    Pr > |t|
        
        1    1      235.45      4.3907       16      53.62      <.0001
        1    2      247.05      4.3907       16      56.27      <.0001
        2    1      255.50      4.3907       16      58.19      <.0001
        2    2      262.60      4.3907       16      59.81      <.0001
        
                Simple Effect Comparisons of a*c Least Squares Means By c   
        Simple
        Effect                           Standard
        Level     a    _a    Estimate       Error       DF    t Value    Pr > |t|
        
        c 1       1    2     -20.0500      6.2094       16      -3.23      0.0052
        c 2       1    2     -15.5500      6.2094       16      -2.50      0.0235

                           b*c Least Squares Means
                              Standard
        b    c    Estimate       Error       DF    t Value    Pr > |t|
        
        1    1      234.40      4.3907       16      53.39      <.0001
        1    2      254.92      4.3907       16      58.06      <.0001
        2    1      256.55      4.3907       16      58.43      <.0001
        2    2      254.73      4.3907       16      58.02      <.0001
        
                 Simple Effect Comparisons of b*c Least Squares Means By c
        Simple
        Effect                           Standard
        Level     b    _b    Estimate       Error       DF    t Value    Pr > |t|
        
        c 1       1    2     -22.1500      6.2094       16      -3.57      0.0026
        c 2       1    2       0.1833      6.2094       16       0.03      0.9768

However, it does look like there is an interaction between A and B if
C=2. We'll check this formally. This time, we're interested in
$\mu_{112} - \mu_{122} - \mu_{212} + \mu_{222}$.

**Practice:** Find the coefficients necessary to test this contrast.

                           Contrasts
                       Num      Den
        Label           DF       DF    F Value    Pr > F
        
        a*b at c=2       1       16      16.49    0.0009

So, the A $\times$ B interaction is significant. So, we must look at
simple effects.

        lsmeans a*b*c/slicediff=b*c slicediff=a*c;

                             a*b*c Least Squares Means  
                                   Standard
        a    b    c    Estimate       Error       DF    t Value    Pr > |t|
        
        1    1    1      223.13      6.2094       16      35.93      <.0001
        1    1    2      234.53      6.2094       16      37.77      <.0001
        1    2    1      247.77      6.2094       16      39.90      <.0001
        1    2    2      259.57      6.2094       16      41.80      <.0001
        2    1    1      245.67      6.2094       16      39.56      <.0001
        2    1    2      275.30      6.2094       16      44.34      <.0001
        2    2    1      265.33      6.2094       16      42.73      <.0001
        2    2    2      249.90      6.2094       16      40.25      <.0001
        
        
                 Simple Effect Comparisons of a*b*c Least Squares Means By b*c
        Simple
        Effect                            Standard
        Level      a    _a    Estimate       Error       DF    t Value    Pr > |t|
        
        b*c 1 1    1    2     -22.5333      8.7815       16      -2.57      0.0207
        b*c 1 2    1    2     -40.7667      8.7815       16      -4.64      0.0003
        b*c 2 1    1    2     -17.5667      8.7815       16      -2.00      0.0627
        b*c 2 2    1    2       9.6667      8.7815       16       1.10      0.2873

                 Simple Effect Comparisons of a*b*c Least Squares Means By a*c
        
        Simple
        Effect                            Standard
        Level      b    _b    Estimate       Error       DF    t Value    Pr > |t|
        
        a*c 1 1    1    2     -24.6333      8.7815       16      -2.81      0.0127
        a*c 1 2    1    2     -25.0333      8.7815       16      -2.85      0.0116
        a*c 2 1    1    2     -19.6667      8.7815       16      -2.24      0.0397
        a*c 2 2    1    2      25.4000      8.7815       16       2.89      0.0106

So, how would we write an overall summary of the results of Example 3?
We explored:

-   'Main' effects of A and B at C=1

-   Simple effects of A and B at C=2
