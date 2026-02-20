**STAT 212:** More than two levels per treatment factor

In the pajama example, we had two levels for each of our two factors.
Fabric was either cotton or polyester, and Additive was either X or Y.
Now we'll consider a factorial with more than two levels for each
factor.

**Example:** An experiment was conducted to aid in developing a product
that can be used as a substrate for making ribbons. The experiment was
designed to investigate the effects of base polymer and additive on the
tensile strength of the resulting ribbon. There are two factors of
interest: (1) base polymer: mylar, nylon, and polyethylene and (2)
additive: c1, c2, c3, c4, and c5. There are 3 $\times$ 5 treatment
combinations, and the researchers plan to test each treatment
combination 6 times, for a total of 90 observations. The data are in the
'ribbon.sas' file.

The model is

and the ANOVA table is

Let's look at an interaction plot to see what's going on. We can also
get an interaction plot using the `lsmeans` statement!

        proc glimmix data=ribbon;
          class polymer add;
          model strength=polymer add polymer*add;
          lsmeans polymer*add/plots=meanplot(sliceby=polymer join);
        run;

Which gives

![image](ribbon plot.png){width="70%"}

So it looks like we have interaction!

We'll also formally test for interaction

               Type III Tests of Fixed Effects

                    Num      Den
    Effect           DF       DF    F Value    Pr > F

    polymer           2       75      24.25    <.0001
    add               4       75       0.14    0.9648
    polymer*add       8       75       2.38    0.0241

        

This verifies what we see in the plot.

So, we must look at simple effects and not main effects. We'll use the
same `slice` and `slicediff` statements we used with the pajama example:

        proc glimmix data=ribbon;
          class polymer add;
          model strength=polymer add polymer*add;
          lsmeans polymer*add/slice=polymer slice=add slicediff=(polymer add);
        run;

This is going to produce a LOT of output. Let's look at the output for
`slice=add` and `slicediff=add`.

               Tests of Effect Slices for
             polymer*add Sliced By add
        
                Num      Den
        add      DF       DF    F Value    Pr > F
        
        c1        2       75       3.41    0.0383
        c2        2       75       3.53    0.0341
        c3        2       75       4.90    0.0100
        c4        2       75      14.31    <.0001
        c5        2       75       7.63    0.0010
        
        
                      Simple Effect Comparisons of polymer*add Least Squares Means By add
        
        Simple
        Effect                                       Standard
        Level     polymer    _polymer    Estimate       Error       DF    t Value    Pr > |t|
        
         add c1    mylar      nylon         6.6833      2.5593       75       2.61      0.0109
        add c1    mylar      poly          3.3500      2.5593       75       1.31      0.1945
        add c1    nylon      poly         -3.3333      2.5593       75      -1.30      0.1968
        add c2    mylar      nylon         6.6167      2.5593       75       2.59      0.0117
        add c2    mylar      poly          1.9333      2.5593       75       0.76      0.4524
        add c2    nylon      poly         -4.6833      2.5593       75      -1.83      0.0712
        add c3    mylar      nylon         7.6833      2.5593       75       3.00      0.0036
        add c3    mylar      poly          5.8167      2.5593       75       2.27      0.0259
        add c3    nylon      poly         -1.8667      2.5593       75      -0.73      0.4681
        add c4    mylar      nylon        12.4667      2.5593       75       4.87      <.0001
        add c4    mylar      poly         11.1333      2.5593       75       4.35      <.0001
        add c4    nylon      poly         -1.3333      2.5593       75      -0.52      0.6039
        add c5    mylar      nylon         3.1167      2.5593       75       1.22      0.2271
        add c5    mylar      poly          9.7833      2.5593       75       3.82      0.0003
        add c5    nylon      poly          6.6667      2.5593       75       2.60      0.0111
        
        
        

The p-values given are for the LSD mean comparisons. We're doing a lot
of comparisons!

