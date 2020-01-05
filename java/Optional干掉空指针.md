1.null引用在历史上被引入到程序设计语言中，目的是为了表示变量值得缺失。

2.java 8中引入了一个新的类java.util.Optional<T>,对存在或缺失的变量值进行建模。

3.Optional类支持多种方法，比如map,flatMap,filter,它们在概念上与Stream类中对应的方法十分相似。

4.使用Optional会迫使你更积极地引用Optional对象，以应对变量缺失的问题。

5.使用Optional能帮助你设计更好的API.