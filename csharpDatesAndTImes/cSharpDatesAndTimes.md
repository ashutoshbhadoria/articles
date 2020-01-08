# Working with Dates and Times in .NET

Storing and parsing dates and times have always a been a core requirement in almost all practical applications. When was the order placed? When will my travel ticket expire? What was the date of joining of an employee? All these situations require our applications to deal with date and time information.


Even if we speak outside the context of software conveying date and time information has always been riddled with ambiguities. For example, if I am working from Mumbai, India and some of my colleagues are working from Copenhagen, Denmark and I put out a message that let's have a Skype meeting at 9. Obvious follow-up questions would be, 9 AM or 9 PM ? If it is 9 AM, your 9 AM or my 9AM? Also if I say I am throwing a party on 7/11/2020, when is the party is it on November 7, 2020 or July 11, 2020?

In this article, we will discuss how to represent and parse date and time values, and also try to address some of the ambiguities associated with these constructs.

(For code demos I used C# with [.NET Core SDK v3.1.100](https://dotnet.microsoft.com/download/dotnet-core/3.0))

## Using C# `DateTime`

### Creating `DateTime` instance

A `DateTime` instance represents an instant in time expressed as a date and time of day. And we can use the constructor of this class to create a representation of date and time using the instance of `DateTime` class using the constructor. Well that doesn't explain much let us jump into the code :smiley:

```cs
using System;

namespace TimeAndDate
{
    class Program
    {
        static void Main(string[] args)
        {
            // Represent Date.
            var pizzaDay = new DateTime(2020, 2, 9);
            Console.WriteLine($"Pizza Day: {pizzaDay}");
            
            // Long date string representation.
            Console.WriteLine($"Pizza Day: {pizzaDay.ToLongDateString()}");

            // Represent both date and point time using overloaded DateTime constructor.
            var partyDateAndTime = new DateTime(2020, 2, 9, 12, 0, 0);
            Console.WriteLine($"Party Date: {partyDateAndTime.ToLongDateString()}"); 
            Console.WriteLine($"Party Time: {partyDateAndTime.ToLongTimeString()}"); 
        }
    }
}
```
Output:
```
C:\Users\MajorTom\TimeAndDate>dotnet run
Pizza Day: 2/9/2020 00:00:00
Pizza Day: Sunday, February 9, 2020
Party Date: Sunday, February 9, 2020
Party Time: 12:00:00
```

### `DateTime` class static properties
`DateTime` class offeres a wide-range of range of static methods and properties which can be really handy. One of the most often used being `DateTime.Now`, which returns an object whose value is the current local date and time. Let's look at some these properties in our code snippet.

```cs
using System;

namespace TimeAndDate
{
    class Program
    {
        static void Main(string[] args)
        {
            var now = DateTime.Now;
            Console.WriteLine($"Now: {now.ToString()}");
            Console.WriteLine($"Hour: {now.Hour}");
            Console.WriteLine($"Minutes: {now.Minute}");
            Console.WriteLine($"Seconds: {now.Second}");
            Console.WriteLine($"Milliseconds: {now.Millisecond}");
            Console.WriteLine($"Day: {now.Day}");
            Console.WriteLine($"Month: {now.Month}");
            Console.WriteLine($"Year: {now.Year}");
            Console.WriteLine($"Day of week: {now.DayOfWeek}");
            Console.WriteLine($"Day of year: {now.DayOfYear}");
        }
    }
}
```
Output:
```
C:\Users\MajorTom\TimeAndDate>dotnet run
Now: 1/9/2020 12:07:01
Hour: 12
Minutes: 7
Seconds: 1
Milliseconds: 464
Day: 9
Month: 1
Year: 2020
Day of week: Thursday
Day of year: 9
```

### Parsing `string` into `DateTime` instances
Using `DateTime.Parse()` we can convert the `string` representation of a date and time to its `DateTime` equivalent. But we will encounter the problem we discussed in the introductory section how date formats can be ambiguous depending on the 'culture'.

```cs
using System;
using System.Globalization;

namespace TimeAndDate
{
    class Program
    {
        static void Main(string[] args)
        {
            var dateString = "7/11/2020";

            // Local Culture set to -> "en-US"
            var myDate = DateTime.Parse(dateString);
            Console.WriteLine($"Local format date: {myDate.ToLongDateString()}");

            // Setting culture explicitly to -> "en-GB"
            var myBritishDate = DateTime.Parse(dateString, CultureInfo.GetCultureInfo("en-GB"));
            Console.WriteLine($"British format date: {myBritishDate.ToLongDateString()}");
        }
    }
}
```
Output:
```
C:\Users\MajorTom\TimeAndDate>dotnet run
Local format date: Saturday, July 11, 2020
British format date: Saturday, November 7, 2020
```

To solve this ambiguity problem we can provide the format string with which we would like to parse the string representing the date.

```cs
using System;
using System.Globalization;

namespace TimeAndDate
{
    class Program
    {
        static void Main(string[] args)
        {
            var dateString = "7/11/2020 7:30:23 PM";
            
            // Parse the string with the specified format and invariant culture. 
            var date = DateTime.ParseExact(dateString, "d/M/yyyy h:mm:ss tt", CultureInfo.InvariantCulture);

            Console.WriteLine(date.ToLongDateString());
            Console.WriteLine(date.ToLongTimeString());
        }
    }
}
```
Output:
```
C:\Users\MajorTom\TimeAndDate>dotnet run
Saturday, November 7, 2020
19:30:23
```

### Dealing with `TimeZone`
As we live on a spherical planet a specific instant my be represented by different times according to the timezone. `TimeZoneInfo` provides time zone information and tools to work with different time zones. We can covert timezones using the static method `ConvertTime `provided by the `TimeZoneInfo` class.

```cs
using System;
using System.Globalization;

namespace TimeAndDate
{
    class Program
    {
        static void Main(string[] args)
        {
            var now = DateTime.Now;
            Console.WriteLine($"Local Time (Indian Standard Time) => {now}");

            var sydneyTimeZone = TimeZoneInfo.FindSystemTimeZoneById("AUS Eastern Standard Time");
            var sydneyTime = TimeZoneInfo.ConvertTime(now, sydneyTimeZone);
            Console.WriteLine($"Sydney Time => {sydneyTime}");
        }
    }
}
```
```
C:\Users\MajorTom\TimeAndDate>dotnet run
Local Time (Indian Standard Time) => 1/9/2020 14:05:06
Sydney Time => 1/9/2020 19:35:06
```
Another static method `TimeZoneInfo.GetSystemTimeZones()` returns a sorted collection of all the timezones about which information is available on the local system.


## C# TimeSpan
`TimeSpan` represents a time interval (duration of time or elapsed time) that is measured as a positive or negative number of days, hours, minutes, seconds, and fractions of a second.

Let us see how to create `TimeSpan` instances and explore some of its properties.
```cs
using System;
using System.Globalization;

namespace TimeAndDate
{
    class Program
    {
        public static void Main(string[] args)
        {
            var timeSpan = new TimeSpan(60, 100, 200);
            
            Console.WriteLine($"timeSpan => {timeSpan.ToString()}");

            Console.WriteLine($"Days component of the timeSpan: {timeSpan.Days}");
            Console.WriteLine($"Hours component of the timeSpan: {timeSpan.Hours}");
            Console.WriteLine($"Minutes component of the timeSpan: {timeSpan.Minutes}");
            Console.WriteLine($"Seconds component of the timeSpan: {timeSpan.Seconds}");
            Console.WriteLine($"Milliseconds component of the timeSpan: {timeSpan.Milliseconds}");
            
            
            Console.WriteLine($"timeSpan expressed in whole and fractional days: {timeSpan.TotalDays}");
            Console.WriteLine($"timeSpan expressed in whole and fractional hours: {timeSpan.TotalHours}");
            Console.WriteLine($"timeSpan expressed in whole and fractional minutes: {timeSpan.TotalMinutes}");
            Console.WriteLine($"timeSpan expressed in whole and fractional seconds: {timeSpan.TotalSeconds}");
            Console.WriteLine($"timeSpan expressed in whole and fractional milliseconds: {timeSpan.TotalMilliseconds}");
        }
    }
}
```
Output:
```
C:\Users\MajorTom\TimeAndDate>dotnet run
timeSpan => 2.13:43:20
Days component of the timeSpan: 2
Hours component of the timeSpan: 13
Minutes component of the timeSpan: 43
Seconds component of the timeSpan: 20
Milliseconds component of the timeSpan: 0
timeSpan expressed in whole and fractional days: 2.571759259259259
timeSpan expressed in whole and fractional hours: 61.72222222222222
timeSpan expressed in whole and fractional minutes: 3703.3333333333335
timeSpan expressed in whole and fractional seconds: 222200
timeSpan expressed in whole and fractional milliseconds: 222200000
```

We can also create standard values of the timespans using the static methods provided by the class.
```cs
using System;

namespace TimeAndDate
{
    class Program
    {
        public static void Main(string[] args)
        {
            Console.WriteLine($"Value representing a zero timespan: {TimeSpan.Zero}");
            Console.WriteLine($"Value representing a 3 hours timespan: {TimeSpan.FromHours(3)}");
            Console.WriteLine($"Value representing a 45 minutes timespan: {TimeSpan.FromMinutes(45)}");
            Console.WriteLine($"Value representing a 69 seconds timespan: {TimeSpan.FromSeconds(69)}");
            Console.WriteLine($"Value representing a 420 milliseconds timespan: {TimeSpan.FromMilliseconds(420)}");
            // A single tick represents one hundred nanoseconds or one ten-millionth of a second.
            Console.WriteLine($"Value representing a 12345 ticks timespan: {TimeSpan.FromTicks(12345)}");    
        }
    }
}
```
Output:
```
C:\Users\MajorTom\TimeAndDate>dotnet run
Value representing a zero timespan: 00:00:00
Value representing a 3 hours timespan: 03:00:00
Value representing a 45 minutes timespan: 00:45:00
Value representing a 69 seconds timespan: 00:01:09
Value representing a 420 milliseconds timespan: 00:00:00.4200000
Value representing a 12345 ticks timespan: 00:00:00.0012345
```

An very important feature of timespan is it allows us to perform arithematic operations on the time intervals. The following code snippet will make it clear.
```cs
using System;

namespace TimeAndDate
{
    class Program
    {
        public static void Main(string[] args)
        {
            var start = new DateTime(2020, 11, 7, 19, 30, 00);
            Console.WriteLine($"start: {start.ToString()}");

            var timeSpan = TimeSpan.FromSeconds(45);
            var end = start + timeSpan; // Can also be achieved by var end = start.AddSeconds(45)
            Console.WriteLine($"end: {end.ToString()}");
            
            timeSpan = timeSpan.Multiply(2);
            Console.WriteLine($"Double our timespan: {timeSpan.ToString()}");

            timeSpan = timeSpan.Divide(5);
            Console.WriteLine($"Quarter of the timespan: {timeSpan.ToString()}");
        }
    }
}
```
Output:
```
C:\Users\MajorTom\TimeAndDate>dotnet run
start: 11/7/2020 19:30:00
end: 11/7/2020 19:30:45
Double our timespan: 00:01:30
Quarter of the timespan: 00:00:18
```

## ISO 8601 (UTC time)
Despite the multitude of functionalities offered by the `DateTime` class it still lacks the information about the timezone. This may lead to a wide variety of problems, what if I store the time at which an order was placed as `DateTime` according to my local timezone while my collegue working on the other side of the globe this particualar instance of `DateTime` may be a completely different instant of time.

