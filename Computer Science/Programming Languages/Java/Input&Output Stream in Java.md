This page describes some general I/O features of the package `java.io`. This is inside the module `java.base` and extends `Objects` for handling input and output data streams. 
![[Pasted image 20241115162313.png]]

# File

Every *file* contains information with byte sequences and they can be identify by what type of data it stores. In Java there is a difference between *bin* and *text* files and there is in common one objects that handles files and file system in general and that is `java.io.File`. It can identify a file inside the file system, get information about it, environment properties, creates directories,... 

Although they have a lot of methods about their behaviour, there are several downsides due to *portability* and for this fact we have to get control of it using some methods and identify our separator, depending on which platform we are working. For example:
```Java
import java.io.*;
import java.util.*;
import java.lang.reflect.*;

public class UseFile {

	public static final String SEP = File.separator;
	// public static final String FILE_NAME = 
	// "/home/mirko/aula/oop/Prova.bin"; // not portable
	public static final String FILE_NAME = 
		System.getProperty("user.home") + SEP + "aula" + SEP + "oop" + SEP + "Prova.bin";

	private static Iterable<Method> accessors(Class<?> c) throws Exception {
		final Collection<Method> list = new ArrayList<>();
		for (final Method m : c.getMethods()) {
			if (m.getParameterTypes().length == 0
					&& m.getName().matches("has.*|is.*|get.*|can.*")) { // REGEX
				list.add(m);
			}
		}
		return list;
	}

	public static void main(String[] args) throws Exception {
		final File f = new File( args.length == 0 ? FILE_NAME : args[0]);
		for (final Method m : accessors(File.class)) {
			System.out.println(m.getName() + " " + m.invoke(f));
		}
	}
}
// REGEX  --> regular expression: http://docs.oracle.com/javase/tutorial/essential/regex/
```
This example demonstrates that transferring a file from one computer to another can be tedious. Making an application portable often depends on the environment.

# InputStream and OutputStream

These objects can handle *streams* of data, or more generally *bytes*. Both `InputStream` and `OutputStream` are abstract classes implemented with the [[Template method pattern]], actually proving all type of implemented methods for reading/writing data but they are based on one abstract method. Respectively we have `read` for input and `write` forf output.

Their classes are specialization of them and implements these methods, for example we are showing the case of `ByteArrayInputStream`. Also we can notice that these methods returns an integer type, and it's the byte red if there are bytes to be handled and `-1` if there are no byte left. For example:
```Java
import java.io.*;

public class UseByteArrayStream {

	public static void main(String[] args) throws IOException {
		final byte[] b = new byte[] { 10, 20, -1, 40, -58 };
		final InputStream in = new ByteArrayInputStream(b);
		int c;
		try {
			while ((c = in.read()) != -1) { // C-style
				System.out.println(c);
			}
			System.out.println(c);
		} finally { // assicura la chiusura anche con eccezioni
			in.close();
		}
	}
}
/*Output
10
20
255
40
198
-1
*/
```

## try-with-resources construct
The `try-with-resources` construct  allows us to declare resources to be used in a _try_ block with the assurance that the resources will be closed after the execution of that block. As restriction, we can declare only classes that implements `Autoclosable` interface.
An example:
```Java
public class UseTryWithResources {

	public static void main(String[] args) throws IOException {
		final byte[] b = new byte[] { 10, 20, 30, 40, 50 };
		int c;
		try (final InputStream in = new ByteArrayInputStream(b)) {
			while ((c = in.read()) != -1) { 
				System.out.println(c);
			}
		}
	}
}
```

## Storing a data structure of bytes
Also Java gives an opportunity in implementing the solution of marshalling problem. In computer science, marshalling or *marshaling* (US spelling) is the process of transforming the memory representation of an object into a data format suitable for storage or transmission, especially between different runtimes.
```Java
public class ListOnFile {

	public static void main(String[] args) throws IOException {
		final List<Byte> list = new ArrayList<>(20);
		final byte[] ar = new byte[20];
		new Random().nextBytes(ar);
		for (final byte b : ar) {
			list.add(b);
		}
		System.out.println("Prima: " + list);
		try (
			final OutputStream file = new FileOutputStream(UseFile.FILE_NAME)
		) {
			for (final byte b : list) { 
				file.write(b);
			}
		}
		try (
			final InputStream file2 = new FileInputStream(UseFile.FILE_NAME)
		) {
			final List<Byte> list2 = new ArrayList<>();
			int c;
			while ((c = file2.read()) != -1) { // Ricarico da file
				list2.add((byte) c);
			}
			System.out.println("Dopo: " + list2);
		}
	}
}
```

## How to read more types of data
Whether we want to read integers, long types, strings and so on, there are some possibilities that we won't use `ByteArrayInputStream` but a class that is more abstracted from our context. In this case Java developers uses the [[Decorator pattern]] for implementing the class `DataInputStream`(also there is the output version).

![[Pasted image 20241115191755.png]]

