## An Intro to Linear Programming
(An Introduction to Algorithms, Fourth Edition, Cormen, Leiserson, Rivest & Stein Chapter29)

## Summary
Linear programming is used to optimize a linear function for a global minimum or a global maximum, within constraints defined by inequalities.

A linear programming problem can be modelled as a list of constraint functions (`constraints`), and an objective function, the `objective`

## Algorithms
### Setting the Stage
"some linear programming solutions run in polynomial time, some do not. According to the book, most are too complicated to be covered. We will be covering the `simplex algorithm`, the most commonly deployed solution method."
notation for describing the problem:
- take as input:
    - n real numbers c1,c2,...,cn
    - m real numbers b1,b2,...,bn 
    - m * n real numbers aij for i = 1, 2, ...,m and j = 1,2,...,n
types of algorithms:
- those that solve in polynomial time:
    - ellipsoid algorithms
    - interior-point algorithms
- those that do not solve in polynomial time:
    - the simplex algorithm 
        - it's worse-case is not polynomial
        - the book asserts that even though it's worse case is not polynomial, that it **works in practice**
sample problem
```
maximize x1 + x2
subject to
    4x1 - x2 <= 8
    2x1 + x2 <= 10
    5x1 - 2x2 >= -2
    x1,x2 >= 0
```
figure 29.2(a) graphs the three inequalities on a 2d grid, and the intersection of the regions under the lines forms the `feasible region`
you could solve this problem by evaluating the objective function for every discrete point in the feasible region and taking the maximum, but that would be too much computation
also, if you allow non-integer point coordinates than the set of points is infinite
if you choose some z for the equation x1 + x2 = z, and plot that line on the graph, the intersection of that line and the `feasible region` is a list of points that solve the system
in our case, if you plot x1 + x2 = 0, x1 + x2 = 4, and x1 + x2 = 8, you'll get three lines. x1 + x2 = 0 will be a line with a slope of negative 1 through the origin. It's set of points is the single point (0, 0)
if you plot x1 + x2 = 4, there will be another line with slope -1, that goes through the midsection of the feasible region
if you plot x1 + x2 = 8, there will be another line with slope -1, that touches the topmost `vertex` of the feasible region
the optimal z for a solution will give a line segment or a single vertex that touches the feasible region
if the optimal z gives a single vertex, than that is the single, optimal solution for the system
if the optimal z gives a line segment, then every point on that line segment has the same objective value
    they are equivalent, but different, optimal solutions
we can only graph the problem in this way and eyeball the solution because we have limited our solution to only two variables. With more variables, this method is unrealistic
in three dimensions (a problem with 3 variables), the feasible region becomes a 3d shape in space, and the optimal solution becomes a 2d plane that intersects that space

### The Simplex Algorithm
the `simplex algorithm` takes as an input a linear program and returns an optimal solution. It starts at some vertex of the simplex and then iterates. At each iteration, it moves along some edge of the simplex towards another vertex whose objective value is no less than the previous vertex's (of course, ideally it would move to a vertex with a higher objective value)
while this only finds a local maximum, because the feasible region is `convex` AND the objective function is linear, that local maximum must be the global maximum
the simplex method can be fast in practice if you are careful, but on carefully crafted inputs it can become exponential

### The Ellipsoid Algorithm
this was the first LP algorithm, and runs slowly in practice

### Interior Point Methods
interior point methods move through the interior of the feasible region
on large inputs, interior point methods can be as fast as the simplex algorithm, or even faster

## Problem Representation
### Problem Represented as a set of Equations
the goal is to find n real numbers x1,x2,...,x3 that we maximize "the sum from j=1 to n of CjXj" (page 854, figure 29.11) (can't represent this in markdown) subject to the constraints in the form of "the sum from j=1 to n of (29.12) AijXj <= Bi for 1 = 1,2,...,m and (29.13) Xj >=0 for j = 1,3,...,n" where 29.12 is a compressed way of writing the list of m+n constraints. The 29.13 constraints are the non-negativity constraints (each variable must be greater than 0)

### Problem Represented as Matrices
it is even more convenient to represent the problem as matrices
suppose we have:
- an m * n matrix A = (Aij) (note that the A in 'Aij' is a small 'a' in the text, but I can't type subscripts i and j, so I made the 'a' uppercase)
- an m vector b = (Bi) (note that here, 'an X vector <vector name>' means a vector of size X)
- an n vector c = (Cj)
- an n vector x = (Xj)
then, our problem could be rewritten as:
```
maximize (c^T)x (little c, superscript T, little x), this is the inner product of two n-vectors
subject to
    Ax <= b (29.15)
    x >= 0 (29.16)
```
29.15 Ax is the m-vector that is the product of an m * n matrix and an n-vector
29.16 means that each value in vector x must be greater than 0 (our non-negativity constraint from earlier)
this is called the `standard form` for a linear program
in real life, some linear programming problems may require variables that can have negative values, and some constraints may need to be equalities (not inequalities). The book says that exercizes 29.1-6 and 29.1-7 ask us to show that such linear programs can be reduced to the standard form

## Integer Linear Program
a LP with the additional constraint that all variables take on integer values
just finding a feasible solution to an ILP is NP-hard


## Vocab
decision variables - variables that represent the decisions to be made in order to solve the problem, these variables will be used to define our `objective function` and our `constraints`
minimization linear program - a linear programming solution designed to minimize something (like cost of raw materials)
maximization linear program - a linear programming solution designed to maximize something (like profits)
constraints - inequalities that apply absolute conditions that define what can and cannot be considered a solution to the problem
objective function - the function to be optimized for a minimum or a maximum, p852
linear equality - (example) f(x1,x2,...,xn) = b, p853
linear inequality - (example) f(x1,x2,...,xn) <= b, p853
linear constraints - a term used to refer to either a `linear equality` or a `linear inequality`, p853
feasible solution - a solution "x bar" (x with a hat) is feasible if it satisfies all of the constraints
infeasible solution - a "solution" that fails to satisfy all constraints
objective value - a solution to the linear problem (c^T)(x with a hat)
optimal solution - a solution whose `objective value` is a maximum over all feasible solutions
infeasible (linear program) - a linear program that has 0 feasible solutions is called `infeasible`
feasible (linear program) - a linear program with at least 1 feasible solution
feasible region - the set of points that satisfy all constraints
unbounded - a linear program that has some feasible solutions but does not have a finite optimal objective value is `unbounded`, that is to say that the problem is unbounded and therefore so is the program
simplex - the feasible region fromed by the intersection of the half-spaces of the constraints of a multi-dimensional linear program
integer linear program - a LP with the additional constraint that all variables take on integer values
