# React Integration Patterns

Guidelines for developing Ionic applications with React.

## Setup

### Installing Ionic React

```bash
npm install @ionic/react @ionic/react-router
```

### App Setup

```tsx
import React from 'react';
import { IonApp, IonRouterOutlet, setupIonicReact } from '@ionic/react';
import { IonReactRouter } from '@ionic/react-router';
import { Route } from 'react-router-dom';

/* Core CSS required for Ionic components */
import '@ionic/react/css/core.css';

setupIonicReact();

const App: React.FC = () => (
  <IonApp>
    <IonReactRouter>
      <IonRouterOutlet>
        <Route exact path="/home" component={Home} />
        <Route exact path="/" render={() => <Redirect to="/home" />} />
      </IonRouterOutlet>
    </IonReactRouter>
  </IonApp>
);

export default App;
```

## Routing

### Ionic React Router

Ionic React uses React Router with enhanced navigation animations.

```tsx
import { IonReactRouter } from '@ionic/react-router';
import { IonTabs, IonTabBar, IonTabButton, IonIcon, IonLabel } from '@ionic/react';
import { home, settings } from 'ionicons/icons';

const App: React.FC = () => (
  <IonApp>
    <IonReactRouter>
      <IonTabs>
        <IonRouterOutlet>
          <Route exact path="/home" component={Home} />
          <Route exact path="/settings" component={Settings} />
        </IonRouterOutlet>

        <IonTabBar slot="bottom">
          <IonTabButton tab="home" href="/home">
            <IonIcon icon={home} />
            <IonLabel>Home</IonLabel>
          </IonTabButton>
          <IonTabButton tab="settings" href="/settings">
            <IonIcon icon={settings} />
            <IonLabel>Settings</IonLabel>
          </IonTabButton>
        </IonTabBar>
      </IonTabs>
    </IonReactRouter>
  </IonApp>
);
```

### Navigation

```tsx
import { useHistory } from 'react-router-dom';

const MyComponent: React.FC = () => {
  const history = useHistory();

  const navigateToDetails = () => {
    history.push('/details');
  };

  const goBack = () => {
    history.goBack();
  };

  return (
    <IonButton onClick={navigateToDetails}>Go to Details</IonButton>
  );
};
```

## Lifecycle Hooks

### Ionic Page Lifecycle with React Hooks

```tsx
import { IonPage, IonContent, useIonViewWillEnter, useIonViewDidEnter, useIonViewWillLeave, useIonViewDidLeave } from '@ionic/react';

const Home: React.FC = () => {
  useIonViewWillEnter(() => {
    console.log('ionViewWillEnter - Fired when entering, before it becomes active');
  });

  useIonViewDidEnter(() => {
    console.log('ionViewDidEnter - Fired when page is fully entered and active');
  });

  useIonViewWillLeave(() => {
    console.log('ionViewWillLeave - Fired when page is about to leave');
  });

  useIonViewDidLeave(() => {
    console.log('ionViewDidLeave - Fired when page has finished leaving');
  });

  return (
    <IonPage>
      <IonContent>
        <h1>Home</h1>
      </IonContent>
    </IonPage>
  );
};
```

**Lifecycle Order:**
1. Component mount / `useEffect` runs
2. `useIonViewWillEnter()` (every time page is entered)
3. `useIonViewDidEnter()` (every time page is entered)
4. `useIonViewWillLeave()` (every time page is left)
5. `useIonViewDidLeave()` (every time page is left)
6. Component unmount / `useEffect` cleanup (only when component is destroyed)

## State Management

### useState Hook

```tsx
import React, { useState } from 'react';
import { IonButton, IonItem, IonInput } from '@ionic/react';

const MyComponent: React.FC = () => {
  const [name, setName] = useState<string>('');
  const [count, setCount] = useState<number>(0);

  return (
    <>
      <IonItem>
        <IonInput value={name} onIonChange={e => setName(e.detail.value!)} />
      </IonItem>
      <IonButton onClick={() => setCount(count + 1)}>
        Count: {count}
      </IonButton>
    </>
  );
};
```

### useReducer Hook

```tsx
import React, { useReducer } from 'react';

interface State {
  count: number;
}

type Action =
  | { type: 'increment' }
  | { type: 'decrement' }
  | { type: 'reset' };

const reducer = (state: State, action: Action): State => {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    case 'reset':
      return { count: 0 };
    default:
      return state;
  }
};

const Counter: React.FC = () => {
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <div>
      <p>Count: {state.count}</p>
      <IonButton onClick={() => dispatch({ type: 'increment' })}>+</IonButton>
      <IonButton onClick={() => dispatch({ type: 'decrement' })}>-</IonButton>
      <IonButton onClick={() => dispatch({ type: 'reset' })}>Reset</IonButton>
    </div>
  );
};
```