Using `DataInputStream` in an example:
```Java
public class UseDataStream {
	public static void main(String[] args) throws IOException {

		try (
			final OutputStream file = new FileOutputStream(UseFile.FILE_NAME);
			final DataOutputStream dstream = new DataOutputStream(file);
		) {
			dstream.writeBoolean(true);
			dstream.writeInt(10000);
			dstream.writeUTF("Ciao");
			dstream.writeDouble(5.2);
		}
		try (
			final InputStream file2 = new FileInputStream(UseFile.FILE_NAME);
			final DataInputStream dstream2 = new DataInputStream(file2);
		){
			System.out.println(dstream2.readBoolean()); // Do not change order
			System.out.println(dstream2.readInt());
			System.out.println(dstream2.readUTF());
			System.out.println(dstream2.readDouble());
		}
	}
}
```

## Charging up buffers for handling information

There are classes that approaches the problem of managing input/output streams in a different way. Instead of taking up bytes one by one, there are classes that implements that stores a fixed amount of bytes (*buffer* of data) and then do the operations that is set. This case is implemented by the classes `BufferedOutputStream` and `BufferedInputStream` and they have better performance then `DataStream` classes.
```Java
public class UseBufferedDataStream {

		public static void main(String[] args) throws IOException {
		// dstream -> bstream -> file
		try (
			final OutputStream file = new FileOutputStream(UseFile.FILE_NAME);
			final OutputStream bstream = new BufferedOutputStream(file);
			final DataOutputStream dstream = new DataOutputStream(bstream);
		){
			dstream.writeBoolean(true);
			dstream.writeInt(10000);
			dstream.writeDouble(5.2);
			dstream.writeUTF("Prova"); //  UTF-16
		}
		// dstream2 -> bstream2 -> file2
		try (
			final InputStream file2 = new FileInputStream(UseFile.FILE_NAME);
			final InputStream bstream2 = new BufferedInputStream(file2);
			final DataInputStream dstream2 = new DataInputStream(bstream2);
		){
			System.out.println(dstream2.readBoolean()); // Do not change order!!
			System.out.println(dstream2.readInt());
			System.out.println(dstream2.readDouble());
			System.out.println(dstream2.readUTF());
		}
	}
}

//In alternative
public class UseBufferedDataStream2 {

		public static void main(String[] args) throws IOException {
		// dstream -> bstream -> file
		try (
			final DataOutputStream dstream = new DataOutputStream(
				new BufferedOutputStream(
					new FileOutputStream(UseFile.FILE_NAME)));
		){
			dstream.writeBoolean(true);
			dstream.writeInt(10000);
			dstream.writeDouble(5.2);
			dstream.writeUTF("Prova"); 
		}
		// dstream2 -> bstream2 -> file2
		try (
			final DataInputStream dstream2 = new DataInputStream(
				new BufferedInputStream(
					new FileInputStream(UseFile.FILE_NAME)));
		){
			System.out.println(dstream2.readBoolean()); // Do not change order!!
			System.out.println(dstream2.readInt());
			System.out.println(dstream2.readDouble());
			System.out.println(dstream2.readUTF());
		}
	}
}
```

## Conclusions
This is a detailed view of the decorator pattern implemented in `java.io`:
![[Pasted image 20241115194516.png]]

Other classes that can be useful for projects:
- `CheckedInputStream` : maintains a *checksum* of data 
- `CipherInputStream` : reads data that are previously ciphered
- `DeflateInputStream` : reads and compress in "deflate" mode data
- `InflateInputStream` : decompress in "deflate" mode data and read them
- `ProgressMonitorInputStream` : reads data in a way that a bunch of them can be "unread"
# Serializable objects
*Serialize* an object is a problem where we want to preserve the state of an object in different runtime moments or in different JVM. An example of this problem comes when there's an object that needs to be transferred from our machine to a different  one via network. Java supports this type of operations since Java 1 version and are collected from an interface called `java.io.Serializable`.
```Java
public class Person implements java.io.Serializable {

	private static final long serialVersionUID = 567742502623265945L;
	private final String name;
	private final int birthYear;
	private final boolean married;

	public Person(
			final String name, 
			final int birthYear, 
			final boolean married
	) {
		this.name = name;
		this.birthYear = birthYear;
		this.married = married;
	}

	public String toString() {
		return this.name + ":" + this.birthYear + ":"
				+ (this.married ? "married" : "not-married");
	}
}
```
Serializable objects can be red from `ObjectInputStream` and `ObjectOutputStream`. Whether we want to serialize an object with a specific format we have to override the methods `readObject` and `writeObject`.

There are some IDE that gives the opportunity to choice if we want to define an UID version for our object, through the `long serialVersionUID` field. This case comes when we serialize an two classes with the same name but different implementations, in way that our object data can be corrupted from one to another. The UID version is evaluated from the method `readObject`, throwing an exception when we put an argument that cannot be compatible.

