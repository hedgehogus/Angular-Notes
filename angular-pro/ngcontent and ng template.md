## Content projection with ng-content

```
<ng-content select="h3"></ng-content>    // works like query selector

<ng-content select=".remember"></ng-content>    // bind by class

```

- get access to the projected content:

```
@ViewChild('email') email: ElementRef;

@ViewChild(AuthMessageComponent) message: AuthMessageComponent;
@ViewChildren(AuthMessageComponent) remember: QueryList<AuthMessageComponent>;

@ContentChild(AuthRememberComponent) remember: AuthRememberComponent;
@ContentChildren(AuthRememberComponent) rememberList: QueryList<AuthRememberComponent>;

constructor(private _cd: ChangeDetectorRef) {}

ngAfterViewInit() {
  this.email.nativeElement.setAttribute('placeholder', 'enter your email address');
  
  if (this.message) {
    this.message.forEach((message) => {  
      message.days = 30;
    }
  }
  this._cd.detectChanges();
}

ngAfterContentInit() {
  if (this.remember){
    this.remember.checked.subscribe((checked: boolean) => {
      this.showMessage = checked;
    });
  }
  
  if (this.rememberList){
    this.remember.forEach((item) => {
      item.checked.subscribe((checked: boolean) => {
        this.showMessage = checked;
      });
    }
  }
}
```

## using Renderer

Renderer is **plaform agnostic**. You can use the same code on web and mobile.

```
constructor(private _renderer: Renderer) {}

ngAfterViewInit() {
  this.renderer.setElementAttribute(this.email.nativeElement, 'placeholder', 'enter your email address');
}
```

## Dynamic components with ComponentFactoryResolver // deprecated
Since 9.0.0 with Ivy, the entryComponents property is no longer necessary

```
@ViewChild('entry', {read: ViewContainerRef }) entry: ViewContainerRef;

constructor(private resolver: ComponentFactoryResolver) {
}

ngAfterContentInit() {
  const authFormFactory = this.resolver.resolveComponentFactory(AuthFormComponent);
  const component = this.entry.createComponent(authFormFactory)
}

moveComponent() {
  this.entry.move(this.component.hostView, 1);
}

```
Using this approach you need to add AuthFormComponent to entryComponents array

**Current approach:**
https://angular.io/guide/dynamic-component-loader

```
const componentRef = viewContainerRef.createComponent<AdComponent>(adItem.component);
this.ref = componentRef;

componentRef.instance.data = adItem.data;

componentRef.instance.submitted.subscribe((a) => console.log(a));

this.ref.destroy();

```

## NgTemplateOutlet

- NgTemplateOutlet
- NgTemplateOutletContext

```
  <ng-container *ngTemplateOutlet="svk; context: myContext"></ng-container>
  
  <ng-template #svk let-person="localSk"><span>Ahoj {{person}}!</span></ng-template>
```

## View Encapsulation

enum ViewEncapsulation {
  Emulated: 0           //default for angular style
  None: 2               
  ShadowDom: 3          // creates a real new dom inside a component
}

