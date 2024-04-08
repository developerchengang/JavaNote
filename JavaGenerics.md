# 泛型

泛型是一种在实现参数化类型的功能。参数化类型允许编写灵活且可重复使用的代码，同一段代码能够操作多种类型，同时保持严格的类型安全。

在没有泛型的情况下，编程语言可能只能使用特定类型或非常宽泛的类型（例如 Object 类型）。这样做就意味着牺牲了类型安全，或者需要编写大量的代码来处理每一种可能的数据类型。

泛型允许在定义类、接口和方法时使用类型参数。这样的集合、数据结构（字段）和算法（方法）可以编写成能够以同样的方式工作在不同的数据类型上，无需为每种数据类型都编写代码。这些类型参数在实例化类、调用方法或实现接口时被具体的类型所替换。

## 泛型的作用

- **类型安全** 泛型提供了增加的类型检查，允许在编译时期捕获到非法的类型。使用泛型可以确保只有符合特定类型参数的对象被插入到集合中，减少运行时出现`ClassCastException`的可能性。

- **消除类型强制转换** 在使用泛型容器存储对象时，不需要强制类型转换就能够取出元素。例如，`List<String>` 会保证其元素都是 `String` 类型，因此取元素时不再需要将 `Object` 类型转换为 `String` 类型。

- **代码重用** 泛型使得相同的代码能够用于多种数据类型。泛型类、接口和方法成为可以工作在多种数据类型下的模板，从而避免了为每个数据类型编写重复的代码。

## 泛型类

泛型类用于实现类的参数化，这样的类被称为参数化类（parameterized classes）或者参数化类型（parameterized types）。。泛型类使得类在被创建时能够指定具体的类型参数，这增强了类的灵活性和复用性，同时保持了代码的类型安全性。

泛型类的定义类似于非泛型类，但是在类名后面添加了类型参数部分。类型参数使用尖括号<> 标记，并定义为一或多个类型变量，如`<T>`、`<T, U>`等。类型变量可以在类定义中的几乎任何地方使用，比如作为字段类型、方法参数类型、方法返回类型或局部变量类型。

当创建这样一个类的实例时，你需要指定相应的具体类型来代替这些泛型类型参数。这样，同一个类的不同实例可以用不同的类型来操作，而不需要为每种类型都写一个特定的类。

### 语法

```java
public class Box<T> {
    private T t;
}
```

- **Box** 一个泛型类。

- **T** 传递给泛型类的类型参数。它可以接受任何对象。

- **t** 泛型类型 T 的实例。

### 说明

T 是传递给泛型类 Box 的类型参数，应在创建 Box 对象时传递。

### 示例

```java
public class GenericsTester {

    public static void main(String[] args) {
        Box<Integer> integerBox = new Box<Integer>();
        Box<String> stringBox = new Box<String>();

        integerBox.add(10);
        stringBox.add(new String("Hello World"));

        System.out.printf("Integer Value: %d\n", integerBox.get());
        System.out.printf("String Value: %s\n", stringBox.get());
    }
}

class Box<T> {

    private T t;

    public void add(T t) {
        this.t = t;
    }

    public T get() {
        return t;
    }
}
```

### 输出

```log
Integer Value: 10
String Value: Hello World
```

## 命名规范

在使用泛型定义类、接口或方法时，类型参数常常用单个大写字母来表示，这个惯例虽然不是强制性的，但遵循它可以让代码更清晰，也更符合Java编程社区的习惯。以下是一些常见的泛型类型参数命名规范：

- **E** - Element（元素），广泛用于集合中的元素，如 `java.util.List<E>`、`java.util.Set<E>`等。

- **K** - Key（键），用于映射的键，在 `java.util.Map<K, V>` 中使用。

- **V** - Value（值），用于映射的值，在 `java.util.Map<K, V>` 中使用。

- **T** - Type（类型），表示一个通用的类型，当没有更具体的含义时使用，例如 `Box<T>`。

- **S, U, V** 等 - 第二、第三、第四个声明的类型参数，通常当有多个类型参数时使用。

- **N** - Number（数字），用于数值类型的泛型。

一般来说，类型参数最好能够反映出某种含义或约定，使得代码可以更容易理解。虽然可以使用任意的类型参数名称，但最好避免使用容易与实际的类名或接口名相混淆的名称，这样可以减少误解。遵循上述规范有助于代码的阅读和维护。

