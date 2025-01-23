# Guide 2: Building Your First Chart

## Overview

In this guide, you’ll learn how to construct a basic chart using Recharts and any helper components you may have. We’ll walk through building a bar chart, adding axes, grids, and tooltips.

## Sample Data

Let’s say we have a dataset of monthly user counts across two platforms (desktop and mobile):

```tsx
const chartData = [
  { month: 'January', desktop: 186, mobile: 80 },
  { month: 'February', desktop: 305, mobile: 200 },
  { month: 'March', desktop: 237, mobile: 120 },
  { month: 'April', desktop: 73, mobile: 190 },
  { month: 'May', desktop: 209, mobile: 130 },
  { month: 'June', desktop: 214, mobile: 140 },
];
```

## Chart Config

We define a **chart config** object to separate presentation details (labels, icons, colors) from the data. For example:

```tsx
import { Monitor } from 'lucide-react';

type ChartConfig = Record<
  string,
  {
    label: string;
    icon?: React.ElementType;
    color?: string;
    theme?: {
      light: string;
      dark: string;
    };
  }
>;

const chartConfig = {
  desktop: {
    label: 'Desktop',
    icon: Monitor,
    color: '#2563eb',
  },
  mobile: {
    label: 'Mobile',
    color: '#60a5fa',
  },
} satisfies ChartConfig;
```

## Creating the Chart

Below is a minimal example of a bar chart using Recharts components and custom helper components (e.g. `ChartContainer`, `ChartTooltip`, etc.). Adapt this to your own setup.

```tsx
import React from 'react';
import { BarChart, Bar } from 'recharts';
// If you're using custom components:
import {
  ChartContainer,
  ChartTooltip,
  ChartTooltipContent,
} from '@/components/ui/chart';

// Data and config from above
const data = [
  { month: 'January', desktop: 186, mobile: 80 },
  // ...rest of data
];

const config = {
  desktop: { label: 'Desktop', color: '#2563eb' },
  mobile: { label: 'Mobile', color: '#60a5fa' },
};

export function MyChart() {
  return (
    <ChartContainer config={config} className="min-h-[200px] w-full">
      <BarChart data={data}>
        {/* Add your custom tooltip */}
        <ChartTooltip content={<ChartTooltipContent />} />
        <Bar dataKey="desktop" fill="var(--color-desktop)" radius={4} />
        <Bar dataKey="mobile" fill="var(--color-mobile)" radius={4} />
      </BarChart>
    </ChartContainer>
  );
}
```

> **Important**  
> When using a custom container like `ChartContainer`, make sure you set an explicit height (e.g. `min-h-[200px]`) or your chart may not be visible.

## Adding a Grid

1. Import `CartesianGrid` from Recharts.
2. Include `<CartesianGrid vertical={false} />` in your chart.

```tsx
import { BarChart, Bar, CartesianGrid } from 'recharts';

<BarChart data={data}>
  <CartesianGrid vertical={false} />
  <Bar dataKey="desktop" fill="var(--color-desktop)" radius={4} />
  <Bar dataKey="mobile" fill="var(--color-mobile)" radius={4} />
</BarChart>
```

## Adding an Axis

To add an X-axis, import `XAxis` and include it:

```tsx
import { BarChart, Bar, CartesianGrid, XAxis } from 'recharts';

<BarChart data={data}>
  <CartesianGrid vertical={false} />
  <XAxis
    dataKey="month"
    tickLine={false}
    tickMargin={10}
    axisLine={false}
    tickFormatter={(value) => value.slice(0, 3)}
  />
  <Bar dataKey="desktop" fill="var(--color-desktop)" radius={4} />
  <Bar dataKey="mobile" fill="var(--color-mobile)" radius={4} />
</BarChart>
```

## Adding Tooltips and Legends

See [Guide 3](#guide-3-advanced-features-theming-tooltips-legends-and-accessibility) for details on using the custom tooltip and legend components.
