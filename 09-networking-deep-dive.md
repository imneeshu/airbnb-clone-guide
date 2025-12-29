# 9. Networking Deep Dive: How It Works in Practice

This section provides a deeper understanding of how networking works in React applications, with real examples you can run and test.

## Understanding the Request-Response Cycle

When you make a network call, here's what happens:

```
1. User Action (button click, page load)
   ↓
2. React Component (triggers useEffect or event handler)
   ↓
3. API Service (makes HTTP request)
   ↓
4. Network Layer (browser sends request)
   ↓
5. Server (processes request)
   ↓
6. Response (data comes back)
   ↓
7. Component State (updates with new data)
   ↓
8. UI Re-render (user sees updated content)
```

## Real Example: Complete Network Flow

Let's trace through a complete example:

**File: `src/components/PropertyLoader.jsx`** (Complete Example)

```javascript
import { useState, useEffect, useRef } from 'react'
import { fetchPropertyById } from '@/services/propertyService'

function PropertyLoader({ propertyId }) {
  // State for managing the network request lifecycle
  const [property, setProperty] = useState(null)
  const [loading, setLoading] = useState(false)
  const [error, setError] = useState(null)
  
  // Ref to track if component is mounted (for cleanup)
  const isMountedRef = useRef(true)

  useEffect(() => {
    // Mark component as mounted
    isMountedRef.current = true
    
    // This function will run when component mounts OR when propertyId changes
    loadProperty()

    // Cleanup function
    return () => {
      // Mark component as unmounted
      isMountedRef.current = false
      console.log('Component cleanup: cancelled pending requests')
    }
  }, [propertyId])

  const loadProperty = async () => {
    // Step 1: Set loading state
    console.log('🔄 Starting network request...')
    setLoading(true)
    setError(null)

    try {
      // Step 2: Make the actual network call
      console.log('📡 Sending HTTP request to server...')
      const data = await fetchPropertyById(propertyId)
      
      // Step 3: Check if component is still mounted before updating state
      if (isMountedRef.current) {
        console.log('✅ Response received:', data)
        setProperty(data)
        setLoading(false)
      } else {
        console.log('⚠️ Component unmounted, ignoring response')
      }

    } catch (err) {
      // Step 4: Handle errors
      console.error('❌ Network error:', err)
      
      if (isMountedRef.current) {
        setError(err.message)
        setLoading(false)
        setProperty(null)
      }
    }
  }

  // Render based on state
  if (loading) {
    return (
      <div>
        <p>🔄 Loading property {propertyId}...</p>
        <p>Check the browser console to see the network flow!</p>
      </div>
    )
  }

  if (error) {
    return (
      <div>
        <p>❌ Error: {error}</p>
        <button onClick={loadProperty}>Retry</button>
      </div>
    )
  }

  if (!property) {
    return <div>No property loaded</div>
  }

  return (
    <div>
      <h2>{property.title}</h2>
      <p>{property.description}</p>
      <button onClick={loadProperty}>Reload</button>
    </div>
  )
}

export default PropertyLoader
```

## Understanding Network Timing

Let's create a component that shows network timing information:

**File: `src/components/NetworkMonitor.jsx`**

```javascript
import { useState, useEffect } from 'react'
import { fetchProperties } from '@/services/propertyService'

function NetworkMonitor() {
  const [data, setData] = useState(null)
  const [timing, setTiming] = useState(null)

  const makeRequest = async () => {
    const startTime = performance.now()
    console.log('⏱️ Request started at:', startTime)

    try {
      // Make the request
      const result = await fetchProperties()
      
      const endTime = performance.now()
      const duration = endTime - startTime

      console.log('⏱️ Request completed at:', endTime)
      console.log('⏱️ Total duration:', duration.toFixed(2), 'ms')

      setData(result)
      setTiming({
        start: startTime,
        end: endTime,
        duration: duration.toFixed(2)
      })
    } catch (error) {
      console.error('Request failed:', error)
    }
  }

  useEffect(() => {
    // Make initial request
    makeRequest()
  }, [])

  return (
    <div>
      <h3>Network Monitor</h3>
      {timing && (
        <div>
          <p>Request Duration: {timing.duration} ms</p>
          <p>Open DevTools → Network tab to see detailed timing</p>
        </div>
      )}
      <button onClick={makeRequest}>Make Request</button>
      {data && <p>Loaded {data.length} items</p>}
    </div>
  )
}

export default NetworkMonitor
```

