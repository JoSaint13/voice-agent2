# Technical Specification

## 1. Technology Stack

### Frontend
- **Framework**: React 18 with TypeScript
- **Build Tool**: Vite 5.x
- **State Management**: Zustand
- **Routing**: React Router v6
- **UI Library**: Tailwind CSS + Shadcn/ui
- **Form Handling**: React Hook Form + Zod
- **Data Fetching**: TanStack Query v5

### Backend
- **Runtime**: Node.js 20 LTS
- **Framework**: Next.js 14 (App Router)
- **Language**: TypeScript
- **Database**: PostgreSQL 15 with Prisma ORM
- **Authentication**: NextAuth.js with JWT
- **API Type**: tRPC for end-to-end type safety

### Infrastructure & DevOps
- **Hosting**: Vercel for frontend and backend
- **Database Hosting**: Supabase
- **File Storage**: AWS S3
- **CI/CD**: GitHub Actions
- **Monitoring**: Sentry, Vercel Analytics
- **Analytics**: PostHog

## 2. System Architecture

### Architecture Pattern
Fullstack monolith with Next.js App Router, leveraging server-side rendering and incremental static regeneration

### Component Diagram
```
┌─────────────────────────────────────────┐
│           User's Browser                │
│  ┌─────────────────────────────────┐   │
│  │   Next.js React Application     │   │
│  │   - Server Components           │   │
│  │   - Client Components           │   │
│  │   - API Routes                  │   │
│  └─────────────┬───────────────────┘   │
└────────────────┼───────────────────────┘
                 │ HTTPS
                 ▼
┌─────────────────────────────────────────┐
│         Server-Side Processing          │
│  ┌─────────────────────────────────┐   │
│  │   tRPC Procedures               │   │
│  │   - Authentication              │   │
│  │   - Business Logic              │   │
│  └─────────────┬───────────────────┘   │
└────────────────┼───────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────┐
│          Data Layer                     │
│  ┌──────────────┐  ┌──────────────┐   │
│  │  PostgreSQL  │  │  Redis Cache │   │
│  │  (Supabase)  │  │  (Sessions)  │   │
│  └──────────────┘  └──────────────┘   │
└─────────────────────────────────────────┘
```

## 3. Data Models & Database Schema

### User Model
```typescript
model User {
  id            String    @id @default(cuid())
  email         String    @unique
  name          String?
  emailVerified DateTime?
  image         String?
  accounts      Account[]
  sessions      Session[]
  voiceProfiles VoiceProfile[]
  meetings      Meeting[]
}

model VoiceProfile {
  id          String   @id @default(cuid())
  userId      String
  user        User     @relation(fields: [userId], references: [id])
  name        String
  language    String
  description String?
  isDefault   Boolean  @default(false)
  createdAt   DateTime @default(now())
}

model Meeting {
  id          String   @id @default(cuid())
  userId      String
  user        User     @relation(fields: [userId], references: [id])
  title       String
  transcript  String
  duration    Int
  startedAt   DateTime
  actionItems ActionItem[]
}

model ActionItem {
  id        String   @id @default(cuid())
  meetingId String
  meeting   Meeting  @relation(fields: [meetingId], references: [id])
  title     String
  status    ActionItemStatus @default(PENDING)
  dueDate   DateTime?
  createdAt DateTime @default(now())
}

enum ActionItemStatus {
  PENDING
  IN_PROGRESS
  COMPLETED
  CANCELLED
}
```

## 4. API Design (tRPC Procedures)

### Authentication Procedures
```typescript
export const authRouter = router({
  signup: publicProcedure
    .input(z.object({
      email: z.string().email(),
      password: z.string().min(8),
      name: z.string().optional()
    }))
    .mutation(async ({ input }) => {
      // User registration logic
    }),

  login: publicProcedure
    .input(z.object({
      email: z.string().email(),
      password: z.string()
    }))
    .mutation(async ({ input }) => {
      // Authentication logic
    })
})

export const meetingRouter = protectedProcedure
  .router({
    create: protectedProcedure
      .input(z.object({
        title: z.string(),
        transcript: z.string(),
        duration: z.number()
      }))
      .mutation(async ({ ctx, input }) => {
        // Create meeting logic
      }),

    getRecent: protectedProcedure
      .query(async ({ ctx }) => {
        // Fetch recent meetings for user
      })
  })
```

## 5. Security Implementation

### Authentication Flow
1. NextAuth.js with credential provider
2. JWT stored in secure, httpOnly cookie
3. Server-side session validation
4. Role-based access control

### Security Checklist
- [x] Input validation with Zod
- [x] CSRF protection
- [x] Rate limiting on auth endpoints
- [x] Secure password hashing (bcrypt)
- [x] HTTPS enforcement
- [x] Strict CSP headers
- [x] Regular dependency updates

## 6. Performance & Scalability

### Optimization Strategies
- Server-side rendering
- Incremental static regeneration
- Prisma query optimization
- Redis caching for sessions
- Efficient database indexing

### Performance Targets
- Lighthouse Score: > 90
- First Contentful Paint: < 1.8s
- Time to Interactive: < 2.5s
- Total Bundle Size: < 200KB

## 7. Deployment Pipeline

### CI/CD Workflow
1. Code push triggers GitHub Actions
2. Run linting and type checks
3. Execute unit and integration tests
4. Build production assets
5. Deploy to Vercel
6. Run smoke tests
7. Optional: Rollback on failure

## 8. MVP Development Roadmap

### Phase 1 (Weeks 1-2)
- Project setup
- Authentication system
- Basic UI components
- Database schema

### Phase 2 (Weeks 3-4)
- Voice profile creation
- Meeting transcription
- Action item extraction
- Initial integrations

### Phase 3 (Weeks 5-6)
- Refinement and polish
- Performance optimization
- Error handling
- User testing

## 9. Future Scalability Considerations
- Microservices architecture
- Distributed caching
- Horizontal database scaling
- Machine learning model improvements