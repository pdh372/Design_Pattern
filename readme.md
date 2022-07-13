# I. What is SOLID?

> Author&nbsp;&nbsp;&nbsp;: **Pham Duc Huy**
> \
> Contact  : **<pdh372@gmail.com>**

---

## A. Introduce?

SOLID is the five fundamental principles of object-oriented software design that make code easier to understand, flexible and maintainable.

Five fundamental principles includes:

- **S**ingle responsibility principle
- **O**pen/closed principle
- **L**iskov substitution principle
- **I**nterface segregation principle
- **D**ependency inversion principle

---

### 1. Single responsibility principle

>A class should have only a single responsibility

This idea help developer decrease class's complex, it would be better if a class just responsibility one purpose.

***Example***

> **UpdateUser** class execute many request.

```typescript
class DBHelper {

    public openConnection() : Connection {};

    public saveUser(user : User) : void {};

    public getProducts() : Product[] {};

    public closeConnection() : void {};

}
```

> We should segregation into classes to be responsible for each type

```typescript
public class DBConnection {

    public openConnection() : void {};

    public closeConnection() : void {};

}

public class UserRepository {

    public saveUser(user : User) : void {};

}

public class ProductRepository {

    public saveUser(User user) : void {};

}
```

---

### 2. Open/closed principle

> Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification.

The idea of ​​this principle is that when implementing new features, instead of modifying existing code, we should extend/inherit.

**Example**: Previously, the logic to handle the shipping of an order was placed inside the Order class.

Giả sử hệ thống cần bổ sung thêm một phương thức vận chuyển mới, chúng ta lại phải bổ sung một case nữa trong method calculateShipping. Điều này sẽ làm code trở nên rất khó quản lý.

```typescript
public class Order {

    public calculateShipping(shippingMethod : string) : number {
        if (shippingMethod === 'GROUND') {
            // Calculate for ground shipping
        } else if (shippingMethod === 'AIR') {
            // Calculate for air shipping
        } else {
            // Default
        }
    }
}
```

Thay vào đó, chúng ta nên tách rời logic xử lý tính phí vận chuyển vào một Shipping interface chẳng hạn. Shipping interface sẽ có nhiều implementation ứng với từng hình thức vận chuyển: GroundShipping, AirShipping,...

```typescript
Shipping interface {
    calculate() : number;
}

class GroundShipping implements Shipping {
    public calculate() : number {
        // Calculate for ground shipping
    }
}

class AirShipping implements Shipping {
    public calculate() : number {
        // Calculate for air shipping
    }
}

public class Order {
    private shipping Shipping;
    public calculateShipping(shippingMethod : Shipping) : number {
        return shippingMethod.calculate();
    }
}
```

---

### 3. Liskov substitution principle

> Objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program.

This principle can be understood that the objects of the parent class can be replaced by the objects of the child classes without changing the correctness of the program.

**Example**: We have the **Animal** interface and two implementations of **Bird** and **Dog** as follows

Apparently, the **Dog** class violated the **Liskov substitution principle**.

```typescript
interface Animal {
    fly() : void;
}

class Bird implements Animal {

    public fly() : void {
        // Flying...
    }
}

class Dog implements Animal {

    public fly() : void {
        // Dog can't fly
        throw new Error("Dog can't fly");
    }
}
```

The workaround here would be: create a **FlyableAnimal** interface as follows

```typescript
public interface Animal {
}

public interface FlyableAnimal {

    void fly();
}

public class Bird implements FlyableAnimal {

    public void fly() {
        // Flying...
    }
}

public class Dog implements Animal {
}
```

---

### 4. Interface segregation principle

> Many client-specific interfaces are better than one general-purpose interface.

This principle can be understood that instead of writing an interface for a general purpose, we should **separate into many small interfaces for specific purposes**. We should not force the client to implement methods that the client does not need.

**Example**:

We have a **Animal** interface like this:

```typescript
interface Animal {

    eat() : void;

    void run();

    void fly();

}
```

we have 2 class **Dog** and **Snake** implement interface **Animal**. But it make no sense. That is how to **Dog()** **Fly()** and **Snake** **Run()**

Instead of, we should separate into 3 small interface like this

```typescript
interface Animal {

    eat() : void;
}

interface RunnableAnimal extends Animal {

    run() : void;
}

interface FlyableAnimal extends Animal {

    fly() : void;
}
```

---

### 5. Dependency inversion principle

> Depend on abstractions, not on concretions.

The idea of the principle is that hight-level modules should not depend on low-level modules, both should depend on abstraction.

**Example**: we have two modules low-level **BackendDev** and **FrontendDev**. And a module hight-level **Project**

````typescript
class BackendDev {

    private void codeNodeJS() {};
}

class FrontendDev {

    private void codeJS() {};
}

public class Project {

    _backendDev = new BackendDev();
    _frontendDev = new FrontendDev();

    public build() : void {
        _backendDev.codeNodeJS();
        _frontendDev.codeJS();
    }
}
````

Suppose if later, the project changes technology. Backend developers don't code **NodeJS** anymore but switch to **C#** code. Frontend developers don't code **pure JS** anymore but upgrade to **JS frameworks**. Obviously **we have to fix not only the code in the low-level modules (BackendDev and FrontendDev) but also the code in the high-level modules (Project) that are using those low-level modules**. This shows that **high-level modules are having to depend on low-level modules**.

At this point, we will add a **Developer abstraction** to which the above modules depend:

```typescript
interface Developer {

    develop() : void;
}

class BackendDeveloper implements Developer {

    public develop() : void {
        codeNodeJS();
        // codeCSharp();
    }

    private codeNodeJS() : void {};

    private codeCSharp() : void {};
}

class FrontendDeveloper implements Developer {

    public develop() : void {
        codeJS();
        // codeAngular();
    }

    private codeJS() : void {};

    private codeAngular() : void {};
}

public class Project {

    private _developers : Developer[];

    constructor(developers : Developers[]) {
        this._developers = developers;
    }

    public build() : void {
        developers.forEach(developer => developer.develop());
    }
}
```

---

<!-- #### References

> [Quick study of divine SOLID](https://kipalog.com/posts/Tim-hieu-nhanh-SOLID-than-thanh) -->
