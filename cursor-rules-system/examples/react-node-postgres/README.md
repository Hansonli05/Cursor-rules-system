# React + Node.js + PostgreSQL Example Configuration

This example shows how to configure the Cursor Rules System for a traditional React frontend with Node.js backend and PostgreSQL database.

## Project Structure
```
your-project/
├── .cursor/
│   ├── global.mdc
│   ├── cursor-memory-bank.mdc
│   ├── memory-bank/
│   ├── feature-rules/
│   ├── tech-stack/
│   └── mcp.json
├── frontend/
│   ├── src/
│   ├── public/
│   └── package.json
├── backend/
│   ├── src/
│   ├── migrations/
│   └── package.json
└── docker-compose.yml
```

## Memory Bank Configuration

### projectbrief.mdc
```markdown
# Project Brief

## Project Name
E-commerce Dashboard

## Core Purpose
Internal dashboard for managing e-commerce operations including inventory, orders, customers, and analytics.

## Key Requirements
- User authentication and role-based access
- Inventory management system
- Order processing workflow
- Customer relationship management
- Real-time analytics and reporting
- Bulk data operations

## Success Criteria
- Process 1000+ orders per day efficiently
- Reduce order processing time by 50%
- Support 50+ concurrent admin users
- 99.9% uptime during business hours
```

### techContext.mdc
```markdown
# Tech Context

## Technology Stack
### Frontend
- **Framework**: React 18 with TypeScript
- **State Management**: Redux Toolkit + RTK Query
- **Styling**: Material-UI + Emotion
- **Build Tool**: Vite
- **Testing**: Jest + React Testing Library

### Backend
- **Runtime**: Node.js 18+
- **Framework**: Express.js
- **Language**: TypeScript
- **API Style**: REST with OpenAPI documentation
- **ORM**: Prisma

### Database
- **Primary Database**: PostgreSQL 15
- **Caching**: Redis 7
- **Search**: Elasticsearch 8
- **Queue**: Bull (Redis-based)

### Infrastructure & Deployment
- **Frontend Hosting**: Nginx + Docker
- **Backend Hosting**: PM2 + Docker
- **Database**: PostgreSQL on dedicated server
- **Load Balancer**: Nginx
- **Monitoring**: Prometheus + Grafana
```

## Feature Rules Examples

### inventory-management.mdc
```markdown
# Inventory Management

## Instructions

### Data Model Patterns
- Track inventory levels with audit trails
- Implement optimistic locking for concurrent updates
- Support bulk operations with proper error handling
- Maintain inventory history for reporting

### API Patterns
```typescript
// Inventory update with optimistic locking
export async function updateInventory(
  productId: string,
  quantity: number,
  version: number
) {
  const result = await db.product.update({
    where: { 
      id: productId,
      version: version  // Optimistic locking
    },
    data: {
      quantity: {
        increment: quantity
      },
      version: {
        increment: 1
      },
      updatedAt: new Date()
    }
  });
  
  if (!result) {
    throw new ConflictError('Inventory was updated by another user');
  }
  
  // Create audit trail
  await createInventoryHistory({
    productId,
    quantityChange: quantity,
    newQuantity: result.quantity,
    userId: getCurrentUserId()
  });
  
  return result;
}
```
```

### order-processing.mdc
```markdown
# Order Processing

## Instructions

### Order State Machine
- Implement clear order status transitions
- Use database transactions for order updates
- Send notifications on status changes
- Handle payment processing integration

### Workflow Patterns
```typescript
// Order state transitions
enum OrderStatus {
  PENDING = 'pending',
  CONFIRMED = 'confirmed',
  PROCESSING = 'processing',
  SHIPPED = 'shipped',
  DELIVERED = 'delivered',
  CANCELLED = 'cancelled'
}

const allowedTransitions: Record<OrderStatus, OrderStatus[]> = {
  [OrderStatus.PENDING]: [OrderStatus.CONFIRMED, OrderStatus.CANCELLED],
  [OrderStatus.CONFIRMED]: [OrderStatus.PROCESSING, OrderStatus.CANCELLED],
  [OrderStatus.PROCESSING]: [OrderStatus.SHIPPED, OrderStatus.CANCELLED],
  [OrderStatus.SHIPPED]: [OrderStatus.DELIVERED],
  [OrderStatus.DELIVERED]: [],
  [OrderStatus.CANCELLED]: []
};

export async function updateOrderStatus(
  orderId: string, 
  newStatus: OrderStatus
) {
  return await db.$transaction(async (tx) => {
    const order = await tx.order.findUnique({ where: { id: orderId } });
    
    if (!order) {
      throw new NotFoundError('Order not found');
    }
    
    const allowedStatuses = allowedTransitions[order.status as OrderStatus];
    if (!allowedStatuses.includes(newStatus)) {
      throw new ValidationError(
        `Cannot transition from ${order.status} to ${newStatus}`
      );
    }
    
    const updatedOrder = await tx.order.update({
      where: { id: orderId },
      data: { status: newStatus, updatedAt: new Date() }
    });
    
    // Create status history
    await tx.orderStatusHistory.create({
      data: {
        orderId,
        fromStatus: order.status,
        toStatus: newStatus,
        createdAt: new Date()
      }
    });
    
    // Queue notification
    await notificationQueue.add('order-status-changed', {
      orderId,
      newStatus,
      customerEmail: order.customerEmail
    });
    
    return updatedOrder;
  });
}
```
```

