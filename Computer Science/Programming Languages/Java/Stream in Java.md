`Stream` in Java are objects that represents a flux of homogeneous data and they can be used one time starting from the creation. 
It can interpreted as the declarative counterpart of `Iterator` class, in fact we won't use them with other constructs but only declaring which method is going to be used.

Data are created only when the stream is used to process them, not as a set of objects like `Collection` classes. 
With their declarative nature, we can write which transformation we want to use, like a "chain" of methods that filters our requests.
In addition, we can parallel some operations, with the support of the class `Thread` and an internal multithreaded system.

# Pipeline
First of all, streams can be composed by some methods that defines the *pipeline* of our stream:
- *sink* : Collections, arrays, I/O sources, generating functions,...
- *transformations* : maps, filters,...
- *terminators* : methods that aggregates data or reduce them in a value, array/Collection or Iterator.

Example:
- `Person` class:
```Java
public class Person {

	private final String name;
	private final Optional<String> city;
	private final double income;
	private final Set<String> jobs;

	public Person(String name, String city, double income, String... jobs) {
		this.name = Objects.requireNonNull(name);
		this.city = Optional.ofNullable(city); 
		this.income = income;
		this.jobs = new HashSet<>(List.of(jobs)); // conversione a set
	}

	public String getName() {
		return this.name;
	}

	public Optional<String> getCity() {
		return this.city;
	}

	public double getIncome() {
		return this.income;
	}

	public Set<String> getJobs() {
		return Collections.unmodifiableSet(this.jobs);
	}

	@Override
	public String toString() {
		return "PersonImpl [name=" + name + ", city=" + city + ", income=" + income
				+ ", jobs=" + jobs + "]";
	}

	@Override
	public int hashCode() {
		return Objects.hash(name, city, income, jobs);
	}

	@Override
	public boolean equals(Object obj) {
		if (this == obj) {
			return true;
		}
		if (!(obj instanceof Person)) {
			return false;
		}
		Person other = (Person) obj;
		return Objects.equals(name, other.name) && Objects.equals(city, other.city)
				&& Double.doubleToLongBits(income) == Double.doubleToLongBits(other.income)
				&& Objects.equals(jobs, other.jobs);
	}

}
```
- Test with `Stream`:
```Java
public class UseStreamsOnPerson2 {

	public static void main(String[] args) {

		final List<Person> list = new ArrayList<>();
		list.add(new Person("Mario", "Cesena", 20000, "Teacher"));
		list.add(new Person("Rino", "Forlì", 30000, "Football player"));
		list.add(new Person("Lino", "Cesena", 110000, "Chef", "Artist"));
		list.add(new Person("Ugo", "Cesena", 20000, "Secretary"));
		list.add(new Person("Marco", null, 4000, "Contractor"));

		// Jobs of people from Cesena
		final String res = list.stream()
				.filter(p -> p.getCity().filter(x -> x.equals("Cesena")).isPresent())
				.flatMap(p -> p.getJobs().stream())
				.distinct()
				.collect(Collectors.joining("|", "[[", "]]"));
		System.out.println(res);

		// Average income of chefs
		final double avg = list.stream()
				.filter(p -> p.getJobs().contains("Chef"))
				.mapToDouble(Person::getIncome)
				.average().getAsDouble();

		System.out.println(avg);
		System.out.println(list.stream()
				.filter(p -> p.getJobs().contains("Chef"))
				.mapToDouble(Person::getIncome)
				.average());
	}
}
```

# java.util.stream 

When we want to use `Stream` we have to import `java.util.stream` package that provides interfaces and classes for building our streams.
The interface `Stream<X>` is the standard way for defining stream objects and factorizing them. 

There are specific interfaces for primitive data, like `DoubleStream<X>, IntStream<X>`,..
They have more different methods than normal streams, like `sum`, but they don't have methods like `mapToObj` or `boxed`.

If we want to create a collection starting from a stream, we can use `Collector<T,A,R>`, representing a reduction of our stream.
`BaseStream<T, S extends BaseStream<T,S>>` is a wrapper of `Stream` and adds methods for parallel and sequential streams, like `sequential`, `parallel`, `unordered`,.... The second parameter represents the type of stream itself and it's used internally when we create streams.

