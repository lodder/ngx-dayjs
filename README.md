# ngx-dayjs

dayjs.js pipes for Angular

[![Build Status](https://travis-ci.org/ljmerza/ngx-dayjs.svg?branch=master)](https://travis-ci.org/ljmerza/ngx-dayjs)

Installation
------------

`npm install --save ngx-dayjs`

### For System.js users:

First you need to install dayjs:

`npm install dayjs --save`

Don't forget to update your systemjs.config.js:

```
packages: {
            app: {
                main: './main.js',
                defaultExtension: 'js'
            },
            'dayjs': {
                main: './dayjs.js',
                defaultExtension: 'js'
            },
            'ngx-dayjs': {
                main: './index.js',
                defaultExtension: 'js'
            }
        }
```

Usage
-----

Import `dayjsModule` into your app's modules:

``` typescript
import { dayjsModule } from 'ngx-dayjs';

@NgModule({
  imports: [
    dayjsModule
  ]
})
```

This makes all the `ngx-dayjs` pipes available for use in your app components.

Available pipes
---------------

## amTimeAgo pipe
Takes an optional `omitSuffix` argument that defaults to `false`.

``` typescript
@Component({
  selector: 'app',
  template: `
    Last updated: {{myDate | amTimeAgo}}
  `
})
```

Prints `Last updated: a few seconds ago`

``` typescript
@Component({
  selector: 'app',
  template: `
    Last updated: {{myDate | amTimeAgo:true}}
  `
})
```

Prints `Last updated: a few seconds`

## amCalendar pipe
Takes optional `referenceTime` argument (defaults to now)
and `formats` argument that could be output formats object or callback function.
See [dayjs docs](http://dayjs.com/docs/#/displaying/calendar-time/) for details.

``` typescript
@Component({
  selector: 'app',
  template: `
    Last updated: {{myDate | amCalendar}}
  `
})
```

Prints `Last updated: Today at 14:00` (default referenceTime is today by default)

``` typescript
@Component({
  selector: 'app',
  template: `
    Last updated: <time>{{myDate | amCalendar:nextDay }}</time>
  `
})
export class AppComponent {
  nextDay: Date;

  constructor() {
      this.nextDay = new Date();
      nextDay.setDate(nextDay.getDate() + 1);
  }
}
```

Prints `Last updated: Yesterday at 14:00` (referenceTime is tomorrow)

``` typescript
@Component({
  selector: 'app',
  template: `
    Last updated: <time>{{myDate | amCalendar:{sameDay:'[Same Day at] h:mm A'} }}</time>
  `
})
```

Prints `Last updated: Same Day at 2:00 PM`

## amDateFormat pipe

``` typescript
@Component({
  selector: 'app',
  template: `
    Last updated: {{myDate | amDateFormat:'LL'}}
  `
})
```

Prints `Last updated: January 24, 2016`

## amParse pipe

Parses a custom-formatted date into a dayjs object that can be used with the other pipes.

``` typescript
@Component({
  selector: 'app',
  template: `
    Last updated: {{'24/01/2014' | amParse:'DD/MM/YYYY' | amDateFormat:'LL'}}
  `
})
```

Prints `Last updated: January 24, 2016`

## amLocal pipe

Converts UTC time to local time.

``` typescript
@Component({
  selector: 'app',
  template: `
    Last updated: {{mydate | amLocal | amDateFormat: 'YYYY-MM-DD HH:mm'}}
  `
})
```

Prints `Last updated 2016-01-24 12:34`

## amLocale pipe

To be used with amDateFormat pipe in order to change locale.

``` typescript
@Component({
  selector: 'app',
  template: `
    Last updated: {{'2016-01-24 14:23:45' | amLocale:'en' | amDateFormat:'MMMM Do YYYY, h:mm:ss a'}}
  `
})
```

Prints `Last updated: January 24th 2016, 2:23:45 pm`

## amFromUnix pipe

``` typescript
@Component({
  selector: 'app',
  template: `
    Last updated: {{ (1456263980 | amFromUnix) | amDateFormat:'hh:mmA'}}
  `
})
```

Prints `Last updated: 01:46PM`

## amDuration pipe

``` typescript
@Component({
  selector: 'app',
  template: `
    Uptime: {{ 365 | amDuration:'seconds' }}
  `
})
```

Prints `Uptime: 6 minutes`

## amDifference pipe

``` typescript
@Component({
  selector: 'app',
  template: `
    Expiration: {{nextDay | amDifference: today :'days' : true}} days
  `
})
```
Prints `Expiration: 1 day`

## amAdd and amSubtract pipes

Use these pipes to perform date arithmetics. See [dayjsjs docs](http://dayjsjs.com/docs/#/manipulating/add/) for details.

``` typescript
@Component({
  selector: 'app',
  template: `
    Expiration: {{'2017-03-17T16:55:00.000+01:00' | amAdd: 2 : 'hours' | amDateFormat: 'YYYY-MM-DD HH:mm'}}
  `
})
```
Prints `Expiration: 2017-03-17 18:55`

``` typescript
@Component({
  selector: 'app',
  template: `
    Last updated: {{'2017-03-17T16:55:00.000+01:00' | amSubtract: 5 : 'years' | amDateFormat: 'YYYY-MM-DD HH:mm'}}
  `
})
```
Prints `Last updated: 2012-03-17 16:55`

## amFromUtc pipe

Parses the date as UTC and enables mode for subsequent dayjs operations (such as displaying the time in UTC). This can be combined with `amLocal` to display a UTC date in local time.

``` typescript
@Component({
  selector: 'app',
  template: `
    Last updated: {{ '2016-12-31T23:00:00.000-01:00' | amFromUtc | amDateFormat: 'YYYY-MM-DD' }}
  `
})
```

Prints `Last updated: 2017-01-01`

## amUtc pipe

Enables UTC mode for subsequent dayjs operations (such as displaying the time in UTC).

``` typescript
@Component({
  selector: 'app',
  template: `
    Last updated: {{ '2016-12-31T23:00:00.000-01:00' | amUtc | amDateFormat: 'YYYY-MM-DD' }}
  `
})
```

Prints `Last updated: 2017-01-01`

Complete Example
----------------

``` typescript
import { NgModule, Component } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { dayjsModule } from 'ngx-dayjs';

@Component({
  selector: 'app',
  template: `
    Last updated: <b>{{myDate | amTimeAgo}}</b>, <b>{{myDate | amCalendar}}</b>, <b>{{myDate | amDateFormat:'LL'}}</b>
  `
})
export class AppComponent {
  myDate: Date;

  constructor() {
    this.myDate = new Date();
  }
}

@NgModule({
  imports: [
    BrowserModule,
    dayjsModule
  ],
  declarations: [ AppComponent ]
  bootstrap: [ AppComponent ]
})
class AppModule {}

platformBrowserDynamic().bootstrapModule(AppModule);
```

Demo
----

[See online demo on Plunker](http://plnkr.co/edit/ziBJ0mftSjnz0SrYPwbo?p=preview)
