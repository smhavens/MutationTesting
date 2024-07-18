# MutationTesting
## How to Run
Choose a desired python source file and run 

```
$ python3 mutate.py [PYTHON FILE] [NUM_OF_MUTANT_FILES]
$ cp [PYTHON_FILE] saved.py ; for mutant in [0-9].py ; do rm -rf *.pyc *cache* ; cp $mutant [PYTHON_FILE] ; python3 [TEST_CASES] 2> test.output ; echo $mutant ; grep FAILED test.output ; done ; cp saved.py [PYTHON_FILE] 
```
## Report
For the mutate.py mutations I had all comparator types (<, >, ==, !, is, in, <=, >=, !=, not
in, and is not) available to swap to the opposite value, a mutation that swaps and to or and vice
versa, a mutation to remove Aug Assignments because of the unlikeliness of compile errors to
occur from this specific mutation, binary operator mutation for all binary operators (+, -, *, /) to
become their opposite, and finally a unary operator mutation that can remove the not in
statements.

The decisions for these mutations were determined through the document index (0 to n),
ensuring the program was deterministic. Most mutations were based on the modularity using
index number and a constant to set mutation type percentage. Comparator mutations were
common, using doc_num % 5 == 0 to pick statements. Bool operator mutations were the only
ones not based on modularity, only running on the first 5 documents to ensure those outliers
would have one mutation and because of the low appearance of BoolOp nodes in fuzzywuzzy.py.
All other mutations were based on various prime or large division values to lower percentage(17,
25, 23). In addition to document number, MyVisitor had class variables to track the current
statement count for each mutable node type (what comparator node statement is being viewed) to
ensure a single instance of a mutation type for documents, a mutation requiring a statement count
based on doc_num for this reason.

In its entirety, working with the AST system was both harder than I imagined and easier
than I anticipated. The greatest difficulty was understanding AST, as most documentation for it
never properly describes what is happening when you walk through nodes and how to change
values, but once I found a well written guide (the Guide to AST as noted in sources) the actual
programming behind mutate.py became relatively easy with some hiccups in errors. The next
biggest challenge was finding good percentages for each type of mutation and determining which
statement to mutate but the programming behind it was easy. All in all, programming was simple
but the logic behind the program was complex. To compare mutation analysis with statement
coverage, I would both have a similar basis to really tear down what they test and try to find
edge-cases or small errors. Though the subject of their scrutiny is tested on a different basis, with
coverage focused on breath and mutation analysis focused on depth for test suites.

### Sources:
Eecs 481 HW3 webpage: https://web.eecs.umich.edu/~weimerw/481/hw3.html

Guide to AST: https://deepsource.io/blog/python-asts-by-building-your-own-linter/

Deleting Assignments/Function calls:
https://stackoverflow.com/questions/48771051/use-ast-module-to-mutate-and-delete-assignment-
function-calls

AST Library: https://docs.python.org/3/library/ast.html

Node Types: https://greentreesnakes.readthedocs.io/en/latest/nodes.html
