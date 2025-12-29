# 8. Practical Example: Network Calls with Hooks

In this section, we'll build a complete working example that combines everything we've learned: making real network calls using hooks. We'll use a free public API to fetch data and see how hooks manage the network request lifecycle.

## What We'll Build

A property search feature that:
- Makes real API calls
- Uses custom hooks to manage state
- Handles loading and error states
- Demonstrates practical hook usage

## Using a Free API for Learning

We'll use **JSONPlaceholder** or a similar free API. For a more realistic example, we can use a property listing API.

**Option 1: JSONPlaceholder (Simple)**
- Free, no API key needed
- Good for learning API calls

**Option 2: Real Estate API (More Realistic)**
- We'll simulate with a mock server approach
- Shows real-world patterns

## Step 1: Create a Custom Hook for API Calls

Let's create a reusable hook that handles API calls:

**File: `src/hooks/useFetch.js`**
```javascript
import { useState, useEffect } from 'react'

function useFetch(url, options = {}) {
  const [data, setData] = useState(null)
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState(null)

  useEffect(() => {
    // Reset states when URL changes
    setLoading(true)
    setError(null)
    setData(null)

    // Create AbortController for cleanup
    const abortController = new AbortController()

    const fetchData = async () => {
      try {
        const response = await fetch(url, {
          ...options,
          signal: abortController.signal
        })

        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`)
        }

        const jsonData = await response.json()
        setData(jsonData)
        setError(null)
      } catch (err) {
        // Don't set error if request was aborted
        if (err.name !== 'AbortError') {
          setError(err.message)
          setData(null)
        }
      } finally {
        setLoading(false)
      }
    }

    fetchData()

    // Cleanup function: abort request if component unmounts
    return () => {
      abortController.abort()
    }
  }, [url]) // Re-run when URL changes

  // Function to manually refetch
  const refetch = () => {
    setLoading(true)
    setError(null)
    
    fetch(url, options)
      .then(response => {
        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`)
        }
        return response.json()
      })
      .then(jsonData => {
        setData(jsonData)
        setError(null)
      })
      .catch(err => {
        setError(err.message)
      })
      .finally(() => {
        setLoading(false)
      })
  }

  return { data, loading, error, refetch }
}

export default useFetch
```

## Step 2: Create API Service with Real Calls

**File: `src/services/propertyService.js`** (Updated with real API example)

```javascript
import axios from 'axios'

// Create axios instance
const api = axios.create({
  baseURL: 'https://jsonplaceholder.typicode.com', // Free test API
  timeout: 10000,
  headers: {
    'Content-Type': 'application/json'
  }
})

// For a real property API, you might use:
// baseURL: 'https://api.airbnb.com/v1' (example)

// Convert posts to property-like format (for demo)
const transformToProperty = (post, index) => ({
  id: post.id,
  title: post.title,
  location: `City ${post.id}`,
  price: (post.id * 20) + 50,
  rating: (4 + (index % 5) * 0.2).toFixed(1),
  image: `https://images.unsplash.com/photo-${1500000000000 + post.id}?w=400`,
  description: post.body
})

// Fetch properties (using JSONPlaceholder as example)
export const fetchProperties = async () => {
  try {
    // This is a real network call!
    const response = await api.get('/posts?_limit=10')
    
    // Transform the data to match our property structure
    const properties = response.data.map((post, index) => 
      transformToProperty(post, index)
    )
    
    return properties
  } catch (error) {
    console.error('Error fetching properties:', error)
    throw new Error('Failed to fetch properties. Please try again.')
  }
}

// Fetch single property by ID
export const fetchPropertyById = async (id) => {
  try {
    // Real network call
    const response = await api.get(`/posts/${id}`)
    const post = response.data
    
    return transformToProperty(post, 0)
  } catch (error) {
    console.error('Error fetching property:', error)
    throw new Error('Failed to fetch property. Please try again.')
  }
}

// Search properties (example)
export const searchProperties = async (query) => {
  try {
    // Real network call with query params
    const response = await api.get('/posts', {
      params: {
        q: query,
        _limit: 10
      }
    })
    
    const properties = response.data.map((post, index) => 
      transformToProperty(post, index)
    )
    
    return properties
  } catch (error) {
    console.error('Error searching properties:', error)
    throw new Error('Search failed. Please try again.')
  }
}
```

## Step 3: Create Hook for Properties

**File: `src/hooks/useProperties.js`** (Updated)

```javascript
import { useState, useEffect, useCallback } from 'react'
import { fetchProperties, searchProperties } from '@/services/propertyService'

function useProperties() {
  const [properties, setProperties] = useState([])
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState(null)

  // Load properties on mount
  useEffect(() => {
    loadProperties()
  }, [])

  // Load all properties
  const loadProperties = useCallback(async () => {
    try {
      setLoading(true)
      setError(null)
      
      // This makes a REAL network call!
      const data = await fetchProperties()
      setProperties(data)
    } catch (err) {
      setError(err.message || 'Failed to load properties')
      setProperties([])
    } finally {
      setLoading(false)
    }
  }, [])

  // Search properties
  const search = useCallback(async (query) => {
    if (!query.trim()) {
      loadProperties()
      return
    }

    try {
      setLoading(true)
      setError(null)
      
      // Real network call with search query
      const data = await searchProperties(query)
      setProperties(data)
    } catch (err) {
      setError(err.message || 'Search failed')
      setProperties([])
    } finally {
      setLoading(false)
    }
  }, [loadProperties])

  return {
    properties,
    loading,
    error,
    loadProperties,
    search
  }
}

export default useProperties
```

## Step 4: Update Home Component with Real API Calls

**File: `src/pages/Home/Home.jsx`** (Updated to use real API)

```javascript
import { useEffect } from 'react'
import PropertyCard from '@/components/common/PropertyCard'
import Button from '@/components/common/Button'
import useProperties from '@/hooks/useProperties'
import './Home.css'

function Home() {
  // Using our custom hook - handles all the network logic!
  const { properties, loading, error, loadProperties, search } = useProperties()

  // Log when properties change (demonstrates useEffect)
  useEffect(() => {
    if (properties.length > 0) {
      console.log(`Loaded ${properties.length} properties from API`)
    }
  }, [properties])

  // Handle search
  const handleSearch = (query) => {
    search(query)
  }

  if (loading) {
    return (
      <div className="loading-container">
        <div className="spinner"></div>
        <p>Loading properties from API...</p>
      </div>
    )
  }

  if (error) {
    return (
      <div className="error-container">
        <h2>Oops! Something went wrong</h2>
        <p>{error}</p>
        <Button onClick={loadProperties} variant="primary">
          Try Again
        </Button>
      </div>
    )
  }

  return (
    <div className="home">
      <div className="home__header">
        <h1>Explore Places to Stay</h1>
        <p>Real data loaded from API</p>
      </div>

      <div className="home__search-bar">
        <input
          type="text"
          placeholder="Search properties..."
          onChange={(e) => {
            const query = e.target.value
            if (query.length > 2 || query.length === 0) {
              handleSearch(query)
            }
          }}
        />
      </div>
      
      <div className="home__grid">
        {properties.length === 0 ? (
          <p>No properties found</p>
        ) : (
          properties.map(property => (
            <PropertyCard
              key={property.id}
              property={property}
              onClick={() => console.log('Clicked property:', property.id)}
            />
          ))
        )}
      </div>
    </div>
  )
}

export default Home
```

Add some CSS for the spinner:

**File: `src/pages/Home/Home.css`** (Add these styles)

```css
.spinner {
  border: 4px solid var(--bg-light);
  border-top: 4px solid var(--primary-color);
  border-radius: 50%;
  width: 40px;
  height: 40px;
  animation: spin 1s linear infinite;
  margin: 0 auto 16px;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.home__search-bar {
  margin-bottom: var(--spacing-xl);
  max-width: 500px;
}

.home__search-bar input {
  width: 100%;
  padding: 12px 16px;
  border: 1px solid var(--border-color);
  border-radius: var(--radius-md);
  font-size: var(--font-size-base);
}

.home__search-bar input:focus {
  outline: none;
  border-color: var(--primary-color);
}
```

## Step 5: Property Detail with Network Call

**File: `src/pages/PropertyDetail/PropertyDetail.jsx`** (Updated)

```javascript
import { useState, useEffect } from 'react'
import { useParams } from 'react-router-dom' // If using React Router
import Button from '@/components/common/Button'
import { fetchPropertyById } from '@/services/propertyService'
import './PropertyDetail.css'

function PropertyDetail() {
  // Get ID from URL (if using React Router)
  // const { id } = useParams()
  // For now, use a default or prop
  const propertyId = 1 // or from props/params

  // State managed with hooks
  const [property, setProperty] = useState(null)
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState(null)

  // useEffect runs when component mounts AND when propertyId changes
  useEffect(() => {
    // This function will be called when component mounts
    loadProperty()

    // Cleanup function (runs when component unmounts or before re-running)
    return () => {
      console.log('Cleaning up PropertyDetail')
      // Cancel any pending requests here if needed
    }
  }, [propertyId]) // Dependency array: re-run if propertyId changes

  const loadProperty = async () => {
    try {
      setLoading(true)
      setError(null)
      
      // Real network call happens here!
      console.log(`Fetching property ${propertyId} from API...`)
      const data = await fetchPropertyById(propertyId)
      
      setProperty(data)
      console.log('Property loaded:', data)
    } catch (err) {
      setError(err.message || 'Failed to load property')
      console.error('Error loading property:', err)
    } finally {
      setLoading(false)
    }
  }

  // Another useEffect to demonstrate multiple effects
  useEffect(() => {
    // This runs every time 'property' changes
    if (property) {
      document.title = property.title // Update page title
      console.log('Property data updated:', property.title)
    }

    // Cleanup: reset title when component unmounts
    return () => {
      document.title = 'Airbnb Clone'
    }
  }, [property])

  // Loading state
  if (loading) {
    return (
      <div className="loading-container">
        <div className="spinner"></div>
        <p>Loading property details from API...</p>
      </div>
    )
  }

  // Error state
  if (error) {
    return (
      <div className="error-container">
        <h2>Error loading property</h2>
        <p>{error}</p>
        <Button onClick={loadProperty}>Retry</Button>
      </div>
    )
  }

  // No property found
  if (!property) {
    return (
      <div className="error-container">
        <p>Property not found</p>
      </div>
    )
  }

  // Success: render property
  return (
    <div className="property-detail">
      <div className="property-detail__images">
        <img 
          src={property.image || 'https://via.placeholder.com/800x400'}
          alt={property.title}
          className="property-detail__image"
        />
      </div>

      <div className="property-detail__content">
        <div className="property-detail__main">
          <div className="property-detail__header">
            <h1>{property.title}</h1>
            <div className="property-detail__meta">
              <span>★ {property.rating}</span>
              <span>{property.location}</span>
            </div>
          </div>

          <div className="property-detail__description">
            <h2>About this place</h2>
            <p>{property.description}</p>
          </div>
        </div>

        <div className="property-detail__sidebar">
          <div className="property-detail__booking">
            <div className="property-detail__price">
              <span className="price">${property.price}</span>
              <span className="period">/ night</span>
            </div>
            <Button variant="primary" size="large" style={{ width: '100%' }}>
              Reserve
            </Button>
          </div>
        </div>
      </div>
    </div>
  )
}

export default PropertyDetail
```

## Step 6: Using useFetch Hook (Alternative Approach)

Here's how to use the generic `useFetch` hook we created:

**File: `src/pages/Home/HomeWithUseFetch.jsx`** (Example)

```javascript
import useFetch from '@/hooks/useFetch'
import PropertyCard from '@/components/common/PropertyCard'

function HomeWithUseFetch() {
  // Using the generic useFetch hook
  const { data: properties, loading, error, refetch } = useFetch(
    'https://jsonplaceholder.typicode.com/posts?_limit=10'
  )

  if (loading) return <div>Loading...</div>
  if (error) return <div>Error: {error}</div>
  if (!properties) return null

  return (
    <div>
      <button onClick={refetch}>Refresh Data</button>
      <div className="grid">
        {properties.map(item => (
          <div key={item.id}>
            <h3>{item.title}</h3>
          </div>
        ))}
      </div>
    </div>
  )
}
```

## Understanding the Network Flow

Here's what happens when you use these hooks:

1. **Component Mounts** → `useEffect` runs
2. **Loading State** → `setLoading(true)`
3. **Network Request** → `fetch()` or `axios.get()` is called
4. **Request in Progress** → Component shows loading UI
5. **Response Received** → Data processed
6. **State Updated** → `setData()` called
7. **Loading Complete** → `setLoading(false)`
8. **Component Re-renders** → UI shows data

## Error Handling Pattern

```javascript
try {
  setLoading(true)
  setError(null) // Clear previous errors
  
  const data = await apiCall()
  setData(data)
  
} catch (err) {
  setError(err.message) // Show error to user
  setData(null) // Clear data on error
  
} finally {
  setLoading(false) // Always stop loading
}
```

## Testing Network Calls

### 1. Test with Real API (JSONPlaceholder)
```javascript
// This will make a real network call
const { data, loading, error } = useFetch('https://jsonplaceholder.typicode.com/posts/1')
```

### 2. Test Error Handling
```javascript
// This will trigger an error
const { data, loading, error } = useFetch('https://invalid-url-12345.com/data')
```

### 3. Check Network Tab
- Open browser DevTools → Network tab
- See the actual HTTP requests
- See request/response headers
- See timing information

## Key Takeaways

1. **Hooks manage the network lifecycle** - Loading, success, error states
2. **useEffect triggers network calls** - When component mounts or dependencies change
3. **Cleanup prevents memory leaks** - Cancel requests on unmount
4. **Error handling is crucial** - Always handle network failures
5. **Loading states improve UX** - Show users what's happening

## Real-World API Integration

When you're ready for a real property API:

```javascript
// Update baseURL in api.js
const api = axios.create({
  baseURL: 'https://api.yourapi.com/v1',
  headers: {
    'Authorization': `Bearer ${API_KEY}`,
    'Content-Type': 'application/json'
  }
})
```

## ✅ Checkpoint

You should now understand:
- ✅ How to make real network calls
- ✅ How hooks manage network state
- ✅ Loading and error handling
- ✅ useEffect for triggering API calls
- ✅ Cleanup and memory management

---

**Files Created/Updated:**
- `src/hooks/useFetch.js` (NEW)
- `src/hooks/useProperties.js` (UPDATED)
- `src/services/propertyService.js` (UPDATED with real API)
- `src/pages/Home/Home.jsx` (UPDATED)
- `src/pages/PropertyDetail/PropertyDetail.jsx` (UPDATED)

