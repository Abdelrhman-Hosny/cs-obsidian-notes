****
Methods that should be invoked come from:
- The object **itself**
- Objects passed as **parameters** to the method
- Any object **created in** method
- Any **component** of the object
This tells us not to call methods on objects returned from calling another method
e.g. `Book.getAuthor().doSomething()` -> `Book.makeAuthorDoSomething()`
****
## Trade-offs
### Advantages
Reduces the dependencies between objects which in turn reduces maintenance
### Disadvantages
Results in more "wrapper" classes being written to handle method calls to other components.
This results in more complexity, more dev time, worse runtime performance.

****
a.k.a the **Law of Demeter**, However the name principle of least knowledge is more accurate
****