# Examples
## Creation
```Java
final List<Integer> li = List.of(10,20,30,5,6,7,10,20,100);
System.out.print("Collection: ");
li.stream()
		.forEach(i->System.out.print(" "+i));

System.out.print("\nEmpty: ");
Stream.empty()
		.forEach(i->System.out.print(" "+i));

System.out.print("\nFromValues: ");
Stream.of("a","b","c")
		.forEach(i->System.out.print(" "+i));

System.out.print("\nIterate(+1): ");
Stream.iterate(0,i->i+1)  // 0,1,2,3,...
		.limit(20)
		.forEach(i->System.out.print(" "+i));

System.out.print("\nSuppl(random): ");
Stream.generate(() -> Math.random()) 
		.limit(5)
		.forEach(i -> System.out.print(" " + i));
// DoubleStream.generate(()->Math.random())... stream unboxed

System.out.print("\nConcat: ");
Stream.concat(Stream.of("a", "b"), Stream.of(1, 2))
		.forEach(i -> System.out.print(" " + i));

System.out.print("\nBuilder: ");
Stream.builder()
		.add(1)
		.add(2)
		.build()
		.forEach(i -> System.out.print(" " + i));

System.out.print("\nRange: ");
IntStream.range(0, 20) // 0,1,..,19
		.forEach(i -> System.out.print(" " + i));
```
Examples with files:
```Java
final Path dirPath = FileSystems.getDefault().getPath(aDir);
		
System.out.println("Found below "+aDir);
Files.find(dirPath, 2, (a,b)->true).forEach(System.out::println);

System.out.println("List directory "+aDir);
Files.list(dirPath).forEach(System.out::println);

final Path filePath = FileSystems.getDefault().getPath(aFile);

System.out.println("Content of "+aFile);
Files.lines(filePath).forEach(System.out::println);

System.out.println("Content of "+aFile+": ");
Files.lines(filePath,StandardCharsets.ISO_8859_1).forEach(System.out::println);

System.out.println("Stream from string..");
"Hellò!".chars().mapToObj(i->(char)i).forEach(System.out::println);
```
## Transformation
```Java
final List<Integer> li = List.of(10, 20, 30, 5, 6, 7, 10, 20, 100);
System.out.print("All\t\t :");
li.stream()
		.forEach(i -> System.out.print(" " + i));

System.out.print("\nFilter(>10)\t :");
li.stream()
		.filter(i -> i > 10)
		.forEach(i -> System.out.print(" " + i));

System.out.print("\nMap(N:i+1)\t :");
li.stream()
		.map(i -> "N:" + (i + 1)) 
		.forEach(i -> System.out.print(" " + i));

System.out.print("\nflatMap(i,i+1)\t :");
li.stream()
		.flatMap(i -> List.of(i, i + 1).stream())   //Important: converts into stream 
		.map(String::valueOf)
		.map(" "::concat)
		.forEach(System.out::print);

System.out.print("\nDistinct\t :");
li.stream().distinct()
		.forEach(i -> System.out.print(" " + i));

System.out.print("\nSorted(down)\t :");
li.stream().sorted((i, j) -> j - i) 
		.forEach(i -> System.out.print(" " + i));

System.out.print("\nPeek(.)\t\t :");
li.stream().peek(i -> System.out.print(".")) 
		.forEach(i -> System.out.print(" " + i));

System.out.print("\nLimit(5)\t :");
li.stream().limit(5) 
		.forEach(i -> System.out.print(" " + i));

System.out.print("\nSkip(5)\t\t :");
li.stream().skip(5) 
		.forEach(i -> System.out.print(" " + i));

```
## Terminator
```Java
final List<Integer> li = List.of(10,20,30,5,6,7,10,20,100);
System.out.print("ForEach:\t ");
li.stream().forEach(i->System.out.print(" "+i));

System.out.print("\nForEachOrdered: ");
li.stream().forEachOrdered(i->System.out.print(" "+i));

final Integer[] array = li.stream().toArray(i->new Integer[i]);
System.out.println("\nToArray:\t "+Arrays.toString(array));

//Integer sum = li.stream().reduce(0,(x,y)->x+y);
final Integer sum = li.stream().reduce(0,Integer::sum);
System.out.println("Sum:\t\t "+sum);

//Optional<Integer> max = li.stream().max((x,y)->x-y);
final Optional<Integer> max = li.stream().max(Integer::compare);
System.out.println("Max:\t\t "+max);
		
final long count = li.stream().count();
System.out.println("Count:\t\t "+count);

final boolean anyMatch = li.stream().anyMatch(x -> x==100);
System.out.println("AnyMatch:\t "+anyMatch);

final Optional<Integer> findAny = li.stream().findAny();
System.out.println("FindAny:\t "+findAny);
```
## stream.collect
```Java
final List<Integer> li = List.of(10,20,30,5,6,7,10,20,100);

final Set<Integer> set = li.stream().collect(
		()->new HashSet<>(), 		// collector
		(h,i)->h.add(i),			// add elements
		(h,h2)->h.addAll(h2));		// union of two collectors
System.out.println("Set: "+set);    // HashSet of list elements

// general-purpose collector
final Set<Integer> set2 = li.stream().collect(Collector.of(
	HashSet::new, 							// collector
	HashSet::add,							// add element
	(h,h2)->{h.addAll(h2); return h;}));	// union
System.out.println("Set: "+set2);

final int res=li.stream().collect(Collector.of(
	()->Arrays.<Integer>asList(0), 				// create accumulators
	(l,i)->l.set(0,i+l.get(0)),					// add elements inside them
	(l,l2)->{l.set(0,l.get(0)+l2.get(0)); return l;}))	//combine partial lists
	.get(0);									// take the first element
System.out.println("Res: "+res);
```
In the last example we have:
- An accumulator in form of a list created by the supplier
- Then it will take all the elements of `li` by adding them all in the first position of the accumulator, iterating with lambda logic. For example:
	- `l.set(0, 10 + l.get(0)) -> l.set(0, 10)` 
	- `l.set(0, 20 + l.get(0)) -> l.set(0, 30)`
