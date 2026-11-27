# 2024
## a01a
e1
How to generate a stream from an iterator:
```Java
private <X> Iterator<X> iteratorAsStream(Iterator<X> iterator, Stream<X> stream) {
        return Stream.concat(Stream.iterate(iterator, Iterator::hasNext, i -> i).map(Iterator::next), stream).iterator();
    }
```
Also external classes can get access to private fields of inner classes:
```Java
private class ComposableParserImpl<X> implements ComposableParser<X> {

        private Set<Iterator<X>> set;

        public ComposableParserImpl(Set<Iterator<X>> set) {
            this.set = set;
        }

        @Override
        public boolean parse(X t) {
            this.set = this.set.stream()
                .filter(Iterator::hasNext)
                .filter(i -> i.next().equals(t))
                .collect(toSet());
            return !this.set.isEmpty();
        }

        @Override
        public boolean end() {
            return this.set.stream().allMatch(i -> !i.hasNext());
        }
    }

    public <X> Stream<Iterator<X>> getIteratorsAsStream(ComposableParser<X> p) {
        return ((ComposableParserImpl<X>)p).set.stream();
    }
```
e2
How to check something between two position :
```Java
    @Override
    public boolean isInRange(Position value) {
        return !this.rectangle.isEmpty()  && !isAVertex(value) &&  
        ( 
            ( value.x() == getMaxAsVertex(p -> p.x()) || value.x() == getMinAsVertex(p -> p.x()) ) &&
            (value.y() <= getMaxAsVertex(p -> p.y()) && value.y() >= getMinAsVertex(p -> p.y())) || 
            ( ( value.y() == getMaxAsVertex(p -> p.y()) || value.y() == getMinAsVertex(p -> p.y()) ) &&
            (value.x() <= getMaxAsVertex(p -> p.x()) && value.x() >= getMinAsVertex(p -> p.x())))
        );
    }
```
## a01b
e1 
Caching the solution is a good thing, also use functions.
e2
Remember how to select a group of positions by their coordinates: 
```Java
@Override
    public boolean isSelected(Position value) {
        var up = this.vertices.get(this.vertices.get(0).y() > this.vertices.get(1).y() ? 1 : 0);
        var down = this.vertices.get(this.vertices.get(0).y() > this.vertices.get(1).y() ? 0 : 1);
        return this.vertices.size() == 2 &&
                value.y() > up.y() && value.y() < down.y() && 
                value.x()-value.y() <= up.x()-up.y() && 
                value.x()-value.y() >= down.x()-down.y() &&
                value.x()+value.y() >= up.x()+up.y() && 
                value.x()+value.y() <= down.x()+down.y(); 
    }
```
## a01c
e1  ---
Window method copied, how to pack something in a stream that was previously taken, then skip the first "size" iterations
e2
A rectangle with points inside the rectangle.

# 2023
## a01a
e1
a way to define a dynamic map, where there are two sets that corresponds to an amount of hours; Furthermore, look at the method `addHour` that calls itself for updating the function.
e2
Pay more attention for linked list and how to use them for a GUI.

## a01b
e1
dynamic map, see it only for some good implementations for the factory method
e2
same as a01a, linked list
## a01c
e1 
Decorator
e2
  
## a01d
e1
good, same as before
e2

## a02a
e1
Builder pattern, use stream instead of list
e2

## a02b
e1 --
There is a lot about it: `Stream.empty` is  a useful std method and `applyRules` is quite challenging method
e2


## a02c
e1 --
Strategy, sub lists and use of flat map
e2

## a03a
e1
use of `subList`, where it checks from *index* to *final index*
e2 


## a03b
e1
Good, but the last method was incomplete, it can be done with `subList` :
```Java
@Override
    public <X> InfiniteIterator<List<X>> window(InfiniteIterator<X> i, int n) { 
        return general(Stream.iterate(Collections.<X>emptyList(), l -> Stream.concat(l.stream(), Stream.of(i.nextElement())).toList())
            .filter(l -> l.size() >= n)
            .map(l -> l.subList(l.size()-n, l.size()))
            .iterator()); 
    }
```
e2

