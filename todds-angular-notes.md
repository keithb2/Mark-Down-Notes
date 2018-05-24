# Angular Notes

## Component
- `template` (view)
- `class` (logic)
- `decorator` (metadata)

## Class
- Use PascalCasing
- Append Component to the class name
- always export
- data elements defined as properties - typescript = strong typing
- properties camelCase
- logic goes in methods - use camelCase

## MetaData (Decorator)
- component decorator - prefix with @ suffix with ()
- selector: component name in html (if using a directive)
- template: view's html 

import what is needed for the component

```javascript
import { Module } from './path' - no file extension
```

## Templates
- inline - for shorter Templates - specified in template proprty in decorator - can use back ticks for multi-line 
- linked temaplates - for longer templates - specified in the templateUrl property in decorator - define the path 

## Directives
- directive as element in template e.g., `<pm-products></pm-products>`
- set selector element in decoartor e.g., selector: 'pm-products'
- declare component in module in declarations array 

## Interpolation
- one way binding from component class property to element property 
- implemented with `{{template expression}}` --> no quotes needed 

## Structural directives:
- assign to quoted string expression 
- `*ngIf` (add/remove element from dom based on expression) 
- `*ngFor`  (repeat each element in the dom for an element and its children)
```javascript
let product of products
```
**safe navigation**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;can use: 
```javascript
product?.productName //will not explode if this is null
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;better to use:

```javascript
<div class='something' &ngIf='product'> // this will not throw undefined error
```

## Data binding 
**Interploation** - `{{}}` -> one way data binding from component to DOM.
```javascript
{{pageTitle}}
```

**Property binding** - `[]` -> sets element property to value of template expression. 
```javascript
<img [src]='product.imageUrl'>
```

**Event binding** - `()` -> listens for events. 
```javascript
<button (click)='toggleImage()'>
```

**Two-way binding** - `[()]` -> displays property and updates property when the user makes a change. Use with ngModel directive.
```javascript
<input [(ngModel)]='listFilter'>
```

**Using ngModel**
- use `[(ngModel)]` syntax in template
- import FormsModule in app module

## Pipes
- transform data to more user friendly format
- use the pipe character `|` and the name of the pipe and any pipe parameters seperated with colons
-example: 
```javascript
{{product.price | currency:'USD':true:'1.2-2'}}
```

## Interfaces
- define custom type 
- promotes strong typing in the app 
- use `Interface` keyword and export it 
- use the `implements` keyword to use interface 
- define all needed properties and methods

## Custom Pipes
- import `Pipe` and `PipeTransform` 
- create class `implements PipeTransform` - has one method `transform`
- export the class 
- write code in `transform` method for the transformation 
- decorate class with the `Pipe` decorator 
- using the custom pipe:
    - import the custom pipe 
    - add pipe to declarations array of the angular module 
    - use the pipe in the template 

## Encapsulate styles
- use styles property array in component decorator for list of styles
- use styleUrls property array in component decorator for list of external style sheet paths

## Nested components (see StarComponent / ProductListComponent for example)
- Input decorator attached to any type - prefix with `@` and suffix with `()` 
- Output decorator send back to parent component 
    - attached to any property and declared as en EventEmitter - use generic for type for payload type 
    - use new to create an instance of the `EventEmitter`
    - prefix with `@` and suffix with `()`
- In the conatiner component, use the nested component as a directive 
     - directive name - see the nested component's selector 
- use property binding to pass data to the nested component 
- use event binding to respond to events from the nested component 
- use `$event` to access the event payload passed from the nested component

## Services and Dependency Injection
### Creating a Service
- Create the service class - PascalCasing, append Service to the class name, use export 
- Service Decorator - Use `Injectable` - prefix with `@` and suffix with `()`
- import what you need 

Register the service in a component - use `providers` property in component's metadata 

Specify the service as a dependency with constructor parameter

## Http Service 
- add `HttpClientModule` to `imports` array of one of the app's angular modules 
- create the service 
    - import what you need 
    - define dependency for the http client service - using a constructor parameter 
    - create method for each http request 
    - call http method like `get()` - pass in the url - use generics to specify the return type 
    - add error handling
- subscribe
    - call subscribe method to subscribe to the `observable`
    - provide a function to handle an emitted item - normally assigns a property to the returned JSON object 
    - provide error function to handle any returned errors

## Routing 
- Nestable components - does not need a route 
    - define selector 
    - nest in another component 
- Routed components 
    - no selector 
    - configure routes 
    - tie routes to actions 
    - place the view 
- Configure routes 
    - define base element in index.html file 
    - add `RouterModule` to an angular module's import array 
    - add each route to the array passed to `RouterModule.forRoot` -> remember that order matters, wild cards should be last
- routes require 
    1. path:
        1. url segment for the route -> no leading slash 
        2. use `''` for default route 
        3. use `'**'` for wildcard route 
    2. component: Not string name, not enclosed in quotes 
- Tying routes to actions 
    - add `RouterLink` directive as an attribute - clickable element - enclose is square brackets 
    - bind a link to link paramters array - first element in the path, all other paramters are route parameters 
- Placing the view 
    - add `RouterOutlet` directive - identifies where to place the component's view - specified in the host component's template 
- Passing paramters:
    - pass paramter value: 
    ```html
    <a [routerLink]="['/products', productList.productId]">{{product.productName}}</a>`
    ```
    - read the passed parameter in the component:
    ```javascript
            import { AvtivatedRoute } from '@angular/core'
            constructor (private _route: ActivatedRoute){
                console.log(this._route.snapshot.paramMap.get('id);)
            }
    ```
    - Route
    ```javascript
    { path: 'products/:id', component: ProductDetailComponent }
    ```
