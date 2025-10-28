# HeatSync Labs Website Technical Documentation

## Project Overview

Vue 3 single-page application built with TypeScript and Vite, deployed on Netlify with serverless functions. Serves as the official website for HeatSync Labs, a community hackerspace in Mesa, Arizona.

## Technology Stack

### Frontend
- **Vue 3.5.22**: Component framework with Composition API
- **TypeScript 5.9**: Type safety and enhanced development experience
- **Vite 7.1.11**: Build tool and development server
- **Vue Router 4.6.3**: Client-side routing
- **Pinia 3.0.3**: State management
- **date-fns 4.1.0**: Date manipulation and formatting

### Backend/Serverless
- **Netlify Functions**: Serverless API endpoints
- **Node.js 20.19.0**: Runtime environment

### External APIs
- **Google Calendar API**: Event data retrieval
- **Flickr API**: Photo gallery content

## Project Structure

```
├── src/
│   ├── components/          # Reusable Vue components
│   │   ├── base/           # Foundation components (buttons, containers)
│   │   ├── calendar/       # Calendar-related components
│   │   ├── events/         # Event display components
│   │   ├── graphics/       # Visual/design components
│   │   ├── icons/          # SVG icon components
│   │   ├── info/           # Information display components
│   │   ├── layout/         # Page layout components
│   │   ├── schedule/       # Schedule-related components
│   │   ├── sections/       # Page section components
│   │   └── status/         # Status indicator components
│   ├── services/           # API service classes
│   ├── stores/             # Pinia state management
│   ├── utils/              # Utility functions
│   ├── views/              # Page-level components
│   ├── router/             # Route definitions
│   └── main.ts             # Application entry point
├── netlify/
│   └── functions/          # Serverless API functions
├── public/                 # Static assets
└── dist/                   # Build output (generated)
```

## Component Architecture

### Base Components
Located in `src/components/base/`

#### BaseButton.vue
Button component with variant support (primary, secondary) and size options (sm, md, lg). Supports both router links and external URLs.

**Props:**
- `variant`: "primary" | "secondary" | "ghost"
- `size`: "sm" | "md" | "lg"
- `to`: Vue Router destination
- `href`: External URL
- `target`, `rel`: Standard link attributes

#### BaseContainer.vue
Responsive container component providing consistent max-width and centering across the site.

**Features:**
- Responsive max-width: 1200px
- Automatic horizontal centering
- Horizontal padding for mobile devices

#### BaseCard.vue
Card component for content grouping with consistent styling.

#### DonateButton.vue
PayPal donation button using hosted button integration.

**Configuration:**
- PayPal hosted button ID: `7596RGJWUWZZ4`
- Opens in new tab/window

#### StyledDonateButton.vue
Enhanced donation button with gradient background and animated heart effects.

**Features:**
- Gradient background (rust to sage)
- Floating heart animations on hover
- Responsive design

### Layout Components
Located in `src/components/layout/`

#### AppHeader.vue
Site navigation header with responsive design.

**Features:**
- Desktop horizontal navigation
- Mobile hamburger menu
- Active link highlighting
- External link handling

**Navigation Links:**
- About, Membership, Events, Projects (Flickr), Wiki, Classes, Support Us

#### AppFooter.vue
Site footer with contact information, social links, and resources.

**Sections:**
- Location information
- Social media links
- Resource links (Wiki, Equipment List, Project Gallery, Member Portal)
- Support options

### Calendar Components
Located in `src/components/calendar/`

#### FullCalendar.vue
Complete calendar implementation with month view and event listings.

**Features:**
- Month grid display with 7-day weeks
- Event dot indicators with truncated titles
- Recurring events carousel
- Upcoming events list
- Responsive design (mobile optimized)
- Event modal integration

**Responsive Breakpoints:**
- 1024px: Tablet adjustments
- 768px: Mobile layout (stacked header, smaller text)
- 480px: Compact mobile (50px cells, 8px event text)
- 360px: Ultra-compact (45px cells, 7px event text)

**Event Handling:**
- Filters out "Open Hours" and "Member Hours" from recurring events
- Groups events by title for recurrence detection
- Sorts events chronologically

### Event Components
Located in `src/components/events/`

#### EventCard.vue
Individual event display card for event listings.

**Props:**
- `event`: CalendarEvent object

**Display Elements:**
- Event title and description
- Date and time formatting
- Location information
- Click handler for modal

#### EventModal.vue
Modal popup for detailed event information.

**Props:**
- `visible`: Boolean for modal state
- `event`: CalendarEvent object

**Features:**
- Overlay background
- Close button and escape key handling
- Event details display

### Graphics Components
Located in `src/components/graphics/`

#### PhotoCollage.vue
Animated photo collage using Flickr API integration.

