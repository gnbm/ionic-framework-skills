# Angular Integration Patterns

Guidelines for developing Ionic applications with Angular.

## Setup

### Installing Ionic Angular

```bash
npm install @ionic/angular
```

### Module Setup

```typescript
import { IonicModule } from '@ionic/angular';

@NgModule({
  imports: [
    BrowserModule,
    IonicModule.forRoot(),
    AppRoutingModule
  ]
})
export class AppModule { }
```

## Routing

### Ionic Angular Router

Ionic Angular uses Angular Router with additional navigation animations and lifecycle hooks.

```typescript
import { RouterModule } from '@angular/router';

const routes: Routes = [
  {
    path: 'home',
    loadChildren: () => import('./home/home.module').then(m => m.HomePageModule)
  }
];

@NgModule({
  imports: [RouterModule.forRoot(routes, { preloadingStrategy: PreloadAllModules })],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

### Navigation

```typescript
import { NavController } from '@ionic/angular';

constructor(private navCtrl: NavController) {}

navigateForward() {
  this.navCtrl.navigateForward('/details');
}

navigateBack() {
  this.navCtrl.back();
}
```

## Lifecycle Hooks

### Ionic Page Lifecycle

Ionic provides lifecycle hooks that complement Angular's lifecycle hooks:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-home',
  templateUrl: 'home.page.html',
})
export class HomePage {
  ionViewWillEnter() {
    console.log('ionViewWillEnter - Fired when entering a page, before it becomes active');
  }

  ionViewDidEnter() {
    console.log('ionViewDidEnter - Fired when page has fully entered and is now active');
  }

  ionViewWillLeave() {
    console.log('ionViewWillLeave - Fired when page is about to leave and no longer active');
  }

  ionViewDidLeave() {
    console.log('ionViewDidLeave - Fired when page has finished leaving');
  }
}
```

**Lifecycle Order:**
1. `ngOnInit()` - Angular lifecycle
2. `ionViewWillEnter()` - Ionic lifecycle (every time)
3. `ionViewDidEnter()` - Ionic lifecycle (every time)
4. `ionViewWillLeave()` - Ionic lifecycle (every time)
5. `ionViewDidLeave()` - Ionic lifecycle (every time)
6. `ngOnDestroy()` - Angular lifecycle (only when component destroyed)

## Forms

### Template-Driven Forms

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-form',
  template: `
    <ion-item>
      <ion-label position="floating">Name</ion-label>
      <ion-input [(ngModel)]="name" name="name"></ion-input>
    </ion-item>

    <ion-item>
      <ion-label>Toggle</ion-label>
      <ion-toggle [(ngModel)]="isEnabled" name="toggle"></ion-toggle>
    </ion-item>
  `
})
export class FormPage {
  name: string = '';
  isEnabled: boolean = false;
}
```

### Reactive Forms

```typescript
import { Component } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'app-reactive-form',
  template: `
    <form [formGroup]="myForm" (ngSubmit)="onSubmit()">
      <ion-item>
        <ion-label position="floating">Email</ion-label>
        <ion-input formControlName="email" type="email"></ion-input>
      </ion-item>

      <ion-button expand="block" type="submit" [disabled]="!myForm.valid">
        Submit
      </ion-button>
    </form>
  `
})
export class ReactiveFormPage {
  myForm: FormGroup;

  constructor(private fb: FormBuilder) {
    this.myForm = this.fb.group({
      email: ['', [Validators.required, Validators.email]]
    });
  }

  onSubmit() {
    console.log(this.myForm.value);
  }
}
```

## Modals

### Creating and Presenting Modals

```typescript
import { Component } from '@angular/core';
import { ModalController } from '@ionic/angular';
import { DetailModalPage } from './detail-modal.page';

@Component({
  selector: 'app-home',
  templateUrl: 'home.page.html'
})
export class HomePage {
  constructor(private modalCtrl: ModalController) {}

  async openModal() {
    const modal = await this.modalCtrl.create({
      component: DetailModalPage,
      componentProps: {
        userId: 123
      }
    });

    await modal.present();

    const { data } = await modal.onWillDismiss();
    console.log('Modal dismissed with data:', data);
  }
}
```

### Modal Component

```typescript
import { Component, Input } from '@angular/core';
import { ModalController } from '@ionic/angular';

@Component({
  selector: 'app-detail-modal',
  template: `
    <ion-header>
      <ion-toolbar>
        <ion-title>Detail Modal</ion-title>
        <ion-buttons slot="end">
          <ion-button (click)="dismiss()">Close</ion-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>
    <ion-content>
      <p>User ID: {{ userId }}</p>
    </ion-content>
  `
})
export class DetailModalPage {
  @Input() userId!: number;

  constructor(private modalCtrl: ModalController) {}