## a04a
e1
If something is in common in the implementation, check a way to define abstract methods
e2 --
How to find a zig-zag path
```Java
private Set<Position> reachedAtRow(int i){
        if (i == 0){
            return this.marks.stream().filter(p -> p.y() == 0).collect(Collectors.toSet());
        }
        var s = reachedAtRow(i-1);
        return s.stream().flatMap(p -> this.marks.stream().filter(q -> q.y() == i).filter(q -> Math.abs(p.x() - q.x()) == 1)).collect(Collectors.toSet());
    }
```

## a05
e1
Abstract
e2
```Java
private Stream<Position> validNeighbours(Position position){
        return Stream.of(
            new Position(1,0), new Position(0,1), new Position(-1,0), new Position(0,-1),
            new Position(1,1), new Position(-1,1),  new Position(-1,-1), new Position(1,-1)
        ).map(p -> new Position(position.x() + p.x(), position.y() + p.y()))
        .filter(p -> p.x() >= 0 && p.y() >= 0)
        .filter(p -> p.x() < this.size && p.y() < this.size);
    }
```
## a06
e1 ZipCombiner

e2

# 2022

## a01a
e1 SubSequenceCombiner
Strategy pattern and :
```Java
@Override
    public <X> SubsequenceCombiner<X, List<X>> tripletsToList() {
        return generic(l -> l.size() == 3, l -> new ArrayList<>(l)); // New Reference to the list
    }
```
e2 
```Java
public class LogicImpl implements Logic {

    private static final List<Integer> ASCENDING = List.of(0,1,2);
    private static final List<Integer> DESCENDING = List.of(2,1,0);
    
    private final List<Pair<Integer,Integer>> line = new LinkedList<>();
    private final Set<Pair<Integer,Integer>> enabled = new HashSet<>();

    @Override
    public boolean toggle(int x, int y) {
        var p = new Pair<>(x,y);
        if (this.enabled.contains(p)){
            this.enabled.remove(p);
            this.line.clear();
        } else {
            this.enabled.add(p);
            this.line.add(p);
            if (this.line.size()>3){
                this.line.remove(0);
            }
        }
        return this.enabled.contains(p);
    }

    private boolean inDiagonal(List<Integer> list){
        return list.equals(ASCENDING) || list.equals(DESCENDING);
    }

    @Override
    public boolean isOver() {
        System.out.println(line);
        if (this.line.size() < 3){
            return false;
        }
        int minx = this.line.stream().map(Pair::getX).mapToInt(i -> i).min().getAsInt();
        int miny = this.line.stream().map(Pair::getY).mapToInt(i -> i).min().getAsInt();
        List<Integer> sx = this.line.stream().map(Pair::getX).map(x -> x - minx).collect(Collectors.toList());
        List<Integer> sy = this.line.stream().map(Pair::getY).map(y -> y - miny).collect(Collectors.toList());
        return inDiagonal(sx) && inDiagonal(sy);
    }           
}
```

## a01b
e1 Flattener
Strategy pattern and use bifunction in combination with stream to folder list
e2
```Java
@Override
    public void hit(int x, int y) {
        var toggles = Stream.of(x-1,x+1)
                .filter(k -> k >= 0)
                .filter(k -> k < size)
                .flatMap(i -> Stream.of(y-1,y+1)
                        .filter(k -> k >= 0)
                        .filter(k -> k < size)
                        .map(j -> new Pair<>(i, j)))
                .map(p -> this.toggle(p.getX(),p.getY()))
                .collect(Collectors.groupingBy(b->b));
        this.over = toggles.size()==2 && toggles.get(false).size()==3 && toggles.get(true).size()==1;
    }

private boolean toggle(int x, int y){
        var p = new Pair<>(x,y);
        if (this.enabled.contains(p)){
            this.enabled.remove(p);
            return false;
        } 
        this.enabled.add(p);
        return true;
    }
```

