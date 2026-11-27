From now on we are talking about designing *software* *systems*, composed of input data, output data and an environment that talks with the system. The software environment uses OS resources, files, networks, ... and communicates with users through GUI or CUI. 

In this page we will talk about GUI and how Java implements it with two packages basically. One of them is *AWT(Abstract Window Toolkit)* and this is inside Java from version 1/2, but this is a low level package. More useful classes belongs to Java Swing. Since version 5, there are some abstract tools that improves ways of creating components inside a frame, that also belongs to its predecessor. 

This is the UML of Java AWT and Swing. Swing can be interpreted as a subset of AWT, for the fact that all of its components extends/implements `JComponent`. As we can see, all the elements of Swing starts with "J" and they are imported with `javax.swing` in the module `java.desktop`. On the other hand there is the main one, AWT, and all of its components uses the interface `Component`
![[Pasted image 20241108214026.png]]

Here there is an example :
```Java
import javax.swing.*;

public class TrySwing {
	public static void main(String[] args){
		final JFrame frame = new JFrame();	   //New frame
		frame.setTitle("Prova di JFrame"); 
		frame.setDefaultCloseOperation(JFrame.DO_NOTHING_ON_CLOSE); //The exit button provided from OS won't work
		frame.setSize(320,240);
		
		final JPanel panel = new JPanel();
		frame.getContentPane().add(panel); //Panel added in frame
		
		panel.add(new JButton("Prova di pulsante")); //Adding component inside the panel
		
		frame.setVisible(true);  //Crucial part: setting the frame visible
	}
}

```

Also Oracle provides official tutorials for handling with these components in I http://docs.oracle.com/javase/tutorial/uiswing/. In this scheme there are the most used classes for GUI :
![[Pasted image 20241109094452.png]]

# Panels

![[Panels in Java and Layouts]]

A technique that is used for *real* *applications* is building a GUI that hides low level implementation. For example we can design an interface for our GUI that doesn't mentions Swing/AWT and all is delegated to a class that extends `JFrame`:

```Java
public interface UserInterface { 

	void show () ; 
	
	void setDimensions (int x, int y);
}

public class UserInterfaceImpl implements UserInterface {

	private final MyFrame frame;
	
	public UserInterfaceImpl() {
		this.frame = new MyFrame(" Flow and Border ", new BorderLayout());
		final JPanel pNorth = new JPanel(new FlowLayout ());
		pNorth.add( new JButton("North 1"));
		final JPanel pSouth = new JPanel(new FlowLayout(FlowLayout.RIGHT));
		pSouth.add(new JButton("OK"));
		this.frame.getMainPanel().add(pNorth,BorderLayout.NORTH) ;
		this.frame.getMainPanel().add(pSouth,BorderLayout.SOUTH) ;
	}
	
	public void show () {
		this.frame.setVisible(true);
	}
	
	public void setDimensions (int x, int y ) {
		this.frame.setSize(x, y);
	}
}
```

# Events and Listeners/Observers
How can we program something that creates an interactions between our GUI and the client? Answering this question is quite simple for actually the existence of *observers* but it will be more complicated once we are getting through the pattern that implements observers. 

## Observers in Java
In Java observers are called *listeners*. Every time that an event, like pressing a button or typing something, they will be triggered and will compute some instructions. 

As described in [[Observer pattern]] there is a component that catches all our events and will call a method. This concrete observer will be an implementations of an interface that describes the behaviour of how we can notify them.

An interface is already given by `java.util` with `EventListener` but the most used are inside `java.awt.event`:
![[Pasted image 20241109111221.png]]

## EDT or Event Dispatcher Thread
There is a thread that is created every time we create a frame in the `main` thread. This thread is called *Event* *Dispatcher* *Thread* and it's created by the JVM every time `JFrame` is invoked. Actually every program showed in this page isn't closed once the `main` thread is finished. This thread loops every GUI instructions and if events handlers aren't created, the thread will be stacked inside this loop. Whether an error occur, the EDT will print the entire stack trace.
## Examples
Here there is an implementations of how *events* are described within classes and interfaces:
```Java
class JButton .. { 
	void setActionCommand(String s) {..} 
	void addActionListener(ActionListener listener) {..} 
	void removeActionListener( ActionListener listener) {..} 
}

interface ActionListener ..{ 
	void actionPerformed (ActionEvent e); 
}

//This class implements an event
class ActionEvent .. { 
	String getActionCommand() {..} 
	long getWhen() {..} 
	.. 
}
```


