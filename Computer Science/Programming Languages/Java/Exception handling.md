# Errors 

## Compile-time errors
Compile-time errors occur **during the compilation phase**—when the code is being converted from source code to executable code by the compiler. These errors prevent the program from compiling successfully and are often related to syntax or type issues.

- **Common Types of Compile-time Errors**:
    - **Syntax Errors**: Mistakes in the code’s syntax, like missing semicolons, unmatched braces, or misspelled keywords.
    - **Type Errors**: Occurs when you try to assign a value of an incompatible type, like assigning a `String` to an `int`.
    - **Declaration Errors**: Trying to use a variable or method that hasn’t been declared.
    - **Access Modifiers Errors**: Accessing a private member from outside its class or accessing a protected member from a non-subclass.
## Run-time errors
Runtime errors occur **while the program is running** after successful compilation. These errors cause the program to behave unexpectedly or to crash.
In Java runtime errors are "throwable", in the meaning that every time they occur, they will launch a message in the error output. Additionally, errors are considered objects in Java, therefore there is an UML graphic that shows how errors can interpreted from the JVM:
![[Pasted image 20241027103259.png]]
As said previously, there is a keyword called `throw` that will give a message in the output. This keyword calls `getMessage` of the thrown object or `toString` method. In particular there are more than one of this type of keywords :
- `throw` :
	- *explicit* : using the keyword followed by a throwable object : `throw new NullPointerException()`;
	- *implicit* : the JVM will use this keyword if operative system produces an error while doing some specific instructions, like access in outbound position in arrays,...
- `throws` :
	- Some methods can be declared using this keyword if it can throw an error : `<method-signature> throws <exception-class>(...){}`
- `try` & `catch` block :
	- we can intercept an exception and give a more specific message if there is a runtime error:
```Java
try{
	//critical instructions
} catch(Exception e){
	throw e;
}
```
- `try-catch-finally` : 
	- `finally` declares a block that will be done even if the program produces an exception. It's useful for closing files, realising resources and even more.
	- There can be more than one `catch` block
```
try {  <body-maybe-throwing-an-exception> } 
catch( <throwable-class> <var> ) {  <handler-body> } 
catch( <throwable-class> <var> ) {  <handler-body> } 
... 
catch( <throwable-class> <var> ) {  <handler-body> } 
finally { <body-that-must-be-executed> }
```
 
 
# Checked exceptions
Checked exceptions are **checked at compile-time** by the compiler. If a method can throw a checked exception, it must either handle it within a `try-catch` block or declare it using the `throws` keyword in the method signature. 
Checked exceptions are typically used for errors that can be anticipated and recovered from, such as file I/O errors or database connectivity issues. They force the programmer to handle possible failure scenarios.

**Examples of Checked Exceptions**:
- **IOException**: Raised when an I/O operation fails or is interrupted.
- **SQLException**: Occurs when there’s a database access error or other SQL issues.
- **FileNotFoundException**: Thrown when a specified file cannot be found.
# Unchecked exceptions

Unchecked exceptions, also known as **runtime exceptions**, occur **at runtime** and are not checked during compilation. These exceptions typically result from programming logic errors and are not required to be handled or declared.
Unchecked exceptions often indicate bugs or flaws in the program logic that could be prevented through better coding practices, such as validating input and checking for null values.

**Examples of Unchecked Exceptions**:
- **NullPointerException**: Accessing a member of a null object.
- **ArithmeticException**: Errors in arithmetic operations, like division by zero.
- **ArrayIndexOutOfBoundsException**: Accessing an invalid index in an array.
