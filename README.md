from random import *
import math

a = -512
b = 512


def func(x1, x2):
    y = (-x1 * math.sin(abs(x1 - 47 - x2) ** 0.5) - (x2 + 47) * math.sin(abs((x1 / 2) + 47 + x2) ** 0.5)) * (-1)
    return y


size_item = 40
size_population = 20
generations = 50
mutation = 0.03
ymin = 959.64067
Ey1 = 0
n = 100


def fitness_function(bin_list):
    global a, b, size_item
    s = ''.join([str(b) for b in bin_list[:len(bin_list) // 2]])
    d = int(s, 2)
    k = d / (2 ** (size_item // 2) - 1)
    x1 = k * (b - a) + a
    s = ''.join([str(b) for b in bin_list[len(bin_list) // 2:]])
    d = int(s, 2)
    k = d / (2 ** (size_item // 2) - 1)
    x2 = k * (b - a) + a
    return func(x1, x2)


for o2 in range(n):
    population = []
    fitness = []
    best_item = None
    best_fitness = None
    for p in range(size_population):
        item = choices([0, 1], k=size_item)
        population.append(item)
        f = fitness_function(item)
        fitness.append(f)
        if best_fitness is None or f > best_fitness:
            best_fitness = f
            best_item = item
    for j1 in range(generations - 1):
        population_child = []
        fitness_child = []
        for j in range(size_population):
            i_parent1 = 0
            i_parent2 = 0
            i_player1 = randint(0, size_population - 1)
            i_player2 = randint(0, size_population - 1)
            if fitness[i_player1] > fitness[i_player2]:
                i_parent1 = i_player1
            else:
                i_parent1 = i_player2

            i_player1 = randint(0, size_population - 1)
            i_player2 = randint(0, size_population - 1)
            if fitness[i_player1] > fitness[i_player2]:
                i_parent2 = i_player1
            else:
                i_parent2 = i_player2
            parent1 = population[i_parent1]
            parent2 = population[i_parent2]

            r = randint(0, size_item - 1)
            child = parent1[:r + 1] + parent2[r + 1:]

            for i1 in range(len(child)):
                if random() <= mutation:
                    if child[i1] == 1:
                        child[i1] = 0
                    else:
                        child[i1] = 1
            population_child.append(child)
            f = fitness_function(child)
            fitness_child.append(f)
            if f > best_fitness:
                best_fitness = f
                best_item = child
        population = population_child
        fitness = fitness_child
    s = ''.join([str(b) for b in best_item[:len(best_item) // 2]])
    d = int(s, 2)
    k = d / (2 ** (size_item // 2) - 1)
    x1 = k * (b - a) + a
    s = ''.join([str(b) for b in best_item[len(best_item) // 2:]])
    d = int(s, 2)
    k = d / (2 ** (size_item // 2) - 1)
    x2 = k * (b - a) + a
    Ey1 += (ymin - best_fitness) ** 2
print(f'y = {best_fitness}')
print(f'x1 = {x1}')
print(f'x2 = {x2}')
Ey = (Ey1 / n) ** 0.5
print(f'E = {Ey}')
