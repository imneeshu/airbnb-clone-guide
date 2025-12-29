# 3. Components Explained: Smart vs Dumb vs Reusable

Understanding different types of components is crucial for building scalable React applications. In this section, we'll learn about **Reusable Components**, **Smart Components** (Container Components), and **Dumb Components** (Presentational Components).

## Component Types Overview

### 1. **Dumb Components** (Presentational Components)
### 2. **Smart Components** (Container Components)
### 3. **Reusable Components** (Shared UI Components)

## Dumb Components (Presentational Components)

**What are they?**
- Components that only handle **how things look**
- Receive data via props
- Don't manage state or side effects
- Easy to test and reuse
- Focused on presentation

**Characteristics:**
- ✅ No state management (or minimal local UI state)
- ✅ No API calls
- ✅ No business logic
- ✅ Pure functions (same input = same output)
- ✅ Highly reusable

**Example:**

**File: `src/components/common/PropertyCard/PropertyCard.jsx`**
```javascript
import './PropertyCard.css'

function PropertyCard({ property, onClick }) {
  const { title, location, price, image, rating } = property

  return (
    <div className="property-card" onClick={onClick}>
      <img src={image} alt={title} className="property-card__image" />
      <div className="property-card__content">
        <div className="property-card__header">
          <h3 className="property-card__title">{title}</h3>
          <span className="property-card__rating">★ {rating}</span>
        </div>
        <p className="property-card__location">{location}</p>
        <p className="property-card__price">${price} <span>night</span></p>
      </div>
    </div>
  )
}

export default PropertyCard
```

**File: `src/components/common/PropertyCard/PropertyCard.css`**
```css
.property-card {
  cursor: pointer;
  border-radius: var(--radius-lg);
  overflow: hidden;
  transition: transform 0.2s;
}

.property-card:hover {
  transform: translateY(-4px);
}

.property-card__image {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.property-card__content {
  padding: var(--spacing-md);
}

.property-card__header {
  display: flex;
  justify-content: space-between;
  align-items: start;
  margin-bottom: var(--spacing-xs);
}

.property-card__title {
  font-size: var(--font-size-base);
  font-weight: 600;
}

.property-card__rating {
  font-size: var(--font-size-sm);
}

.property-card__location {
  color: var(--text-secondary);
  font-size: var(--font-size-sm);
  margin-bottom: var(--spacing-xs);
}

.property-card__price {
  font-weight: 600;
  font-size: var(--font-size-base);
}

.property-card__price span {
  font-weight: 400;
  color: var(--text-secondary);
}
```

**File: `src/components/common/PropertyCard/index.js`**
```javascript
export { default } from './PropertyCard'
```

## Smart Components (Container Components)

**What are they?**
- Components that handle **how things work**
- Manage state and business logic
- Make API calls
- Pass data down to dumb components
- Usually page-level or feature-level components

**Characteristics:**
- ✅ State management (useState, useReducer, etc.)
- ✅ API calls and data fetching
- ✅ Event handlers and business logic
- ✅ May use custom hooks
- ✅ Orchestrate multiple child components

**Example:**

**File: `src/pages/Home/Home.jsx`**
```javascript
import { useState, useEffect } from 'react'
import PropertyCard from '@/components/common/PropertyCard'
import { fetchProperties } from '@/services/propertyService'
import './Home.css'

function Home() {
  // State management
  const [properties, setProperties] = useState([])
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState(null)

  // Side effects (API calls)
  useEffect(() => {
    loadProperties()
  }, [])

  // Business logic
  const loadProperties = async () => {
    try {
      setLoading(true)
      const data = await fetchProperties()
      setProperties(data)
    } catch (err) {
      setError(err.message)
    } finally {
      setLoading(false)
    }
  }

  const handlePropertyClick = (propertyId) => {
    // Navigation or other business logic
    console.log('Navigate to property:', propertyId)
  }

  // Render logic with conditional rendering
  if (loading) return <div>Loading...</div>
  if (error) return <div>Error: {error}</div>

  return (
    <div className="home">
      <h1>Explore Properties</h1>
      <div className="home__grid">
        {properties.map(property => (
          <PropertyCard
            key={property.id}
            property={property}
            onClick={() => handlePropertyClick(property.id)}
          />
        ))}
      </div>
    </div>
  )
}

export default Home
```

**File: `src/pages/Home/Home.css`**
```css
.home {
  padding: var(--spacing-xl);
}

.home__grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: var(--spacing-lg);
  margin-top: var(--spacing-lg);
}
```

**File: `src/pages/Home/index.js`**
```javascript
export { default } from './Home'
```

## Reusable Components

**What are they?**
- Components designed to be used **multiple times** across the app
- Can be either smart or dumb (usually dumb)
- Generic and configurable via props
- Examples: Button, Input, Card, Modal, etc.

**Characteristics:**
- ✅ Highly reusable
- ✅ Well-documented props
- ✅ Consistent styling and behavior
- ✅ Can be composed together
- ✅ Stored in `/components/common`

**Example:**

