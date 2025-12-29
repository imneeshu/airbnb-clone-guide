# 5. Working with Mock Data

Mock data is essential during development. It allows you to build and test your UI without needing a real backend API. In this section, we'll create a structured mock data system for our Airbnb clone.

## Why Use Mock Data?

- 🚀 **Faster Development** - No need to wait for API
- 🧪 **Testing** - Test UI with various data scenarios
- 🎨 **Design** - Design UI before backend is ready
- 🔄 **Independence** - Frontend and backend teams can work separately

## Creating Mock Data Structure

Let's create comprehensive mock data for our properties.

**File: `src/data/mockProperties.js`**
```javascript
// Mock properties data
export const mockProperties = [
  {
    id: 1,
    title: 'Cozy Apartment in Downtown',
    location: 'San Francisco, CA',
    address: '123 Market Street',
    price: 120,
    rating: 4.8,
    reviewCount: 127,
    image: 'https://images.unsplash.com/photo-1502672260266-1c1ef2d93688?w=800',
    images: [
      'https://images.unsplash.com/photo-1502672260266-1c1ef2d93688?w=800',
      'https://images.unsplash.com/photo-1522708323590-d24dbb6b0267?w=800',
      'https://images.unsplash.com/photo-1484154218962-a197022b5858?w=800'
    ],
    description: 'A beautiful and cozy apartment located in the heart of downtown San Francisco. Perfect for couples or solo travelers looking to explore the city.',
    host: {
      name: 'John Doe',
      avatar: 'https://images.unsplash.com/photo-1507003211169-0a1dd7228f2d?w=100',
      isSuperhost: true,
      joinedDate: '2018'
    },
    specs: {
      guests: 4,
      bedrooms: 2,
      beds: 2,
      baths: 1
    },
    amenities: ['WiFi', 'Kitchen', 'Parking', 'Washer', 'Dryer'],
    coordinates: {
      lat: 37.7749,
      lng: -122.4194
    }
  },
  {
    id: 2,
    title: 'Modern Loft with City View',
    location: 'New York, NY',
    address: '456 Broadway',
    price: 200,
    rating: 4.9,
    reviewCount: 89,
    image: 'https://images.unsplash.com/photo-1512918728675-ed5a9ecdebfd?w=800',
    images: [
      'https://images.unsplash.com/photo-1512918728675-ed5a9ecdebfd?w=800',
      'https://images.unsplash.com/photo-1522771739844-6a9f6d5f14af?w=800'
    ],
    description: 'Stunning modern loft with breathtaking city views. Located in the vibrant heart of Manhattan.',
    host: {
      name: 'Jane Smith',
      avatar: 'https://images.unsplash.com/photo-1494790108377-be9c29b29330?w=100',
      isSuperhost: true,
      joinedDate: '2019'
    },
    specs: {
      guests: 6,
      bedrooms: 3,
      beds: 3,
      baths: 2
    },
    amenities: ['WiFi', 'Kitchen', 'Air Conditioning', 'TV', 'Elevator'],
    coordinates: {
      lat: 40.7128,
      lng: -74.0060
    }
  },
  {
    id: 3,
    title: 'Beachfront Villa',
    location: 'Miami, FL',
    address: '789 Ocean Drive',
    price: 350,
    rating: 4.7,
    reviewCount: 156,
    image: 'https://images.unsplash.com/photo-1613977257363-707ba9348227?w=800',
    images: [
      'https://images.unsplash.com/photo-1613977257363-707ba9348227?w=800',
      'https://images.unsplash.com/photo-1571896349842-33c89424de2d?w=800'
    ],
    description: 'Luxurious beachfront villa with private access to the beach. Perfect for families and groups.',
    host: {
      name: 'Mike Johnson',
      avatar: 'https://images.unsplash.com/photo-1500648767791-00dcc994a43e?w=100',
      isSuperhost: false,
      joinedDate: '2020'
    },
    specs: {
      guests: 8,
      bedrooms: 4,
      beds: 5,
      baths: 3
    },
    amenities: ['WiFi', 'Kitchen', 'Pool', 'Beach Access', 'Parking', 'TV'],
    coordinates: {
      lat: 25.7617,
      lng: -80.1918
    }
  },
  {
    id: 4,
    title: 'Mountain Cabin Retreat',
    location: 'Aspen, CO',
    address: '321 Mountain View Road',
    price: 250,
    rating: 4.9,
    reviewCount: 203,
    image: 'https://images.unsplash.com/photo-1542718610-a1d656d1884c?w=800',
    images: [
      'https://images.unsplash.com/photo-1542718610-a1d656d1884c?w=800',
      'https://images.unsplash.com/photo-1582268611958-ebfd161ef9cf?w=800'
    ],
    description: 'Cozy mountain cabin with fireplace and stunning mountain views. Ideal for winter getaways.',
    host: {
      name: 'Sarah Williams',
      avatar: 'https://images.unsplash.com/photo-1438761681033-6461ffad8d80?w=100',
      isSuperhost: true,
      joinedDate: '2017'
    },
    specs: {
      guests: 6,
      bedrooms: 3,
      beds: 4,
      baths: 2
    },
    amenities: ['WiFi', 'Kitchen', 'Fireplace', 'Parking', 'Mountain View'],
    coordinates: {
      lat: 39.1911,
      lng: -106.8175
    }
  },
  {
    id: 5,
    title: 'Urban Studio Apartment',
    location: 'Seattle, WA',
    address: '654 Pine Street',
    price: 95,
    rating: 4.6,
    reviewCount: 94,
    image: 'https://images.unsplash.com/photo-1522771739844-6a9f6d5f14af?w=800',
    images: [
      'https://images.unsplash.com/photo-1522771739844-6a9f6d5f14af?w=800',
      'https://images.unsplash.com/photo-1493809842364-78817add7ffb?w=800'
    ],
    description: 'Modern studio apartment in the heart of Seattle. Great for solo travelers or couples.',
    host: {
      name: 'David Brown',
      avatar: 'https://images.unsplash.com/photo-1506794778202-cad84cf45f1d?w=100',
      isSuperhost: false,
      joinedDate: '2021'
    },
    specs: {
      guests: 2,
      bedrooms: 1,
      beds: 1,
      baths: 1
    },
    amenities: ['WiFi', 'Kitchen', 'TV', 'Washer'],
    coordinates: {
      lat: 47.6062,
      lng: -122.3321
    }
  },
  {
    id: 6,
    title: 'Historic Brownstone',
    location: 'Boston, MA',
    address: '987 Beacon Street',
    price: 180,
    rating: 4.8,
    reviewCount: 112,
    image: 'https://images.unsplash.com/photo-1484154218962-a197022b5858?w=800',
    images: [
      'https://images.unsplash.com/photo-1484154218962-a197022b5858?w=800',
      'https://images.unsplash.com/photo-1560448204-e02f11c3d0e2?w=800'
    ],
    description: 'Beautifully restored historic brownstone with original features and modern amenities.',
    host: {
      name: 'Emily Davis',
      avatar: 'https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=100',
      isSuperhost: true,
      joinedDate: '2016'
    },
    specs: {
      guests: 5,
      bedrooms: 2,
      beds: 3,
      baths: 2
    },
    amenities: ['WiFi', 'Kitchen', 'Parking', 'Washer', 'Dryer', 'TV'],
    coordinates: {
      lat: 42.3601,
      lng: -71.0589
    }
  }
]

// Helper function to get property by ID
export const getPropertyById = (id) => {
  return mockProperties.find(property => property.id === id)
}

// Helper function to search properties
export const searchProperties = (query) => {
  const lowerQuery = query.toLowerCase()
  return mockProperties.filter(property => 
    property.title.toLowerCase().includes(lowerQuery) ||
    property.location.toLowerCase().includes(lowerQuery) ||
    property.description.toLowerCase().includes(lowerQuery)
  )
}

// Helper function to get properties by location
export const getPropertiesByLocation = (location) => {
  return mockProperties.filter(property => 
    property.location.toLowerCase().includes(location.toLowerCase())
  )
}
```