- Activate a route with code
    - Use the Router service
        1. import the service 
        2. define as a dependency 
        3. create method that calls the navigate method of the Router service - pass in the link paramters array 
        4. add a user interface element - use event binding to call the created method
- Protect routes with guards 
    - build Guard Service - implement the guard type - `CanActivate` for example 
    - create the associated method - `canActivate()`
    - Register the guard service provider in an angular module 
    - add the guard to the desired route 

## Module structure
- every app has to have a root application module (`App.module`) - bootstraps `AppComponent`
- Feature module - define a module for each feature set - e.g., ProductModule, CustomerModule, InvoiceModule, etc 
    - organizes and separates concerns 
- Shared Modules - exports common pieces 
- Core Module - can use to create services shared across modules - import only once - in root module 
    - have providers, none of which are exported 
- Routing Module 
    - refator routes 
- When creating an Angular module
    - create a class and decorate it with `NgModule` decorator 
        1. bootstrap array - start up components 
        2. declarations - components, etc that belong 
        3. exports - defines what an importing module can use 
        4. imports - supporting modules 
        5. providers - service providers 

## Angular CLI:
### install the Angular CLI
```javascript
    npm install -g @angular/cli
```

### Create New Empty Project
```javascript
ng new project-name
```
- `--prefix (default prefix here)` -> sets the default prefix
- `--routing` -> create default routing module

### Start an Angular App
- `ng serve` -> starts app 
- `ng start` -> alias to ng serve (set in package.json)
- update `package.json`, set `ng start` alias to `ng serve -o` -> will launch browser

### Generate Things
- `ng generate` --> creates class, components, modules, directives, enums, guards,  etc 
- Examples
    - `ng g c products/product-deatil.component --flat` --> will create new component --flat will not create it in a subfolder
    - `ng g m product/product --flat -m app.module`  --> will create new module --flat will not create it in a subfolder, -m will import into specified module) 

### Testing
- `ng test` --> runs unit tests 
- `ng e2e` --> runs end to end tests

### Building
- `ng build` --> builds app for deployment in dist folder 
- `ng build --prod` -> builds app for deployment to prod (minify/uglify code, tree shake, pre-compile)
- `--base-href` --> sets base url for app 