## Forms

### Controlled Components

```tsx
import React, { useState, FormEvent } from 'react';
import { IonItem, IonLabel, IonInput, IonToggle, IonButton } from '@ionic/react';

const FormComponent: React.FC = () => {
  const [email, setEmail] = useState<string>('');
  const [isEnabled, setIsEnabled] = useState<boolean>(false);

  const handleSubmit = (e: FormEvent) => {
    e.preventDefault();
    console.log({ email, isEnabled });
  };

  return (
    <form onSubmit={handleSubmit}>
      <IonItem>
        <IonLabel position="floating">Email</IonLabel>
        <IonInput
          type="email"
          value={email}
          onIonChange={e => setEmail(e.detail.value!)}
        />
      </IonItem>

      <IonItem>
        <IonLabel>Enable</IonLabel>
        <IonToggle
          checked={isEnabled}
          onIonChange={e => setIsEnabled(e.detail.checked)}
        />
      </IonItem>

      <IonButton expand="block" type="submit">Submit</IonButton>
    </form>
  );
};
```

## Modals

### Using useIonModal Hook

```tsx
import React, { useState } from 'react';
import { IonButton, IonContent, IonHeader, IonTitle, IonToolbar, useIonModal } from '@ionic/react';

interface ModalProps {
  onDismiss: (data?: string, role?: string) => void;
  userId: number;
}

const ModalContent: React.FC<ModalProps> = ({ onDismiss, userId }) => {
  return (
    <>
      <IonHeader>
        <IonToolbar>
          <IonTitle>Modal</IonTitle>
        </IonToolbar>
      </IonHeader>
      <IonContent>
        <p>User ID: {userId}</p>
        <IonButton onClick={() => onDismiss('confirmed', 'confirm')}>
          Dismiss
        </IonButton>
      </IonContent>
    </>
  );
};

const Page: React.FC = () => {
  const [present, dismiss] = useIonModal(ModalContent, {
    userId: 123,
    onDismiss: (data: string, role: string) => dismiss(data, role),
  });

  const openModal = () => {
    present({
      onWillDismiss: (ev: CustomEvent) => {
        console.log('Modal dismissed with data:', ev.detail);
      },
    });
  };

  return <IonButton onClick={openModal}>Open Modal</IonButton>;
};
```

### Inline Modal

```tsx
import React, { useState, useRef } from 'react';
import { IonModal, IonButton, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/react';

const ModalExample: React.FC = () => {
  const [isOpen, setIsOpen] = useState(false);
  const modal = useRef<HTMLIonModalElement>(null);

  return (
    <>
      <IonButton onClick={() => setIsOpen(true)}>Open</IonButton>
      <IonModal ref={modal} isOpen={isOpen} onDidDismiss={() => setIsOpen(false)}>
        <IonHeader>
          <IonToolbar>
            <IonTitle>Modal</IonTitle>
          </IonToolbar>
        </IonHeader>
        <IonContent>
          <p>Modal content</p>
          <IonButton onClick={() => setIsOpen(false)}>Close</IonButton>
        </IonContent>
      </IonModal>
    </>
  );
};
```

## Alerts, Toasts, and Loading

### useIonAlert Hook

```tsx
import { useIonAlert } from '@ionic/react';

const AlertExample: React.FC = () => {
  const [presentAlert] = useIonAlert();

  const showAlert = () => {
    presentAlert({
      header: 'Alert',
      message: 'This is an alert message',
      buttons: ['OK'],
    });
  };

  const showConfirm = () => {
    presentAlert({
      header: 'Confirm',
      message: 'Are you sure?',
      buttons: [
        {
          text: 'Cancel',
          role: 'cancel',
        },
        {
          text: 'OK',
          handler: () => {
            console.log('Confirmed');
          },
        },
      ],
    });
  };

  return (
    <>
      <IonButton onClick={showAlert}>Show Alert</IonButton>
      <IonButton onClick={showConfirm}>Show Confirm</IonButton>
    </>
  );
};
```

### useIonToast Hook

```tsx
import { useIonToast } from '@ionic/react';

const ToastExample: React.FC = () => {
  const [present] = useIonToast();

  const showToast = () => {
    present({
      message: 'Success!',
      duration: 2000,
      position: 'top',
      color: 'success',
    });
  };

  return <IonButton onClick={showToast}>Show Toast</IonButton>;
};
```

