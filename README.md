# Map

## Learning Goals

- Understand the Map interface.
- Review the commonly used methods of the HashMap.
- Discuss iterating over a HashMap.

## Map Interface

While Lists are ordered collections of items that are referenced by their
position in the list, **maps** are lists of what are known as "key/value" pairs,
where a specific "key" is mapped to a specific "value". Duplicate keys are not
allowed and a key can only map to one value.

## Map Implementations

Like the `Collection` interface, the `Map` interface has multiple
implementations that focus on different aspects of maps. The following are the
most commonly used implementations:

1. `HashMap`: permits null keys and null values, and makes no guarantee on the
   order of the elements - this is the implementation we will focus on in this
   section.
2. `LinkedHashMap`: is like a `HashMap` except it maintains the order in
   which the items are inserted. We will talk more about this type of map in
   a later module.
3. `TreeMap`: is a sorted map, based on the natural ordering of the keys (or
   based on a custom sort). Like the `LinkedHashMap`, this implementation of the
   map interface is beyond the scope of this lesson.

## HashMap in Action

Consider we have a list of students, and we want to keep track of their letter
grade in a specific class. Instead of keeping up with two lists, one for the
students and one for their grades, we can instead make use of the `Map`
interface! We can make the key the student's name and our value the student's
letter grade:

```java
Map<String, Character> studentGrades = new HashMap<String, Character>();
```

To use the `Map` interface and the `HashMap` class, we will need to import them
from the `java.util` package:

```java
import java.util.Map;
import java.util.HashMap;

public class MapExample {
   public static void main(String[] args) {
      Map<String, Character> studentGrades = new HashMap<String, Character>();
      studentGrades.put("Dustin", 'B');
      studentGrades.put("Suzie", 'A');
      studentGrades.put("Mike", 'C');

      System.out.println(studentGrades.get("Dustin"));
      System.out.println(studentGrades.get("Suzie"));
      System.out.println(studentGrades.get("Mike"));
   }
}
```

We can add key/value pairs to our `Map` by using the `put()` method which takes
two parameters - the key object and the value object. We can access the value of
the items in our list by using the `get()` method, which takes the key object as
its parameter.

Similarly, for maps, we don't have to include the data types in the second
diamond operator `<>` since it will be implied by the `Map` declaration on the
left-hand side of the assignment `=` operator.

### HashMap Constructors

Another way we could have written the above example is to define the map as a
`HashMap` from the start like so:

```java
HashMap<String, Character> studentGrades = new HashMap<>();
```

While this is fine, it is almost always better to declare a map using `Map`
instead of `HashMap`. Remember, `Map` is an interface, and if we needed to
swap out the implementation of a `HashMap`, for maybe a `LinkedHashMap`, then
the change would be more difficult if being used by other classes within the
codebase.

Just like the  `ArrayList` and `HashSet`, if we have an estimate of how many
items will be in the `HashMap`, then we can specify the initial capacity like
so:

```java
Map<String, Character> studentGrades = new HashMap<>(3);
```

If we know some key/pair values when we are initializing the `HashMap`,
we could even construct it like this:

```java
import java.util.Map;
import java.util.HashMap;

public class MapExample {
    public static void main(String[] args) {
        Map<String, Character> studentGrades = new HashMap<>(
            Map.ofEntries(Map.entry("Dustin", 'B'),
                   Map.entry("Suzie", 'A'),
                   Map.entry("Mike", 'C')
            )        
         );
        System.out.println(studentGrades.get("Dustin"));
        System.out.println(studentGrades.get("Suzie"));
        System.out.println(studentGrades.get("Mike"));
    }
}
```

## Commonly Used Methods

Here are the key methods of the `HashMap` class:

| Method                        | Return Type                                                                               | Description                                         |
|-------------------------------|-------------------------------------------------------------------------------------------|-----------------------------------------------------|
| `put(Key key, Value value)`   | value previously associated with the key, or null if there was no mapping for the key     | Adds or Updates a key/value pair to the map         |
| `get(Key key)`                | value of the given key, or null if the map does not contain the given key                 | Gets a value from a map based on its key            |
| `remove(Key key)`             | value that is removed at the given key, or null if the map does not contain the given key | Removes a key/value pair from a map                 |
| `size()`                      | int                                                                                       | Gets the current size of the map                    |
| `contains(Key key)`           | boolean                                                                                   | Checks to see if a map contains a specific key      |
| `contains(Value value)`       | boolean                                                                                   | Checks to see if a map contains a specific value    |
| `isEmpty()`                   | boolean                                                                                   | Checks to see if a list is empty or has a size of 0 |
| `keyset()`                    | set of keys                                                                               | Gets a set of the keys in the current map           |
| `values()`                    | collection of values                                                                      | Gets a collection of values in the current map      |
| `entry(Key key, Value value)` | map entry                                                                                 | Converts the key/value pair into a Map.Entry        |

## Iterating Through a Map

In the last method, we saw something called `Map.Entry` and we saw the `entry()`
method used when constructing the last `HashMap`... but what is a `Map.Entry`?
`Map.Entry` is another interface that refers to the key/pair value from
an entry set. An entry set is a `Set` of all the mappings contained in a `Map`.
To get an entry set from a `Map`, we can call the `entrySet()`
method:

```java
Set<Map.Entry<String, Character>> studentGradeSet = studentGrades.entrySet();
```

Since our `Map` is now converted into a `Set`, which is just a special
kind of `List`, we can iterate through a `Map` like so:

```java
import java.util.Map;
import java.util.HashMap;

public class MapExample {
   public static void main(String[] args) {
      Map<String, Character> studentGrades = new HashMap<>();

      studentGrades.put("Dustin", 'B');
      studentGrades.put("Suzie", 'A');
      studentGrades.put("Mike", 'C');

      for (Map.Entry<String, Character> studentGrade : studentGrades.entrySet()) {
         System.out.println(studentGrade.getKey() + "'s grade is " + studentGrade.getValue());
      }
   }
}
```

Now that we are in a `for` loop, we can easily get each map entry's key and
value. We may notice that we can use the methods `getKey()` and `getValue()`
which will return the key and the value for each of our map entries.

Let's take a look at this code in the Java Visualizer. We'll place a breakpoint
at the `for` loop.

![studentgrades-hashmap-visualizer](https://curriculum-content.s3.amazonaws.com/java-mod-3/map/java-visualizer-hashmap-student-grades.png)

As we can see, the `Map`, unlike the `List`, does **not** store the key/value
pairs in the order that they are inserted. By looking at how the `HashMap` is
stored in memory, we can see that the first key/value pair we might print out is
actually going to be "Mike" rather than "Dustin".

Let's switch over to the debugger now and use the "step-over" action to see what
the `studentGrade` variable will be assigned to when iterating through the map:

![debug-first-iteration](https://curriculum-content.s3.amazonaws.com/java-mod-3/map/debugger-map-first-iteration.png)

Sure enough, we see that the `studentGrade` variable is referencing the
key/value pair with "Mike" and 'C'. If we continue stepping over, we'll see
`Mike's grade is C` is printed to the console.

Let's look at the second iteration of the `for` loop:

![debug-second-iteration](https://curriculum-content.s3.amazonaws.com/java-mod-3/map/debugger-map-second-iteration.png)

As we can see, the next key/pair value that `studentGrade` is assigned to
"Dustin" and 'B'. The way Java iterates through the map is the way the map is
stored in memory. We can see in the visualizer above that the order goes "Mike",
"Dustin", and then "Suzie". If we continue stepping over, we'll see that on the
third iteration of the loop, `studentGrade` gets assigned to "Suzie" and 'A' as
the last key/value pair.

![debug-third-iteration](https://curriculum-content.s3.amazonaws.com/java-mod-3/map/debugger-map-third-iteration.png)

The above code will output the following:

```text
Mike's grade is C
Dustin's grade is B
Suzie's grade is A
```

## References

- [Java 17 Map Interface](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Map.html)
- [Java 17 HashMap](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/HashMap.html)