**Features:**
- 6-photo grid layout (3x2)
- Featured single image mode
- Automatic rotation (6s collage, 3s featured)
- Fade transitions
- Image lazy loading
- Error handling for failed images

**Responsive Design:**
- Desktop: 3x2 grid
- Tablet (1024px): Reduced spacing
- Mobile (768px): Hidden (replaced by MobilePhotoCarousel)

#### MobilePhotoCarousel.vue
Mobile-optimized single image carousel.

**Features:**
- Single image display with 4-second rotation
- Smooth fade transitions
- Loading spinner
- Error handling and auto-skip
- Responsive height (300px desktop, 250px mobile)

**Usage:**
- Only visible on screens ≤768px
- Positioned below hero content
- Uses same Flickr service as PhotoCollage

#### GeometricPattern.vue
SVG-based decorative geometric pattern component.

### Service Layer
Located in `src/services/`

#### calendarService.ts
Google Calendar API integration service.

**Class: CalendarService**

**Methods:**
- `getEventsForMonth(date: Date)`: Retrieves events for specific month
- `getRecurringEvents(days: number)`: Gets upcoming recurring events
- `getAllEvents(days: number)`: Retrieves all events for date range

**API Integration:**
- Uses Netlify function `/netlify/functions/calendar-api`
- Processes Google Calendar API responses
- Filters and formats event data
- Handles timezone conversion

**Event Processing:**
- Converts ISO date strings to Date objects
- Determines all-day vs. timed events
- Extracts location and description data

#### flickrService.ts
Flickr API integration service.

**Class: FlickrService**

**Methods:**
- `getPhotos(limit: number)`: Retrieves photos from Flickr

**Configuration:**
- User ID: `60827818@N07`
- Tag filter: `publish`
- Format: REST XML

**API Integration:**
- Uses Netlify function `/netlify/functions/flickr-api`
- Parses XML response using DOMParser
- Constructs image URLs using Flickr URL patterns
- Returns structured photo objects with thumbnails and full-size URLs

**Image URL Construction:**
- Thumbnail: `_m.jpg` suffix
- Full size: `_b.jpg` suffix
- Pattern: `https://farm{farm}.staticflickr.com/{server}/{id}_{secret}_{size}.jpg`

### Utility Functions
Located in `src/utils/`

#### icalParser.ts
iCalendar (RFC 5545) parsing utilities for calendar data processing.

### State Management
Located in `src/stores/`

#### counter.ts
Example Pinia store (can be removed if unused).

### Views (Pages)
Located in `src/views/`

#### HomeView.vue
Landing page with hero section, info grid, and schedule.

**Sections:**
- HeroSection (with PhotoCollage/MobilePhotoCarousel)
- InfoSection
- ScheduleSection

#### AboutView.vue
About page with organization information.

#### MembershipView.vue
Membership information and application process.

**Features:**
- Membership tier explanations (Associate $25, Basic $50, Plus $100)
- Benefits listing
- Application process steps
- Code of conduct reference

#### CalendarView.vue
Full calendar page implementation.

**Features:**
- FullCalendar component integration
- Event filtering and display
- Recurring events showcase

#### ClassesView.vue
Classes and educational program information.

#### SupportView.vue
Support and donation page.

**Support Methods:**
- Monthly membership signup
- One-time PayPal donations
- Cash/check donations
- eBay store purchases
- Equipment donations
- Corporate sponsorship

#### RegisterView.vue
User registration page (if applicable).

## Netlify Functions

Located in `netlify/functions/`

### calendar-api.js
Serverless function for Google Calendar API proxy.

**Endpoint:** `/.netlify/functions/calendar-api`

**Environment Variables Required:**
- `GOOGLE_API_KEY`: Google Calendar API key
- `CALENDAR_ID`: Google Calendar ID

**Parameters:**
- `timeMin`: ISO date string for start range
- `timeMax`: ISO date string for end range
- `maxResults`: Maximum number of events (default: 50)

**Functionality:**
- Proxies requests to Google Calendar API
- Adds CORS headers for frontend access
- Handles API key authentication
- Returns formatted JSON response

**Error Handling:**
- Missing environment variables
- Google API errors
- Network request failures

### flickr-api.js
Serverless function for Flickr API proxy.

**Endpoint:** `/.netlify/functions/flickr-api`

**Parameters:**
- `limit`: Number of photos to retrieve (default: 20)

**Functionality:**
- Proxies requests to Flickr API
- Fetches XML response from Flickr REST API
- Adds CORS headers
- Returns wrapped response for frontend consumption

**Configuration:**
- API Key: `bec64c9c0f28889dc6e0c5ef7be3511f`
- User ID: `60827818@N07`
- Tag Filter: `publish`

