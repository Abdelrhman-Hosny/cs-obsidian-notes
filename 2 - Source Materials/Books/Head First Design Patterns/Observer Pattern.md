defines a one-to-many dependency between objects, so that when one object changes state, all of its dependents are notified and updated automatically.

![[observer-uml.png]]
****
## Publish-Subscribe vs Observer Pattern
You could say Pub-Sub is a more complex version of the Observer pattern.
Pub-Sub allows subscribers to express interest in different types of messages and further separates publishers from subscribers.

****
## Keeping a reference to the Observable
This is useful if you want to un-register yourself from the `Observers` list.
****
You'll find different variations of this pattern, some people will send only the needed data i.e. `update(float temp, float humidity)`,
Some people will pass the concrete Observer objects to observers and then implement a method in the `Observer` called `getObserverState` which gets the data that is needed. 
****
