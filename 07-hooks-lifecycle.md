# 7. React Hooks & Lifecycle

Understanding React Hooks and component lifecycle is crucial for building React applications. In this section, we'll learn about hooks and how they relate to component lifecycle in the context of our Airbnb clone.

## What are React Hooks?

Hooks are functions that let you "hook into" React features like state and lifecycle from function components. They were introduced in React 16.8.

## Common Hooks We'll Use

### 1. useState Hook

Manages component state.

**Syntax:**
```javascript
const [state, setState] = useState(initialValue)
```

**Example:**
```javascript
import { useState } from 'react'

function Counter() {
  const [count, setCount] = useState(0)

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  )
}
```

**In Our Project:**
```javascript
function Home() {
  const [properties, setProperties] = useState([])
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState(null)
  // ...
}
```

### 2. useEffect Hook

Handles side effects (API calls, subscriptions, DOM manipulation).

**Syntax:**
```javascript
useEffect(() => {
  // Side effect code
  return () => {
    // Cleanup (optional)
  }
}, [dependencies])
```

**Example - Fetching Data:**
```javascript
import { useState, useEffect } from 'react'

function PropertyList() {
  const [properties, setProperties] = useState([])

  useEffect(() => {
    // This runs after component mounts
    fetchProperties().then(data => setProperties(data))
  }, []) // Empty array = runs once on mount

  return <div>{/* Render properties */}</div>
}
```

**Example - With Dependencies:**
```javascript
function PropertyDetail({ propertyId }) {
  const [property, setProperty] = useState(null)

  useEffect(() => {
    // Runs when propertyId changes
    fetchPropertyById(propertyId).then(setProperty)
  }, [propertyId]) // Re-runs when propertyId changes

  return <div>{/* Render property */}</div>
}
```

**Example - Cleanup:**
```javascript
useEffect(() => {
  const intervalId = setInterval(() => {
    console.log('Tick')
  }, 1000)

  // Cleanup function
  return () => {
    clearInterval(intervalId)
  }
}, [])
```

### 3. useContext Hook

Accesses context values.

**Example:**
```javascript
import { createContext, useContext } from 'react'

const ThemeContext = createContext()

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Component />
    </ThemeContext.Provider>
  )
}

function Component() {
  const theme = useContext(ThemeContext)
  return <div>Theme: {theme}</div>
}
```

### 4. useRef Hook

Creates mutable references that persist across renders.

**Example:**
```javascript
import { useRef } from 'react'

function SearchInput() {
  const inputRef = useRef(null)

  const focusInput = () => {
    inputRef.current.focus()
  }

  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={focusInput}>Focus Input</button>
    </div>
  )
}
```

## Component Lifecycle with Hooks

In class components, we had lifecycle methods. With hooks, we use `useEffect` to replicate this behavior.

### Lifecycle Mapping

| Class Component | Hook Equivalent |
|----------------|-----------------|
| `componentDidMount` | `useEffect(() => {}, [])` |
| `componentDidUpdate` | `useEffect(() => {}, [deps])` |
| `componentWillUnmount` | `useEffect(() => { return () => {} }, [])` |

### Example: Component Lifecycle in Our Project

**File: `src/pages/Home/Home.jsx`**
```javascript
import { useState, useEffect } from 'react'
import { fetchProperties } from '@/services/propertyService'

function Home() {
  // 1. Initialization (constructor equivalent)
  const [properties, setProperties] = useState([])
  const [loading, setLoading] = useState(true)

  // 2. Component Did Mount (runs once after mount)
  useEffect(() => {
    console.log('Component mounted')
    loadProperties()
  }, [])

  // 3. Component Did Update (runs when dependencies change)
  useEffect(() => {
    console.log('Properties updated:', properties.length)
  }, [properties])

  // 4. Component Will Unmount (cleanup)
  useEffect(() => {
    return () => {
      console.log('Component unmounting')
      // Cleanup: cancel requests, remove listeners, etc.
    }
  }, [])

  const loadProperties = async () => {
    try {
      const data = await fetchProperties()
      setProperties(data)
      setLoading(false)
    } catch (error) {
      console.error(error)
      setLoading(false)
    }
  }

  // 5. Render
  return <div>{/* JSX */}</div>
}
```

## Custom Hooks

Create reusable logic with custom hooks.

