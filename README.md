# Top 10 features in Kotlin
Teching myself Kotlin for two years and having written two full blown applications in the language, here is my take on 10 awesome features of Kotlin that could (possibly) convice a seasoned Java developer to start his/her journey into Kotlin. I have taken the "Talk is cheap. Show me the code" approach of Linus Torvalds.

1. Null-safe and Elvis operation
	"Null References: The Billion Dollar Mistake" Tony Hoare (https://www.infoq.com/presentations/Null-References-The-Billion-Dollar-Mistake-Tony-Hoare/)

	Kotlin provides a way to think beforehand to avoid that mistake with nullable references.

	```kotlin
	var s1: String  = "abc"   // s1 is not-nullable
	var s2: String? = "abc"   // s2 is nullable

	s1 = null    // compilation error
	s1 = s2      // compilation error

	s2 = s1      // o.k.
	s2 = null    // o.k.
	```

	#### Safe calls 

	```kotlin
	println(s1.length) 		//s1 is guarenteed to be not null
	println(s2?.length)     //safe call : returns length of s2 or null otherwise but does not throw a NPE
	```

	#### The '!!' operator
	```kotlin
	println(s2!!.length)     //when s2 is designed to be not null and ignores the check of null safety, 
	```

	#### Elvis operator
	```kotlin
	println(s2?.length ?: 0)     // returns length of s2, if s2 is not null or 0 otherwise
	```

	#### A bit extreme case
	```kotlin
	val departmentHead = university?.science?.department?.name ?: "does not exist."
	```	

	Equivalent in Java :
	```Java
	String departmentHead = null;
	if (university != null) {
		if (university.getScience() != null) {
			if (university.getScience().getName() != null) {
				return university.getScience().getName();
			}
			return "does not exist";
		}

		return "does not exist";
	}  
	```	

2. Data Class

	Kotlin knows how to "Walk the talk", FTW!
	
	```kotlin
	data class User(var name: String, var age: Int)
	```

	Equivalent in Java :
	```Java
	public class User
	  {
	    private String    name;
	    private Integer    age;
	    
	    ...   // constructor with every field
	    ...   // get()/set() methods for each field
	    ...   // method toString()
	    ...   // methods hashCode() and equals()
	  }
    ```

3. Multiple class in a single File
	File name in Java is tightly connected to the class name and only one public class can be defined in a file. No such restriction anymore.

	```kotlin
	// Student.kt
	data class User(var name: String, var age: Int, var homeAdress: Address)

	data class Address(var streetName:String, var blockNumber: Int, postNumber: Int)
	```


4. Switch construct with when
	A switch with Superpowers

	```Java
	final int x = // value;
	final String xResult;

	switch (x){
	  case 0:
	  case 11:
	    xResult = "0 or 11";
	    break;
	  case 1:
	  case 2:
	    //...
	  case 10:
	    xResult = "from 1 to 10";
	    break;
	  default:
	    if(x < 12 && x > 14) {
	      xResult = "not from 12 to 14";
	      break;
	    }

	    if(isOdd(x)) {
	      xResult = "is odd";
	      break;
	    }

	    xResult = "otherwise";
	}

	```

	```kotlin
	val x = // value
	val xResult = when (x) {
	  0, 11 -> "0 or 11"
	  in 1..10 -> "from 1 to 10"
	  !in 12..14 -> "not from 12 to 14"
	  else -> if (isOdd(x)) { "is odd" } else { "otherwise" }
	}
	```

	#### Arbitrary Condition Branches
	```kotlin
	var result = when(number) {
	    0 -> "Invalid number"
	    1, 2 -> "Number too low"
	    3 -> "Number correct"
	    in 4..10 -> "Number too high, but acceptable"
	    !in 100..Int.MAX_VALUE -> "Number too high, but solvable"
	    else -> "Number too high"
	}
	```

	#### Smart Cast
	```kotlin
	var x:Any = getResult()

	when (x) {
	    is Int -> print(x + 1)
	    is String -> print(x.length + 1)
	    is IntArray -> print(x.sum())
	}
	```

	#### When without an argument
	```kotlin
	val y = // value
	val yResult = when {
	  isNegative(y) -> "is Negative"
	  isZero(y) -> "is Zero"
	  isOdd(y) -> "is odd"
	  else -> "otherwise"
	}
	```


5. Object deconstruction

	#### unpacking with style

	```kotlin
	data class Book(val author: String, val title: String, val year: Int)

	// ...
	val book = Book("Gillian Flynn", "Gone girl", 2012)

	val (author, title, year) = book
	if (year > 2010) {
	    // ...
	}
	```

	#### Simplifying loops on Collections and Maps:
	```kotlin
	val books = listOf<Book>()
	for ((author, title, year) in books) {
	    // ...
	}
	```

	#### Destructuring in lambdas
	```kotlin
	books.map { (author, title, year) ->  
	    // ...
	}
	```

	#### Skipping unnecessary variables
	```kotlin
	val (_, _, year) = book
    
	for ((_, title, _) in books) {
	    // ...
	}

	books.map { (_, _, year) ->  
	    // ...
		}
	```

6. Extension functions

	#### Work smart, not work hard
	```kotlin
	fun List.midElement(): T {
	    if (isEmpty())
	        throw NoSuchElementException("List is empty.")
	    return this[size / 2]
	}

	var list = listOf(1, 2, 3, 4, 5)
	var mid = list.midElement()
	```

7. Default values (optional arguments) and named argument

 	#### Swiss knife of Kotlin.
	```Java
	public void hello(String name) {
	  if (name == null) {
	    name = "World";
	  }

	  System.out.print("Hello, " + name + "!");
	}
	```

	```kotlin
	fun hello(name: String = "World") {
	    println("Hello, $name!")
	}
	```

	```kotlin
		fun connect(url: String, connectTimeout: Int = 1000, enableRetry: Boolean = true) {
	    println("The parameters are url = $url, connectTimeout = $connectTimeout, enableRetry = $enableRetry")
	}

	#### named arguments
	connect(
		connectTimeout = 500,
		url = "www.google.com"
	)
	```

8. Operator overloading
	```kotlin
	data class Point(val x: Double, val y: Double)

	// Here how to provide `+` operator on our object Point
	operator fun plus(p: Point) = Point(this.x + p.x, this.y + p.x)

	//similarly
	operator fun Point.minus(p: Point) = Point(x - p.x, y - p.y)
	operator fun Point.times(p: Point) = Point(x * p.x, y * p.y)
	operator fun Point.div(p: Point) = Point(x / p.x, y / p.y)
	operator fun Point.unaryPlus() = Point(x + 1, y + 1)
	operator fun Point.unaryMinus() = Point(x - 1, y - 1)

	// the implementation of equals is automatically generated by the compiler
	fun Point.equals(obj: Any?) : Boolean {
	    if (obj === this) return true
	    if (obj !is Point) return false
	    return obj.x == x && obj.y == y
	}

	//usage
	val point1 = Point(1,1)
	val point2 = Point(2,2)

	println(point1 + point2)  //Point(x=3, y=3)
	println(+point1)          //Point(x=2, y=2)
	println(Point(1,1) == Point(1,1)) //true

	```
	
9. Smart Casting
	#### Casting done smoothly and painlessly
	```Java
	if(s instanceof String){
	  final String result = ((String) s).substring(1);
	}
	```

	```kotlin
	if (s is String) {
	  val result = s.substring(1)
	}
	```

10. String template
	#### simply a string literal containing embedded expressions.
	```Java
	String message = "n = " + n;
	```

	```kotlin
	val message = "n = $n"
	```

	#### String template can contain any valid expression
	```kotlin
	val message1 = "n + 1 = ${n + 1}"
	val message2 = "$n is ${if(n > 0) "positive" else "not positive"}
	```

	#### Can be nested
	```kotlin
	val message = "$n is ${if (n > 0) "positive" else 
  		if (n < 0) "negative and ${if (n % 2 == 0) "even" else "odd"}" else "zero"}"
	```

	#### Raw String
	```Java
	String path = "C:\\Repository\\read.me";
	```

	```kotlin
	val path = """C:\Repository\read.me"""
	```

	#### Multiline String
	```kotlin
	val receipt = """Item 1: $1.00
                >Item 2: $0.50""".trimMargin(">")
  ```






