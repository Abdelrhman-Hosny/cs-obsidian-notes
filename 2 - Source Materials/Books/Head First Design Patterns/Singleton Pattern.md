Ensures a class has only 1 instance and provides global access to it.
****
Keep in mind to **synchronize** if you have multiple threads.
```java
    public static void getInstance() {
        if (Database.instance == null) {
            acquireThreadLock() 
			// Ensure that the instance hasn't yet been
			// initialized by another thread while this one
			// has been waiting for the lock's release.
			if (Database.instance == null)
				Database.instance = new Database()
			leaveThreadLock()
		}
        return Database.instance
	}
```

**N.B.** Double check locking has a problem in java and might still cause issues.
****
## References
- https://stackoverflow.com/questions/1625118/java-double-checked-locking