## Tech Stack Configuration

### react-patterns.mdc
```markdown
# React Patterns

## Component Structure
- Use functional components with hooks
- Implement proper error boundaries
- Follow container/presentational pattern
- Use React.memo for expensive components

### State Management
```typescript
// Redux slice pattern
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';

export const fetchProducts = createAsyncThunk(
  'inventory/fetchProducts',
  async (filters: ProductFilters) => {
    const response = await api.get('/products', { params: filters });
    return response.data;
  }
);

const inventorySlice = createSlice({
  name: 'inventory',
  initialState: {
    products: [],
    status: 'idle',
    error: null
  },
  reducers: {
    productUpdated: (state, action) => {
      const index = state.products.findIndex(p => p.id === action.payload.id);
      if (index !== -1) {
        state.products[index] = action.payload;
      }
    }
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetchProducts.pending, (state) => {
        state.status = 'loading';
      })
      .addCase(fetchProducts.fulfilled, (state, action) => {
        state.status = 'succeeded';
        state.products = action.payload;
      })
      .addCase(fetchProducts.rejected, (state, action) => {
        state.status = 'failed';
        state.error = action.error.message;
      });
  }
});
```
```

### express-api-patterns.mdc
```markdown
# Express.js API Patterns

## API Structure
- Use router modules for different resources
- Implement consistent error handling middleware
- Add request validation with joi or zod
- Use proper HTTP status codes

### Route Patterns
```typescript
// Product routes with validation
import { Router } from 'express';
import { z } from 'zod';
import { validateRequest } from '../middleware/validation';
import { requireAuth, requireRole } from '../middleware/auth';

const router = Router();

const createProductSchema = z.object({
  body: z.object({
    name: z.string().min(1).max(255),
    description: z.string().optional(),
    price: z.number().positive(),
    categoryId: z.string().uuid()
  })
});

router.post('/products', 
  requireAuth,
  requireRole(['admin', 'manager']),
  validateRequest(createProductSchema),
  async (req, res, next) => {
    try {
      const product = await productService.createProduct(req.body, req.user.id);
      res.status(201).json(product);
    } catch (error) {
      next(error);
    }
  }
);

// Error handling middleware
export function errorHandler(err: Error, req: Request, res: Response, next: NextFunction) {
  if (err instanceof ValidationError) {
    return res.status(400).json({
      error: 'Validation Error',
      details: err.details
    });
  }
  
  if (err instanceof NotFoundError) {
    return res.status(404).json({
      error: 'Resource not found'
    });
  }
  
  console.error(err);
  res.status(500).json({
    error: 'Internal server error'
  });
}
```
```

### postgresql-patterns.mdc
```markdown
# PostgreSQL Patterns

## Database Design
- Use proper indexing strategies
- Implement foreign key constraints
- Use database-level defaults and constraints
- Design for query performance

### Migration Patterns
```sql
-- Migration: Add inventory tracking
CREATE TABLE inventory_history (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  product_id UUID NOT NULL REFERENCES products(id),
  quantity_change INTEGER NOT NULL,
  quantity_before INTEGER NOT NULL,
  quantity_after INTEGER NOT NULL,
  reason VARCHAR(100) NOT NULL,
  user_id UUID REFERENCES users(id),
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_inventory_history_product_id ON inventory_history(product_id);
CREATE INDEX idx_inventory_history_created_at ON inventory_history(created_at);

-- Add optimistic locking to products
ALTER TABLE products ADD COLUMN version INTEGER DEFAULT 1;
```

### Query Patterns
```typescript
// Complex query with joins and aggregations
export async function getInventoryReport(filters: InventoryFilters) {
  const query = `
    SELECT 
      p.id,
      p.name,
      p.sku,
      p.quantity,
      c.name as category_name,
      COALESCE(sales.total_sold, 0) as total_sold,
      COALESCE(sales.revenue, 0) as revenue
    FROM products p
    LEFT JOIN categories c ON p.category_id = c.id
    LEFT JOIN (
      SELECT 
        product_id,
        SUM(quantity) as total_sold,
        SUM(quantity * price) as revenue
      FROM order_items oi
      INNER JOIN orders o ON oi.order_id = o.id
      WHERE o.created_at >= $1 AND o.created_at <= $2
      GROUP BY product_id
    ) sales ON p.id = sales.product_id
    WHERE ($3::text IS NULL OR c.name = $3)
    ORDER BY p.name
  `;
  
  return await db.query(query, [
    filters.startDate,
    filters.endDate,
    filters.category
  ]);
}
```
```

## Development Workflow

### Database Setup
```bash
# Start services
docker-compose up -d postgres redis

# Run migrations
npm run migrate

# Seed test data
npm run seed
```

### Development Commands
```bash
# Frontend
cd frontend
npm run dev

# Backend
cd backend
npm run dev

# Run tests
npm run test

# Type checking
npm run type-check
```

## Best Practices

- Use database transactions for multi-step operations
- Implement proper error handling and logging
- Cache frequently accessed data
- Use connection pooling for database access
- Implement rate limiting for APIs
- Regular database maintenance and optimization