# Hopwise App - Data Structures & API Integration Guide

## Overview

This document defines the JSON data structures expected by the Hopwise frontend for seamless backend integration.

---

## 1. Place/Restaurant Object

Used in: Search results, Trending, Saved items, Detail pages

```json
{
  "id": "place-123",
  "name": "Joe's Pizza",
  "category": "Italian",
  "subcategory": "Pizza",
  "priceLevel": "$",
  "distance": "0.3 mi",
  "rating": 4.8,
  "reviewCount": 2347,
  "tags": ["🔥 Trending", "👥 Popular"],
  "badge": "🏆 #1",
  "image": "https://example.com/images/joes-pizza.jpg",
  "gradient": "linear-gradient(135deg, #FFE5B4, #FFB347)",
  "isOpen": true,
  "hours": "Open · Closes 4:00 AM",
  "address": "7 Carmine St, New York, NY 10014",
  "phone": "(212) 366-1182",
  "website": "https://joespizzanyc.com",
  "coordinates": {
    "lat": 40.7305,
    "lng": -74.0027
  },
  "badges": ["🏆 #1 Pizza in NYC", "📸 2.3k photos"],
  "photos": [
    "https://example.com/photo1.jpg",
    "https://example.com/photo2.jpg"
  ]
}
```

### Field Descriptions

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | string | ✅ | Unique identifier |
| name | string | ✅ | Place name |
| category | string | ✅ | Primary category (e.g., "Italian", "Japanese") |
| subcategory | string | ❌ | Secondary category (e.g., "Pizza", "Sushi") |
| priceLevel | string | ✅ | Price range: "$", "$$", "$$$", "$$$$" |
| distance | string | ✅ | Distance from user (e.g., "0.3 mi") |
| rating | number | ✅ | Rating 0-5 |
| reviewCount | number | ❌ | Number of reviews |
| tags | array | ❌ | Display tags with emojis |
| badge | string | ❌ | Special badge (e.g., "#1", "New") |
| image | string | ❌ | Image URL (fallback to gradient) |
| gradient | string | ❌ | CSS gradient for placeholder |
| isOpen | boolean | ❌ | Currently open |
| hours | string | ❌ | Hours display string |
| address | string | ✅ | Full address |
| phone | string | ❌ | Phone number |
| coordinates | object | ✅ | Lat/lng for map |

---

## 2. Ride Option Object

Used in: Ride comparison page

```json
{
  "provider": "Uber",
  "name": "Uber X",
  "type": "Standard",
  "seats": 4,
  "price": 45,
  "priceRange": {
    "min": 42,
    "max": 48
  },
  "pickup": 5,
  "duration": 42,
  "rating": 4.9,
  "surge": null,
  "promoCode": "HOPWISE20",
  "promoDiscount": 0.2,
  "deepLink": "uber://riderequest?pickup=...",
  "estimatedArrival": "2024-01-15T14:45:00Z"
}
```

### Field Descriptions

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| provider | string | ✅ | "Uber", "Lyft", "Via", etc. |
| name | string | ✅ | Service name (e.g., "Uber X", "Lyft") |
| type | string | ✅ | "Standard", "Premium", "Shared" |
| seats | number | ✅ | Number of seats |
| price | number | ✅ | Estimated price in USD |
| priceRange | object | ❌ | Min/max price range |
| pickup | number | ✅ | Pickup time in minutes |
| duration | number | ✅ | Trip duration in minutes |
| rating | number | ❌ | Driver rating average |
| surge | number | ❌ | Surge multiplier (e.g., 1.2) |
| promoCode | string | ❌ | Available promo code |
| promoDiscount | number | ❌ | Discount percentage (0-1) |
| deepLink | string | ✅ | Deep link to open provider app |

---

## 3. Search Request

```json
{
  "query": "pizza",
  "category": "food",
  "filters": {
    "priceLevel": ["$", "$$"],
    "rating": 4.0,
    "distance": 1.0,
    "openNow": true
  },
  "sort": "rating",
  "limit": 20,
  "offset": 0,
  "location": {
    "lat": 40.7580,
    "lng": -73.9855
  }
}
```

