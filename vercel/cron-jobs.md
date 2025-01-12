# Cron Jobs Quickstart

Learn how to schedule cron jobs to run at specific times or intervals. Cron Jobs are available on all plans.

## Prerequisites

- A Vercel account
- A project with a Vercel Function

## Creating a Cron Job

This guide demonstrates how to create a cron job that executes every day at 5 am UTC.

### 1. Create a Function

First, create a function that will be executed by the cron job. The example below shows a simple function that returns the user's region.

#### Next.js App Router (`/app`)

```typescript
// app/api/hello/route.ts
export const dynamic = 'force-dynamic'; // static by default, unless reading the request

export function GET(request: Request) {
  return new Response(`Hello from ${process.env.VERCEL_REGION}`);
}
```

### 2. Configure Cron Settings

Create or update your `vercel.json` file with the following configuration:

```json
{
  "crons": [
    {
      "path": "/api/hello",
      "schedule": "0 5 * * *"
    }
  ]
}
```

The `crons` property accepts an array of cron job configurations. Each cron job requires:

- `path`: The endpoint to call (must start with `/`)
- `schedule`: A cron expression string specifying when to run the job
  - In this example: `0 5 * * *` runs every day at 5:00 am UTC

### Notes

- Cron jobs are only executed in production environments
- Deployments will automatically configure your cron jobs via CI/CD
- The function endpoint must be accessible via HTTP GET request
- The cron schedule uses UTC timezone

## Common Cron Patterns

| Pattern | Description |
|---------|-------------|
| `0 5 * * *` | Every day at 5am |
| `*/15 * * * *` | Every 15 minutes |
| `0 */2 * * *` | Every 2 hours |
| `0 0 * * *` | Every day at midnight |
| `0 0 * * MON` | Every Monday at midnight |