## Testing Different Network Scenarios

### 1. Testing with Different APIs

Create a test component to try different endpoints:

**File: `src/components/NetworkTester.jsx`**

```javascript
import { useState } from 'react'

function NetworkTester() {
  const [results, setResults] = useState({})

  const testEndpoint = async (name, url) => {
    const startTime = performance.now()
    
    try {
      const response = await fetch(url)
      const data = await response.json()
      const duration = (performance.now() - startTime).toFixed(2)
      
      setResults(prev => ({
        ...prev,
        [name]: {
          status: '✅ Success',
          duration: `${duration}ms`,
          dataSize: JSON.stringify(data).length
        }
      }))
    } catch (error) {
      const duration = (performance.now() - startTime).toFixed(2)
      setResults(prev => ({
        ...prev,
        [name]: {
          status: '❌ Failed',
          duration: `${duration}ms`,
          error: error.message
        }
      }))
    }
  }

  const testAll = () => {
    testEndpoint('JSONPlaceholder', 'https://jsonplaceholder.typicode.com/posts/1')
    testEndpoint('HTTPBin', 'https://httpbin.org/json')
    testEndpoint('Invalid URL', 'https://this-does-not-exist-12345.com/data')
  }

  return (
    <div>
      <h3>Network Tester</h3>
      <button onClick={testAll}>Test All Endpoints</button>
      
      <div>
        {Object.entries(results).map(([name, result]) => (
          <div key={name} style={{ marginTop: '16px', padding: '12px', border: '1px solid #ccc' }}>
            <strong>{name}:</strong>
            <div>Status: {result.status}</div>
            <div>Duration: {result.duration}</div>
            {result.dataSize && <div>Data Size: {result.dataSize} bytes</div>}
            {result.error && <div>Error: {result.error}</div>}
          </div>
        ))}
      </div>
    </div>
  )
}

export default NetworkTester
```

## Understanding Hook Dependencies

Here's how dependencies work with network calls:

```javascript
function PropertyList({ userId, filter }) {
  const [properties, setProperties] = useState([])

  // This runs ONLY when component first mounts
  useEffect(() => {
    fetchProperties().then(setProperties)
  }, []) // Empty array = run once

  // This runs when userId changes
  useEffect(() => {
    if (userId) {
      fetchUserProperties(userId).then(setProperties)
    }
  }, [userId]) // Runs when userId changes

  // This runs when EITHER userId OR filter changes
  useEffect(() => {
    fetchFilteredProperties(userId, filter).then(setProperties)
  }, [userId, filter]) // Runs when either dependency changes
}
```

## Canceling Requests (Important!)

Prevent memory leaks by canceling requests when component unmounts:

```javascript
import { useEffect, useRef } from 'react'

function PropertyDetail({ id }) {
  const [property, setProperty] = useState(null)
  const abortControllerRef = useRef(null)

  useEffect(() => {
    // Create new AbortController for this request
    abortControllerRef.current = new AbortController()
    const signal = abortControllerRef.current.signal

    const loadProperty = async () => {
      try {
        const response = await fetch(`/api/properties/${id}`, {
          signal // Pass signal to fetch
        })
        const data = await response.json()
        setProperty(data)
      } catch (error) {
        if (error.name !== 'AbortError') {
          console.error('Error:', error)
        }
      }
    }

    loadProperty()

    // Cleanup: cancel request if component unmounts
    return () => {
      if (abortControllerRef.current) {
        abortControllerRef.current.abort()
        console.log('Request cancelled')
      }
    }
  }, [id])

  return <div>{/* Render property */}</div>
}
```

## Debugging Network Calls

