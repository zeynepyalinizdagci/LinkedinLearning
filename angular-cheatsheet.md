# Angular Cheatsheet
## 1. Basics
## 1.1. Angular Overview
- **Framework**: Angular is a platform for building web applications.
- **Written in**: TypeScript
- **Architecture**: Component-based, with modules, components, services, and directives.
## 1.2. Setting Up an Angular Project
- **Install Angular CLI**: npm install -g @angular/cli
- **Create a New Project**: ng new my-app
- **Navigate to Project**: cd my-app
- **Serve the Application**: ng serve (http://localhost:4200)
## 1.3. Basic Structure
- **Module**: app.module.ts
- **Component**: app.component.ts (HTML, CSS, TS)
- **Service**: data.service.ts
## 1.4. Creating Components
- **Generate Component**: ng generate component component-name
- **Basic Structure**:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-component',
  templateUrl: './component.component.html',
  styleUrls: ['./component.component.css']
})
export class ComponentName { }
```
## 1.5. Data Binding
- Interpolation: {{ expression }}
- Property Binding: [property]="expression"
- Event Binding: (event)="method()"
- Two-Way Binding: [(ngModel)]="property"
## 1.6. Directives
- Structural Directives: *ngIf, *ngFor, *ngSwitch
- Attribute Directives: [ngClass], [ngStyle]
## 1.7. Services and Dependency Injection
- Generate Service: ng generate service service-name
- Basic Service:

```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class MyService { }
```
## 2. Intermediate Concepts
## 2.1. Routing
- RouterModule: Import RouterModule in AppModule.
- Define Routes:

```typescript
const routes: Routes = [
  { path: 'home', component: HomeComponent },
  { path: 'about', component: AboutComponent },
  { path: '', redirectTo: '/home', pathMatch: 'full' }
];
```
- RouterOutlet: <router-outlet></router-outlet>
- RouterLink: <a routerLink="/about">About</a>
## 2.2. Forms
- Template-Driven Forms: Use FormsModule, ngModel.
- Reactive Forms: Use ReactiveFormsModule, FormGroup, FormControl.

```typescript
import { FormBuilder, FormGroup } from '@angular/forms';

export class MyComponent {
  myForm: FormGroup;

  constructor(private fb: FormBuilder) {
    this.myForm = this.fb.group({
      name: ['']
    });
  }
}
```
## 2.3. Pipes
- Built-in Pipes: DatePipe, CurrencyPipe, DecimalPipe, JsonPipe
- Custom Pipe:

```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'customPipe'
})
export class CustomPipe implements PipeTransform {
  transform(value: string): string {
    return value.toUpperCase();
  }
}
```
## 2.4. Lifecycle Hooks
- Common Hooks: ngOnInit(), ngOnChanges(), ngOnDestroy()

```typescript
export class MyComponent implements OnInit, OnDestroy {
  ngOnInit() { }
  ngOnDestroy() { }
}
```
## 3. Advanced Topics
## 3.1. Angular Modules
- Feature Modules: Create feature-specific modules to organize code.

```typescript
@NgModule({
  declarations: [FeatureComponent],
  imports: [CommonModule],
  exports: [FeatureComponent]
})
export class FeatureModule { }
```
## 3.2. Lazy Loading
- Configure Lazy Loading: Define in routing module.

```typescript
const routes: Routes = [
  {
    path: 'feature',
    loadChildren: () => import('./feature/feature.module').then(m => m.FeatureModule)
  }
];
```
## 3.3. State Management
- NgRx: Use for state management.
- Actions: Define actions.
- Reducers: Handle state changes.
- Selectors: Access state.
- Effects: Handle side effects.
## 3.4. HTTP Client
- HttpClient: Use for making HTTP requests.

```typescript
import { HttpClient } from '@angular/common/http';

export class MyService {
  constructor(private http: HttpClient) { }

