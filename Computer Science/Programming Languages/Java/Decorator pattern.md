# Intent
Attach additional responsibilities to an object dynamically. *Decorators* provide a
flexible alternative to *subclassing* for *extending functionality*.

# Motivation

Sometimes we want to add responsibilities to individual objects, not to an entire class. One way to add new features is with *inheritance*, but this is a static design, because each subclass depends on the parent class. 
A more flexible and dynamic approach is taking advantage of the *encapsulation*, in other words *enclosing objects* in way that they can be *added as components* of a class called *decorator*.
The decorator forwards requests to its components and makes additional actions before or after forwarding.

# Application
- Add responsibilities to individual objects dynamically and transparently, without affecting other objects.
- Responsibilities can be withdrawn
- When extension by subclassing is *impractical*, in other terms there multiple extensions that can be added on a large scale.
# Structure
![[Pasted image 20241222163419.png]]

# Participants
- **Component** : it's an interface that defines objects that have responsibilities added dynamically
- **Concrete Component** : objects that attaches to itself new features
- **Decorator** : maintains a reference to the Component interface and defines an interface that conforms the Component interface
- **Concrete Decorator** : add responsibilities to the component

# Consequences
- More flexibility than static inheritance.
- Decorator offers a payas-you-go approach to adding responsibilities. Instead of trying to support all foreseeable features in a complex, customizable class, you can define a simple class and add functionality incrementally with Decorator objects.
- from an object identity point of view, a decorated component is not identical to the component itself. Hence you shouldn't rely on object identity when you use decorators.
- A design that uses Decorator often results in systems composed of lots of little objects that all look alike. The objects differ only in the way they are interconnected, not in their class or in the value of their variables.

# Implementation
1) Interface conformance. A decorator object's interface must conform to the interface of the component it decorates.
2) Omitting the abstract Decorator class.
3) Keeping Component classes lightweight.
4) *Changing the skin of an object versus changing its guts*. We can think of a decorator as a skin over an object that changes its behavior. An alternative is to change the object's guts. The [[Strategy pattern]] is a good example of a pattern for changing the guts.

Strategies are a better choice in situations where the Component class is intrinsically heavyweight, thereby making the Decorator pattern too costly to apply. In the Strategy pattern, the component forwards some of i ts behavior to a separate strategy object. The Strategy pattern lets us alter or extend the component's functionality by replacing the strategy object.
# Examples

## Pizza decorator

```Java
//Component
public interface Pizza {
    int getCost(); 
    String getIngredients(); 
}

//Concrete component
public class Margherita implements Pizza {
    public int getCost(){
        return 650;
    }

    public String getIngredients(){
        return "Pomodoro, Mozzarella";
    }
}

//Decorator : in this case abstract class but it can be an interface
public abstract class IngredientDecorator implements Pizza {
    protected final Pizza decorated;
    
    protected IngredientDecorator(Pizza decorated){
        this.decorated = decorated;
    }

    public int getCost(){
        return this.decorated.getCost();
    }

    public String getIngredients(){
        return this.decorated.getIngredients();
    }
}

// Decorator class
public class BasicIngredient extends IngredientDecorator {
	
	private final String name;
	private final int cost;
	
	public BasicIngredient(String name, int cost,Pizza p){
		super(p);
		this.name = name;
		this.cost = cost;
	}
	
	public int getCost(){
		return super.getCost() + this.cost;
	}
	
	public String getIngredients(){
		return super.getIngredients() + ", " + this.name;
	}
}

public class GlutFree extends IngredientDecorator {
	
	public GlutFree(Pizza p){
		super(p);
	}
	
	public int getCost(){
		return (int)(super.getCost() * 1.1);
	}
	
	public String getIngredients(){
		return "[GF] "+super.getIngredients();
	}
}

// Decorator components
public class Funghi extends BasicIngredient {
	
	public Funghi(Pizza p){
		super("AbstractIngredient",100,p);
	}	
}

public class Salsiccia extends BasicIngredient {
	
	public Salsiccia(Pizza p){
		super("Salsiccia",150,p);
	}	
}


public void test(){
		Pizza p1 = new GlutFree(new Salsiccia(new Salsiccia(new Margherita())));
		System.out.println(p1.getCost()+" "+p1.getIngredients());
		assertEquals(p1.getCost(),1045);// (6.50 + 1.50 + 1.50) + 10%
		assertEquals(p1.getIngredients(),"[GF] Pomodoro, Mozzarella, Salsiccia, Salsiccia");
		
	}
```

