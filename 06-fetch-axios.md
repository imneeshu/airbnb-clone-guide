# 6. Fetch & Axios Explained

In this section, we'll learn about making HTTP requests in React. We'll cover both the native `fetch` API and the `axios` library, understanding when and why to use each.

## Why Do We Need Fetch/Axios?

Modern web applications need to:
- 📡 Communicate with backend APIs
- 💾 Retrieve data from servers
- 📤 Send data to servers
- 🔄 Update and delete resources

## Native Fetch API

`fetch` is a built-in browser API for making HTTP requests. No installation needed!

### Basic Fetch Syntax

```javascript
fetch(url, options)
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error))
```

### Fetch Examples

#### GET Request (Fetching Data)

```javascript
// Simple GET request
fetch('https://api.example.com/properties')
  .then(response => {
    if (!response.ok) {
      throw new Error('Network response was not ok')
    }
    return response.json()
  })
  .then(data => {
    console.log('Properties:', data)
  })
  .catch(error => {
    console.error('Error fetching properties:', error)
  })
```

#### Using Async/Await (Cleaner Syntax)

```javascript
async function fetchProperties() {
  try {
    const response = await fetch('https://api.example.com/properties')
    
    if (!response.ok) {
      throw new Error('Network response was not ok')
    }
    
    const data = await response.json()
    return data
  } catch (error) {
    console.error('Error:', error)
    throw error
  }
}
```

#### POST Request (Sending Data)

```javascript
async function createProperty(propertyData) {
  try {
    const response = await fetch('https://api.example.com/properties', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(propertyData)
    })
    
    if (!response.ok) {
      throw new Error('Failed to create property')
    }
    
    const newProperty = await response.json()
    return newProperty
  } catch (error) {
    console.error('Error:', error)
    throw error
  }
}
```

#### PUT and DELETE Requests

```javascript
// PUT - Update
async function updateProperty(id, propertyData) {
  const response = await fetch(`https://api.example.com/properties/${id}`, {
    method: 'PUT',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(propertyData)
  })
  return response.json()
}

// DELETE
async function deleteProperty(id) {
  const response = await fetch(`https://api.example.com/properties/${id}`, {
    method: 'DELETE'
  })
  return response.json()
}
```

## Axios Library

**Axios** is a popular HTTP client library that provides:
- ✅ Simpler syntax
- ✅ Automatic JSON parsing
- ✅ Request/Response interceptors
- ✅ Better error handling
- ✅ Request cancellation

### Installing Axios

```bash
npm install axios
```

### Basic Axios Syntax

```javascript
import axios from 'axios'

// GET request
axios.get(url)
  .then(response => console.log(response.data))
  .catch(error => console.error(error))
```

### Axios Examples

#### GET Request

```javascript
import axios from 'axios'

// Simple GET
async function fetchProperties() {
  try {
    const response = await axios.get('https://api.example.com/properties')
    return response.data
  } catch (error) {
    console.error('Error:', error)
    throw error
  }
}

// GET with query parameters
async function searchProperties(query) {
  try {
    const response = await axios.get('https://api.example.com/properties', {
      params: {
        search: query,
        limit: 20
      }
    })
    return response.data
  } catch (error) {
    console.error('Error:', error)
    throw error
  }
}
```

#### POST Request

```javascript
async function createProperty(propertyData) {
  try {
    const response = await axios.post(
      'https://api.example.com/properties',
      propertyData,
      {
        headers: {
          'Content-Type': 'application/json'
        }
      }
    )
    return response.data
  } catch (error) {
    console.error('Error:', error)
    throw error
  }
}
```

#### PUT and DELETE

```javascript
// PUT
async function updateProperty(id, propertyData) {
  const response = await axios.put(
    `https://api.example.com/properties/${id}`,
    propertyData
  )
  return response.data
}

// DELETE
async function deleteProperty(id) {
  const response = await axios.delete(`https://api.example.com/properties/${id}`)
  return response.data
}
```

## Creating an API Service

Let's create a service file to organize all our API calls.

**File: `src/services/api.js`**
```javascript
import axios from 'axios'

// Create axios instance with base configuration
const api = axios.create({
  baseURL: 'https://api.example.com', // Replace with your API URL
  timeout: 10000, // 10 seconds
  headers: {
    'Content-Type': 'application/json'
  }
})

// Request interceptor (add auth token, etc.)
api.interceptors.request.use(
  (config) => {
    // You can add auth token here
    const token = localStorage.getItem('token')
    if (token) {
      config.headers.Authorization = `Bearer ${token}`
    }
    return config
  },
  (error) => {
    return Promise.reject(error)
  }
)

// Response interceptor (handle errors globally)
api.interceptors.response.use(
  (response) => {
    return response
  },
  (error) => {
    // Handle common errors
    if (error.response?.status === 401) {
      // Unauthorized - redirect to login
      console.error('Unauthorized')
    } else if (error.response?.status === 500) {
      // Server error
      console.error('Server error')
    }
    return Promise.reject(error)
  }
)

export default api
```

**File: `src/services/propertyService.js`**
```javascript
import api from './api'

// Get all properties
export const fetchProperties = async () => {
  try {
    const response = await api.get('/properties')
    return response.data
  } catch (error) {
    console.error('Error fetching properties:', error)
    throw error
  }
}

