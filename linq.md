# C# LINQ Methods Overview

This document provides an overview of various LINQ methods in C#, along with definitions and examples of their usage and execution behavior.

## Filtering
- **Where** (deferred execution)
  Filters a sequence of values based on a predicate.

    ```csharp
    var numbers = new List<int> { 1, 2, 3, 4, 5, 6 };
    var evenNumbers = numbers.Where(n => n % 2 == 0); // Filters out only even numbers

    evenNumbers.Dump("Even Numbers");
    ```

    In this example, Where is used to filter out all numbers that are not even. The predicate n % 2 == 0 checks if a number is divisible by 2. The Dump() method is used to display the contents of the evenNumbers collection, which will output:

    ```console
    Even Numbers:
    2
    4
    6
    ```
- **OfType** (deferred execution)
  Filters the elements of an `IEnumerable` based on a specified type.
    ```csharp
    var mixedList = new List<object> { 1, "two", 3, "four", 5 };
    var strings = mixedList.OfType<string>(); // Filters out only the strings

    strings.Dump("Strings");
    ```
    In this example, OfType is used to filter out all elements that are not of type string. This is useful when working with collections containing multiple types of objects. The Dump() method is used to display the contents of the strings collection, which will output:

    ```console
    Strings:
    "two"
    "four"
    ```

    
## Partitioning
- **Skip** (deferred execution)
  Bypasses a specified number of elements in a sequence and then returns the remaining elements.
    ```csharp
    var numbers = Enumerable.Range(1, 10); // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    var result = numbers.Skip(3); // Skips the first 3 elements
    result.Dump(); // Output: [4, 5, 6, 7, 8, 9, 10]
    ```
    

- **Take** (deferred execution)
  Returns a specified number of contiguous elements from the start of a sequence.
    ```csharp
    var numbers = Enumerable.Range(1, 10); // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    var result = numbers.Take(4); // Takes the first 4 elements
    result.Dump(); // Output: [1, 2, 3, 4]
    ```

- **SkipLast** (deferred execution)
  Returns a new enumerable collection that contains the elements from source with the last count elements of the source collection omitted.
    ```csharp
    var numbers = Enumerable.Range(1, 10); // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    var result = numbers.SkipLast(2); // Skips the last 2 elements
    result.Dump(); // Output: [1, 2, 3, 4, 5, 6, 7, 8]
    ```

- **TakeLast** (deferred execution)
  Returns a new enumerable collection that contains the last count elements from source.
    ```csharp
    var numbers = Enumerable.Range(1, 10); // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    var result = numbers.TakeLast(3); // Takes the last 3 elements
    result.Dump(); // Output: [8, 9, 10]
    ```

- **SkipWhile** (deferred execution)
  Bypasses elements in a sequence as long as a specified condition is true and then returns the remaining elements.
    ```csharp
    var numbers = Enumerable.Range(1, 10); // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    var result = numbers.SkipWhile(n => n < 5); // Skips elements while they are less than 5
    result.Dump(); // Output: [5, 6, 7, 8, 9, 10]
    ```

- **TakeWhile** (deferred execution)
  Returns elements from a sequence as long as a specified condition is true and then skips the remaining elements.
    ```csharp
    var numbers = Enumerable.Range(1, 10); // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    var result = numbers.TakeWhile(n => n < 6); // Takes elements while they are less than 6
    result.Dump(); // Output: [1, 2, 3, 4, 5]
    ```

## Projection
- **Select** (deferred execution)
  Projects each element of a sequence into a new form. Can pass the index of the element as a parameter.
    ```csharp
    var numbers = new List<int> { 1, 2, 3, 4, 5 };

    // Simple projection
    var squares = numbers.Select(n => n * n);
    squares.Dump();  // Output: 1, 4, 9, 16, 25

    // Projection with index
    var indexedSquares = numbers.Select((n, index) => new { Number = n, Index = index });
    indexedSquares.Dump();  
    /* 
    Output: 
    { Number = 1, Index = 0 }
    { Number = 2, Index = 1 }
    { Number = 3, Index = 2 }
    { Number = 4, Index = 3 }
    { Number = 5, Index = 4 }
    */
    ```

