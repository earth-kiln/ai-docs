# Guide 1: Introduction and Setup

## Overview

This guide covers the basics of installing and setting up Recharts along with helper components (e.g., `ChartContainer`, `ChartTooltipContent`, etc.). These helpers provide a polished starting point for building beautiful charts in your React application.

## Prerequisites

- Basic familiarity with React
- Tailwind CSS or a similar styling setup (optional but recommended)
- Node.js environment

## Installation

If you haven’t installed [Recharts](https://recharts.org/) already, you can do so using your package manager of choice:

```bash
npm install recharts
```

**Optional**: If you’re using a starter kit that provides scaffolding (like `shadcn`), run:

```bash
npx shadcn@latest add chart
```

> **Note**  
> If you are using charts with **React 19** or **Next.js 15**, please ensure you check any specific Recharts compatibility notes from their documentation or from your framework’s official migration guide.

## Adding Colors

To get consistent theming across light and dark modes, define color variables in your CSS. For example (in `globals.css` or an equivalent):

```css
@layer base {
  :root {
    --chart-1: 12 76% 61%;
    --chart-2: 173 58% 39%;
    --chart-3: 197 37% 24%;
    --chart-4: 43 74% 66%;
    --chart-5: 27 87% 67%;
  }

  .dark {
    --chart-1: 220 70% 50%;
    --chart-2: 160 60% 45%;
    --chart-3: 30 80% 55%;
    --chart-4: 280 65% 60%;
    --chart-5: 340 75% 55%;
  }
}
```

## Creating `chart.tsx` (Optional)

If you want a convenient place to store helper components (e.g., `ChartContainer`, `ChartTooltip`, `ChartTooltipContent`, `ChartLegend`, `ChartLegendContent`), create a file named `chart.tsx` in your `components/ui` directory (or wherever you prefer). Copy and paste the relevant code from your chosen library or from the snippet provided by your scaffolding tool.

```tsx
"use client"
import { AreaChart, Area, XAxis } from "recharts"
import { ChartConfig, ChartContainer, ChartTooltip, ChartTooltipContent } from '@/components/ui/chart'

const data = [
  { month: "Jan", sales: 100, users: 200 },
  { month: "Feb", sales: 200, users: 300 },
  { month: "Mar", sales: 150, users: 250 }
]

const config = {
  sales: {
    label: "Sales",
    color: "hsl(var(--chart-1))"
  },
  users: {
    label: "Users",
    color: "hsl(var(--chart-2))"
  }
} satisfies ChartConfig

export default function Chart() {
  return (
    <div className="h-[300px]">
      <ChartContainer config={config}>
        <AreaChart data={data}>
          <XAxis dataKey="month" />
          <ChartTooltip content={<ChartTooltipContent />} />
          <Area dataKey="sales" stackId="stack" />
          <Area dataKey="users" stackId="stack" />
        </AreaChart>
      </ChartContainer>
    </div>
  )
}
```