## Using Mock Data in Components

Now let's update our Home component to use this mock data:

**File: `src/pages/Home/Home.jsx`** (updated)
```javascript
import { useState, useEffect } from 'react'
import PropertyCard from '@/components/common/PropertyCard'
import { mockProperties } from '@/data/mockProperties'
import './Home.css'

function Home() {
  const [properties, setProperties] = useState([])
  const [loading, setLoading] = useState(true)

  useEffect(() => {
    // Simulate API delay
    setTimeout(() => {
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

Update PropertyDetail to use the helper function:

**File: `src/pages/PropertyDetail/PropertyDetail.jsx`** (updated)
```javascript
import { useState, useEffect } from 'react'
import Button from '@/components/common/Button'
import { getPropertyById } from '@/data/mockProperties'
import './PropertyDetail.css'

function PropertyDetail({ propertyId = 1 }) {
  const [property, setProperty] = useState(null)
  const [loading, setLoading] = useState(true)

  useEffect(() => {
    // Simulate API call
    setTimeout(() => {
      const foundProperty = getPropertyById(propertyId)
      setProperty(foundProperty)
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
        {property.images?.map((image, index) => (
          <img 
            key={index} 
            src={image} 
            alt={`${property.title} ${index + 1}`}
            className="property-detail__image"
          />
        )) || (
          <img 
            src={property.image} 
            alt={property.title}
            className="property-detail__image"
          />
        )}
      </div>

      <div className="property-detail__content">
        <div className="property-detail__main">
          <div className="property-detail__header">
            <h1>{property.title}</h1>
            <div className="property-detail__meta">
              <span>★ {property.rating} ({property.reviewCount} reviews)</span>
              <span>{property.location}</span>
            </div>
          </div>

          <div className="property-detail__info">
            <div className="property-detail__spec">
              <span>👥 {property.specs?.guests} guests</span>
              <span>🛏️ {property.specs?.bedrooms} bedrooms</span>
              <span>🛌 {property.specs?.beds} beds</span>
              <span>🚿 {property.specs?.baths} baths</span>
            </div>
          </div>

          {property.amenities && property.amenities.length > 0 && (
            <div className="property-detail__amenities">
              <h2>Amenities</h2>
              <div className="amenities-list">
                {property.amenities.map((amenity, index) => (
                  <span key={index} className="amenity-tag">{amenity}</span>
                ))}
              </div>
            </div>
          )}

          <div className="property-detail__description">
            <h2>About this place</h2>
            <p>{property.description}</p>
          </div>

          {property.host && (
            <div className="property-detail__host">
              <h2>Meet your host</h2>
              <div className="host-info">
                <img src={property.host.avatar} alt={property.host.name} />
                <div>
                  <h3>{property.host.name}</h3>
                  <p>Joined in {property.host.joinedDate}</p>
                  {property.host.isSuperhost && <span className="superhost">Superhost</span>}
                </div>
              </div>
            </div>
          )}
        </div>

        <div className="property-detail__sidebar">
          <div className="property-detail__booking">
            <div className="property-detail__price">
              <span className="price">${property.price}</span>
              <span className="period">/ night</span>
            </div>
            <Button variant="primary" size="large" style={{ width: '100%', marginTop: '16px' }}>
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

Add some additional CSS for the new elements:

**File: `src/pages/PropertyDetail/PropertyDetail.css`** (add these styles)
```css
.property-detail__amenities {
  margin-top: var(--spacing-xl);
  padding-top: var(--spacing-xl);
  border-top: 1px solid var(--border-color);
}

.amenities-list {
  display: flex;
  flex-wrap: wrap;
  gap: var(--spacing-md);
  margin-top: var(--spacing-md);
}

.amenity-tag {
  padding: var(--spacing-sm) var(--spacing-md);
  background-color: var(--bg-light);
  border-radius: var(--radius-sm);
  font-size: var(--font-size-sm);
}

.property-detail__host {
  margin-top: var(--spacing-xl);
  padding-top: var(--spacing-xl);
  border-top: 1px solid var(--border-color);
}

.host-info {
  display: flex;
  gap: var(--spacing-md);
  margin-top: var(--spacing-md);
}

.host-info img {
  width: 64px;
  height: 64px;
  border-radius: 50%;
  object-fit: cover;
}

.superhost {
  display: inline-block;
  margin-top: var(--spacing-xs);
  padding: 4px 8px;
  background-color: var(--primary-color);
  color: white;
  border-radius: var(--radius-sm);
  font-size: var(--font-size-sm);
  font-weight: 600;
}
```

Update Search component to use the search helper:

**File: `src/pages/Search/Search.jsx`** (updated)
```javascript
import { useState } from 'react'
import PropertyCard from '@/components/common/PropertyCard'
import Input from '@/components/common/Input'
import Button from '@/components/common/Button'
import { searchProperties } from '@/data/mockProperties'
import './Search.css'

function Search() {
  const [searchTerm, setSearchTerm] = useState('')
  const [results, setResults] = useState([])
  const [isSearching, setIsSearching] = useState(false)

  const handleSearch = () => {
    if (!searchTerm.trim()) return
    
    setIsSearching(true)
    // Simulate search delay
    setTimeout(() => {
      const searchResults = searchProperties(searchTerm)
      setResults(searchResults)
      setIsSearching(false)
    }, 500)
  }

  const handleKeyPress = (e) => {
    if (e.key === 'Enter') {
      handleSearch()
    }
  }

  return (
    <div className="search">
      <div className="search__header">
        <h1>Search Properties</h1>
        <p>Find your perfect stay</p>
      </div>

      <div className="search__form">
        <Input
          type="text"
          placeholder="Search by location, title, or description..."
          value={searchTerm}
          onChange={(e) => setSearchTerm(e.target.value)}
          onKeyPress={handleKeyPress}
        />
        <Button onClick={handleSearch} disabled={isSearching}>
          {isSearching ? 'Searching...' : 'Search'}
        </Button>
      </div>

      {results.length > 0 ? (
        <div className="search__results">
          <h2>Found {results.length} properties</h2>
          <div className="search__grid">
            {results.map(property => (
              <PropertyCard
                key={property.id}
                property={property}
              />
            ))}
          </div>
        </div>
      ) : searchTerm && !isSearching ? (
        <div className="search__no-results">
          <p>No properties found matching "{searchTerm}"</p>
        </div>
      ) : null}
    </div>
  )
}

export default Search
```

## Benefits of This Approach

1. **Centralized Data** - All mock data in one place
2. **Easy to Update** - Change data structure in one file
3. **Helper Functions** - Reusable utility functions
4. **Easy Migration** - When ready, replace mock functions with real API calls

## Transitioning to Real API

When you're ready to use a real API, you can:
1. Keep the same data structure
2. Replace mock functions with API calls
3. Your components won't need major changes

## ✅ Checkpoint

You should now have:
- ✅ Comprehensive mock data structure
- ✅ Helper functions for data operations
- ✅ Components using mock data
- ✅ Understanding of mock data benefits

## Next Steps

Now let's learn how to make real API calls:

**[Fetch & Axios →](./06-fetch-axios.md)**

---

**Files Created/Updated:**
- `src/data/mockProperties.js` (NEW)
- `src/pages/Home/Home.jsx` (UPDATED)
- `src/pages/PropertyDetail/PropertyDetail.jsx` (UPDATED)
- `src/pages/PropertyDetail/PropertyDetail.css` (UPDATED)
- `src/pages/Search/Search.jsx` (UPDATED)

