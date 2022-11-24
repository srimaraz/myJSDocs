# myJSDocs

# Interesting observations 

Known : var has function scope.let has block scope.

So, in a for loop like this:

```javascript
for (let i = 0; i < row.length; i++) 
```

Each iteration of the loop has it's own separate i variable. When using asynchronous operations inside the loop, that keeps each separate i variable with it's own asynchronous callback and thus one doesn't overwrite the other.

Since var is function scoped, when you do this:

```javascript
for (var i = 0; i < row.length; i++) 
```

There's only one i variable in the whole function and each iteration of the for loop uses the same one. The second iteration of the for loop has changed the value of i that any asynchronous operations in the first iteration may be trying to use. This can cause problems for those asynchronous operations if they are trying to use their value of i inside an asynchronous callback because meanwhile the for loop will have already changed it.

```javascript
for (var i = 0; i < 5; i++) {
    setTimeout(function() {
        console.log(i);
    }, 10);
}
```
### prints: 5 5 5 5 5
### But :
```javascript
for (var i = 0; i < 5; i++) {
    setTimeout(function() {
        console.log(i);
    }, 10);
}
```
### prints: 1 2 3 4 5
