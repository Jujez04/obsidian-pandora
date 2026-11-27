Source code is a workspace where more than one programmer can modify code projects. There are some rules that defines how to create a good and comprehensive code, in fact keywords for this approach are "make it easy, understandable and consistent".

For each company there are internal rules that outlines how to make readable code for our team. These rules are based on Java Code Conventions, but each company can decide what can be added (but not removed).
# Base conventions
* *"One True Brace Style"* : each curly bracket is placed like described in *Egyptian brackets*, where the open brackets have to be placed after the statement and the close must be placed in the next line.
* Each loop or statement must have curly brackets, even with one instruction.
* *PascalCase* --> for classes
* *camelCase* --> for variables, fields and methods
* *SNAKE_CASE* --> for constants (every letter in uppercase)

# Formatting rules
- Indentation of 2/4 characters
- Lines no longer than 90 characters
- "// ... " for lines
- "/\*...\*/" for more lines and at the start of methods / classes
- "/\*\*...\*/ " for documentation
- Each method/constructor divided by one empty line
- Order of class properties :
	1) Static field (first public then static)
	2) Attribute field (first public then static)
	3) Constructors (first public then static)
	4) Methods (assembled by purpose)

# UML - Unified Modelling Language
It's a graphic OO-based language. This is a standard for documentation on developing classes and hierarchies, handling most of complex structures in a simple way. It's a standard of OMG (*Object Management Group*) since 1996.

These are some examples :
![[Pasted image 20241005172026.png]]

The number on the right corner of the `Lamp` implies that the referred uses that number of objects of that class :
![[Pasted image 20241005172044.png]]

![[Pasted image 20241005172224.png]]

For interfaces :
![[Pasted image 20241005173357.png]]