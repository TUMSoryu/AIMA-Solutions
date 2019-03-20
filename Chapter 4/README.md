# Chapter 4 - Informed Search Methods #
[Flashcards for chapter summary](http://www.cram.com/flashcards/chapter-4-informed-search-methods-7509345)

To be completed:
4.13, 4.14, 4.15, 4.16, 4.17, 4.18

## Exercise 4.1 ##
Suppose that we run a greedy search algorithm with h(n) = —g(n). What sort of search will
the greedy search emulate?

### Solution ###
This would be similar to a depth first search where the depth is based on the distance
travelled so far rather than than the number of nodes


## Exercise 4.2 ##
Come up with heuristics for the following problems. Explain whether they are admissible,
and whether the state spaces contain local maxima with your heuristic:

1. The general case of the chain problem (i.e., with an arbitrary goal state) from Exercise 3.15.
2. Algebraic equation solving (e.g., "solve x**2 * y**3 = 3 — x*y for x").
3. Path planning in the plane with rectangular obstacles (see also Exercise 4.13).
4. Maze problems, as defined in Chapter 3.

### Solution ###
Heuristic Function h(n) returns estimate of the cost to the goal from the current state
Admissible - Path cost plus heuristic for n' must always be greater than for n
Local Maxima - Can reach points where all operators results in worse improvement

1. h(n) = Number of links - Length of the longest chain. Admissible, no local maxima
2. h(n) = Absolute value of equation. Not admissible, can have local maxima depending on formula and number of variables
3. h(n) = g(n) + straight line distance to goal. Admissible, no local maxima assuming goal is reachable (A* search)
4. h(n) = Number of unique squares visited. Admissible, no local maxima (although no efficient)


## Exercise 4.3 ##
Consider the problem of constructing crossword puzzles: fitting words into a grid of
intersecting horizontal and vertical squares. Assume that a list of words (i.e., a dictionary) is
provided, and that the task is to fill in the squares using any subset of this list. Go through a
complete goal and problem formulation for this domain, and choose a search strategy to solve it.
Specify the heuristic function, if you think one is needed.

### Solution ###
  * Formulation - For each empty length of squares place in a word that allows all squares to be filled with valid words
  * Problem Formulation - Crossword shape, (word lengths, intersection points), dictionary
  * Search Strategy - Least constraining variable (treat as a CSP)
  * Heuristic - Not needed with CSP as each variable holds a subset of available remaining words from the dictionary. If
not using this then could use something like h(n) = Empty Squares which would be admissible but have local maxima


## Exercise 4.4 ##
Sometimes there is no good evaluation function for a problem, but there is a good comparison
method: a way to tell if one node is better than another, without assigning numerical values to
either. Show that this is enough to do a best-first search. What properties of best-first search do
we give up if we only have a comparison method?

### Solution ###
Best First Search is defined as performing a search where the nodes queued to explore are ordered by an
evaluation function. In order to sort you only require a Boolean check whether a > b, which is the same as
non-numerical ranking. An example is the game Hotter Colder is a search using a non-numerical evaluation function.
It gives up the ability to objective state how much more likely a path is to lead to a goal, or how close we
are to the goal


## Exercise 4.5 ##
We saw on page 95 that the straight-line distance heuristic is misleading on the problem of
going from Iasi to Fagaras. However, the heuristic is perfect on the opposite problem: going from
Fagaras to Iasi. Are there problems for which the heuristic is misleading in both directions?

### Solution ###
If you had a search space that looked like a crescent moon where the tips almost touched, and you were trying to
travel from just inside the tip of one side to just inside the tip of the other, then both sides would want to head
towards the tip (and around the outside of the moon), whereas the shortest path is initially stepping away and moving
around the inside path


## Exercise 4.6 ##
Invent a heuristic function for the 8-puzzle that sometimes overestimates, and show how it
can lead to a suboptimal solution on a particular problem

### Solution ###
h(n) = Manhattan Distance + tile value
Would make the agent much more likely to move smaller valued tiles first, which can still lead to a solution, but
could be suboptimal


## Exercise 4.7 ##
Prove that if the heuristic function h obeys the triangle inequality, then the f-cost along any
path in the search tree is non-decreasing. (The triangle inequality says that the sum of the costs
from A to B and B to C must not be less than the cost from A to C directly.)

### Solution ###
A triangle is made up of three lines and angles adding to 180 degrees, if you flatten two sides and extend the hypotenuse
you can get a triangle with angles 0, 180, 0, where two lines sit on top of the hypotenuse. In this position A + B = C,
If you change the edge angles in order to keep it a triangle you have to both extend A or B, and reduce C. This means
every triangle has a value A + B >= C. If you replace A, B and C with locations, the hypotenuse is the SLD, so all
possible paths from A to the goal must be equal to (B is on the line A, B, C) or greater than the distance A->C
according to the same principles. This means that the heuristic function using SLD can only provide an estimate that
is equal to or under the actual cost, making it an admissible heuristic


## Exercise 4.8 ##
We showed in the chapter that an admissible heuristic heuristic (when combined with path-max)
leads to monotonically non-decreasing f values along any path (i.e., f(successor(n)) > f(n)).
Does the implication go the other way? That is, does monotonicity in f imply admissibility of the associated h?

### Solution ###
No. Admissibility is based on the difference between the actual cost to the goal always being greater than the
estimated cost. A counter example would be f(successor(n)) = f(n) + g(n->successor(n)) + 1,000,000. This is monotonic
but not an admissible heuristic


## Exercise 4.9 ##
We gave two simple heuristics for the 8-puzzle: Manhattan distance and misplaced tiles.
Several heuristics in the literature purport to be better than either of these. (See, for example, Nilsson
(1971) for additional improvements on Manhattan distance, and Mostow and Prieditis (1989)
for heuristics derived by semi-mechanical methods.) Test these claims by implementing the
heuristics and comparing the performance of the resulting algorithms.

### Solution ###
See 8_puzzle.py for Env and informed_search.py for A* search

Average Solve Times (nodes searched: moves taken)

  * [Sequence Score](http://www.cse.buffalo.edu/~rapaport/572/S02/nilsson.8puzzle.pdf): (1063: 20), (1619: 14), (3927: 25), (703: 21), (5235: 28) ~ (2509: 21.6 B* = 1.437)
  * Manhattan Distance: (249: 17), (42: 13), (963: 17), (1583: 22), (1516: 22) ~ (871: 18 B* = 1.457)
  * Euclidean Distance: (1091: 16), (1136: 21), (5153: 22), (2503: 18), (7809: 24) ~ (3538: 20 B* = 1.505)
  * Misplaced Tiles: (3769: 18), (363: 12), (245: 11), (6314: 19), (15302: 21) ~ (5198: 16 B* = 1.707)


## Exercise 4.10 ##
Would a bidirectional A* search be a good idea? Under what conditions would it be applicable?
Describe how the algorithm would work.

### Solution ###
It could work but would involved calculating the distance to all current expanded nodes on the other side and
expanding the node on it's side that minimises that distance. If there's a bottle neck before a curve in the
solution path it could work well. Essentially you would run 2 A* where the evaluation function references the
expand node list of the other, and loops through all expanded nodes of the other search to determine which
node on it's side to expand next


## Exercise 4.11 ##
The travelling salesperson problem (TSP) can be solved using the minimum spanning tree (MST)
heuristic, which is used to estimate the cost of completing a tour, given that a partial tour
has already been constructed. The MST cost of a set of cities is the smallest sum of the link costs
of any tree that connects all the cities.

1. Show how this heuristic can be derived using a relaxed version of the TSP.
2. Show that the MST heuristic dominates straight-line distance.
3. Write a problem generator for instances of the TSP where cities are represented by random
points in the unit square.
4. Find an efficient algorithm in the literature for constructing the MST, and use it with an
admissible search algorithm to solve instances of the TSP.

### Solution ###
1. The TSP can be defined as a traveller having to visit x cities, where each city is only visited once and they
end up in their original city, with the goal to minimise the length travelled. MST is the same problem with the
relaxations that cities can be visited more than once, and the cost to travel an already travelled path is 0
2. MST dominates straight-line because it is is based on the minimum connection distance, making it an admissible heuristic
3. See TravellingSalesman in travelling_salesman.py
4. MST Algorithm in mst.py based on [this](http://people.csail.mit.edu/rivest/mst.py). See travelling_salesman.py
for TSP solver.


## Exercise 4.12 ##
Implement the n-queens problem and solve it using hill-climbing, hill-climbing with
random restart, and simulated annealing. Measure the search cost and percentage of solved
problems using randomly generated start states. Graph these against the difficulty of the problems
(as measured by the optimal solution length). Comment on your results

### Solution ###
Tested over 100 trials of each (limited at 100 steps)
98% solved in 20.1 moves - Hill Climbing No Sideways Movement Random Restart ~= (1-0.12)^(100/5.3)
86% solved in 20.4 moves - Hill Climbing w/ Sideways Movement
14% solved in 22.8 moves - Simulated Annealing (T: 1, A: 0.95)
12% solved in 5.3 moves - Hill Climbing No Sideways Movement
7% solved in 24 moves - Simulated Annealing (T: 1, A: 0.98)

Not compared to optimal solutions because code to determine those is too slow to run 100 times per env


## Exercise 4.13 ##
Consider the problem of finding the shortest path between two points on a plane that has
convex polygonal obstacles as shown in Figure 4.17. This is an idealization of the problem that
a robot has to solve to navigate its way around a crowded environment.

1. Suppose the state space consists of all positions (x,y) in the plane. How many states are
there? How many paths are there to the goal?
2. Explain briefly why the shortest path from one polygon vertex to any other in the scene
must consist of straight-line segments joining some of the vertices of the polygons. Define
a good state space now. How large is this state space?
3. Define the necessary functions to implement the search problem, including a successor
function that takes a vertex as input and returns the set of vertices that can be reached in
a straight line from the given vertex. (Do not forget the neighbors on the same polygon.)
Use the straight-line distance for the heuristic function.
4. Implement any of the admissible algorithms discussed in the chapter. Keep the implementation
of the algorithm completely independent of the specific domain. Then apply it to
solve various problem instances in the domain.

### Solution ### INCOMPLETE
1. x * y possible states. Infinite because you can have loops
2. Because the shortest distance between two points is a straight line, so the shortest distance between two points
with an obstacle in the way is to wrap the path as close to a straight lines around those obstacles as possible.
A good state space is therefore the points along the polygons plus the start and goal as all moves will involve
starting at ending on one of these points. State Space = sum(Polygon Perimeters) + 2
3. Not sure on an efficient way to generate and place polygons in a domain and track the intersection of lines
over a discrete space. May come back to this in future