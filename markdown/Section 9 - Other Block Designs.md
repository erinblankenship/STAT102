**STAT 212:** Other Block Designs

**Row-Column Designs**

Sometimes there are two blocking variables that can be used to reduce
extraneous variability, rather than one factor. If it possible to block
on both of the variables we may be able to increase precision while
using the same number of experimental units as in a randomized complete
block design.

Designs with two blocking criteria are called **row-column designs**.
There are several different types of row-column designs,and we'll
discuss one of them, specifically the Latin square.

Row-column designs tend to be situations in which there are two
gradients:

The generic model for row-column designs is

::: center
observation = row + column + treatment + error
:::

We assume there is no row$\times$column interaction, no
row$\times$treatment interaction, and no column$\times$treatment
interaction.

**Example:** Suppose we want to compare five kinds of glue, and glue is
affected by temperature and humidity. Temperature and humidity vary
across days and over time throughout the day, so two possible blocking
criteria are day and hour. A **Latin Square** for five treatments blocks
on day and hour looks like this

Each day has a full set of five treatments and each hour has a full set
of five treatments, so both day and hour could be considered blocks. The
two types of blocking are superimposed on each other.

The Latin Square design requires

By using a Latin square arrangement, we are able to control for 5 rows
$\times$ 5 columns $\times$ 5 treatments = 125 combinations using only
25 experimental units. Because of this feature, Latin squares have the
potential for large gains in efficiency without using more experimental
material.

The model for a Latin square is

Just like with RCBDs we can consider rows and columns to be either fixed
or random. The corresponding ANOVA table is

**Example:** Consider an experiment to study yield of four wheat
varieties (A, B, C, D). There are gradients running parallel to both
sides of the field

To account for both gradients, the experiment was conducted as a 4
$\times$ 4 Latin square. Yields are in kg per plot. The plot layout with
the data:

::: center
  ----- ---------- ---------- ---------- ----------
          Column                         
   Row      1          2          3          4
    1    10.5 (C)   7.7 (D)    12.0 (B)   13.2 (A)
    2    11.1 (B)   12.0 (A)   10.3 (C)   7.5 (D)
    3    5.8 (D)    12.2 (C)   11.2 (A)   13.7 (B)
    4    11.6 (A)   12.3 (B)   5.9 (D)    10.2 (C)
  ----- ---------- ---------- ---------- ----------
:::

In this example, the model is

and ANOVA table

The analysis in SAS is very similar to the RCBD:

        proc glimmix data=wheat;
          class row col variety;
          model yield=variety;
          random row col;
        run;

In this example, we are considering rows and columns as random effects.
The output is:

          Covariance Parameter Estimates
                                Standard
        Cov Parm    Estimate       Error
        
        row          0.04958      0.1482
        col           0.4533      0.4673
        Residual      0.4533      0.2617
        
               Type III Tests of Fixed Effects
                      Num      Den
        Effect         DF       DF    F Value    Pr > F
        
        variety         3        6      58.03    <.0001
        

It looks like there is a difference among the four varieties. We could
follow up with whatever treatment design analysis is most appropriate
(LSD, Tukey, maybe contrasts) just as before. A Latin square is a
experimental design and like the RCBD can be paired with any treatment
design.

The downside to the Latin square is that it can be restrictive, since \#
treatments = \# columns = \# rows. This means

-   
-   

The randomization of Latin squares is quite involved, and details will
be left to a more advanced design course. There are other row-column
designs as well which offer more flexibility than the Latin square.
These include Latin rectangles and replicated Latin squares. We'll also
leave those to a second design course.

**Incomplete Block Designs**

When we discussed RCBDs, we noted that we needed enough experimental
units in each block to accommodate all treatments. However, in some
situations, the number of treatments exceeds block size. This can happen
because of the physical size of the block or shortages of experimental
equipment or facilities. In instances like this, we can use randomized
block designs in which every treatment is not present in every block.
These designs are called **incomplete block designs**.

**Example:** A baker is testing different cookie recipes, and has 9
recipes to test. Due to oven size and baking time available, the baker
can only test 6 recipes per day. However, they know it's important to
block on day since oven temperature can vary day to to day. The RCDB
would say use blocks of 1.5 days, and ignore the fact that days might
not be homogeneous within a block. A better way is to use incomplete
blocks of size six (one day). Consider the following set up:

::: center
              Treatments
  ------- ------------------
    Day 1  4, 5, 6, 7, 8, 9
        2  2, 3, 5, 6, 8, 9
        3  2, 3, 4, 6, 7, 8
        4  2, 3, 4, 5, 7, 9
        5  1, 3, 5, 6, 7, 8
        6  1, 3, 4, 6, 7, 9
        7  1, 3, 4, 5, 8, 9
        8  1, 2, 5, 6, 8, 9
        9  1, 2, 4, 6, 8, 9
       10  1, 2, 4, 5, 7, 8
       11  1, 2, 3, 7, 8, 9
       12  1, 2, 3, 4, 5, 6
:::

This design is a **balanced incomplete block** (BIB) design. It's
balanced because

Standard notation for incomplete blocks is

-   
-   
-   
-   
-   

These values are not independent, but must satisfy three conditions:

