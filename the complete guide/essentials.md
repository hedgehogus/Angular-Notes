## generate component
ng g c user

## create custom two way binding, not only for forms

without signals:

```
@Input() size: number
@Output() sizeChange: number
```

using signals

```
size = model();
//or
size = model.required();
```

## get rid of zone.js
**angular.json** -> remove "zone.js"
**main.ts** -> change to 
```
bootstrapApplication(AppComponent{
  providers: [provideExperimentalZonelessChangeDetection()]
}).catch((err) => console.error(err));
```

## Building for production

*ng build || npm run build*

### Single Page Application (SPA)
- Build a client-side only web application
- All code executes in the browser
- No dynamic web server needed - a static host suffices
- **disadvantages**: Initially missing content, bad SEO

#### Deployment with angular CLI
https://angular.io/guide/deployment

### Server Side Renderend App (SSR)
- Angular app routes are rendered on-demand on a dynamic web server
- Browser receives finished, rendered page
- Web app is hydrated ("activated") and becomes a SPA after initial rendering
- Dynamic web server is required
- **disadvantages**: Long-taking tasks may cause empty pages, complexity

*ng add @angular/ssr*

https://angular.dev/guide/ssr

*npm run serve:ssr:routing*

**fix localstorage problem with ssr (and other browser side features)**

```
constructor() {
    afterNextRender(() => {
      const tasks = localStorage.getItem('tasks');

      if (tasks) {
        this.tasks.set(JSON.parse(tasks));
      }
    });
  }
```
### Static Site Generation (SSG)
- Angular app routes are pre-rendered at build time
- Browser receives finished, rendered pages
- Web app is hydrated ("activated") and becomes a SPA 
- Dynamic web server is required - static host suffices if ALL pages are pre-rendered
- **disadvantages**: No dynamic server-side data fetching

*ng add @angular/ssr*

-- add new file (user-routes.txt) and write all dynamic routs that should be prerendered
```
/users/u1/tasks
/users/u2/tasks
```

-- angular.json file -> change "prerender": true  - to
```
"prerender": {
  "routesFile": "user-routes.txt"
},
```
