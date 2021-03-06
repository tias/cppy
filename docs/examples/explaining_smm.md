## Send More Money

Now let us take a look in detail how easy is to integrate cp programming through the Send More Money example. 
First we need to import all the tools that we will need to create our CP model:

```python
from cpmpy import *
import numpy as np
```

Secondly, as in every constraint programming model we need to define variables and constraints. Variables are introduced 
as follows:

```python
s,e,n,d,m,o,r,y = IntVar(0,9, 8)
```

This line indicates that we are creating 8 integer variables, s,e,n,d,m,o,r,y, with domain between 0 and 9. In general, the sintax to generate
n integer variables between a and b is

```python
ListOfVariables = [IntVar(a,b,n)]
```


Constraints are included in the model as a list. First, we create a list to add the constraints. Then, we append an 'all different constraint' in a straightforward fashion. Finally, we add the constraint saying SEND + MORE = MONEY. 

```python
constraint = []
constraint += [ alldifferent([s,e,n,d,m,o,r,y]) ]
constraint += [    sum(   [s,e,n,d] * np.flip(10**np.arange(4)) )
                 + sum(   [m,o,r,e] * np.flip(10**np.arange(4)) )
                == sum( [m,o,n,e,y] * np.flip(10**np.arange(5)) ) ]
```             
Note that we can use numpy library to efficiently state the last constraint. As final modeling step we need to create a Model object. To do this, we need to state the model
object with the list of constraints as argument.

```python
model = Model(constraint)
```
We finally solve the CSP with the following command:

```python
stats = model.solve()
```
where the variable ```python status``` saves the exit status of the feasibility problem and the execution time. In this case: ```ExitStatus.FEASIBLE (0.24 seconds)```, indicating that the problem was solved by finding a feasible solution, i.e. a solution that satisfies all the constraints.
 

In the case that you may add an objective function, a second argument must be added. This can be a maximization or a minimization. As we aim to maximize the value
of the word MONEY.

```python
coefs  = np.flip(10**np.arange(5))
objective = np.dot([m,o,n,e,y],coefs)
model = Model(constraint, maximize = objective)
```
Note that in this COP the exit status changes to ```ExitStatus.OPTIMAL (0.24 seconds)```, indicating that we found an optimal solution, i.e. the best amongst all the feasible solutions.
