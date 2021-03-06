<h1 align="center"> 
  Gradient-Free-Optimizers
</h1>

<h2 align="center">
  Simple and reliable optimization with local, global, population-based and sequential techniques in numerical search spaces.
</h2>

<br>

<table>
  <tbody>
    <tr align="left" valign="center">
      <td>
        <strong>Master status:</strong>
      </td>
      <td>
        <a href="https://travis-ci.com/SimonBlanke/Gradient-Free-Optimizers">
          <img src="https://img.shields.io/travis/com/SimonBlanke/Gradient-Free-Optimizers/master?style=flat-square&logo=travis" alt="img not loaded: try F5 :)">
        </a>
        <a href="https://coveralls.io/github/SimonBlanke/Gradient-Free-Optimizers">
          <img src="https://img.shields.io/coveralls/github/SimonBlanke/Gradient-Free-Optimizers?style=flat-square&logo=codecov" alt="img not loaded: try F5 :)">
        </a>
      </td>
    </tr>
    <tr/>
    <tr align="left" valign="center">
      <td>
         <strong>Code quality:</strong>
      </td>
      <td>
        <a href="https://codeclimate.com/github/SimonBlanke/Gradient-Free-Optimizers">
        <img src="https://img.shields.io/codeclimate/maintainability/SimonBlanke/Gradient-Free-Optimizers?style=flat-square&logo=code-climate" alt="img not loaded: try F5 :)">
        </a>
        <a href="https://scrutinizer-ci.com/g/SimonBlanke/Gradient-Free-Optimizers/">
        <img src="https://img.shields.io/scrutinizer/quality/g/SimonBlanke/Gradient-Free-Optimizers?style=flat-square&logo=scrutinizer-ci" alt="img not loaded: try F5 :)">
        </a>
      </td>
    </tr>
    <tr/>    <tr align="left" valign="center">
      <td>
        <strong>Latest versions:</strong>
      </td>
      <td>
        <a href="https://pypi.org/project/gradient_free_optimizers/">
          <img src="https://img.shields.io/pypi/v/Gradient-Free-Optimizers?style=flat-square&logo=PyPi&logoColor=white&color=blue" alt="img not loaded: try F5 :)">
        </a>
      </td>
    </tr>
  </tbody>
</table>

<br>

## Introduction

Gradient-Free-Optimizers provides a collection of easy to use optimization techniques, 
whose objective function only requires an arbitrary score that gets maximized. 
This makes gradient-free methods capable of solving various optimization problems, including: 
- Optimizing arbitrary mathematical functions.
- Fitting multiple gauss-distributions to data.
- Hyperparameter-optimization of machine-learning methods.


<br>

---

<div align="center"><a name="menu"></a>
  <h3>
    <a href="https://github.com/SimonBlanke/Gradient-Free-Optimizers#main-features">Main features</a> •
    <a href="https://github.com/SimonBlanke/Gradient-Free-Optimizers#installation">Installation</a> •
    <a href="https://github.com/SimonBlanke/Gradient-Free-Optimizers#examples">Examples</a> •
    <a href="https://github.com/SimonBlanke/Gradient-Free-Optimizers#basic-api-information">API-info</a> •
    <a href="https://github.com/SimonBlanke/Gradient-Free-Optimizers#citation">Citation</a> •
    <a href="https://github.com/SimonBlanke/Gradient-Free-Optimizers#license">License</a>
  </h3>
</div>

---

<br>



## Main features

- Easy to use:
  - <a href="https://github.com/SimonBlanke/Gradient-Free-Optimizers#examples">Simple API-design</a>
  - Receive prepared information about ongoing and finished optimization runs

- High performance:
  - Modern optimization techniques
  - Lightweight backend
  - Save time with "short term memory"

- High reliability:
  - Extensive testing
  - Performance test for each optimizer

<br>

### Optimization strategies:

- Local search
  - Hill Climbing
  - Stochastic Hill Climbing
  - Tabu Search
  - Simulated Annealing

- Global search
  - Random Search
  - Random Restart Hill Climbing
  - Random Annealing