  dismiss() {
    this.modalCtrl.dismiss({
      dismissed: true
    });
  }
}
```

## Popovers

```typescript
import { Component } from '@angular/core';
import { PopoverController } from '@ionic/angular';
import { PopoverComponent } from './popover.component';

@Component({
  selector: 'app-page',
  templateUrl: 'page.html'
})
export class Page {
  constructor(private popoverCtrl: PopoverController) {}

  async presentPopover(event: Event) {
    const popover = await this.popoverCtrl.create({
      component: PopoverComponent,
      event: event,
      translucent: true
    });

    await popover.present();
  }
}
```

## Alerts

```typescript
import { AlertController } from '@ionic/angular';

constructor(private alertCtrl: AlertController) {}

async presentAlert() {
  const alert = await this.alertCtrl.create({
    header: 'Alert',
    subHeader: 'Subtitle',
    message: 'This is an alert message.',
    buttons: ['OK']
  });

  await alert.present();
}

async presentConfirm() {
  const alert = await this.alertCtrl.create({
    header: 'Confirm!',
    message: 'Are you sure?',
    buttons: [
      {
        text: 'Cancel',
        role: 'cancel',
        handler: () => {
          console.log('Confirm Cancel');
        }
      },
      {
        text: 'OK',
        handler: () => {
          console.log('Confirm OK');
        }
      }
    ]
  });

  await alert.present();
}
```

## Dependency Injection

### Using Ionic Services

```typescript
import { Component } from '@angular/core';
import { Platform, ToastController } from '@ionic/angular';

@Component({
  selector: 'app-root',
  templateUrl: 'app.component.html'
})
export class AppComponent {
  constructor(
    private platform: Platform,
    private toastCtrl: ToastController
  ) {
    this.initializeApp();
  }

  initializeApp() {
    this.platform.ready().then(() => {
      console.log('Platform is ready');
      this.showToast();
    });
  }

  async showToast() {
    const toast = await this.toastCtrl.create({
      message: 'App initialized',
      duration: 2000
    });
    toast.present();
  }
}
```

## Testing

### Unit Testing with Jasmine

```typescript
import { ComponentFixture, TestBed } from '@angular/core/testing';
import { IonicModule } from '@ionic/angular';
import { HomePage } from './home.page';

describe('HomePage', () => {
  let component: HomePage;
  let fixture: ComponentFixture<HomePage>;

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      declarations: [HomePage],
      imports: [IonicModule.forRoot()]
    }).compileComponents();

    fixture = TestBed.createComponent(HomePage);
    component = fixture.componentInstance;
    fixture.detectChanges();
  });

  it('should create', () => {
    expect(component).toBeTruthy();
  });
});
```

## Best Practices

### Component Organization

✅ **Do:**
- Use standalone pages for each route
- Keep components focused and single-purpose
- Use dependency injection for services
- Leverage Ionic lifecycle hooks appropriately

❌ **Don't:**
- Mix Angular and Ionic navigation methods
- Forget to unsubscribe from observables
- Use `ngOnInit` for actions that should happen every time a page is visited (use `ionViewWillEnter` instead)

### Performance

✅ **Do:**
- Use lazy loading for pages
- Use `trackBy` with `*ngFor`
- Use virtual scrolling for long lists (`ion-virtual-scroll`)
- Use ChangeDetectionStrategy.OnPush when appropriate

❌ **Don't:**
- Load all modules eagerly
- Create large templates with complex expressions
- Forget to optimize images

### Styling

✅ **Do:**
- Use CSS variables for theming
- Use Ionic's built-in utility classes
- Use Shadow DOM CSS variables for component customization

❌ **Don't:**
- Use `::ng-deep` (deprecated)
- Override Ionic component styles directly
- Use `!important` excessively

## Common Patterns

### Loading Data on Page Enter

```typescript
export class HomePage {
  data: any[] = [];

  ionViewWillEnter() {
    this.loadData();
  }

  loadData() {
    // Fetch data every time page is entered
  }
}
```

### Handling Back Button

```typescript
import { Platform } from '@ionic/angular';

constructor(private platform: Platform) {
  this.platform.backButton.subscribeWithPriority(10, () => {
    console.log('Back button pressed');
  });
}
```

### Platform-Specific Code

```typescript
import { Platform } from '@ionic/angular';

constructor(private platform: Platform) {
  if (this.platform.is('ios')) {
    // iOS-specific code
  }

  if (this.platform.is('android')) {
    // Android-specific code
  }

  if (this.platform.is('mobile')) {
    // Mobile-specific code
  }
}
```

## Resources

- [Ionic Angular Documentation](https://ionicframework.com/docs/angular/overview)
- [Angular Documentation](https://angular.io/docs)
- [Ionic Angular API Reference](https://ionicframework.com/docs/api)
