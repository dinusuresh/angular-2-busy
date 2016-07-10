# Angular2-Busy

**Angular 2 Busy** can show busy/loading indicators on any promise, or on any  Observable's subscription.

Rewritten from [angular-busy](https://github.com/cgross/angular-busy), and add some new features in terms of Angular 2.

#### Built with Angular 2 RC4

## Demo

[devyumao.github.io/angular2-busy/demo](https://devyumao.github.io/angular2-busy/demo/)

## Installation

```shell
npm install --save angular2-busy
```

## Link CSS

```html
<link rel="stylesheet" href="/node_modules/angular2-busy/build/style/busy.css">
```

## Getting Started

Inject the `BusyService` into your app's root component:

```typescript
import {Component} from '@angular/core';
import {BusyService} from 'angular2-busy';

@Component({
    selector: 'app',
    providers: [BusyService],
    // ...
})
class AppComponent {
    // ...
}
```

Reference your promise in the `ngBusy` directive:

```typescript
import {Component, OnInit} from '@angular/core';
import {Http} from '@angular/http';
import {BusyDirective} from 'angular2-busy';

@Component({
    selector: 'some',
    template: `
        <div [ngBusy]="busy"></div>
    `
})
class SomeComponent implements OnInit {
    busy: Promise<any>;

    constructor(private http: Http) {}

    ngOnInit() {
        this.busy = this.http.get('...').toPromise();
    }
}
```

Moreover, the subscription of an Observable is also supported:

```typescript
// ...
import {Subscription} from 'rxjs';

// ...
class SomeComponent implements OnInit {
    busy: Subscription;

    // ...

    ngOnInit() {
        this.busy = this.http.get('...').subscribe();
    }
}
```

## Directive Syntax

The `ngBusy` directive expects a ***busy thing***, which means:
- A promise
- Or an Observable's subscription
- Or an array of them
- Or a configuration object

In other words, you may use flexble syntax:

```html
<!-- Simple syntax -->
<div [ngBusy]="busy"></div>
```

```html
<!-- Collection syntax -->
<div [ngBusy]="[busyA, busyB, busyC]"></div>
```

```html
<!-- Advanced syntax -->
<div [ngBusy]="{busy: busy, message: 'Loading...', backdrop: false, delay: 200, minDuration: 600}"></div>
```

## Options

| Option | Required | Default | Details |
| ---- | ---- | ---- | ---- |
| busy | Required | null | A busy thing (or an array of busy things) that will cause the loading indicator to show. |
| message | Optional | 'Please wait...' | The message to show in the indicator which will reflect the updated values as they are changed. |
| backdrop | Optional | true | A faded backdrop will be shown behind the indicator if true. |
| template | Optional | A default template string | If provided, the custom template will be shown in place of the default indicatory template. The scope can be augmented with a `{{message}}` field containing the indicator message text. |
| delay | Optional | 0 | The amount of time to wait until showing the indicator. Specified in milliseconds.
| minDuration | Optional | 0 | The amount of time to keep the indicator showing even if the busy thing was completed quicker. Specified in milliseconds.|
| wrapperClass | Optional | 'ng-busy' | The name(s) of the CSS classes to be applied to the wrapper element of the indicator. |


## Overriding Defaults

The default values of options can be overriden by assigning your custom configuration to the `busyService.config`.

In your app's root component, you can do this:

```typescript
import {Component} from '@angular/core';
import {BusyService} from 'angular2-busy';

@Component({
    selector: 'app',
    providers: [BusyService],
    // ...
})
class AppComponent {
    constructor(private busyService: BusyService) {
        busyService.config = {
            message: 'Uploading...',
            backdrop: false,
            template: `
            	<div>{{message}}</div>
            `,
            delay: 200,
            minDuration: 600,
            wrapperClass: 'my-class'
        };
    }
}
```

## TODO

- Provide custom animations for the indicator

- Unit & E2E test

## Credits

Rewritten from [cgross](https://github.com/cgross)'s [angular-busy](https://github.com/cgross/angular-busy).

Inspired by [ajoslin](https://github.com/ajoslin)'s [angular-promise-tracker](https://github.com/ajoslin/angular-promise-tracker).

## LICENSE

This project is licensed under the MIT license. See the [LICENSE](https://github.com/devyumao/angular2-busy/blob/master/LICENSE) file for more info.
