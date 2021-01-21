# Datasets used in publication :

<div id="ref-Collin_2020" class="csl-entry">

Collin, Francois David, Ghislain Durif, Louis Raynal, Eric Lombaert,
Mathieu Gautier, Renaud Vitalis, Jean Michel Marin, and Arnaud Estoup.
2020. “Extending Approximate Bayesian Computation with Supervised
Machine Learning to Infer Demographic History from Genetic Polymorphisms
Using DIYABC Random Forest,” July.
<https://doi.org/10.22541/au.159480722.26357192>.

</div>

## Command lines used

### Prerequisite: Random Seed File

In the case you have more than 32 computing cores at your disposal, you should regenerate the random seeds file :

```bash
./diyabc -p ./ -n "t:<n>" 
```

where `<n>` is the number of your available cores.

### Scenario choice

1. Simulation of a training set with 12000 particles (6 scenarios, 2000 particles per scenario cf. corresponding `header.txt`)

    ```bash
    ./diyabc -p ./ -R ALL -r 12000 -g 50 -m -t 32
    ```

    ---> reftable of 12000 records with all stats, "loopsize" of 50, computed on 32 cores

2. To do a random forest scenario choice from the previously obtained training (1000 trees forest)

   - Without scenario grouping

       ```bash
       ./abcranger -n 12000 -t 1000 -j 32 -o modelchoice_results_no_grouping
       ```

   - With scenario grouping, two groups of 3 scenarios

       ```bash
       ./abcranger -n 12000 -t 1000 -g "1,2,3;4,5,6" -j 32 -o modelchoice_results_with_grouping
       ```

### Parameter estimation

1. Simulation of a training set with 10000 particles for only one scenario cf. best scenario for scenario choice  ( cf. corresponding `header.txt`)

    ```bash
    ./diyabc -p ./ -R ALL -r 10000 -g 50 -m -t 32
    ```

    ---> reftable of 10000 records with all stats, "loopsize" of 50, computed on 32 cores

2. To estimate `ra` parameter of scenario 1 from the obtained training set, with PLS (cf. selection of 95% variance explaining axis)

    ```bash
    ./abcranger -n 10000 -t 1000 -j 32 --parameter ra --chosenscen 1 --plsmaxvar 0.95 --noob 10000 -o estim_param_ra_with_PLS
    ```