- **SelectMany** (deferred execution)
  Projects each element of a sequence to an `IEnumerable<T>` and flattens the resulting sequences into one sequence. Can pass the index of the element as a parameter.
    ```csharp
    var people = new List<Person>
    {
        new Person { Name = "Alice", Pets = new List<string> { "Dog", "Cat" } },
        new Person { Name = "Bob", Pets = new List<string> { "Bird" } }
    };

    // Simple projection
    var pets = people.SelectMany(person => person.Pets);
    pets.Dump();  // Output: "Dog", "Cat", "Bird"

    // Projection with index
    var indexedPets = people.SelectMany((person, index) => person.Pets.Select(pet => new { PersonIndex = index, Pet = pet }));
    indexedPets.Dump();  
    /* 
    Output: 
    { PersonIndex = 0, Pet = "Dog" }
    { PersonIndex = 0, Pet = "Cat" }
    { PersonIndex = 1, Pet = "Bird" }
    */
    ```

- **Cast** (deferred execution)
  Casts the elements of an `IEnumerable` to the specified type.
    ```csharp
    var mixedList = new ArrayList { 1, "two", 3.0, "four" };
    var strings = mixedList.Cast<string>();
    strings.Dump();  // Output: InvalidCastException (since not all elements can be cast to string)
    ```


- **Chunk** (deferred execution)
  Splits the elements of a sequence into chunks of a specified size.
    ```csharp
    var numbers = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
    var chunks = numbers.Chunk(3);
    chunks.Dump();  
    /* 
    Output: 
    [1, 2, 3]
    [4, 5, 6]
    [7, 8, 9]
    */
    ```

## Existence or Quantity Checks
- **Any** (immediate execution)
  Determines whether any element of a sequence satisfies a condition.
    ```csharp
    var numbers = new List<int> { 1, 2, 3, 4, 5 };
    bool hasEvenNumbers = numbers.Any(n => n % 2 == 0);
    hasEvenNumbers.Dump();  // Output: true
    ```

- **All** (immediate execution)
  Determines whether all elements of a sequence satisfy a condition.
    ```csharp
    var numbers = new List<int> { 1, 2, 3, 4, 5 };
    bool allPositive = numbers.All(n => n > 0);
    allPositive.Dump();  // Output: true
    ```


- **Contains** (immediate execution)
  Determines whether a sequence contains a specified element.
    ```csharp
    var numbers = new List<int> { 1, 2, 3, 4, 5 };
    bool containsThree = numbers.Contains(3);
    containsThree.Dump();  // Output: true
    ```

## Sequence Manipulation
- **Append** (deferred execution)
  Appends a value to the end of the sequence.
    ```csharp
    var numbers = new List<int> { 1, 2, 3 };
    var result = numbers.Append(4);
    result.Dump();  // Output: 1, 2, 3, 4
    ```

- **Prepend** (deferred execution)
  Adds a value to the beginning of the sequence.
    ```csharp
    var numbers = new List<int> { 2, 3, 4 };
    var result = numbers.Prepend(1);
    result.Dump();  // Output: 1, 2, 3, 4
    ```

## Aggregation Methods
- **Count** (immediate execution)
  Returns the number of elements in a sequence.
    ```csharp
    var numbers = new List<int> { 1, 2, 3, 4, 5 };
    var count = numbers.Count();
    count.Dump();  // Output: 5
    ```

- **TryGetNonEnumeratedCount** (immediate execution)
  Attempts to determine the count of elements in a sequence without forcing enumeration.
    ```csharp
    var numbers = new List<int> { 1, 2, 3, 4, 5 };
    if (numbers.TryGetNonEnumeratedCount(out int count))
    {
        count.Dump();  // Output: 5
    }
    ```

- **Max** (immediate execution)
  Returns the maximum value in a sequence.
    ```csharp
    var numbers = new List<int> { 1, 2, 3, 4, 5 };
    var max = numbers.Max();
    max.Dump();  // Output: 5
    ```

- **MaxBy** (immediate execution)
  Returns the maximum element of the sequence, according to a specified key selector function.
    ```csharp
    var people = new List<Person>
    {
        new Person { Name = "Alice", Age = 30 },
        new Person { Name = "Bob", Age = 40 },
        new Person { Name = "Charlie", Age = 25 }
    };
    var maxByAge = people.MaxBy(p => p.Age);
    maxByAge.Dump();  // Output: Person { Name = "Bob", Age = 40 }
    ```

- **Min** (immediate execution)
  Returns the minimum value in a sequence.
    ```csharp
    var numbers = new List<int> { 1, 2, 3, 4, 5 };
    var min = numbers.Min();
    min.Dump();  // Output: 1
    ```