**File: `src/hooks/useProperties.js`**
```javascript
import { useState, useEffect } from 'react'
import { fetchProperties } from '@/services/propertyService'

function useProperties() {
  const [properties, setProperties] = useState([])
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState(null)

  useEffect(() => {
    loadProperties()
  }, [])

  const loadProperties = async () => {
    try {
      setLoading(true)
      setError(null)
      const data = await fetchProperties()
      setProperties(data)
    } catch (err) {
      setError(err.message)
    } finally {
      setLoading(false)
    }
  }

  const refetch = () => {
    loadProperties()
  }

  return { properties, loading, error, refetch }
}

export default useProperties
```

**Using the Custom Hook:**
```javascript
import useProperties from '@/hooks/useProperties'

function Home() {
  const { properties, loading, error, refetch } = useProperties()

  if (loading) return <div>Loading...</div>
  if (error) return <div>Error: {error}</div>

  return <div>{/* Render properties */}</div>
}
```

**File: `src/hooks/useProperty.js`**
```javascript
import { useState, useEffect } from 'react'
import { fetchPropertyById } from '@/services/propertyService'

function useProperty(propertyId) {
  const [property, setProperty] = useState(null)
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState(null)

  useEffect(() => {
    if (!propertyId) return

    const loadProperty = async () => {
      try {
        setLoading(true)
        setError(null)
        const data = await fetchPropertyById(propertyId)
        setProperty(data)
      } catch (err) {
        setError(err.message)
      } finally {
        setLoading(false)
      }
    }

    loadProperty()
  }, [propertyId])

  return { property, loading, error }
}

export default useProperty
```

## Complete Example: Property Detail with Hooks

**File: `src/pages/PropertyDetail/PropertyDetail.jsx`**
```javascript
import { useState, useEffect, useRef } from 'react'
import { useProperty } from '@/hooks/useProperty'
import Button from '@/components/common/Button'

function PropertyDetail({ propertyId }) {
  // Using custom hook
  const { property, loading, error } = useProperty(propertyId)

  // Local state for UI
  const [selectedImageIndex, setSelectedImageIndex] = useState(0)
  const [isBooked, setIsBooked] = useState(false)

  // Ref for scrolling
  const imagesRef = useRef(null)

  // Effect: Scroll to top when property changes
  useEffect(() => {
    window.scrollTo(0, 0)
  }, [propertyId])

  // Effect: Reset image selection when property changes
  useEffect(() => {
    setSelectedImageIndex(0)
  }, [propertyId])

  // Effect: Log booking status
  useEffect(() => {
    if (isBooked) {
      console.log('Property booked!')
    }
  }, [isBooked])

  const handleBook = () => {
    setIsBooked(true)
    // Booking logic here
  }

  if (loading) return <div>Loading...</div>
  if (error) return <div>Error: {error}</div>
  if (!property) return <div>Property not found</div>

  return (
    <div>
      {/* Render property details */}
      <Button onClick={handleBook} disabled={isBooked}>
        {isBooked ? 'Booked' : 'Reserve'}
      </Button>
    </div>
  )
}
```

## Rules of Hooks

1. **Only call hooks at the top level** - Don't call inside loops, conditions, or nested functions
2. **Only call hooks from React functions** - Function components or custom hooks

**❌ Wrong:**
```javascript
function Component() {
  if (condition) {
    const [state, setState] = useState(0) // ❌ Wrong!
  }
}
```

**✅ Correct:**
```javascript
function Component() {
  const [state, setState] = useState(0) // ✅ Correct
  
  if (condition) {
    // Use state here
  }
}
```

## Common Patterns

### 1. Fetching Data on Mount
```javascript
useEffect(() => {
  fetchData()
}, [])
```

### 2. Fetching Data When Prop Changes
```javascript
useEffect(() => {
  fetchData(id)
}, [id])
```

### 3. Cleanup on Unmount
```javascript
useEffect(() => {
  const subscription = subscribe()
  return () => {
    subscription.unsubscribe()
  }
}, [])
```

### 4. Multiple Effects
```javascript
// Separate concerns with multiple effects
useEffect(() => {
  // Handle data fetching
}, [propertyId])

useEffect(() => {
  // Handle window resize
}, [])
```

## ✅ Checkpoint

You should now understand:
- ✅ useState for managing state
- ✅ useEffect for side effects and lifecycle
- ✅ How hooks map to lifecycle methods
- ✅ Creating custom hooks
- ✅ Rules of hooks

## Summary

Hooks provide a powerful way to manage state and side effects in React. By understanding:
- **useState** - State management
- **useEffect** - Side effects and lifecycle
- **Custom hooks** - Reusable logic

You can build complex, maintainable React applications!

---

**Files to create:**
- `src/hooks/useProperties.js`
- `src/hooks/useProperty.js`