在实际编程中，尤其是编写泛型时，通常会有文档或注释来说明这些泛型参数的具体意义，以便使用者能够更加直观地理解它们的用途。

### 示例

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class GenericsTester {

    public static void main(String[] args) {
        Box<Integer, String> box = new Box<Integer, String>();
        box.add(10, "Hello World");
        System.out.printf("Integer Value :%d\n", box.getFirst());
        System.out.printf("String Value :%s\n", box.getSecond());

        Pair<String, Integer> pair = new Pair<String, Integer>();
        pair.add("One", 1);
        System.out.println("Pair Integer Value: " + pair.getValue("One"));

        CustomList<Box<Integer, String>> customList = new CustomList<>();
        customList.addItem(box);
        System.out.printf("CustomList Value :%d\n", customList.getItem(0).getFirst());
    }
}

class Box<T, S> {

    private T t;
    private S s;

    public void add(T t, S s) {
        this.t = t;
        this.s = s;
    }

    public T getFirst() {
        return t;
    }

    public S getSecond() {
        return s;
    }
}

class Pair<K, V> {

    private Map<K, V> map = new HashMap<K, V>();

    public void add(K key, V value) {
        map.put(key, value);
    }

    public V getValue(K key) {
        return map.get(key);
    }
}

class CustomList<E> {

    private List<E> list = new ArrayList<E>();

    public void addItem(E value) {
        list.add(value);
    }

    public E getItem(int index) {
        return list.get(index);
    }
}
```

### 输出

```log
Integer Value: 10
String Value: Hello World
Pair Integer Value: 1
CustomList Value: 10
```

## 类型推断

类型推断代表Java编译器查看方法调用及其相应声明的能力，用于检查并确定类型参数。类型推断算法会检查参数的类型，并且如果可能，返回分配的类型。推断算法尝试找到一个特定的类型，这个类型可以满足所有的类型参数。

如果不使用类型推断，编译器会生成未经检查的转换警告。

### 语法

```java
Box<Integer> integerBox = new Box<>();
```

- Box  一个泛型类。

- <> 菱形运算符表示类型推断。

使用菱形运算符，编译器确定参数的类型。该运算符从 Java SE 7 版本开始可用。

### 示例

```java
public class GenericsTester {

    public static void main(String[] args) {
        // type inference 
        Box<Integer> integerBox = new Box<>();
        integerBox.add(10);

        // unchecked conversion warning
        Box<String> stringBox = new Box<String>();
        stringBox.add("Hello World");

        System.out.printf("Integer Value: %d\n", integerBox.get());
        System.out.printf("String Value: %s\n", stringBox.get());
    }
}

class Box<T> {
        private T t;

        public void add(T t) {
            this.t = t;
        }

        public T get() {
            return t;
        }

}
```

### 输出

```log
Integer Value: 10
String Value: Hello World
```

## 泛型方法

泛型方法是指在方法中引入类型参数，从而使得该方法能够灵活地接受多种数据类型的参数。不同于泛型类，泛型方法允许在方法级别声明类型参数，这些类型参数在调用方法时确定具体的类型。泛型方法可以独立于类定义，即它们可以在非泛型类中定义，也可以在泛型类中定义。

### 示例

```java
public class GenericMethodTest {
    public static void main(String[] args) {
        Integer[] intArray = {1, 2, 3, 4, 5};
        Double[] doubleArray = {1.1, 2.2, 3.3, 4.4};
        Character[] charArray = {'H', 'E', 'L', 'L', 'O'};

        System.out.println("Array integerArray contains:");
        printArray(intArray);
        System.out.println("Array doubleArray contains:");
        printArray(doubleArray);
        System.out.println("Array charArray contains:");
        printArray(charArray);
    }

