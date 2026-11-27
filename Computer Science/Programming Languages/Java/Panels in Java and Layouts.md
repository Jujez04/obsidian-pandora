n Java's Swing library, layout managers are essential for controlling the arrangement of components within a container, like a `JPanel`. This design is an application of the Strategy pattern, as it allows the layout to be changed by simply switching the layout manager without altering the individual components.

Applying the Strategy pattern, `LayoutManager` interface has several classes that provides unique ways for placing objects. For example :
1. **`FlowLayout`**: The default layout for panels, which places components in a row, wrapping to the next line when there's no space.
2. **`BorderLayout`**: Divides the container into five regions: NORTH, SOUTH, EAST, WEST, and CENTER. Each region can hold only one component.
3. **`GridBagLayout`**: Highly flexible but more complex, allowing precise control over the positioning and sizing of components using constraints.
4. **`GridLayout`**: Arranges components in a grid of equally sized cells.
5. **`CardLayout`**: Allows components to be swapped in and out, useful for implementing a series of screens or "cards."

Each layout manager provides different behaviors, making it suitable for various GUI designs. Most of them can be consulted in http://docs.oracle.com/javase/tutorial/uiswing/layout/visual.html .


# Examples
## Without any specific layout --> deprecated
```Java
public class UseNoLayout {
	public static void main(String[] args){
		final JFrame frame = new JFrame();	   
		frame.setTitle("AbsoluteLayout Example"); 
		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		frame.setResizable(false); // o true..
		frame.setSize(320,200);
		
		final JPanel panel = new JPanel();
		panel.setLayout(null); // Nessun layout
		frame.getContentPane().add(panel);

		final JButton b1 = new JButton("Button 1");
		final JButton b2 = new JButton("Button 2");
		panel.add(b1);
		panel.add(b2);
		
		Dimension size = b1.getPreferredSize();
		b1.setBounds(25, 5, size.width, size.height);
		size = b2.getPreferredSize();
		b2.setBounds(55, 40, size.width*3, size.height*3);
		
		frame.setVisible(true);
	}
}
```
![[Pasted image 20241109101351.png]]
## FlowLayout
```Java
import java.awt.*;
import javax.swing.*;

public class UseFlowLayout{
	public static void main(String[] args){
		final FlowLayout lay = new FlowLayout(FlowLayout.CENTER);
		final MyFrame frame = new MyFrame("FlowLayout Example",lay);
		frame.getMainPanel().add(new JButton("A"));
		frame.getMainPanel().add(new JButton("BCD"));
		frame.getMainPanel().add(new JButton("EDFGHI"));
		frame.getMainPanel().add(new JButton("L"));
		frame.getMainPanel().add(new JButton("MNO"));
		frame.getMainPanel().add(new JButton("PQRSTU"));
		frame.getMainPanel().add(new JButton("V"));
		frame.getMainPanel().add(new JButton("ZZZZZZZ"));
		//frame.pack(); //Window ridemensioning
		frame.setVisible(true);
	}
}
```
![[Pasted image 20241109102740.png]]
## BorderLayout
```Java
import java.awt.*;
import javax.swing.*;

public class UseBorderLayout {
	public static void main(String[] args){
		final JFrame frame = new JFrame();	   
		frame.setTitle("BorderLayout Example"); 
		frame.setSize(320,200);
		
		final JPanel panel = new JPanel();
		panel.setLayout(new BorderLayout());
		frame.getContentPane().add(panel);

		//Every button hans't its default dimension
		panel.add(new JButton("North"),BorderLayout.NORTH);
		panel.add(new JButton("South"),BorderLayout.SOUTH);
		panel.add(new JButton("Center"),BorderLayout.CENTER);
		panel.add(new JButton("East"),BorderLayout.EAST);
		panel.add(new JButton("West"),BorderLayout.WEST);
		
		frame.setVisible(true);
	}
}
```
![[Pasted image 20241109102516.png]]