- **MinBy** (immediate execution)
  Returns the minimum element of the sequence, according to a specified key selector function.
    ```csharp
    var people = new List<Person>
    {
        new Person { Name = "Alice", Age = 30 },
        new Person { Name = "Bob", Age = 40 },
        new Person { Name = "Charlie", Age = 25 }
    };
    var minByAge = people.MinBy(p => p.Age);
    minByAge.Dump();  // Output: Person { Name = "Charlie", Age = 25 }
    ```

- **Average** (immediate execution)
  Computes the average of a sequence of numeric values.
    ```csharp
    var numbers = new List<int> { 1, 2, 3, 4, 5 };
    var average = numbers.Average();
    average.Dump();  // Output: 3
    ```

- **Aggregate** (immediate execution)
  Applies an accumulator function over a sequence.
    ```csharp
    var numbers = new List<int> { 1, 2, 3, 4, 5 };
    var sum = numbers.Aggregate((total, n) => total + n);
    sum.Dump();  // Output: 15
    ```

- **LongCount** (immediate execution)
  Returns an `Int64` that represents the total number of elements in a sequence.
    ```csharp
    var numbers = new List<int> { 1, 2, 3, 4, 5 };
    var longCount = numbers.LongCount();
    longCount.Dump();  // Output: 5
    ```

## Element Operators
- **First** (immediate execution)
  Returns the first element in a sequence.
    ```csharp
    var numbers = new List<int> { 1, 2, 3, 4, 5 };
    var firstElement = numbers.First();
    firstElement.Dump();  // Output: 1
    ```

- **FirstOrDefault** (immediate execution)
  Returns the first element in a sequence, or a default value if the sequence contains no elements.
    ```csharp
    var emptyList = new List<int>();
    var firstOrDefaultElement = emptyList.FirstOrDefault();
    firstOrDefaultElement.Dump();  // Output: 0 (default value for int)
    ```

- **Single** (immediate execution)
  Returns the only element of a sequence, and throws an exception if there is not exactly one element in the sequence.
    ```csharp
    var singleElementList = new List<int> { 42 };
    var singleElement = singleElementList.Single();
    singleElement.Dump();  // Output: 42
    ```

- **SingleOrDefault** (immediate execution)
  Returns the only element of a sequence, or a default value if the sequence is empty; this method throws an exception if there is more than one element in the sequence.
    ```csharp
    var emptyList = new List<int>();
    var singleOrDefaultElement = emptyList.SingleOrDefault();
    singleOrDefaultElement.Dump();  // Output: 0 (default value for int)
    ```

- **Last** (immediate execution)
  Returns the last element of a sequence.
    ```csharp
    var numbers = new List<int> { 1, 2, 3, 4, 5 };
    var lastElement = numbers.Last();
    lastElement.Dump();  // Output: 5
    ```

- **LastOrDefault** (immediate execution)
  Returns the last element of a sequence, or a default value if the sequence contains no elements.
    ```csharp
        
    ```

    ```console

    ```

- **ElementAt** (immediate execution)
  Returns the element at a specified index in a sequence.
    ```csharp
        
    ```

    ```console

    ```

- **ElementAtOrDefault** (immediate execution)
  Returns the element at a specified index in a sequence, or a default value if the index is out of range.
    ```csharp
        
    ```

    ```console

    ```

- **DefaultIfEmpty** (immediate execution)
  Returns the elements of the specified sequence or the type parameter's default value in a singleton collection if the sequence is empty.
    ```csharp
        
    ```

    ```console

    ```

## Conversion Methods
- **ToArray** (immediate execution)
  Creates an array from a `IEnumerable<T>`.
    ```csharp
        
    ```

    ```console

    ```

- **ToList** (immediate execution)
  Creates a `List<T>` from an `IEnumerable<T>`.
    ```csharp
        
    ```

    ```console

    ```

- **ToDictionary** (immediate execution)
  Creates a `Dictionary<TKey,TValue>` from an `IEnumerable<T>` according to specified key selector and element selector functions.
    ```csharp
        
    ```

    ```console

    ```

- **ToHashSet** (immediate execution)
  Creates a `HashSet<T>` from an `IEnumerable<T>`.
    ```csharp
        
    ```

    ```console

    ```

- **ToLookup** (immediate execution)
  Creates a `Lookup<TKey, TElement>` from an `IEnumerable<T>` according to specified key selector and element selector functions.
    ```csharp
        
    ```

    ```console

    ```

## Generation Methods
- **AsEnumerable** (deferred execution)
  Returns the input typed as `IEnumerable<T>`.
    ```csharp
        
    ```

    ```console

    ```