From now on there are some example of how we can use listeners:
```Java
public class UseButtonEvents{
	public static void main(String[] args){
		final MyFrame frame = new MyFrame("Events Example",new FlowLayout());
		
		final ActionListener listener = new MyActionListener();
		
		final JButton b1 = new JButton("Say Hello");
		b1.setActionCommand("hello"); 
		b1.addActionListener(listener); //!!
		
		final JButton b2 = new JButton("Quit");
		b2.setActionCommand("quit");
		b2.addActionListener(listener);
		
		final JButton b3 = new JButton("Looping..");
		b3.setActionCommand("loop");
		b3.addActionListener(listener);
		
		frame.getMainPanel().add(b1);
		frame.getMainPanel().add(b2);
		frame.getMainPanel().add(b3);
		frame.setVisible(true);
	}
}

public class UseButtonEvents2{
	public static void main(String[] args){
		final JButton b1 = new JButton("Say Hello");
		b1.addActionListener(new ActionListener(){
			public void actionPerformed(ActionEvent e) {
				System.out.println("Hello!!");
			}
		});
		
		final JButton b2 = new JButton("Quit");
		b2.addActionListener(new ActionListener(){
			public void actionPerformed(ActionEvent e) {
				System.out.println("Quitting.."); 
				System.exit(0);
			}
		}); 
		
		final MyFrame frame = new MyFrame("Events Example",new FlowLayout());
		frame.getMainPanel().add(b1);
		frame.getMainPanel().add(b2);
		frame.setVisible(true);
	}
}

```

Inner listeners:
```Java
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.*;

public class EventsFrame {

	private final JButton b1 = new JButton("Say Hello");
	private final JButton b2 = new JButton("Quit");
	private final JButton b3 = new JButton("Looping..");

	public EventsFrame() {
		final MyFrame frame = new MyFrame("Events Example", new FlowLayout());
		frame.getMainPanel().add(b1); // aggiungo i pulsanti
		frame.getMainPanel().add(b2);
		frame.getMainPanel().add(b3);
		final ActionListener listener = new MyActionListener(); //listener unico
		b1.addActionListener(listener);// registro il listener
		b2.addActionListener(listener);// senza actionCommand!!
		b3.addActionListener(listener);
		frame.setVisible(true);
	}

	private class MyActionListener implements ActionListener {

		public void actionPerformed(ActionEvent e) { //switch su getSource
			if (e.getSource()==EventsFrame.this.b1) {
				System.out.println("Hello!!");
			} else if (e.getSource()==EventsFrame.this.b2) {
				System.out.println("Quitting..");
				System.exit(0);
			} else if (e.getSource()==EventsFrame.this.b3) {
				System.out.println("Going stuck..");
				for (; true;) {
				} // Nota l'effetto del loop sulla GUI
			}
		}
	}
	
	public static void main(String[] args){
		new EventsFrame();
	}
}

```


# Advanced Functionalities

