---
title: What is new in C# 7.0!
author: Aruna Nishantha 
linkedin: https://lk.linkedin.com/in/anishantha87
---

C# 7.0 has been announced with tremendous changes to development environment with code simplification, data consumption and performance improvements. Unlike in older releases, with the latest features of C# 7.0 making your code clean, efficient and improves the productivity as well. In the following, I have elaborated the usage of new features.

### 1.	Out Variables
Before C# 7, if you had to pass a variable as out parameter, the variable needs to be declared before passing it to the method. Hence, in C# 7, now you can directly declare variable when you are passing it inside a method.
```cs
        if (DateTime.TryParse(datetimeToConvert, out var dt2))
        {
             WriteLine($"dt2: {dt2}");
         }
        WriteLine($"DayOfWeek is: {dt2.DayOfWeek}");
```
As you see in above screenshot, for C# 7 I am declaring an out variable inside method and it is being accessed from outside also.

### 2.	Tuples
Another richest new feature is tuples that make it easy to contain multiple results. Tuples concept for C#, was inspired by functional programming and it has its own benefits and unique features. In C# 6, that were created as a type of tuple, a structure. However, the major improvements have been released decoupling with C# in build library, so you have to install “System.ValueTuple” instead of the “System.Tuple” via nugget packages.
```cs
            var output2 = CalculateProductVal(20, 10);
            Console.WriteLine($"sum {output2.sum}, sub {output2.sub}, mul {output2.mul}");
            
            Console.WriteLine(output2.GetType());
            
            (int a, int b, int c) output3 = CalculateProductVal(20, 10);
            Console.WriteLine($"sum {output3.a}, sub {output3.b}, mul {output3.c}");
            
            var (sum1, sub1, mul1) = CalculateProductVal(20, 10);
            Console.WriteLine($"sum {sum1}, sub {sub1}, mul {mul1}");
            
            var person = (FirstName: "Aruna", LastName: "Perera", Gender: "Male");
            Console.WriteLine($"Person: {person}");
            
            var calculation = new Func<int, int, (int sum, int sub, int mul)>((a, b)
                                => (a + b, a - b, a * b));
            var result = calculation(20, 10);
            Console.WriteLine($"result: {result}");
```
```cs
            private (int sum, int sub, int mul) CalculateProductVal(int x, int y)
            {
                        return (x + y, x - y, x * y);
            }
```

### 3.	Patten Matching
Patterns are a new kind of language element in C#. We can do a lot more using switch statements unlike in C# older versions.
```cs
            switch (shape)
            {
                case Circle c:
                    Console.WriteLine($"circle with radius {c.Radius}");
                    break;
                case Rectangle s when (s.Width == s.Height):
                    Console.WriteLine($"{s.Width} x {s.Height} square");
                    break;
                case Rectangle rec when (rec.Width != rec.Height):
                    Console.WriteLine($"{rec.Width} x {rec.Height} rectangle");

                    int user = 10;
                    Console.WriteLine(nameof(user));
                    break;
                case null:
                    throw new ArgumentNullException(message: "Unable to find a matching type",
                                                        paramName: nameof(shape));
                default:
                    throw new InvalidOperationException("unknown item type");
            }
```

### 4.	Ref local and returns
Before C# 7 ref was only used to pass as a parameter in a method without any provision to 	return it and use later, but C# 7.0 has the ability to return by reference and to store references 	to local variables.
Ref local is a new variable type introduced in C# 7.0 to store references. This is mostly used to 	store the reference in a local variable in conjunction with Ref returns.

```cs
    public class RefLocalsDemo
    {
        public RefLocalsDemo()
        {
            int[] numbers = { 100,200,300,400,500};
            ref int refTo300 = ref Find(numbers, 300);

            Console.WriteLine(string.Join(",", numbers));
            refTo300 = 900;
            Console.WriteLine(string.Join(",",numbers));

            Find(numbers, 900) = 444_444;
            Console.WriteLine(string.Join(",", numbers));
        }
        public ref int Find(int[] numbers, int numberToFind)
        {
            for (int i = 0; i < numbers.Length; i++)
            {
                if (numbers[i]== numberToFind)
                {
                    return ref numbers[i];
                }
            }
            // return -1 ==> compile error
            //int ret = -1;
            //return ref ret; ==> Cannot return local 'ret'
            //by reference because it is not a ref local
            
            throw new ArgumentNullException(nameof(numberToFind));
        }
    }
```
### 5.	Local Functions

Local functions feature allows local methods to be defined and called within any method, the 	constructor of a class or inside a property. When this is compiled using a C# compiler, a local 	function gets transformed into a private method. This feature was not allowed in previous 	versions. 