    public static <T> void printArray(T[] array) {
        for (T element : array) {
            System.out.printf("%s ", element);
        }
        System.out.println();
    }
}
```

### 输出

```log
Array integerArray contains:
1 2 3 4 5
Array doubleArray contains:
1.1 2.2 3.3 4.4
Array charArray contains:
H E L L O
```

## 原始类型

原始类型（Raw Type）是指没有指定泛型类型参数的泛型类或接口。简言之，原始类型就是在类名之后没有尖括号和类型参数的类型。它们在泛型引入到 Java 语言（Java 5）之前就存在，为了兼容性而保留下来。

原始类型忽略了泛型的类型参数，并视泛型类或接口为非参数化的状态，其行为类似于所有的泛型类型信息都被擦除后的类型。这就意味着使用原始类型会失去泛型带来的类型安全性，因为原始类型的变量可以接受任何类型的对象，而不受限于泛型声明的类型参数。

使用原始类型是不推荐的做法，因为它们会使得你失去泛型的优势，例如编译时的类型检查。原始类型通常在以下两种情景中出现：

*为了向后兼容**：早期的 Java 代码在泛型加入 Java 之前就已经存在，这些代码使用了原始类型。为了兼容这些旧代码，Java 设计泛型时保留了原始类型的使用。

**类型不明确**：在某些情况下，可能不清楚或不关心具体的泛型类型，或者因为特定的原因需要使用原始类型，尽管在大多数情况下，这可能表明设计上的问题。

### 示例

```java
public class GenericsTester {

    public static void main(String[] args) {
        Box<Integer> integerBox = new Box<>();
        integerBox.set(10);
        System.out.printf("Integer Value :%d\n", integerBox.get());

        // 警告：未经检查的转换
        Box rawBox = new Box();

        // 没有警告
        rawBox = integerBox;
        System.out.printf("Integer Value :%d\n", rawBox.get());

        // 警告：未经检查的调用set(T)
        rawBox.set(20);
        System.out.printf("Integer Value :%d\n", rawBox.get());

        // 需要强制类型转换
        Integer value = (Integer) rawBox.get();

        // 警告：未经检查的转换
        integerBox = rawBox;
        System.out.printf("Integer Value :%d\n", integerBox.get());
    }
}

class Box<T> {
        private T t;

        public void set(T t) {
            this.t = t;
        }

        public T get() {
            return t;
        }
}
```

### 输出

```log
Integer Value: 10
Integer Value: 10
Integer Value: 10
Integer Value: 10
```

## 有界类型参数

有界类型参数（Bounded Type Parameters）是一种使得类型参数（Type Parameters）受到一定约束的机制。有界类型参数使得可以限制将用于特定泛型类、接口或方法的类型参数的范围。这通常是通过继承（extends）一定的类或实现（implements）某些接口来实现的。

### 示例

```java
public class MaximumTest {
    public static void main(String[] args) {
        System.out.printf("Max of %d, %d and %d is %d\n\n", 
        3, 4, 5, maximum(3, 4, 5));
        System.out.printf("Max of %.1f,%.1f and %.1f is %.1f\n\n", 
        6.6, 8.8, 7.7, maximum(6.6, 8.8, 7.7));
        System.out.printf("Max of %s, %s and %s is %s\n", 
        "pear", "apple", "orange", maximum("pear", "apple", "orange"));
    }

    public static <T extends Comparable<T>> T maximum(T x, T y, T z) {
        T max = x;
        if (y.compareTo(max) > 0) {
            max = y;
        }
        if (z.compareTo(max) > 0) {
            max = z;
        }
        return max;
    }
}
```

在此例中，`<T extends Comparable<T>>` 部分指定了 T 类型必须实现 Comparable 接口，这样在方法内部可以安全地调用 T 类型对象的 compareTo 方法。

### 输出

```log
Max of 3, 4 and 5 is 5

Max of 6.6,8.8 and 7.7 is 8.8

Max of pear, apple and orange is pear
```

## 多重限制

### 语法

```java
public static <T extends Number & Comparable<T>> T maximum(T x, T y, T z)
```

- *maximum* 一个泛型方法。

- *T* 传递给泛型方法的泛型类型参数。它可以接受任何对象。

T 是传递给泛型类 Box 的类型参数，应该是 Number 类的子类型，并且必须实现 Comparable 接口。如果类作为绑定传递，则应在接口之前，否则会发生编译时错误。

### 示例

```java
public class MultipleBounds {
    public static void main(String[] args) {
        System.out.printf("Max of %d, %d and %d is %d\n\n", 
        3, 4, 5, maximum(3, 4, 5));
        System.out.printf("Max of %.1f,%.1f and %.1f is %.1f\n\n", 
        6.6, 8.8, 7.7, maximum(6.6, 8.8, 7.7));


        // The following code will not compile because the String class does not implement the Comparable interface.
        /*System.out.printf("Max of %s, %s and %s is %s\n", 
        "pear", "apple", "orange", maximum("pear", "apple", "orange"));*/
    }

