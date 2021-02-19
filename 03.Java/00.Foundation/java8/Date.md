<details>
<summary>点击展开目录</summary>
<!-- TOC -->

- [ZoneId](#zoneid)
- [Instant](#instant)
- [LocalDate](#localdate)
- [LocalTime](#localtime)
- [LocalDateTime](#localdatetime)
- [Clock](#clock)
- [Period](#period)
- [Duration](#duration)
- [DateTimeFormatter](#datetimeformatter)
- [新旧API的转换](#新旧api的转换)

<!-- /TOC -->
</details>

## ZoneId

时区ID, 用来确定Instant和LocalDateTime互相转换的规则


## Instant

用来表示时间线上的一个点


## LocalDate

表示没有时区的日期, LocalDate是不可变并且线程安全的
yyyy-MM-dd

## LocalTime

表示没有时区的时间, LocalTime是不可变并且线程安全的


## LocalDateTime

表示没有时区的日期时间, LocalDateTime是不可变并且线程安全的

```Java
// 零时的毫秒值
LocalDate.now().atStartOfDay(ZoneId.systemDefault()).toInstant().toEpochMilli();
LocalDateTime.now().with(LocalTime.MIN).toInstant(ZoneOffset.of("+8")).toEpochMilli();
```

## Clock

用于访问当前时刻, 日期, 时间, 用到时区

## Period
日期间隔

Period.of
`PnYnMnD`

## Duration
秒或纳秒时间间隔, 适合处理较短的时间
`PnDTnHnMn.nS`
Duration.parse("P1DT1H10M10.5S");

用秒和纳秒表示时间的数量
java.time.Duration

ChronoUnit

## DateTimeFormatter

```Java
// Date和Instant互相转换
Date date = Date.from(Instant.now());
Instant instant = date.toInstant();
// Date转换为LocalDateTime
LocalDateTime localDateTime = LocalDateTime.from(new Date());
System.out.println(localDateTime);
// LocalDateTime转Date
Date date =
Date.from(localDateTime.atZone(ZoneId.systemDefault()).toInstant());
// LocalDate转Date
Date date =
Date.from(LocalDate.now().atStartOfDay().atZone(ZoneId.systemDefault()).toInstant()
```




```Java
// 00:00:00
LocalDateTime today_start = LocalDateTime.of(LocalDate.now(), LocalTime.MIN);
// 23:59:59
LocalDateTime today_end = LocalDateTime.of(LocalDate.now(), LocalTime.MAX);
```
```Java
// 0时的毫秒值
long timestamp = LocalDate.now()
        .atStartOfDay(ZoneId.systemDefault())
        .toInstant().toEpochMilli();
```

## 新旧API的转换

```Java
// LocalDate -> Date
Date date = Date.from(LocalDate.now().atStartOfDay(ZoneOffset.ofHours(8)).toInstant());
// LocalDateTime -> Date
Date date = Date.from(LocalDateTime.now().atZone(ZoneOffset.ofHours(8)).toInstant());
// LocalDate -> 时间戳
long timestamp = LocalDate.now().atStartOfDay(ZoneOffset.ofHours(8)).toInstant().toEpochMilli();
// LocalDateTime -> 时间戳
long timestamp = LocalDateTime.now().toInstant(ZoneOffset.ofHours(8)).toEpochMilli();
```

```Java
// Date -> LocalDate
LocalDate localDate = new Date().toInstant().atZone(ZoneOffset.ofHours(8)).toLocalDate();
// Date -> LocalDateTime
LocalDateTime localDateTime = new Date().toInstant().atZone(ZoneOffset.ofHours(8)).toLocalDateTime();
// 时间戳 -> LocalDate
LocalDate localDate = Instant.ofEpochMilli(System.currentTimeMillis()).atZone(ZoneOffset.ofHours(8)).toLocalDate();
// 时间戳 -> LocalDateTime
LocalDateTime localDateTime = Instant.ofEpochMilli(System.currentTimeMillis()).atZone(ZoneOffset.ofHours(8)).toLocalDateTime();
```
