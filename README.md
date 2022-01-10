# Critter World: Simulating Evolving Artificial Life
### Created by Noah Schiff, Kevin Cui, and Bryan Lee for CS 2112 at Cornell University

*[CS 2112: Honors Object Oriented Design and Data Structures](https://www.cs.cornell.edu/courses/cs2112/2021fa/) was taught in Java by Professor Andrew Myers in Fall 2021*

## Project Overview
The goal of this project was to simulate and display a world of self-controlling artificial life forms (critters) capable of interacting with eachother and the environment.

The project was naturally split into three primary steps:
1. create a recursive descent a parser for the critter language
2. simulate the critter world using a controller and interpreter for the critter langauge
3. build a graphical user interface in JavaFX

In short, critters exist on a regular hexagonal grid. Criters require energy to perform any function, such as attacking, reproducing, and even eating. When critters run out of energy, they die and become food for other critters. If critters accumulate
enough energy, they can reproduce. The genome of a critter consists of a program that determines what the critter does in each time step. Programs are written according to a context-free grammar with 16 different production rules.  The program includes rules defining what critters should do
under certain conditions. When critters reproduce, their genome can be copied and mutated, and over time, this will result in evolution.  View the [project specifications](https://www.cs.cornell.edu/courses/cs2112/2021fa/project/project.pdf) for more information about the critters, thier rule language (grammar), the world, mutations that contribute to evolution, and the requirements for the GUI.

*Note: in accordance with Cornell University's Academic Integrity Policy, we cannot show any of our code or explain our solutions to required tasks. Thus, this page will only provide a demonstration of our graphic user interface and highlight some features that we are especially proud of.  The capabilities of our project imply we were successful in achieving our goals without revealing the manner in which we did so.*
