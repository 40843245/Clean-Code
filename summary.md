# `<<Clean Code>>`
## important principle
### CH17 - good practice of clean code
#### Comments
##### C1: Inappropriate Information Should be Removed

Comments that contain inappropriate informations should be removed as possible.

Comments that contain inappropriate informations include:
+ changelog info (such as `author name`, `last modified datetime`. Extra info[^1], It should be in changelog file. Or a better way to store changelog info is that to use Source Code Manager. It will stores all changes about source code.
+ the comment of method describes something that is different from the method itself. Items applies to method, function, package etc.

##### C2: Obsolete Comments Should be Removed or Updated

Obsolete comments should be removed or updated.

Obsolete comments includes:
+ the code snippet under the next line of comment is change but the comment is not updated.

##### C3: Redundant Comments Should be Removed

Redundant comments should be removed.

Redudant comments are comnents that one can easily understand what the code snippet or code without it.

Redudant comments includes:

+ obvious info that almost people should know. Such as

```
i++; // increase i by 1.
```

##### C4: Poorly Written Comments Should be Removed or Updated

One should write comments with correct syntax and correct word. And the comments should easily understand as possible. Like describe something into a newbabie.

##### C5: Commented-Out Code

After each test, the commented-out code should be removed. The commented-code may ***NEVER** be used forever.

#### Environment
##### E1: Build Requires Steps As Less As Possible

It is better to build a new environment (or virtual environment (i.e. `venv`) for developing a project with step as less as possible.

> [!IMPORTANT]
> Almost IDE provides a functionality to build a new environment.

> [!IMPORTANT]
> Almost IDE also provides a command in command line to build a new environment. In most case, ***only*** one command is required.

##### E2: Tests Require Steps As Less As Possible

It is better that each test requires steps as less as possible.

> [!IMPORTANT]
> Almost IDE provides a functionality to run the test.

> [!IMPORTANT]
> Almost IDE also provides a command in command line to run the test. In most case, ***only*** one or two commands is required. (In some IDE and specific progamming language, it is required that one command for compiling the code and another one for running the test.)

#### Functions
##### F1: Number of Parameters Should As Less As Possible
Number of parameters should as Less as possible since more parameters the function or method has, the more code one has to write (and it does NOT increase the readability and flexibility.)

##### F2: Avoid Output Argument As Possible

> [!IMPORTANT]
> In function, one passed argument in function call. And the parameter of function will be used during the function call. After that, the function will return a value or `void`.

So, the arguments should be used for inputs.

If one has to change an object status in function or method, please follow these steps.

1. put the function or method into the class that instantiate the object.
2. change it from the object name in the method to `this` keyword.

> [!NOTE]
> In Java, the `this` keyword indicates the class itself.

First, I grab the code from geeksforgeeks [Different Ways to Achieve Pass By Reference in Java](https://www.geeksforgeeks.org/different-ways-to-achieve-pass-by-reference-in-java/) and modify a little.

It is ***NOT*** a clean code. One passed the object `ob` as an argument and change its property in `inc` function which violates the rule.

`F2_2.java` at Example Code of `<<Clean Code>>`[^1]

```
import java.io.*;

class GFG {
	int number;
	void GFG() { this.number = 0; }

	static void inc(GFG ob,int step) { ob.number+= step ; }

	public static void main(String[] args)
	{
		GFG ob = new GFG();

		System.out.println("Number value " + (ob.number));

		inc(ob,1);

		System.out.println("Updated Number value "
						+ (ob.number));
	}
}
```

I modify a little bit to avoid the use case of output argument.

`F2_3.java` at Example Code of `<<Clean Code>>`[^1]

```
import java.io.*;

class GFG {
	int number;
	void GFG() { this.number = 0; }

	void inc(int step) { this.number+= step; }

	public static void main(String[] args)
	{
		GFG ob = new GFG();

		System.out.println("Number value " + (ob.number));

		ob.inc(1);

		System.out.println("Updated Number value "
						+ (ob.number));
	}
}
```

##### F3: Avoid Flag Arguments
If a function contains an argument which is used as a flag. It means that the function does NOT only execute one thing. It voilates the [single-responsibility principle](https://en.wikipedia.org/wiki/Single-responsibility_principle)

O.S.[^0]

> I think it does not matter to have a function with flag arguments.
>
> However, in a huge package (such as package about Android Studio), the code looks so dirty if a function with flag arguments is in code.

I modify an example from a teacher who teaches us Kotlin a little bit and take it as an example.

`F3_1.kt` at Example Code of `<<Clean Code>>`[^1]

```
fun getBusTimeInfo(busArriveMinute: Int, shouldStop: Boolean = true): String =
    if (shouldStop) {
        when (busArriveMinute) {
            0 -> "進站中"
            in 1..3 -> "將到站"
            else -> "預計在n分鐘站,n="+"${busArriveMinute}"
        }
    } else {
        "不停靠"
	}

fun main(){
	var i : Int = 0
	var tf:Boolean = false
	var booleanArray: ArrayList<Boolean> = ArrayList<Boolean>()
	booleanArray.add(true)
	booleanArray.add(false)

	for(tf in booleanArray){
		for(i in 0..4){
			println(getBusTimeInfo(i,tf))
		}
	}
}
```

`F3_2.kt` at Example Code of `<<Clean Code>>`[^1]

```
fun getBusTimeInfoOrOtherInfo(busArriveMinute: Int, shouldStop: Boolean = true,busArriveInfo:ArrayList<String>): String =
	if(shouldStop){
		getShouldStopBusTimeInfo(busArriveMinute,busArriveInfo)
	}else{
		busArriveInfo.get(3)
	}

fun getShouldStopBusTimeInfo(busArriveMinute:Int,busArriveInfo:ArrayList<String>):String =
	if(busArriveMinute==0){
		busArriveInfo.get(0)
	}else if(busArriveMinute in 1..3) {
		busArriveInfo.get(1)
	}else{
		busArriveInfo.get(2) + "${busArriveMinute}"
	}

fun main(){
	val list1 : ArrayList<String> = ArrayList<String>()
	list1.add("將到站")
	list1.add("進站中")
	list1.add("預計在n分鐘站,n=")
	list1.add("不停靠")

	var i : Int = 0
	var tf:Boolean = false
	var booleanArray: ArrayList<Boolean> = ArrayList<Boolean>()
	booleanArray.add(true)
	booleanArray.add(false)

	for(tf in booleanArray){
		for(i in 0..4){
			println(getBusTimeInfoOrOtherInfo(i,tf,list1))
		}
	}
}
```

##### F4: Remove Dead Function
Unused function should be removed since it will reduce the readability for human and it will spend more time to compile the code.

##### Extra info[^1] F5: A Function Should Only Do One Thing If Possible -- [Single-responsibility principle](https://en.wikipedia.org/wiki/Single-responsibility_principle)

Ideally, a function should only do one thing if possible. Otherwise, it will violate [Single-responsibility principle](https://en.wikipedia.org/wiki/Single-responsibility_principle). Then the code will be harder to read and understand.

#### General
##### G1: Avoid Multiple Languages in One Source File As Possible
Always avoid multiple languages in one source file as possible.

If there are more than multiple languages in one source file, one should try to remove other language into other file as source file. 

Especially for comment, it should be place in other file as notes (such as `README.md`).

##### G2: Implement All Obvious Behavior
According to [`Principle of least astonishment`](https://en.wikipedia.org/wiki/Principle_of_least_astonishment), all functions and non-abstract methods should be implemented.

It is NOT allowed after one finishes to write code or develop a big project.

`G2_1.kt` at Example Code of `<<Clean Code>>`[^1]

```
fun addIntegers(num1:Int,num2:Int):Int{
	// TODO
	return num1
}
fun main(){
	println(addIntegers(2,5))
}
```

##### G3: Fix Incorrect Behavior at Boundaries

> [!IMPORTANT]
> Always tests for all cases until they are passed, even if one simply test a simple method.

> [!CAUTION]
> ***DON'T*** depend on your intuition and think a simple method you write has no buggy.

##### G4: NOT to Overridden Safeties

> [!CAUTION]
> NOT to ignore any warnings, errors, and other bugs. Especially, errors and other bugs.
>
> it is dangerous to ignore any warnings, errors, and other bugs.

The concept of ignoring any warnings, errors, and other bugs is similar to the concept of one ignores any warnings whcich are pasted in a building site and walk into there (the worst case is the one get hurts).

##### G5: Rewrite if Duplication exist.
My teacher in C/C++ in NFU, at C course, teaches why function is important along with the principle -- [`DRY` (stands for `Don't repeat yourself`)](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)

The teacher first shows a C code without using a function and then a C code with using a function.

At that moment, I realize why the `DRY` principle is huge important. It is important since it is easier to read and maintain.

> [!IMPORTANT]
> [`DRY` (stands for `Don't repeat yourself`)] principle is huge important. It has a few pros.
>
> + more readable
> + easier to maintain
> + less buggy
> + etc

First, I will write a code that violates `DRY`.

`G5_1.c` at Example Code of `<<Clean Code>>`[^1]

```
/*
it is equivalent to 
	+ G5_2.c
*/
#include <stdio.h>

int main(){
	int a = 0, b = 2;

	a = 3;
	b = 5;
	printf("Do some addition.");
	printf("%d+%d=%d",a,b,a+b);
	printf("Do some multiplication.");
	printf("%d*%d=%d",a,b,a*b);
	
	a = 7;
	b = 9;
	printf("Do some addition.");
	printf("%d+%d=%d",a,b,a+b);
	printf("Do some multiplication.");
	printf("%d*%d=%d",a,b,a*b);

	a = 1;
	b = 3;
	printf("Do some addition.");
	printf("%d+%d=%d",a,b,a+b);
	printf("Do some multiplication.");
	printf("%d*%d=%d",a,b,a*b);

	a = -1;
	b = 4;
	printf("Do some addition.");
	printf("%d+%d=%d",a,b,a+b);
	printf("Do some multiplication.");
	printf("%d*%d=%d",a,b,a*b);
}
```

Then we look at code which obeys `DRY`.

`G5_2.c` at Example Code of `<<Clean Code>>`[^1]

```
/*
it is equivalent to 
	+ G5_1.c
*/
#include <stdio.h>

void doSomething(int a,int b){
	printf("Do some addition.");
	printf("%d+%d=%d",a,b,a+b);
	printf("Do some multiplication.");
	printf("%d*%d=%d",a,b,a*b);
}
int main(){
	int a = 0, b = 2;

	a = 3;
	b = 5;
	doSomething(a,b);
	
	a = 7;
	b = 9;
	doSomething(a,b);

	a = 1;
	b = 3;
	doSomething(a,b);
	
	a = -1;
	b = 4;
	doSomething(a,b);

	return 0;
}
```

Obviously, `G5_2.c` at Example Code of `<<Clean Code>>`[^1] is more readable and easier to maintain then `G5_1.c` at Example Code of `<<Clean Code>>`[^1], isn't it?

##### G6: Code at Wrong Level Of Abstraction

> [!IMPORTANT]
> It is important to create abstractions that separate higher level general concepts from lower level detailed concepts. 

> [!CAUTION]
> Sometimes we do this by creating abstract classes to hold the higher level concepts and derivatives to hold the lower level concepts.
>
> When we do this, we need to make sure that the separation is complete. 

> [!IMPORTANT]
> We want all the lower level concepts to be in the derivatives and all the higher level concepts to be in the base class.

Consider the following code.

`G6_1.java` at Example Code of `<<Clean Code>>`[^1]

```
   public interface Stack {
     Object pop() throws EmptyException;
     void push(Object o) throws FullException;
     double percentFull();

     class EmptyException extends Exception {}
     class FullException extends Exception {}
   }
```

The `percentFull` function is at the wrong level of abstraction.

Although there are many implementations of Stack where the concept of fullness is reasonable, 

there are other implementations that simply could not know how full they are. So the function would be better placed in a derivative interface such as `BoundedStack`.

##### G7: Avoid Based Class Depending on Their Derived Class.

> [!IMPORTANT]
> A derived class (for example `DerivedClass`) derives to a base class (for example `BaseClass`)

In this case, if `BaseClass` depends on `DerivedClass`. Then phenomenons will occur.

+ Change of `BaseClass` affects `DerivedClass` then affects `BaseClass`. A dead circular may occur. And thus, it may lead to infinite compilation at compile time.
+ Change of `DerivedClass` affects `BaseClass`.

These phenomenons are strange, aren't they?


#### Names
##### N1: Choose Descriptive Name

Look at these code snippets.

`N1_1.java` at Example Code of `<<Clean Code>>`[^2].

```
public int x() {
	int q = 0;
	int z = 0;
	for (int kk = 0; kk < 10; kk++) {
	  if (l[z] == 10)
	  {
		q += 10 + (l[z + 1] + l[z + 2]);
		z += 1;
	  }
	  else if (l[z] + l[z + 1] == 10)
	  {
		q += 10 + l[z + 2];
		z += 2;
	  } else {
		q += l[z] + l[z + 1];
		z += 2;
	  }
	}
	return q;
  }
```

`N1_2.java` at Example Code of `<<Clean Code>>`[^2].

```
public int score() {
	int score = 0;
	int frame = 0;
	for (int frameNumber = 0; frameNumber < 10; frameNumber++) {
	  if (isStrike(frame)) {
		score += 10 + nextTwoBallsForStrike(frame);
		frame += 1;
	  } else if (isSpare(frame)) {
		score += 10 + nextBallForSpare(frame);
		frame += 2;
	  } else {
		score += twoBallsInFrame(frame);
		frame += 2;
	  }
	}
	return score;
  }
```

which one is readable? Obviously, is `N1_2.java` at Example Code of `<<Clean Code>>`[^2], isn't it? 

(Though `N1_2.java` at Example Code of `<<Clean Code>>`[^2] is NOT more complete than `N1_1.java` at Example Code of `<<Clean Code>>`[^2].)

##### N2: Choose Names at the Appropriate Level of Abstraction

Again, look at these code snippets.

`N2_1.java` at Example Code of `<<Clean Code>>`[^2]

```
public interface Modem {
	boolean dial(String phoneNumber);
	boolean disconnect();
	boolean send(char c);
	char recv();
	String getConnectedPhoneNumber();
  }
```

`N2_2.java` at Example Code of `<<Clean Code>>`[^2]

```
public interface Modem {
	boolean connect(String connectionLocator);
	boolean disconnect();
	boolean send(char c);
	char recv();
	String getConnectedLocator();
  }
```

which is more extensible? Though they have some extensibility, in `N2_2.java` at Example Code of `<<Clean Code>>`[^2], the name of property looks so descriptive in different cases (***not only*** about phone numbers, ***but also*** about all connectors). And

in `N2_2.java` at Example Code of `<<Clean Code>>`[^2], he name of property looks so descriptive ***only*** in cases about phone numbers. Thus, it is better way to name these property like in `N2_2.java` at Example Code of `<<Clean Code>>`[^2].

##### N3: Use Standard Nomenclature Where Possible

Such as the case.

Case 1:

If you are defining a function of decorator, it is recommend to name the function name as `xxxDecorator`.

Case 2:

If you are defining a function that converts it to string, it is recommend that name the function name as `toString()`.

Case 3:

If you are defining a method of a class that converts it to string, it is recommend that name the method name as `toString()`.

##### N4: Unambiguous Names

`N4_1.java` at Example Code of `<<Clean Code>>`[^2]

```
private String doRename() throws Exception
   {
     if(refactorReferences)
       renameReferences();
     renamePage();
 
     pathToRename.removeNameFromEnd();
     pathToRename.addNameToEnd(newName);
     return PathParser.render(pathToRename);
   }
```

as a reader, can one see the difference between `doRename` and `renamePage`? Obvisuously, can not, can it? Thus, it is NOT recommend to name it like this way.

##### N5: Use Long Name for Long Scope

From the old standard `Bowling Game`.

`N5_1.java` at Example Code of `<<Clean Code>>`[^2]

```
 private void rollMany(int n, int pins)
   {
     for (int i=0; i<n; i++)
       g.roll(pins);
   }
```

it is readable, isn't it? 

On the other hand, if we define a method with many expressions, we have to use a more precise name. Thus, it is recommend that more expression the function has, a longer name one gives.

##### N6: Avoid Encodings

Such as `m_phoneNumber` is bad way to give a name at present. It is better to name it such as `myPhoneNumber`.

##### N7: Names Should Describe Side-Effects

From `testNG`.

`N7_1.java` at Example Code of `<<Clean Code>>`[^2]

```
public ObjectOutputStream getOos() throws IOException {
     if (m_oos == null) {
       m_oos = new ObjectOutputStream(m_socket.getOutputStream());
     }
     return m_oos;
   }
```

`N7_2.java` at Example Code of `<<Clean Code>>`[^2] looks better since one can more easily to known what actually the method do. On the other hand, I rename `m_oos` to `myOss` according `N6: Avoid Encodings` section.

`N7_2.java` at Example Code of `<<Clean Code>>`[^2]

```
public ObjectOutputStream createOrReturnOos() throws IOException {
     if (myOos == null) {
		myOos = new ObjectOutputStream(m_socket.getOutputStream());
     }
     return myOos;
   }
```

#### Tests
##### T1: Sufficient Tests

If any cases are NOT be tested, the test is ***NOT*** sufficient enough.

##### T2: Use a Coverage Tool

Coverage tools can tell one what cases have NOT be tested yet. Additionally, some useful IDE will provide one tips visually to tell one what cases have NOT be tested yet.

##### T3: Don't Skip Trivial Tests
Do some trivial tests takes less time. But it is important since one can NOT tell that the trivial tests will be success unless one do trivial tests. In addition, if one fails on one or more trivial tests, then the code snippet has bugs.

##### T4: An Ignored Test Is a Question about an Ambiguity

As a programmer, what can one do for an ignored test, commenting it or mark it as `@Ignore`?

According to different case.

##### T5: Test [Boundary Conditions](https://resources.system-analysis.cadence.com/blog/msa2022-what-are-boundary-conditions-in-simulations#:~:text=Boundary%20conditions%20allow%20a%20simulation%20design%20to%20constrain,terms%20proportional%20to%20the%20value%20of%20the%20solution.)

Be aware of boundary conditions.

A famous example as follows.

`T5_1.java` at Example Code of `<<Clean Code>>`[^2]

```
import java.io.*;

import java.util.ArrayList;

public void boundaryConditionWrongExample(){
	ArrayList<String> arrayList1 = new ArrayList<String>();

	arrayList1.add("Jay");
	arrayList1.add("Nico");
	arrayList1.add("Midori");
	arrayList1.add("Akari");

	System.out.println("Enter the index to find corresponding name.");
	
	BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));

	String inputString = reader.readLine();
	int index = Integer.parseInt(inputString);
	
	System.out.println(arrayList1.get(index)); // may throw Exception.

}

public void boundaryConditionCorrectExample(){
	ArrayList<String> arrayList1 = new ArrayList<String>();

	arrayList1.add("Jay");
	arrayList1.add("Nico");
	arrayList1.add("Midori");
	arrayList1.add("Akari");

	System.out.println("Enter the index to find corresponding name.");
	
	BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));

	String inputString = reader.readLine();
	int index = Integer.parseInt(inputString);
	
	if(0 <= index && index <= arrayList1.size()-1) {
		System.out.println(arrayList1.get(index)); // NEVER throw Exception.
	}
}
```

##### T6: Exhaustively Test Near Bugs

It is a wise way to do test for function or method once one finish writing it.

##### T7: Patterns of Failure Are Revealing
To know where the bugs is, one can either:
+ Look at error log.
+ Extra info[^1] Use debugger to debug step by step. 

##### T8: Test Coverage Pattern Can Be Revealing

Like `T7: Patterns of Failure Are Revealing` section said.

##### T9: Tests Should Be Fast.

It should take less time as possible to test it once since in developement of a big project, it may be tested more than one hundred times.

##### Extra info[^1] T10: Tests Should Be Simple As Possible
In an earlier section, we have discuss that 

> [!IMPORTANT]
> The test should be as simple as one can.
>
> The best case should be that click one button to run the tests (only possible in IDE).
>
> The worst case should be type two commands (one for compilation, another for running) to run the tests in terminal.

[^0]: O.S. means my opinion or something I want to say.

[^1]: Extra info means My extraneous info.

[^2]: [Example Code of `<<Clean Code>>`]()
