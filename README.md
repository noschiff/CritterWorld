# Critter World: Simulating Evolving Artificial Life
### Created by Noah Schiff, Kevin Cui, and Bryan Lee for CS 2112 at Cornell University

*[CS 2112: Honors Object Oriented Design and Data Structures](https://www.cs.cornell.edu/courses/cs2112/2021fa/) was taught in Java by Professor Andrew Myers in Fall 2021*

## Project Overview
The goal of this project was to simulate and display a world of self-controlling artificial life forms (critters) capable of interacting with each other and the environment.

The project was naturally split into three primary steps:
1. create a recursive descent parser for the critter language to store the program in an abstract syntax tree
2. simulate the critter world using a controller and interpreter for the abstract syntax tree
3. build a graphical user interface in JavaFX to display the world and control simulation

In short, critters exist on a regular hexagonal grid. Critters require energy to perform any function, such as attacking, reproducing, and even eating. When critters run out of energy, they die and become food for other critters. If critters accumulate enough energy, they can reproduce. The genome of a critter consists of a program that determines what the critter does in each time step. Programs are written according to a context-free grammar with 16 different production rules. The program includes rules defining what critters should do under certain conditions. When critters reproduce, their genome can be copied and mutated, and over time, this will result in evolution. View the [project specifications](https://www.cs.cornell.edu/courses/cs2112/2021fa/project/project.pdf) for more information about the critters, their rule language (grammar), the world, mutations that contribute to evolution, and the requirements for the GUI.

*Note: in accordance with Cornell University's Academic Integrity Policy, we cannot show any of our code or explain our solutions to required tasks. Thus, this page will only provide a demonstration of our graphic user interface and highlight some features that we are especially proud of. The capabilities of our project imply we were successful in achieving our goals without revealing the manner in which we did so.*

## Graphic User Interface and Simulation
Our GUI was written using JavaFX and was designed with user experience as the utmost priority behind functionality.

<p align="center">
<img width="862" alt="Picture of GUI" src="https://user-images.githubusercontent.com/40209975/146696510-a0606dac-d576-48c9-8ed6-fabecc461a4c.png">
</p>

As seen above, our GUI used a muted, pastel color scheme and was separated into three sections. The leftmost section contains the controls and details of the simulation, allowing a user to load in a new world, set the simulation speed, toggle features related to the world, view information about the current world state, and pop out a help menu. The middle section displays a view of the simulation as well as two buttons to zoom in and out. Further, the middle view supports scrolling, enabling a user to better navigate large worlds. Finally, the rightmost section has information about a highlighted critter, including its attributes, last rule/action executed, and program (a.k.a. its genome or brain). This section is collapsible and is only shown when a user clicks on a hexagon containing a critter.

In our application, we represent critters as colored circles with an arrow indicating the direction that they are facing. Critters of different species are drawn with different colors, while critters of species with the same name have the same color. Our use of species-specific colors allows the user to clearly see the evolution of the critter species over time, as the dominant colored species may change over time. Food is represented as a yellow-orange hexagon labeled by the amount of food at that location. Rocks and out-of-bound spaces are represented as gray hexagons. Finally, empty spaces in the world are represented as light green hexagons.

In action, a simulation of the critter world running continuously looks like this:

<p align="center">
<img width="750" alt = "GUI in action"src="https://user-images.githubusercontent.com/40209975/146692687-536df529-c7bb-46c3-a8b4-93eef1dd1b8b.gif">
</p>

### Simulation
The application runs on a main thread—the **JavaFX Application Thread**. All updates to the screen and handling of user input had to be done on this thread. If we were not careful, we could ask this thread to do too much, resulting in the application lagging and being unresponsive. The simulation speed indicates how many turns are drawn per second, and it can be set anywhere from 0 all the way up to 1,000,000. Our rendering of the view is hard-capped at 30 frames per second to prevent the application from lagging when trying to draw too many frames. Thus, when the speed was greater than 30, we needed to ensure all the desired turns were run and only up to 30 frames were drawn while maintaining the smoothness of the simulation. An ineffective solution might result in a uneven amount of updates to the view per frame or might cause the screen to lag. Additionally, if the drawing action "fell behind" the real state of the world, the view may try to display an invalid world or one that no longer exists in memory.

We solved these issues with a careful design, using **multithreading** and **synchronization** techniques. First, we split up the drawing and simulation onto different threads; all simulation work was done on background threads. We used JavaFX's **concurrent** package to manage the additional threads, ensuring only one turn was simulated at a time. Second, the view was rendered from a read-only clone of the world so the simulation could be independent of the rendering. Finally, we encountered issues in which sometimes an excess of user inputs, such as dragging the screen or resizing the application, could still cause the rendering to fall behind. We solved this by keeping our view up to date with the simulation rather than drawing every frame. This allowed us to avoid interfering with the simulation and thus reach much higher speeds, which kept the GUI stayed clean and smooth.

### ASCII Art vs GUI
Due to the nature of the project assignment, we had to create our world simulation before the GUI. It would be nearly impossible to debug the simulation without some form of visual representation, so for that reason, we also created a basic ASCII art representation of the world that was displayed in the console. Because we implemented our code with a MVC (model-view-controller) design pattern in mind, doing this was very easy. Below is a side-by-side comparison of the same critter world, the left being the ASCII art version and the right being our final GUI representation.

<p align = "center">
  <img width="179" height = "330" alt="ASCII Art" src="https://user-images.githubusercontent.com/40209975/146839109-e697e023-2154-47e1-92c7-47316346b367.png">
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  <img width="400" height = "330" alt="GUI Parallel" src="https://user-images.githubusercontent.com/40209975/146839149-1bbeefcb-328b-45f8-ad5f-baa42475547f.png">
</p>  
</br>

## Example Critters
Another aspect of the project was to write programs for specific critter behavior. These programs would be parsed into an AST when they were loaded in, and this AST would be interpreted at each time step, determining the action the critter should take at that given time. 

One such program we wrote was for a critter that moved in a growing hexagon spiral. Theoretically, in an infinite rock-less world, this critter would eventually reach every possible spot. Below is a demonstration of our critter in action.

<p align = "center">
  <img width = "750" alt = "Spiral Demo" src =https://user-images.githubusercontent.com/40209975/146841174-fcfbad6c-b772-41f5-a00b-41b4e01c1d32.gif>
</p>

Here is another critter, named "Ruthless Ricky", that wanders around the world and attacks other critters of different species. Below is its program followed by a demo of it in a small world with other critters (Ricky is the purple critter).
<p align = "center">
  <img width="800" alt="Ruthless Program" src="https://user-images.githubusercontent.com/40209975/146858908-400e4295-c56a-4c6d-9cc2-29eda71f9422.png">
  <img width = "750" alt="Ruthless Demo" src = "https://user-images.githubusercontent.com/40209975/146855978-6340ef0e-1941-446e-ada8-2af863c88fd7.gif">
</p>

And finally, we wrote a critter named "Bouncing Borat" that bounces back and forth. Its program is below, along with a demo of two Borats bouncing.

<p align = "center">
  <img width="503" alt="Bounce Program" src="https://user-images.githubusercontent.com/40209975/146858719-351327bc-d031-4cdd-b291-5f66be3b3804.png">
  <img width = "750" alt = "Double Bounce" src = "https://user-images.githubusercontent.com/40209975/146858263-533f3021-5e18-41f7-af10-b7674bf69ecf.gif">
</p>

## Testing
We tested the edge cases of the GUI by clicking all over the place in the GUI and trying actively to break it. Besides testing out the features and edge cases ourselves, we also conducted alpha and beta-testing with friends and family. Using their feedback, we made functionality more user-friendly and flattened the learning curve to use our system. In this way, testing the GUI was relatively straightforward.

Testing that parsing and interpreting were working, however, was significantly more challenging and time-consuming. This is where the bulk of our JUnit testing took place, as we created corner case unit tests programs and asserted that they maintained our abstract syntax tree invariant when parsed. We also used parameterized testing to create many thousand random programs and parse them, checking that each parsed program generated a valid AST. 

### Visualising the Program
Beyond manually creating parse trees, we also developed a visualizer to make the AST (abstract syntax tree) testing process easier. Below is the visualization of the AST of "Bouncing Borat" shown in the previous section.

<img width="1440" alt="Bounce AST" src="https://user-images.githubusercontent.com/40209975/146847379-697379d2-ea54-46d5-b95c-fe45f4245811.png">

### Fault Injection
Besides the visualizer, another way we tested the AST was using fault injection that was built into critter simulation logic. As context, when critters reproduce, there is a chance their genome, or program, will mutate. There are various kinds of mutation, but regardless, all of them are valid changes to an AST and should not break our AST invariant. Thus as extra reinforcement to our tests, we parsed in a known valid program, recursively called random mutations, and ensured that at each step of the way, we had a valid AST. Through this extensive testing, we became confident that all of our components were working and that our critter world simulation worked seamlessly.

___
*Special thanks to Kevin Cui for helping to write this.* 
