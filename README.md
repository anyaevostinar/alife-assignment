# Artificial Life

[Artificial Life](https://en.wikipedia.org/wiki/Artificial_life) is a field of computer science that uses the power of computers to gain understanding about the fundamental processes of living systems. Research in Artificial Life (or A-Life) often uses highly complex object-oriented software programs to model properties of a living organism. However, the necessary elements for an artificial life simulation are actually fairly easy. In this assignment, you will be using your skills in object-oriented programming to make an artificial life simulation of cooperation between bacteria. 

Bacteria actually cooperate in [lots of interesting ways.](https://en.wikipedia.org/wiki/Microbial_cooperation) Because they are so small, however, it is difficult for biologists to figure out exactly what each individual is doing. Instead, we have a good understanding of the large-scale patterns seen in bacterial colonies. This topic is therefore great for a computer simulation, because you can test out hypotheses about what individual organisms are doing and compare your large-scale results to what we already know is happening in biological systems.

## Implementing Artificial Life

The fundamental pieces of an artificial life simulation consist of only two classes:

The `Population` class is how you represent your virtual petri dish. It keeps track of how long the experiment has been running and what sort of environment your digital bacteria are attempting to live in. It also has methods for collecting statistical information about your experiment. The Population class will hold a list of all of your organisms and at every "time point" or *tick* it will loop over those organisms and allow them to perform one action, represented by an `update` method.

The `Organism` class is how you represent a single individual organism. This project is one where object-oriented programming is a powerful paradigm, because you will have hundreds of instances of Organisms, all with different values for their variables. The `Organism` class holds methods for determining what instance is going to do at any given time (in our project, cooperate or not) and instructions for reproduction if that instance has gotten enough resources. Each organism has an "energy level" that measures how much energy that organism has currently. When the organism has enough energy, it is able to reproduce, which uses up the energy required to reproduce. You will give each organism one new energy at each update.

## Cooperation

You are going to use your artificial life simulation to study the dynamics of cooperation between bacteria of the same species. Your `Organism` classes will track the likelihood that it cooperates with other organisms, its *cooperation probability*. When an organism has a chance to act, *i.e.* when its `update` method is called. You will use that cooperation probability number to determine if the organism cooperates with its neighbors. In this project, the cooperation probability will be either 1 (100%) or 0 (0%). If the organism's probability is 1, it will cooperate with organisms around it and if it is 0, it does not cooperate.

When an organism cooperates, it *loses one of its energy units and in exchange it gives eight other organisms an energy unit.*

## The Population Class

You will need to make two classes to achieve the above results. The first class you will need to make is the Population class. Your class should have the following constructor and methods:

* `Population(Map<String, Integer> counts)`: constructs a population of organisms dictated by the given mapping from (case-sensitive) names of organisms to counts. Organisms not mentioned in the mapping do not appear in the population. Throws an `IllegalArgumentException` if the mapping mentions organism types that do not exist in the program.
* `void update()`: loops through all the organisms in the population and (1) updates them (by calling their `update` method), (2) checks to see if they cooperate as described above and (3) checks to see if they reproduce. We check to see if an organism reproduces by first checking its energy level. If the organism has at least 10 energy units, then it reproduces and that new organism replaces a random organism in the population.
* `double calculateCooperationMean()`: calculates the *mean cooperation probability* of all the organisms in the population—the average of the cooperation probabilities of all the organisms in the population.
* `Map<String, Integer> getPopulationCounts()`: returns the counts of all the organisms in the population.

Note that when implementing `update()` that you should respect *encapsulation* of data between the population and organism class. Furthermore, you should also be aware of `ConcurrentModificationException`s in your code. Such an exception occurs whenever you add or remove from a list that you are currently iterating over with an iterator. You will need to be careful with how you design your iteration pattern in `update` to avoid this problem.

## The Organism Class 
The second class you need to make is the Organism class, which will represent individual bacterium in your population. It should have the following constructor and methods:
* `Organism()`: construct an `Organism`, initializing the organism-specific state of this object. By default, an organism starts with zero energy.
* `void update()`: updates the given organism. By default, an organism gains one new energy point.
* `int getEnergy()`: returns the current energy of this organism.
* `void incrementEnergy()`: increments this organism's energy by 1.
* `void decrementEnergy()`: decrements this organism's energy by 1. An organism's energy cannot be decremented below 0.
* `abstract String getType()`: returns the type of this Organism as a string.
* `abstract Organism reproduce()`: called by update when the organism can reproduce. Creates an offspring organism and returns it. Decreases organism's energy by 10.
* `abstract double getCooperationProbability()`: returns the cooperation probability of this organism.
* `abstract boolean cooperates()`: returns whether or not the organism cooperates.

Note that no `Organism` objects themselves actually exist as they are an abstract concept in this program; you should only create instances of sub-classes of the `Organism` class described below. Also note which methods are marked `abstract` above and which provide some sort of default implementation.

## Organism Sub-Classes

We will use inheritance to differentiate the behavior of the different organisms in our system. In your program, implement the following sub-classes of `Organism`:
* `Cooperator`: a type of organism ("Cooperator") that always cooperates with its peers.
* `Defector`: a type of organism ("Defector") that never cooperates with its peers.
* `PartialCooperator`: a type of organism ("PartialCooperator") that cooperates with probability 0.5.

## The ALifeSim Class

Finally, write a driver class `ALifeSim` that creates a simulation with parameters provided from the command-line and runs that simulation for a particular number of iterations. Your `ALifeSim` class should contain `main` method that accepts command-line arguments of the following form:  

`java ALifeSim <#/iterations> <#/cooperators> <#/defectors> <#/partial cooperators>`

Your `ALifeSim` simulates the given population for the given number of iterations of the simulation. It then reports the final counts of organisms and mean cooperation probability for the population in the following format:

`After <#/iterations> ticks:
Cooperators = <final #/cooperators>
Defectors   = <final #/defectors>
Partial     = <final #/partial cooperators>

Mean Cooperation Probability = <mean cooperation probability>`

## Report

Finally, you will use your program to perform a number of experiments with artificial life and write a short essay on the results.

You should use the following settings:
* Run all experiments for 100 updates 
* Test population sizes of 10 and 100 
* Test starting populations with 1 cooperator and the rest defectors; 1 defector and the rest cooperators; and 1/3 cooperators, 1/3 defectors and 1/3 partial cooperators.
* Therefore you should run 6 different experiments for each population size and each population starting numbers. 
* Run all experiments 10 times with different random number seeds. 
* For all experiments you will be reporting the average cooperation value of the population at the end of the experiment.

For each experiment you should write a paragraph reporting: 
* A prediciton and why you have that prediction 
* The average cooperation mean for each replicate and the average of averages. 
* Whether or not the results support your prediction and why.

Finally, you should write a concluding paragraph that summarizes the results you found. What insights can you make regarding what lets cooperators succeed in this simulation versus go extinct?

All together this report should be at least a page in length. You may want to test other population sizes and starting proportions to get a better idea of what dynamics are happening.

## Bonus

You will receive 1 point back on previous homework assignments for each of the bonus items you complete and write a half-page addition to your report.
* Add mutation: In real artificial life simulations we allow for mutation when the organism reproduce. Add to the `reproduce` method that an offspring has a small percentage chance of being of a different subtype. Predict and test how this changes your previous results.
* Different costs and benefits: Pick 2 more settings for the benefit/cost of cooperation. Predict and test how that changes your previous results.
