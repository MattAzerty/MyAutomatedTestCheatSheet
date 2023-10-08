# LOCAL TESTING

## Java Virtual Machine
![JVM](https://github.com/MattAzerty/MyAutomatedTestCheatSheet/blob/main/docs/JVMTests/JVM.png)

Java was designed with the idea of "Write Once, Run Anywhere" (WORA) in mind. This means that once you write your source code in Java (in a .java/.kt file), you can compile it into bytecode (.class file). This bytecode can then be executed, or run, on any platform that has a Java Virtual Machine (for android, runtime are [ART or Dalvik](https://source.android.com/docs/core/runtime)[^1] with use of .dex files).

### ■ Classloader:

Classloaders are responsible for loading compiled Java classes into memory at runtime:

1. Class file are loaded into memory.
2. The classloader verifies the class file to make sure that it is valid.
3. Then links the class file to other classes that it depends on.
4. Allocates memory for the class's static variables.
5. Initializes the static variables.
6. Executes the static code.
   
Once the class's static variables and static code[^2] have been initialized, the class is ready to be used. This means that instances of the class can be created and used by the program.

### ■ Runtime Data:

Runtime Data is responsible for all program data such as:

  - **Method Area:** it's a shared data area where the class-level datas are stored, including static variables, class and interface definitions. The method area is created when the JVM starts up and is destroyed when the JVM exits.
  - **Heap Area:** allows global access and data stores (Objects, Arrays) available to all threads[^3] during the lifetime of the application. Since the Method and Heap areas share memory for multiple threads, it's not thread-safe (concurency issue(s) can happen, leading to flacky behavior).The heap is created when the JVM starts up and is destroyed when the JVM exits.
  - **Stack Area:** it's a thread-local data area that stores method call frames (data structure that contains information about a method, such as the method's parameters, local variables[^4], and return value). The stack is created when a thread is created and is destroyed when the thread exits.
  - **PC Registers:** contain the program counter, which is a pointer to the next instruction that the JVM will execute. Each thread has its own PC register.

### STACK VS HEAP:

```kotlin

fun main() {

  val num = 13 //Primitive types are stored on the stack not on the heap.
  val myObject = MyObject("myObjectName", 7)
  val nameObj = myObject.name
  val myString = "Hello world !"

}/* When the main() function finishes executing, all local variables are removed from the stack
 and any objects that were referenced by those variables become eligible for garbage collection.*/

data class MyObject(val name: String, val id: Int)

```

![STACKNHEAP](https://github.com/MattAzerty/MyAutomatedTestCheatSheet/blob/main/docs/JVMTests/StackHeap.png)

> [!WARNING]
> The stack memory is smaller compared to Heap memory and can throw the famous "StackOverflowError".
> The heap memory (RAM dependent) is the most critical area when it comes to memory leaks ("OutOfMemoryError").
> - Avoid sharing mutable objects between threads. If you must share a mutable object, make sure to protect it with synchronization.
> - Use immutable objects whenever possible. Immutable objects cannot be changed once they are created, so they are inherently thread-safe.
> - Be careful with concurrent modifications. If you are modifying a data structure while another thread is iterating over it, you may need to use synchronization to prevent data corruption.

### ■ Execution Engine:
...In progress
The area for executing our compiled and loaded code and cleaning up all the garbage that we’ve generated (Garbage Collector).

> [!WARNING]
> A memory leak can easily occur in Android when AsyncTasks, Handlers, Singletons, Threads, and other components are used incorrectly.

## JUnit



### Annotations
| JUnit 4 Annotation | JUnit 5 Annotation | Description |
|--------------------|--------------------|-------------|
| @After             | @AfterEach          | The annotated method will be executed after each @Test method in the current class. |
| @AfterClass        | @AfterAll           | The annotated (static) method will be executed once after all @Test methods in the current class. |
| @Before            | @BeforeEach         | The annotated method will be executed before each @Test method in the current class. |
| @BeforeClass       | @BeforeAll          | The annotated (static) method will be executed once before any @Test method in the current class. |
| @Ignore            | @Disabled           | The annotated method will not be executed (it will be skipped) but reported as such. |
| @Test              | @Test               | No change from JUnit4. The annotated method is a test method. |

## Assertk
... in progress
## Test doubles
... in progress
## Suspending function testing
... in progress

[^1]: Each process in Android has its own Dalvik or ART registered based Virtual Machine instance (with registers to store operands and results) while the JVM is a stack based virtual machine ('simple' LIFO). This mean difference can exist when tests are deployed on instrumented devices VS local machine.
[^2]: Static variables and static code can be used to implement a variety of features, such as: Class-level configuration, Singleton objects, Utility classes, ...
[^3]: A thread is a lightweight process that can run concurrently with other threads. Threads share the same memory space as the process they belong to, but they have their own stack and program counter. This allows threads to run independently of each other, while still sharing data and resources.
[^4]: Local variables are variables that are declared inside a method or block.

