HW: Sudoku 3
------------

We're ready to begin programming our Sudoku puzzle builder. First things
first -- let's make data structures!

1. Create a class called Literal such that we can create propositional logic
literals corresponding to B and NOT B by typing 

    Literal("B", polarity=True)
    Literal("B", polarity=False)
    
respectively. Make sure that it supports the following operators:

  - equality: e.g. ```Literal("B", polarity=True) == Literal("B", polarity=True)```
  - inequality: e.g. ```Literal("B", polarity=True) != Literal("B", polarity=False)``` 
  
Also ensure that if we add duplicate literals to a set, then it doesn't count
them as separate objects, e.g., we should see the following behavior:

    > s = set([])
    > s.add(Literal("B", polarity=True))
    > len(s)
    1
    > s.add(Literal("B", polarity=True))
    > len(s)
    1
    > s.add(Literal("B", polarity=False))
    > len(s)
    2

Once you have a successful implementation, the following unit tests should
succeed:

    python -m unittest test_cnf.TestLiteral


2. Create a class called Clause such that we can create propositional logic
clauses (disjunctions of literals) by typing

        Clause([Literal("B", polarity=True), Literal("C", polarity=False)])
    
which would create a clause corresponding to (B or NOT C). 

Once this is complete, we've provided a convenience function in ```cnf.py``` for
constructing a clause from a string representation. Namely, the code

    import cnf
    cnf.c('B || !C')
    
constructs the same Clause instance as the (longer) expression above.

Make sure the Clause class supports the following operators:
  - equality: e.g. ```cnf.c('!A || B') == cnf.c('A || !B')```. Note that the
  literals of a clause have no ordering, so these clauses are considered equal.
  - inequality: e.g. ```cnf.c('!A || B') != cnf.c('!A || !B')```.
  - indexing: e.g. ```cnf.c('!A || B')['A']``` should evaluate to ```False```
  (because ```A``` has negative polarity in the clause) and 
  ```cnf.c('!A || B')['B']``` should evaluate to ```True``` (because ```B``` has
  positive polarity in the clause).
  - Boolean conversion: ```bool(cnf.c('FALSE'))``` should evaluate to False,
  whereas any other clause should evaluate to True.
  - String conversion: ```str(cnf.c('!A || B'))``` should evaluate to ```'!A || B'```.
  The literals should be alphabetically ordered, so  
  ```str(cnf.c('B || !A'))``` should also evaluate to ```'!A || B'```.
  ```str(cnf.c('FALSE'))``` should evaluate to ```'FALSE'```.
  - .symbols() method: ```cnf.c('!A || B').symbols()``` should return the set ```{'A', 'B'}```,
  corresponding to the symbols that appear in the clause.
  - membership:  ```'A' in cnf.c('!A || B')``` should return ```True```, 
  because symbol ```'A'``` appears in the clause. Correspondingly, 
  ```'D' in cnf.c('!A || B')``` should return ```False```.
  - .remove() method: ```cnf.c('!A || B || !C').remove('C')``` should return
  a new clause equal to  ```cnf.c('!A || B')```. If you try to remove a symbol
  that does not appear in the clause, then a copy of the original clause
  is returned. This method should be **immutable**, meaning that it doesn't modify
  the original Clause instance, it simply returns a new Clause instance.
  
Also ensure that if we add duplicate clauses to a set, then it doesn't count
them as separate objects, e.g., we should see the following behavior:

    > s = set([])
    > s.add(cnf.c('!A || B'))
    > len(s)
    1
    > s.add(cnf.c('!A || B'))
    > len(s)
    1
    > s.add(cnf.c('A || B'))
    > len(s)
    2  
 
Once you have a successful implementation, the following unit tests should
succeed:

    python -m unittest test_cnf.TestClause

3. Create a class called Cnf such that we can create CNF sentences by typing

        Cnf([cnf.c('!A || B'), cnf.c('B || D')])
    
   which would create a CNF sentence corresponding to (NOT A OR B) AND (B OR D). 

   Once this is complete, we've provided a convenience function in ```cnf.py``` for
   constructing a CNF sentence from a string representation. Namely, the code

       import cnf
       cnf.sentence('\n'.join(['B || !C', 'B || D']))
    
   constructs the same Cnf instance as the expression above.
   
   Make sure the Cnf class supports the following operators:
      
     - String conversion: ```str(cnf.sentence('!A || B\n!B || !C'))``` 
     should evaluate to ```'!A || B\n!B || !C'```. The clauses should be 
     alphabetically ordered, so ```str(cnf.sentence('!B || !C\n!A || B'))```
     should also evaluate to ```'!A || B\n!B || !C'```.
     - .symbols() method: ```cnf.sentence('!A || B\n!B || !C')).symbols()``` 
     should return the set ```{'A', 'B', 'C'}```,
     corresponding to the symbols that appear in the clause.
  
   Once you have a successful implementation, the following unit tests should
   succeed:

       python -m unittest test_cnf.TestSentence