Also `JFrame` can be specialized for creating our own frame :
```Java
import java.awt.*;
import javax.swing.*;

public class MyFrame extends JFrame{
	
	private final JPanel jp;
	
	public MyFrame(String title, LayoutManager lm){
		super(title); 
		this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		this.setSize(320,200);
		this.jp = new JPanel(lm);
		this.getContentPane().add(this.jp);
	}
	
	public JPanel getMainPanel(){
		return this.jp;
	}

}

public class UseBorderLayout3{
	public static void main(String[] args){
		final BorderLayout b = new BorderLayout();
		b.setHgap(10); 
		b.setVgap(10);
		final MyFrame frame = new MyFrame("BorderLayout Example Bis",b);	   
		final JButton button = new JButton("North");
		final Dimension d = button.getPreferredSize(); 
		button.setPreferredSize(new Dimension(d.width,d.height*2));
		frame.getMainPanel().add(button,BorderLayout.NORTH);
		frame.getMainPanel().add(new JButton("South"),BorderLayout.SOUTH);
		frame.getMainPanel().add(new JButton("Center"),BorderLayout.CENTER);
		frame.getMainPanel().add(new JButton("East"),BorderLayout.EAST);
		frame.getMainPanel().add(new JButton("WestWest"),BorderLayout.WEST);
		
		frame.setVisible(true);
	}
}

```
![[Pasted image 20241109102718.png]]

## BorderLayout + FlowLayout
```Java
public class UseFlowBorder{
	public static void main(String[] args){
		final MyFrame frame = new MyFrame("Flow and Border",new BorderLayout());
		
		final JPanel pNorth = new JPanel(new FlowLayout());
		pNorth.add(new JButton("North 1"));
		pNorth.add(new JButton("North 2"));

		final JPanel pSouth = new JPanel(new FlowLayout(FlowLayout.RIGHT));
		pSouth.add(new JButton("OK"));
		pSouth.add(new JButton("Quit"));

		frame.getMainPanel().add(pNorth,BorderLayout.NORTH);
		frame.getMainPanel().add(pSouth,BorderLayout.SOUTH);
		
		frame.setVisible(true);
	}
}
```
![[Pasted image 20241109102937.png]]
## Mixed Layouts using GridBagLayout
```Java
public class UseMixedLayouts{
	
	public static void main(String[] args){
		final JTextArea textArea = new JTextArea(); 
		textArea.setLineWrap(true);
		final JScrollPane scroll = new JScrollPane(textArea); 
		scroll.setVerticalScrollBarPolicy(ScrollPaneConstants.VERTICAL_SCROLLBAR_ALWAYS);
		
		final JPanel pEastInternal = new JPanel(new GridBagLayout()); 
		pEastInternal.setBorder(new TitledBorder("GridBagLayout"));
		final GridBagConstraints cnst = new GridBagConstraints();
		cnst.gridy = 0;								  
		cnst.insets = new Insets(3,3,3,3);			  
		cnst.fill = GridBagConstraints.HORIZONTAL; 	 
		pEastInternal.add(new JButton("LOAD"),cnst);
		cnst.gridy++;								  
		pEastInternal.add(new JButton("SAVE"),cnst);
		cnst.gridy++;
		pEastInternal.add(new JButton("CLEAR"),cnst);
		cnst.gridy++;
		pEastInternal.add(new JButton("QUIT"),cnst);
		
		final JPanel pEast = new JPanel(new FlowLayout());
		pEast.setBorder(new TitledBorder("FlowLayout"));
		pEast.add(pEastInternal);
		
		final MyFrame frame = new MyFrame("MixedLayouts Example",new BorderLayout(5,5));
		frame.getMainPanel().add(scroll,BorderLayout.CENTER);
		frame.getMainPanel().add(pEast,BorderLayout.EAST);
		frame.getMainPanel().setBorder(new TitledBorder("BorderLayout"));
		frame.setVisible(true);
	}
}
```
![[Pasted image 20241109103251.png]]