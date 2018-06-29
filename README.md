<p align="center" >
<img src="Documentation/SwiftDate.png" width=597px alt="SwiftDate" title="SwiftDate">
</p>


SwiftDate is the definitive toolchain to manipulate and display dates and time zones on all Apple platform and even on Linux and Swift Server Side frameworks like Vapor or Kitura.

<p align="center">
<h2>10 Good Reasons to use SwiftDate</h2>
</p>

### 1. Date Parsing
SwiftDate can recognize all the major datetime formats  automatically (ISO8601, RSS, Alt RSS, .NET, SQL, HTTP...) and you can also provide your own formats.
Creating a new date has never been so easy!

```swift
// All default datetime formats (15+) are recognized automatically
let _ = "2010-05-20 15:30:00".toDate()
// You can also provide your own format!
let _ = "2010-05-20 15:30".toDate("yyyy-MM-dd HH:mm")
// All ISO8601 variants are supported too with timezone parsing!
let _ = "2017-09-17T11:59:29+02:00".toISODate()
// RSS, Extended, HTTP, SQL, .NET and all the major variants are supported!
let _ = "19 Nov 2015 22:20:40 +0100".toRSS(alt: true)

```

### 2. Date Manipulation
Date can be manipulated by adding or removing time components using a natural language; time unit extraction is also easy and includes the support for timezone, calendar and locales!

Manipulation can be done with standard math operators and between dates, time intervals, date components and relevant time units!

```swift
// Math operations support time units
let _ = ("2010-05-20 15:30:00".toDate() + 3.months - 2.days)
let _ = Date() + 3.hours
let _ = date1 + [.year:1, .month:2, .hour:5]
let _ = date1 + date2
// extract single time unit components from date manipulation
let over1Year = (date3 - date2).year > 1
```

### 3. Date Comparison
SwiftDate include an extensive set of comparison functions; you can compare two dates by granularity, check if a date is an particular day, range and pratically any other comparison you ever need.

Comparison is also available via standard math operators like (`>, >=, <, <=`).

```swift
// Standard math comparison is allowed
let _ = dateA >= dateB || dateC < dateB

// Complex comparisons includes granularity support
let _ = dateA.compare(toDate: dateB, granularity: .hour) == .orderedSame
let _ = dateA.isAfterDate(dateB, orEqual: true, granularity: .month) // > until month granularity
let _ = dateC.isInRange(date: dateA, and: dateB, orEqual: true, granularity: .day) // > until day granularity
let _ = dateA.earlierDate(dateB) // earlier date
let _ = dateA.laterDate(dateB) // later date

// Check if date is close to another with a given precision
let _ = dateA.compareCloseTo(dateB, precision: 1.hours.timeInterval

// Compare for relevant events:
// .isToday, .isYesterday, .isTomorrow, .isWeekend, isNextWeek
// .isSameDay, .isMorning, .isWeekday ...
let _ = date.compare(.isToday)
let _ = date.compare(.isNight)
let _ = date.compare(.isNextWeek)
let _ = date.compare(.isThisMonth)
let _ = date.compare(.startOfWeek)
let _ = date.compare(.isNextYear)
// ...and MORE THAN 30 OTHER COMPARISONS BUILT IN

// Operation in arrays (oldestIn, newestIn, sortedByNewest, sortedByOldest...)
let _ = DateInRegion.oldestIn(list: datesArray)
let _ = DateInRegion.sortedByNewest(list: datesArray)
```

### 4. Date Creation with Region (Timezone, Calendar & Locale)
You can create new dates from a string, time intervals or using date components. SwiftDate offers a wide set of functions to create and derivate your dates even with random generation!

```swift
// All dates includes timezone, calendar and locales!
// Create from string
let rome = Region(calendar: Calendars.gregorian, zone: Zones.europeRome, locale: Locales.italian)
let date1 = DateInRegion("2010-01-01 00:00:00", region: rome)!

// Create date from intervals
let _ = DateInRegion(seconds: 39940, region: rome)
let _ = DateInRegion(milliseconds: 5000, region: rome)

// Date from components
let _ = DateInRegion(components: {
	$0.year = 2001
	$0.month = 9
	$0.day = 11
	$0.hour = 12
	$0.minute = 0
}, region: rome)
let _ = DateInRegion(year: 2001, month: 1, day: 5, hour: 23, minute: 30, second: 0, region: rome)

// Random date generation with/without bounds
let _ = DateInRegion.randomDate(region: rome)
let _ = DateInRegion.randomDate(withinDaysBeforeToday: 5)
let _ = DateInRegion.randomDates(count: 50, between: lowerLimitDate, and: upperLimitDate, region: rome)
```

### 5. Derivated Dates
Date can be also generated starting from other dates; SwiftDate includes an extensive set of functions to generate.
Over 20 different derivated dates can be created easily using `dateAt()` function.

