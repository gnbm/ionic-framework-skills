# Vue Integration Patterns

Guidelines for developing Ionic applications with Vue 3.

## Setup

### Installing Ionic Vue

```bash
npm install @ionic/vue @ionic/vue-router
```

### App Setup

```typescript
import { createApp } from 'vue';
import { IonicVue } from '@ionic/vue';
import { createRouter, createWebHistory } from '@ionic/vue-router';
import App from './App.vue';

/* Core CSS required for Ionic components */
import '@ionic/vue/css/core.css';

const routes = [
  {
    path: '/',
    redirect: '/home'
  },
  {
    path: '/home',
    component: () => import('./views/Home.vue')
  }
];

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes
});

const app = createApp(App)
  .use(IonicVue)
  .use(router);

router.isReady().then(() => {
  app.mount('#app');
});
```

## Routing

### Ionic Vue Router

Ionic Vue uses Vue Router with enhanced navigation animations.

```typescript
import { createRouter, createWebHistory } from '@ionic/vue-router';

const routes = [
  {
    path: '/tabs/',
    component: () => import('./views/Tabs.vue'),
    children: [
      {
        path: '',
        redirect: '/tabs/home'
      },
      {
        path: 'home',
        component: () => import('./views/Home.vue')
      },
      {
        path: 'settings',
        component: () => import('./views/Settings.vue')
      }
    ]
  }
];

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes
});

export default router;
```

### Navigation

```vue
<template>
  <ion-button @click="navigateToDetails">Go to Details</ion-button>
  <ion-button @click="goBack">Go Back</ion-button>
</template>

<script setup lang="ts">
import { useRouter } from 'vue-router';
import { IonButton } from '@ionic/vue';

const router = useRouter();

const navigateToDetails = () => {
  router.push('/details');
};

const goBack = () => {
  router.back();
};
</script>
```

## Composition API

### Using Ionic Lifecycle Hooks

```vue
<template>
  <ion-page>
    <ion-content>
      <h1>Home</h1>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { onIonViewWillEnter, onIonViewDidEnter, onIonViewWillLeave, onIonViewDidLeave } from '@ionic/vue';
import { IonPage, IonContent } from '@ionic/vue';

onIonViewWillEnter(() => {
  console.log('ionViewWillEnter - Fired when entering, before it becomes active');
});

onIonViewDidEnter(() => {
  console.log('ionViewDidEnter - Fired when page is fully entered and active');
});

onIonViewWillLeave(() => {
  console.log('ionViewWillLeave - Fired when page is about to leave');
});

onIonViewDidLeave(() => {
  console.log('ionViewDidLeave - Fired when page has finished leaving');
});
</script>
```

**Lifecycle Order:**
1. Component setup / `onMounted()`
2. `onIonViewWillEnter()` (every time page is entered)
3. `onIonViewDidEnter()` (every time page is entered)
4. `onIonViewWillLeave()` (every time page is left)
5. `onIonViewDidLeave()` (every time page is left)
6. `onUnmounted()` (only when component is destroyed)

## Reactivity

### ref and reactive

```vue
<template>
  <ion-item>
    <ion-label position="floating">Name</ion-label>
    <ion-input v-model="name"></ion-input>
  </ion-item>

  <ion-button @click="increment">Count: {{ count }}</ion-button>

  <ion-item>
    <ion-label>{{ user.name }}</ion-label>
    <ion-note>{{ user.email }}</ion-note>
  </ion-item>
</template>

<script setup lang="ts">
import { ref, reactive } from 'vue';
import { IonItem, IonLabel, IonInput, IonButton, IonNote } from '@ionic/vue';

const name = ref<string>('');
const count = ref<number>(0);

const user = reactive({
  name: 'John Doe',
  email: 'john@example.com'
});

const increment = () => {
  count.value++;
};
</script>
```

### computed

```vue
<template>
  <p>Full Name: {{ fullName }}</p>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';

const firstName = ref('John');
const lastName = ref('Doe');

const fullName = computed(() => {
  return `${firstName.value} ${lastName.value}`;
});
</script>
```

### watch and watchEffect

```vue
<script setup lang="ts">
import { ref, watch, watchEffect } from 'vue';

const count = ref(0);
const name = ref('');

// watch specific ref
watch(count, (newValue, oldValue) => {
  console.log(`Count changed from ${oldValue} to ${newValue}`);
});

// watch multiple sources
watch([count, name], ([newCount, newName], [oldCount, oldName]) => {
  console.log('Multiple values changed');
});

// watchEffect runs immediately and re-runs when dependencies change
watchEffect(() => {
  console.log(`Current count: ${count.value}`);
});
</script>
```

## Forms

### v-model with Ionic Components