    public static <T extends Number & Comparable<T>> T maximum(T x, T y, T z) {
        T max = x;
        if (y.compareTo(max) > 0) {
            max = y;
        }
        if (z.compareTo(max) > 0) {
            max = z;
        }
        return max;
    }

    // The following code will not compile 
    // because the Number class must be the first class in the list of bounds.
    /*public static <T extends Comparable<T> & Number> T maximum(T x, T y, T z) {
        T max = x;
        if (y.compareTo(max) > 0) {
            max = y;
        }
        if (z.compareTo(max) > 0) {
            max = z;
        }
        return max;
    }*/
}
```

### 输出

```log
Max of 3, 4 and 5 is 5

Max of 6.6,8.8 and 7.7 is 8.8
```

## 通配符

通配符是用来表示未知类型的一种特殊符号。通配符在泛型代码编写中十分常见，通常是用一个问号 `?` 代表。使用通配符可以提高代码的灵活性，允许编写出更加通用的代码。Java 中的通配符主要有三种形式：无界通配符（Unbounded Wildcard）、有界通配符：分为上界通配符（Upper Bounded Wildcard）和下界通配符（Lower Bounded Wildcard）。

使用通配符的主要目的是编写更加灵活的代码，特别是在你的代码需要兼容多种类型的时候。在通配符使用过程中要谨慎的一点是，带通配符的泛型对象通常不能安全地写入数据，因为具体的类型未知。

在API设计中，通常遵循"PECS"原则（Producer Extends, Consumer Super），意思是如果要从集合中读取类型（即作为数据的生产者），那么使用上界通配符`extends`；如果你要写入数据到集合中（即作为数据的消费者），那么使用下界通配符`super`。这个原则有助于记住何时使用上界和下界通配符来提高代码的灵活性和类型安全。

- **无界通配符（Unbounded Wildcard）** 用一个问号 `?` 代表。

- **上界通配符（Upper Bounded Wildcard）** 使用关键字 `extends` 来表示。

- **下界通配符（Lower Bounded Wildcards）** 使用关键字 `super` 来指定类型的下界。

## 上界通配符（Upper Bounded Wildcards）

上界通配符是 Java 泛型中的一个特性，用来限制泛型类型的上限，即泛型类型参数必须是某个指定类的子类型（包括该类型本身）。在 Java 中，上界通配符使用关键字 `extends` 来表示。

上界通配符通常与通配符（wildcard）`?` 结合使用，形成了这样的形式：`? extends Type`，表示可以接受 Type 类型或者其任何子类型的对象。上界通配符非常有用，尤其是在定义泛型方法或者泛型类型的时候，当你想要对输入或输出的类型施加上限约束。

使用上界通配符的好处是它增加了泛型的灵活性。它可以使你的泛型代码能用于更广泛的类型范围，同时仍然保持类型安全。在读取操作（如迭代）中使用上界通配符是很常见的，因为它能够确保集合中元素的类型都是某个特定类的实例或派生。然而，需要注意的是，当使用带有上界通配符的泛型类型时，通常不能向其中写入任何具体的类型，因为Java编译器无法确定实际接受的类型是什么。

### 示例

```java
public class GenericsTester {

    public static void main(String[] args) {
        List<Integer> intList = Arrays.asList(1, 2, 3);
        System.out.println("Sum of integers: " + sum(intList));

        List<Double> doubleList = Arrays.asList(1.2, 2.3, 3.5);
        System.out.println("Sum of doubles: " + sum(doubleList));
    }

    public static double sum(List<? extends Number> list) {
        double sum = 0.0;
        for (Number i : list) {
            sum += i.doubleValue();
        }
        return sum;
    }
}
```

### 输出

```log
Sum of integers: 6.0
Sum of doubles: 7.0
```

## 无界通配符

无界通配符（Unbounded Wildcard）它不对类型参数做任何限制，即 `?` 代表"任意类型"。

然而，使用无界通配符也有限制：

- 当使用无界通配符时，除了 `null` 不能向其写入任何以外的值。因为 Java 编译器无法确认容器中应该放置何种类型的对象。
- 你可以从使用无界通配符的泛型容器中读取数据，但是所有读取出来的数据将被视为 `Object` 类型，因为 `Object` 是所有 Java 类的最终父类。

### 示例

```java
public class GenericsTester {