```cs
    public class LocalFunctionDemo
    {
        private enum AgeCategory
        {
            Baby,
            Toddler,
            Preschooler,
            Gradeschooler
        }

        public LocalFunctionDemo(int studentAge)
        {
            var ageGroup = GetStudentMatchingAgeGroup(studentAge);
            Console.WriteLine(ageGroup);
        }
        private AgeCategory GetStudentMatchingAgeGroup(int age)
        {
            if (age < 0)
                throw new ArgumentOutOfRangeException(paramName: nameof(age));

            return CalculateAge(age);
            
            AgeCategory CalculateAge(int i)
            {
                switch (i)
                {
                    case int val when (val > 0 && val < 1):
                        return AgeCategory.Baby;
                    case int val when (val > 1 && val < 3):
                        return AgeCategory.Toddler;
                    case int val when (val > 3 && val < 5):
                        return AgeCategory.Preschooler;
                    case int val when (val > 5 && val < 12):
                        return AgeCategory.Gradeschooler;
                    default:
                        throw new ArgumentOutOfRangeException(paramName: nameof(age));
                }
            }
        }
    }
```
### 6.	More expression-bodied members
C# 7.0 allows expression bodied members with constructors, destructors, and also property 	accessors.
```cs
    public class Rectangle : Shape
    {
        public int Height { get; } = 10;
        public int Width => 20;
    }

    public class Circle : Shape
    {
        public int Radius => 40;
        public override string ToString() => $" Hi from Circle shape.";
    }
```

### 7.	Throw Expressions
Throwing exceptions can be used in a number of places that were not allowed in previous versions. It is possible to add exception throwing to expression bodied members, null-coalescing expressions, and conditional expressions.
```cs
    private static string appConnectionString = LoadTenantConnectionString() ?? throw new                             InvalidOperationException("Could not load connection string.");

        private string _applicationName;
        private string _versionNumber;

        public string ApplicationName
        {
            get => _applicationName;
            set => _applicationName = value ??
                    throw new ArgumentNullException(paramName: nameof(value),
                    message: "Application Name must not be null");
        }

        public string ApplicationVersion
        {
            get => _versionNumber;
            set => _versionNumber = value ?? "v1.0";
        }
        public override string ToString() =>
                        $" App: {_applicationName}, Version: {_versionNumber}";

        public ThrowExpressionDemo(string applicationName) => _applicationName = applicationName ??                                           "Bonafide";
```

### 8.	Generalized async return types

C# 7.0 introduces the ability to define custom return types on  async methods compared to  	previous versions, including new ValueTask<T> by enabling to have better control over the     	performance concerns

```cs
    public class AsyncTypeDemo
    {

        public async Task<long> ReadDirectorySize(string directory)
        {
            if (!Directory.EnumerateFileSystemEntries(directory).Any())
                return 0;

            return await Task.Run(() => Directory.GetFiles(directory, "*.*",                                              SearchOption.TopDirectoryOnly).Sum(f => new FileInfo(f).Length));
        }

        // Install-Package System.Threading.Tasks.Extensions 
        public async ValueTask<long> ReadDirectorySize1(string directory)
        {
            if (!Directory.EnumerateFileSystemEntries(directory).Any())
                return 0;

            return await Task.Run(() => Directory.GetFiles(directory, "*.*",                                              SearchOption.TopDirectoryOnly).Sum(f => new FileInfo(f).Length));
        }

        private bool cache = false;
        private int cacheResult;
        public ValueTask<int> GetCachedSettings()
        {
            return (cache) ? new ValueTask<int>(cacheResult) : new ValueTask<int>(LoadCache());
        }
        private async Task<int> LoadCache()
        {
            // simulate async work:
            await Task.Delay(100);
            cacheResult = 100;
            cache = true;
            return cacheResult;
        }

        public AsyncTypeDemo()
        {
            var result = 
            ReadDirectorySize1(@"E:\SigmaBravo.Bonafide-Ultima\SigmaBravo.Bonafide.Web")
                    .Result;
            Console.WriteLine($"Sum of the File sizes: {result}");
            // GetCachedSettings();
        }
    }
```

### 9.	Numeric literal syntax improvements
C# 7.0 introduces two literal improvements, a digit separator and a hex literal. Digit separator is used to improve the readability for numeric constants. This“_” can be used 	inside number literals. C# 7.0 defines binary literals by adding“0b”to the beginning of the constant.

```cs
            var anyNumber = 10_0_050_000;
            WriteLine($"The Number is: {anyNumber}");

            int sixteen = 0b1_0000, thirtyTwo = 0b0010_0000, sixtyFour = 0b0100_0000;
            WriteLine($"sixteen: {sixteen}, thirty two: {thirtyTwo}, sixty four: {sixtyFour}");

            var hexadecimalNumber = 0xA;
            WriteLine($"The Hexadecimal is: {hexadecimalNumber}");
```








