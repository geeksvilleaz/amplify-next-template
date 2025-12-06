# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

AWS Amplify Gen 2 Next.js starter template using the App Router. Pre-configured with authentication (Cognito), GraphQL API (AppSync), and real-time database (DynamoDB).

## Development Commands

```bash
# Install dependencies
npm install

# Run development server
npm run dev

# Build for production
npm run build

# Start production server
npm start

# Lint code
npm run lint
```

## Architecture

### Backend Structure (amplify/)

The Amplify Gen 2 backend uses a code-first approach with TypeScript:

- **amplify/backend.ts**: Entry point that combines all backend resources using `defineBackend()`
- **amplify/auth/resource.ts**: Authentication configuration using `defineAuth()` - currently email-based login with Cognito
- **amplify/data/resource.ts**: GraphQL schema and data model definitions using `defineData()` with Amplify's data client

The `amplify/` directory has its own package.json and tsconfig.json, separate from the Next.js app.

### Data Layer

GraphQL schema is defined using Amplify's `a.schema()` builder pattern in `amplify/data/resource.ts`. Models are defined with `.model()` and authorization rules with `.authorization()`.

Default authorization mode is API key (expires in 30 days). The starter includes a basic Todo model with public API key access.

### Frontend Structure (app/)

Next.js 14 App Router with client components:

- **app/page.tsx**: Main page component (client component with "use client" directive)
- **app/layout.tsx**: Root layout with metadata
- Amplify is configured at the component level via `Amplify.configure(outputs)` where outputs come from `amplify_outputs.json`

### Amplify Client Usage

Data client is generated per-component:
```typescript
import { generateClient } from "aws-amplify/data";
import type { Schema } from "@/amplify/data/resource";
const client = generateClient<Schema>();
```

Real-time subscriptions use `observeQuery()` for live data updates.

## Path Aliases

The project uses `@/*` to reference root-level imports (configured in tsconfig.json).

## Deployment

Uses `amplify.yml` for AWS Amplify Hosting pipeline configuration. Backend deployment runs `npx ampx pipeline-deploy` before frontend build.

## Key Files

- **amplify_outputs.json**: Generated configuration file (gitignored) containing backend resource details
- **tsconfig.json**: Excludes the `amplify/` directory from Next.js compilation