## a02a
e1 RecursiveIterator
Recursion
e2 Bishop
```Java
private boolean inLine(Pair<Integer, Integer> p1, Pair<Integer, Integer> p2){
	return p1.getX() - p1.getY() == p2.getX() - p2.getY() || p1.getX() + p1.getY() == p2.getX() + p2.getY();
}

private void clear() {
	this.available = IntStream.range(0, size)
			.mapToObj(i -> i)
			.flatMap(x -> IntStream.range(0, size)
					.mapToObj(y -> new Pair<>(x,y)))
			.collect(Collectors.toCollection(()->new HashSet<>()));
	this.bishops.clear();
}

private void dropUnavailable(Pair<Integer, Integer> bishop) {
	var iterator = this.available.iterator();
	while (iterator.hasNext()){
		var position = iterator.next();
		if (!isBishop(position.getX(), position.getY()) && inLine(position, bishop)){
			iterator.remove(); // For remoove from the collection
		}
	}
}
```

## a02b
e1 Cursor
Iterator
e2 Three Stars
Simulation about diagonals
```Java
@Override
    public Optional<Set<Pair<Integer, Integer>>> checkThree() {
        return checkThreeStars(p -> p.getX()-p.getY()); // X-Y is an invariant for cells in the same diagonal
    }

    private Optional<Set<Pair<Integer, Integer>>> checkThreeStars(Function<Pair<Integer,Integer>, Integer> fun) {
        return this.selected
            .stream()
            .collect(Collectors.groupingBy(fun::apply, Collectors.counting()))
            .entrySet()
            .stream()
            .filter(e -> e.getValue() == 3L)
            .map(e -> e.getKey())
            .findAny()
            .map(i -> this.all().filter(p -> fun.apply(p) == i).collect(Collectors.toSet()))
        ;
    }

    private Stream<Pair<Integer,Integer>> all(){
        return IntStream.range(0, size)
            .mapToObj(i -> i)
            .flatMap(x -> IntStream.range(0, size)
                .mapToObj(y -> new Pair<>(x,y)));
    }
```

## a03a
e1 Parser
Check for fromGraph
e2 Chess with tower --
ATTENTION
```Java
public interface Logic {
    enum Player { HUMAN, COMPUTER }
    boolean humanMove(int x, int y);
    void computerMove();
    boolean over();
    Pair<Integer, Integer> getPosition(Player player);
    void reset(); 
}

public class LogicImpl implements Logic {

    private final int size;
    private final Random random = new Random();
    private final Map<Player, Pair<Integer,Integer>> positions = new HashMap<>();

    public LogicImpl(int size) {
        this.size = size;
        this.reset();
    }

    private Pair<Integer,Integer> randomPosition(){
        return new Pair<>(this.random.nextInt(this.size), this.random.nextInt(this.size));
    }

    private List<Pair<Integer,Integer>> allAttacked(Pair<Integer,Integer> position){
        return IntStream.range(0, this.size)
                .mapToObj(i->i)
                .flatMap(x -> IntStream.range(0, this.size)
                        .mapToObj(y -> new Pair<>(x,y)))
                .filter(p -> attacks(p, position))
                .collect(Collectors.toList());
    }

    private boolean attacks(Pair<Integer,Integer> p1, Pair<Integer,Integer> p2){
        return !p1.equals(p2) && (p1.getX() == p2.getX() || p1.getY() == p2.getY());
    }

    @Override
    public final void reset() {
        this.positions.put(Player.HUMAN, this.randomPosition());
        do {
            this.setPosition(Player.COMPUTER, this.randomPosition());
        } while (this.attacks(this.getPosition(Player.COMPUTER),this.getPosition(Player.HUMAN)));
    }

    @Override
    public boolean humanMove(int x, int y) {
        var position = new Pair<>(x,y);
        if (this.attacks(this.getPosition(Player.HUMAN),position)){
            this.setPosition(Player.HUMAN, position);
            return true;
        }
        return false;
    }

    @Override
    public boolean over() {
        return this.getPosition(Player.COMPUTER).equals(this.getPosition(Player.HUMAN));
    }

    @Override
    public void computerMove() {
        var valid = this.allAttacked(this.getPosition(Player.COMPUTER));
        if (valid.contains(this.getPosition(Player.HUMAN))){
            this.setPosition(Player.COMPUTER, this.getPosition(Player.HUMAN));
        } else {
            this.setPosition(Player.COMPUTER, valid.get(this.random.nextInt(valid.size())));
        }
    }

    @Override
    public Pair<Integer, Integer> getPosition(Player player) {
        return this.positions.get(player);
    }

    private void setPosition(Player player, Pair<Integer,Integer> position){
        this.positions.put(player,position);
    }
}
```