We can try to adjust for multiplicity by using the Tukey adjustment:

          lsmeans polymer*add/slicediff=(polymer add) cl adjust=tukey;

        
                  Simple Effect Comparisons of polymer*add Least Squares Means By add
                              Adjustment for Multiple Comparisons: Tukey
    Simple
    Effect                                   Standard
    Level    polymer   _polymer   Estimate      Error      DF   t Value   Pr > |t|    Adj P    Alpha

    add c1   mylar     nylon        6.6833     2.5593      75      2.61     0.0109   0.0290     0.05
    add c1   mylar     poly         3.3500     2.5593      75      1.31     0.1945   0.3947     0.05
    add c1   nylon     poly        -3.3333     2.5593      75     -1.30     0.1968   0.3983     0.05
    add c2   mylar     nylon        6.6167     2.5593      75      2.59     0.0117   0.0310     0.05
    add c2   mylar     poly         1.9333     2.5593      75      0.76     0.4524   0.7313     0.05
    add c2   nylon     poly        -4.6833     2.5593      75     -1.83     0.0712   0.1669     0.05
    add c3   mylar     nylon        7.6833     2.5593      75      3.00     0.0036   0.0101     0.05
    add c3   mylar     poly         5.8167     2.5593      75      2.27     0.0259   0.0659     0.05
    add c3   nylon     poly        -1.8667     2.5593      75     -0.73     0.4681   0.7469     0.05
    add c4   mylar     nylon       12.4667     2.5593      75      4.87     <.0001   <.0001     0.05
    add c4   mylar     poly        11.1333     2.5593      75      4.35     <.0001   0.0001     0.05
    add c4   nylon     poly        -1.3333     2.5593      75     -0.52     0.6039   0.8614     0.05
    add c5   mylar     nylon        3.1167     2.5593      75      1.22     0.2271   0.4465     0.05
    add c5   mylar     poly         9.7833     2.5593      75      3.82     0.0003   0.0008     0.05
    add c5   nylon     poly         6.6667     2.5593      75      2.60     0.0111   0.0295     0.05

     Simple Effect Comparisons of polymer*add Least Squares Means By add
                     Adjustment for Multiple Comparisons: Tukey
    Simple
    Effect                                                     Adj         Adj
    Level    polymer   _polymer      Lower       Upper       Lower       Upper

    add c1   mylar     nylon        1.5849     11.7817      0.5637     12.8029
    add c1   mylar     poly        -1.7484      8.4484     -2.7696      9.4696
    add c1   nylon     poly        -8.4317      1.7651     -9.4529      2.7863
    add c2   mylar     nylon        1.5183     11.7151      0.4971     12.7363
    add c2   mylar     poly        -3.1651      7.0317     -4.1863      8.0529
    add c2   nylon     poly        -9.7817      0.4151    -10.8029      1.4363
    add c3   mylar     nylon        2.5849     12.7817      1.5637     13.8029
    add c3   mylar     poly         0.7183     10.9151     -0.3029     11.9363
    add c3   nylon     poly        -6.9651      3.2317     -7.9863      4.2529
    add c4   mylar     nylon        7.3683     17.5651      6.3471     18.5863
    add c4   mylar     poly         6.0349     16.2317      5.0137     17.2529
    add c4   nylon     poly        -6.4317      3.7651     -7.4529      4.7863
    add c5   mylar     nylon       -1.9817      8.2151     -3.0029      9.2363
    add c5   mylar     poly         4.6849     14.8817      3.6637     15.9029
    add c5   nylon     poly         1.5683     11.7651      0.5471     12.7863

Even with the Tukey adjustment, we need to remember the total number of
comparisons we're doing far exceeds the df for treatment. This means

A better way to try to keep the experiment-wise error rate reasonable is
to look at only a small set of comparisons that are planned before
carrying out the experiment. `contrast` and `estimate` statements (or
`lsmestimate`) can be used to look at these pre-planned comparisons.

**Planned Comparisons**

In our example, we were not specific about the goals of the experiment,
beyond considering these two factors. Typically, a researcher has more
information about the specific questions of interest. Suppose nylon and
polyethylene have been used by the company in the past, and they are
interest in how mylar compares as a potentially new option. This leads
to some specific hypotheses the company may want to test.

So, in general, the company may be interested in

But there are several ways to look at this comparison

Not all of these will be appropriate comparisons! It depends on whether
or not interaction is present. If there is interaction, we have to use
simple effects or a compromise. If there is no interaction, then we can
use the main effects.

