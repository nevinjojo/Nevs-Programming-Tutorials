# Clean Code Talks: "Ask for Things, Don't Look for Things"

## Dependency Injection

Dependency injection happens when a class/function is constructed, the dependencies of the class/function is directly created as part of the constructor. This way we are injecting a dependency that might or not might not exist/have been altered, which is bad. 

    function Engine() {
      this.action = () => {
        console.log("The engine goes vroom vroom.");
      };
      console.log("Made an engine.");
    }
    
    function Car() {
      **this.engine = new Engine(); // <- Bad!!!**
      this.action = () => {
        this.engine.action();
        console.log("The car drives by.");
      };
      console.log("Made a car.");
    }

### Problem 1.0

1. The components directly depend on each other. So you will have to make the components in the right order for it to work.
2. You cannot develop components in parallel.
3. components are creating their own dependencies instead of fetching it from somehwere else. This gives less options for testing (you can't create `MockEngine` and pass it on to the constructor parameter to test the `Car` class).

### Solution 1.0: Dependency Injection & Inversion of Control

Pass Dependencies to the class being created as a constructor parameter. This will resolve problem #3:

    function Engine() {
      this.action = () => {
        console.log("The engine goes vroom vroom.");
      };
      console.log("Made an engine.");
    
    
    function Car(**engine**) **/*<- Dependency Injection */**
    {
      **this.engine = engine;**
      this.action = () => {
        this.engine.action();
        console.log("The car drives by.");
      }
      console.log("Made a car.");
    }

## Service Locator/Handler/Factory

### **Problem 2.0: Cost of construction**

- Test has to successfully create a constructor each time an instance is needed.
- **Objects require more complex constructors, which makes testing more difficult** (as you need to create the parameter object before constructing the desired class)
- I**n the example above, If I want to create a `Pison` class that is used by `Engine` class, we will have to change the every tests where Engine class does not have Piston class.**

### Solution 2.0: Service Locator/Handler/Factory

The service locator abstracts the API lookup services, and provide a single source to fetch any class that is required during the construction of another class.

    function Engine() {
      this.action = () => {
        console.log("The engine goes vroom vroom.");
      };
      console.log("Made an engine.");
    }
    
    class ServiceLocator = {
    	function **getEngine() {
    		return engine();
    	}**
    }
    
    function Car(**locator**) {
      **this.engine = locator.getEngine();**
      this.action = () => {
        this.engine.action();
        console.log("The car drives by.");
      }
      console.log("Made a car.");
    }
    
    let locator = new ServiceLocator();
    car(locator);

## Law of Demeter: Don't talk to strangers!

1. Each class or method should only have limited knowledge about other classes/functions.

    `House` class only needs to know about `Door` class, IF it is being used to construct the House class.

2. Each unit should only talk to your immedeate friend class

    Do not fetch do something like `book.pages().last().text()`. In order to get the text, you will need to know about pages, last page, and the text from the page, which is a bad implementation.  Instead, we’re supposed to go with `book.textOfLastPage()`.

    - `serviceLocator.getService()`

## Object Lifetime

### Constructor injection

- [If the object is going to be made in the constructor](s): only inject object whose lifetime is equal to or greater than the "injectee"

### Pass in objects as method parameters at time of execution

- [If objects lifetime is shorter than the injectee](s): pass the object as a parameter instead of constructing it along with the constructor.

## Do not Null Check if not necessary

To test if the `paintHouseRed()` function in the `House` class, we might need to pass in a `Door` object, even though we are not using it. This means that we can pass in `null` as the value for `Door` while testing the function.

### Problem & Solution

This will not be possible if the Door hasa `NotNull` check on it. **Even though it is safe to have not null checks in the program, it makes it difficult to test the simple functions**. It also makes the tests complicated by adding unnecessary factors into the test. Therefore avoid using `NotNull` checks if not necessary.

### Test v/s Production

If you are calling an external API, then doing not null checks is fine. Similarly, notNull checks are suitable in production code, but not in test environment.

## [Always Ask for Things](zx)

- Abandon the new operator in your constructor. **Always ask for /fetch required parameters in the constructor, when possible**.
- There is no need to know about objects you don't directly use. **Use Factories/Services to fetch [only](s) the things you need.**