## Listeners that modifies GUI
```Java
import java.awt.*;
import java.awt.event.*;
import javax.swing.*;

public class UseIOGUI{
	public static void main(String[] args){
		final JTextField tf = new JTextField(10);
		final JLabel lb = new JLabel("Result: 0");
		final JButton bt = new JButton("Multiply by 2");
		
		bt.addActionListener(new ActionListener(){
			public void actionPerformed(ActionEvent e) {
				String s = tf.getText(); // "21"
				int n = Integer.parseInt(s); // 21
				lb.setText("Result :"+n*2);
			}
		});
		
		final FlowLayout lay = new FlowLayout(FlowLayout.CENTER,10,10); 
		final MyFrame frame = new MyFrame("I/O Example",lay);
		frame.getMainPanel().add(tf);
		frame.getMainPanel().add(lb);
		frame.getMainPanel().add(bt);
		frame.setVisible(true);
	}
}
```
![[Pasted image 20241109123547.png]]
## Dynamic Layout
```Java
public class UseDynamicLayout{
	public static void main(String[] args){
		
		final FlowLayout lay = new FlowLayout(FlowLayout.CENTER,10,10); 
		final MyFrame frame = new MyFrame("Adding Buttons",lay);
		final JPanel panel = frame.getMainPanel();
		
		final JButton bt = new JButton("Add a Button");
		bt.addActionListener(new ActionListener(){
			int count = 0;
			public void actionPerformed(ActionEvent e) {
				panel.add(new JButton("Button"+count++));
				panel.validate(); //!!
			}
		});
		panel.add(bt);
		frame.setVisible(true);
	}
}
```
## Canvas and Drawing in a panel
```Java
public class UseCanvas{
	
	public static void main(String[] args){
		final DrawPanel pCenter = new DrawPanel();
		pCenter.setBorder(new TitledBorder("Circles here.."));
		pCenter.addMouseListener(new MouseAdapter(){  //Adapter has all methods empty
			public void mouseClicked(MouseEvent e) {
				pCenter.addPoint(e.getX(), e.getY());
				pCenter.repaint();
			}
		});
		final JPanel pEast = new JPanel(new FlowLayout());
		final JButton bt = new JButton("Draw");
		bt.addActionListener(new ActionListener(){
			public void actionPerformed(ActionEvent e) {
				pCenter.addRandomPoint();
				pCenter.repaint();
			}
		});
		pEast.add(bt);
		
		final MyFrame frame = new MyFrame("Canvas Example",new BorderLayout());
		frame.getMainPanel().add(pCenter,BorderLayout.CENTER);
		frame.getMainPanel().add(pEast,BorderLayout.EAST);
		frame.setResizable(false); // non estendibile!!
		frame.setVisible(true);
	}
}
```
![[Pasted image 20241109123633.png]]
## Specializing Panels
```Java
public class DrawPanel extends JPanel {

	private static final int RADIUS = 20;
	private static final Random RND = new Random();
	private final Map<Point,Color> circles = new HashMap<>();

	protected void paintComponent(Graphics g) {
		super.paintComponent(g);
		for (Map.Entry<Point,Color> e : this.circles.entrySet()) {
			g.setColor(e.getValue());
			g.fillOval(e.getKey().x, e.getKey().y, RADIUS * 2, RADIUS * 2);
		}
	}
	
	public void addRandomPoint(){
		int x = RND.nextInt(this.getWidth());
		int y = RND.nextInt(this.getHeight());
		this.addPoint(x,y);
	}
	
	public void addPoint(int x, int y){
		final Color c = new Color(RND.nextInt(256),RND.nextInt(256),RND.nextInt(256));
		this.circles.put(new Point(x - RADIUS, y - RADIUS), c);
	}
}
```
## Dialog with the user
```Java
public class UseDialogs{
	public static void main(String[] args){
		final FlowLayout fl = new FlowLayout(FlowLayout.CENTER,10,10);
		final MyFrame frame = new MyFrame("Dialogs Example",fl);
		frame.setDefaultCloseOperation(WindowConstants.DO_NOTHING_ON_CLOSE);
		frame.addWindowListener(new WindowAdapter(){
			public void windowClosing(WindowEvent e) {
				int n = JOptionPane.showConfirmDialog(frame,
					    "Do you really want to quit?",
					    "Quitting..", JOptionPane.YES_NO_OPTION);
				if (n == JOptionPane.YES_OPTION){
					System.exit(0);
				}
			}
		});
		final JTextField tf = new JTextField(10);
		final JLabel lb = new JLabel("Result: 0");
		final JButton bt = new JButton("Multiply by 2");
		bt.addActionListener(new ActionListener(){
			public void actionPerformed(ActionEvent e) {
				try{
					lb.setText("Result :"+Integer.parseInt(tf.getText())*2);
				} catch (Exception ex){
					JOptionPane.showMessageDialog(frame, "An integer please..");
				}
			}
		});		
		frame.getMainPanel().add(tf);
		frame.getMainPanel().add(lb);
		frame.getMainPanel().add(bt);
		frame.setVisible(true);
	}
}

```
# MVC for graphic applications

First of all we are going to describe how a software architecture works. This is the structure that our software needed to be well implemented. It's defined by :
- software elements
- relation between elements
- elements properties

Software architecture is essential for designing application that needs an interface with the user. There are some ways in developing software architecture and all of them can be included within a terminology called *Architectural* *Pattern*.
![[Pasted image 20241109142633.png]]

In this section we will talk about MVC or *Model* *View* *Controller* strategy, that regards exclusively OO languages:
- *Model* : it is responsible for managing the data, logic, and rules of the application.
- *View* : the part of the application that displays data to the user and sends user interactions to the controller.
- *Controllers* : the component that interprets user inputs from the View, modifies the Model accordingly, and updates the View based on changes in the Model.

A key benefit in creating this type of structure is that every part needs to be *independent* among each others. This separation improves *modularity*, making it easier to maintain or modify one part (like the View) without affecting others (like the Model). 


// In 15 Graphical User Interface there is an example of this type of architecture.