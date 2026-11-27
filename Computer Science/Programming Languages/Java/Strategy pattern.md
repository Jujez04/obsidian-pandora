# Intent
Define a family of algorithms, encapsulate each one, and make them interchangeable.
This pattern allows the algorithm to vary independently from clients that use it. It is often used to implement different behaviors or algorithms without modifying the client code.

# Motivation

There are family of algorithms that can be hard-coded in a single class, but it isn't desirable for several reason: 
- We want to maintain a simple code, without the client code gets more complex after including these algorithms.
- Different algorithms will be appropriated at different times, then we don't want algorithms unused.
- It's hard to modify them when previous versions are integral part of the client code

We can avoid these problems by defining classes that encapsulate different algorithms. An algorithm that's encapsulated in this way is called a *strategy*.


# Application

- When many related classes differ only in their behaviour.
- There is a necessity of many variants of the algorithm.
- The client shouldn't know about know about what kind of behaviour that belongs to an algorithm is going to be used
- There are multiple conditional statements in its operations. Instead of many conditionals, move related conditional branches into their own Strategy class.
# Structure
![[Pasted image 20241223073153.png]]

# Participants
- *Strategy*
- *Concrete* *Strategy*
- *Context*


Whenever a Context reformats its input, it forwards this responsibility to its Strategy object. The client of Context specifies which Strategy should be used by installing the Concrete Strategy it desires into the Context.
# Consequences
- Families of related algorithms
- An alternative to subclassing.
- Strategies eliminate conditional statements.
- A choice of implementations.
- Clients must be aware of different Strategies.
- Communication overhead between Strategy and Context.
- Increased number of objects

# Implementation

- Defining t he Strategy and Context interfaces.
- Strategies as template parameters
- Making Strategy objects optional.

# Examples

## Sorting

```Java
// Context Class
public class SortContext {
    private SortingStrategy strategy;

    // Constructor to set the initial strategy
    public SortContext(SortingStrategy strategy) {
        this.strategy = strategy;
    }

    // Method to change the strategy dynamically
    public void setStrategy(SortingStrategy strategy) {
        this.strategy = strategy;
    }

    // Execute the sorting strategy
    public void sort(int[] numbers) {
        strategy.sort(numbers);
    }
}

// Strategy Interface
public interface SortingStrategy {
    void sort(int[] numbers);
}

// Concrete Strategy: Bubble Sort
public class BubbleSort implements SortingStrategy {
    @Override
    public void sort(int[] numbers) {
        int n = numbers.length;
        for (int i = 0; i < n - 1; i++) {
            for (int j = 0; j < n - i - 1; j++) {
                if (numbers[j] > numbers[j + 1]) {
                    // Swap numbers[j] and numbers[j + 1]
                    int temp = numbers[j];
                    numbers[j] = numbers[j + 1];
                    numbers[j + 1] = temp;
                }
            }
        }
        System.out.println("Sorted using Bubble Sort");
    }
}

// Concrete Strategy: Quick Sort
public class QuickSort implements SortingStrategy {
    @Override
    public void sort(int[] numbers) {
        quickSort(numbers, 0, numbers.length - 1);
        System.out.println("Sorted using Quick Sort");
    }

    private void quickSort(int[] arr, int low, int high) {
        if (low < high) {
            int pi = partition(arr, low, high);
            quickSort(arr, low, pi - 1);
            quickSort(arr, pi + 1, high);
        }
    }

    private int partition(int[] arr, int low, int high) {
        int pivot = arr[high];
        int i = (low - 1);
        for (int j = low; j < high; j++) {
            if (arr[j] <= pivot) {
                i++;
                int temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }
        int temp = arr[i + 1];
        arr[i + 1] = arr[high];
        arr[high] = temp;
        return i + 1;
    }
}

```