# 🏗️ KFAR Marketplace - Architecture & Technical Documentation

[← Back to Dashboard](./README.md) | [Accomplishments →](./ACCOMPLISHMENTS_REPORT.md)

---

<div align="center">
  
  ## 🔧 Technical Architecture Overview
  
  **Modern** | **Scalable** | **Secure** | **AI-Powered**
  
</div>

---

## 📊 System Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                          KFAR MARKETPLACE                           │
│                    AI-Powered Voice Commerce Platform               │
└─────────────────────────────────────────────────────────────────────┘
                                    │
        ┌───────────────────────────┴───────────────────────────┐
        │                                                       │
┌───────▼─────────┐                                   ┌─────────▼────────┐
│   Frontend      │                                   │   Voice Layer    │
│   Next.js 15    │◄──────────────────────────────────┤  Web Speech API  │
│   React 19      │                                   │  ElevenLabs TTS  │
│   TypeScript    │                                   └──────────────────┘
└───────┬─────────┘                                            
        │                                                       
        │◄────────────────────────┐                            
        │                         │                            
┌───────▼─────────┐      ┌────────┴────────┐         ┌──────────────────┐
│   API Layer     │      │   AI Services   │         │  External APIs   │
│  Edge Functions │◄─────┤  OpenRouter     │         │  SendGrid        │
│  Middleware     │      │  Gemini 2.0     │         │  Payment Gateway │
└───────┬─────────┘      └─────────────────┘         └──────────────────┘
        │                                                       
┌───────▼─────────────────────────────────────────────────────────────┐
│                         Database Layer                              │
│                      Supabase (PostgreSQL)                         │
│  ┌─────────────┐  ┌──────────────┐  ┌───────────────┐            │
│  │   Users     │  │   Products   │  │    Orders     │            │
│  └─────────────┘  └──────────────┘  └───────────────┘            │
│  ┌─────────────┐  ┌──────────────┐  ┌───────────────┐            │
│  │  Vendors    │  │     Cart     │  │   Analytics   │            │
│  └─────────────┘  └──────────────┘  └───────────────┘            │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Technology Stack

### Frontend Technologies

<details open>
<summary><strong>Core Framework</strong></summary>

```typescript
// Next.js 15 App Router Configuration
{
  "framework": "Next.js 15.0.3",
  "features": [
    "App Router",
    "Server Components",
    "Server Actions",
    "Streaming SSR",
    "Partial Prerendering"
  ]
}
```

</details>

<details>
<summary><strong>UI Libraries</strong></summary>

- **React 19**: Latest features including use() hook
- **TypeScript 5.3**: Type safety throughout
- **Tailwind CSS 3.4**: Utility-first styling
- **Radix UI**: Accessible component primitives
- **Framer Motion**: Smooth animations

</details>

### Backend Architecture

<details>
<summary><strong>Database Schema</strong></summary>

```sql
-- Core Tables Structure
CREATE TABLE users (
  id UUID PRIMARY KEY,
  email VARCHAR UNIQUE,
  phone VARCHAR,
  preferred_language VARCHAR,
  voice_enabled BOOLEAN
);

CREATE TABLE vendors (
  id UUID PRIMARY KEY,
  name VARCHAR,
  description TEXT,
  location JSONB,
  ratings DECIMAL
);

CREATE TABLE products (
  id UUID PRIMARY KEY,
  vendor_id UUID REFERENCES vendors(id),
  name VARCHAR,
  price DECIMAL,
  voice_keywords TEXT[]
);

CREATE TABLE orders (
  id UUID PRIMARY KEY,
  user_id UUID REFERENCES users(id),
  vendor_id UUID REFERENCES vendors(id),
  total DECIMAL,
  voice_order BOOLEAN
);
```

</details>

<details>
<summary><strong>API Structure</strong></summary>

```typescript
// API Route Structure
/api/
├── auth/
│   ├── login
│   ├── register
│   └── logout
├── products/
│   ├── search
│   ├── voice-search
│   └── recommendations
├── orders/
│   ├── create
│   ├── track
│   └── history
└── vendors/
    ├── list
    ├── products
    └── analytics
```

</details>

### AI & Voice Integration

<details>
<summary><strong>Voice Processing Pipeline</strong></summary>

```javascript
// Voice Processing Flow
const voiceWorkflow = {
  1: "Capture Audio (Web Speech API)",
  2: "Speech to Text Conversion",
  3: "Natural Language Processing",
  4: "Intent Classification",
  5: "Action Execution",
  6: "Response Generation",
  7: "Text to Speech (ElevenLabs)"
};
```

</details>

<details>
<summary><strong>AI Model Configuration</strong></summary>

```typescript
// OpenRouter Configuration
const aiConfig = {
  model: "google/gemini-2.0-flash-thinking-exp",
  temperature: 0.7,
  maxTokens: 1024,
  features: [
    "product_search",
    "recommendations",
    "cart_management",
    "conversational_ui"
  ]
};
```

</details>

---

## 🔐 Security Architecture

### Authentication Flow
```
User ──► Phone/Email ──► OTP ──► JWT Token ──► Session
         │                        │
         └── Voice Auth ──────────┘
```

