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
    var emptyList = new List<int>();
    var lastOrDefaultElement = emptyList.LastOrDefault();
    lastOrDefaultElement.Dump();  // Output: 0 (default value for int)
    ```

- **ElementAt** (immediate execution)
  Returns the element at a specified index in a sequence.
    ```csharp
    var numbers = new List<int> { 1, 2, 3, 4, 5 };
    var elementAtIndex = numbers.ElementAt(2);
    elementAtIndex.Dump();  // Output: 3
    ```

- **ElementAtOrDefault** (immediate execution)
  Returns the element at a specified index in a sequence, or a default value if the index is out of range.
    ```csharp
    var numbers = new List<int> { 1, 2, 3, 4, 5 };
    var elementAtOrDefaultIndex = numbers.ElementAtOrDefault(10);
    elementAtOrDefaultIndex.Dump();  // Output: 0 (default value for int)
    ```

- **DefaultIfEmpty** (immediate execution)
  Returns the elements of the specified sequence or the type parameter's default value in a singleton collection if the sequence is empty.
    ```csharp
    var emptyList = new List<int>();
    var defaultIfEmpty = emptyList.DefaultIfEmpty();
    defaultIfEmpty.Dump();  // Output: 0 (default value for int)
    ```

## Conversion Methods
- **ToArray** (immediate execution)
  Creates an array from a `IEnumerable<T>`.
    ```csharp
    var numbers = new List<int> { 1, 2, 3, 4, 5 };
    var array = numbers.ToArray();
    array.Dump();  // Output: [1, 2, 3, 4, 5]
    ```

- **ToList** (immediate execution)
  Creates a `List<T>` from an `IEnumerable<T>`.
    ```csharp
    var numbers = new List<int> { 1, 2, 3, 4, 5 };
    var list = numbers.ToList();
    list.Dump();  // Output: List<int> { 1, 2, 3, 4, 5 }
    ```

- **ToDictionary** (immediate execution)
  Creates a `Dictionary<TKey,TValue>` from an `IEnumerable<T>` according to specified key selector and element selector functions.
    ```csharp
    var people = new List<Person>
    {
        new Person { Id = 1, Name = "John" },
        new Person { Id = 2, Name = "Jane" },
        new Person { Id = 3, Name = "Jake" }
    };
    var dictionary = people.ToDictionary(p => p.Id, p => p.Name);
    dictionary.Dump();  
    // Output: 
    // {
    //   { 1, "John" },
    //   { 2, "Jane" },
    //   { 3, "Jake" }
    // }
    ```

- **ToHashSet** (immediate execution)
  Creates a `HashSet<T>` from an `IEnumerable<T>`.
    ```csharp
    var numbers = new List<int> { 1, 2, 3, 2, 1, 4 };
    var hashSet = numbers.ToHashSet();
    hashSet.Dump();  // Output: HashSet<int> { 1, 2, 3, 4 }
    ```

- **ToLookup** (immediate execution)
  Creates a `Lookup<TKey, TElement>` from an `IEnumerable<T>` according to specified key selector and element selector functions.
    ```csharp
    var people = new List<Person>
    {
        new Person { Id = 1, Name = "John", City = "New York" },
        new Person { Id = 2, Name = "Jane", City = "London" },
        new Person { Id = 3, Name = "Jake", City = "New York" }
    };
    var lookup = people.ToLookup(p => p.City, p => p.Name);
    lookup.Dump();
    // Output:
    // { "New York" => ["John", "Jake"],
    //   "London" => ["Jane"] }
    ```

## Generation Methods
- **AsEnumerable** (deferred execution)
  Returns the input typed as `IEnumerable<T>`.
    ```csharp
    var numbers = new List<int> { 1, 2, 3, 4, 5 };
    var result = numbers.AsEnumerable();
    result.Dump();  // Output: 1, 2, 3, 4, 5
    ```

- **AsQueryable** (deferred execution)
  Converts an `IEnumerable` to an `IQueryable`.
    ```csharp
    var numbers = new List<int> { 1, 2, 3, 4, 5 };
    var result = numbers.AsQueryable();
    result.Dump();  // Output: 1, 2, 3, 4, 5
    ```

- **Range** (immediate execution)
  Generates a sequence of integral numbers within a specified range.
    ```csharp
    var result = Enumerable.Range(1, 5);
    result.Dump();  // Output: 1, 2, 3, 4, 5
    ```

- **Repeat** (immediate execution)
  Generates a sequence that contains one repeated value.
    ```csharp
    var result = Enumerable.Repeat("Hello", 3);
    result.Dump();  // Output: "Hello", "Hello", "Hello"
    ```

- **Empty** (immediate execution)
  Returns an empty `IEnumerable<T>` that has the specified type argument.
    ```csharp
    var result = Enumerable.Empty<int>();
    result.Dump();  // Output: <empty sequence>
    ```

## Set Operations
- **Distinct** (deferred execution)
  Returns distinct elements from a sequence by using the default equality comparer to compare values.
    ```csharp
    var numbers = new List<int> { 1, 2, 2, 3, 4, 4, 5 };
    var result = numbers.Distinct();
    result.Dump();  // Output: 1, 2, 3, 4, 5
    ```

- **DistinctBy** (deferred execution)
  Returns distinct elements from a sequence according to a specified key selector function.
    ```csharp
    var people = new List<Person>
    {
        new Person { Name = "John", Age = 30 },
        new Person { Name = "Jane", Age = 25 },
        new Person { Name = "John", Age = 35 }
    };
    var result = people.DistinctBy(p => p.Name);
    result.Dump();  // Output: John (30), Jane (25)
    ```

- **Union** (deferred execution)
  Produces the set union of two sequences by using the default equality comparer.
    ```csharp
    var numbers1 = new List<int> { 1, 2, 3 };
    var numbers2 = new List<int> { 3, 4, 5 };
    var result = numbers1.Union(numbers2);
    result.Dump();  // Output: 1, 2, 3, 4, 5
    ```

- **Intersect** (deferred execution)
  Produces the set intersection of two sequences by using the default equality comparer to compare values.
    ```csharp
    var numbers1 = new List<int> { 1, 2, 3 };
    var numbers2 = new List<int> { 2, 3, 4 };
    var result = numbers1.Intersect(numbers2);
    result.Dump();  // Output: 2, 3
    ```

- **Except** (deferred execution)
  Produces the set difference of two sequences by using the default equality comparer to compare values.
    ```csharp
    var numbers1 = new List<int> { 1, 2, 3 };
    var numbers2 = new List<int> { 2, 3, 4 };
    var result = numbers1.Except(numbers2);
    result.Dump();  // Output: 1
    ```

- **ExceptBy** (deferred execution)
  Produces the set difference of two sequences according to a specified key selector function.
    ```csharp
    var people1 = new List<Person>
    {
        new Person { Name = "John", Age = 30 },
        new Person { Name = "Jane", Age = 25 }
    };
    var people2 = new List<Person>
    {
        new Person { Name = "Jane", Age = 30 },
        new Person { Name = "Sam", Age = 35 }
    };
    var result = people1.ExceptBy(people2, p => p.Name);
    result.Dump();  // Output: John (30)
    ```

- **IntersectBy** (deferred execution)
  Produces the set intersection of two sequences according to a specified key selector function.
    ```csharp
    var people1 = new List<Person>
    {
        new Person { Name = "John", Age = 30 },
        new Person { Name = "Jane", Age = 25 }
    };
    var people2 = new List<Person>
    {
        new Person { Name = "Jane", Age = 30 },
        new Person { Name = "Sam", Age = 35 }
    };
    var result = people1.IntersectBy(people2, p => p.Name);
    result.Dump();  // Output: Jane (25)
    ```

- **SequenceEqual** (immediate execution)
  Determines whether two sequences are equal by comparing the elements by using the default equality comparer for their type.
    ```csharp
    var numbers1 = new List<int> { 1, 2, 3 };
    var numbers2 = new List<int> { 1, 2, 3 };
    bool areEqual = numbers1.SequenceEqual(numbers2);
    areEqual.Dump();  // Output: True
    ```

## Joining and Grouping
- **Zip** (deferred execution)
  Applies a specified function to the corresponding elements of two sequences, producing a sequence of the results.
    ```csharp
    var numbers = new List<int> { 1, 2, 3 };
    var words = new List<string> { "one", "two", "three" };
    var zipped = numbers.Zip(words, (n, w) => $"{n}-{w}");
    zipped.Dump();  // Output: "1-one", "2-two", "3-three"
    ```

- **Join** (deferred execution)
  Correlates the elements of two sequences based on matching keys.
    ```csharp
    var people = new List<Person>
    {
        new Person { Id = 1, Name = "John" },
        new Person { Id = 2, Name = "Jane" },
        new Person { Id = 3, Name = "Jake" }
    };

    var pets = new List<Pet>
    {
        new Pet { Name = "Fluffy", OwnerId = 1 },
        new Pet { Name = "Milo", OwnerId = 2 }
    };

    var joinResult = people.Join(
        pets,
        person => person.Id,
        pet => pet.OwnerId,
        (person, pet) => new { person.Name, pet.Name });

    joinResult.Dump();  // Output: { Name = "John", Name = "Fluffy" }, { Name = "Jane", Name = "Milo" }
    ```

- **GroupJoin** (deferred execution)
  Correlates the elements of two sequences based on key equality, and groups the results.
    ```csharp
    var groupJoinResult = people.GroupJoin(
        pets,
        person => person.Id,
        pet => pet.OwnerId,
        (person, pets) => new { person.Name, Pets = pets.Select(pet => pet.Name).ToList() });

    groupJoinResult.Dump();  // Output: { Name = "John", Pets = ["Fluffy"] }, { Name = "Jane", Pets = ["Milo"] }, { Name = "Jake", Pets = [] }
    ```

- **Concat** (deferred execution)
  Concatenates two sequences.
    ```csharp
    var numbers1 = new List<int> { 1, 2, 3 };
    var numbers2 = new List<int> { 4, 5, 6 };
    var concatenated = numbers1.Concat(numbers2);
    concatenated.Dump();  // Output: 1, 2, 3, 4, 5, 6
    ```

- **GroupBy** (deferred execution)
  Groups the elements of a sequence according to a specified key selector function.
    ```csharp
    var words = new List<string> { "apple", "banana", "apricot", "blueberry" };
    var grouped = words.GroupBy(word => word[0]);
    grouped.Dump();  // Output: { Key = 'a', Values = ["apple", "apricot"] }, { Key = 'b', Values = ["banana", "blueberry"] }
    ```

## Sorting
- **OrderBy** (deferred execution)
  Sorts the elements of a sequence in ascending order according to a key.
    ```csharp
    var numbers = new List<int> { 5, 2, 8, 3, 1 };
    var result = numbers.OrderBy(n => n);
    result.Dump();  // Output: 1, 2, 3, 5, 8
    ```

- **OrderByDescending** (deferred execution)
  Sorts the elements of a sequence in descending order according to a key.
    ```csharp
    var numbers = new List<int> { 5, 2, 8, 3, 1 };
    var result = numbers.OrderByDescending(n => n);
    result.Dump();  // Output: 8, 5, 3, 2, 1
    ```

- **ThenBy** (deferred execution)
  Performs a subsequent ordering of the elements in a sequence in ascending order according to a key.
    ```csharp
    var people = new List<Person>
    {
        new Person { FirstName = "John", LastName = "Doe" },
        new Person { FirstName = "Jane", LastName = "Smith" },
        new Person { FirstName = "John", LastName = "Smith" },
    };

    var result = people.OrderBy(p => p.LastName).ThenBy(p => p.FirstName);
    result.Dump();
    // Output: John Doe, Jane Smith, John Smith
    ```

- **ThenByDescending** (deferred execution)
  Performs a subsequent ordering of the elements in a sequence in descending order according to a key.
    ```csharp
    var people = new List<Person>
    {
        new Person { FirstName = "John", LastName = "Doe" },
        new Person { FirstName = "Jane", LastName = "Smith" },
        new Person { FirstName = "John", LastName = "Smith" },
    };

    var result = people.OrderBy(p => p.LastName).ThenByDescending(p => p.FirstName);
    result.Dump();
    // Output: John Doe, John Smith, Jane Smith
    ```

- **Reverse** (deferred execution)
  Inverts the order of the elements in a sequence.
    ```csharp
    var numbers = new List<int> { 1, 2, 3, 4, 5 };
    var result = numbers.Reverse();
    result.Dump();  // Output: 5, 4, 3, 2, 1
    ```

- **Order** (deferred execution)
  Sorts the elements of a sequence in ascending order. Equivalent to `OrderBy`.
    ```csharp
    var numbers = new List<int> { 5, 2, 8, 3, 1 };
    var result = numbers.Order();
    result.Dump();  // Output: 1, 2, 3, 5, 8
    ```

- **OrderDescending** (deferred execution)
  Sorts the elements of a sequence in descending order. Equivalent to `OrderByDescending`.
    ```csharp
    var numbers = new List<int> { 5, 2, 8, 3, 1 };
    var result = numbers.OrderDescending();
    result.Dump();  // Output: 8, 5, 3, 2, 1
    ```

## Parallel LINQ (PLINQ)
- **AsParallel** (deferred execution)
  The `AsParallel` method enables parallelization of a query.
    ```csharp
    var numbers = Enumerable.Range(1, 10);
    var parallelQuery = numbers.AsParallel().Select(n => n * 2);
    parallelQuery.Dump();  // Output (order might vary): 2, 4, 6, 8, 10, 12, 14, 16, 18, 20
    ```

- **WithDegreeOfParallelism**
  The `WithDegreeOfParallelism` method sets the degree of parallelism, i.e., the number of concurrent tasks to be used for the query.
    ```csharp
    var numbers = Enumerable.Range(1, 10);
    var parallelQuery = numbers.AsParallel().WithDegreeOfParallelism(4).Select(n => n * 2);
    parallelQuery.Dump();  // Output (order might vary): 2, 4, 6, 8, 10, 12, 14, 16, 18, 20
    ```

- **WithExecutionMode**
  The `WithExecutionMode` method sets the execution mode of the parallel query, which can force parallelism.
    ```csharp
    var numbers = Enumerable.Range(1, 10);
    var parallelQuery = numbers.AsParallel()
                            .WithExecutionMode(ParallelExecutionMode.ForceParallelism)
                            .Select(n => n * 2);
    parallelQuery.Dump();  // Output (order might vary): 2, 4, 6, 8, 10, 12, 14, 16, 18, 20
    ```

- **WithMergeOptions**
  The `WithMergeOptions` method sets the merge options for the parallel query, affecting how the results are buffered and output.
    ```csharp
    var numbers = Enumerable.Range(1, 10);
    var parallelQuery = numbers.AsParallel()
                            .WithMergeOptions(ParallelMergeOptions.NotBuffered)
                            .Select(n => n * 2);
    parallelQuery.Dump();  // Output (order might vary): 2, 4, 6, 8, 10, 12, 14, 16, 18, 20 
    ```

- **AsOrdered**
  The `AsOrdered` method preserves the ordering of the elements in a parallel query.
    ```csharp
    var numbers = Enumerable.Range(1, 10);
    var parallelQuery = numbers.AsParallel().AsOrdered().Select(n => n * 2);
    parallelQuery.Dump();  // Output: 2, 4, 6, 8, 10, 12, 14, 16, 18, 20
    ```