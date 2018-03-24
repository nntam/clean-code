# Clean Code
Notes on the book: ["Clean Code - A Handbook of Agile Software Craftsmanship"](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) written by Robert C. Martin

# Contents

[Foreword](#foreword)

[Introduction](#introduction)

1. [Clean Code](#chapter-1---clean-code)
2. [Meaningful Names](#chapter-2---meaningful-names)
3. [Functions](#chapter-3---functions)
4. [Comments](#chapter-4---comments)
5. [Formatting](#chapter-5---formatting) 
6. [Objects and Data Structures](#chapter-6---objects-and-data-structures) 
7. [Error Handling](#chapter-7---error-handling) 
8. [Boundaries](#chapter-8---boundaries) 
9. [Unit Tests](#chapter-9---unit-tests)
10. [Classes](#chapter-10---classes) 
11. [Systems](#chapter-11---systems) 
12. [Emergence](#chapter-12---emergence)
13. [Concurrency](#chapter-13---concurrency)
14. [Successive Refinement](#chapter-14---successive-refinement) 
15. [JUnit Internals](#chapter-15---junit-internals) 
16. [Refactoring SerialDate](#chapter-16---refactoring-serialdate) 
17. [Smells and Heuristics](#chapter-17---smells-and-heuristics)

## Foreword
In software, **80%** or more of what we do is quaintly called *"maintenance"*: the act of repair.

Good software practice requires such discipline: *focus, presence of mind, and thinking*

The 5S philosophy comprises these concepts (Total Productive Maintenance - 1951):
* **S**eiri (organization): Knowing where things are—using approaches such as suitable naming—is crucial
* **S**eiton (tidiness): There is an old American saying: *A place for everything, and everything in its place*. A piece of code should be where you expect to find it - and, if not, you should re-factor to get it there.
* **S**eiso (cleaning): Remove unused things (comments, etc). Get rid of them
* **S**eiketsu (standardization): The group agrees about how to keep the workplace clean.
* **S**hutsuke (self-discipline): Follow the practices, frequently reflect on one's work and be willing to change

You should name a variable using the same care with which you name a first-born child.

"The code is the design" and "Simple code" are their mantras.

We are honest about the state of our code because code is never perfect.

## Introduction

Learning to write clean code is *hard work*. It requires more than just the knowledge of principles and patterns. 

You must practice it yourself, and watch yourself fail.

This book will make you work, *and work hard*. What kind of work will you be doing?

You'll be reading code, lots of code. And you will be challenged to think about what's right about that code and what's wrong with it.

This book into three parts:
* First part: The first several chapters describe the principles, patterns, and practices of writing clean code.
* Second part: It consists of several case studies of ever-increasing complexity. Each case study is an exercise in cleaning up some code of transforming code that has some problems into code that has fewer problems.
* Third part: It is a single chapter containing a list of heuristics and smells gathered while creating the case studies.

## Chapter 1 - Clean Code

### There will be code

*Programmers simply won't be needed because business people will generate programs from specifications*

=> Nonsense! We will never be rid of code, because code represents the details of the requirements

### Bad code

*It was the bad code that brought the company down.*

*Have you ever been significantly impeded by bad code?* If you are a programmer of any experience then you've felt this impediment many times.
 
So then, *why did you write it?* Were you trying to go fast? Were you in a rush?

We've all said we'd go back and clean it up later. **Later equals never.**

### Attitude

Why does this happen to code? Why does good code rot so quickly into bad code? What about the requirements? What about the schedule? What about the stupid managers and the useless marketing types? Don't they bear some of the blame? **No!**

Most managers want good code, even when they are obsessing about the schedule. They may defend the schedule and requirements with passion, but that's their job.

It's your job to defend the code with equal passion.

### The Primal Conundrum

The only way to make the deadline - * the only way to go fast * - is to keep the code as clean as possible at all times.

### The Art of Clean Code?

Let's say you believe that messy code is a significant impediment. 

*"How do I write clean code?"* It's no good trying to write clean code if you don't know what it means for code to be clean!

Clean code is a lot like painting a picture.

Able to recognize clean code from dirty code does **not mean that we know how to write clean code!**

Writing clean code requires using myriad little techniques. *This "code-sense" is the key.*

In short, a programmer who writes clean code is an artist.

### What is Clean Code?

Clean code is pleasing to read.

Clean code exhibits close attention to detail.

Clean code makes it easy for other people to enhance it.

There is a difference between *code that is easy to read* and *code that is easy to change*.

Code, without tests, is not clean.

Smaller is better. Keep it simple and orderly.

The beautiful code *makes the language look like it was made for the problem!*

### We are Authors

The ratio of time spent reading vs. writing is well over 10:1.

To write new code we need to read the old one.

## Chapter 2 - Meaningful Names

Names are everywhere in software. We name our variables, our functions, our arguments, classes, and packages...

So, we need to follow some simple rules for creating good names.

### Use Intention-Revealing Names

The name should answer all the big questions. It should tell you:
* Why it exists?
* What it does?
* How it is used?

The name *d* reveals nothing:
```java
int d; // elapsed time in days
```

We should choose a name that specifies what is being measured and the unit of that measurement:
```java
int elapsedTimeInDays;
int daysSinceCreation;
int daysSinceModification;
int fileAgeInDays;
```

Choosing names that reveal intent can make it much easier to understand and change code.

What is the purpose of this code?
```java
public List<int[]> getThem() {
	List<int[]> list1 = new ArrayList<int[]>();
	for (int[] x : theList)
		if (x[0] == 4)
			list1.add(x);
	return list1;
}
```

The code implicitly requires that we know the answers to questions such as:
* What kinds of things are in *theList*?
* What is the significance of the zeroth subscript of an item in *theList*?
* What is the significance of the value *4*?
* How would I use the list being returned?

We can improve the code:
```java
public List<int[]> getFlaggedCells() {
	List<int[]> flaggedCells = new ArrayList<int[]>();
	for (int[] cell : gameBoard)
		if (cell[STATUS_VALUE] == FLAGGED)
			flaggedCells.add(cell);
	return flaggedCells;
}
```

or

```java
// Class Cell instead of ints
// Function isFlagged to hide the magic numbers
public List<Cell> getFlaggedCells() {
	List<Cell> flaggedCells = new ArrayList<Cell>();
	for (Cell cell : gameBoard)
		if (cell.isFlagged())
			flaggedCells.add(cell);
	return flaggedCells;
}
```

With these simple name changes, it's not difficult to understand what's going on. This is the power of choosing good names.

### Avoid Disinformation

Do not refer to a *grouping of accounts* as an *accountList* unless it’s actually a *List*.

*accountGroup* or *bunchOfAccounts* or just plain accounts would be better.

Avoid use of lower-case **L** or uppercase **O** as variable names, especially in combination. They look almost entirely like the constants one and zero.
```java
int a = l;
if ( O == l )
	a = O1;
else
	l = 01;
```

### Make Meaningful Distinctions

Consider:
```java
public static void copyChars(char a1[], char a2[]) {
	for (int i = 0; i < a1.length; i++) {
		a2[i] = a1[i];
	}
}
```

This function reads much better when *source* and *destination* are used for the argumentnames.

Noise words are redundant, avoid use words: *info, data, a, an, the*

How are the programmers supposed to know which of these functions to call?
```java
getActiveAccount();
getActiveAccounts();
getActiveAccountInfo();
```

Distinguish names in such a way that the reader knows what the differences offer.
* *moneyAmount* is indistinguishable from *money*
* *customerInfo( is indistinguishable from *customer*
* *accountData* is indistinguishable from *account*
* *theMessage* is indistinguishable from *message*

### Use Pronounceable Names

If you can't pronounce it, you can't discuss it.

Compare
```java
class DtaRcrd102 {
	private Date genymdhms;
	private Date modymdhms;
	private final String pszqint = "102";
	/* ... */
};
```

to
```java
class Customer {
	private Date generationTimestamp;
	private Date modificationTimestamp;;
	private final String recordId = "102";
	/* ... */
};
```

### Use Searchable Names

If a variable or constant might be seen or used in multiple places in a body of code, it is imperative to give it a search-friendly name.

Compare
```java
for (int j=0; j<34; j++) {
	s += (t[j]*4)/5;
}
```

to
```java
int realDaysPerIdealDay = 4;
const int WORK_DAYS_PER_WEEK = 5;
int sum = 0;
for (int j=0; j < NUMBER_OF_TASKS; j++) {
	int realTaskDays = taskEstimate[j] * realDaysPerIdealDay;
	int realTaskWeeks = (realdays / WORK_DAYS_PER_WEEK);
	sum += realTaskWeeks;
}
```

### Avoid Encodings

Encoded names are seldom pronounceable and are easy to mis-type.

### Member Prefixes

You also don't need to prefix member variables with *m_* anymore.

Compare
```java
public class Part {
	private String m_dsc; // The textual description
	void setName(String name) {
		m_dsc = name;
	}
}
```

to
```java
public class Part {
	private String description;
	void setDescription(String description) {
		this.description = description;
	}
}
```

### Interfaces and Implementations

The interface *IShapeFactory* and implement *ShapeFactory* is bad code.
The interface *ShapeFactory* and implement *ShapeFactoryImp* is good code.

### Avoid Mental Mapping

One difference between a smart programmer and a professional programmer is that the professional understands that **clarity is king**. Professionals use their powers for good and write code that others can understand.

### Class Names

Classes and objects should have noun or noun phrase names like Customer, WikiPage, Account, and AddressParser. Avoid words like Manager, Processor, Data, or Info in the name of a class.

**A class name should not be a verb**

### Method Names

Methods should have verb or verb phrase names like postPayment, deletePage, or save.

Accessors, mutators, and predicates should be named for their value and prefixed with get, set, and is according to the javabean standard.

### Don't Be Cute

Don't use the name *whack()* to mean *kill()*

Say what you mean. Mean what you say.

### Pick One Word per Concept

A consistent lexicon is a great boon to the programmers who must use your code.

### Don't Pun
Avoid using the same word for two purposes. 

### Use Solution Domain Names

Remember that the people who read your code will be programmers. So go ahead and use computer science terms, algorithm names, pattern names, math terms, and so forth.

Choosing technical names for those things is usually the most appropriate course.

### Add Meaningful Context

You have variables named *firstName, lastName, street, houseNumber, city, state, and zipcode*. Taken together it's pretty clear that they form an address.

You can add context by using prefixes: *addrFirstName, addrLastName, addrState, and so on*. At least readers will understand that these variables are part of a larger structure. Of course, a better solution is to create a class named **Address**.

The algorithm to be made much cleaner by breaking it into many smaller functions.

### Don't Add Gratuitous Context

it is a bad idea to prefix every class: GSDAccountAddress (GSD = Gas Station Deluxe)

Shorter names are generally better than longer ones

### Final Words

Follow some of these rules and see whether you don’t improve the readability of your code.

If you are maintaining someone else’s code, use refactoring tools to help resolve these problems.

## Chapter 3 - Functions

### Small!

The first rule of functions is that they should be small. The second rule of functions is that they should be smaller than that.

Tunctions should be very small.

Functions should hardly ever be 20 lines long.

Each was transparently obvious. Each told a story. Each led you to the next in a compelling order. *That’s how short your functions should be!*

### Blocks and Indenting

This implies that the blocks within *if* statements, *else* statements, *while* statements, and so on should be one line long. Probably that line should be a function call.

The indent level of a function *should not be greater than one or two*. This, of course, makes the functions easier to read and understand.

### Do One Thing

**FUNCTIONS SHOULD DO ONE THING. THEY SHOULD DO IT WELL. THEY SHOULD DO IT ONLY**

we write functions is to decompose a larger concept (in other words, the name of the function) into a set of steps at the next level of abstraction.

### Sections within Functions

Functions do one thing cannot be reasonably divided into sections.

### One Level of Abstraction per Function

In order to make sure functions are doing *"one thing"*, you need to make sure that the statements within our function are all at the same level of abstraction.

Mixing levels of abstraction within a function is always confusing.

### Reading Code from Top to Bottom: The Stepdown Rule

Making the code read like a top-down set of TO paragraphs is an effective technique for keeping the abstraction level consistent.

### Switch Statements

The example shows just one of the operations that might depend on the type of employee:
```java
public Money calculatePay(Employee e) throws InvalidEmployeeType {
	switch (e.type) {
		case COMMISSIONED:
			return calculateCommissionedPay(e);
		case HOURLY:
			return calculateHourlyPay(e);
		case SALARIED:
			return calculateSalariedPay(e);
		default:
			throw new InvalidEmployeeType(e.type);
	}
}
```

There are several problems with this function. 
* it's large, and when new employee types are added, it will grow. 
* it very clearly does more than one thing.
* it violates the Single Responsibility Principle (SRP) because there is more than one reason for it to change.
* it violates the Open Closed Principle8 (OCP) because it must change whenever new types are added. 

## Chapter 4 - Comments
## Chapter 5 - Formatting
## Chapter 6 - Objects and Data Structures
## Chapter 7 - Error Handling
## Chapter 8 - Boundaries
## Chapter 9 - Unit Tests
## Chapter 10 - Classes
## Chapter 11 - Systems
## Chapter 12 - Emergence
## Chapter 13 - Concurrency
## Chapter 14 - Successive Refinement
## Chapter 15 - JUnit Internals
## Chapter 16 - Refactoring SerialDate
## Chapter 17 - Smells and Heuristics]