When we would use the compromise? When there are subsets of additives
where the polymer does not interact with additive.

We'll now see how to we can use `estimate`, `contrast`, and
`lsmestimate` statements to determine which of the comparisons above are
important. The `estimate` statement is constructed based on the
estimates of the effects in the model. The `lsmestimate` statement is
constructed based on the treatment `lsmeans` (the cell means).

Here's a three step procedure to construct the `contrast` and `estimate`
statements:

1.  Write the linear combination you want to test or estimate in terms
    of the cell means, $\mu_{ij}$

2.  Convert means into model parameters

3.  Gather like terms

This will give you the coefficients to use in the statements. We're
going to start with `lsmestimate`, because this is where it really gets
to shine.

Before determining the linear combinations, we need to know what order
SAS has the `lsmeans`--this will tell us the order in which they go in
the statements. The order is determined by the order the factors appear
in the `class` statment (not the `model` statement) and then in
alphanumeric order within each factor.

For example:

            class polymer add;
            model strength=polymer|add;
            model strength=add|polymer;

will put the factors and levels in the same order.

We have three polymers with 5 additives, so the order of the
coefficients would be

However, if we use

            class add polymer;
            model strength=polymer|add;
            model strength=add|polymer;

we'd get a different order. Now, the order of the coefficients would be

Let's stick with `class polymer add` order. We want to find the
coefficients that we need to compare mylar vs the average of nylon and
polyethylene in additive 1.

We want to test

which maps to coefficients

::: center
  ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- -----
   Mc1   Mc2   Mc3   Mc4   Mc5   Nc1   Nc2   Nc3   Nc4   Nc5   Pc1   Pc2   Pc3   Pc4   Pc5
                                                                                      
  ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- -----
:::

We can now add the `lsmestimate` statement to our program

            lsmestimate polymer*add 'Mylar vs Nylon,Poly in Add c1' 1 0 0 0 0 -0.5 0 0 0 0 -0.5 0 0 0 0;
            lsmestimate polymer*add 'Mylar vs Nylon,Poly in Add c1' 2 0 0 0 0 -1 0 0 0 0 -1 0 0 0 0/divisor=2;

We could additional statements to look at this same comparison with the
other additives. Note that I'm not looking at this comparison in
Additive c5, because we already saw there is a difference between nylon
and polyethylene in c5 so it doesn't make sense to look at this.

            proc glimmix data=ribbon;
              class polymer add;
              model strength=polymer|add;
              lsmestimate polymer*add 
                'Mylar vs Nylon,Poly in Add c1' 2 0 0 0 0 -1 0 0 0 0 -1 0 0 0 0,
              'Mylar vs Nylon,Poly in Add c2' 0 2 0 0 0 0 -1 0 0 0 0 -1 0 0 0,
                'Mylar vs Nylon,Poly in Add c3' 0 0 2 0 0 0 0 -1 0 0 0 0 -1 0 0,
                'Mylar vs Nylon,Poly in Add c4' 0 0 0 2 0 0 0 0 -1 0 0 0 0 -1 0/divisor=2;
            run;

                                             Least Squares Means Estimates
            
                                                                   Standard
            Effect        Label                           Estimate    Error      DF   t Value   Pr > |t|
            
        polymer*add   Mylar vs Nylon,Poly in Add c1     5.0167     2.2164      75      2.26     0.0265
        polymer*add   Mylar vs Nylon,Poly in Add c2     4.2750     2.2164      75      1.93     0.0575
        polymer*add   Mylar vs Nylon,Poly in Add c3     6.7500     2.2164      75      3.05     0.0032
        polymer*add   Mylar vs Nylon,Poly in Add c4    11.8000     2.2164      75      5.32     <.0001

We see the estimates differ for the different additives, indicating a
potential interaction between Mylar vs (Nylon & Poly) and Additive. We
can investigate this further by comparing the simple effects. But for
this, we need to use `estimate`.

Again the difference between `lsmestimate` and `estimate` is

Using `estimate`, we'll need to go through the 3 step method outlined
above.

1.  State the comparison in terms of the cell means

2.  Convert cell means into treatment effects

3.  Gather like terms

