##### JDK1.8--体验Stream表达式，从一个对象集合中获取每一个对象的某一个值返回新集合

  ```java
public static void main(String[] args) {

    A a = new A();
    a.setId(1);
    a.setAge(22);
    a.setName("a");

    A a1 = new A();
    a1.setId(2);
    a1.setAge(22);
    a1.setName("a1");

    A a2 = new A();
    a2.setId(3);
    a2.setAge(22);
    a2.setName("a2");

    A a3 = new A();
    a3.setId(4);
    a3.setAge(22);
    a3.setName("a3");

    List<A> list = new ArrayList<>();
    list.add(a3);
    list.add(a2);
    list.add(a1);
    list.add(a);

    List<Integer> list1 = list.stream().map(A::getId).collect(Collectors.toList());
    List<Integer> list2 = list.stream().map(A::getAge).collect(Collectors.toList());//未去重
    List<Integer> list3 = list.stream().map(A::getAge).distinct().collect(Collectors.toList());//已去重
    System.out.println(list1);
    System.out.println(list2);
    System.out.println(list3);
}

static class A{
    private Integer id;
    private Integer age;
    private String name;
    public Integer getId() {
        return id;
    }
    public void setId(Integer id) {
        this.id = id;
    }
    public Integer getAge() {
        return age;
    }
    public void setAge(Integer age) {
        this.age = age;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
}
  ```



# Collectors toMap

## 1. 介绍

在本教程中，我们将讨论`Collectors`类的`toMap()`方法。我们使用它将流收集到一个`Map`实例中。

对于本教程中涉及的所有示例，我们将使用图书列表作为数据源，并将其转换为不同的`Map`实现。

## 2. List 转换 Map

我们将从最简单的情况开始，将`List` 转换 `Map`。

`Book`类定义如下:

```java
class Book {
    private String name;
    private int releaseYear;
    private String isbn;
    //getters and setters
}
```

接着，我们将创建一个`List<Book>`来验证我们的代码:

```java
List<Book> bookList = new ArrayList<>();
bookList.add(new Book("The Fellowship of the Ring", 1954, "0395489318"));
bookList.add(new Book("The Two Towers", 1954, "0345339711"));
bookList.add(new Book("The Return of the King", 1955, "0618129111"));
```

对于这个场景，我们将使用以下重载的`toMap()`方法:

```java
Collector<T, ?, Map<K,U>> toMap(Function<? super T, ? extends K> keyMapper,
  Function<? super T, ? extends U> valueMapper)
```

使用`Collectors.toMap()`, 我们将会得到一个`Map<String,String>`,其中key是isbn的值，value为name的值。

```java
public Map<String, String> listToMap(List<Book> books) {
    return books.stream().collect(Collectors.toMap(Book::getIsbn, Book::getName));
}
```

我们使用单元测试来验证一下代码:

```java
@Test
public void whenConvertFromListToMap() {
    assertTrue(convertToMap.listToMap(bookList).size() == 3);//true
}
```

## 3. 解决 Key 的冲突

上面的例子运行得很好，但是如果有一个重复的key会发生什么呢?

让我们来想象一下，我们将每本图书的出版年份作为key，转换到`Map<Integer, Book>`中。

```java
public Map<Integer, Book> listToMapWithDupKeyError(List<Book> books) {
    return books.stream().collect(Collectors.toMap(Book::getReleaseYear, Function.identity()));
}
```

鉴于我们上面的例子，我们会看到一个`IllegalStateException`的异常:

```java
@Test(expected = IllegalStateException.class)
public void whenMapHasDuplicateKey_without_merge_function_then_runtime_exception() {
    convertToMap.listToMapWithDupKeyError(bookList);
}
```

要解决这个问题，我们需要使用另一种`toMap()`方法，附加一个参数，mergeFunction:

```java
Collector<T, ?, M> toMap(Function<? super T, ? extends K> keyMapper,
  Function<? super T, ? extends U> valueMapper,
  BinaryOperator<U> mergeFunction)
```

让我们引入一个merge函数，它表明，在发生冲突的情况下，我们保留现有的元素:

```java
public Map<Integer, Book> listToMapWithDupKey(List<Book> books) {
    return books.stream().collect(Collectors.toMap(Book::getReleaseYear, Function.identity(),
      (existing, replacement) -> existing));
}
```

或者，换句话说，我们获得了未发生异常的元素:

```java
@Test
public void whenMapHasDuplicateKeyThenMergeFunctionHandlesCollision() {
    Map<Integer, Book> booksByYear = convertToMap.listToMapWithDupKey(bookList);
    assertEquals(2, booksByYear.size());
    assertEquals("0395489318", booksByYear.get(1954).getIsbn());
}
```

## 4. 其他Map类型

默认情况下，`toMap()`方法将返回一个`HashMap`。
但是我们也可以返回不同的`Map`实现。

```java
Collector<T, ?, M> toMap(Function<? super T, ? extends K> keyMapper,
  Function<? super T, ? extends U> valueMapper,
  BinaryOperator<U> mergeFunction,
  Supplier<M> mapSupplier)
```

其中mapSupplier是一个函数，它返回一个新的、带有结果的空`Map`。

### 4.1. List 转换 ConcurrentMap

让我们以上面的例子为例，添加一个mapSupplier函数来返回一个`ConcurrentHashMap`:

```java
public Map<Integer, Book> listToConcurrentMap(List<Book> books) {
    return books.stream().collect(Collectors.toMap(Book::getReleaseYear, Function.identity(),
      (o1, o2) -> o1, ConcurrentHashMap::new));
}
```

下面我们测试一下

```java
@Test
public void whenCreateConcurrentHashMap() {
    assertTrue(convertToMap.listToConcurrentMap(bookList) instanceof ConcurrentHashMap);
}
```

### 4.2. List 转换 SortedMap

最后，让我们看看如何返回一个排序后的Map。为此，我们需要对`List<Book>`进行排序，并使用`TreeMap`作为mapSupplier参数:

```java
public TreeMap<String, Book> listToSortedMap(List<Book> books) {
    return books.stream() 
      .sorted(Comparator.comparing(Book::getName))
      .collect(Collectors.toMap(Book::getName, Function.identity(), (o1, o2) -> o1, TreeMap::new));
}
```

上面的代码将`List<Book>`按照书名进行排序，然后将结果收集到`TreeMap<String, Book>`中:

```java
@Test
public void whenMapisSorted() {
    assertTrue(convertToMap.listToSortedMap(bookList).firstKey().equals("The Fellowship of the Ring"));
}
```

## 5. 结论

在本文中，我们研究了`Collectors`类的`toMap()`方法。它允许我们从一个流创建一个新的`Map`。我们还学习了如何解决key冲突和创建不同的`Map`实现。