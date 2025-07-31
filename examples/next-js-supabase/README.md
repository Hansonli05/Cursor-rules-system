# Next.js + Supabase Example Configuration

This example shows how to configure the Cursor Rules System for a Next.js application with Supabase backend.

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
├── src/
│   ├── app/          # Next.js 13+ App Router
│   ├── components/
│   ├── lib/
│   └── types/
├── supabase/
└── package.json
```

## Memory Bank Configuration

### projectbrief.mdc
```markdown
# Project Brief

## Project Name
Learning Platform MVP

## Core Purpose
Create a social learning platform where users can share knowledge, collaborate on projects, and track their learning progress.

## Key Requirements
- User authentication and profiles
- Content creation and sharing
- Social features (comments, likes, follows)
- Progress tracking and analytics
- Real-time collaboration features

## Success Criteria
- 1000+ active users within 6 months
- Average session time > 15 minutes
- User retention rate > 40% after 30 days
```

### techContext.mdc
```markdown
# Tech Context

## Technology Stack
### Frontend
- **Framework**: Next.js 14 with App Router
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **State Management**: Zustand
- **Build Tool**: Next.js built-in

### Backend
- **Runtime**: Node.js (Serverless Functions)
- **Framework**: Next.js API Routes
- **Language**: TypeScript
- **API Style**: REST with some GraphQL

### Database
- **Primary Database**: Supabase (PostgreSQL)
- **Authentication**: Supabase Auth
- **File Storage**: Supabase Storage
- **Real-time**: Supabase Realtime

### Infrastructure & Deployment
- **Hosting**: Vercel
- **Database Hosting**: Supabase
- **CDN**: Vercel Edge Network
- **Monitoring**: Vercel Analytics + Supabase Dashboard
```

## Feature Rules Examples

### authentication-system.mdc
```markdown
# Authentication System

## Instructions

### Supabase Auth Integration
- Use Supabase client for all auth operations
- Handle auth state with custom React hook
- Implement Row Level Security (RLS) policies
- Use server-side auth validation for API routes

### Auth Flow Patterns
```typescript
// Auth hook pattern
export function useAuth() {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    const { data: { subscription } } = supabase.auth.onAuthStateChange(
      (event, session) => {
        setUser(session?.user ?? null);
        setLoading(false);
      }
    );
    
    return () => subscription.unsubscribe();
  }, []);
  
  return { user, loading };
}
```
```

### real-time-features.mdc
```markdown
# Real-time Features

## Instructions

### Supabase Realtime Setup
- Subscribe to database changes using Supabase Realtime
- Handle connection states and reconnection
- Optimize subscriptions to prevent unnecessary re-renders

### Real-time Patterns
```typescript
// Real-time subscription pattern
useEffect(() => {
  const channel = supabase
    .channel('posts')
    .on('postgres_changes', 
      { event: 'INSERT', schema: 'public', table: 'posts' },
      (payload) => {
        setPosts(prev => [payload.new, ...prev]);
      }
    )
    .subscribe();
    
  return () => {
    supabase.removeChannel(channel);
  };
}, []);
```
```

## Tech Stack Configuration

### next-js-patterns.mdc
```markdown
# Next.js Patterns

## App Router Structure
- Use server components by default
- Add 'use client' only when necessary
- Implement proper loading and error boundaries
- Use parallel routes for complex layouts

## API Route Patterns
```typescript
// API route with Supabase
import { createServerComponentClient } from '@supabase/auth-helpers-nextjs';
import { cookies } from 'next/headers';

export async function GET() {
  const supabase = createServerComponentClient({ cookies });
  
  const { data: { session } } = await supabase.auth.getSession();
  
  if (!session) {
    return new Response('Unauthorized', { status: 401 });
  }
  
  // API logic here
  
  return Response.json(data);
}
```
```

### supabase-patterns.mdc
```markdown
# Supabase Patterns

## Client Setup
- Separate clients for server/client components
- Use appropriate auth helpers for each context
- Implement proper TypeScript types from database

## Database Patterns
```typescript
// Type-safe database queries
export async function getPosts() {
  const supabase = createServerComponentClient({ cookies });
  
  const { data, error } = await supabase
    .from('posts')
    .select(`
      id,
      title,
      content,
      created_at,
      author:profiles(id, name, avatar_url)
    `)
    .order('created_at', { ascending: false });
    
  if (error) throw error;
  return data;
}
```
```

## MCP Configuration

### mcp.json
```json
{
  "mcpServers": {
    "supabase": {
      "command": "npx",
      "args": [
        "-y",
        "@supabase/mcp-server-supabase@latest",
        "--access-token",
        "${input:supabase_access_token}"
      ],
      "env": {
        "SUPABASE_URL": "${input:supabase_url}",
        "SUPABASE_ANON_KEY": "${input:supabase_anon_key}"
      }
    }
  },
  "inputs": [
    {
      "id": "supabase_access_token",
      "type": "promptString",
      "description": "Supabase Access Token",
      "password": true
    },
    {
      "id": "supabase_url",
      "type": "promptString",
      "description": "Supabase Project URL"
    },
    {
      "id": "supabase_anon_key",
      "type": "promptString",
      "description": "Supabase Anonymous Key",
      "password": true
    }
  ]
}
```

## Getting Started

1. **Copy the Configuration**
   ```bash
   cp -r cursor-rules-system/.cursor your-nextjs-project/
   ```

2. **Update Memory Bank Files**
   - Customize `memory-bank/projectbrief.mdc` with your project details
   - Update `memory-bank/techContext.mdc` with your specific stack
   - Modify other memory bank files as needed

3. **Configure MCP**
   - Update `mcp.json` with your Supabase credentials
   - Add any additional MCP servers you need

4. **Start Development**
   - Use `/plan` mode for significant features
   - Let the AI load context from memory bank
   - Update `activeContext.mdc` as you work

## Best Practices

- Keep memory bank files updated with project evolution
- Use feature rules for reusable patterns
- Document new patterns in tech stack files
- Regular reviews of rule effectiveness