So, our estimate statement will only involve the polymer and
polymer\*additive estimates.

        proc glimmix data=ribbon;
          class polymer add;
          model strength=polymer|add;
          estimate 'Mylar vs Nylon,Poly in Add c1' 
            polymer 2 -1 -1 polymer*add 2 0 0 0 0 -1 0 0 0 0 -1 0 0 0 0/divisor=2;
        run;

which gives

                                               Estimates
        
                                                     Standard
        Label                            Estimate       Error       DF    t Value    Pr > |t|
        
         Mylar vs Nylon,Poly in Add c1      5.0167      2.2164       75       2.26      0.0265
        
        

We could also look at all four comparisons at once like we did with
`lsmestimate` and we could also add the `adjust=` option to control
multiplicity.

        proc glimmix data=ribbon;
          class polymer add;
          model strength=polymer|add;
          estimate 'Mylar vs Nylon,Poly in Add c1' 
              polymer 2 -1 -1 polymer*add 2 0 0 0 0 -1 0 0 0 0 -1 0 0 0 0,
           'Mylar vs Nylon,Poly in Add c2' 
              polymer 2 -1 -1 polymer*add 0 2 0 0 0 0 -1 0 0 0 0 -1 0 0 0,  
           'Mylar vs Nylon,Poly in Add c3' 
              polymer 2 -1 -1 polymer*add 0 0 2 0 0 0 0 -1 0 0 0 0 -1 0 0,
           'Mylar vs Nylon,Poly in Add c4' 
              polymer 2 -1 -1 polymer*add 0 0 0 2 0 0 0 0 -1 0 0 0 0 -1 0/divisor=2;
        run;

These give us the same results we saw with `lsmestimate`! So why bother
going through all of this mess if we could get them more easily with
`lsmestimate`? So now that we know how to build the `estimate`
statements we can do so to explore contrasts to see if there is
significant interaction. We can see there is from the Type III tests,
but interaction now has 8 df--there's a lot more going on with
interaction than there was in a 2 $\times$ 2 model, and we need to tease
it apart. Right now, we're specifically interested in whether there is
interaction between the mylar vs nylon/poly comparison and additive.

Let's write out the null hypothesis for this interaction.

Rather than a single equation for interaction like we saw in the 2
$\times$ 2, we actually have a set of 4 equations. This is a

How can we come up with the 4 equations to see what goes in the contrast
statement?

We'll do the same thing we did in the three step process--write the
means in terms of their treatment effects and gather like terms. Let's
look at c1 vs c5:

So the main effects cancel out and the only term we need in our contrast
statement is polymer\*add. That would give us coefficients:

::: center
  ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- -----
   Mc1   Mc2   Mc3   Mc4   Mc5   Nc1   Nc2   Nc3   Nc4   Nc5   Pc1   Pc2   Pc3   Pc4   Pc5
                                                                                      
  ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- -----
:::

and our contrast statement would look like this:

        proc glimmix data=ribbon;
          class polymer add;
          model strength=polymer|add;
          contrast 'Mylar vs Nylon,Poly by Add interaction'
            polymer*add 1 0 0 0 -1  -0.5 0 0 0 0.5  -0.5 0 0 0 0.5, 
            polymer*add 0 1 0 0 -1  0 -0.5 0 0 0.5  0 -0.5 0 0 0.5,
            polymer*add 0 0 1 0 -1  0 0 -0.5 0 0.5  0 0 -0.5 0 0.5,
            polymer*add 0 0 0 1 -1  0 0 0 -0.5 0.5  0 0 0 -0.5 0.5;
        run;

which gives

                Type III Tests of Fixed Effects
        
                        Num      Den
        Effect           DF       DF    F Value    Pr > F
        
        polymer           2       75      24.25    <.0001
        add               4       75       0.14    0.9648
        polymer*add       8       75       2.38    0.0241
        
        
                                    Contrasts
        
                                                   Num      Den
        Label                                       DF       DF    F Value    Pr > F
        
        Mylar vs Nylon,Poly by Add interaction       4       75       1.76    0.1450
        

So it looks like there is potentially interaction between this
comparison of polymers and additive. If we wanted to present results for
this comparison, we should do it separately for the 5 additives.
