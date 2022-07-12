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

### 2. Open/closed principle

> Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification.

The idea of ​​this principle is that when implementing new features, instead of modifying existing code, we should extend/inherit.

**Example**: Previously, the logic to handle the shipping of an order was placed inside the Order class.

Giả sử hệ thống cần bổ sung thêm một phương thức vận chuyển mới, chúng ta lại phải bổ sung một case nữa trong method calculateShipping. Điều này sẽ làm code trở nên rất khó quản lý.

```typescript
public class Order {

    public long calculateShipping(shippingMethod : string) {
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

Thay vào đó, chúng ta nên tách rời logic xử lý tính phí vận chuyển vào một interface Shipping chẳng hạn. Interface Shipping sẽ có nhiều implementation ứng với từng hình thức vận chuyển: GroundShipping, AirShipping,...

```typescript
interface Shipping {
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

### 4. Interface segregation principle
