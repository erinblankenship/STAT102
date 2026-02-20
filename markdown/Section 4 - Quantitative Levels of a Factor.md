**STAT 212:** Quantitative Levels of a Factor

As a reminder, we've considered four basic treatment structures that may
be used in a one-way treatment design:

1.  Unstructured

2.  Control versus other treatments

3.  Quantitative

4.  Other structure

The last of the four treatment structures we still need address is the
quantitative factor. Up to now, we've only considered qualitative
factors. A **qualitative factor** is

We've seen many of these examples:

On the other hand, a **quantitative factor** is

When we look at the initial experimental design (how the treatments are
assigned to experimental units) and analysis of a study, it doesn't
really matter whether our factor is quantitative or qualitative. The
researcher wants to know whether the response differs based on the
treatment levels.

However, many studies that have a quantitative factor are conducted
because

The general approach to fitting these models is regression analysis. The
functional relationship between the quantitative factor and the response
may be linear or non-linear, and a polynomial model is often used to
describe the functional relationship. It is the simplest of the linear
functions.

The general form of a **polynomial model of $p$ degrees** is

The objective is to determine

**Example:** A researcher is interested in studying the tensile strength
of a new synthetic fiber to use in a cotton blend for shirts. The
researcher knows the strength of material is affected by the percent (by
weight) of cotton used in the blend and a range between 10 and 40%
cotton is necessary to meet other characteristics criteria, like
accepting a no-iron treatment. The researcher decides to test a range of
percentages of cotton: 15, 20, 25, 30, and 35% and has the resources to
test five samples at each level. The data are:

::: center
                  Observations                 
  -------------- -------------- ---- ---- ---- ----
   2-6 Cotton %        1         2    3    4    5
        15             7         7    15   11   9
        20             12        17   12   18   18
        25             15        19   19   21   19
        30             19        25   22   19   23
        35             7         10   11   15   11
:::

-   **Treatment Design:**

    -   Factor:

    -   Levels:

-   **Experimental Design:**

    -   

Let's first look at a plot of the treatment means by treatment level.
We'll use this code:

        proc glimmix data=cotton;
          class percent; *treating percent as a qualitative var only to get the trt means;
          model strength=percent;
          lsmeans percent/plot=meanplot(join); *connect the dots;
        run;

This produces the plot

![image](cotton plot.png){width="70%"}

Anytime we're fitting a polynomial model, the number of degrees $p$ that
can be fit is

and each degree corresponds to 1 treatment degree of freedom. If we fit
all terms, we get a complete partition of the treatment effect into its
one degree of freedom components.

In the case, the highest order model we can fit is

From the plot, it appears

The first model we'll try fitting is a quartic model:

where

There are two models for fitting the polynomial model:

-   direct regression

-   orthogonal polynomials

**Direct regression**

