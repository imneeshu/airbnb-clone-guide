# 4. Building Screens Separately

In this section, we'll build each screen of our Airbnb clone step-by-step. Each screen is a separate component that can be developed and tested independently.

## Screens We'll Build

1. **Home Screen** - Property listings grid
2. **Property Detail Screen** - Individual property view
3. **Search Screen** - Search results
4. **Header/Navigation** - App navigation

Let's start building!

## Screen 1: Home Screen

The home screen displays a grid of available properties.

### Step 1: Create the Home Component

**File: `src/pages/Home/Home.jsx`**
```javascript
import { useState, useEffect } from 'react'
import PropertyCard from '@/components/common/PropertyCard'
import './Home.css'

function Home() {
  const [properties, setProperties] = useState([])
  const [loading, setLoading] = useState(true)

  useEffect(() => {
    // Simulate API call - we'll use mock data for now
    setTimeout(() => {
      const mockProperties = [
        {
          id: 1,
          title: 'Cozy Apartment in Downtown',
          location: 'San Francisco, CA',
          price: 120,
          rating: 4.8,
          image: 'https://images.unsplash.com/photo-1502672260266-1c1ef2d93688?w=400'
        },
        {
          id: 2,
          title: 'Modern Loft with City View',
          location: 'New York, NY',
          price: 200,
          rating: 4.9,
          image: 'https://images.unsplash.com/photo-1512918728675-ed5a9ecdebfd?w=400'
        },
        {
          id: 3,
          title: 'Beachfront Villa',
          location: 'Miami, FL',
          price: 350,
          rating: 4.7,
          image: 'https://images.unsplash.com/photo-1613977257363-707ba9348227?w=400'
        }
      ]
      setProperties(mockProperties)
      setLoading(false)
    }, 1000)
  }, [])

  if (loading) {
    return (
      <div className="loading-container">
        <p>Loading properties...</p>
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

**File: `src/pages/Home/Home.css`**
```css
.home {
  padding: var(--spacing-xl);
  max-width: 1400px;
  margin: 0 auto;
}

.home__header {
  margin-bottom: var(--spacing-xl);
}

.home__header h1 {
  font-size: 2rem;
  margin-bottom: var(--spacing-sm);
}

.home__header p {
  color: var(--text-secondary);
  font-size: var(--font-size-lg);
}

.home__grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: var(--spacing-xl);
}

.loading-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 400px;
  font-size: var(--font-size-lg);
  color: var(--text-secondary);
}
```

**File: `src/pages/Home/index.js`**
```javascript
export { default } from './Home'
```

### Step 2: Update App.jsx to Show Home

**File: `src/App.jsx`**
```javascript
import Home from './pages/Home'
import './App.css'

function App() {
  return (
    <div className="App">
      <Home />
    </div>
  )
}

export default App
```

## Screen 2: Header/Navigation Component

Let's create a reusable header component.

**File: `src/components/layout/Header/Header.jsx`**
```javascript
import './Header.css'

function Header() {
  return (
    <header className="header">
      <div className="header__container">
        <div className="header__logo">
          <h2>airbnb</h2>
        </div>
        
        <nav className="header__nav">
          <a href="#" className="header__link">Places to stay</a>
          <a href="#" className="header__link">Experiences</a>
          <a href="#" className="header__link">Online Experiences</a>
        </nav>
        
        <div className="header__user">
          <button className="header__menu-btn">☰</button>
        </div>
      </div>
    </header>
  )
}

export default Header
```

**File: `src/components/layout/Header/Header.css`**
```css
.header {
  border-bottom: 1px solid var(--border-color);
  background-color: var(--bg-white);
  position: sticky;
  top: 0;
  z-index: 100;
}