### useIonLoading Hook

```tsx
import { useIonLoading } from '@ionic/react';

const LoadingExample: React.FC = () => {
  const [present, dismiss] = useIonLoading();

  const showLoading = async () => {
    await present({
      message: 'Loading...',
      duration: 3000,
    });
  };

  return <IonButton onClick={showLoading}>Show Loading</IonButton>;
};
```

## Platform Detection

### useIonPlatform Hook

```tsx
import { useIonPlatform } from '@ionic/react';

const PlatformExample: React.FC = () => {
  const platform = useIonPlatform();

  if (platform.is('ios')) {
    return <p>iOS specific content</p>;
  }

  if (platform.is('android')) {
    return <p>Android specific content</p>;
  }

  if (platform.is('mobile')) {
    return <p>Mobile specific content</p>;
  }

  return <p>Desktop content</p>;
};
```

## Testing

### Unit Testing with React Testing Library

```tsx
import { render, screen } from '@testing-library/react';
import '@testing-library/jest-dom';
import Home from './Home';

test('renders home page', () => {
  render(<Home />);
  const headingElement = screen.getByText(/Home/i);
  expect(headingElement).toBeInTheDocument();
});
```

### Testing with Ionic Components

```tsx
import { render } from '@testing-library/react';
import { IonButton } from '@ionic/react';

test('renders button', () => {
  const { getByText } = render(<IonButton>Click Me</IonButton>);
  expect(getByText('Click Me')).toBeInTheDocument();
});
```

## Best Practices

### Component Organization

✅ **Do:**
- Use functional components with hooks
- Keep components small and focused
- Use TypeScript for type safety
- Extract reusable logic into custom hooks

❌ **Don't:**
- Use class components (prefer functional components)
- Mix navigation libraries (use IonReactRouter)
- Forget to cleanup effects and subscriptions

### Performance

✅ **Do:**
- Use React.memo for expensive components
- Use useCallback and useMemo to prevent unnecessary re-renders
- Use virtual scrolling for long lists (`IonVirtualScroll`)
- Lazy load routes

❌ **Don't:**
- Create inline functions in JSX (causes re-renders)
- Use index as key in lists
- Forget to optimize images

### Styling

✅ **Do:**
- Use CSS variables for theming
- Use CSS Modules or styled-components
- Use Ionic's built-in utility classes

❌ **Don't:**
- Use inline styles excessively
- Override Ionic component styles directly
- Use `!important` unnecessarily

## Common Patterns

### Loading Data on Mount

```tsx
import { useState, useEffect } from 'react';

const DataPage: React.FC = () => {
  const [data, setData] = useState<any[]>([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetchData();
  }, []);

  const fetchData = async () => {
    try {
      const response = await fetch('/api/data');
      const json = await response.json();
      setData(json);
    } finally {
      setLoading(false);
    }
  };

  if (loading) return <IonSpinner />;

  return <IonList>{/* Render data */}</IonList>;
};
```

### Custom Hooks

```tsx
import { useState, useEffect } from 'react';

function useFetch<T>(url: string) {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);

  useEffect(() => {
    fetch(url)
      .then(res => res.json())
      .then(setData)
      .catch(setError)
      .finally(() => setLoading(false));
  }, [url]);

  return { data, loading, error };
}

// Usage
const MyComponent: React.FC = () => {
  const { data, loading, error } = useFetch<User[]>('/api/users');

  if (loading) return <IonSpinner />;
  if (error) return <p>Error: {error.message}</p>;

  return <IonList>{/* Render data */}</IonList>;
};
```

### Context for Global State

```tsx
import React, { createContext, useContext, useState, ReactNode } from 'react';

interface AuthContextType {
  user: User | null;
  login: (user: User) => void;
  logout: () => void;
}

const AuthContext = createContext<AuthContextType | undefined>(undefined);

export const AuthProvider: React.FC<{ children: ReactNode }> = ({ children }) => {
  const [user, setUser] = useState<User | null>(null);

  const login = (user: User) => setUser(user);
  const logout = () => setUser(null);

  return (
    <AuthContext.Provider value={{ user, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
};

export const useAuth = () => {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within AuthProvider');
  }
  return context;
};
```

## Resources

- [Ionic React Documentation](https://ionicframework.com/docs/react/overview)
- [React Documentation](https://react.dev/)
- [Ionic React API Reference](https://ionicframework.com/docs/api)