```vue
<template>
  <form @submit.prevent="handleSubmit">
    <ion-item>
      <ion-label position="floating">Email</ion-label>
      <ion-input v-model="email" type="email"></ion-input>
    </ion-item>

    <ion-item>
      <ion-label>Agree to Terms</ion-label>
      <ion-checkbox v-model="agreedToTerms"></ion-checkbox>
    </ion-item>

    <ion-item>
      <ion-label>Enable Notifications</ion-label>
      <ion-toggle v-model="notificationsEnabled"></ion-toggle>
    </ion-item>

    <ion-button expand="block" type="submit">Submit</ion-button>
  </form>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { IonItem, IonLabel, IonInput, IonCheckbox, IonToggle, IonButton } from '@ionic/vue';

const email = ref('');
const agreedToTerms = ref(false);
const notificationsEnabled = ref(false);

const handleSubmit = () => {
  console.log({ email: email.value, agreedToTerms: agreedToTerms.value, notificationsEnabled: notificationsEnabled.value });
};
</script>
```

### Form Validation

```vue
<template>
  <form @submit.prevent="handleSubmit">
    <ion-item>
      <ion-label position="floating">Email</ion-label>
      <ion-input v-model="email" type="email" @ionBlur="validateEmail"></ion-input>
    </ion-item>
    <ion-note v-if="emailError" color="danger">{{ emailError }}</ion-note>

    <ion-button expand="block" type="submit" :disabled="!isValid">Submit</ion-button>
  </form>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import { IonItem, IonLabel, IonInput, IonButton, IonNote } from '@ionic/vue';

const email = ref('');
const emailError = ref('');

const validateEmail = () => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  if (!email.value) {
    emailError.value = 'Email is required';
  } else if (!emailRegex.test(email.value)) {
    emailError.value = 'Invalid email format';
  } else {
    emailError.value = '';
  }
};

const isValid = computed(() => {
  return email.value && !emailError.value;
});

const handleSubmit = () => {
  if (isValid.value) {
    console.log('Form submitted:', email.value);
  }
};
</script>
```

## Modals

### Using modalController

```vue
<template>
  <ion-button @click="openModal">Open Modal</ion-button>
</template>

<script setup lang="ts">
import { modalController } from '@ionic/vue';
import { IonButton } from '@ionic/vue';
import ModalContent from './ModalContent.vue';

const openModal = async () => {
  const modal = await modalController.create({
    component: ModalContent,
    componentProps: {
      userId: 123
    }
  });

  await modal.present();

  const { data } = await modal.onWillDismiss();
  console.log('Modal dismissed with data:', data);
};
</script>
```

### Modal Content Component

```vue
<template>
  <ion-header>
    <ion-toolbar>
      <ion-title>Modal</ion-title>
      <ion-buttons slot="end">
        <ion-button @click="dismiss()">Close</ion-button>
      </ion-buttons>
    </ion-toolbar>
  </ion-header>
  <ion-content>
    <p>User ID: {{ userId }}</p>
  </ion-content>
</template>

<script setup lang="ts">
import { modalController } from '@ionic/vue';
import { IonHeader, IonToolbar, IonTitle, IonButtons, IonButton, IonContent } from '@ionic/vue';

interface Props {
  userId: number;
}

const props = defineProps<Props>();

const dismiss = () => {
  modalController.dismiss({
    dismissed: true
  });
};
</script>
```

### Inline Modal

```vue
<template>
  <ion-button @click="isOpen = true">Open</ion-button>

  <ion-modal :is-open="isOpen" @did-dismiss="isOpen = false">
    <ion-header>
      <ion-toolbar>
        <ion-title>Modal</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content>
      <p>Modal content</p>
      <ion-button @click="isOpen = false">Close</ion-button>
    </ion-content>
  </ion-modal>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { IonButton, IonModal, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/vue';

const isOpen = ref(false);
</script>
```

## Alerts, Toasts, and Loading

### alertController

```vue
<template>
  <ion-button @click="presentAlert">Show Alert</ion-button>
  <ion-button @click="presentConfirm">Show Confirm</ion-button>
</template>

<script setup lang="ts">
import { alertController } from '@ionic/vue';
import { IonButton } from '@ionic/vue';

const presentAlert = async () => {
  const alert = await alertController.create({
    header: 'Alert',
    message: 'This is an alert message',
    buttons: ['OK']
  });

  await alert.present();
};

const presentConfirm = async () => {
  const alert = await alertController.create({
    header: 'Confirm',
    message: 'Are you sure?',
    buttons: [
      {
        text: 'Cancel',
        role: 'cancel',
        handler: () => {
          console.log('Cancelled');
        }
      },
      {
        text: 'OK',
        handler: () => {
          console.log('Confirmed');
        }
      }
    ]
  });

  await alert.present();
};
</script>
```

### toastController