- A combiner that combines partial lists, in fact the operation of adding elements is done in parallel

### Collectors
Inside this package exists also a support library called `Collectors`. It has useful methods that interacts with `Collector` class, like `groupingBy`, that groups elements creating a new collector.

Here there are some useful methods, and all of them are declared `static`:
```Java
class Collectors {

    public static <T, C> Collector<T, ?, C> toCollection(Supplier<C> collectionFactory) {
        // Implementation
    }

    public static <T> Collector<T, ?, List<T>> toList() {
        // Implementation
    }

    public static <T> Collector<T, ?, Set<T>> toSet() {
        // Implementation
    }

    public static Collector<CharSequence, ?, String> joining(CharSequence delimiter, 
                                                              CharSequence prefix, 
                                                              CharSequence suffix) {
        // Implementation
    }

    public static <T> Collector<T, ?, Optional<T>> minBy(Comparator<? super T> comparator) {
        // Implementation
    }

    public static <T> Collector<T, ?, Optional<T>> maxBy(Comparator<? super T> comparator) {
        // Implementation
    }

    public static <T> Collector<T, ?, Integer> summingInt(ToIntFunction<? super T> mapper) {
        // Implementation
    }

    public static <T> Collector<T, ?, Long> summingLong(ToLongFunction<? super T> mapper) {
        // Implementation
    }

    public static <T, K> Collector<T, ?, Map<K, List<T>>> groupingBy(Function<? super T, ? extends K> classifier) {
        // Implementation
    }

    public static <T, K, D, A> Collector<T, ?, Map<K, D>> groupingBy(Function<? super T, ? extends K> classifier, 
                                                                     Collector<? super T, A, D> downstream) {
        // Implementation
    }

    public static <T, K, U> Collector<T, ?, Map<K, U>> toMap(Function<? super T, ? extends K> keyMapper, 
                                                             Function<? super T, ? extends U> valueMapper) {
        // Implementation
    }

    public static <T, K, U> Collector<T, ?, Map<K, U>> toMap(Function<? super T, ? extends K> keyMapper, 
                                                             Function<? super T, ? extends U> valueMapper, 
                                                             BinaryOperator<U> mergeFunction) {
        // Implementation
    }

    public static <T> Collector<T, ?, DoubleSummaryStatistics> summarizingDouble(ToDoubleFunction<? super T> mapper) {
        // Implementation
    }
}
```