**File: `src/components/common/Button/Button.jsx`**
```javascript
import './Button.css'

function Button({ 
  children, 
  onClick, 
  variant = 'primary',
  size = 'medium',
  disabled = false,
  type = 'button'
}) {
  return (
    <button
      type={type}
      className={`button button--${variant} button--${size}`}
      onClick={onClick}
      disabled={disabled}
    >
      {children}
    </button>
  )
}

export default Button
```

**File: `src/components/common/Button/Button.css`**
```css
.button {
  border: none;
  border-radius: var(--radius-sm);
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s;
  font-family: inherit;
}

.button--primary {
  background-color: var(--primary-color);
  color: white;
}

.button--primary:hover:not(:disabled) {
  background-color: #E61E4D;
}

.button--secondary {
  background-color: var(--bg-white);
  color: var(--text-primary);
  border: 1px solid var(--border-color);
}

.button--secondary:hover:not(:disabled) {
  border-color: var(--text-primary);
}

.button--small {
  padding: 8px 16px;
  font-size: var(--font-size-sm);
}

.button--medium {
  padding: 12px 24px;
  font-size: var(--font-size-base);
}

.button--large {
  padding: 16px 32px;
  font-size: var(--font-size-lg);
}

.button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
```

**File: `src/components/common/Input/Input.jsx`**
```javascript
import './Input.css'

function Input({
  label,
  type = 'text',
  value,
  onChange,
  placeholder,
  error,
  ...props
}) {
  return (
    <div className="input-group">
      {label && <label className="input-label">{label}</label>}
      <input
        type={type}
        className={`input ${error ? 'input--error' : ''}`}
        value={value}
        onChange={onChange}
        placeholder={placeholder}
        {...props}
      />
      {error && <span className="input-error">{error}</span>}
    </div>
  )
}

export default Input
```

**File: `src/components/common/Input/Input.css`**
```css
.input-group {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-xs);
}

.input-label {
  font-size: var(--font-size-sm);
  font-weight: 600;
  color: var(--text-primary);
}

.input {
  padding: 12px 16px;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-sm);
  font-size: var(--font-size-base);
  font-family: inherit;
  transition: border-color 0.2s;
}

.input:focus {
  outline: none;
  border-color: var(--primary-color);
}

.input--error {
  border-color: #FF385C;
}

.input-error {
  font-size: var(--font-size-sm);
  color: #FF385C;
}
```

## Component Categorization for Our Project

### Reusable Components (`/components/common`)
These are generic components used throughout the app:
- ✅ `Button` - Reusable button component
- ✅ `Input` - Form input component
- ✅ `PropertyCard` - Display property information
- ✅ `Card` - Generic card container
- ✅ `Modal` - Popup/dialog component
- ✅ `Spinner` - Loading indicator

### Dumb Components (Presentational)
These focus on display:
- ✅ `PropertyCard` - Shows property data (receives via props)
- ✅ `Header` - Displays navigation (receives props)
- ✅ `Footer` - Shows footer info (receives props)
- ✅ `SearchBar` - Displays search input (receives props)

### Smart Components (Container/Pages)
These manage logic and state:
- ✅ `Home` page - Fetches and manages properties list
- ✅ `PropertyDetail` page - Fetches and manages single property
- ✅ `Search` page - Manages search state and results
- ✅ `Listing` page - Manages property listing logic

## Component Composition Example

Here's how components work together:

```javascript
// Smart Component (Home page)
function Home() {
  const [properties, setProperties] = useState([])
  
  useEffect(() => {
    // Fetch data
    fetchProperties().then(setProperties)
  }, [])
  
  return (
    <div>
      <Header /> {/* Layout component */}
      <SearchBar onSearch={handleSearch} /> {/* Reusable component */}
      <div className="grid">
        {properties.map(property => (
          <PropertyCard 
            key={property.id}
            property={property}  {/* Dumb component - just displays */}
          />
        ))}
      </div>
    </div>
  )
}
```

## Best Practices

### 1. **Keep Components Small and Focused**
One component = one responsibility

### 2. **Props for Configuration**
Use props to make components flexible

### 3. **Composition over Configuration**
Combine small components to build complex UIs

### 4. **Lift State Up**
When multiple components need the same state, lift it to a common parent

### 5. **Separate Concerns**
- Smart components = logic
- Dumb components = presentation
- Reusable components = shared UI

## ✅ Checkpoint

You should now understand:
- ✅ Difference between Smart, Dumb, and Reusable components
- ✅ When to use each type
- ✅ How components work together
- ✅ Best practices for component design

## Next Steps

Now let's start building screens:

**[Building Screens →](./04-building-screens.md)**

---

**Files to create:**
- `src/components/common/PropertyCard/PropertyCard.jsx`
- `src/components/common/PropertyCard/PropertyCard.css`
- `src/components/common/PropertyCard/index.js`
- `src/components/common/Button/Button.jsx` (updated)
- `src/components/common/Button/Button.css` (updated)
- `src/components/common/Input/Input.jsx`
- `src/components/common/Input/Input.css`
- `src/components/common/Input/index.js`

