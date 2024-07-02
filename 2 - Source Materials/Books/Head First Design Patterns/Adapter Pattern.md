Converts the interface of a class into another interface.
Adapter lets classes work together that couldn't otherwise because of **incompatible interfaces**
![[adapter-uml.png]]
****
This is useful when you want to use an external library, so you have to modify the interface using the adapter, so that if you decide to change the interface later on, you only create a new Adapter class.
****
## Uses
### System Integration
If you have two clients, both clients have the same functionality but the format response of the API is different.
You could create one response and then use an Adapter class that converts the response.
****
### Using a library that could be changed
You can use the adapter to wrap the desired functionality.
That way you don't have to change all usages of that library, create another Adapter to wrap the new Adaptee.
****