And also few examples of them:
```Java
	final List<Integer> li = List.of(10,20,30,5,6,7,10,20,100);
	// List
	System.out.println(li.stream().collect(toList()));
	// Set
	System.out.println(li.stream().collect(toSet())); 
	// TreeSet
	System.out.println(li.stream().collect(toCollection(TreeSet::new)));
	System.out.println(li.stream().collect(minBy(Integer::compare)));
	System.out.println(li.stream().collect(summingInt(Number::intValue)).toString());
	System.out.println(li.stream().map(i->i.toString())
								  .collect(joining(",","(",")")));
	// (10,20,30,5,6,7,10,20,100)
	final Map<Integer, List<Integer>> map = li.stream()
		.collect(groupingBy(x -> x / 10));
	System.out.println(map); // {0=[5,6,7], 1=[10,10], ..}

	final Map<Boolean, Optional<Integer>> map2 = li.stream()
		.collect(groupingBy(x -> x % 2 == 0, minBy(Integer::compare)));
	System.out.println(map2); // Minimum of odds and evens
	//Output: {false=Optional[5], true=Optional[6]}

	final Map<Integer, Integer> map3 = li.stream()
		.distinct()
		.collect(toMap(x -> x, x -> x + 1));
	System.out.println(map3); // {20=21, 100=101, 5=6, 6=7, 7=8, 10=11, 30=31}

	final Map<Integer, Integer> map4 = li.stream()
		.collect(toMap(x -> x / 10, x -> x, (x, y) -> x + y));
	System.out.println(map4); // {0=18, 1=20, 2=40, 3=30, 10=100}

	System.out.println(li.stream()
		.collect(summarizingInt(Number::intValue)).toString());
	// IntSummaryStatistics{count=9, sum=208, min=5, average=23.111111, max=100}
```
## Functional algorithms
```Java
System.out.println(LongStream.iterate(2, x -> x + 1)
	.filter((i) -> LongStream.range(2, i / 2 + 1).noneMatch(j -> i % j == 0))
	.limit(1000)
	.mapToObj(String::valueOf)
	.collect(Collectors.joining(",", "[", "]")));

final Random r = new Random();
System.out.println(IntStream.range(0, 10000)
	.map(i -> r.nextInt(6) + r.nextInt(6) + 2)
	.boxed() // from int to Integer
	.collect(groupingBy(x -> x,
			collectingAndThen(counting(),
			d -> d / 10000.0))));
System.out.println(
	"Result: "
		.chars()
		.mapToObj(x -> String.valueOf((char) x))
		.collect(groupingBy(x -> x, counting()))
		.entrySet()
		.stream()
		.sorted((e1, e2) -> -Long.compare(e1.getValue(), e2.getValue()))
		.limit(3)
		.map(String::valueOf)
		.collect(Collectors.joining(",", "[", "]")));
```
The last example maps characters inside the string to long numbers and them print them.

# Parallel stream

Java provides some hidden mechanics that improve *parallelism* with some operations that regards streams. Some methods can be done with a multithreaded implementations and some of them increase performance, but there are others that decrease it.

We can turn on parallelism with the method `parallel()` with a stream object or `parallelStream()` with a `Collection` object.