## Bank Account (Factory)
```Java
// Component
public interface BankAccount {
   int getBalance();
   void setBalance();
   void deposit(int amount);
   boolean withdraw(int amount);
}

// Decorator class
private static class BankAccountDecorator implements BankAccount {
	private final BankAccount base;
	public BankAccountDecorator(BankAccount base){
		this.base = base;
	}
	public int getBalance() {
		return base.getBalance();
	}
	public void deposit(int amount) {
		base.deposit(amount);
	}
	public boolean withdraw(int amount) {
		return base.withdraw(amount);
	}
	
	protected void setBalance(int amount) {
		base.setBalance(amount);
	}
}

// Concrete component
private BankAccount refinedCreateBasic() {
	return new BankAccount() {

		private int balance = 0;

		@Override
		public int getBalance() {
			return this.balance;
		}

		@Override
		protected void setBalance(int amount){
			this.balance = amount;
		}

		@Override
		public void deposit(int amount) {
			this.balance = this.balance + amount;
		}

		@Override
		public boolean withdraw(int amount) {
			if (this.balance < amount){
				return false;
			}
			this.balance = this.balance - amount;
			return true;
		}
	};
}

// Decorator component
private BankAccount refinedCreateWithFee(UnaryOperator<Integer> feeFunction, BankAccount base) {
	return new BankAccountDecorator(base){
		@Override
		public boolean withdraw(int amount){
			return super.withdraw(amount + feeFunction.apply(amount));
		}
	};
}

//Test - Factory
public BankAccount createBasic() {
	return refinedCreateBasic();
}

public BankAccount createWithFee(UnaryOperator<Integer> feeFunction) {
	return refinedCreateWithFee(feeFunction, refinedCreateBasic());
}

public BankAccount createWithBlock(BiPredicate<Integer, Integer> blockingPolicy) {
	return new BankAccountDecorator(refinedCreateBasic()){
		private boolean blocked = false;

		@Override
		public boolean withdraw(int amount){
			if (blocked) {
				return false;
			}
			if (blockingPolicy.test(amount, super.getBalance())){
				blocked = true;
				return false;
			}
			return super.withdraw(amount);
		}
	};
}
```

## Time Sheet
```Java
public class TimeSheetFactoryImpl implements TimeSheetFactory {

    // An implementation of a Timesheet as an immutable class with activities, days, and an association of activities+days to hours
    // Since it is immutable, we use a record, which automatically gives fields, getters, constructors, hashcode, equals, toString.
    //Concrete Component
    private static record TimeSheetData(Set<String> activities, Set<String> days, BiFunction<String, String, Integer> fun) implements TimeSheet {

        @Override
        public int getSingleData(String activity, String day) {
            return fun.apply(activity,day);
        }

        @Override
        public boolean isValid() {
            return true;
        }
    }

    // We handle various validy policies, and combine them, by the decorator pattern
    private static class TimeSheetDecorator implements TimeSheet {
        private final TimeSheet base;

        public TimeSheetDecorator(TimeSheet base) {
            this.base = base;
        }

        public Set<String> activities() {
            return base.activities();
        }

        public Set<String> days() {
            return base.days();
        }

        public int getSingleData(String activity, String day) {
            return base.getSingleData(activity, day);
        }

        public boolean isValid() {
            return base.isValid();
        }

        public int sumPerActivity(String activity) {
            return days().stream()
                            .map(day -> getSingleData(activity,day))
                            .collect(Collectors.summingInt(i->i));
        }

        public int sumPerDay(String day) {
            return activities().stream()
                            .map(act -> getSingleData(act,day))
                            .collect(Collectors.summingInt(i->i));
        }
    }

    @Override
    public TimeSheet ofRawData(List<Pair<String, String>> data) {
        var activities = data.stream().map(Pair::get1).sorted().collect(Collectors.toSet());
        var days = data.stream().map(Pair::get2).sorted().collect(Collectors.toSet());
        return new TimeSheetData(
            activities,
            days, 
            (a,d) -> (int)data.stream().filter(p -> p.get1().equals(a) && p.get2().equals(d)).count());
    }

    private TimeSheet decorateWithBoundsPerActivity(TimeSheet base, Map<String, Integer> bounds) {
        return new TimeSheetDecorator(base){
            public boolean isValid() {
                return base.isValid() && bounds.entrySet()
                        .stream()
                        .allMatch(e -> sumPerActivity(e.getKey()) <= e.getValue());
            }
        };

    }

    private TimeSheet decorateWithBoundsPerDay(TimeSheet base, Map<String, Integer> bounds) {
        return new TimeSheetDecorator(base){
            public boolean isValid() {
                return base.isValid() && bounds.entrySet()
                        .stream()
                        .allMatch(e -> sumPerDay(e.getKey()) <= e.getValue());
            }
        };

    }

    private TimeSheet decorateWithBounds(TimeSheet base, Map<String, Integer> boundsPerActivity, Map<String, Integer> boundsPerDay){
        return decorateWithBoundsPerActivity(decorateWithBoundsPerDay(base, boundsPerDay), boundsPerActivity);
    }

    @Override
    public TimeSheet withBoundsPerActivity(List<Pair<String, String>> data, Map<String, Integer> bounds) {
        return decorateWithBoundsPerActivity(ofRawData(data), bounds);
    }

    @Override
    public TimeSheet withBoundsPerDay(List<Pair<String, String>> data, Map<String, Integer> bounds) {
        return decorateWithBoundsPerDay(ofRawData(data), bounds);
    }

    @Override
    public TimeSheet withBounds(List<Pair<String, String>> data, Map<String, Integer> boundsOnActivities, Map<String, Integer> boundsOnDays) {
        return decorateWithBounds(ofRawData(data), boundsOnActivities, boundsOnDays);
    }

}
```