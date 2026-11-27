- #recall : Reactor pattern, Observer pattern, CPS
- synchronous and asynchronous streams
- Reactive variable: variables that change in time and they represents stream of results, we can define operations on it
- Declarative paradigm
- Differently from normal variables, we are creating *observable variables*:
```
normal variables:
x = 1
y = x + 1
x = 2
-> y = 2

reactive variables:
x = 1
y = x + 1
x = 2
-> y = 3
```