---

## 4. Search Response

```json
{
  "success": true,
  "data": {
    "results": [
      { /* Place object */ },
      { /* Place object */ }
    ],
    "total": 156,
    "page": 1,
    "hasMore": true,
    "aiRecommendation": {
      "placeId": "place-123",
      "reason": "Highest rated pizza near you with quick service"
    }
  },
  "meta": {
    "searchTime": 0.234,
    "query": "pizza"
  }
}
```

---

## 5. Ride Comparison Request

```json
{
  "origin": {
    "address": "Times Square, NYC",
    "lat": 40.7580,
    "lng": -73.9855
  },
  "destination": {
    "address": "JFK Airport Terminal 4",
    "lat": 40.6413,
    "lng": -73.7781
  },
  "passengers": 1,
  "rideType": "standard"
}
```

---

## 6. Ride Comparison Response

```json
{
  "success": true,
  "data": {
    "rides": [
      { /* Ride object */ },
      { /* Ride object */ }
    ],
    "tripInfo": {
      "distance": "18.5 mi",
      "duration": "42 min",
      "savings": 7
    },
    "recommendation": {
      "provider": "Uber",
      "reason": "$7 cheaper than Lyft with similar pickup time"
    }
  }
}
```

---

## 7. User Object

```json
{
  "id": "user-456",
  "name": "Alex Johnson",
  "email": "alex.johnson@email.com",
  "avatar": "A",
  "avatarUrl": null,
  "stats": {
    "trips": 12,
    "saved": 48,
    "totalSavings": 127
  },
  "preferences": {
    "homeLocation": "New York, NY",
    "currency": "USD",
    "darkMode": false,
    "notifications": true
  },
  "subscription": {
    "plan": "free",
    "expiresAt": null
  },
  "savedItems": ["place-123", "place-456"]
}
```

---

## 8. API Endpoints (Suggested)

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/places/search` | POST | Search places |
| `/api/places/:id` | GET | Get place details |
| `/api/rides/compare` | POST | Compare ride options |
| `/api/user/profile` | GET | Get user profile |
| `/api/user/saved` | GET/POST/DELETE | Manage saved items |

---

## 9. Element IDs for Data Binding

### Home Page
- `#home-greeting` - Greeting text
- `#trending-container` - Trending items container
- `#activities-container` - Activities grid

### Search Page
- `#search-input` - Search input field
- `#search-results` - Results list container
- `#results-count` - Results count text
- `#filter-chips` - Filter buttons

### Detail Page
- `#detail-container` - Full detail content

### Rides Page
- `#ride-pickup` - Pickup address
- `#ride-destination` - Destination address
- `#ride-distance` - Trip distance
- `#ride-time` - Trip time
- `#ride-savings` - Savings amount
- `#rides-container` - Ride cards container
- `#ride-recommendation` - AI recommendation banner
- `#book-ride-btn` - Book button

### Saved Page
- `#saved-container` - Saved items container
- `#saved-tabs` - Category tabs

### Profile Page
- `#profile-avatar` - Avatar initial/image
- `#profile-name` - User name
- `#profile-email` - User email
- `#stat-trips` - Trip count
- `#stat-saved` - Saved count
- `#stat-savings` - Total savings

---

## 10. Error Responses

```json
{
  "success": false,
  "error": {
    "code": "RATE_LIMIT",
    "message": "Too many requests. Please try again later.",
    "retryAfter": 60
  }
}
```

### Error Codes
- `NOT_FOUND` - Resource not found
- `INVALID_REQUEST` - Bad request parameters
- `RATE_LIMIT` - Rate limit exceeded
- `API_ERROR` - Third-party API error
- `AUTH_REQUIRED` - Authentication needed
- `SERVER_ERROR` - Internal server error

---

## 11. Integration Checklist

- [ ] Replace mock data in `app.js` with actual API calls
- [ ] Implement error handling for API failures
- [ ] Add authentication headers to requests
- [ ] Implement pagination for search results
- [ ] Add deep linking for ride providers
- [ ] Cache frequent requests (places, user data)
- [ ] Implement offline support with localStorage