## a03b
e1 LazyTree
Always check for a general solution already given, also pay attention for supplier things
e2 -- Chess with Pedons
```Java
public interface Logic {

    enum Player { HUMAN, COMPUTER }

    boolean humanMove(int x, int y);

    boolean over();

    boolean hasPiece(Player player, int x, int y);

    void reset();
    
}

public class LogicImpl implements Logic {

    private final int size;
    private final Map<Pair<Integer,Integer>, Player> positions = new HashMap<>();
    private Random random = new Random();

    public LogicImpl(int size) {
        this.size = size;
        this.reset();
    }

    private boolean validPosition(Pair<Integer, Integer> position){
        return position.getX() >= 0 && position.getY() >= 0 && position.getX() < this.size && position.getY() < this.size;
    }

    private List<Pair<Integer,Integer>> moves(Pair<Integer,Integer> position){
        List<Pair<Integer,Integer>> moves = Stream.of(position)
            .flatMap(p -> Stream.of(new Pair<>(1,-1), new Pair<>(-1,-1))
                        .map(pp -> new Pair<>(pp.getX()+p.getX(), pp.getY()+p.getY())))
            .filter(this::validPosition)
            .filter(this.positions::containsKey)
            .filter(p -> !this.positions.get(p).equals(Player.HUMAN))
            .collect(Collectors.toList());
        if (!moves.isEmpty()){
            return moves;
        }
        return Stream.of(new Pair<>(position.getX(), position.getY()-1))
                .filter(this::validPosition)
                .filter(p -> !this.positions.containsKey(p))
                .collect(Collectors.toList());
    }

    @Override
    public final void reset() {
        this.positions.clear();
        for (int i=0; i<this.size; i++){
            this.positions.put(new Pair<>(i,this.random.nextInt(2)), Player.COMPUTER);
            this.positions.put(new Pair<>(i,size-1), Player.HUMAN);
        }
    }

    private boolean doMove(Pair<Integer, Integer> position){
        var player = this.positions.get(position);
        var moves = new LinkedList<>(this.moves(position));
        if (moves.isEmpty()){
            return false;
        }
        Collections.shuffle(moves);
        this.positions.remove(position);
        this.positions.put(moves.get(0), player);
        return true;
    }

    @Override
    public boolean humanMove(int x, int y) {
        return doMove(new Pair<>(x,y));
    }

    @Override
    public boolean over() {
        return this.positions.values().stream().distinct().count() == 1;
    }

    @Override
    public boolean hasPiece(Player player, int x, int y) {
        return Optional.ofNullable(this.positions.get(new Pair<>(x,y))).filter(pl -> pl == player).isPresent();
    }

}
```

