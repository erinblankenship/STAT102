**STAT 212:** Introduction to Design

> In interpreting and in presenting experimental results there is no
adequate substitute for thought--thought about the questions to be
asked, thought about the nature and weight of evidence the data provide
on those questions, and thought about how the story can be told with
clarity and full honesty to a reader. Statistical techniques must be
chosen and used to aid, but not to replace, relevant thought.    
- Bryan-Jones and Finney, 1983

There are two aspects to any design of experiments problem: the actual
design and the analysis of the data from the experiment. We'll be
discussing both aspects at length is semester in about equal measure.

Experimentation is one of the most common activities that people engage
in, because it allows an investigator to find out what happens to a
response when settings of another variable are purposefully changed. The
results of the experiment provide a basis for selecting optimum settings
or determine a plan of action. Experimentation is carried out by
everyone, in everyday activities. For example, observing what happens to
the taste of brownies if you change the material of the baking dish or
the oven temperature. Changing the time you visit the dining hall to
determine if it is less crowded. Changing a pet's food to see if they
eat better/more. All of these experiments.

Can you think of others?

The goal of STAT 212 is to fully explore the "relevant thought" in the
Bryan-Jones and Finney quote above, and to help you become independent
in this thought. By the end of the semester you should be able to:

1.  Optimally design simple studies to answer a research question.

2.  Compute and interpret the analysis of data resulting from these
    designs.

3.  Communicate the results of your analysis to non-statisticians both
    verbally and in writing.

4.  Converse intelligently about more complex designs with someone who
    has more training.

Well-designed experiments allow an investigator to conduct better
studies more efficiently, analyze data effectively, and make the
connections between the conclusions from the analysis and the original
research objectives. In every experiment, there are seven basic steps:

1.  
2.  
3.  
4.  
5.  
6.  
7.  

Throughout these steps, we must always keeps the following in mind:

1.  
2.  
3.  
4.  

To look at these in more detail, let's consider an example (and state a
few definitions along the way).

**Example:** Two students at Queensland University of Technology, as a
project for their statistics class, carried out an experiment to test
the effect certain factors such as refrigeration, stem length, and water
content have on the life of a cut rose. The students considered

-   Stem length (15 cm or 25 cm)

-   Water content (tap water or tap water + citric acid)

-   Temperature (refrigerated or room temperature)

The response measured was the number of days until death, and the goal
is to determine the conditions that will extend rose life.

In this example, there are 3 **factors**.

**Definition:**

In order to study the effect of the factor on the response, two or more
values of the factor are considered. These values are called **levels**.

A **treatment** is a combination of factor levels. In this experiment:

-   Factors:

-   Levels:

-   Treatments:

**Definition:** The **treatment design**

**Definition:** The **experimental design**

There are three main principles of **experimental design** that must be
addressed with every design. They are:

1.  **Replication:**

    **Why it's important:**

    Each rose in the experiment above, each replicate, is an
    **experimental unit**. **An experimental unit is the smallest
    physical entity to which a treatment can be
    [INDEPENDENTLY]{.underline} applied.**

    The **sampling unit** is the entity that is actually observed or
    measured.

    **Pseudo-replication** occurs when when subsets of the experimental
    units (often sampling units) are mistakenly identified as
    experimental units. The is the MOST COMMON SERIOUS DESIGN ERROR.

    **Examples:**

2.  **Randomization:**

    **Why it's important:**

3.  **Control/Blocking:**

    **Why it's important:**

To make sure everyone's on the same page, let's review the two-sample
case. Back to the example.

**Example:** Let's consider just the data on refrigeration vs room
temperature. There were 16 roses: 8 stored at room temperature and 8
stored in a refrigerator. Let $$\mu_1 = \hspace{80mm}$$
$$\mu_2 = \hspace{80mm}$$

We have observations $$y_{ij} = \hspace{80mm}$$

We can express each observation as a combination of the treatment mean
and the characteristics unique to each individual rose.

In STAT 102/318 we assume:

We can further decompose $\mu_i$:

The Central Limit Theorem says that the sample means
$\overline{y}_{i\cdot}$ will be (approximately) normally distributed
with mean $\mu_i$ and variance $\sigma^2/n$. We also have to estimate
$\sigma^2$, the variance of $e_{ij}$ and hence the variance of $y_{ij}$.

To determine if refrigeration and room temperature produce the same
lifetime of cut roses we can test H$_0: \mu_1 = \mu_2$ or we can
estimate $\mu_1 - \mu_2$ with confidence using $t$-procedures.

We could also use the Analysis of Variance, the ANOVA table would look
like:

::: center
   Source   df   Sum of Sq   Mean Sq       F       p-value
  -------- ---- ----------- --------- ----------- ---------
   Model           SSTrt      MSTrt    MSTrt/MSE  
   Error          SSError      MSE                
   Total          SSTotal                         
:::

There are two mistakes we could make carrying out this test.

-   **Type I Error:**

-   **Type II Error:**

A new concept we'll talk about this semester is closely related to Type
II Error: **power**.

**Definition:** **Power**

All of these concepts of treatment design, experimental design, power,
factors, and levels will be revisited many times over the course of the
semester with increasingly complex treatment and experimental designs.
We'll start with the simplest experimental design: the **Completely
Randomized Design** (CRD).