The **direct regression** approach considers the treatment/explanatory
variable as a regression variable, not an ANOVA variable (though we know
they're really the same model at their core!). Because we are treating
our explanatory varibale as a regression variable, it *[must
not]{.underline}* appear in the `class` statement. The following code
fits a quartic model to the data:

    proc glimmix data=cotton;
      model strength=percent percent*percent percent*percent*percent 
                     percent*percent*percent*percent/htype=1,3;
    /*alternate model statement */
    * model strength=percent|percent|percent|percent/htype=1,3;
    run;

A few notes:

-   We're asking for both Type I and Type III tests.

-   Note the alternate `model` statement.

Here's part of the output

                        Type I Tests of Fixed Effects
        
                                 Num      Den
        Effect                    DF       DF    F Value    Pr > F
        
        percent                    1       20       4.12    0.0559
        percent*percent            1       20      47.66    <.0001
        percen*percen*percen       1       20       7.96    0.0105
        perc*perc*perc*perce       1       20       2.19    0.1549
        
        
                       Type III Tests of Fixed Effects
        
                                 Num      Den
        Effect                    DF       DF    F Value    Pr > F
        
        percent                    1       20       1.57    0.2242
        percent*percent            1       20       1.67    0.2115
        percen*percen*percen       1       20       1.88    0.1857
        perc*perc*perc*perce       1       20       2.19    0.1549
        

The **Type I Tests**

So, it looks like

The **Type III Tests**

So now we can rerun the analyses, keeping the terms up to the highest
order that has a significant contribution. In our example

We'll use the code:

        proc glimmix data=cotton;
          model strengt=percent|percent|percent/solution htype=1;
        run;

Note we've added the `solution` option, which will provide the parameter
estimates for the effects. Here's the output:

                                     Parameter Estimates
        
                                            Standard
        Effect                  Estimate       Error       DF    t Value    Pr > |t|
        
        Intercept                59.5257     38.2927       21       1.55      0.1350
        percent                  -8.7257      5.0052       21      -1.74      0.0959
        percent*percent           0.4757      0.2081       21       2.29      0.0327
        percen*percen*percen    -0.00760    0.002768       21      -2.75      0.0121
        Scale                     8.6205      2.6604        .        .         .
        
        
                                    Type I Tests of Fixed Effects
        
                                 Num      Den
        Effect                    DF       DF    F Value    Pr > F
        
        percent                    1       21       3.90    0.0616
        percent*percent            1       21      45.12    <.0001
        percen*percen*percen       1       21       7.54    0.0121
        

We can use these estimates to construct the fitted model:

We can also plot the predicted values from our equation

        proc glimmix data=cotton;
          model strength=percent|percent|percent/solution htype=1;
          output out=cottonout pred=predicted;
        run;
        
        proc print data=cottonout; run;

Which gives us the output data set that I've called cottonout:

    Obs    percent    strength    predicted
        
      1       15          7        10.0257
         2       15          7        10.0257
         3       15         15        10.0257
         4       15         11        10.0257
         5       15          9        10.0257
         6       20         12        14.4971
         7       20         17        14.4971
         8       20         12        14.4971
         9       20         18        14.4971
        10       20         18        14.4971
        11       25         15        19.9543
        12       25         19        19.9543
        13       25         19        19.9543
        14       25         21        19.9543
        15       25         19        19.9543
        16       30         19        20.6971
        17       30         25        20.6971
        18       30         22        20.6971
        19       30         19        20.6971
        20       30         23        20.6971
        21       35          7        11.0257
        22       35         10        11.0257
        23       35         11        11.0257
        24       35         15        11.0257
        25       35         11        11.0257
        

To plot the predicted values there are few options

        title "Regression model for cotton data";
        symbol1 interpol=none value=dot color=black;
        symbol2 interpol=join value=diamond color=red;
        proc gplot data=cottonout;
          plot strength*percent predicted*percent/overlay;
        run;
        
        title "Regression model for cotton data";
        symbol1 interpol=none value=dot color=black;
        symbol2 interpol=RC value=diamond color=red; *can only go up to a cubic;
        proc gplot data=cottonout;
          plot strength*percent predicted*percent/overlay;
        run;
        
        title "Regression model for cotton data";
        symbol1 interpol=none value=dot color=black;
        symbol2 interpol=spline value=diamond color=red; 
        proc gplot data=cottonout;
          plot strength*percent predicted*percent/overlay;
        run;

Let's go to SAS to see the differences among these three plots.

Notice that at the percents outside the inference space we get nonsense
strength values. Only values between 15 and 35 percent are within the
inference space.

**Orthogonal polynomials**

Quantitative factors levels are one of the few cases where a full set of
**orthogonal contrasts** can be useful. However, this approach does
require

and involves testing orthogonal contrasts among the treatment factor
levels. These contrasts allow us to evaluate the importance of each
polynomial component with a specific contrast.

Here are the steps:

-   
-   
-   
-   
-   

The coefficients for polynomial contrasts are not obvious. It's easiest
to look them up (google orthogonal polynomial coefficients table). For a
design with 5 equally spaced levels:

![image](ortho poly.jpg){width="70%"}

So, our code would look like:

        proc glimmix data=cotton;
          class percent;
          model strength=percent;
          contrast 'linear' percent -2 -1 0 1 2;
          contrast 'quadratic' percent 2 -1 -2 -1 2;
          contrast 'cubic' percent -1 2 0 -2 1;
          contrast 'quartic' percent 1 -4 6 -4 1;
        run;

Which gives the output:

           Type III Tests of Fixed Effects

                  Num      Den
    Effect         DF       DF    F Value    Pr > F

    percent         4       20      15.48    <.0001


                      Contrasts

                   Num      Den
    Label          DF       DF    F Value    Pr > F

    linear          1       20       4.12    0.0559
    quadratic       1       20      47.66    <.0001
    cubic           1       20       7.96    0.0105
    quartic         1       20       2.19    0.1549

Note that these results are the same as we got with the direct
regression method!
