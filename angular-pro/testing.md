### init test environment 

included to general test.ts file

TestBed.initTestEnvironment(
  BrowserDynamicTestingModule,
  platformBrowserDynamicTesting()
)

## shallow testing Pipes

```
describe('Shallow FileSizePipe test', () => {
    @Component({
      selector: 'comp',
      template: `
        size: {{ size | filesize:suffix }}
      `
    })
    class TestComponent {
      suffix: any;
      size:number = 123456789;
    }

    let component: TestComponent;
    let fixture: ComponentFixture<TestComponent>;
    let el: HTMLElement;

    beforeEach(() => {
      TestBed.configureTestingModule({
        declarations: [
          FileSizePipe,
          TestComponent
        ],
      });
      fixture = TestBed.createComponent(TestComponent);
      component = fixture.componentInstance;
      el = fixture.nativeElement;
    })

    it('should convert bytes to megabytes', () => {
      fixture.detectChanges();
      expect(el.textContent).toBe(' size: 117.74MB ');
      component.size = 1029281;
      fixture.detectChanges();
      expect(el.textContent).toBe(' size: 0.98MB ');
    })
  })
```