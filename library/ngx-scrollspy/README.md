[![npm version](https://img.shields.io/npm/v/@uniprank/ngx-scrollspy.svg?style=flat)](https://www.npmjs.com/package/@uniprank/ngx-scrollspy)
[![npm downloads](https://img.shields.io/npm/dm/@uniprank/ngx-scrollspy.svg)](https://npmjs.org/package/@uniprank/ngx-scrollspy)

You can use this angular service to spy scroll events from `window` or any other scrollable element.

This library implements an service to collect observables from scroll spy directives. It can be used to create you own components or if you prefer use one of the following directives.

## Installation

First you need to install the npm module:

```sh
npm install @uniprank/ngx-scrollspy --save
```

If you use SystemJS to load your files, you might have to update your config with this if you don't use `defaultJSExtensions: true`:

```js
System.config({
    packages: {
        '@uniprank/ngx-scrollspy': { defaultExtension: 'js' }
    }
});
```

Finally, you can use ngx-scrollspy in your Angular project (NOT AngularJS).
It is recommended to instantiate `ScrollSpyService` in the bootstrap of your application and to never add it to the "providers" property of your components, this way you will keep it as a singleton.
If you add it to the "providers" property of a component it will instantiate a new instance of the service that won't be initialized.

```js
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

import { ScrollSpyModule } from '@uniprank/ngx-scrollspy';

@NgModule({
  imports: [
  	BrowserModule,
  	ScrollSpyModule.forRoot()
  ],
  declarations: [ AppComponent ],
  bootstrap: [ AppComponent ]
})
```

## Using

#### Spy window scroll

Use `ScrollSpyDirective` to spy on window as default or set also stream to spy on a other scroll able element.

```js
import { NgModule, Component, Injectable, AfterViewInit } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

import { ScrollSpyModule, ScrollSpyService, ScrollObjectInterface } from '@uniprank/ngx-scrollspy';

@Injectable()
@Component({
	selector: 'app',
	template: `<div uniScrollSpy></div>`
})
export class AppComponent implements AfterViewInit {
	constructor(private _scrollSpyService: ScrollSpyService) {}

	ngAfterViewInit() {
		this._scrollSpyService.getObservable('window').subscribe((element: ScrollObjectInterface) => {
			console.log('ScrollSpy::window: ', element);
		});
	}
}

@NgModule({
  imports: [
  	BrowserModule,
  	ScrollSpyModule.forRoot()
  ],
  declarations: [
  	AppComponent
  ],
  bootstrap: [ AppComponent ]
})
```

#### Spy any element scroll

Use `ScrollSpyElementDirective` to spy on any element. You must give an unique id to each instance.
This unique id is called streamID and you need this streamID to connect your `ScrollSpyItemDirective` or your `ScrollSpyDirective`.

```js
import { NgModule, Component, Injectable, AfterViewInit } from '@angular/core';
import { ScrollSpyModule, ScrollSpyService, ScrollObjectInterface } from '@uniprank/ngx-scrollspy';

@Injectable()
@Component({
	selector: 'yourComponent',
  template: `
  <div uniScrollSpyItem="part2" stream="test">Get class active if part2 is in focus.</div>
	<div uniScrollSpyElement="test" style="max-height: 100px; overflow: auto;">
		<div uniScrollSpy="part1" stream="test" style="height: 500px;"></div>
		<div uniScrollSpy="part2" stream="test" style="height: 500px;"></div>
	</div>`
})
export class YourComponent implements AfterViewInit {

	constructor(private _scrollSpyService: ScrollSpyService) {}

	ngAfterViewInit() {
		this._scrollSpyService.getObservable('test').subscribe((element: ScrollObjectInterface) => {
			console.log('ScrollSpy::test: ', element);
		});
	}
}

@NgModule({
  imports: [
		ScrollSpyModule
  ],
  declarations: [
  	AppComponent
  ],
  providers: [ ]
})
export class YourModule { }
```

Because `ScrollSpyService` is a singleton, you can get any ScrollSpy observable from anywhere withing your application.

# TODO:

-   Finish unit tests

## License

[MIT](LICENSE)