```vue
<template>
  <ion-button @click="presentToast">Show Toast</ion-button>
</template>

<script setup lang="ts">
import { toastController } from '@ionic/vue';
import { IonButton } from '@ionic/vue';

const presentToast = async () => {
  const toast = await toastController.create({
    message: 'Success!',
    duration: 2000,
    position: 'top',
    color: 'success'
  });

  await toast.present();
};
</script>
```

### loadingController

```vue
<template>
  <ion-button @click="showLoading">Show Loading</ion-button>
</template>

<script setup lang="ts">
import { loadingController } from '@ionic/vue';
import { IonButton } from '@ionic/vue';

const showLoading = async () => {
  const loading = await loadingController.create({
    message: 'Loading...',
    duration: 3000
  });

  await loading.present();
};
</script>
```

## Platform Detection

### useIonRouter

```vue
<script setup lang="ts">
import { useIonRouter } from '@ionic/vue';

const ionRouter = useIonRouter();

const checkPlatform = () => {
  if (ionRouter.canGoBack()) {
    console.log('Can go back');
  }
};
</script>
```

### Platform Service

```vue
<template>
  <div v-if="isIOS">iOS specific content</div>
  <div v-else-if="isAndroid">Android specific content</div>
  <div v-else>Desktop content</div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { getPlatforms } from '@ionic/vue';

const isIOS = ref(false);
const isAndroid = ref(false);

onMounted(() => {
  const platforms = getPlatforms();
  isIOS.value = platforms.includes('ios');
  isAndroid.value = platforms.includes('android');
});
</script>
```

## Testing

### Unit Testing with Vitest

```typescript
import { describe, it, expect } from 'vitest';
import { mount } from '@vue/test-utils';
import Home from './Home.vue';

describe('Home.vue', () => {
  it('renders home page', () => {
    const wrapper = mount(Home);
    expect(wrapper.text()).toContain('Home');
  });
});
```

### Testing with Ionic Components

```typescript
import { mount } from '@vue/test-utils';
import { IonButton } from '@ionic/vue';

it('renders button', () => {
  const wrapper = mount(IonButton, {
    slots: {
      default: 'Click Me'
    }
  });
  expect(wrapper.text()).toBe('Click Me');
});
```

## Best Practices

### Component Organization

✅ **Do:**
- Use Composition API with `<script setup>`
- Keep components small and focused
- Use TypeScript for type safety
- Extract reusable logic into composables

❌ **Don't:**
- Use Options API (prefer Composition API)
- Mix navigation libraries (use IonVueRouter)
- Forget to cleanup watchers and effects

### Performance

✅ **Do:**
- Use `v-show` for frequently toggled elements
- Use `v-if` for conditional rendering
- Use virtual scrolling for long lists
- Lazy load routes

❌ **Don't:**
- Use inline functions in templates excessively
- Use index as key in v-for
- Forget to optimize images

### Styling

✅ **Do:**
- Use scoped styles in SFCs
- Use CSS variables for theming
- Use Ionic's built-in utility classes

❌ **Don't:**
- Use deep selectors excessively (`::v-deep`, `:deep()`)
- Override Ionic component styles directly
- Use `!important` unnecessarily

## Common Patterns

### Composables (Reusable Logic)

```typescript
// composables/useFetch.ts
import { ref } from 'vue';

export function useFetch<T>(url: string) {
  const data = ref<T | null>(null);
  const loading = ref(true);
  const error = ref<Error | null>(null);

  fetch(url)
    .then(res => res.json())
    .then(json => { data.value = json; })
    .catch(err => { error.value = err; })
    .finally(() => { loading.value = false; });

  return { data, loading, error };
}

// Usage in component
const { data, loading, error } = useFetch<User[]>('/api/users');
```

### Provide/Inject for Global State

```vue
<!-- App.vue -->
<script setup lang="ts">
import { provide, ref } from 'vue';

const user = ref<User | null>(null);

const login = (newUser: User) => {
  user.value = newUser;
};

const logout = () => {
  user.value = null;
};

provide('auth', { user, login, logout });
</script>

<!-- Child component -->
<script setup lang="ts">
import { inject } from 'vue';

const auth = inject('auth');

if (auth) {
  console.log('Current user:', auth.user.value);
}
</script>
```

### Loading Data on View Enter

```vue
<script setup lang="ts">
import { ref } from 'vue';
import { onIonViewWillEnter } from '@ionic/vue';

const data = ref<any[]>([]);

onIonViewWillEnter(() => {
  loadData();
});

const loadData = async () => {
  const response = await fetch('/api/data');
  data.value = await response.json();
};
</script>
```

## Resources

- [Ionic Vue Documentation](https://ionicframework.com/docs/vue/overview)
- [Vue 3 Documentation](https://vuejs.org/)
- [Ionic Vue API Reference](https://ionicframework.com/docs/api)