## Environment Variables

### Required for Netlify Functions
```
GOOGLE_API_KEY=your_google_calendar_api_key
CALENDAR_ID=your_google_calendar_id
```

### Build Environment
```
NODE_VERSION=20.19.0
```

## Development Setup

### Prerequisites
- Node.js 20.19.0 or higher
- npm 10.8.2 or higher

### Installation
```bash
# Clone repository
git clone https://github.com/virgilvox/heatsynclabs-org
cd heatsynclabs-org

# Install dependencies
npm install

# Create environment file
cp .env.example .env
# Edit .env with required API keys
```

### Development Server
```bash
# Start Vite development server
npm run dev

# Start Netlify development server (includes functions)
netlify dev
```

### Available Scripts
```bash
npm run dev          # Start Vite development server
npm run build        # Build for production
npm run preview      # Preview production build
npm run type-check   # Run TypeScript type checking
```

## Deployment

### Netlify Configuration
Configuration defined in `netlify.toml`:

```toml
[build]
  publish = "dist"
  command = "npm run build"
  functions = "netlify/functions"

[build.environment]
  NODE_VERSION = "20.19.0"

[dev]
  command = "npm run dev"
  port = 8888
  targetPort = 5173
  publish = "dist"
  autoLaunch = false

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

### Build Process
1. `npm run build` executes
2. Runs TypeScript type checking (`vue-tsc --build`)
3. Runs Vite build (`vite build`)
4. Outputs to `dist/` directory
5. Deploys static files and functions to Netlify

### Environment Configuration
Set the following environment variables in Netlify dashboard:
- `GOOGLE_API_KEY`
- `CALENDAR_ID`

### Deployment Steps
1. Push code to main branch
2. Netlify automatically detects changes
3. Runs build process
4. Deploys to production URL

### Manual Deployment
```bash
# Build locally
npm run build

# Deploy using Netlify CLI
netlify deploy --prod --dir=dist
```

## Responsive Design

### Breakpoints
- **1024px**: Tablet adjustments
- **768px**: Mobile layout changes
- **480px**: Compact mobile design
- **360px**: Ultra-compact for small devices

### Mobile Optimizations
- Collapsible navigation menu
- Photo collage replaced with carousel
- Calendar grid optimization
- Touch-friendly button sizes
- Reduced font sizes and spacing

## API Integration

### Google Calendar
- **Purpose**: Event data retrieval
- **Authentication**: API key-based
- **Rate Limits**: Standard Google API limits
- **Data Format**: Google Calendar API v3 JSON

### Flickr
- **Purpose**: Photo gallery content
- **Authentication**: Public API key
- **Data Format**: XML (parsed to JSON)
- **Image Sizes**: Multiple sizes available

## Performance Considerations

### Image Optimization
- Lazy loading for Flickr images
- Multiple image sizes (thumbnail, full)
- Error handling for failed loads

### API Caching
- No client-side caching implemented
- Relies on browser and CDN caching

### Bundle Size
- Vite tree-shaking for unused code elimination
- Dynamic imports where applicable

## Security

### API Key Protection
- API keys stored as Netlify environment variables
- No client-side API key exposure
- Serverless functions act as proxy

### CORS Configuration
- Netlify functions include appropriate CORS headers
- Frontend restricted to same-origin requests to functions

## Browser Support

### Minimum Requirements
- Modern browsers supporting ES2020+
- Vue 3 browser compatibility
- CSS Grid and Flexbox support

### Tested Browsers
- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

## Troubleshooting

### Common Issues

#### Build Failures
- **Node Version**: Ensure Node.js 20.19.0+ is used
- **Dependencies**: Run `npm install` for fresh dependencies
- **Environment Variables**: Verify all required env vars are set

#### API Errors
- **Calendar API**: Check `GOOGLE_API_KEY` and `CALENDAR_ID`
- **Flickr API**: Verify Flickr service configuration
- **CORS**: Ensure functions return proper CORS headers

#### Development Server
- **Port Conflicts**: Netlify dev uses port 8888, Vite uses 5173
- **Function Testing**: Use `/.netlify/functions/` prefix for local testing

### Debugging
```bash
# Check Netlify function logs
netlify dev

# Run type checking
npm run type-check

# Build locally to test
npm run build
```

## Maintenance

### Regular Tasks
- Update dependencies monthly
- Monitor API rate limits
- Review Netlify function performance
- Update content as needed

### Dependency Updates
```bash
# Check for updates
npm outdated

# Update dependencies
npm update

# Update to latest major versions (with caution)
npx npm-check-updates -u
npm install
```

### Performance Monitoring
- Netlify Analytics for traffic
- Browser DevTools for frontend performance
- Netlify Function logs for backend monitoring
