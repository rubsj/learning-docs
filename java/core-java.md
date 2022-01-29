## core concepts
#### conditionals and loops
- the `break` statement does not apply to `if` or `if-else` statements
- labeled `break` and `continue` statements. The break and continue statements apply to the innermost `for` or `while` loop. Sometimes we want to jump out of several levels of nested loops. Java provides the labeled break and labeled continue statements to accomplish this.
- The break statement terminates the labeled statement; it does not transfer the flow of control to the label. Control flow is transferred to the statement immediately following the labeled (terminated) statement.
```
   search:
        for (i = 0; i < arrayOfInts.length; i++) {
            for (j = 0; j < arrayOfInts[i].length;
                 j++) {
                if (arrayOfInts[i][j] == searchfor) {
                    foundIt = true;
                    break search;
                }
            }
        }

        if (foundIt) {
              System.out.println("Found " + searchfor + " at " + i + ", " + j);
        } 
 ```


 #### var pseudo keyword
 - allowed only for local variables
 - NOT allowed for fields , static variables , method parameters
 - must be declared and initialized together