- **AsQueryable** (deferred execution)
  Converts an `IEnumerable` to an `IQueryable`.
    ```csharp
        
    ```

    ```console

    ```

- **Range** (immediate execution)
  Generates a sequence of integral numbers within a specified range.
    ```csharp
        
    ```

    ```console

    ```

- **Repeat** (immediate execution)
  Generates a sequence that contains one repeated value.
    ```csharp
        
    ```

    ```console

    ```

- **Empty** (immediate execution)
  Returns an empty `IEnumerable<T>` that has the specified type argument.
    ```csharp
        
    ```

    ```console

    ```

## Set Operations
- **Distinct** (deferred execution)
  Returns distinct elements from a sequence by using the default equality comparer to compare values.
    ```csharp
        
    ```

    ```console

    ```

- **DistinctBy** (deferred execution)
  Returns distinct elements from a sequence according to a specified key selector function.
    ```csharp
    
    ```

    ```console

    ```

- **Union** (deferred execution)
  Produces the set union of two sequences by using the default equality comparer.
    ```csharp
        
    ```

    ```console

    ```

- **Intersect** (deferred execution)
  Produces the set intersection of two sequences by using the default equality comparer to compare values.
    ```csharp
        
    ```

    ```console

    ```

- **Except** (deferred execution)
  Produces the set difference of two sequences by using the default equality comparer to compare values.
    ```csharp
        
    ```

    ```console

    ```

- **ExceptBy** (deferred execution)
  Produces the set difference of two sequences according to a specified key selector function.
    ```csharp
        
    ```

    ```console

    ```

- **IntersectBy** (deferred execution)
  Produces the set intersection of two sequences according to a specified key selector function.
    ```csharp
        
    ```

    ```console

    ```

- **SequenceEqual** (immediate execution)
  Determines whether two sequences are equal by comparing the elements by using the default equality comparer for their type.
    ```csharp
        
    ```

    ```console

    ```

## Joining and Grouping
- **Zip** (deferred execution)
  Applies a specified function to the corresponding elements of two sequences, producing a sequence of the results.
    ```csharp
        
    ```

    ```console

    ```

- **Join** (deferred execution)
  Correlates the elements of two sequences based on matching keys.
    ```csharp
        
    ```

    ```console

    ```

- **GroupJoin** (deferred execution)
  Correlates the elements of two sequences based on key equality, and groups the results.
    ```csharp
        
    ```

    ```console

    ```

- **Concat** (deferred execution)
  Concatenates two sequences.
    ```csharp
        
    ```

    ```console

    ```

- **GroupBy** (deferred execution)
  Groups the elements of a sequence according to a specified key selector function.
    ```csharp
        
    ```

    ```console

    ```

## Sorting
- **OrderBy** (deferred execution)
  Sorts the elements of a sequence in ascending order according to a key.
    ```csharp
        
    ```

    ```console

    ```

- **OrderByDescending** (deferred execution)
  Sorts the elements of a sequence in descending order according to a key.
    ```csharp
        
    ```

    ```console

    ```

- **ThenBy** (deferred execution)
  Performs a subsequent ordering of the elements in a sequence in ascending order according to a key.
    ```csharp
        
    ```

    ```console

    ```

- **ThenByDescending** (deferred execution)
  Performs a subsequent ordering of the elements in a sequence in descending order according to a key.
    ```csharp
        
    ```

    ```console

    ```

- **Reverse** (deferred execution)
  Inverts the order of the elements in a sequence.
    ```csharp
        
    ```

    ```console

    ```

- **Order** (deferred execution)
  Sorts the elements of a sequence in ascending order. Equivalent to `OrderBy`.
    ```csharp
        
    ```

    ```console

    ```

- **OrderDescending** (deferred execution)
  Sorts the elements of a sequence in descending order. Equivalent to `OrderByDescending`.
    ```csharp

    ```

    ```console

    ```

## Parallel LINQ (PLINQ)
- **AsParallel** (deferred execution)
  Enables parallelization of a query.
    ```csharp
        
    ```

    ```console

    ```

- **WithDegreeOfParallelism**
  Sets the degree of parallelism for a query.
    ```csharp
        
    ```

    ```console

    ```

- **WithExecutionMode**
  Sets the execution mode of the parallel query.
    ```csharp
        
    ```

    ```console

    ```

- **WithMergeOptions**
  Sets the merge options for the parallel query.
    ```csharp
        
    ```

    ```console

    ```

- **AsOrdered**
  Preserves the order of the elements in a parallel query.
    ```csharp
        
    ```

    ```console

    ```