There's even a keyword called `transient` that allows to the JVM to avoid particular fields that needn't to be serialized. For example if we need to serialize an object an it contains another object that cannot be serialized, we can use this keyword to resolve this problem.
More in general, `transient` allows to be written in different situations:
- *Performance* : avoid fields that they can evaluated from other fields
- *Fields* that have information about current JVM
- *Not serializable objects*
- *Fields that we want a different serializable operation*
Performance details for example comes with the serialization of a `String` from the `toString` method:

```Java
public class CPerson implements java.io.Serializable {

	private String name;
	private int birthYear;
	private boolean married;
	transient private String cachedToString = null;  //!!!!!!!

	public CPerson(String name, int birthYear, boolean married) {
		this.name = name;
		this.birthYear = birthYear;
		this.married = married;
	}

	public String getName() {
		return this.name;
	}

	public int getBirthYear() {
		return this.birthYear;
	}

	public boolean isMarried() {
		return this.married;
	}

	public void setName(final String name) {
		this.name = name;
		this.cachedToString = null;
	}

	public void setBirthYear(final int birthYear) {
		this.birthYear = birthYear;
		this.cachedToString = null;
	}

	public void setMarried(final boolean married) {
		this.married = married;
		this.cachedToString = null;
	}

	private String computeToString() {
		return this.name + ":" + this.birthYear + ":"
				+ (this.married ? "married" : "not-married");
	}

	public String toString() { //!!!!!
		if (this.cachedToString == null) {
			System.err.println("Log: The cache is empty...");
			this.cachedToString = this.computeToString();
		}
		return this.cachedToString;
	}
}
```

If we want to serialize an object that contains a date from the class `java.util.Date` we can recall at the reading moment:
```Java
public class APerson implements java.io.Serializable {
	private static final long serialVersionUID = -8985026380526620812L;

	private final String name;
	private transient Date lastUse = new Date();

	public APerson(final String name) {
		this.name = name;
	}

	public void used() {
		this.lastUse = new Date();
	}

	public String toString() {
		return this.name + ":"
				+ (this.lastUse == null ? "null" : this.lastUse.getTime());
	}

	private void writeObject(final ObjectOutputStream out) throws IOException {
		out.defaultWriteObject(); // accetto il comportamento di default
		System.err.println("writing");
	}

	// una sorta di costruttore..
	private void readObject(final ObjectInputStream in) throws IOException, ClassNotFoundException {
		in.defaultReadObject();
		System.err.println("reading");
		this.lastUse = new Date(); // in lettura, ripristino la data corrente
	}
}
```

Also we can serialize arrays:
![[Pasted image 20241116101938.png]]

# Random access file
Some files has large dimensions and they can be red in their whole context, but only in specific parts of them. An example can be a video, where it shows only one by one frames.
Then we use the class `RandomAccessFile` that contains methods from `DataInput` and `DataOutput`:
![[Pasted image 20241116102508.png]]

An example can be when to read different parts of an array that cannot be consecutive:
```Java
public class UseRandomAccessFile {

	public static void main(String[] args) throws IOException {

		try (final RandomAccessFile raf = new RandomAccessFile(
				UseFile.FILE_NAME, "rw")) { // read-write

			for (int i = 0; i < 100000; i++) {
				System.out.println("writing: " + i);
				raf.writeInt(i);
			}
			raf.seek(23000 * 4);
			System.out.println("reading in position 23000*4: " + raf.readInt());
			raf.setLength(800000);
			System.out.println("extending the size");
			raf.seek(123000 * 4);
			System.out.println("reading in position 123000*4: " + raf.readInt());
		}
	}
}
```

# Text file

Text files contains string and they cannot be converted from byte to byte in our JVM. 
There are some formats than converts from objects to string and viceversa:
- *JSON (JavaScript Object Notation)*
- *TOML(Tom's Obvious Minimal Language)*
- *YAML Ain't Markup Language*

Consecutively we have to convert a string to a text file, and this results in the *text* *encoding* problem. There are different types of encoding:
- *ASCII* (default)
- *UTF-8* (default in Linux)
- *UTF-16* (default in Java)
- *ISO LATIN* (default in Windows)

## Readers (and Writers)
- `Reader` : reads text lines
- `FileReader` : extension that reads from a `FileInputStream`
- `StringReader` : decorator that reads from `String` objects
- `BufferedReader` : decorator for increase performance
- `InputStreamReader` : decorator that encapsule an `InputStream`

Here there is a specific solution for UTF-16  encoding:
```Java
public class UseStreamReadersWriters {

	public static void main(String[] args) throws Exception{

		try(final BufferedWriter w = new BufferedWriter(
				new OutputStreamWriter(
					new FileOutputStream(UseReadersWriters.FILE_NAME),"UTF-16"))){
			w.write("Test");
			w.newLine();
			w.write("file");
			w.newLine();
		}
		
		try(final BufferedReader r = new BufferedReader(
				new InputStreamReader(
					new FileInputStream(UseReadersWriters.FILE_NAME),"UTF-16"))){
			System.out.println(r.readLine());
			System.out.println(r.readLine());
			System.out.println(r.readLine()); // null
		}	
	}
}
```