## a04
e1 BankAccount
Template method
e2 -- Rook and king
```Java
public class LogicsImpl implements Logics {

    private final int size;
    private Pair<Integer,Integer> posKing;
    private Pair<Integer,Integer> posRook;
    private final Random random = new Random();

    public LogicsImpl(int size) {
        this.size = size;
    }


    @Override
    public boolean rookMove(Pair<Integer, Integer> position) {
        if (position.getX() == this.posRook.getX() && this.posRook.getY() == position.getY()) {
            return false;
        }
        if (position.getX() != this.posRook.getX() && this.posRook.getY() != position.getY()) {
            return false;
        }
        if (position.getX() == this.posRook.getX() && position.getX() == this.posKing.getX() && inBetween(position.getY(), this.posRook.getY(), this.posKing.getY())){
            return false;
        }
        if (position.getY() == this.posRook.getY() && position.getY() == this.posKing.getY() && inBetween(position.getX(), this.posRook.getX(), this.posKing.getX())){
            return false;
        }
        this.posRook = new Pair<>(position.getX(),position.getY());
        return true;
    }

    private boolean inBetween(int y1, int y2, int y) {
        return y1 < y2 ? (y > y1 && y < y2) : (y > y2 && y < y1);
    }

    @Override
    public boolean isOver() {
        return this.posKing.equals(this.posRook);
    }


    @Override
    public void reset() {
        posKing = new Pair<>(random.nextInt(size), random.nextInt(size));
        do {
            posRook = new Pair<>(random.nextInt(size), random.nextInt(size));
        } while(posRook.equals(posKing));
    }


    @Override
    public void kingMove() {
        if (Math.abs(posKing.getX() - posRook.getX()) <= 1 && Math.abs(posKing.getY() - posRook.getY()) <= 1) {
            this.posKing = this.posRook;
        } else {
            Pair<Integer,Integer> newPos;
            do {
                newPos = new Pair<Integer,Integer>(this.posKing.getX() + random.nextInt(3) - 1,this.posKing.getX() +  random.nextInt(3) - 1);
            } while(newPos.equals(this.posKing) || notInBounds(newPos));
            this.posKing = newPos;
        }
    }

    private boolean notInBounds(Pair<Integer, Integer> newPos) {
        return newPos.getX() < 0 || newPos.getX() >= size ||
            newPos.getY() < 0 || newPos.getY() >= size;
    }

    @Override
    public boolean hasRook(Pair<Integer,Integer> pair) {
        return this.posRook.getX() == pair.getX() && this.posRook.getY() == pair.getY();
    }


    @Override
    public boolean hasKing(Pair<Integer,Integer> pair) {
        return this.posKing.getX() == pair.getX() && this.posKing.getY() == pair.getY();
    }

}
```

# 2021

## a01a
e1
check for strategy pattern and how it is used with optional classes
e2
Well done, check only for `rangeClosed` for streams.
## a01b
e1
how to use optional class for invalid states and how it can adapt while using streams with `takeWhile` , `takeWhile(Optional::isPresent)` : 
```Java
private <E> Optional<E> convertIntoOptional(Supplier<E> supplier) {
	try {
		return Optional.of(supplier.get());
	} catch (Exception e) {
		return Optional.empty();
	}
}

private <E> Stream<E> takePresent(Supplier<E> supplier) {
	return Stream.generate(() -> convertIntoOptional(supplier::get))
		.takeWhile(Optional::isPresent)
		.map(o -> o.get());
}
```
e2 --
Checking for how the logic can contaminate the view.
## a01c
e1
IMPORTANT : THERE ARE FILES IN THERE, CHECK FOR SOME CLARIFICATIONS
Work with parallel iterators ( iterators of pair)
```Java
    @Override
    public EventHistory<String> fromFile(String file) throws IOException {
        Map<Double, String> map = new HashMap<>();
        // FILE READER 
        try ( BufferedReader br = new BufferedReader(new FileReader(file))){
			String str = null;
			while ((str = br.readLine())!=null) {
				String[] s = str.split(":",2);
				map.put(Double.parseDouble(s[0]), s[1]);
			}
		}
        return this.fromMap(map);
    }
```
e2
Good job, look only for optional classes
## a02a
e1 --
Look for `merge` function for maps and how to use properly `Predicate` class
```Java
@Override
public boolean isValid(Map<String, Double> dietMap) {
	Map<Nutrient,Double> calories = new HashMap<>();
	for (var entry : dietMap.entrySet()) {
		for (var entry2 : this.map.get(entry.getKey()).entrySet()) {
			calories.merge(entry2.getKey(),
				( entry.getValue() / 100 ) * entry2.getValue(),
				(i,j) -> i + j);
		}
	}
	// System.out.println(calories);
	return this.getConstraints().stream().allMatch(p -> isConstraintValid(p, calories));
}
```
e2
the first time where button and pair are swapped