### 1. Console Logging

```javascript
const loadData = async () => {
  console.group('🌐 Network Request')
  console.log('URL:', url)
  console.log('Method: GET')
  console.time('Request Duration')
  
  try {
    const response = await fetch(url)
    console.log('Status:', response.status)
    console.log('Headers:', response.headers)
    
    const data = await response.json()
    console.timeEnd('Request Duration')
    console.log('Data received:', data)
    console.groupEnd()
    
    return data
  } catch (error) {
    console.timeEnd('Request Duration')
    console.error('Error:', error)
    console.groupEnd()
    throw error
  }
}
```

### 2. Using Browser DevTools

1. Open DevTools (F12)
2. Go to **Network** tab
3. Filter by **XHR** or **Fetch**
4. Click on a request to see:
   - Request headers
   - Response headers
   - Response body
   - Timing information
   - Preview of data

### 3. Network Interceptor (Advanced)

Add logging to all API calls:

**File: `src/services/api.js`** (Updated with logging)

```javascript
import axios from 'axios'

const api = axios.create({
  baseURL: 'https://jsonplaceholder.typicode.com',
  timeout: 10000
})

// Request interceptor - log all outgoing requests
api.interceptors.request.use(
  (config) => {
    console.log('📤 Request:', {
      method: config.method.toUpperCase(),
      url: config.url,
      data: config.data
    })
    return config
  },
  (error) => {
    console.error('❌ Request Error:', error)
    return Promise.reject(error)
  }
)

// Response interceptor - log all incoming responses
api.interceptors.response.use(
  (response) => {
    console.log('📥 Response:', {
      status: response.status,
      url: response.config.url,
      data: response.data
    })
    return response
  },
  (error) => {
    console.error('❌ Response Error:', {
      status: error.response?.status,
      message: error.message,
      url: error.config?.url
    })
    return Promise.reject(error)
  }
)

export default api
```

## Best Practices Summary

1. **Always handle loading states** - Users need feedback
2. **Handle errors gracefully** - Show user-friendly messages
3. **Cancel requests on unmount** - Prevent memory leaks
4. **Use loading flags correctly** - Set true before, false after
5. **Clear previous data on new requests** - Avoid showing stale data
6. **Use appropriate HTTP methods** - GET for reading, POST for creating
7. **Handle network failures** - Network can be unreliable
8. **Log for debugging** - Console logs help during development

## Real-World Example: Search with Debouncing

**File: `src/hooks/useSearch.js`**

```javascript
import { useState, useEffect } from 'react'
import { searchProperties } from '@/services/propertyService'

function useSearch() {
  const [query, setQuery] = useState('')
  const [results, setResults] = useState([])
  const [loading, setLoading] = useState(false)
  const [error, setError] = useState(null)

  useEffect(() => {
    // Don't search if query is too short
    if (query.length < 2) {
      setResults([])
      return
    }

    // Debounce: wait 500ms before searching
    const timeoutId = setTimeout(async () => {
      try {
        setLoading(true)
        setError(null)
        
        console.log('🔍 Searching for:', query)
        const data = await searchProperties(query)
        
        setResults(data)
        console.log('✅ Found', data.length, 'results')
      } catch (err) {
        setError(err.message)
        setResults([])
      } finally {
        setLoading(false)
      }
    }, 500)

    // Cleanup: cancel timeout if query changes before timeout completes
    return () => {
      clearTimeout(timeoutId)
    }
  }, [query]) // Re-run when query changes

  return {
    query,
    setQuery,
    results,
    loading,
    error
  }
}

export default useSearch
```

## ✅ Checkpoint

You should now understand:
- ✅ Complete network request lifecycle
- ✅ How hooks manage network state
- ✅ Request cancellation and cleanup
- ✅ Debugging network calls
- ✅ Error handling patterns
- ✅ Timing and performance monitoring

---

**Files Created:**
- `src/components/PropertyLoader.jsx`
- `src/components/NetworkMonitor.jsx`
- `src/components/NetworkTester.jsx`
- `src/hooks/useSearch.js`

