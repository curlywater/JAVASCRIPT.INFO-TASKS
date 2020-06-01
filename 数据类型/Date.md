# Date

## 创建日期

创建一个 Date 对象，日期是：Feb 20, 2012, 3:12am。时区是当地时区。

### 解决方案

``` javascript
console.log(new Date(2012, 1, 20, 3, 12));
```


## 显示星期数

编写一个函数 getWeekDay(date) 以短格式来显示一个日期的星期数：‘MO’，‘TU’，‘WE’，‘TH’，‘FR’，‘SA’，‘SU’。

例如：

``` javascript
let date = new Date(2012, 0, 3);  // 3 Jan 2012
alert( getWeekDay(date) );        // 应该输出 "TU"
```

### 解决方案

``` javascript
function getWeekDay (date) {
    const dayToFormat = ["SU", "MO", "TU", "WE", "TH", "FR", "SA"];
    const day = date.getDay();
    return dayToFormat[day];
}

let date = new Date(2012, 0, 3);
console.log( getWeekDay(date));
```

## 欧洲的星期表示方法

欧洲国家的星期计算是从星期一（数字 1）开始的，然后是星期二（数字 2），直到星期日（数字 7）。编写一个函数 getLocalDay(date)，并返回日期的欧洲式星期数。

let date = new Date(2012, 0, 3);  // 3 Jan 2012
alert( getLocalDay(date) );       // 星期二，应该显示 2

``` javascript
function getLocalDay (date) {
    const day = date.getDay();
    if (day === 0) {
        return 7;
    } else {
        return day;
    }
}
```

## 许多天之前是哪个月几号？

写一个函数 getDateAgo(date, days)，返回特定日期 date 往前 days 天是哪个月的哪一天。

例如，假设今天是 20 号，那么 getDateAgo(new Date(), 1) 的结果应该是 19 号，getDateAgo(new Date(), 2) 的结果应该是 18 号。

跨月、年也应该是正确输出：

let date = new Date(2015, 0, 2);

alert( getDateAgo(date, 1) ); // 1, (1 Jan 2015)
alert( getDateAgo(date, 2) ); // 31, (31 Dec 2014)
alert( getDateAgo(date, 365) ); // 2, (2 Jan 2014)
P.S. 函数不应该修改给定的 date 值。

### 解决方案

``` javascript
function getDateAgo (date, days) {
    const dateAgo = new Date(date - days * 24 * 60 * 60 * 1000);
    return dateAgo.getDate();
}

let date = new Date(2015, 0, 2);

console.log( getDateAgo(date, 1) ); // 1, (1 Jan 2015)
console.log( getDateAgo(date, 2) ); // 31, (31 Dec 2014)
console.log( getDateAgo(date, 365) ); // 2, (2 Jan 2014)
```

``` javascript
function getDateAgo (date, days) {
    const dateAgo = new Date(date);
    dateAgo.setDate(date.getDate() - days);
    return dateAgo.getDate();
}
```

## 某月的最后一天？

写一个函数 getLastDayOfMonth(year, month) 返回 month 月的最后一天。有时候是 30，有时是 31，甚至在二月的时候会是 28/29。

参数：

year —— 四位数的年份，比如 2012。
month —— 月份，从 0 到 11。
举个例子，getLastDayOfMonth(2012, 1) = 29（闰年，二月）

### 解决方案

``` javascript
function getLastDayOfMonth (year, month) {
    return new Date(year, month + 1, 0).getDate();
}
console.log(getLastDayOfMonth(2012, 1))
```

## 今天过去了多少秒？

写一个函数 getSecondsToday()，返回今天已经过去了多少秒？

例如：如果现在是 10:00 am，并且没有夏令时转换，那么：

getSecondsToday() == 36000 // (3600 * 10)
该函数应该在任意一天都能正确运行。那意味着，它不应具有“今天”的硬编码值。

### 解决方案

``` javascript
function getSecondsToday () {
    const date = new Date();
    return date.getHours() * 3600 + date.getMinutes() * 60 + date.getSeconds();
}
console.log(getSecondsToday())
```

## 距离明天还有多少秒？

写一个函数 getSecondsToTomorrow()，返回距离明天的秒数。

例如，现在是 23:00，那么：

getSecondsToTomorrow() == 3600
P.S. 该函数应该在任意一天都能正确运行。那意味着，它不应具有“今天”的硬编码值。

### 解决方案

``` javascript
function getSecondsToTomorrow () {
    const now = new Date();
    const tomorrow = new Date(now.getFullYear(), now.getMonth(), now.getDate() + 1);
    return Math.round((tomorrow - now) / 1000);
}
console.log(getSecondsToTomorrow());
```

## 格式化相对日期

写一个函数 formatDate(date)，能够对 date 进行如下格式化：

如果 date 距离现在不到 1 秒，输出 "right now"。
否则，如果 date 距离现在不到 1 分钟，输出 "n sec. ago"。
否则，如果不到 1 小时，输出 "m min. ago"。
否则，以 "DD.MM.YY HH:mm" 格式输出完整日期。即："day.month.year hours:minutes"，全部以两位数格式表示，例如：31.12.16 10:00。

### 解决方案

``` javascript
function formatPart (number) {
    return ("0" + number).slice(-2);
}

function formatDate (date) {
    const now = Date.now();
    const diff = now - date;
    if (diff < 1000) {
        return "right now";
    } else if (diff < 60000) {
        return `${Math.floor(diff / 1000)} sec. ago`;
    } else if (diff < 3600000) {
        return `${Math.floor(diff / 60000)} min. ago`;
    } else {
        const datePart = [date.getDate(), date.getMonth() + 1, date.getFullYear(), date.getHours(), date.getMinutes()].map(formatPart);
        return datePart.slice(0, 3).join(".") + " " + datePart.slice(3).join(":")
    }
}

console.log( formatDate(new Date(new Date - 1)) ); // "right now"

console.log( formatDate(new Date(new Date - 30 * 1000)) ); // "30 sec. ago"

console.log( formatDate(new Date(new Date - 5 * 60 * 1000)) ); // "5 min. ago"

// 昨天的日期，例如 31.12.16 20:00
console.log( formatDate(new Date(new Date - 86400 * 1000)) );
```