- Population methods
  - Parallel Tempering
  - Particle Swarm Optimization
  - Evolution Strategy

- Sequential methods
  - Bayesian Optimization
  - Tree of Parzen Estimators
  - Decision Tree Optimizer


<br>


## Installation

[![PyPI version](https://badge.fury.io/py/gradient-free-optimizers.svg)](https://badge.fury.io/py/gradient-free-optimizers)

The most recent version of Gradient-Free-Optimizers is available on PyPi:

```console
pip install gradient-free-optimizers
```

<br>


## Examples

<details open>
<summary><b>Convex function</b></summary>

```python
import numpy as np
from gradient_free_optimizers import RandomSearchOptimizer


def parabola_function(para):
    loss = para["x"] * para["x"]
    return -loss


search_space = {"x": np.arange(-10, 10, 0.1)}

opt = RandomSearchOptimizer(search_space)
opt.search(parabola_function, n_iter=100000)
```

</details>


<details>
<summary><b>Non-convex function</b></summary>

```python
import numpy as np
from gradient_free_optimizers import RandomSearchOptimizer


def ackley_function(pos_new):
    x = pos_new["x1"]
    y = pos_new["x2"]

    a1 = -20 * np.exp(-0.2 * np.sqrt(0.5 * (x * x + y * y)))
    a2 = -np.exp(0.5 * (np.cos(2 * np.pi * x) + np.cos(2 * np.pi * y)))
    score = a1 + a2 + 20
    return -score


search_space = {
    "x1": np.arange(-100, 101, 0.1),
    "x2": np.arange(-100, 101, 0.1),
}

opt = RandomSearchOptimizer(search_space)
opt.search(ackley_function, n_iter=30000)
```

</details>


<details>
<summary><b>Machine learning example</b></summary>

```python
import numpy as np
from sklearn.model_selection import cross_val_score
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.datasets import load_wine

from gradient_free_optimizers import HillClimbingOptimizer


data = load_wine()
X, y = data.data, data.target


def model(para):
    gbc = GradientBoostingClassifier(
        n_estimators=para["n_estimators"],
        max_depth=para["max_depth"],
        min_samples_split=para["min_samples_split"],
        min_samples_leaf=para["min_samples_leaf"],
    )
    scores = cross_val_score(gbc, X, y, cv=3)

    return scores.mean()


search_space = {
    "n_estimators": np.arange(20, 120, 1),
    "max_depth": np.arange(2, 12, 1),
    "min_samples_split": np.arange(2, 12, 1),
    "min_samples_leaf": np.arange(1, 12, 1),
}

opt = HillClimbingOptimizer(search_space)
opt.search(model, n_iter=50)
```

</details>


<br>

## Basic API-information

### Optimization classes

<details>
<summary><b> HillClimbingOptimizer</b></summary>

    - search_space
    - epsilon=0.05
    - distribution="normal"
    - n_neighbours=3
    - rand_rest_p=0.03

</details>

<details>
<summary><b> StochasticHillClimbingOptimizer</b></summary>

    - search_space
    - epsilon=0.05
    - distribution="normal"
    - n_neighbours=3
    - rand_rest_p=0.03
    - p_accept=0.1
    - norm_factor="adaptive"

</details>

<details>
<summary><b> TabuOptimizer</b></summary>

    - search_space
    - epsilon=0.05
    - distribution="normal"
    - n_neighbours=3
    - rand_rest_p=0.03
    - tabu_factor=3

</details>

<details>
<summary><b> SimulatedAnnealingOptimizer</b></summary>

    - search_space
    - epsilon=0.05
    - distribution="normal"
    - n_neighbours=3
    - rand_rest_p=0.03
    - p_accept=0.1
    - norm_factor="adaptive"
    - annealing_rate=0.975
    - start_temp=1

</details>

<details>
<summary><b> RandomSearchOptimizer</b></summary>

    - search_space

</details>

<details>
<summary><b> RandomRestartHillClimbingOptimizer</b></summary>

    - search_space
    - epsilon=0.05
    - distribution="normal"
    - n_neighbours=3
    - rand_rest_p=0.03
    - n_iter_restart=10

</details>

<details>
<summary><b> RandomAnnealingOptimizer</b></summary>

    - search_space
    - epsilon=0.05
    - distribution="normal"
    - n_neighbours=3
    - rand_rest_p=0.03
    - annealing_rate=0.975
    - start_temp=1

</details>

<details>
<summary><b> ParallelTemperingOptimizer</b></summary>

    - search_space
    - n_iter_swap=10
    - rand_rest_p=0.03

</details>

<details>
<summary><b> ParticleSwarmOptimizer</b></summary>

    - search_space
    - inertia=0.5
    - cognitive_weight=0.5
    - social_weight=0.5
    - temp_weight=0.2
    - rand_rest_p=0.03

</details>

<details>
<summary><b> EvolutionStrategyOptimizer</b></summary>

    - search_space
    - mutation_rate=0.7
    - crossover_rate=0.3
    - rand_rest_p=0.03

</details>

<details>
<summary><b> BayesianOptimizer</b></summary>

    - search_space
    - gpr=gaussian_process["gp_nonlinear"]
    - xi=0.03
    - warm_start_smbo=None
    - rand_rest_p=0.03

</details>

<details>
<summary><b> TreeStructuredParzenEstimators</b></summary>

    - search_space
    - gamma_tpe=0.5
    - warm_start_smbo=None
    - rand_rest_p=0.03

</details>

<details>
<summary><b> DecisionTreeOptimizer</b></summary>

    - search_space
    - tree_regressor="extra_tree"
    - xi=0.01
    - warm_start_smbo=None
    - rand_rest_p=0.03

</details>

<details>
<summary><b> EnsembleOptimizer</b></summary>

    - search_space
    - estimators=[
            GradientBoostingRegressor(n_estimators=5),
            GaussianProcessRegressor(),
        ]
    - xi=0.01
    - warm_start_smbo=None
    - rand_rest_p=0.03

</details>



### Input parameters

- search_space
  - Pass the search_space to the optimizer class to define the space were the optimization algorithm can search for the best parameters for the given objective function.

    example:
    ```python
    {
      "x1": numpy.arange(-10, 31, 0.3),
      "x2": numpy.arange(-10, 31, 0.3),
    }
    ```

#### Search method parameters

- objective_function
  - (callable)

  - The objective function defines the optimization problem. The optimization algorithm will try to maximize the numerical value that is returned by the objective function by trying out different parameters from the search space.

    example:
    ```python
    def objective_function(para):
        score = -(para["x1"] * para["x1"] + para["x2"] * para["x2"])
        return score
    ```

- n_iter 
  - (int)

  - The number of iterations that will be performed during the optimiation run. The entire iteration consists of the optimization-step, which decides the next parameter that will be evaluated and the evaluation-step, which will run the objective function with the chosen parameter and return the score.

- initialize={"grid": 8, "random": 4, "vertices": 8}
  - (dict, None)

  - The initialization dictionary automatically determines a number of parameters that will be evaluated in the first n iterations (n is the sum of the values in initialize). 

- warm_start=None
  - (list, None)
  - List of parameter dictionaries that marks additional start points for the optimization run.

- max_time=None
  - (float, None)
  - Maximum number of seconds until the optimization stops. The time will be checked after each completed iteration.

- max_score=None
  - (float, None)
  - Maximum score until the optimization stops. The score will be checked after each completed iteration.

- memory=True
  - (bool)
  - Whether or not to use the "memory"-feature. The memory is a dictionary, which gets filled with parameters and scores during the optimization run. If the optimizer encounters a parameter that is already in the dictionary it just extracts the score instead of reevaluating the objective function (which can take a long time).


- memory_warm_start=None
  - (pandas dataframe, None)
  - Pandas dataframe that contains score and paramter information that will be automatically loaded into the memory-dictionary.

      example:

      <table class="table">
        <thead class="table-head">
          <tr class="row">
            <td class="cell">score</td>
            <td class="cell">x1</td>
            <td class="cell">x2</td>
            <td class="cell">x...</td>
          </tr>
        </thead>
        <tbody class="table-body">
          <tr class="row">
            <td class="cell">0.756</td>
            <td class="cell">0.1</td>
            <td class="cell">0.2</td>
            <td class="cell">...</td>
          </tr>
          <tr class="row">
            <td class="cell">0.823</td>
            <td class="cell">0.3</td>
            <td class="cell">0.1</td>
            <td class="cell">...</td>
          </tr>
          <tr class="row">
            <td class="cell">...</td>
            <td class="cell">...</td>
            <td class="cell">...</td>
            <td class="cell">...</td>
          </tr>
          <tr class="row">
            <td class="cell">...</td>
            <td class="cell">...</td>
            <td class="cell">...</td>
            <td class="cell">...</td>
          </tr>
        </tbody>
      </table>



- verbosity={
          "progress_bar": True,
          "print_results": True,
          "print_times": True,
      }
  - (dict, None)
  - The verbosity dictionary determines what part of the optimization information will be printed in the command line.

- random_state=None
    - (int, None)
    - Random state for random processes in the random, numpy and scipy module.





### Results from attributes

  - .results
    - Dataframe, that contains information about the score, the value of each parameter and the evaluation and iteration time. Each row shows the information of one optimization iteration.

      example:

      <table class="table">
        <thead class="table-head">
          <tr class="row">
            <td class="cell">score</td>
            <td class="cell">x1</td>
            <td class="cell">x2</td>
            <td class="cell">x...</td>
            <td class="cell">eval_time</td>
            <td class="cell">iter_time</td>
          </tr>
        </thead>
        <tbody class="table-body">
          <tr class="row">
            <td class="cell">0.756</td>
            <td class="cell">0.1</td>
            <td class="cell">0.2</td>
            <td class="cell">...</td>
            <td class="cell">0.000016</td>
            <td class="cell">0.0.000034</td>
          </tr>
          <tr class="row">
            <td class="cell">0.823</td>
            <td class="cell">0.3</td>
            <td class="cell">0.1</td>
            <td class="cell">...</td>
            <td class="cell">0.000017</td>
            <td class="cell">0.0.000032</td>
          </tr>
          <tr class="row">
            <td class="cell">...</td>
            <td class="cell">...</td>
            <td class="cell">...</td>
            <td class="cell">...</td>
            <td class="cell">...</td>
            <td class="cell">...</td>
          </tr>
          <tr class="row">
            <td class="cell">...</td>
            <td class="cell">...</td>
            <td class="cell">...</td>
            <td class="cell">...</td>
            <td class="cell">...</td>
            <td class="cell">...</td>
          </tr>
        </tbody>
      </table>

  - .best_score
    - numerical value of the best score, that was found during the optimization run.

  - .best_para
    - parameter dictionary of the best score, that was found during the optimization run.

      example:
      ```python
      {
        'x1': 0.2, 
        'x2': 0.3,
      }
      ```
</details>



<br>

### Gradient Free Optimizers <=> Hyperactive

This package was created as the optimization backend of the Hyperactive package.
The separation of Gradient-Free-Optimizers from Hyperactive enables multiple advantages:
  - Other developers can easily use GFOs as an optimizaton backend if desired
  - Separate and more thorough testing
  - Better isolation from the complex information flow in Hyperactive. GFOs only uses positions and scores in a N-dimensional search-space. It returns only the new position after each iteration.
  - a smaller and cleaner code base, if you want to explore my implementation of these optimization techniques.



<br>

## Citation

    @Misc{gfo2020,
      author =   {{Simon Blanke}},
      title =    {{Gradient-Free-Optimizers}: Simple and reliable optimization with local, global, population-based and sequential techniques in numerical search spaces.},
      howpublished = {\url{https://github.com/SimonBlanke}},
      year = {since 2020}
    }


<br>

## License

Gradient-Free-Optimizers is licensed under the following License:

[![LICENSE](https://img.shields.io/github/license/SimonBlanke/Gradient-Free-Optimizers?style=for-the-badge)](https://github.com/SimonBlanke/Gradient-Free-Optimizers/blob/master/LICENSE)


