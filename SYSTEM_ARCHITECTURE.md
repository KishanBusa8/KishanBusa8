# System Architecture Documentation
## Construction Materials Supply Chain Digital Ecosystem

**Version:** 1.0  
**Date:** November 4, 2025  
**Prepared By:** Development Team  
**Technology Stack:** Next.js, NestJS, MongoDB, Flutter

---

## Table of Contents
1. [Executive Summary](#executive-summary)
2. [System Architecture Overview](#system-architecture-overview)
3. [Technology Stack Details](#technology-stack-details)
4. [Database Architecture](#database-architecture)
5. [Security Architecture](#security-architecture)
6. [Module Architecture](#module-architecture)
7. [Role-Based Access Control (RBAC)](#role-based-access-control)
8. [API Architecture](#api-architecture)
9. [Integration Architecture](#integration-architecture)
10. [Deployment Architecture](#deployment-architecture)
11. [Scalability & Performance](#scalability-performance)
12. [Module-wise Features & Responsibilities](#module-wise-features)
13. [Budget Estimation](#budget-estimation)

---

## 1. Executive Summary

This document outlines the comprehensive system architecture for a multi-platform digital ecosystem connecting manufacturers, distributors, dealers, applicators, developers, transporters, and end consumers in the construction materials supply chain. The platform is designed with security, scalability, and user experience as core principles.

**Key Objectives:**
- Enable fast demand fulfillment (within hours)
- Create transparent market operations
- Implement role-based reusable view architecture
- Ensure data security and compliance
- Support real-time inventory and logistics tracking
- Provide analytics and MIS reporting

---

## 2. System Architecture Overview

### 2.1 High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        CLIENT LAYER                             │
├─────────────────────────────────────────────────────────────────┤
│  Flutter Mobile App (iOS/Android)  │  Next.js Web Apps          │
│  - User App                         │  - User Portal            │
│  - Dealer/Distributor App          │  - Admin Dashboard         │
│  - Transporter App                 │  - Analytics Portal        │
└─────────────────────────────────────────────────────────────────┘
                              ↕ HTTPS/WSS
┌─────────────────────────────────────────────────────────────────┐
│                      API GATEWAY LAYER                          │
├─────────────────────────────────────────────────────────────────┤
│  - Rate Limiting                                                │
│  - Authentication/Authorization                                 │
│  - Request Validation                                           │
│  - Load Balancing                                               │
└─────────────────────────────────────────────────────────────────┘
                              ↕ Internal Network
┌─────────────────────────────────────────────────────────────────┐
│                    APPLICATION LAYER (NestJS)                   │
├─────────────────────────────────────────────────────────────────┤
│  Microservices Architecture:                                    │
│  ┌──────────────┬──────────────┬──────────────┬──────────────┐  │
│  │ Auth Service │ User Service │ Order Service│ Product Svc  │  │
│  ├──────────────┼──────────────┼──────────────┼──────────────┤  │
│  │Inventory Svc │Logistics Svc │Payment Svc   │ Chat Service │  │
│  ├──────────────┼──────────────┼──────────────┼──────────────┤  │
│  │Document Svc  │Analytics Svc │Notification  │ Reward Svc   │  │
│  └──────────────┴──────────────┴──────────────┴──────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────────┐
│                      DATA LAYER                                 │
├─────────────────────────────────────────────────────────────────┤
│  MongoDB (Primary)  │  Redis Cache  │  S3/Cloud Storage         │
│  - User Data        │  - Sessions   │  - Documents              │
│  - Transactions     │  - Real-time  │  - Images                 │
│  - Analytics        │  - Inventory  │  - Backups                │
└─────────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────────┐
│                   EXTERNAL INTEGRATIONS                         │
├─────────────────────────────────────────────────────────────────┤
│  Payment Gateways  │ Logistics APIs  │ Maps APIs   │  SMS/Email │
│  - Razorpay        │  - Porter       │ - Google    │  - Twilio  │
│  - Paytm           │  - Delhivery    │ - MapmyIndia│ - SendGrid │
└─────────────────────────────────────────────────────────────────┘
```

### 2.2 Architecture Principles

1. **Microservices Architecture**: Independent, scalable services
2. **API-First Design**: RESTful APIs with GraphQL for complex queries
3. **Event-Driven Communication**: Message queues for async operations
4. **Separation of Concerns**: Clear boundaries between layers
5. **Scalability**: Horizontal scaling capability
6. **Security by Design**: Security at every layer

---

## 3. Technology Stack Details

### 3.1 Frontend Technologies

#### Next.js (Web Applications)
- **Version**: 14.x (App Router)
- **State Management**: Zustand/Redux Toolkit
- **UI Library**: Tailwind CSS + shadcn/ui
- **Form Handling**: React Hook Form + Zod
- **API Communication**: Axios with interceptors
- **Real-time**: Socket.io-client
- **Charts**: Recharts/Chart.js
- **Maps**: Google Maps API/Mapbox

**Applications:**
1. **User Portal** (users.domain.com)
2. **Admin Dashboard** (admin.domain.com)
3. **Analytics Portal** (analytics.domain.com)

#### Flutter (Mobile Applications)
- **Version**: 3.x
- **State Management**: Riverpod/Bloc
- **Local Storage**: Hive/SQLite
- **API Communication**: Dio
- **Real-time**: Socket.io-client-dart
- **Maps**: Google Maps Flutter
- **Push Notifications**: Firebase Cloud Messaging

### 3.2 Backend Technologies

#### NestJS (API Server)
- **Version**: 10.x
- **Architecture**: Modular Microservices
- **Authentication**: JWT + Refresh Tokens
- **Authorization**: RBAC with CASL
- **Validation**: Class-validator + Class-transformer
- **Documentation**: Swagger/OpenAPI
- **Real-time**: Socket.io/WebSockets
- **Job Queue**: Bull (Redis-based)
- **Logging**: Winston + Morgan
- **Testing**: Jest + Supertest

**Key Modules:**
- Authentication & Authorization
- User Management
- Product & Inventory
- Order Management
- Logistics & Transport
- Payment Processing
- Document Management
- Chat & Communication
- Analytics & Reporting
- Notification Service

### 3.3 Database & Storage

#### MongoDB (Primary Database)
- **Version**: 7.x
- **Deployment**: MongoDB Atlas (Cloud)
- **Replica Set**: 3-node configuration
- **Backup**: Automated daily backups
- **Indexes**: Strategic indexing for performance

#### Redis (Caching & Sessions)
- **Version**: 7.x
- **Use Cases**:
  - Session management
  - Real-time inventory cache
  - Rate limiting
  - Job queue
  - Pub/Sub messaging

#### Cloud Storage (AWS S3/Google Cloud Storage)
- Document storage (invoices, challans, MTCs)
- Product images
- User profile pictures
- Backup archives

### 3.4 DevOps & Infrastructure

- **Containerization**: Docker
- **Orchestration**: Kubernetes/Docker Swarm
- **CI/CD**: GitHub Actions/GitLab CI
- **Monitoring**: Prometheus + Grafana
- **Logging**: ELK Stack (Elasticsearch, Logstash, Kibana)
- **CDN**: Cloudflare
- **SSL**: Let's Encrypt/Cloudflare SSL

---

## 4. Database Architecture

### 4.1 Database Design Principles

1. **Schema Design**: Flexible document-based schema
2. **Denormalization**: Strategic denormalization for read performance
3. **Referencing**: Use references for large, frequently updated data
4. **Embedding**: Embed small, rarely changing data
5. **Indexing**: Compound indexes for query optimization

### 4.2 Core Collections

#### Users Collection
```javascript
{
  _id: ObjectId,
  email: String (unique, indexed),
  phone: String (unique, indexed),
  password: String (hashed),
  userType: Enum ['manufacturer', 'distributor', 'dealer', 'applicator', 'developer', 'transporter', 'consumer'],
  profile: {
    firstName: String,
    lastName: String,
    avatar: String,
    businessName: String,
    gst: String,
    pan: String
  },
  roleId: ObjectId (ref: 'Roles'),
  organizationId: ObjectId (ref: 'Organizations'),
  isVerified: Boolean,
  isActive: Boolean,
  kycStatus: Enum ['pending', 'approved', 'rejected'],
  kycDocuments: [{
    type: String,
    url: String,
    verifiedAt: Date
  }],
  addresses: [{
    type: Enum ['billing', 'shipping', 'warehouse'],
    line1: String,
    line2: String,
    city: String,
    state: String,
    pincode: String,
    coordinates: {
      lat: Number,
      lng: Number
    },
    isDefault: Boolean
  }],
  preferences: {
    language: String,
    currency: String,
    notifications: Object
  },
  createdAt: Date,
  updatedAt: Date,
  lastLoginAt: Date
}
```

#### Organizations Collection
```javascript
{
  _id: ObjectId,
  name: String,
  type: Enum ['manufacturer', 'distributor', 'dealer', 'contractor', 'developer'],
  registrationNumber: String,
  gst: String,
  pan: String,
  adminUserId: ObjectId (ref: 'Users'),
  members: [{
    userId: ObjectId (ref: 'Users'),
    roleId: ObjectId (ref: 'Roles'),
    joinedAt: Date
  }],
  verificationStatus: String,
  businessDocuments: Array,
  settings: Object,
  isActive: Boolean,
  createdAt: Date,
  updatedAt: Date
}
```

#### Roles Collection
```javascript
{
  _id: ObjectId,
  name: String,
  organizationType: String,
  permissions: [{
    module: String,
    actions: [String] // ['create', 'read', 'update', 'delete']
  }],
  description: String,
  isSystem: Boolean,
  createdAt: Date,
  updatedAt: Date
}
```

#### Products Collection
```javascript
{
  _id: ObjectId,
  name: String (indexed),
  sku: String (unique, indexed),
  manufacturerId: ObjectId (ref: 'Organizations'), // Original manufacturer
  createdBy: ObjectId (ref: 'Organizations'), // Who added to catalog (can be distributor/dealer)
  creatorType: Enum ['manufacturer', 'distributor', 'dealer'],
  category: String (indexed),
  subCategory: String,
  description: String,
  specifications: Object,
  images: [String],
  videos: [String],
  documents: [{
    type: String, // 'msds', 'tds', 'coa'
    url: String
  }],
  basePrice: Number, // Manufacturer's base price
  mrp: Number, // Maximum Retail Price
  unit: String,
  hsn: String,
  gst: Number,
  rewardPoints: Number,
  qrCode: String,
  rewardAppLink: String,
  isActive: Boolean,
  approvalStatus: Enum ['pending', 'approved', 'rejected'],
  approvedBy: ObjectId (ref: 'Users'),
  tags: [String] (indexed),
  searchKeywords: [String] (text indexed),
  createdAt: Date,
  updatedAt: Date
}
```

#### Pricing Tiers Collection (NEW)
```javascript
{
  _id: ObjectId,
  organizationId: ObjectId (ref: 'Organizations'),
  organizationType: Enum ['distributor', 'dealer'],
  tierLevel: Enum ['bronze', 'silver', 'gold', 'platinum'],
  tierCriteria: {
    volumeType: Enum ['weekly', 'monthly', 'quarterly'],
    minVolume: Number, // in MT or units
    minOrderCount: Number,
    minOrderValue: Number
  },
  discountPercentage: Number,
  specialRates: [{
    productId: ObjectId (ref: 'Products'),
    discountPercentage: Number,
    fixedPrice: Number
  }],
  benefits: [String],
  validFrom: Date,
  validUntil: Date,
  isActive: Boolean,
  createdAt: Date,
  updatedAt: Date
}
```

#### Product Pricing Collection (NEW)
```javascript
{
  _id: ObjectId,
  productId: ObjectId (ref: 'Products'),
  locationId: ObjectId (ref: 'Organizations'), // Distributor or Dealer
  locationType: Enum ['manufacturer', 'distributor', 'dealer'],
  basePrice: Number,
  sellingPrice: Number,
  tierPricing: [{
    tierLevel: String,
    price: Number,
    discountPercentage: Number
  }],
  bulkPricing: [{
    minQuantity: Number,
    maxQuantity: Number,
    price: Number
  }],
  marginPercentage: Number,
  lastUpdated: Date,
  updatedBy: ObjectId (ref: 'Users')
}
```

#### Nearby Stock View Collection (NEW)
```javascript
{
  _id: ObjectId,
  productId: ObjectId (ref: 'Products'),
  userLocation: {
    coordinates: {
      lat: Number,
      lng: Number
    },
    address: String
  },
  nearbyStockPoints: [{
    locationId: ObjectId (ref: 'Organizations'),
    organizationName: String,
    locationType: Enum ['distributor', 'dealer'],
    distance: Number, // in kilometers
    availableQuantity: Number,
    sellingPrice: Number,
    applicableTierPrice: Number, // For logged-in users with tier
    estimatedDeliveryTime: Number, // in hours
    address: Object,
    coordinates: Object,
    isVerified: Boolean
  }],
  searchedAt: Date,
  expiresAt: Date // Cache expiry
}
```

#### Inventory Collection
```javascript
{
  _id: ObjectId,
  productId: ObjectId (ref: 'Products'),
  locationId: ObjectId (ref: 'Organizations'),
  locationType: Enum ['manufacturer', 'distributor', 'dealer'],
  quantity: Number,
  reservedQuantity: Number,
  availableQuantity: Number (virtual),
  minStockLevel: Number,
  maxStockLevel: Number,
  reorderPoint: Number,
  batchDetails: [{
    batchNo: String,
    quantity: Number,
    mfgDate: Date,
    expiryDate: Date
  }],
  lastRestockedAt: Date,
  updatedAt: Date
}
```

#### Orders Collection
```javascript
{
  _id: ObjectId,
  orderNumber: String (unique, indexed),
  orderType: Enum ['purchase', 'sale'],
  customerId: ObjectId (ref: 'Users'),
  customerOrgId: ObjectId (ref: 'Organizations'),
  sellerId: ObjectId (ref: 'Organizations'),
  items: [{
    productId: ObjectId (ref: 'Products'),
    productName: String,
    sku: String,
    quantity: Number,
    unit: String,
    rate: Number,
    discount: Number,
    tax: Number,
    amount: Number
  }],
  deliveryAddress: Object,
  deliveryType: Enum ['site', 'shop', 'warehouse'],
  status: Enum ['pending', 'confirmed', 'processing', 'dispatched', 'delivered', 'cancelled'],
  statusHistory: [{
    status: String,
    updatedBy: ObjectId,
    timestamp: Date,
    notes: String
  }],
  pricing: {
    subtotal: Number,
    discount: Number,
    tax: Number,
    shippingCharges: Number,
    total: Number
  },
  paymentStatus: Enum ['pending', 'partial', 'paid', 'refunded'],
  paymentDetails: Object,
  transportDetails: {
    transporterId: ObjectId,
    vehicleNumber: String,
    driverName: String,
    driverPhone: String,
    trackingUrl: String
  },
  documents: [{
    type: Enum ['invoice', 'challan', 'mtc', 'pod'],
    url: String,
    uploadedAt: Date
  }],
  expectedDeliveryDate: Date,
  actualDeliveryDate: Date,
  notes: String,
  createdAt: Date,
  updatedAt: Date
}
```

#### Logistics Collection
```javascript
{
  _id: ObjectId,
  orderId: ObjectId (ref: 'Orders'),
  provider: Enum ['internal', 'porter', 'delhivery', 'other'],
  trackingId: String,
  vehicleType: String,
  vehicleNumber: String,
  driverDetails: {
    name: String,
    phone: String,
    userId: ObjectId (ref: 'Users')
  },
  route: [{
    location: String,
    coordinates: Object,
    timestamp: Date,
    status: String
  }],
  pickupLocation: Object,
  deliveryLocation: Object,
  status: Enum ['assigned', 'picked', 'in_transit', 'delivered', 'failed'],
  estimatedDelivery: Date,
  actualDelivery: Date,
  proof: {
    signature: String,
    photo: String,
    receivedBy: String
  },
  createdAt: Date,
  updatedAt: Date
}
```

#### Payments Collection
```javascript
{
  _id: ObjectId,
  orderId: ObjectId (ref: 'Orders'),
  customerId: ObjectId (ref: 'Users'),
  amount: Number,
  paymentMethod: Enum ['online', 'credit', 'wallet', 'cash', 'cheque'],
  gateway: String,
  transactionId: String,
  status: Enum ['pending', 'success', 'failed', 'refunded'],
  metadata: Object,
  createdAt: Date,
  updatedAt: Date
}
```

#### Credit Ledger Collection
```javascript
{
  _id: ObjectId,
  customerId: ObjectId (ref: 'Users'),
  sellerId: ObjectId (ref: 'Organizations'),
  creditLimit: Number,
  availableCredit: Number,
  usedCredit: Number,
  transactions: [{
    orderId: ObjectId,
    type: Enum ['debit', 'credit'],
    amount: Number,
    balance: Number,
    timestamp: Date
  }],
  updatedAt: Date
}
```

#### Messages Collection
```javascript
{
  _id: ObjectId,
  conversationId: ObjectId (ref: 'Conversations'),
  senderId: ObjectId (ref: 'Users'),
  receiverId: ObjectId (ref: 'Users'),
  messageType: Enum ['text', 'image', 'document', 'location'],
  content: String,
  attachments: [String],
  isRead: Boolean,
  readAt: Date,
  createdAt: Date
}
```

#### Conversations Collection
```javascript
{
  _id: ObjectId,
  participants: [ObjectId] (ref: 'Users'),
  type: Enum ['direct', 'group', 'order'],
  metadata: {
    orderId: ObjectId,
    title: String
  },
  lastMessage: String,
  lastMessageAt: Date,
  unreadCount: Map, // userId -> count
  createdAt: Date,
  updatedAt: Date
}
```

#### Notifications Collection
```javascript
{
  _id: ObjectId,
  userId: ObjectId (ref: 'Users'),
  type: Enum ['order', 'payment', 'delivery', 'stock', 'system'],
  title: String,
  message: String,
  data: Object,
  isRead: Boolean,
  readAt: Date,
  channel: Enum ['push', 'sms', 'email'],
  status: String,
  createdAt: Date
}
```

#### Analytics Collection
```javascript
{
  _id: ObjectId,
  entityType: String,
  entityId: ObjectId,
  metricType: String,
  period: String, // 'daily', 'weekly', 'monthly'
  date: Date (indexed),
  metrics: Object,
  createdAt: Date
}
```

### 4.3 Indexing Strategy

```javascript
// Users Collection
db.users.createIndex({ email: 1 }, { unique: true })
db.users.createIndex({ phone: 1 }, { unique: true })
db.users.createIndex({ userType: 1, isActive: 1 })
db.users.createIndex({ organizationId: 1 })

// Products Collection
db.products.createIndex({ sku: 1 }, { unique: true })
db.products.createIndex({ name: "text", searchKeywords: "text" })
db.products.createIndex({ category: 1, isActive: 1 })
db.products.createIndex({ manufacturerId: 1 })

// Inventory Collection
db.inventory.createIndex({ productId: 1, locationId: 1 }, { unique: true })
db.inventory.createIndex({ availableQuantity: 1 })
db.inventory.createIndex({ locationId: 1 })

// Orders Collection
db.orders.createIndex({ orderNumber: 1 }, { unique: true })
db.orders.createIndex({ customerId: 1, createdAt: -1 })
db.orders.createIndex({ sellerId: 1, status: 1 })
db.orders.createIndex({ status: 1, createdAt: -1 })

// Messages Collection
db.messages.createIndex({ conversationId: 1, createdAt: -1 })
db.messages.createIndex({ senderId: 1, isRead: 1 })

// Indexes for pricing collections
db.pricing_tiers.createIndex({ organizationId: 1, isActive: 1 })
db.pricing_tiers.createIndex({ tierLevel: 1 })
db.product_pricing.createIndex({ productId: 1, locationId: 1 }, { unique: true })
db.product_pricing.createIndex({ locationId: 1, lastUpdated: -1 })
db.nearby_stock_view.createIndex({ 
  "userLocation.coordinates": "2dsphere",
  expiresAt: 1 
})
```

---

## 5. Security Architecture

### 5.1 Authentication & Authorization

#### Multi-Layer Authentication
```
┌─────────────────────────────────────────────────────────────┐
│                   AUTHENTICATION FLOW                       │
└─────────────────────────────────────────────────────────────┘

1. User Registration
   ├─ Email/Phone verification (OTP)
   ├─ Password hashing (bcrypt, salt rounds: 12)
   ├─ KYC document upload
   └─ Admin approval (for business users)

2. Login Process
   ├─ Email/Phone + Password
   ├─ Multi-factor Authentication (OTP)
   ├─ JWT Access Token (15 min expiry)
   ├─ Refresh Token (7 days, stored in httpOnly cookie)
   └─ Device fingerprinting

3. Token Management
   ├─ Access Token: Short-lived, stored in memory
   ├─ Refresh Token: Long-lived, httpOnly cookie
   ├─ Token rotation on refresh
   └─ Blacklist for revoked tokens (Redis)

4. Session Management
   ├─ Redis-based session store
   ├─ Concurrent session limit
   ├─ Automatic logout on suspicious activity
   └─ Session activity tracking
```

#### Role-Based Access Control (RBAC)

**Permission Structure:**
```javascript
{
  module: 'orders',
  resource: 'order',
  actions: {
    create: ['manufacturer', 'distributor', 'dealer', 'consumer'],
    read: ['all'],
    update: ['manufacturer', 'distributor', 'dealer', 'order_owner'],
    delete: ['admin', 'order_owner'],
    approve: ['distributor', 'dealer'],
    cancel: ['order_owner', 'admin']
  },
  conditions: {
    read: { organizationId: '$user.organizationId' },
    update: { status: { $ne: 'delivered' } }
  }
}
```

### 5.2 Data Security

#### Encryption
- **In Transit**: TLS 1.3 for all communications
- **At Rest**: AES-256 encryption for sensitive data
- **Database**: MongoDB encryption at rest
- **Passwords**: bcrypt with salt (rounds: 12)
- **Sensitive Fields**: Field-level encryption (PAN, Bank details)

#### Data Privacy
- **PII Protection**: Separate collection for sensitive data
- **Data Masking**: Mask sensitive data in logs
- **GDPR Compliance**: Right to deletion, data export
- **Audit Logs**: Track all data access and modifications

### 5.3 API Security

```javascript
// API Gateway Security Layers

1. Rate Limiting
   ├─ Per IP: 100 requests/minute
   ├─ Per User: 1000 requests/hour
   ├─ Per Endpoint: Custom limits
   └─ DDoS protection (Cloudflare)

2. Input Validation
   ├─ Request schema validation (Joi/Zod)
   ├─ SQL/NoSQL injection prevention
   ├─ XSS protection
   ├─ CSRF tokens for state-changing operations
   └─ File upload validation (size, type, virus scan)

3. Authentication
   ├─ JWT verification
   ├─ Token expiry check
   ├─ Signature validation
   └─ Revocation list check

4. Authorization
   ├─ Role-based access check
   ├─ Resource ownership verification
   ├─ Organization boundary check
   └─ Feature flag verification

5. Response Security
   ├─ Sensitive data filtering
   ├─ Error message sanitization
   ├─ CORS configuration
   └─ Security headers (Helmet.js)
```

### 5.4 Payment Security

- **PCI DSS Compliance**: No card data storage
- **Payment Gateway**: Tokenization via Razorpay/Paytm
- **3D Secure**: Mandatory for online payments
- **Webhook Verification**: Signature validation
- **Transaction Logs**: Immutable audit trail
- **Fraud Detection**: Unusual pattern detection

### 5.5 Security Monitoring

```javascript
// Security Monitoring & Alerting

1. Real-time Monitoring
   ├─ Failed login attempts (> 5 in 10 min)
   ├─ Unusual access patterns
   ├─ API abuse detection
   ├─ Privilege escalation attempts
   └─ Data exfiltration patterns

2. Logging & Auditing
   ├─ All authentication events
   ├─ Authorization failures
   ├─ Data modifications
   ├─ Admin actions
   └─ Payment transactions

3. Incident Response
   ├─ Automatic account lockout
   ├─ Alert to security team
   ├─ Session termination
   └─ IP blocking
```

### 5.6 Compliance & Standards

- **Data Protection**: GDPR, Indian Data Protection Law
- **Payment Security**: PCI DSS
- **API Security**: OWASP Top 10
- **Code Security**: SAST, DAST scans
- **Penetration Testing**: Quarterly third-party audits
- **Vulnerability Management**: Regular dependency updates

---

## 6. Module Architecture

### 6.0 Product Creation & Ownership Flow

**Understanding the Product Flow:**

You're absolutely correct! Here's how the product creation and ownership works:

#### **Product Creation Rights:**

1. **Manufacturers** 
   - Create original products with base specifications
   - Set MRP (Maximum Retail Price)
   - Define base pricing
   - Upload technical documents (MSDS, TDS, COA)
   - Generate QR codes for reward programs

2. **Distributors**
   - Can add manufacturer's products to their catalog
   - Set their own selling prices (within margin limits)
   - Manage their inventory for these products
   - Cannot modify product specifications
   - Can add location-specific information

3. **Dealers**
   - Can add products from manufacturers OR distributors
   - Set their own retail prices
   - Manage their shop inventory
   - Cannot modify product specifications
   - Can add local delivery information

#### **Product Visibility Flow:**

```
MANUFACTURER creates Product
        ↓
[Product Base Data Created]
        ↓
DISTRIBUTOR adds to catalog → Sets selling price
        ↓
[Product available at distributor level]
        ↓
DEALER adds to catalog → Sets retail price
        ↓
[Product available at dealer level]
        ↓
APPLICATOR/DEVELOPER/CONSUMER can VIEW & ORDER
```

#### **Why This Flow Makes Sense:**

1. **Inventory Ownership**: Only those who physically stock products (Manufacturer, Distributor, Dealer) can list them, ensuring accurate availability.

2. **Price Control**: Each level in the supply chain sets their own price while maintaining margins, creating a realistic market structure.

3. **Quality Assurance**: Manufacturers maintain product specifications, preventing incorrect information from entering the system.

4. **Stock Accuracy**: Since only inventory holders list products, stock levels are always accurate and traceable.

5. **Order Routing**: When a consumer orders, the system knows exactly where the stock is and can fulfill from the nearest location.

#### **Example Scenario:**

```
Dr. Fixit Waterproofing (Manufacturer)
  ├─ Base Price: ₹450/unit
  ├─ MRP: ₹600/unit
  └─ Creates product with specs

    ↓ [Added by]

Surat Distributors Pvt Ltd (Distributor)
  ├─ Buying Price: ₹450/unit
  ├─ Selling Price: ₹520/unit (15% margin)
  ├─ Stock: 500 units in Surat warehouse
  └─ Delivery: Can deliver in Surat city

    ↓ [Added by]

Patel Building Materials (Dealer)
  ├─ Buying Price: ₹520/unit (from distributor)
  ├─ Retail Price: ₹580/unit (11.5% margin)
  ├─ Stock: 50 units in shop
  └─ Delivery: Can deliver in Katargam area

    ↓ [Visible to]

Applicator/Consumer searches "Dr. Fixit Waterproofing"
  ├─ Sees 2 options:
  │   1. Surat Distributors - ₹520 (10km away, 3-4 hrs delivery)
  │   2. Patel Materials - ₹580 (2km away, 1-2 hrs delivery)
  └─ Chooses based on price vs. delivery time preference
```

### 6.0A Smart Purchase & Pricing Control System

This is a critical feature that differentiates your platform and ensures its success.

#### **Objective:**
Make purchases through the app MORE BENEFICIAL than offline transactions while maintaining fair margins for all stakeholders.

#### **How It Works:**

**1. Tier-Based Pricing for Volume Buyers**

```javascript
// Pricing Tier Structure
const pricingTiers = {
  bronze: {
    criteria: {
      weeklyVolume: { min: 0, max: 10 }, // MT
      monthlyVolume: { min: 0, max: 40 }
    },
    discount: 0, // No discount
    benefits: ['Standard pricing', 'Regular delivery']
  },
  silver: {
    criteria: {
      weeklyVolume: { min: 10, max: 20 },
      monthlyVolume: { min: 40, max: 80 }
    },
    discount: 3, // 3% off
    benefits: ['Priority delivery', 'Extended credit']
  },
  gold: {
    criteria: {
      weeklyVolume: { min: 20, max: 30 },
      monthlyVolume: { min: 80, max: 120 }
    },
    discount: 5, // 5% off
    benefits: ['Free delivery', 'Dedicated support', '15-day credit']
  },
  platinum: {
    criteria: {
      weeklyVolume: { min: 30, max: Infinity },
      monthlyVolume: { min: 120, max: Infinity }
    },
    discount: 8, // 8% off
    benefits: ['Free delivery', '24/7 support', '30-day credit', 'Exclusive rates']
  }
};

// Automatic Tier Calculation
async calculateTier(userId: string, timeframe: 'weekly' | 'monthly') {
  const orders = await this.getOrderHistory(userId, timeframe);
  const totalVolume = orders.reduce((sum, order) => sum + order.volume, 0);
  
  // Find applicable tier
  for (const [tierName, tier] of Object.entries(pricingTiers)) {
    if (totalVolume >= tier.criteria[`${timeframe}Volume`].min &&
        totalVolume < tier.criteria[`${timeframe}Volume`].max) {
      return tierName;
    }
  }
}
```

**2. Dynamic Price Calculation**

```javascript
// Price Calculation Engine
async calculateBestPrice(productId, quantity, userId, locationId) {
  // 1. Get base pricing
  const product = await getProduct(productId);
  const basePrice = product.basePrice;
  
  // 2. Get location-specific pricing
  const locationPricing = await getLocationPricing(productId, locationId);
  const sellingPrice = locationPricing.sellingPrice;
  
  // 3. Get user's tier
  const userTier = await calculateTier(userId, 'monthly');
  const tierDiscount = pricingTiers[userTier].discount;
  
  // 4. Check bulk discounts
  const bulkDiscount = await getBulkDiscount(productId, quantity);
  
  // 5. Check promotional offers
  const promoDiscount = await getActivePromotions(productId, userId);
  
  // 6. Calculate final price
  let finalPrice = sellingPrice;
  finalPrice -= (finalPrice * tierDiscount / 100);
  finalPrice -= (finalPrice * bulkDiscount / 100);
  finalPrice -= (finalPrice * promoDiscount / 100);
  
  // 7. Ensure minimum margin
  const minimumPrice = basePrice * 1.05; // 5% minimum margin
  finalPrice = Math.max(finalPrice, minimumPrice);
  
  return {
    basePrice,
    sellingPrice,
    tierDiscount,
    bulkDiscount,
    promoDiscount,
    finalPrice,
    savings: sellingPrice - finalPrice,
    savingsPercentage: ((sellingPrice - finalPrice) / sellingPrice * 100).toFixed(2)
  };
}
```

**3. Nearest Stock Point Finder with Price Comparison**

```javascript
// Find Best Purchase Option
async findBestPurchaseOption(productId, userLocation, quantity) {
  // 1. Find all stock points within radius
  const nearbyStockPoints = await this.findNearbyStock(
    productId,
    userLocation,
    radius: 50 // km
  );
  
  // 2. Calculate price for each location
  const options = await Promise.all(
    nearbyStockPoints.map(async (stockPoint) => {
      const pricing = await this.calculateBestPrice(
        productId,
        quantity,
        userId,
        stockPoint.locationId
      );
      
      const deliveryTime = await this.estimateDeliveryTime(
        stockPoint.location,
        userLocation
      );
      
      const deliveryCharge = await this.calculateDeliveryCharge(
        stockPoint.location,
        userLocation,
        quantity
      );
      
      return {
        ...stockPoint,
        pricing,
        deliveryTime,
        deliveryCharge,
        totalCost: pricing.finalPrice * quantity + deliveryCharge,
        // Score based on price and delivery time
        score: this.calculateScore(pricing.finalPrice, deliveryTime)
      };
    })
  );
  
  // 3. Sort by best value (score)
  options.sort((a, b) => b.score - a.score);
  
  return {
    recommended: options[0], // Best option
    alternatives: options.slice(1, 4), // Next 3 options
    comparison: {
      cheapest: options.sort((a, b) => a.totalCost - b.totalCost)[0],
      fastest: options.sort((a, b) => a.deliveryTime - b.deliveryTime)[0]
    }
  };
}
```

**4. Price Comparison UI Example**

```javascript
// What the user sees
{
  product: "Dr. Fixit Waterproofing 20kg",
  quantity: 100,
  userTier: "Gold",
  
  recommendations: {
    bestValue: {
      seller: "Surat Distributors Pvt Ltd",
      distance: "8 km",
      basePrice: "₹520/unit",
      yourPrice: "₹494/unit", // After 5% gold tier discount
      totalCost: "₹49,400",
      deliveryTime: "3-4 hours",
      deliveryCharge: "Free",
      savings: "₹2,600 (5% off)",
      whyBest: "Best combination of price and delivery time"
    },
    
    cheapest: {
      seller: "Gujarat Construction Supplies",
      distance: "25 km",
      basePrice: "₹510/unit",
      yourPrice: "₹484/unit",
      totalCost: "₹48,900", // ₹500 cheaper but slower
      deliveryTime: "6-8 hours",
      deliveryCharge: "₹500",
      savings: "₹3,100 (6% off)"
    },
    
    fastest: {
      seller: "Patel Building Materials",
      distance: "2 km",
      basePrice: "₹580/unit",
      yourPrice: "₹551/unit",
      totalCost: "₹55,100",
      deliveryTime: "1-2 hours",
      deliveryCharge: "Free",
      savings: "₹2,900 (5% off)"
    }
  },
  
  offlineComparison: {
    estimatedOfflinePrice: "₹600/unit",
    platformAdvantage: "₹10,600 cheaper (17.6% savings)",
    message: "You're saving ₹106 per unit by ordering through the app!"
  }
}
```

**5. Tier Progression Incentive**

```javascript
// Show users their progress towards next tier
async getTierProgress(userId) {
  const currentTier = await this.getUserTier(userId);
  const nextTier = this.getNextTier(currentTier);
  
  const monthlyVolume = await this.getMonthlyVolume(userId);
  const requiredForNextTier = nextTier.criteria.monthlyVolume.min;
  const remaining = requiredForNextTier - monthlyVolume;
  
  return {
    currentTier: {
      name: "Gold",
      discount: "5%",
      benefits: pricingTiers.gold.benefits
    },
    nextTier: {
      name: "Platinum",
      discount: "8%",
      benefits: pricingTiers.platinum.benefits
    },
    progress: {
      current: monthlyVolume, // 95 MT
      required: requiredForNextTier, // 120 MT
      remaining: remaining, // 25 MT
      percentage: (monthlyVolume / requiredForNextTier * 100).toFixed(1), // 79.2%
      message: "Order 25 MT more this month to unlock Platinum tier!"
    },
    potentialSavings: {
      currentMonthSavings: "₹45,000",
      platinumMonthSavings: "₹72,000",
      additionalSavings: "₹27,000 extra per month"
    }
  };
}
```

#### **Benefits of This System:**

**For Buyers (Applicators/Developers/Consumers):**
- ✓ Always get better prices than offline
- ✓ Transparent pricing across locations
- ✓ Volume discounts encourage bulk buying
- ✓ Loyalty rewards for consistent business
- ✓ Time saved in price comparison

**For Sellers (Distributors/Dealers):**
- ✓ Automated pricing eliminates negotiation
- ✓ Tier system encourages customer loyalty
- ✓ Platform guarantees volume business
- ✓ Competitive advantage over offline market
- ✓ Data-driven pricing insights

**For the Platform:**
- ✓ Ensures users prefer app over offline
- ✓ Increases transaction volume
- ✓ Creates network effects
- ✓ Sustainable business model
- ✓ Clear differentiation from competitors

#### **Implementation Notes:**

1. **Margin Protection**: System ensures sellers always maintain minimum margins (e.g., 5%)
2. **Dynamic Adjustments**: Pricing can be adjusted based on demand, competition, and inventory levels
3. **Promotional Pricing**: Special offers can be layered on top of tier pricing
4. **Credit Integration**: Higher tiers get better credit terms
5. **Analytics Dashboard**: All stakeholders can see how pricing impacts their business

---

## 6.1 Reusable View Architecture

**Concept**: Single codebase with role-based view rendering

```javascript
// Component Structure
<Layout userRole={currentUser.role}>
  <Dashboard>
    <MetricsWidget 
      metrics={getRoleSpecificMetrics(role)}
      permissions={getUserPermissions(role)}
    />
    <OrdersWidget 
      viewType={getOrderViewType(role)}
      actions={getAvailableActions(role)}
    />
    <InventoryWidget 
      visible={hasPermission(role, 'inventory.view')}
      editMode={hasPermission(role, 'inventory.edit')}
    />
  </Dashboard>
</Layout>

// Role Configuration
const ROLE_CONFIGS = {
  manufacturer: {
    dashboard: ['production_metrics', 'inventory', 'orders', 'distributors'],
    orderView: 'seller',
    canCreate: ['products', 'inventory'],
    canApprove: ['distributor_requests']
  },
  distributor: {
    dashboard: ['sales_metrics', 'inventory', 'orders', 'dealers'],
    orderView: 'both',
    canCreate: ['orders', 'dealer_accounts'],
    canApprove: ['dealer_orders']
  },
  dealer: {
    dashboard: ['sales_metrics', 'inventory', 'orders', 'customers'],
    orderView: 'both',
    canCreate: ['orders', 'customer_accounts'],
    canApprove: ['customer_orders']
  },
  applicator: {
    dashboard: ['project_metrics', 'orders', 'rewards'],
    orderView: 'buyer',
    canCreate: ['orders', 'service_requests'],
    canApprove: []
  },
  consumer: {
    dashboard: ['order_history', 'wishlist', 'rewards'],
    orderView: 'buyer',
    canCreate: ['orders'],
    canApprove: []
  }
}
```

### 6.2 Core Modules

#### 1. Authentication & Authorization Module

**Features:**
- User registration with email/phone
- OTP-based verification
- Multi-factor authentication
- Role-based access control
- Session management
- Password reset
- Device management

**Endpoints:**
```
POST   /auth/register
POST   /auth/verify-otp
POST   /auth/login
POST   /auth/logout
POST   /auth/refresh-token
POST   /auth/forgot-password
POST   /auth/reset-password
GET    /auth/me
```

#### 2. User Management Module

**Features:**
- User profile management
- KYC document upload and verification
- Organization management
- Sub-user management
- Role assignment
- Address management
- Preference settings

**Endpoints:**
```
GET    /users/profile
PUT    /users/profile
POST   /users/kyc
GET    /users/organization
POST   /users/organization/members
PUT    /users/organization/members/:id/role
GET    /users/addresses
POST   /users/addresses
```

#### 3. Product Catalog Module

**Features:**
- Product listing with filters
- Category-wise browsing
- Search with autocomplete
- Product details
- Image gallery
- Document downloads (MSDS, TDS)
- QR code scanning for rewards
- Product comparison
- **Smart Price Comparison** (NEW)
- **Nearest Stock Finder** (NEW)
- **Tier-based Pricing Display** (NEW)
- **Volume-based Discounts** (NEW)

**Endpoints:**
```
GET    /products
GET    /products/:id
GET    /products/search?q=
GET    /products/categories
GET    /products/:id/documents
POST   /products/:id/scan-qr
GET    /products/compare?ids=
POST   /products                          // For Manufacturer, Distributor, Dealer
PUT    /products/:id                      // For Manufacturer, Distributor, Dealer
GET    /products/:id/nearby-stock         // NEW: Find nearest available stock
GET    /products/:id/pricing-tiers        // NEW: Get tier-based pricing
GET    /products/:id/compare-prices       // NEW: Compare prices across locations
POST   /products/:id/calculate-best-price // NEW: Calculate best price for user
```

#### 3A. Pricing Management Module (NEW)

**Features:**
- Tier-based pricing configuration
- Volume-based discount rules
- Dynamic pricing engine
- Price comparison across locations
- Automatic tier calculation
- Promotional pricing
- Margin control

**Endpoints:**
```
GET    /pricing/tiers                     // Get all pricing tiers
POST   /pricing/tiers                     // Create pricing tier
PUT    /pricing/tiers/:id                 // Update pricing tier
GET    /pricing/tiers/calculate           // Calculate user's tier
GET    /pricing/products/:productId       // Get product pricing
PUT    /pricing/products/:productId       // Update product pricing
GET    /pricing/compare                   // Compare prices across sellers
POST   /pricing/bulk-update               // Bulk pricing update
GET    /pricing/analytics                 // Pricing analytics
```

#### 4. Inventory Management Module

**Features:**
- Real-time stock levels
- Multi-location inventory
- Stock alerts (low stock, reorder)
- Batch tracking
- Stock transfer
- Stock adjustment
- Inventory reports

**Endpoints:**
```
GET    /inventory/locations
GET    /inventory/products/:productId
PUT    /inventory/products/:productId/adjust
POST   /inventory/transfer
GET    /inventory/alerts
GET    /inventory/reports
```

#### 5. Order Management Module

**Features:**
- Order creation and placement
- Multi-level approval workflow
- Order tracking
- Status updates
- Order modifications
- Cancellation and returns
- Bulk ordering
- Repeat orders

**Endpoints:**
```
POST   /orders
GET    /orders
GET    /orders/:id
PUT    /orders/:id/status
PUT    /orders/:id/approve
PUT    /orders/:id/cancel
GET    /orders/:id/track
POST   /orders/bulk
```

#### 6. Logistics & Transport Module

**Features:**
- Transport assignment
- Real-time tracking
- Delivery scheduling
- Route optimization
- Driver management
- Proof of delivery
- Integration with 3PL (Porter, Delhivery)

**Endpoints:**
```
POST   /logistics/assign
GET    /logistics/track/:orderId
PUT    /logistics/:id/update-location
POST   /logistics/proof-of-delivery
GET    /logistics/drivers
POST   /logistics/integrate/porter
POST   /logistics/integrate/delhivery
```

#### 7. Payment Processing Module

**Features:**
- Multiple payment methods
- Credit management
- Payment gateway integration
- Transaction history
- Invoice generation
- Payment reminders
- Refund processing

**Endpoints:**
```
POST   /payments/initiate
POST   /payments/verify
GET    /payments/history
GET    /payments/credit/balance
POST   /payments/credit/request
POST   /payments/refund
```

#### 8. Document Management Module

**Features:**
- Invoice generation
- Delivery challan
- MTC (Material Test Certificate)
- Document templates
- Digital signatures
- Document versioning
- Bulk download

**Endpoints:**
```
GET    /documents/orders/:orderId
POST   /documents/generate/invoice
POST   /documents/generate/challan
POST   /documents/upload
GET    /documents/download/:id
```

#### 9. Chat & Communication Module

**Features:**
- One-to-one chat
- Group conversations
- Order-specific chats
- File sharing
- Message history
- Online status
- Typing indicators
- Read receipts

**Endpoints:**
```
POST   /chat/conversations
GET    /chat/conversations
POST   /chat/messages
GET    /chat/conversations/:id/messages
PUT    /chat/messages/:id/read
POST   /chat/upload
```

#### 10. Notification Module

**Features:**
- Push notifications
- SMS alerts
- Email notifications
- In-app notifications
- Notification preferences
- Order updates
- Payment reminders
- Stock alerts

**Endpoints:**
```
GET    /notifications
PUT    /notifications/:id/read
PUT    /notifications/mark-all-read
POST   /notifications/preferences
GET    /notifications/preferences
DELETE /notifications/:id
```

#### 11. Analytics & Reporting Module

**Features:**
- Role-based dashboards
- Sales analytics
- Inventory analytics
- Customer behavior analysis
- Product performance
- Geographic heat maps
- Custom reports
- Data export (CSV, PDF)
- Real-time metrics

**Endpoints:**
```
GET    /analytics/dashboard
GET    /analytics/sales
GET    /analytics/inventory
GET    /analytics/customers
GET    /analytics/products
GET    /analytics/geographic
POST   /analytics/custom-report
GET    /analytics/export
```

#### 12. Rewards Integration Module

**Features:**
- QR code scanning
- Reward points tracking
- Third-party app redirection
- Points redemption
- Reward history
- Manufacturer program integration

**Endpoints:**
```
POST   /rewards/scan-qr
GET    /rewards/balance
GET    /rewards/history
POST   /rewards/redeem
GET    /rewards/programs
```

#### 13. Admin Control Panel Module

**Features:**
- User management
- Role and permission management
- Product approval
- Pricing control
- Tier-based pricing
- System configuration
- Analytics overview
- Audit logs
- Support ticket management

**Endpoints:**
```
GET    /admin/users
PUT    /admin/users/:id/status
GET    /admin/roles
POST   /admin/roles
PUT    /admin/products/:id/approve
PUT    /admin/pricing/tiers
GET    /admin/audit-logs
GET    /admin/system-config
PUT    /admin/system-config
```

---

## 7. Role-Based Access Control (RBAC)

### 7.1 User Roles & Permissions Matrix

| Module | Feature | Manufacturer | Distributor | Dealer | Applicator | Developer | Transporter | Consumer | Admin |
|--------|---------|--------------|-------------|--------|------------|-----------|-------------|----------|-------|
| **Products** |
| View Products | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Create Products | ✓ | ✓ | ✓ | - | - | - | - | ✓ |
| Edit Own Products | ✓ | ✓ | ✓ | - | - | - | - | ✓ |
| Approve Products | - | - | - | - | - | - | - | ✓ |
| View Pricing Tiers | ✓ | ✓ | ✓ | ✓ | ✓ | - | ✓ | ✓ |
| Compare Prices | - | - | - | ✓ | ✓ | - | ✓ | - |
| Find Nearest Stock | - | - | - | ✓ | ✓ | - | ✓ | - |
| **Inventory** |
| View Inventory | ✓ | ✓ | ✓ | - | - | - | - | ✓ |
| Update Inventory | ✓ | ✓ | ✓ | - | - | - | - | ✓ |
| Transfer Stock | ✓ | ✓ | ✓ | - | - | - | - | ✓ |
| **Orders** |
| Create Order | ✓ | ✓ | ✓ | ✓ | ✓ | - | ✓ | ✓ |
| View Own Orders | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Approve Orders | - | ✓ | ✓ | - | - | - | - | ✓ |
| Cancel Orders | ✓* | ✓* | ✓* | ✓* | ✓* | - | ✓* | ✓ |
| Track Orders | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| **Logistics** |
| Assign Transport | ✓ | ✓ | ✓ | - | - | ✓ | - | ✓ |
| Update Delivery | - | - | - | - | - | ✓ | - | ✓ |
| View Tracking | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| **Payments** |
| Make Payment | - | ✓ | ✓ | ✓ | ✓ | - | ✓ | ✓ |
| View Transactions | ✓ | ✓ | ✓ | ✓ | ✓ | - | ✓ | ✓ |
| Manage Credit | - | ✓ | ✓ | - | - | - | - | ✓ |
| Issue Refund | ✓ | ✓ | ✓ | - | - | - | - | ✓ |
| **Communication** |
| Send Messages | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Create Groups | ✓ | ✓ | ✓ | - | - | - | - | ✓ |
| **Analytics** |
| View Dashboard | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Generate Reports | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | - | ✓ |
| Export Data | ✓ | ✓ | ✓ | ✓ | ✓ | - | - | ✓ |
| **User Management** |
| Create Sub-users | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | - | ✓ |
| Manage Roles | ✓ | ✓ | ✓ | ✓ | ✓ | - | - | ✓ |
| Approve KYC | - | - | - | - | - | - | - | ✓ |

*✓* = Own orders only, within specific time window

### 7.2 Sub-User Roles

#### Manufacturer Sub-Roles
- **Owner/Director**: Full access
- **Sales Executive**: Orders, customers, analytics
- **Dispatch In-charge**: Inventory, logistics, orders
- **QC In-charge**: Product quality, documents
- **Transport Coordinator**: Logistics only

#### Distributor Sub-Roles
- **Owner/Partner**: Full access
- **Warehouse In-charge**: Inventory, receiving
- **Billing Executive**: Orders, invoices, payments
- **Sales Representative**: Customers, orders

#### Dealer Sub-Roles
- **Owner**: Full access
- **Store Keeper**: Inventory, orders
- **Sales Executive**: Customers, orders

#### Applicator/Contractor Sub-Roles
- **Owner**: Full access
- **Site Supervisor**: Orders, delivery tracking
- **Purchase Coordinator**: Orders only
- **Accountant**: Payments, invoices

### 7.3 Permission Implementation

```javascript
// Permission Decorator (NestJS)
@Permissions('orders:create', 'orders:update')
@UseGuards(JwtAuthGuard, PermissionsGuard)
async updateOrder(@Param('id') id: string, @Body() dto: UpdateOrderDto) {
  return this.ordersService.update(id, dto);
}

// Permission Check with Conditions
@CheckPolicies((ability: AppAbility) => 
  ability.can('read', 'Order', { 
    organizationId: user.organizationId 
  })
)
async getOrders(@User() user: UserEntity) {
  return this.ordersService.findForOrganization(user.organizationId);
}

// CASL Ability Definition
export const defineAbilitiesFor = (user: UserEntity) => {
  const { can, cannot, build } = new AbilityBuilder(Ability);

  if (user.role === 'admin') {
    can('manage', 'all');
  }

  if (user.role === 'manufacturer') {
    can('create', 'Product');
    can('update', 'Product', { manufacturerId: user.organizationId });
    can('read', 'Order', { sellerId: user.organizationId });
  }

  if (user.role === 'dealer') {
    can('read', 'Product');
    can('create', 'Order');
    can('read', 'Order', { customerId: user.id });
    can('update', 'Order', { 
      customerId: user.id, 
      status: { $in: ['pending', 'confirmed'] }
    });
  }

  return build();
};
```

---

## 8. API Architecture

### 8.1 RESTful API Design

**Naming Conventions:**
```
GET    /api/v1/resource           // List all
GET    /api/v1/resource/:id       // Get single
POST   /api/v1/resource           // Create
PUT    /api/v1/resource/:id       // Update (full)
PATCH  /api/v1/resource/:id       // Update (partial)
DELETE /api/v1/resource/:id       // Delete
```

**Response Format:**
```javascript
// Success Response
{
  success: true,
  data: { ... },
  message: "Operation successful",
  timestamp: "2025-11-04T10:30:00Z"
}

// Error Response
{
  success: false,
  error: {
    code: "INVALID_INPUT",
    message: "Validation failed",
    details: [
      { field: "email", message: "Invalid email format" }
    ]
  },
  timestamp: "2025-11-04T10:30:00Z"
}

// Paginated Response
{
  success: true,
  data: [ ... ],
  pagination: {
    page: 1,
    limit: 20,
    total: 150,
    totalPages: 8,
    hasNext: true,
    hasPrev: false
  }
}
```

### 8.2 WebSocket Events

**Real-time Communication:**
```javascript
// Client -> Server Events
socket.emit('join_order_room', { orderId: '123' });
socket.emit('send_message', { conversationId: '456', message: '...' });
socket.emit('update_location', { lat: 21.1702, lng: 72.8311 });

// Server -> Client Events
socket.on('order_status_changed', (data) => { ... });
socket.on('new_message', (data) => { ... });
socket.on('inventory_updated', (data) => { ... });
socket.on('delivery_location_update', (data) => { ... });
socket.on('notification', (data) => { ... });
```

### 8.3 API Gateway Configuration

```javascript
// Rate Limiting Configuration
const rateLimits = {
  global: { windowMs: 60000, max: 100 },
  auth: { windowMs: 900000, max: 5 }, // 5 attempts per 15 min
  orders: { windowMs: 60000, max: 50 },
  search: { windowMs: 60000, max: 200 },
  analytics: { windowMs: 60000, max: 30 }
};

// CORS Configuration
const corsOptions = {
  origin: [
    'https://app.yourdomain.com',
    'https://admin.yourdomain.com',
    'https://analytics.yourdomain.com'
  ],
  credentials: true,
  methods: ['GET', 'POST', 'PUT', 'PATCH', 'DELETE'],
  allowedHeaders: ['Content-Type', 'Authorization']
};

// API Versioning
/api/v1/...  // Current stable
/api/v2/...  // Next version (when breaking changes)
```

---

## 9. Integration Architecture

### 9.1 Payment Gateway Integration

**Razorpay Integration:**
```javascript
// Payment Initiation
POST /api/v1/payments/initiate
{
  orderId: "ORD123",
  amount: 50000, // in paise
  currency: "INR",
  method: "online"
}

// Webhook Handler
POST /webhooks/razorpay
Headers: X-Razorpay-Signature
{
  event: "payment.captured",
  payload: { ... }
}

// Verification
- Verify webhook signature
- Update order payment status
- Trigger notifications
- Update ledger
```

### 9.2 Logistics Integration

**Porter API Integration:**
```javascript
// Create Delivery Request
POST https://api.porter.in/v1/orders
{
  pickup_details: {
    address: { ... },
    contact: { ... }
  },
  drop_details: {
    address: { ... },
    contact: { ... }
  },
  item_details: {
    description: "Construction Materials",
    size: "large"
  }
}

// Webhook for Status Updates
POST /webhooks/porter
{
  order_id: "POR123",
  status: "in_transit",
  driver: { ... },
  location: { ... }
}
```

**Delhivery Integration:**
```javascript
// Create Shipment
POST https://track.delhivery.com/api/cmu/create.json
{
  shipments: [{
    name: "Customer Name",
    add: "Address",
    pin: "395006",
    phone: "9876543210",
    payment_mode: "Prepaid"
  }]
}

// Track Shipment
GET https://track.delhivery.com/api/v1/packages/json/?waybill=WAY123
```

### 9.3 Maps Integration

**Google Maps API:**
```javascript
// Address Autocomplete
GET /maps/api/place/autocomplete/json
  ?input=user_input
  &types=address
  &components=country:in

// Geocoding
GET /maps/api/geocode/json
  ?address=full_address

// Distance Matrix
GET /maps/api/distancematrix/json
  ?origins=lat1,lng1
  &destinations=lat2,lng2

// Route Optimization
POST /maps/api/directions/json
{
  origin: { ... },
  destination: { ... },
  waypoints: [ ... ]
}
```

### 9.4 SMS & Email Integration

**Twilio (SMS):**
```javascript
// Send OTP
POST https://api.twilio.com/2010-04-01/Accounts/{AccountSid}/Messages.json
{
  From: "+1234567890",
  To: "+919876543210",
  Body: "Your OTP is: 123456"
}
```

**SendGrid (Email):**
```javascript
// Send Email
POST https://api.sendgrid.com/v3/mail/send
{
  personalizations: [{
    to: [{ email: "user@example.com" }],
    subject: "Order Confirmed"
  }],
  from: { email: "noreply@yourdomain.com" },
  content: [{
    type: "text/html",
    value: "<html>...</html>"
  }]
}
```

### 9.5 QR Code & Rewards Integration

**Manufacturer App Integration:**
```javascript
// QR Code Data Structure
{
  type: "reward_points",
  productId: "PRD123",
  batchNo: "BATCH001",
  manufacturer: "dr_fixit",
  appLink: {
    android: "https://play.google.com/store/apps/details?id=com.pidilite.ginnie",
    ios: "https://apps.apple.com/app/ginnie/id123456789"
  },
  deepLink: "ginnie://scan?code=ABC123"
}

// Handle QR Scan
1. Parse QR code data
2. Check if app installed (deep link)
3. If installed: Open app with data
4. If not: Redirect to Play Store/App Store
5. Log scan event for analytics
```

---

## 10. Deployment Architecture

### 10.1 Cloud Infrastructure (AWS/GCP)

```
┌──────────────────────────────────────────────────────────────┐
│                    PRODUCTION ENVIRONMENT                     │
└──────────────────────────────────────────────────────────────┘

                         ┌─────────────┐
                         │   Route 53  │
                         │  (DNS/CDN)  │
                         └──────┬──────┘
                                │
                    ┌───────────┴───────────┐
                    │                       │
            ┌───────▼──────┐        ┌──────▼──────┐
            │  CloudFlare  │        │  CloudFront │
            │   (WAF/CDN)  │        │    (CDN)    │
            └───────┬──────┘        └──────┬──────┘
                    │                       │
            ┌───────▼───────────────────────▼──────┐
            │        Load Balancer (ALB)           │
            │   - SSL Termination                  │
            │   - Health Checks                    │
            └───────┬──────────────────────────────┘
                    │
        ┌───────────┴────────────┐
        │                        │
┌───────▼────────┐      ┌───────▼────────┐
│  Web Servers   │      │  API Servers   │
│   (Next.js)    │      │   (NestJS)     │
│                │      │                │
│  - User App    │      │  - Auth Svc    │
│  - Admin App   │      │  - Order Svc   │
│  - Analytics   │      │  - Payment Svc │
│                │      │  - Chat Svc    │
│  Auto-scaling  │      │                │
│  Min: 2        │      │  Auto-scaling  │
│  Max: 10       │      │  Min: 3        │
└────────────────┘      │  Max: 20       │
                        └───────┬────────┘
                                │
                    ┌───────────┴────────────┐
                    │                        │
            ┌───────▼────────┐      ┌───────▼────────┐
            │   MongoDB      │      │     Redis      │
            │   Atlas        │      │   ElastiCache  │
            │                │      │                │
            │  - 3 Replicas  │      │  - 2 Nodes     │
            │  - Auto Backup │      │  - Cluster     │
            └────────────────┘      └────────────────┘

┌──────────────────────────────────────────────────────────────┐
│                    STORAGE & SERVICES                        │
├──────────────────────────────────────────────────────────────┤
│  S3/Cloud Storage  │  SQS/Pub-Sub  │  CloudWatch/Monitoring │
│  - Documents       │  - Job Queue  │  - Logs                │
│  - Images          │  - Events     │  - Metrics             │
│  - Backups         │  - Async Ops  │  - Alerts              │
└──────────────────────────────────────────────────────────────┘
```

### 10.2 Container Architecture (Docker + Kubernetes)

```yaml
# Kubernetes Deployment Structure

┌─────────────────────────────────────────────────────────┐
│                   K8s Cluster                           │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  ┌─────────────────────────────────────────────────┐  │
│  │           Ingress Controller (Nginx)            │  │
│  │  - SSL/TLS Termination                          │  │
│  │  - Path-based routing                           │  │
│  └─────────────────────────────────────────────────┘  │
│                         │                              │
│     ┌───────────────────┼───────────────────┐         │
│     │                   │                   │         │
│  ┌──▼──────┐    ┌──────▼─────┐    ┌───────▼──────┐  │
│  │  Web    │    │  API       │    │  Worker      │  │
│  │  Pods   │    │  Pods      │    │  Pods        │  │
│  │         │    │            │    │              │  │
│  │ Next.js │    │  NestJS    │    │  Bull Queue  │  │
│  │ Replicas│    │  Replicas  │    │  Processors  │  │
│  │ Min: 2  │    │  Min: 3    │    │  Min: 2      │  │
│  │ Max: 10 │    │  Max: 20   │    │  Max: 10     │  │
│  └─────────┘    └────────────┘    └──────────────┘  │
│                                                        │
│  ┌─────────────────────────────────────────────────┐ │
│  │            Services (ClusterIP/NodePort)        │ │
│  │  - web-service                                  │ │
│  │  - api-service                                  │ │
│  │  - websocket-service                            │ │
│  └─────────────────────────────────────────────────┘ │
│                                                        │
│  ┌─────────────────────────────────────────────────┐ │
│  │         ConfigMaps & Secrets                    │ │
│  │  - Environment variables                        │ │
│  │  - API keys (encrypted)                         │ │
│  │  - Database credentials                         │ │
│  └─────────────────────────────────────────────────┘ │
│                                                        │
│  ┌─────────────────────────────────────────────────┐ │
│  │        Persistent Volumes                       │ │
│  │  - Logs volume                                  │ │
│  │  - Upload volume                                │ │
│  └─────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────┘
```

### 10.3 CI/CD Pipeline

```yaml
# GitHub Actions Workflow

name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - Checkout code
      - Setup Node.js
      - Install dependencies
      - Run unit tests
      - Run integration tests
      - Run security scan (Snyk)
      - Generate coverage report

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - Build Docker images
      - Tag images with commit SHA
      - Push to Container Registry
      - Scan images for vulnerabilities

  deploy-staging:
    needs: build
    environment: staging
    steps:
      - Deploy to staging cluster
      - Run smoke tests
      - Run E2E tests
      - Performance tests

  deploy-production:
    needs: deploy-staging
    environment: production
    steps:
      - Deploy to production cluster
      - Blue-green deployment
      - Health check
      - Rollback on failure
      - Notify team (Slack)
```

### 10.4 Environment Configuration

**Development:**
- Local Docker Compose
- MongoDB local instance
- Redis local instance
- Mock payment gateway
- Mock SMS/Email

**Staging:**
- AWS/GCP staging environment
- MongoDB Atlas shared cluster
- Redis ElastiCache
- Sandbox payment gateway
- Test SMS/Email accounts

**Production:**
- AWS/GCP production environment
- MongoDB Atlas dedicated cluster (M30+)
- Redis ElastiCache cluster mode
- Production payment gateway
- Production SMS/Email services
- CDN enabled
- Auto-scaling configured
- Monitoring & alerts active

---

## 11. Scalability & Performance

### 11.1 Horizontal Scaling Strategy

**Auto-scaling Rules:**
```yaml
API Servers:
  Min Instances: 3
  Max Instances: 20
  Scale Up:
    - CPU > 70% for 3 minutes
    - Memory > 80% for 3 minutes
    - Request queue > 1000
  Scale Down:
    - CPU < 30% for 10 minutes
    - Memory < 40% for 10 minutes

Web Servers:
  Min Instances: 2
  Max Instances: 10
  Scale Up:
    - CPU > 60% for 5 minutes
  Scale Down:
    - CPU < 20% for 15 minutes

Worker Pods:
  Min Instances: 2
  Max Instances: 10
  Scale based on Queue Length:
    - Queue > 500 jobs: Scale up
    - Queue < 50 jobs: Scale down
```

### 11.2 Caching Strategy

**Multi-Layer Caching:**
```javascript
// Layer 1: Browser Cache (Static Assets)
Cache-Control: public, max-age=31536000, immutable

// Layer 2: CDN Cache (Static + Dynamic)
- Product images: 24 hours
- Product data: 5 minutes
- User-specific: No cache

// Layer 3: Redis Cache (Application)
- User sessions: 7 days
- Product catalog: 1 hour
- Inventory: 5 minutes (real-time)
- Analytics: 15 minutes

// Layer 4: Database Query Cache
- MongoDB query results cache
- Aggregation pipeline cache

// Cache Invalidation Strategy
- Time-based: TTL on all cache entries
- Event-based: Invalidate on update
- Pattern-based: Clear related keys
```

### 11.3 Database Optimization

**Read Replicas:**
```
Primary (Write): All write operations
Secondary-1 (Read): Analytics queries
Secondary-2 (Read): User queries
Secondary-3 (Read): Order queries
```

**Query Optimization:**
```javascript
// Use projection to return only needed fields
db.products.find({}, { name: 1, price: 1, image: 1 })

// Use indexes for frequent queries
db.orders.find({ customerId: id, status: 'pending' })
  .hint({ customerId: 1, status: 1 })

// Aggregation pipeline optimization
db.orders.aggregate([
  { $match: { createdAt: { $gte: startDate } } }, // Filter early
  { $group: { _id: "$status", count: { $sum: 1 } } },
  { $sort: { count: -1 } },
  { $limit: 10 }
])

// Avoid large document scans
- Use pagination
- Implement cursor-based navigation
- Set reasonable limits
```

### 11.4 Performance Targets

| Metric | Target | Critical Threshold |
|--------|--------|-------------------|
| API Response Time (p95) | < 500ms | < 1s |
| API Response Time (p99) | < 1s | < 2s |
| Page Load Time | < 2s | < 3s |
| Time to Interactive | < 3s | < 5s |
| Database Query Time | < 100ms | < 300ms |
| Search Response Time | < 300ms | < 500ms |
| WebSocket Latency | < 100ms | < 200ms |
| Uptime | 99.9% | 99.5% |
| Error Rate | < 0.1% | < 0.5% |

---

## 12. Module-wise Features & Responsibilities

### 12.1 Frontend Modules (Next.js)

#### User Portal (users.domain.com)

**1. Authentication Pages**
- `/login` - Login page
- `/register` - Registration with role selection
- `/verify-otp` - OTP verification
- `/forgot-password` - Password reset

**2. Dashboard Module**
- `/dashboard` - Role-based dashboard
- Real-time metrics
- Quick actions
- Recent activities

**3. Product Catalog Module**
- `/products` - Product listing with filters
- `/products/[id]` - Product details
- `/products/search` - Search results
- `/products/compare` - Product comparison
- `/products/nearby` - Nearby stock finder (NEW)
- `/products/price-compare` - Price comparison (NEW)

**3A. Smart Purchase Module** (NEW)
- `/purchase/find-best-price` - Find best price & location
- `/purchase/compare-sellers` - Compare sellers
- `/purchase/nearest-stock` - Find nearest stock points
- `/purchase/tier-benefits` - View tier benefits

**4. Order Management Module**
- `/orders` - Order history
- `/orders/create` - New order creation
- `/orders/[id]` - Order details & tracking
- `/orders/[id]/invoice` - Invoice view

**5. Inventory Module** (Dealer/Distributor)
- `/inventory` - Stock overview
- `/inventory/products/[id]` - Product inventory
- `/inventory/transfer` - Stock transfer

**6. Chat Module**
- `/messages` - Conversations list
- `/messages/[id]` - Chat interface

**7. Profile & Settings**
- `/profile` - User profile
- `/profile/organization` - Organization details
- `/profile/settings` - Preferences
- `/profile/kyc` - KYC documents

**8. Analytics Module**
- `/analytics` - Reports dashboard
- `/analytics/sales` - Sales analytics
- `/analytics/inventory` - Inventory analytics

#### Admin Dashboard (admin.domain.com)

**1. User Management**
- `/users` - User list & management
- `/users/[id]` - User details
- `/users/kyc-approvals` - KYC verification queue

**2. Product Management**
- `/products` - Product management
- `/products/approvals` - Product approval queue
- `/products/categories` - Category management
- `/products/pricing` - Pricing control (NEW)
- `/products/tiers` - Tier management (NEW)

**2A. Pricing Control** (NEW)
- `/pricing/tiers` - Tier configuration
- `/pricing/rules` - Pricing rules management
- `/pricing/analytics` - Pricing analytics
- `/pricing/compare` - Price comparison report

**3. Order Management**
- `/orders` - All orders overview
- `/orders/pending` - Pending approvals
- `/orders/analytics` - Order analytics

**4. Pricing Control**
- `/pricing/tiers` - Tier management
- `/pricing/products` - Product pricing

**5. System Configuration**
- `/settings/roles` - Role management
- `/settings/permissions` - Permission configuration
- `/settings/general` - General settings

**6. Monitoring**
- `/monitoring/system` - System health
- `/monitoring/logs` - Audit logs
- `/monitoring/analytics` - Platform analytics

### 12.2 Backend Modules (NestJS)

#### Core Modules

**1. Auth Module** (`src/modules/auth`)
```
auth.module.ts
auth.controller.ts
auth.service.ts
├── strategies/
│   ├── jwt.strategy.ts
│   ├── refresh.strategy.ts
│   └── local.strategy.ts
├── guards/
│   ├── jwt-auth.guard.ts
│   ├── roles.guard.ts
│   └── permissions.guard.ts
├── decorators/
│   ├── public.decorator.ts
│   ├── roles.decorator.ts
│   └── permissions.decorator.ts
└── dto/
    ├── login.dto.ts
    ├── register.dto.ts
    └── verify-otp.dto.ts
```

**2. Users Module** (`src/modules/users`)
```
users.module.ts
users.controller.ts
users.service.ts
users.repository.ts
├── schemas/
│   └── user.schema.ts
├── dto/
│   ├── create-user.dto.ts
│   ├── update-user.dto.ts
│   └── kyc.dto.ts
└── interfaces/
    └── user.interface.ts
```

**3. Products Module** (`src/modules/products`)
```
products.module.ts
products.controller.ts
products.service.ts
products.repository.ts
├── schemas/
│   ├── product.schema.ts
│   ├── category.schema.ts
│   ├── product-pricing.schema.ts         // NEW
│   └── pricing-tier.schema.ts            // NEW
├── dto/
│   ├── create-product.dto.ts
│   ├── update-product.dto.ts
│   ├── product-filter.dto.ts
│   ├── pricing-tier.dto.ts               // NEW
│   └── price-comparison.dto.ts           // NEW
└── services/
    ├── search.service.ts
    ├── qr-code.service.ts
    ├── pricing.service.ts                // NEW
    ├── tier-calculation.service.ts       // NEW
    └── nearby-stock.service.ts           // NEW
```

**3A. Pricing Module** (`src/modules/pricing`) - NEW
```
pricing.module.ts
pricing.controller.ts
pricing.service.ts
pricing.repository.ts
├── schemas/
│   ├── pricing-tier.schema.ts
│   ├── product-pricing.schema.ts
│   └── pricing-rule.schema.ts
├── dto/
│   ├── create-tier.dto.ts
│   ├── update-pricing.dto.ts
│   └── calculate-price.dto.ts
├── services/
│   ├── tier-management.service.ts
│   ├── dynamic-pricing.service.ts
│   ├── price-comparison.service.ts
│   └── volume-discount.service.ts
└── processors/
    └── tier-calculation.processor.ts
```

**4. Inventory Module** (`src/modules/inventory`)
```
inventory.module.ts
inventory.controller.ts
inventory.service.ts
inventory.repository.ts
├── schemas/
│   └── inventory.schema.ts
├── dto/
│   ├── adjust-inventory.dto.ts
│   └── transfer-inventory.dto.ts
└── services/
    ├── stock-alert.service.ts
    └── batch-tracking.service.ts
```

**5. Orders Module** (`src/modules/orders`)
```
orders.module.ts
orders.controller.ts
orders.service.ts
orders.repository.ts
├── schemas/
│   └── order.schema.ts
├── dto/
│   ├── create-order.dto.ts
│   ├── update-order.dto.ts
│   └── order-filter.dto.ts
├── services/
│   ├── order-approval.service.ts
│   ├── order-workflow.service.ts
│   └── pricing.service.ts
└── processors/
    └── order.processor.ts
```

**6. Logistics Module** (`src/modules/logistics`)
```
logistics.module.ts
logistics.controller.ts
logistics.service.ts
logistics.repository.ts
├── schemas/
│   └── logistics.schema.ts
├── dto/
│   ├── assign-transport.dto.ts
│   └── update-location.dto.ts
├── services/
│   ├── porter.service.ts
│   ├── delhivery.service.ts
│   └── route-optimization.service.ts
└── gateways/
    └── tracking.gateway.ts
```

**7. Payments Module** (`src/modules/payments`)
```
payments.module.ts
payments.controller.ts
payments.service.ts
payments.repository.ts
├── schemas/
│   ├── payment.schema.ts
│   └── credit-ledger.schema.ts
├── dto/
│   ├── initiate-payment.dto.ts
│   └── verify-payment.dto.ts
├── services/
│   ├── razorpay.service.ts
│   ├── paytm.service.ts
│   └── credit-management.service.ts
└── webhooks/
    └── payment-webhook.controller.ts
```

**8. Documents Module** (`src/modules/documents`)
```
documents.module.ts
documents.controller.ts
documents.service.ts
documents.repository.ts
├── schemas/
│   └── document.schema.ts
├── dto/
│   ├── upload-document.dto.ts
│   └── generate-invoice.dto.ts
├── services/
│   ├── invoice-generator.service.ts
│   ├── challan-generator.service.ts
│   └── storage.service.ts
└── templates/
    ├── invoice.template.ts
    └── challan.template.ts
```

**9. Chat Module** (`src/modules/chat`)
```
chat.module.ts
chat.controller.ts
chat.service.ts
chat.repository.ts
├── schemas/
│   ├── message.schema.ts
│   └── conversation.schema.ts
├── dto/
│   ├── send-message.dto.ts
│   └── create-conversation.dto.ts
├── gateways/
│   └── chat.gateway.ts
└── services/
    └── message-notification.service.ts
```

**10. Notifications Module** (`src/modules/notifications`)
```
notifications.module.ts
notifications.controller.ts
notifications.service.ts
notifications.repository.ts
├── schemas/
│   └── notification.schema.ts
├── dto/
│   ├── send-notification.dto.ts
│   └── notification-preferences.dto.ts
├── services/
│   ├── push-notification.service.ts
│   ├── sms.service.ts
│   └── email.service.ts
└── processors/
    └── notification.processor.ts
```

**11. Analytics Module** (`src/modules/analytics`)
```
analytics.module.ts
analytics.controller.ts
analytics.service.ts
analytics.repository.ts
├── schemas/
│   └── analytics.schema.ts
├── dto/
│   ├── analytics-query.dto.ts
│   └── custom-report.dto.ts
├── services/
│   ├── sales-analytics.service.ts
│   ├── inventory-analytics.service.ts
│   └── customer-analytics.service.ts
└── processors/
    └── analytics-aggregation.processor.ts
```

**12. Rewards Module** (`src/modules/rewards`)
```
rewards.module.ts
rewards.controller.ts
rewards.service.ts
rewards.repository.ts
├── schemas/
│   └── rewards.schema.ts
├── dto/
│   ├── scan-qr.dto.ts
│   └── redeem-points.dto.ts
└── services/
    └── manufacturer-integration.service.ts
```

#### Shared Modules

**Common Module** (`src/common`)
```
├── decorators/
│   ├── user.decorator.ts
│   ├── roles.decorator.ts
│   └── permissions.decorator.ts
├── filters/
│   ├── http-exception.filter.ts
│   └── all-exceptions.filter.ts
├── guards/
│   ├── jwt-auth.guard.ts
│   ├── roles.guard.ts
│   └── permissions.guard.ts
├── interceptors/
│   ├── logging.interceptor.ts
│   ├── transform.interceptor.ts
│   └── cache.interceptor.ts
├── pipes/
│   ├── validation.pipe.ts
│   └── parse-objectid.pipe.ts
└── utils/
    ├── password.util.ts
    ├── otp.util.ts
    └── file-upload.util.ts
```

**Database Module** (`src/database`)
```
database.module.ts
database.service.ts
└── providers/
    ├── mongodb.provider.ts
    └── redis.provider.ts
```

**Config Module** (`src/config`)
```
config.module.ts
├── app.config.ts
├── database.config.ts
├── jwt.config.ts
├── payment.config.ts
└── storage.config.ts
```

### 12.3 Mobile App Modules (Flutter)

**Project Structure:**
```
lib/
├── main.dart
├── app.dart
├── core/
│   ├── constants/
│   ├── themes/
│   ├── utils/
│   └── network/
│       ├── api_client.dart
│       ├── api_endpoints.dart
│       └── websocket_client.dart
├── features/
│   ├── auth/
│   │   ├── data/
│   │   ├── domain/
│   │   ├── presentation/
│   │   │   ├── pages/
│   │   │   ├── widgets/
│   │   │   └── bloc/
│   │   └── auth_module.dart
│   ├── home/
│   ├── products/
│   ├── orders/
│   ├── inventory/
│   ├── chat/
│   ├── profile/
│   ├── analytics/
│   └── notifications/
├── shared/
│   ├── widgets/
│   ├── models/
│   └── services/
└── routes/
    └── app_routes.dart
```

**Key Flutter Packages:**
```yaml
dependencies:
  flutter: sdk: flutter
  
  # State Management
  flutter_riverpod: ^2.4.0
  
  # Network
  dio: ^5.3.0
  socket_io_client: ^2.0.0
  
  # Local Storage
  hive: ^2.2.3
  hive_flutter: ^1.1.0
  
  # UI
  flutter_svg: ^2.0.7
  cached_network_image: ^3.3.0
  shimmer: ^3.0.0
  
  # Maps & Location
  google_maps_flutter: ^2.5.0
  geolocator: ^10.0.0
  
  # Notifications
  firebase_messaging: ^14.6.9
  flutter_local_notifications: ^15.1.1
  
  # QR Code
  qr_code_scanner: ^1.0.1
  qr_flutter: ^4.1.0
  
  # Utilities
  intl: ^0.18.1
  url_launcher: ^6.1.14
  share_plus: ^7.1.0
  
  # Payment
  razorpay_flutter: ^1.3.5
```

---

## 13. Budget Estimation

### 13.1 Phase-wise Budget Distribution

| Phase | Deliverables | Duration | Cost (INR) |
|-------|-------------|----------|------------|
| **Phase 1: Foundation** | | | |
| - Requirement Analysis | Detailed specs, wireframes | 2 weeks | ₹50,000 |
| - UI/UX Design | Complete design system | 3 weeks | ₹80,000 |
| - Project Setup | Repository, CI/CD, infrastructure | 1 week | ₹30,000 |
| | **Phase 1 Subtotal** | **6 weeks** | **₹1,60,000** |
| **Phase 2: Core Development** | | | |
| - Backend API Development | Auth, Users, Products, Inventory | 5 weeks | ₹2,00,000 |
| - Database Schema & Setup | MongoDB, Redis setup | 1 week | ₹40,000 |
| - Pricing Engine | Tier-based pricing system | 2 weeks | ₹80,000 |
| - Admin Dashboard | User management, Product mgmt | 4 weeks | ₹1,20,000 |
| - User Web Portal (Basic) | Auth, Profile, Products listing | 3 weeks | ₹90,000 |
| | **Phase 2 Subtotal** | **15 weeks** | **₹5,30,000** |
| **Phase 3: Advanced Features** | | | |
| - Smart Purchase System | Nearest stock, price comparison | 2 weeks | ₹80,000 |
| - Order Management System | Complete order workflow | 3 weeks | ₹1,20,000 |
| - Logistics Integration | Porter, Delhivery APIs | 2 weeks | ₹60,000 |
| - Payment Integration | Razorpay, Credit system | 2 weeks | ₹50,000 |
| - Mobile App (Flutter) | iOS & Android apps | 5 weeks | ₹1,50,000 |
| | **Phase 3 Subtotal** | **14 weeks** | **₹4,60,000** |
| **Phase 4: Communication & Analytics** | | | |
| - Chat System | Real-time messaging | 2 weeks | ₹60,000 |
| - Notification System | Push, SMS, Email | 1.5 weeks | ₹40,000 |
| - Analytics Dashboard | MIS reports, charts | 2.5 weeks | ₹70,000 |
| - Document Management | Invoice, Challan generation | 1.5 weeks | ₹40,000 |
| - Tier Progress Tracking | User tier dashboard | 1 week | ₹30,000 |
| | **Phase 4 Subtotal** | **8.5 weeks** | **₹2,40,000** |
| **Phase 5: Testing & Deployment** | | | |
| - Testing (QA) | Unit, Integration, E2E tests | 3 weeks | ₹80,000 |
| - Security Audit | Penetration testing | 1 week | ₹40,000 |
| - Deployment | Production setup, monitoring | 1 week | ₹50,000 |
| - Documentation | User manuals, API docs | 1 week | ₹30,000 |
| - Training | User & admin training | 1 week | ₹20,000 |
| | **Phase 5 Subtotal** | **7 weeks** | **₹2,20,000** |
| **Contingency & Buffer** | Bug fixes, minor changes | - | ₹90,000 |
| | | | |
| | **TOTAL PROJECT COST** | **50.5 weeks** | **₹16,00,000** |

### 13.2 Technology-wise Budget Breakdown

| Technology/Component | Description | Cost (INR) | Percentage |
|---------------------|-------------|------------|------------|
| **Frontend Development** | | | |
| Next.js Web Apps | User portal + Admin dashboard + Analytics | ₹3,00,000 | 18.75% |
| Flutter Mobile App | iOS & Android development | ₹1,50,000 | 9.38% |
| UI/UX Design | Complete design system, prototypes | ₹80,000 | 5% |
| **Subtotal Frontend** | | **₹5,30,000** | **33.13%** |
| **Backend Development** | | | |
| NestJS API Development | All microservices & APIs | ₹3,20,000 | 20% |
| Smart Pricing Engine | Tier-based pricing, price comparison | ₹80,000 | 5% |
| Database Design & Setup | MongoDB schema, indexes, optimization | ₹60,000 | 3.75% |
| Redis Implementation | Caching, sessions, queue | ₹30,000 | 1.88% |
| WebSocket/Real-time | Chat, tracking, notifications | ₹40,000 | 2.5% |
| **Subtotal Backend** | | **₹5,30,000** | **33.13%** |
| **Integrations** | | | |
| Payment Gateways | Razorpay, Paytm integration | ₹50,000 | 3.13% |
| Logistics APIs | Porter, Delhivery integration | ₹60,000 | 3.75% |
| Maps Integration | Google Maps, geocoding, nearby search | ₹40,000 | 2.5% |
| SMS/Email Services | Twilio, SendGrid setup | ₹30,000 | 1.88% |
| **Subtotal Integrations** | | **₹1,80,000** | **11.25%** |
| **Infrastructure & DevOps** | | | |
| Cloud Setup (AWS/GCP) | Initial setup, configuration | ₹40,000 | 2.5% |
| CI/CD Pipeline | GitHub Actions, Docker, K8s | ₹30,000 | 1.88% |
| Monitoring & Logging | ELK stack, Prometheus, Grafana | ₹30,000 | 1.88% |
| **Subtotal Infrastructure** | | **₹1,00,000** | **6.25%** |
| **Testing & Quality** | | | |
| QA Testing | Manual + Automated testing | ₹80,000 | 5% |
| Security Audit | Penetration testing, VAPT | ₹40,000 | 2.5% |
| **Subtotal Testing** | | **₹1,20,000** | **7.5%** |
| **Documentation & Training** | | | |
| Technical Documentation | API docs, architecture docs | ₹20,000 | 1.25% |
| User Documentation | User manuals, guides | ₹15,000 | 0.94% |
| Training Sessions | User & admin training | ₹25,000 | 1.56% |
| **Subtotal Docs & Training** | | **₹60,000** | **3.75%** |
| **Project Management** | | | |
| Project Management | Planning, coordination, meetings | ₹60,000 | 3.75% |
| **Contingency Buffer** | Unexpected changes, fixes | ₹20,000 | 1.25% |
| | | | |
| | **TOTAL PROJECT COST** | **₹16,00,000** | **100%** |

### 13.3 Monthly Operational Costs (Post-Launch)

| Service | Description | Monthly Cost (INR) |
|---------|-------------|-------------------|
| **Infrastructure** | | |
| Cloud Hosting (AWS/GCP) | EC2/Compute Engine instances | ₹15,000 |
| MongoDB Atlas | M30 Cluster (Dedicated) | ₹25,000 |
| Redis ElastiCache | 2-node cluster | ₹8,000 |
| Cloud Storage (S3) | Document & image storage | ₹3,000 |
| CDN (Cloudflare) | Content delivery | ₹2,000 |
| **Subtotal Infrastructure** | | **₹53,000** |
| **External Services** | | |
| SMS (Twilio) | ~10,000 SMS/month | ₹5,000 |
| Email (SendGrid) | ~50,000 emails/month | ₹2,000 |
| Payment Gateway | Transaction fees (1.5-2%) | Variable |
| Maps API (Google) | Geocoding, directions | ₹3,000 |
| Push Notifications (FCM) | Firebase | ₹0 (Free tier) |
| **Subtotal Services** | | **₹10,000** |
| **Monitoring & Security** | | |
| SSL Certificates | Let's Encrypt/Cloudflare | ₹0 (Free) |
| Monitoring Tools | Datadog/New Relic | ₹5,000 |
| Backup Storage | Automated backups | ₹2,000 |
| **Subtotal Monitoring** | | **₹7,000** |
| **Maintenance & Support** | | |
| Bug Fixes & Updates | Developer hours | ₹20,000 |
| Customer Support | Helpdesk | ₹15,000 |
| **Subtotal Maintenance** | | **₹35,000** |
| | | |
| | **TOTAL MONTHLY COST** | **₹1,05,000** |

### 13.4 Revised Budget Breakdown (₹8-10 Lakhs Target)

**Note:** This MVP version focuses on essential features including the Smart Pricing System, which is critical for platform adoption.

| Phase | Deliverables | Duration | Cost (INR) |
|-------|-------------|----------|------------|
| **Phase 1: MVP Core Features** | | | |
| - Project Setup & Design | Wireframes, basic design | 2 weeks | ₹40,000 |
| - Backend Core APIs | Auth, Users, Products, Orders | 5 weeks | ₹1,50,000 |
| - Basic Pricing Engine | Essential tier-based pricing | 1.5 weeks | ₹50,000 |
| - Database Setup | MongoDB, Redis | 1 week | ₹30,000 |
| - Admin Dashboard (Basic) | Essential features only | 3 weeks | ₹80,000 |
| - User Web Portal (Basic) | Core features + price comparison | 3 weeks | ₹80,000 |
| | **Phase 1 Subtotal** | **15.5 weeks** | **₹4,30,000** |
| **Phase 2: Mobile & Integrations** | | | |
| - Mobile App (Flutter) | Core features, both platforms | 4 weeks | ₹1,20,000 |
| - Nearby Stock Finder | Location-based search | 1 week | ₹30,000 |
| - Payment Integration | Razorpay only | 1.5 weeks | ₹40,000 |
| - Logistics Integration | Porter or Delhivery (one) | 1.5 weeks | ₹40,000 |
| - Basic Chat | Simple messaging | 1.5 weeks | ₹35,000 |
| | **Phase 2 Subtotal** | **9.5 weeks** | **₹2,65,000** |
| **Phase 3: Analytics & Polish** | | | |
| - Notifications | Push + Email (no SMS) | 1.5 weeks | ₹30,000 |
| - Basic Analytics | Essential reports + tier tracking | 2 weeks | ₹50,000 |
| - Document Generation | Invoice, Challan | 1.5 weeks | ₹30,000 |
| | **Phase 3 Subtotal** | **5 weeks** | **₹1,10,000** |
| **Phase 4: Testing & Launch** | | | |
| - Testing | Essential QA | 2 weeks | ₹50,000 |
| - Deployment | Basic setup | 1 week | ₹30,000 |
| - Documentation | Essential docs | 1 week | ₹20,000 |
| **Contingency** | Bug fixes, minor changes | - | ₹95,000 |
| | **Phase 4 Subtotal** | **4 weeks** | **₹1,95,000** |
| | | | |
| | **TOTAL MVP COST** | **34 weeks** | **₹10,00,000** |

**MVP Includes (Critical Features):**
- ✓ Smart Pricing & Tier-based system
- ✓ Nearest stock finder with price comparison
- ✓ Product creation by Manufacturer/Distributor/Dealer
- ✓ Basic order management
- ✓ Mobile app (iOS & Android)
- ✓ One payment gateway
- ✓ One logistics integration
- ✓ Basic chat & notifications

**Post-MVP Additions (Can be added later):**
- Advanced analytics dashboard
- Multiple logistics integrations
- SMS notifications
- Rewards integration (QR scanning)
- Advanced chat features (file sharing, groups)
- Multiple payment gateways
- Credit management system

---

## 14. Implementation Timeline

### Gantt Chart Overview (50.5-week Full Version)

```
Weeks:  1  2  3  4  5  6  7  8  9  10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50

Phase 1: Foundation
└ Requirements       [====]
└ UI/UX Design            [======]
└ Project Setup                 [==]

Phase 2: Core Development
└ Backend APIs                     [===========]
└ Database                              [==]
└ Pricing Engine                           [====]
└ Admin Dashboard                              [========]
└ Web Portal                                      [======]

Phase 3: Advanced
└ Smart Purchase                                         [====]
└ Order System                                              [======]
└ Logistics                                                      [====]
└ Payments                                                          [====]
└ Mobile App                                                            [==========]

Phase 4: Features
└ Chat System                                                                    [====]
└ Notifications                                                                     [===]
└ Analytics                                                                           [=====]
└ Documents                                                                              [===]
└ Tier Tracking                                                                            [==]

Phase 5: Launch
└ Testing                                                                                   [======]
└ Security Audit                                                                               [==]
└ Deployment                                                                                     [==]
└ Documentation                                                                                    [==]
└ Training                                                                                          [==]
```

### MVP Timeline (34-week Version)

```
Weeks:  1  2  3  4  5  6  7  8  9  10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34

Phase 1: MVP Core
└ Setup & Design  [====]
└ Backend Core         [===========]
└ Pricing Engine                  [===]
└ Database                        [==]
└ Admin Basic                        [======]
└ Web Portal                              [======]

Phase 2: Mobile & Integrations
└ Mobile App                                    [========]
└ Nearby Stock                                         [==]
└ Payment                                               [===]
└ Logistics                                                [===]
└ Chat                                                       [===]

Phase 3: Analytics & Polish
└ Notifications                                                 [===]
└ Analytics                                                       [====]
└ Documents                                                          [===]

Phase 4: Testing & Launch
└ Testing                                                             [====]
└ Deployment                                                              [==]
└ Docs                                                                     [==]
```

---

## 15. Risk Management & Mitigation

| Risk | Impact | Probability | Mitigation Strategy |
|------|--------|-------------|---------------------|
| **Technical Risks** |
| Third-party API downtime | High | Medium | Implement fallback mechanisms, cache responses |
| Database performance issues | High | Low | Optimize queries, implement caching, use indexes |
| Scalability challenges | High | Medium | Design for horizontal scaling from day 1 |
| Security vulnerabilities | Critical | Medium | Regular security audits, follow best practices |
| **Business Risks** |
| Scope creep | Medium | High | Strict change management process, phased approach |
| Budget overrun | High | Medium | 10% contingency buffer, regular cost tracking |
| Timeline delays | Medium | Medium | Buffer time in schedule, parallel development |
| **Operational Risks** |
| User adoption issues | High | Medium | Comprehensive training, intuitive UI/UX |
| Data migration challenges | Medium | Low | Thorough testing, rollback plan |
| Integration failures | Medium | Medium | Sandbox testing, error handling, monitoring |

---

## 16. Success Metrics & KPIs

### Platform Health Metrics
- **Uptime**: Target 99.9%
- **API Response Time**: < 500ms (p95)
- **Error Rate**: < 0.1%
- **Active Users**: Track DAU, MAU

### Business Metrics
- **Order Fulfillment Time**: Target < 4 hours
- **Order Completion Rate**: > 95%
- **User Satisfaction**: NPS > 50
- **Transaction Value**: Monthly GMV tracking

### Technical Metrics
- **Code Coverage**: > 80%
- **Build Success Rate**: > 95%
- **Deployment Frequency**: Weekly
- **Mean Time to Recovery**: < 1 hour

---

## 17. Post-Launch Support Plan

### Support Levels

**Level 1: Immediate (0-3 months)**
- 24/7 monitoring
- Critical bug fixes within 4 hours
- Daily health checks
- Weekly performance reports

**Level 2: Standard (3-6 months)**
- Business hours support (9 AM - 6 PM)
- Bug fixes within 24 hours
- Bi-weekly performance reports
- Monthly feature updates

**Level 3: Maintenance (6+ months)**
- Email/ticket support
- Bug fixes within 48 hours
- Monthly reports
- Quarterly feature updates

---

## 18. Handover Checklist

### Code & Documentation
- ✓ Complete source code (Frontend, Backend, Mobile)
- ✓ Database schemas and migration scripts
- ✓ API documentation (Swagger/Postman)
- ✓ Architecture diagrams
- ✓ Setup and deployment guides

### Access & Credentials
- ✓ Repository access (GitHub/GitLab)
- ✓ Cloud provider accounts
- ✓ Database credentials
- ✓ Third-party service API keys
- ✓ Domain and SSL certificates

### Operations
- ✓ Monitoring dashboard access
- ✓ CI/CD pipeline documentation
- ✓ Backup and recovery procedures
- ✓ Incident response plan
- ✓ User training materials

---

## 19. Conclusion

This architecture document provides a comprehensive blueprint for building a scalable, secure, and efficient construction materials supply chain platform. The modular design ensures:

1. **Flexibility**: Easy to add new features and integrations
2. **Scalability**: Handles growth from 100 to 100,000+ users
3. **Security**: Multi-layered security with industry best practices
4. **Performance**: Optimized for fast response times
5. **Maintainability**: Clean code structure and documentation
6. **Cost-effectiveness**: Phased approach allows budget control

The platform will revolutionize the construction materials market by:
- **Reducing order fulfillment time from days to hours**
- **Providing complete transparency in pricing and availability**
- **Smart pricing that makes app purchases cheaper than offline** (KEY DIFFERENTIATOR)
- **Empowering all stakeholders with data-driven insights**
- **Creating a sustainable digital ecosystem where everyone benefits**

### Key Differentiators:

**1. Smart Pricing System**
- Tier-based pricing rewards loyal, high-volume customers
- Automatic price comparison across locations
- Always cheaper than offline purchases
- Creates strong incentive for platform adoption

**2. Transparent Supply Chain**
- Clear product ownership (Manufacturer → Distributor → Dealer)
- Real-time inventory visibility
- Nearest stock point identification
- Estimated delivery times

**3. Complete Ecosystem**
- Connects all stakeholders on one platform
- Role-based views with reusable architecture
- Integrated logistics and payments
- End-to-end transaction management

**Recommended Approach**: 

**For ₹10 Lakhs Budget (34 weeks):**
- Launch with MVP that includes the critical Smart Pricing feature
- This is the minimum viable product that can compete in the market
- Focus on core user experience and pricing advantage
- Add advanced features in Phase 2 based on user feedback

**For ₹16 Lakhs Budget (50.5 weeks):**
- Complete platform with all advanced features
- Multiple integrations (payment, logistics)
- Comprehensive analytics and reporting
- Full reward system integration
- Advanced credit management

**Success Metrics to Track:**
1. **User Adoption**: Target 100+ businesses in first 3 months
2. **Transaction Volume**: ₹50 lakhs+ GMV in first quarter
3. **Cost Savings**: Prove 10-15% savings vs offline purchases
4. **Delivery Speed**: Achieve 80%+ orders delivered within 4 hours
5. **User Satisfaction**: NPS score > 50

**Next Steps**:
1. **Week 1-2**: Finalize budget and detailed requirements
2. **Week 3-4**: Complete UI/UX design and get stakeholder approval
3. **Week 5**: Set up development environment and CI/CD
4. **Week 6+**: Begin development with bi-weekly sprint reviews
5. **Ongoing**: Regular stakeholder feedback sessions every 2 weeks

**Critical Success Factors:**
- **Pricing Strategy**: The Smart Pricing system MUST demonstrate clear value over offline
- **User Training**: Comprehensive training for all user types
- **Early Adoption**: Focus on getting 2-3 manufacturers + 5-10 distributors as launch partners
- **Performance**: System must be fast and reliable from day one
- **Support**: Responsive support during initial adoption phase

---