## a02b
e1 
Exercises with the exchange of predicate about university program
e2
Another example of inversion between buttons and pairs, and the implementation of a map of pairs and directions, also there is the use of `ofNullable` (`Optional` class)
## a02c
e1
Perfect use of template method pattern
e2
It is a flipper with random features, see it if you want to have an hint about *listing* possibilities for random moving

## a03a
e1
Chain of Responsibility pattern, Linked list for recursive calls ( uses poll for extracting the head of the list)
e2
difference between left and right, and also some good methods to recognize if a rectangle inside the GUI
## a03b
e1
Strategy, pay attention when you use composition of generic objects
e2
Linked list
## a03c
e1 --
Use of strategy and `flatMap` with `Optional`, remember that if you pass something empty in a flat map, it will return an optional empty. Also read every single feature about each method, because sometimes it will help you, (maybe take a look on test section). Furthermore, it's not forbidden to use some support methods created by you.
```Java
private <E> Optional<List<E>> takeTail(List<E> l) {
        if (l.isEmpty()) {
            return Optional.empty();
        }
        List<E> l2 = new ArrayList<>(l);
        l2.removeFirst();
        return Optional.of(l2);
    }

private DeterministicParser many(DeterministicParser parser) {
        return tokens -> parser.accepts(tokens).isEmpty() ? Optional.of(tokens) : this.sequence(parser, many(parser)).accepts(tokens);
    }
@Override
    public DeterministicParser sequence(DeterministicParser first, DeterministicParser second) {
        return tokens -> first.accepts(tokens).flatMap(second::accepts);   //flatMap per fare le cose in successione
    }

private DeterministicParser increasingSequenceOfNumbers(int lowerBound) {
	return tokens -> {
		var res = filteredSequence(s -> Integer.parseInt(s) > lowerBound).accepts(tokens);
		return  res.flatMap(t -> increasingSequenceOfNumbers(Integer.parseInt(tokens.get(0))).accepts(t)).or(() -> Optional.of(tokens));
	};
}
```
e2 --
```Java
private static enum Direction {
	UP(0,-1), RIGHT(1,0), DOWN(0,1), LEFT(-1,0);
	
	int x;
	int y;
	Direction(int x, int y){
		this.x = x;
		this.y = y;
	}
	
	Direction next() {
		return Direction.values()[(this.ordinal()+1) % Direction.values().length];
	}
}
// RETURN THE SET WHEN YOU WANT TO UPDATE ALL THE BUTTONS
private void updateCells(Set<Pair<Integer,Integer>> p) {
    	for (var entry: this.cells.entrySet()) {
			entry.getValue().setText(logics.obstacleAt(entry.getKey().getX(),entry.getKey().getY()) ? "O" : " ");
			if (p.contains(entry.getKey())) {
				entry.getValue().setText("*");
			}
		}
	}
```


## a04
e1 --
A test about Optionals, also see how does it work a map/flat map in general
```Java
public <A> Either<Exception, A> of(Supplier<A> computation) {
	try {
		return EitherImpl.createSuccess(computation.get());
	} catch (Exception e) {
		return EitherImpl.createFailure(e);
	}
}

@Override
    public <B1> Either<A, B1> map(Function<B, B1> function) {
        return fold(EitherImpl::createFailure, function.andThen(EitherImpl::createSuccess));  //andThen per concatenare due funzioni
    }
```
e2 --> the table with numbers and operations
Check for identify an implementation for making a path defined by the operation chosen.
Use an enumerator that covers all possible operations.

## a05
e1
Good exercise for generics and type checking, along a massive use of streams and *recursive calls*.
e2
the same as a04

## a06
e1
Template method, passing the body method by the constructor using `Function` class
e2
`Optional` + `Iterator` + `List` is a combo.