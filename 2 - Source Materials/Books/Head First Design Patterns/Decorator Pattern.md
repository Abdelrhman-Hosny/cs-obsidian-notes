Attaching additional responsibility to a class at runtime.
Provides a flexible alternative to subclassing for extending functionality.
****
![[decorator-uml.png]]
****
## Real Life Examples
Java and .NET use the decorator pattern for their `InputStream` classes.
![[decorator-input-stream-java.png]]
****
What if we wanted to write another decorator that would make all our inputs lowercase ?
```java
public class LowerCaseInputStream extends FilterInputStream {
	InputStream in;
	public LowerCaseInputStream(InputStream in) {
		super(in);
		this.in = in;
	}

	@Override
	public int read() throws IOException {
		int c = in.read();
		return (c == -1? c: Character.toLowerCase((char) c));
	}

	@Override
	public int read(byte[] b, int offset, int len) throws IOException {
		// ...
	}
}
```

This new decorator makes use of the inner input stream and then modifies the behavior, this way we avoid creating a new class every time we want to make the input of any `InputStream` lowercase.
****