  getData() {
    return this.http.get('api/data');
  }
}
```
## **3.5. Angular Universal (Server-Side Rendering)**
- Setup: ng add @nguniversal/express-engine
- Run: npm run build:ssr and npm run serve:ssr

## **3.6. Performance Optimization**
- Change Detection Strategy: OnPush

```typescript
@Component({
  selector: 'app-component',
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class ComponentName { }
```
- TrackBy Function: Improve *ngFor performance.
```typescript
trackById(index: number, item: any): number { return item.id; }
```
## 4 Testing
## 4.1 Frameworks
- TestBed
## Sample Usage of TestBed
- Component Test:
```Typescript
import { HttpClient } from '@angular/common/http';
import { HttpClientTestingModule } from '@angular/common/http/testing';
import { TestBed } from '@angular/core/testing';
import { RouterTestingModule } from '@angular/router/testing';
import { AppComponent } from './app.component';
import { MessageService } from './services/message.service';

describe('AppComponent', () => {
  beforeEach(async () => {
    await TestBed.configureTestingModule({
      imports: [RouterTestingModule, HttpClientTestingModule],
      declarations: [AppComponent],
      providers: [MessageService, HttpClient]
    }).compileComponents();
  });

  it('should create the app', () => {
    const fixture = TestBed.createComponent(AppComponent);
    const app = fixture.componentInstance;
    expect(app).toBeTruthy();
  });
});


```
- Service Test:
```Typescript
import { TestBed } from '@angular/core/testing';

import { AnalyticsService } from './analytics.service';

describe('AnalyticsService', () => {
  let service: AnalyticsService;

  beforeEach(() => {
    TestBed.configureTestingModule({});
    service = TestBed.inject(AnalyticsService);
  });

  it('should be created', () => {
    expect(service).toBeTruthy();
  });
});

```
- Apicall Test:
```Typescript
import { HttpClient } from '@angular/common/http';
import { HttpClientTestingModule } from '@angular/common/http/testing';
import { TestBed } from '@angular/core/testing';
import { of } from 'rxjs';
import { Player } from '../interfaces/player';
import { ApiService } from './api.service';

describe('ApiService', () => {
  let httpClient: HttpClient;
  let service: ApiService;

  const EXPECTED_PLAYERS_LIST: Player[] = [{
    gems: ['Diamond'],
    id: 'abc123',
    name: 'Abc Xyz',
    online: true,
    score: 0.12345
  }];

  beforeEach(() => {
    TestBed.configureTestingModule({
      declarations: [],
      imports: [HttpClientTestingModule],
      providers: [ApiService, HttpClient]
    });

    httpClient = TestBed.inject(HttpClient);
    service = TestBed.inject(ApiService);

    spyOn(httpClient, 'get').and.returnValue(of(EXPECTED_PLAYERS_LIST));
  });

  describe('getAllPlayers$()', () => {
    it('should get all players', (done: DoneFn) => {
      service.getAllPlayers$().subscribe(actual => {
        expect(actual).toEqual(EXPECTED_PLAYERS_LIST);
        done();
      });
    });
  });

  describe('getPlayerById$()', () => {
    it('should get a player by ID', (done: DoneFn) => {
      service.getPlayerById$('abc123').subscribe(actual => {
        expect(actual).toEqual(EXPECTED_PLAYERS_LIST.at(0));
        done();
      });
    });

    it('should return undefined if no player is found', (done: DoneFn) => {
      service.getPlayerById$('does-not-exist').subscribe(actual => {
        expect(actual).toEqual(undefined);
        done();
      });
    });
  });

  describe('getPlayersByName$()', () => {
    it('should get players by Name', (done: DoneFn) => {
      service.getPlayersByName$('abc').subscribe(actual => {
        expect(actual).toEqual(EXPECTED_PLAYERS_LIST);
        done();
      });
    });

    it('should return an empty array if no players are found', (done: DoneFn) => {
      service.getPlayersByName$('does-not-exist').subscribe(actual => {
        expect(actual).toEqual([]);
        done();
      });
    });
  });
});

```
**see other sample test code in src/app**