1.  
2.  
3.  

Additionally,

BIBs do not exist for all combinations of blocks sizes, number of
treatments, and number of replications. For example, with the cookie
scenario, there is no BIB design with few than 8 replications. These
restrictions means BIBs are not always feasible. However, when they are
an option, they can very useful.

**Example:** Suppose we have $t=6$ treatments and block size that can
accommodate $k=4$ observations per block.

So if BIBs are not always feasible, why are they still useful? BIB
designs are **optimal** in that

If you are willing to give up balance, we can find a design with far
fewer replicates that is nearly balanced and nearly as efficient as a
BIB would be if it existed. **Partially balanced incomplete block**
(PBIB) designs share many features with BIBs. In PBIBs

Not all incomplete block designs qualify as partially balanced. A PBIB
must satisfy three criteria:

1.  
2.  
3.  

Consider the following set-up:

::: center
  ------- --- ---
   Block      
     1     2   3
     1     2   3
     4     5   6
     2     3   1
     5     6   4
  ------- --- ---
:::

The analysis of incomplete block designs is just like the analysis for
an RCBD with missing data. We didn't discuss the implications of missing
data on the RCBD, but missing data means that we don't get the same
results for random and fixed blocks and we have to carefully think about
which choice is most appropriate. Missing data also means that cell
(arithmetic) means won't be the same as lsmeans, and we should be sure
to look at lsmeans.

**Example:** An experiment was designed to test the wearing quality of
$t=7$ types of cloth. The machine used to test the wearing quality can
only process four pieces of cloth at the same time. Machine run is
considered the block. Here's a layout of the design

::: center
  ------- ------------ ----- ----- ----- ------ ----- -----
           Cloth Type                                 
   Block       A         B     C     D     E      F     G
     1                  627         248          563   252
     2        344             233                442   226
     3                        251   211   160          297
     4        337       537               195          300
     5                  520   278         199    595  
     6        369                   196   1985   606  
     7        396       602   240   273               
  ------- ------------ ----- ----- ----- ------ ----- -----
:::

Let's check that this is BIB design.

The data are in 'fabric.sas'. The model is

and the ANOVA table is

We can use the same code we did with RCBDs:

        proc glimmix data=fabric;
          class block type;
          model y=type;
          random block;
          lsmeans type/pdiff adjust=tukey;
        run;

It seems like the 7 machine runs we used could be thought of as a random
sample of all runs we could ever do on the machine, so I'm treating
blocks as random. The output is

          Covariance Parameter Estimates
                                Standard
        Cov Parm    Estimate       Error
        
        block         273.40      428.98
        Residual     1471.43      537.29
        
                Type III Tests of Fixed Effects
                      Num      Den
        Effect         DF       DF    F Value    Pr > F
        
        type            6       15      62.73    <.0001
        
                         type Least Squares Means
                            Standard
        type    Estimate       Error       DF    t Value    Pr > |t|
        
        A         363.84     20.6074       15      17.66      <.0001
        B         566.49     20.6074       15      27.49      <.0001
        C         252.61     20.6074       15      12.26      <.0001
        D         227.19     20.6074       15      11.02      <.0001
        E         184.03     20.6074       15       8.93      <.0001
        F         553.22     20.6074       15      26.85      <.0001
        G         273.13     20.6074       15      13.25      <.0001
        
         
                      Differences of type Least Squares Means
                  Adjustment for Multiple Comparisons: Tukey-Kramer
                                     Standard
        type    _type    Estimate       Error       DF    t Value    Pr > |t|     Adj P
        
        A       B         -202.65     27.8771       15      -7.27      <.0001    <.0001
        A       C          111.23     27.8771       15       3.99      0.0012    0.0160
        A       D          136.65     27.8771       15       4.90      0.0002    0.0029
        A       E          179.80     27.8771       15       6.45      <.0001    0.0002
        A       F         -189.38     27.8771       15      -6.79      <.0001    <.0001
        A       G         90.7093     27.8771       15       3.25      0.0053    0.0630
        B       C          313.88     27.8771       15      11.26      <.0001    <.0001
        B       D          339.30     27.8771       15      12.17      <.0001    <.0001
        B       E          382.46     27.8771       15      13.72      <.0001    <.0001
        B       F         13.2728     27.8771       15       0.48      0.6408    0.9988
        B       G          293.36     27.8771       15      10.52      <.0001    <.0001
        C       D         25.4242     27.8771       15       0.91      0.3762    0.9648
        C       E         68.5788     27.8771       15       2.46      0.0265    0.2407
        C       F         -300.61     27.8771       15     -10.78      <.0001    <.0001
        C       G        -20.5159     27.8771       15      -0.74      0.4731    0.9878
        D       E         43.1546     27.8771       15       1.55      0.1425    0.7142
        D       F         -326.03     27.8771       15     -11.70      <.0001    <.0001
        D       G        -45.9401     27.8771       15      -1.65      0.1201    0.6566
        E       F         -369.18     27.8771       15     -13.24      <.0001    <.0001
        E       G        -89.0946     27.8771       15      -3.20      0.0060    0.0700
        F       G          280.09     27.8771       15      10.05      <.0001    <.0001
        
