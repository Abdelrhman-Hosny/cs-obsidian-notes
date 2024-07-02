Encapsulates method invocation as an object, letting you parameterize other objects with different methods, queue or log requests, and support undoing operations.

**N.B.** Using command pattern also allows you to use the **Meta Command** Pattern which allows you to create macros of commands, so that you can **execute multiple commands at once**
****
```java
public class SimpleRemoteControl {
	Command slot;
	public void setCommand(Command command) {...}
	
	public void buttonWasPressed() {
	   slot.execute();
	}
}

public interface Command {
	void execute();
	void undo();
}

public class LightOnCommand implements Command {
	Light light;

	public void execute() {
		light.on();
	}
}

```

This was we can add any command to the remote without knowing what the actual command does.
****
There is also a way to create `MacroCommands`.
```java
public class MacroCommand implements Command {
	Command[] commands;
	
	public void execute() {
		for (Command c: commands) {
			c.execute();
		}
	}

	public void undo() {
		for (int i = commands.length - 1; i >= 0; i--) {
			commands[i].undo();
		}
	}
}
```

****
## Examples
If you think about an application like Photoshop, the toolbox could be thought of as a remote control.
Each button executes a command, this way you can customize everything with minimum changes.
This also allows for logging, redoing and undoing of operations.
Another added benefit is that a command can be used in multiple places. e.g. `CopyCommand` can be used with `Ctrl+c` as well as the `Copy` with the mouse or adding any additional shortcuts.
****







