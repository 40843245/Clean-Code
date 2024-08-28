# `<<Clean Code>>`
## important principle
### CH17 - good practice of clean code
#### Names
##### N1: Choose Descriptive Name

Look at these code snippets.

`N1_1.java` at Example Code of `<<Clean Code>>`[^1].

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

`N1_2.java` at Example Code of `<<Clean Code>>`[^1].

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

which one is readable? Obviously, is `N1_2.java` at Example Code of `<<Clean Code>>`[^1], isn't it? 

(Though `N1_2.java` at Example Code of `<<Clean Code>>`[^1] is NOT more complete than `N1_1.java` at Example Code of `<<Clean Code>>`[^1].)

##### N2: Choose Names at the Appropriate Level of Abstraction

Again, look at these code snippets.

`N2_1.java` at Example Code of `<<Clean Code>>`[^1]

```
public interface Modem {
	boolean dial(String phoneNumber);
	boolean disconnect();
	boolean send(char c);
	char recv();
	String getConnectedPhoneNumber();
  }
```

`N2_2.java` at Example Code of `<<Clean Code>>`[^1]

```
public interface Modem {
	boolean connect(String connectionLocator);
	boolean disconnect();
	boolean send(char c);
	char recv();
	String getConnectedLocator();
  }
```

which is more extensible? Though they have some extensibility, in `N2_2.java` at Example Code of `<<Clean Code>>`[^1], the name of property looks so descriptive in different cases (***not only*** about phone numbers, ***but also*** about all connectors). And

in `N2_2.java` at Example Code of `<<Clean Code>>`[^1], he name of property looks so descriptive ***only*** in cases about phone numbers. Thus, it is better way to name these property like in `N2_2.java` at Example Code of `<<Clean Code>>`[^1].

##### N3: Use Standard Nomenclature Where Possible

Such as the case.

Case 1:

If you are defining a function of decorator, it is recommend to name the function name as `xxxDecorator`.

Case 2:

If you are defining a function that converts it to string, it is recommend that name the function name as `toString()`.

Case 3:

If you are defining a method of a class that converts it to string, it is recommend that name the method name as `toString()`.

##### N4: Unambiguous Names

`N4_1.java` at Example Code of `<<Clean Code>>`[^1]

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

`N5_1.java` at Example Code of `<<Clean Code>>`[^1] 

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

`N7_1.java` at Example Code of `<<Clean Code>>`[^1]

```
public ObjectOutputStream getOos() throws IOException {
     if (m_oos == null) {
       m_oos = new ObjectOutputStream(m_socket.getOutputStream());
     }
     return m_oos;
   }
```

`N7_2.java` at Example Code of `<<Clean Code>>`[^1] looks better since one can more easily to known what actually the method do. On the other hand, I rename `m_oos` to `myOss` according `N6: Avoid Encodings` section.

`N7_2.java` at Example Code of `<<Clean Code>>`[^1]

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

`T5_1.java` at Example Code of `<<Clean Code>>`[^1]

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
+ Extra info[^0]: Use debugger to debug step by step. 

##### T8: Test Coverage Pattern Can Be Revealing

Like `T7: Patterns of Failure Are Revealing` section said.

##### T9: Tests Should Be Fast.

It should take less time as possible to test it once since in developement of a big project, it may be tested more than one hundred times.

##### Extra info[^0]: T10: Tests Should Be simple


[^0]: My extraneous info.

[^1]: [Example Code of `<<Clean Code>>`]()