### Data Protection
- 🔒 **Encryption**: AES-256 at rest, TLS 1.3 in transit
- 🛡️ **Authentication**: JWT with refresh tokens
- 🔑 **API Security**: Rate limiting, CORS, CSP headers
- 🚨 **Monitoring**: Real-time threat detection

---

## 📈 Performance Optimization

### Frontend Optimization
```javascript
// Performance Strategies
{
  "strategies": [
    "Image optimization with next/image",
    "Code splitting per route",
    "Lazy loading components",
    "Service worker caching",
    "Edge caching with Vercel"
  ],
  "metrics": {
    "FCP": "1.2s",
    "LCP": "2.1s",
    "TTI": "2.5s",
    "CLS": "0.05"
  }
}
```

### Database Optimization
- **Indexes**: On all foreign keys and search fields
- **Caching**: Redis for session and product data
- **Connection Pooling**: PgBouncer for scalability
- **Query Optimization**: Prepared statements, batch operations

---

## 🚀 Deployment Architecture

### Infrastructure
```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   Vercel Edge   │────►│   Cloudflare    │────►│    Supabase     │
│   Functions      │     │      CDN        │     │   PostgreSQL    │
└─────────────────┘     └─────────────────┘     └─────────────────┘
        │                                                 │
        ▼                                                 ▼
┌─────────────────┐                             ┌─────────────────┐
│   Static Assets │                             │   File Storage   │
│   (Vercel CDN)  │                             │   (Supabase)     │
└─────────────────┘                             └─────────────────┘
```

### CI/CD Pipeline
1. **Source Control**: GitHub with branch protection
2. **Build Process**: Vercel automatic builds
3. **Testing**: Jest, React Testing Library
4. **Deployment**: Automatic to preview/production
5. **Monitoring**: Vercel Analytics, Sentry

---

## 📱 Mobile Architecture

### Progressive Web App
```javascript
// PWA Configuration
{
  "manifest": {
    "name": "KFAR Marketplace",
    "short_name": "KFAR",
    "start_url": "/",
    "display": "standalone",
    "orientation": "portrait",
    "theme_color": "#10B981",
    "background_color": "#FFFFFF"
  },
  "features": [
    "Offline support",
    "Install prompt",
    "Push notifications",
    "Background sync"
  ]
}
```

### Mobile Optimizations
- Touch-optimized UI components
- Gesture support for navigation
- Adaptive layouts for all screens
- Hardware acceleration for animations

---

## 🔄 Integration Points

### Third-Party Services
| Service | Purpose | Integration |
|---------|---------|-------------|
| SendGrid | Email delivery | REST API |
| ElevenLabs | Voice synthesis | WebSocket |
| OpenRouter | AI processing | REST API |
| Cloudinary | Image optimization | SDK |
| Stripe | Payment processing | SDK |

### Webhook Endpoints
```
/webhooks/
├── payment-confirmation
├── order-status-update
├── vendor-notification
└── inventory-sync
```

---

## 📊 Monitoring & Analytics

### Real-Time Monitoring
- **Application**: Vercel Analytics
- **Errors**: Sentry error tracking
- **Performance**: Web Vitals monitoring
- **User Analytics**: Privacy-first analytics

### Key Metrics Tracked
```typescript
interface Metrics {
  performance: {
    pageLoadTime: number;
    apiResponseTime: number;
    voiceRecognitionAccuracy: number;
  };
  business: {
    conversionRate: number;
    averageOrderValue: number;
    voiceOrderPercentage: number;
  };
}
```

---

## 🔧 Development Setup

### Local Environment
```bash
# Clone repository
git clone https://github.com/bakiel/kfar-shop.git

# Install dependencies
npm install

# Environment setup
cp .env.example .env.local

# Database setup
npm run db:migrate

# Start development
npm run dev
```

### Environment Variables
```env
# Core Configuration
NEXT_PUBLIC_APP_URL=http://localhost:3000
NEXT_PUBLIC_SUPABASE_URL=your-supabase-url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key

# AI Services
OPENROUTER_API_KEY=your-api-key
ELEVENLABS_API_KEY=your-api-key

# External Services
SENDGRID_API_KEY=your-api-key
STRIPE_SECRET_KEY=your-api-key
```

---

## 🚦 API Documentation

### REST Endpoints
Full API documentation available at `/api-docs`

### GraphQL Schema
```graphql
type Query {
  products(filter: ProductFilter): [Product!]!
  vendors(location: Location): [Vendor!]!
  user(id: ID!): User
}

type Mutation {
  createOrder(input: OrderInput!): Order!
  updateCart(items: [CartItem!]!): Cart!
}
```

---

## 🎯 Future Architecture Plans

### Scaling Strategy
1. **Microservices**: Separate voice, search, ordering
2. **Multi-region**: Deploy to multiple edge locations
3. **Database Sharding**: Horizontal scaling for growth
4. **AI Model Optimization**: Edge AI for faster response

### Planned Integrations
- WhatsApp Business API
- Google Assistant / Alexa
- AR product visualization
- Blockchain for supply chain

---

<div align="center">
  
  ## 📚 Additional Resources
  
  [API Docs](https://kfarmarket.com/api-docs) | [GitHub](https://github.com/bakiel/kfar-shop) | [Support](mailto:tech@kfarmarket.com)
  
</div>

---

**Last Updated:** December 19, 2024  
**Version:** 1.0  
**Status:** Production Ready