// Get property by ID
export const fetchPropertyById = async (id) => {
  try {
    const response = await api.get(`/properties/${id}`)
    return response.data
  } catch (error) {
    console.error('Error fetching property:', error)
    throw error
  }
}

// Search properties
export const searchProperties = async (query) => {
  try {
    const response = await api.get('/properties/search', {
      params: { q: query }
    })
    return response.data
  } catch (error) {
    console.error('Error searching properties:', error)
    throw error
  }
}

// Create new property
export const createProperty = async (propertyData) => {
  try {
    const response = await api.post('/properties', propertyData)
    return response.data
  } catch (error) {
    console.error('Error creating property:', error)
    throw error
  }
}

// Update property
export const updateProperty = async (id, propertyData) => {
  try {
    const response = await api.put(`/properties/${id}`, propertyData)
    return response.data
  } catch (error) {
    console.error('Error updating property:', error)
    throw error
  }
}

// Delete property
export const deleteProperty = async (id) => {
  try {
    const response = await api.delete(`/properties/${id}`)
    return response.data
  } catch (error) {
    console.error('Error deleting property:', error)
    throw error
  }
}
```

## Using API Service in Components

Update the Home component to use the service:

**File: `src/pages/Home/Home.jsx`** (example with real API)
```javascript
import { useState, useEffect } from 'react'
import PropertyCard from '@/components/common/PropertyCard'
import { fetchProperties } from '@/services/propertyService'
import './Home.css'

function Home() {
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
      setError(err.message || 'Failed to load properties')
      console.error('Error loading properties:', err)
    } finally {
      setLoading(false)
    }
  }

  if (loading) {
    return (
      <div className="loading-container">
        <p>Loading properties...</p>
      </div>
    )
  }

  if (error) {
    return (
      <div className="error-container">
        <p>Error: {error}</p>
        <button onClick={loadProperties}>Try Again</button>
      </div>
    )
  }

  return (
    <div className="home">
      <div className="home__header">
        <h1>Explore Places to Stay</h1>
        <p>Find the perfect accommodation for your trip</p>
      </div>
      
      <div className="home__grid">
        {properties.map(property => (
          <PropertyCard
            key={property.id}
            property={property}
            onClick={() => console.log('Clicked property:', property.id)}
          />
        ))}
      </div>
    </div>
  )
}

export default Home
```

## Fetch vs Axios: Which to Use?

### Use Fetch When:
- ✅ You want to use native browser APIs (no dependencies)
- ✅ Your project is small and simple
- ✅ You don't need advanced features

### Use Axios When:
- ✅ You want simpler, cleaner syntax
- ✅ You need interceptors (auth tokens, error handling)
- ✅ You want automatic JSON parsing
- ✅ You need request/response transformation
- ✅ Better TypeScript support

## Using with Mock Data (Development)

For development, you can create a service that uses mock data:

**File: `src/services/propertyService.js`** (with mock data fallback)
```javascript
import api from './api'
import { mockProperties, getPropertyById, searchProperties as mockSearch } from '@/data/mockProperties'

const USE_MOCK_DATA = true // Set to false when API is ready

// Get all properties
export const fetchProperties = async () => {
  if (USE_MOCK_DATA) {
    // Simulate API delay
    await new Promise(resolve => setTimeout(resolve, 1000))
    return mockProperties
  }
  
  try {
    const response = await api.get('/properties')
    return response.data
  } catch (error) {
    console.error('Error fetching properties:', error)
    throw error
  }
}

// Get property by ID
export const fetchPropertyById = async (id) => {
  if (USE_MOCK_DATA) {
    await new Promise(resolve => setTimeout(resolve, 500))
    return getPropertyById(id)
  }
  
  try {
    const response = await api.get(`/properties/${id}`)
    return response.data
  } catch (error) {
    console.error('Error fetching property:', error)
    throw error
  }
}

// Search properties
export const searchProperties = async (query) => {
  if (USE_MOCK_DATA) {
    await new Promise(resolve => setTimeout(resolve, 500))
    return mockSearch(query)
  }
  
  try {
    const response = await api.get('/properties/search', {
      params: { q: query }
    })
    return response.data
  } catch (error) {
    console.error('Error searching properties:', error)
    throw error
  }
}
```

## Error Handling Best Practices

```javascript
async function fetchData() {
  try {
    const response = await axios.get('/api/data')
    return response.data
  } catch (error) {
    if (error.response) {
      // Server responded with error status
      console.error('Server error:', error.response.status)
      console.error('Error data:', error.response.data)
    } else if (error.request) {
      // Request made but no response
      console.error('No response:', error.request)
    } else {
      // Something else happened
      console.error('Error:', error.message)
    }
    throw error
  }
}
```

## ✅ Checkpoint

You should now understand:
- ✅ How to use Fetch API
- ✅ How to use Axios
- ✅ When to use each
- ✅ How to organize API calls in services
- ✅ Error handling strategies

## Next Steps

Let's learn about React Hooks and Lifecycle:

**[Hooks & Lifecycle →](./07-hooks-lifecycle.md)**

---

**Files to create:**
- `src/services/api.js`
- `src/services/propertyService.js`

**Package to install:**
- `npm install axios`