```Java
private final static int DIM = 1_000_000;
private final static int STEPS = 1_000;
long time;
long time2;
final List<Double> l = IntStream.range(0, DIM)
		.mapToObj(x -> Math.random())
		.collect(toList());

time = System.currentTimeMillis();
IntStream.range(0, STEPS).forEach(
		i -> l.stream()
		.collect(Collectors.averagingDouble(x -> x)));
time = System.currentTimeMillis() - time;
System.out.println("Time: " + time);

time2 = System.currentTimeMillis();
IntStream.range(0, STEPS).forEach(
		i -> l.stream()
		.parallel()
		.collect(Collectors.averagingDouble(x -> x)));
time2 = System.currentTimeMillis() - time2;
System.out.println("Time2: " + time2);
System.out.println("Gain: " + (((double) time) / time2));
```

With a 6-cores processor, there is a multiplier of performance that depends on the number of the elements, for example:
- With 10_000 elements, we have a multiplier of 0.9 (More inefficient than the sequential execution)
- With 1_000_000 elements, we have a gain of 6.5 in terms of performance!

As a practice, all involved lambdas with parallel features have to be as simple as possible.

## How a stream works inside the JVM

With API's source code, we can deduce how a stream works in Java. 

First of all, the JVM creates a **Stream Source**. The stream pipeline begins with a source (e.g., a `List`, an `Array`, or an I/O channel) wrapped in a `Spliterator`, which provides both sequential and parallel traversal capabilities. The `Spliterator` splits the data when parallel streams are used, allowing chunks to be processed concurrently.

Each intermediate operation adds a new layer to the pipeline by decorating the `AbstractPipeline` object. These layers encapsulate the logic for transformations such as filtering, mapping, or sorting.

The last stream created calls previous streams to pull elements from the source, applying the intermediate operations step by step in a **lazy manner**. This means that no elements are processed until a terminal operation is invoked.

For parallel streams, the `Spliterator` divides the data into smaller partitions that can be processed by multiple threads. The results of these threads are then merged by the terminal operation. 

```Java
public class CustomSpliterator implements Spliterator<String> {
    private final String[] data;
    private int current;

    public CustomSpliterator(String[] data) {
        this.data = data;
        this.current = 0;
    }

    @Override
    public boolean tryAdvance(Consumer<? super String> action) {
        if (current < data.length) {
            action.accept(data[current]);
            current++;
            return true;
        }
        return false;
    }

    @Override
    public Spliterator<String> trySplit() {
        int currentSize = data.length - current;
        if (currentSize < 2) {
            return null; // Not enough elements to split
        }
        int splitPos = current + currentSize / 2;
        Spliterator<String> newSpliterator = new CustomSpliterator(
            java.util.Arrays.copyOfRange(data, current, splitPos)
        );
        current = splitPos; // Update current position
        return newSpliterator;
    }

    @Override
    public long estimateSize() {
        return data.length - current;
    }

    @Override
    public int characteristics() {
        return ORDERED | SIZED | SUBSIZED;
    }

    public static void main(String[] args) {
        String[] names = {"Alice", "Bob", "Charlie", "David", "Eve"};
        Spliterator<String> spliterator = new CustomSpliterator(names);

        spliterator.tryAdvance(System.out::println); // Prints: Alice

        Spliterator<String> secondHalf = spliterator.trySplit();
        secondHalf.forEachRemaining(System.out::println); // Prints: Charlie, David, Eve
        spliterator.forEachRemaining(System.out::println); // Prints: Bob
    }
}
```

### A more detailed explanation about mapping

|map() method|flatMap() method|
|---|---|
|It only performs the mapping.|It performs both mapping and flattening.|
|It transforms Stream<T> to Stream<R> or Optional<T> to Optional<R>.|It performs the mapping functionality and converts Stream<Stream<T>> to Stream<T> or Optional<Optional<T>> to Optional<T>.|
### `takeWhile` for taking subsets of objects that match given predicate

The function `takeWhile` takes all the objects that match a predicate until something doesn't match the predicate. Furthermore, if the stream is ordered, it will take all objects that matches the predicate of a particular stream. For example:
```Java
import java.util.List;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        List<Integer> numbers = List.of(1, 2, 3, 10, 4, 5);

        List<Integer> result = numbers.stream()
                                      .takeWhile(n -> n < 10)
                                      .collect(Collectors.toList());

        System.out.println(result); // Output: [1, 2, 3]
    }
}
```