```swift
let _ = DateInRegion().dateAt(.endOfDay) // today at the end of the day
// Over 20 different relevant dates including .startOfDay,
// .endOfDay, .startOfWeek, .tomorrow, .nextWeekday, .nextMonth, .prevYear, .nearestMinute and many others!
let _ = dateA.nextWeekday(.friday) // the next friday after dateA
let _ = (date.dateAt(.startOfMonth) - 3.days)
let _ = dateA.compare(.endOfWeek)

// Enumerate dates in range by providing your own custom
// increment expressed in date components
let from = DateInRegion("2015-01-01 10:00:00", region: rome)!
let to = DateInRegion("2015-01-02 03:00:00", region: rome)!
let increment2 = DateComponents.create {
	$0.hour = 1
	$0.minute = 30
	$0.second = 10
}
// generate dates in range by incrementing +1h,30m,10s each new date
let dates = DateInRegion.enumerateDates(from: fromDate2, to: toDate2, increment: increment2)
```

### 6. Components Extraction
You can extract components directly from dates and it includes the right value expressed in date's region (the right timezone and set locale!).

```swift
// Create a date in a region, London but with the lcoale set to IT
let london = Region(calendar: .gregorian, zone: .europeLondon, locale: .italian)
let date = DateInRegion("2018-02-05 23:14:45", format: dateFormat, region: london)!

// You can extract any of the all available time units.
// VALUES ARE EXPRESSED IN THE REGION OF THE DATE (THE RIGHT TIMEZONE).
// (you can still get the UTC/absolute value by getting the inner's absoluteDate).

let _ = date.year // 2018
let _ = date.month // 2
let _ = date.monthNameDefault // 'Febbraio' as the locale is the to IT!
let _ = date.firstDayOfWeek // 5
let _ = date.weekdayNameShort // 'Lun' as locale is the to IT
// ... all components are supported: .year, .month, .day, .hour, .minute, .second,
// .monthName, .weekday, .nearestHour, .firstDayOfWeek. .quarter and so on...
```

### 7. Switch between timezones/locale and calendars
You can easily convert any date to another region (aka another calendar, locale or timezone) easily!
New date contains all values expressed into the destination reason

```swift
// Conversion between timezones is easy using convertTo(region:) function
let rNY = Region(calendar: Calendars.gregorian, zone: Zones.americaNewYork, locale: Locales.english)
let rRome = Region(calendar: Calendars.gregorian, zone: Zones.europeRome, locale: Locales.italian)
let dateInNY = "2017-01-01 00:00:00".toDate(region: rNY)
let dateInRome = dateInNY?.convertTo(region: rRome)!
print(dateInRome.toString()) // "dom gen 01 06:00:00 +0100 2017\n"

// You can also convert single region's attributes
let dateInIndia = dateInNY?.convertTo(timezone: Zones.indianChristmas, locale: Locales.nepaliIndia)
print("\(dateInIndia!.toString())") // "आइत जनवरी ०१ १२:००:०० +0700 २०१७\n"
```

### 8. Date Formatting
Date formatting is easy, you can specify your own format, locale or use any of the provided ones.

```swift
// Date Formatting
let london = Region(calendar: .gregorian, zone: .europeLondon, locale: .english)
let date = ... // 2017-07-22T18:27:02+02:00 in london region
let _ = date.toDotNET() // /Date(1500740822000+0200)/
let _ = date.toISODate() // 2017-07-22T18:27:02+02:00
let _ = date.toFormat("dd MMM yyyy 'at' HH:mm") // "22 July 2017 at 18:27"

// You can also easily change locale when formatting a region
let _ = date.toFormat("dd MMM", locale: .italian) // "22 Luglio"

// Time Interval Formatting as Countdown
let interval: TimeInterval = (2.hours.timeInterval) + (34.minutes.timeInterval) + (5.seconds.timeInterval)
let _ = interval.toClock() // "2:34:05"

// Time Interval Formatting by Components
let _ = interval.toString {
	$0.maximumUnitCount = 4
	$0.allowedUnits = [.day, .hour, .minute]
	$0.collapsesLargestUnit = true
	$0.unitsStyle = .abbreviated
} // "2h 34m"
```

### 9. Relative Date Formatting (fully customizable!)
Relative formatting is all new in SwiftDate; it supports 120+ languages with two different styles (`.default, .twitter`), 9 flavours (`.long, .longTime, .longConvenient, .short, .shortTime, .shortConvenient, .narrow, .tiny, .quantify`) and all of them are customizable as you need.
The extensible format allows you to provide your own translations and rules to override the default behaviour.

```swift
// Twitter Style
let _ = (Date() - 3.minutes).toRelative(style: RelativeFormatter.twitterStyle(), locale: Locales.english) // "3m"
let _ = (Date() - 6.minutes).toRelative(style: RelativeFormatter.twitterStyle(), locale: Locales.italian) // "6 min fa"

// Default Style
let _ = (now2 - 5.hours).toRelative(style: RelativeFormatter.defaultStyle(), locale: Locales.english) // "5 hours ago"
let y = (now2 - 40.minutes).toRelative(style: RelativeFormatter.defaultStyle(), locale: Locales.italian) // "45 minuti fa"
```

### 10. Period Support
SwiftDate integrates the great Matthew York's [DateTools](https://github.com/MatthewYork/DateTools) module in order to support Time Periods.

Dates are important, but the real world is a little less discrete than that. Life is made up of spans of time, like an afternoon appointment or a weeklong vacation. In DateTools, time periods are represented by the TimePeriod class and come with a suite of initializaiton, manipulation, and comparison methods to make working with them a breeze.

 