.header__container {
  max-width: 1400px;
  margin: 0 auto;
  padding: var(--spacing-md) var(--spacing-xl);
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.header__logo h2 {
  color: var(--primary-color);
  font-size: var(--font-size-xl);
  font-weight: 700;
}

.header__nav {
  display: flex;
  gap: var(--spacing-lg);
}

.header__link {
  padding: var(--spacing-sm) var(--spacing-md);
  color: var(--text-primary);
  font-weight: 600;
  border-radius: var(--radius-sm);
  transition: background-color 0.2s;
}

.header__link:hover {
  background-color: var(--bg-light);
}

.header__user {
  display: flex;
  align-items: center;
}

.header__menu-btn {
  padding: var(--spacing-sm) var(--spacing-md);
  border: 1px solid var(--border-color);
  border-radius: var(--radius-md);
  background: none;
  font-size: var(--font-size-lg);
}
```

**File: `src/components/layout/Header/index.js`**
```javascript
export { default } from './Header'
```

Update `App.jsx` to include the Header:

**File: `src/App.jsx`** (updated)
```javascript
import Header from './components/layout/Header'
import Home from './pages/Home'
import './App.css'

function App() {
  return (
    <div className="App">
      <Header />
      <Home />
    </div>
  )
}

export default App
```

## Screen 3: Property Detail Screen

Shows detailed information about a single property.

**File: `src/pages/PropertyDetail/PropertyDetail.jsx`**
```javascript
import { useState, useEffect } from 'react'
import Button from '@/components/common/Button'
import './PropertyDetail.css'

function PropertyDetail({ propertyId }) {
  const [property, setProperty] = useState(null)
  const [loading, setLoading] = useState(true)

  useEffect(() => {
    // Simulate fetching property by ID
    setTimeout(() => {
      const mockProperty = {
        id: propertyId || 1,
        title: 'Cozy Apartment in Downtown',
        location: 'San Francisco, CA',
        price: 120,
        rating: 4.8,
        images: [
          'https://images.unsplash.com/photo-1502672260266-1c1ef2d93688?w=800',
          'https://images.unsplash.com/photo-1512918728675-ed5a9ecdebfd?w=800'
        ],
        description: 'A beautiful and cozy apartment located in the heart of downtown. Perfect for couples or solo travelers.',
        host: 'John Doe',
        guests: 4,
        bedrooms: 2,
        beds: 2,
        baths: 1
      }
      setProperty(mockProperty)
      setLoading(false)
    }, 1000)
  }, [propertyId])

  if (loading) {
    return <div className="loading-container">Loading...</div>
  }

  if (!property) {
    return <div className="error-container">Property not found</div>
  }

  return (
    <div className="property-detail">
      <div className="property-detail__images">
        {property.images.map((image, index) => (
          <img 
            key={index} 
            src={image} 
            alt={`${property.title} ${index + 1}`}
            className="property-detail__image"
          />
        ))}
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

          <div className="property-detail__info">
            <div className="property-detail__spec">
              <span>👥 {property.guests} guests</span>
              <span>🛏️ {property.bedrooms} bedrooms</span>
              <span>🛌 {property.beds} beds</span>
              <span>🚿 {property.baths} baths</span>
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

**File: `src/pages/PropertyDetail/PropertyDetail.css`**
```css
.property-detail {
  max-width: 1400px;
  margin: 0 auto;
  padding: var(--spacing-xl);
}

.property-detail__images {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: var(--spacing-md);
  margin-bottom: var(--spacing-xl);
  border-radius: var(--radius-lg);
  overflow: hidden;
}

.property-detail__image {
  width: 100%;
  height: 400px;
  object-fit: cover;
}

.property-detail__content {
  display: grid;
  grid-template-columns: 2fr 1fr;
  gap: var(--spacing-xl);
}

.property-detail__header h1 {
  font-size: 1.75rem;
  margin-bottom: var(--spacing-sm);
}

.property-detail__meta {
  display: flex;
  gap: var(--spacing-md);
  color: var(--text-secondary);
  margin-bottom: var(--spacing-lg);
}

.property-detail__spec {
  display: flex;
  gap: var(--spacing-lg);
  padding: var(--spacing-lg) 0;
  border-top: 1px solid var(--border-color);
  border-bottom: 1px solid var(--border-color);
}

.property-detail__description {
  margin-top: var(--spacing-xl);
}

.property-detail__description h2 {
  font-size: var(--font-size-xl);
  margin-bottom: var(--spacing-md);
}

.property-detail__sidebar {
  position: sticky;
  top: 80px;
  height: fit-content;
}

.property-detail__booking {
  border: 1px solid var(--border-color);
  border-radius: var(--radius-lg);
  padding: var(--spacing-lg);
}

.property-detail__price {
  margin-bottom: var(--spacing-lg);
}

.property-detail__price .price {
  font-size: 1.5rem;
  font-weight: 600;
}

.property-detail__price .period {
  color: var(--text-secondary);
}

.loading-container,
.error-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 400px;
  font-size: var(--font-size-lg);
}
```

**File: `src/pages/PropertyDetail/index.js`**
```javascript
export { default } from './PropertyDetail'
```

## Screen 4: Search Screen

A screen for displaying search results.

**File: `src/pages/Search/Search.jsx`**
```javascript
import { useState } from 'react'
import PropertyCard from '@/components/common/PropertyCard'
import Input from '@/components/common/Input'
import Button from '@/components/common/Button'
import './Search.css'

function Search() {
  const [searchTerm, setSearchTerm] = useState('')
  const [results, setResults] = useState([])
  const [isSearching, setIsSearching] = useState(false)

  const handleSearch = () => {
    setIsSearching(true)
    // Simulate search
    setTimeout(() => {
      const mockResults = [
        {
          id: 1,
          title: 'Cozy Apartment',
          location: 'San Francisco, CA',
          price: 120,
          rating: 4.8,
          image: 'https://images.unsplash.com/photo-1502672260266-1c1ef2d93688?w=400'
        }
      ]
      setResults(mockResults)
      setIsSearching(false)
    }, 500)
  }

  return (
    <div className="search">
      <div className="search__form">
        <Input
          type="text"
          placeholder="Search destinations..."
          value={searchTerm}
          onChange={(e) => setSearchTerm(e.target.value)}
        />
        <Button onClick={handleSearch} disabled={isSearching}>
          {isSearching ? 'Searching...' : 'Search'}
        </Button>
      </div>

      {results.length > 0 && (
        <div className="search__results">
          <h2>Search Results</h2>
          <div className="search__grid">
            {results.map(property => (
              <PropertyCard
                key={property.id}
                property={property}
              />
            ))}
          </div>
        </div>
      )}
    </div>
  )
}

export default Search
```

**File: `src/pages/Search/Search.css`**
```css
.search {
  max-width: 1400px;
  margin: 0 auto;
  padding: var(--spacing-xl);
}

.search__form {
  display: flex;
  gap: var(--spacing-md);
  margin-bottom: var(--spacing-xl);
  max-width: 600px;
}

.search__form .input {
  flex: 1;
}

.search__results h2 {
  margin-bottom: var(--spacing-lg);
}

.search__grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: var(--spacing-xl);
}
```

**File: `src/pages/Search/index.js`**
```javascript
export { default } from './Search'
```

## Summary

You've now created:
- ✅ Home Screen - Lists all properties
- ✅ Header Component - Navigation bar
- ✅ Property Detail Screen - Shows single property details
- ✅ Search Screen - Search functionality

Each screen is independent and can be tested separately!

## ✅ Checkpoint

You should now have:
- ✅ Multiple screen components created
- ✅ Understanding of how screens are structured
- ✅ Reusable components integrated
- ✅ Basic layout with Header

## Next Steps

Now let's add mock data to make development easier:

**[Working with Mock Data →](./05-mock-data.md)**

---

**Files Created:**
- `src/pages/Home/Home.jsx`
- `src/pages/Home/Home.css`
- `src/pages/Home/index.js`
- `src/components/layout/Header/Header.jsx`
- `src/components/layout/Header/Header.css`
- `src/components/layout/Header/index.js`
- `src/pages/PropertyDetail/PropertyDetail.jsx`
- `src/pages/PropertyDetail/PropertyDetail.css`
- `src/pages/PropertyDetail/index.js`
- `src/pages/Search/Search.jsx`
- `src/pages/Search/Search.css`
- `src/pages/Search/index.js`

