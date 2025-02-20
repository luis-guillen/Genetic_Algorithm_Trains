Based on the information gathered, here is a draft README for your project:

---

# Genetic Algorithm for Train Scheduling

This project implements a genetic algorithm to optimize the scheduling of train arrivals at various operational docks. The goal is to minimize the total waiting time and penalties incurred by trains waiting for their turn at a dock.

## Table of Contents
- [Introduction](#introduction)
- [Installation](#installation)
- [Usage](#usage)
- [Algorithm Details](#algorithm-details)
  - [Fitness Function](#fitness-function)
  - [Mutation Function](#mutation-function)
  - [Crossover Function](#crossover-function)
- [Examples](#examples)
- [Contributing](#contributing)
- [License](#license)

## Introduction

This project uses a genetic algorithm to find an optimal sequence for train arrivals at operational docks. The algorithm aims to minimize the waiting time and penalties for each train based on the number of wagons and the type of operation (gas, coal, containers).

## Installation

To run this project, you need to have Python installed along with the following libraries:
- `numpy`
- `deap`

You can install these dependencies using pip:

```bash
pip install numpy deap
```

## Usage

1. Clone the repository:
   ```bash
   git clone https://github.com/luis-guillen/Genetic_Algorithm_Trains.git
   cd Genetic_Algorithm_Trains
   ```

2. Open the Jupyter Notebook `genetic_algorithm_trains.ipynb` to explore and run the code.

3. (Optional) Run the notebook using Jupyter:
   ```bash
   jupyter notebook genetic_algorithm_trains.ipynb
   ```

## Algorithm Details

### Fitness Function

The fitness function calculates the total waiting time and penalties for a given sequence of train arrivals. It considers the number of wagons and the type of operation for each train.

```python
def evaluacion(individuo):
    contadores_operaciones = {"gas": 0, "carbon": 0, "contenedores": 0}
    tiempo = 0

    for tren in individuo:
        proxima_demora = tren.numero_vagones
        if contadores_operaciones[tren.operacion] == 0:
            contadores_operaciones[tren.operacion] = proxima_demora
        else:
            demora_actual = contadores_operaciones[tren.operacion]
            for k, v in contadores_operaciones.items():
                contadores_operaciones[k] = max(0, v - demora_actual)
            contadores_operaciones[tren.operacion] = proxima_demora
            tiempo += demora_actual

    tiempo += max(contadores_operaciones.values())
    return tiempo,
```

### Mutation Function

The mutation function randomly swaps two trains in the sequence to introduce variability.

```python
def funcion_mutacion(individuo):
    indices = np.random.choice(len(individuo), 2, replace=False)
    indice1 = indices[0]
    indice2 = indices[1]
    
    individuo[indice1], individuo[indice2] = individuo[indice2], individuo[indice1]
    return individuo,
```

### Crossover Function

The crossover function exchanges sub-sequences between two individuals to create offspring.

```python
def funcion_cruce(individuo1, individuo2):
    indice = np.random.randint(len(individuo1))
    izquierda_individuo1 = individuo1[:indice]
    derecha_individuo1 = individuo1[indice:]
    izquierda_individuo2 = individuo2[:indice]
    derecha_individuo2 = individuo2[indice:]

    nuevo_derecha_individuo1 = [t if t not in izquierda_individuo1 else diferencia1.pop() for t in derecha_individuo2]
    nuevo_derecha_individuo2 = [t if t not in izquierda_individuo2 else diferencia2.pop() for t in derecha_individuo1]

    nuevo_individuo1 = izquierda_individuo1 + nuevo_derecha_individuo2
    nuevo_individuo2 = izquierda_individuo2 + nuevo_derecha_individuo1
    
    individuo1[:] = nuevo_individuo1
    individuo2[:] = nuevo_individuo2
    return individuo1, individuo2
```

## Examples

You can find examples of running the genetic algorithm in the provided Jupyter Notebook. The notebook includes various examples of initializing the population, running the genetic algorithm, and interpreting the results.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request.

## License

This project is licensed under the MIT License.

---

Feel free to modify the content as needed.