    public static void main(String[] args) {
        List<Integer> intList = Arrays.asList(1, 2, 3);
        printList(intList);

        List<Double> doubleList = Arrays.asList(1.2, 2.3, 3.5);
        printList(doubleList);
    }

    public static void printList(List<?> list) {
        for (Object elem : list) {
            System.out.print(elem + " ");
        }
        System.out.prin
```

总结起来，无界通配符的关键特点是它提供对任何类型的泛型对象的读取访问权，但对写入操作加以限制（除非写入 `null`）。这种通配符适用于数据的不变行为，如只读操作。当你的代码不需要关心泛型对象具体类型时，无界通配符是一种简单有效的泛型类型表示方式。

## 下界通配符（Lower Bounded Wildcards）

下界通配符使用关键字 `super` 来指定类型的下界。下界通配符表明泛型类型参数必须是指定类型的父类型（包括该类型本身）。在表达式中它通常写作 `? super Type`，这表示可以接受 Type 类型或者是它的任何父类型的对象。

下界通配符最常用于需要写入或修改集合的场合，因为它们提供了关于集合中可以插入哪些类型对象的有用信息。这有助于保持类型安全，避免在集合中插入不正确的类型。

使用下界通配符的原则通常遵循：“Producer Extends, Consumer Super”（简称PECS）。它由 Joshua Bloch 提出，意思是如果你要从一个数据结构获取数据，可以用 `extends`（生产者），如果你要把数据放入一个数据结构，就可以用 `super`（消费者）。这个原则是泛型使用中的一个重要指导准则，帮助你合理地使用泛型以及保持类型安全。

在实践中，使用下界通配符可以提高API的灵活性。例如，在Java的 `Collections` 类中就存在这样的方法：

```java
public static void copy(List<? super T> dest, List<? extends T> src) {
    // ...
}
```

这个 `copy` 方法意味着你可以将源列表(src)中的元素复制到目标列表(dest)中，并且这两个列表可以有不同的类型参数，只需要满足目标列表的类型参数是来源列表的类型参数的父类即可。这样设计可以让你在不牺牲类型安全的前提下，复制对象到更加通用类型的列表中去。

### 示例

```java
public class GenericsTester {

    public static void main(String[] args) {
        List<Animal> animals = new ArrayList<>();
        List<Cat> cats = new ArrayList<>();
        List<RedCat> redCats = new ArrayList<>();
        List<Dog> dogs = new ArrayList<>();

        addCat(animals);
        addCat(cats);
        // compile time error
        // can not add list of subclass RedCat of Cat class
        // addCat(redCats);

        // compile time error
        // can not add list of subclass Dog of Superclass Animal of Cat class
        // addCat.addMethod(dogList); 
    }

    public static void addCat(List<? super Cat> catList) {
        catList.add(new RedCat());
        System.out.println("Cat added");
    }
}
```

## in（逆变）、out（协变）

为了确定哪种类型的通配符最适合该条件，我们将传递给方法的参数类型分类为输入（in）和输出（out）参数。

`in` 表明一个类型参数是逆变的，意味着这个泛型类在这个类型参数上是一个消费者。可以将其理解为 “只输入，不输出” 的类型参数。可以安全地传递类型 `T` 的值给这个对象，但不能安全地读取这个类型的值。在 Java 中下界通配符对应 `in`。

```java
public void processElements(List<? extends Number> list) {
    for (Number num : list) {
        // 可从 list 中读取 Number 或其子类型的元素
    }
    // 不可以将新的元素添加到 list 中
}
```

`out`表明一个类型参数是协变的，意味着这个泛型类在这个类型参数上是一个生产者。可以将其简单理解为 “只输出，不输入” 的类型参数。可以安全地读取出类型`T`的值，但不能写入特定的`T`对象。在 Java 中上界通配符对应 `out`。

```java
public void addNumbers(List<? super Integer> list) {
    list.add(1); // 可以将 Integer 添加到 list 中
    // 不能保证从 list 中安全地读取特定类型的元素
}
```

### 示例

```java
public class GenericsTester {

    public static void main(String[] args) {
        List<Animal> animals = new ArrayList<>();
        List<Cat> cats = new ArrayList<>();
        List<RedCat> redCats = new ArrayList<>();

        addCat(animals);
        addCat(cats);
        printCat(cats);
        printCat(redCats);
    }

    public static void addCat(List<? super Cat> catList) {
        catList.add(new RedCat()); // / 可向 catList 中添加 Cat 或其超类型的

        // 读取的元素只能存放在 Object 类型的变量中，
        // 不能存放在 Cat 类型的变量中，需要强制类型转换
        for (Object cat : catList) {
            System.out.println(cat);
        }
    }

    public static void printCat(List<? extends Cat> catList) {
        for (Cat cat : catList) {
            // / 可从 catList 中读取 Cat 或其子类型的
            System.out.println(cat);
        }

        // 编译错误，不能添加元素
        // catList.add(new Cat());
    }
}

class Animal {}

class Cat extends Animal {}

class RedCat extends Cat {}
```

## 类型擦除

Java泛型类型擦除（Type Erasure）是 Java 编译器应用的一种过程，它允许在编写泛型代码时使用类型参数，而在运行时则移除这些类型参数信息。这样做的目的是为了确保泛型代码能与 Java 早期版本的代码兼容。

泛型是在 Java 5中引入的一个特性，它允许在类、接口和方法中使用类型参数，从而提供更强的类型检查，并减少代码中的类型强转。当你使用泛型时，例如创建一个泛型类`ArrayList<T>`，你可以指定一个具体的类型来代替`T`。比如，使用`ArrayList<String>`将`T`指定为`String`。

类型擦除发生在编译时，编译器使用泛型来进行类型检查，但随后会移除所有的类型参数并替换为它们的限定类型（bounded types）或者 Object 类型。这样一来，在字节码级别，泛型类和方法就变成了普通的类和方法，并且不包含任何泛型信息。

例如：

```java
List<String> list = new ArrayList<String>();
```

在编译之后会变成：

```java
List list = new ArrayList();
```

在这个例子中，编译后的代码把`List<String>`变成了`List`，`ArrayList<String>`变成了`ArrayList`。这表示在运行时，你不能知道这个list原本被指定的是`String`类型，因为这部分信息已经被擦除了。这也是为什么你不能直接查询泛型参数的类型信息（例如：不能用`if (list instanceof ArrayList<String>)`）。

类型擦除可能引起某些问题，像是类型转换异常（ClassCastException），因为泛型只在编译期间提供类型安全性，而在运行时，这些安全性检查不复存在。为了应对这个问题，编译器会在必要时插入类型转换代码，以确保运行时的安全性。

总的来说，类型擦除是 Java 中实现泛型的一种折中办法，它保证了泛型代码的兼容性和与旧Java 代码的集成，虽然牺牲了类型信息。

### 类型擦除规则

类型擦除规则决定了泛型在编译过程中如何被转换为普通的类和方法调用。其主要规则如下：

**泛型类型参数擦除**： 如果类型参数未指定上界（如 `T`），则替换为 `Object`。 如果类型参数指定了上界（如 `T extends Comparable`），则替换为 `Comparable` 作为上界。

**泛型方法和类中的类型参数**： 泛型方法的类型参数在方法被调用时同样进行擦除，使用相同的规则（替换为 `Object` 或指定的上界）。

**桥接方法（Bridge Methods）**： 因为类型擦除可能会导致方法签名冲突或不明确，Java 编译器可能会生成一个或多个桥接方法来保证多态性。桥接方法是编译器自动创建的方法，它会调用原始方法，但是其参数和返回类型使用了擦除后的类型。

**类型参数的运行时检查**： 由于类型信息在运行时不可用，因此我们无法对泛型类型参数进行 `instanceof` 检查（例如 `object instanceof T` 是非法的）。

**泛型数组创建是不合法的**： 因为类型擦除，Java 不允许直接创建泛型类型的数组（例如 `new T[]`），这是因为在运行时无法确定具体的类型来确保类型安全。

类型擦除确保了泛型类型在运行时和普通类一样被处理，这样做的好处是使得泛型能与之前版本的Java代码兼容，但缺点是某些泛型操作在运行时变得不安全或不可能执行。

### 有界类型擦除示例

如果使用有界类型参数，Java 编译器会将泛型类型中的类型参数替换为其绑定的类型参数。

```java
// 源代码
public class GenericsTester {
   public static void main(String[] args) {
      Box<Integer> integerBox = new Box<Integer>();
      Box<Double> doubleBox = new Box<Double>();

      integerBox.add(new Integer(10));
      doubleBox.add(new Double(10.0));

      System.out.printf("Integer Value :%d\n", integerBox.get());
      System.out.printf("Double Value :%s\n", doubleBox.get());
   }
}

class Box<T extends Number> {
   private T t;

   public void add(T t) {
      this.t = t;
   }

   public T get() {
      return t;
   }   
}
```

在这种情况下，Java 编译器将用 Number 类替换 T，并且在类型擦除后，编译器将为以下代码生成字节码。

```java
public class GenericsTester {
   public static void main(String[] args) {
      Box integerBox = new Box();
      Box doubleBox = new Box();

      integerBox.add(new Integer(10));
      doubleBox.add(new Double(10.0));

      System.out.printf("Integer Value :%d\n", integerBox.get());
      System.out.printf("Double Value :%s\n", doubleBox.get());
   }
}

class Box {
   private Number t;

   public void add(Number t) {
      this.t = t;
   }

   public Number get() {
      return t;
   }   
}
```

### 无界类型擦除示例

如果使用无界类型参数，Java 编译器会将泛型类型中的类型参数替换为 Object。

```java
public class GenericsTester {
   public static void main(String[] args) {
      Box<Integer> integerBox = new Box<Integer>();
      Box<String> stringBox = new Box<String>();

      integerBox.add(new Integer(10));
      stringBox.add(new String("Hello World"));

      System.out.printf("Integer Value :%d\n", integerBox.get());
      System.out.printf("String Value :%s\n", stringBox.get());
   }
}

class Box<T> {
   private T t;

   public void add(T t) {
      this.t = t;
   }

   public T get() {
      return t;
   }   
}
```

在这种情况下，Java 编译器将用 Object 类替换 T，并且在类型擦除后，编译器将为以下代码生成字节码。

```java
public class GenericsTester {
   public static void main(String[] args) {
      Box integerBox = new Box();
      Box stringBox = new Box();

      integerBox.add(new Integer(10));
      stringBox.add(new String("Hello World"));

      System.out.printf("Integer Value :%d\n", integerBox.get());
      System.out.printf("String Value :%s\n", stringBox.get());
   }
}

class Box {
   private Object t;

   public void add(Object t) {
      this.t = t;
   }

   public Object get() {
      return t;
   }   
}
```

### 泛型方法类型擦除示例

如果使用无界类型参数，Java 编译器将泛型类型中的类型参数替换为 Object；如果使用有界类型参数作为方法参数，则 Java 编译器将替换类型参数。

```java
public class GenericsTester {
    public static void main(String[] args) {
       Box<Integer> integerBox = new Box<Integer>();
       Box<String> stringBox = new Box<String>();

       integerBox.add(10);
       stringBox.add(new String("Hello World"));

       printBoundBox(integerBox);
       printUnboundBox(stringBox);
    }

    private static <T extends Box> void printBoundBox(T box) {
       System.out.println("Integer Value :" + box.get());
    }

    private static <T> void printUnboundBox(T box) {
       System.out.println("String Value :" + ((Box)box).get());
    }
 }

 class Box<T> {
    private T t;

    public void add(T t) {
       this.t = t;
    }

    public T get() {
         return t;
    }
}
```

在这种情况下，Java 编译器将用 Object 类替换 T，并且在类型擦除后，编译器将为以下代码生成字节码。

```java
public class GenericsTester {
   public static void main(String[] args) {
      Box integerBox = new Box();
      Box stringBox = new Box();

      integerBox.add(10);
      stringBox.add(new String("Hello World"));

      printBoundBox(integerBox);
      printUnboundBox(stringBox);
   }

   //Bounded Types Erasure
   private static void printBoundBox(Box box) {
      System.out.println("Integer Value :" + box.get());
   }

   //Unbounded Types Erasure
   private static void printUnboundBox(Object box) {
      System.out.println("String Value :" + ((Box)box).get());
   }
}

class Box {
   private Object t;

   public void add(Object t) {
      this.t = t;
   }

   public Object get() {
      return t;
   }   
}
```

## 泛型限制

### 不支持基本类型

使用泛型时，基本类型不能作为类型参数传递。在下面给出的示例中，如果我们将 int 类型传递给 Box 类，那么编译器会报错。为了解决这种情况，我们需要传递 Integer 对象而不是 int 基本类型。

```java
public class GenericsTester {
    public static void main(String[] args) {
       Box<Integer> integerBox = new Box<Integer>();

       //Syntax error
       //Box<int> stringBox = new Box<int>();

       integerBox.add(10);
       printBox(integerBox);
    }

    private static <T> void printBox(Box<T> box) {
       System.out.println("Value: " + box.get());
    }  
 }

 class Box<T> {
    private T t;

    public void add(T t) {
       this.t = t;
    }

    public T get() {
       return t;
    }   
 }
```

### 不能实例化泛型类型参数

类型参数不能在方法内实例化其对象。

```java
public static <T> void add(Box<T> box) {
   // compiler error
   // Cannot instantiate the type T
   // T item = new T();  
   // box.add(item);
}
```

要实现此类功能，请使用反射。

```java
public static <T> void add(Box<T> box, Class<T> clazz) 
   throws InstantiationException, IllegalAccessException{
   T item = clazz.newInstance();   // OK
   box.add(item);
   System.out.println("Item added.");
}
```

### 不能使用静态变量

使用泛型时，类型参数不允许为静态变量（`static`）。由于静态变量在对象之间共享，因此编译器无法确定要使用的类型。

```java
class Box<T> {
   // compiler error
   private static T t;

   public void add(T t) {
      this.t = t;
   }

   public T get() {
      return t;
   }   
}
```

### 不能转换类型

泛型一个类的实例不能直接转换为另一个泛型类型参数不同的实例，即使它们的原始类型是相同的。比如 `List<String>` 不能转换为 `List<Integer>`。

```java
Box<Integer> integerBox = new Box<Integer>();
Box<Number> numberBox = new Box<Number>();
// Compiler Error: Cannot cast from Box<Number> to Box<Integer>
integerBox = (Box<Integer>)numberBox;
```

为了实现上面功能转换功能，可以使用无界通配符。

```java
private static void add(Box<?> box) {
   Box<Integer> integerBox = (Box<Integer>)box;
}
```

### 不能使用 instanceof 运算符

因为编译器使用类型擦除，运行时不会跟踪类型参数，所以在`Box<Integer>`和`Box <String>` 之间的运行时差异无法使用 `instanceOf` 运算符进行验证。所以类似下面的代码用法是错误的！

```java
Box<Integer> integerBox = new Box<Integer>();

// 编译错误：
// 不允许对参数化类型进行 instanceof 检查
// 参数化类型 Box<Integer> 的 instanceof 检查是不允许的
// 使用 Box<?> 形式，因为泛型类型信息将在运行时被擦除
if(integerBox instanceof Box<Integer>) { }
```

### 不支持数组

不允许使用参数化类型的数组。如下代码是错误的：

```java
// Cannot create a generic array of Box<Integer>
Box<Integer>[] arrayOfLists = new Box<Integer>[2];
```

因为编译器使用类型擦除，类型参数被替换为`Object`，用户可以向数组添加任何类型的对象。但在运行时，代码将无法抛出`ArrayStoreException`。


```java
// compiler error, but if it is allowed
Object[] stringBoxes = new Box<String>[];
  
// OK
stringBoxes[0] = new Box<String>();  

// An ArrayStoreException should be thrown,
// but the runtime can't detect it.
stringBoxes[1] = new Box<Integer>();  
```

### 不能使用异常

泛型不允许直接或间接继承`Throwable`类。

```java
// The generic class Box<T> may not subclass java.lang.Throwable
class Box<T> extends Exception {}

// The generic class Box<T> may not subclass java.lang.Throwable
class Box1<T> extends Throwable {}
```

在方法中，不允许捕获类型参数的实例，如下代码

```java
public static <T extends Exception, J> void execute(List<J> jobs) {
      try {
         for (J job : jobs) {}
  
         // compile-time error
         // Cannot use the type parameter T in a catch block
      } catch (T e) { 
         // ...
   }
} 
```

类型参数运行使用 Throwable 以及其子类。

```java
class Box<T extends Throwable> {
   private int t;

   public void add(int t) throws T {
      this.t = t;
   }

   public int get() {
      return t;
   }   
}
```

### 重载限制

如果两个方法的签名在擦除泛型后相同，则不能重载这两个方法。

```java
class Box {
   // Compiler error
   // Erasure of method print(List<String>) 
   // is the same as another method in type Box
   public void print(List<String> stringList) { }
   public void print(List<Integer> integerList) { }
}
```

最终在编译后的字节码中这两个方法都会变成如下签名

```java
class Box {
   public void print(List list) { }
}
```
