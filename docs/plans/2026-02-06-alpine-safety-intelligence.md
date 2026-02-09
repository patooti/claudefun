# Alpine Safety Intelligence - Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a full-stack web platform where government and private organizations can dispatch drones to run alpine safety tests (snow surveys, visibility, avalanche risk, wind conditions) on mountains before summit attempts, displaying results on an interactive map dashboard.

**Architecture:** Next.js 15 App Router with shadcn/ui for the frontend, featuring a hero landing page with an interactive 3D globe (cobe) over an animated dotted particle background (three.js). The backend uses Next.js API routes serving mock data that simulates drone missions dispatching to mountain locations and returning safety test results. Authentication has a dev bypass button for demo access.

**Tech Stack:** Next.js 15, React 19, TypeScript, Tailwind CSS, shadcn/ui, cobe (globe), three.js (dotted surface), Leaflet (map), Outfit font, next-themes

---

## Task 1: Scaffold Next.js Project with shadcn/ui

**Files:**
- Create: `alpine-safety/` (entire project scaffold)
- Create: `alpine-safety/components.json`
- Create: `alpine-safety/src/lib/utils.ts`

**Step 1: Create Next.js project**

Run:
```bash
cd /Users/jackson/Desktop/claudeprojects && npx create-next-app@latest alpine-safety --typescript --tailwind --eslint --app --src-dir --import-alias "@/*" --yes
```

Expected: Project created at `alpine-safety/` with App Router structure.

**Step 2: Initialize shadcn/ui**

Run:
```bash
cd /Users/jackson/Desktop/claudeprojects/alpine-safety && npx shadcn@latest init -d
```

Expected: `components.json` created, `src/lib/utils.ts` created with `cn()` helper.

**Step 3: Install all project dependencies**

Run:
```bash
cd /Users/jackson/Desktop/claudeprojects/alpine-safety && npm install cobe three next-themes leaflet react-leaflet && npm install -D @types/three @types/leaflet
```

Expected: All deps installed without errors.

**Step 4: Add core shadcn components**

Run:
```bash
cd /Users/jackson/Desktop/claudeprojects/alpine-safety && npx shadcn@latest add button card input label badge separator sheet dialog tabs
```

Expected: Components added to `src/components/ui/`.

**Step 5: Initialize git and commit**

Run:
```bash
cd /Users/jackson/Desktop/claudeprojects/alpine-safety && git init && git add -A && git commit -m "feat: scaffold Next.js project with shadcn/ui and dependencies"
```

---

## Task 2: Configure Theme, Fonts, and Color Palette

**Files:**
- Modify: `alpine-safety/src/app/layout.tsx`
- Modify: `alpine-safety/src/app/globals.css`
- Create: `alpine-safety/src/components/theme-provider.tsx`

**Step 1: Create ThemeProvider component**

Create `alpine-safety/src/components/theme-provider.tsx`:
```tsx
"use client"

import * as React from "react"
import { ThemeProvider as NextThemesProvider } from "next-themes"

export function ThemeProvider({
  children,
  ...props
}: React.ComponentProps<typeof NextThemesProvider>) {
  return <NextThemesProvider {...props}>{children}</NextThemesProvider>
}
```

**Step 2: Update globals.css with alpine color palette and Outfit font**

Replace `alpine-safety/src/app/globals.css` with:
```css
@import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;500;600;700;800&display=swap');

@import "tailwindcss";
@plugin "tailwindcss-animate";

@custom-variant dark (&:is(.dark *));

:root {
  --font-outfit: 'Outfit', sans-serif;

  --background: 220 20% 97%;
  --foreground: 220 20% 10%;
  --card: 0 0% 100%;
  --card-foreground: 220 20% 10%;
  --popover: 0 0% 100%;
  --popover-foreground: 220 20% 10%;
  --primary: 210 80% 45%;
  --primary-foreground: 0 0% 100%;
  --secondary: 195 60% 90%;
  --secondary-foreground: 210 80% 25%;
  --muted: 210 15% 92%;
  --muted-foreground: 210 10% 45%;
  --accent: 35 90% 55%;
  --accent-foreground: 35 90% 15%;
  --destructive: 0 72% 51%;
  --destructive-foreground: 0 0% 100%;
  --border: 210 15% 85%;
  --input: 210 15% 85%;
  --ring: 210 80% 45%;
  --radius: 0.625rem;

  /* Alpine custom tokens */
  --alpine-snow: 210 30% 96%;
  --alpine-ice: 195 70% 85%;
  --alpine-sky: 210 80% 55%;
  --alpine-peak: 220 25% 20%;
  --alpine-warning: 35 90% 55%;
  --alpine-danger: 0 72% 51%;
  --alpine-safe: 145 60% 42%;
}

.dark {
  --background: 220 25% 8%;
  --foreground: 210 20% 95%;
  --card: 220 25% 12%;
  --card-foreground: 210 20% 95%;
  --popover: 220 25% 12%;
  --popover-foreground: 210 20% 95%;
  --primary: 210 80% 55%;
  --primary-foreground: 0 0% 100%;
  --secondary: 210 30% 20%;
  --secondary-foreground: 210 20% 85%;
  --muted: 220 20% 18%;
  --muted-foreground: 210 15% 60%;
  --accent: 35 90% 55%;
  --accent-foreground: 35 90% 95%;
  --destructive: 0 72% 51%;
  --destructive-foreground: 0 0% 100%;
  --border: 220 20% 22%;
  --input: 220 20% 22%;
  --ring: 210 80% 55%;

  --alpine-snow: 220 20% 18%;
  --alpine-ice: 195 40% 25%;
  --alpine-sky: 210 70% 50%;
  --alpine-peak: 210 20% 85%;
  --alpine-warning: 35 85% 50%;
  --alpine-danger: 0 72% 55%;
  --alpine-safe: 145 55% 45%;
}

@theme inline {
  --color-background: hsl(var(--background));
  --color-foreground: hsl(var(--foreground));
  --color-card: hsl(var(--card));
  --color-card-foreground: hsl(var(--card-foreground));
  --color-popover: hsl(var(--popover));
  --color-popover-foreground: hsl(var(--popover-foreground));
  --color-primary: hsl(var(--primary));
  --color-primary-foreground: hsl(var(--primary-foreground));
  --color-secondary: hsl(var(--secondary));
  --color-secondary-foreground: hsl(var(--secondary-foreground));
  --color-muted: hsl(var(--muted));
  --color-muted-foreground: hsl(var(--muted-foreground));
  --color-accent: hsl(var(--accent));
  --color-accent-foreground: hsl(var(--accent-foreground));
  --color-destructive: hsl(var(--destructive));
  --color-destructive-foreground: hsl(var(--destructive-foreground));
  --color-border: hsl(var(--border));
  --color-input: hsl(var(--input));
  --color-ring: hsl(var(--ring));
  --color-alpine-snow: hsl(var(--alpine-snow));
  --color-alpine-ice: hsl(var(--alpine-ice));
  --color-alpine-sky: hsl(var(--alpine-sky));
  --color-alpine-peak: hsl(var(--alpine-peak));
  --color-alpine-warning: hsl(var(--alpine-warning));
  --color-alpine-danger: hsl(var(--alpine-danger));
  --color-alpine-safe: hsl(var(--alpine-safe));
  --radius-sm: calc(var(--radius) - 4px);
  --radius-md: calc(var(--radius) - 2px);
  --radius-lg: var(--radius);
  --radius-xl: calc(var(--radius) + 4px);
  --font-sans: var(--font-outfit);
}

body {
  font-family: var(--font-outfit), sans-serif;
}
```

**Step 3: Update root layout with ThemeProvider and Outfit font**

Replace `alpine-safety/src/app/layout.tsx`:
```tsx
import type { Metadata } from "next";
import { ThemeProvider } from "@/components/theme-provider";
import "./globals.css";

export const metadata: Metadata = {
  title: "Alpine Safety Intelligence",
  description: "Drone-powered alpine safety testing platform for mountain research and summit preparation",
};

export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode;
}>) {
  return (
    <html lang="en" suppressHydrationWarning>
      <body className="antialiased">
        <ThemeProvider
          attribute="class"
          defaultTheme="dark"
          enableSystem
          disableTransitionOnChange
        >
          {children}
        </ThemeProvider>
      </body>
    </html>
  );
}
```

**Step 4: Verify dev server starts**

Run:
```bash
cd /Users/jackson/Desktop/claudeprojects/alpine-safety && npm run dev &
sleep 5 && curl -s -o /dev/null -w "%{http_code}" http://localhost:3000
```

Expected: `200`

Kill dev server after verification.

**Step 5: Commit**

Run:
```bash
cd /Users/jackson/Desktop/claudeprojects/alpine-safety && git add -A && git commit -m "feat: configure alpine theme, Outfit font, and dark mode support"
```

---

## Task 3: Add Globe and DottedSurface Components

**Files:**
- Create: `alpine-safety/src/components/ui/globe.tsx`
- Create: `alpine-safety/src/components/ui/dotted-surface.tsx`

**Step 1: Create the Globe component**

Create `alpine-safety/src/components/ui/globe.tsx` with the user-provided Globe component code (from cobe library). Update the markers to show famous mountain locations instead of cities:

```tsx
"use client"

import createGlobe, { COBEOptions } from "cobe"
import { useCallback, useEffect, useRef, useState } from "react"

import { cn } from "@/lib/utils"

const GLOBE_CONFIG: COBEOptions = {
  width: 800,
  height: 800,
  onRender: () => {},
  devicePixelRatio: 2,
  phi: 0,
  theta: 0.3,
  dark: 1,
  diffuse: 0.4,
  mapSamples: 16000,
  mapBrightness: 1.2,
  baseColor: [0.15, 0.2, 0.3],
  markerColor: [251 / 255, 100 / 255, 21 / 255],
  glowColor: [0.15, 0.2, 0.35],
  markers: [
    { location: [27.9881, 86.9250], size: 0.12 },   // Mt. Everest
    { location: [35.8825, 76.5133], size: 0.1 },     // K2
    { location: [28.5961, 83.8203], size: 0.08 },    // Annapurna
    { location: [45.8326, 6.8652], size: 0.07 },     // Mont Blanc
    { location: [-3.0674, 37.3556], size: 0.06 },    // Kilimanjaro
    { location: [63.0692, -151.0070], size: 0.09 },  // Denali
    { location: [-32.6532, -70.0109], size: 0.08 },  // Aconcagua
    { location: [43.3499, 42.4453], size: 0.06 },    // Mt. Elbrus
    { location: [-4.0784, 137.1585], size: 0.05 },   // Puncak Jaya
    { location: [-43.5950, 170.1418], size: 0.06 },  // Mt. Cook
  ],
}

export function Globe({
  className,
  config = GLOBE_CONFIG,
}: {
  className?: string
  config?: COBEOptions
}) {
  let phi = 0
  let width = 0
  const canvasRef = useRef<HTMLCanvasElement>(null)
  const pointerInteracting = useRef(null)
  const pointerInteractionMovement = useRef(0)
  const [r, setR] = useState(0)

  const updatePointerInteraction = (value: any) => {
    pointerInteracting.current = value
    if (canvasRef.current) {
      canvasRef.current.style.cursor = value ? "grabbing" : "grab"
    }
  }

  const updateMovement = (clientX: any) => {
    if (pointerInteracting.current !== null) {
      const delta = clientX - pointerInteracting.current
      pointerInteractionMovement.current = delta
      setR(delta / 200)
    }
  }

  const onRender = useCallback(
    (state: Record<string, any>) => {
      if (!pointerInteracting.current) phi += 0.005
      state.phi = phi + r
      state.width = width * 2
      state.height = width * 2
    },
    [r],
  )

  const onResize = () => {
    if (canvasRef.current) {
      width = canvasRef.current.offsetWidth
    }
  }

  useEffect(() => {
    window.addEventListener("resize", onResize)
    onResize()

    const globe = createGlobe(canvasRef.current!, {
      ...config,
      width: width * 2,
      height: width * 2,
      onRender,
    })

    setTimeout(() => (canvasRef.current!.style.opacity = "1"))
    return () => globe.destroy()
  }, [])

  return (
    <div
      className={cn(
        "absolute inset-0 mx-auto aspect-[1/1] w-full max-w-[600px]",
        className,
      )}
    >
      <canvas
        className={cn(
          "size-full opacity-0 transition-opacity duration-500 [contain:layout_paint_size]",
        )}
        ref={canvasRef}
        onPointerDown={(e) =>
          updatePointerInteraction(
            e.clientX - pointerInteractionMovement.current,
          )
        }
        onPointerUp={() => updatePointerInteraction(null)}
        onPointerOut={() => updatePointerInteraction(null)}
        onMouseMove={(e) => updateMovement(e.clientX)}
        onTouchMove={(e) =>
          e.touches[0] && updateMovement(e.touches[0].clientX)
        }
      />
    </div>
  )
}
```

**Step 2: Create the DottedSurface component**

Create `alpine-safety/src/components/ui/dotted-surface.tsx` with the user-provided DottedSurface component code (exact copy from user input).

**Step 3: Verify build compiles**

Run:
```bash
cd /Users/jackson/Desktop/claudeprojects/alpine-safety && npx next build 2>&1 | tail -5
```

Expected: Build succeeds or only has warnings (not errors).

**Step 4: Commit**

Run:
```bash
cd /Users/jackson/Desktop/claudeprojects/alpine-safety && git add -A && git commit -m "feat: add Globe and DottedSurface UI components"
```

---

## Task 4: Build Mock Data Layer and API Routes

> **Agent:** Use @backend-architect agent for this task.

**Files:**
- Create: `alpine-safety/src/lib/mock-data.ts`
- Create: `alpine-safety/src/lib/types.ts`
- Create: `alpine-safety/src/app/api/mountains/route.ts`
- Create: `alpine-safety/src/app/api/missions/route.ts`
- Create: `alpine-safety/src/app/api/missions/[id]/route.ts`
- Create: `alpine-safety/src/app/api/auth/login/route.ts`

**Step 1: Create shared types**

Create `alpine-safety/src/lib/types.ts`:
```ts
export type SafetyLevel = "safe" | "caution" | "warning" | "danger" | "critical"

export interface Mountain {
  id: string
  name: string
  location: { lat: number; lng: number }
  elevation: number // meters
  region: string
  country: string
  imageUrl?: string
}

export interface SafetyTest {
  id: string
  type: "snow_depth" | "visibility" | "wind_speed" | "avalanche_risk" | "temperature" | "ice_thickness" | "crevasse_detection" | "oxygen_level"
  label: string
  value: number
  unit: string
  level: SafetyLevel
  timestamp: string
}

export interface DroneMission {
  id: string
  mountainId: string
  mountainName: string
  status: "queued" | "in_progress" | "completed" | "failed"
  droneId: string
  altitude: number
  startedAt: string
  completedAt?: string
  tests: SafetyTest[]
  coordinates: { lat: number; lng: number }
}

export interface User {
  id: string
  email: string
  name: string
  role: "admin" | "researcher" | "viewer"
  organization: string
}
```

**Step 2: Create mock data generator**

Create `alpine-safety/src/lib/mock-data.ts`:
```ts
import { Mountain, DroneMission, SafetyTest, SafetyLevel, User } from "./types"

export const MOUNTAINS: Mountain[] = [
  { id: "everest", name: "Mt. Everest", location: { lat: 27.9881, lng: 86.9250 }, elevation: 8849, region: "Himalayas", country: "Nepal/China" },
  { id: "k2", name: "K2", location: { lat: 35.8825, lng: 76.5133 }, elevation: 8611, region: "Karakoram", country: "Pakistan/China" },
  { id: "annapurna", name: "Annapurna", location: { lat: 28.5961, lng: 83.8203 }, elevation: 8091, region: "Himalayas", country: "Nepal" },
  { id: "mont-blanc", name: "Mont Blanc", location: { lat: 45.8326, lng: 6.8652 }, elevation: 4809, region: "Alps", country: "France/Italy" },
  { id: "kilimanjaro", name: "Kilimanjaro", location: { lat: -3.0674, lng: 37.3556 }, elevation: 5895, region: "East Africa", country: "Tanzania" },
  { id: "denali", name: "Denali", location: { lat: 63.0692, lng: -151.0070 }, elevation: 6190, region: "Alaska Range", country: "USA" },
  { id: "aconcagua", name: "Aconcagua", location: { lat: -32.6532, lng: -70.0109 }, elevation: 6961, region: "Andes", country: "Argentina" },
  { id: "elbrus", name: "Mt. Elbrus", location: { lat: 43.3499, lng: 42.4453 }, elevation: 5642, region: "Caucasus", country: "Russia" },
  { id: "rainier", name: "Mt. Rainier", location: { lat: 46.8523, lng: -121.7603 }, elevation: 4392, region: "Cascades", country: "USA" },
  { id: "matterhorn", name: "Matterhorn", location: { lat: 45.9763, lng: 7.6586 }, elevation: 4478, region: "Alps", country: "Switzerland/Italy" },
]

function randomLevel(value: number, thresholds: number[]): SafetyLevel {
  if (value <= thresholds[0]) return "safe"
  if (value <= thresholds[1]) return "caution"
  if (value <= thresholds[2]) return "warning"
  if (value <= thresholds[3]) return "danger"
  return "critical"
}

function generateTest(type: SafetyTest["type"], timestamp: string): SafetyTest {
  const configs: Record<SafetyTest["type"], { label: string; min: number; max: number; unit: string; thresholds: number[] }> = {
    snow_depth: { label: "Snow Depth", min: 0, max: 500, unit: "cm", thresholds: [50, 150, 300, 400] },
    visibility: { label: "Visibility", min: 0, max: 20, unit: "km", thresholds: [15, 10, 5, 2] },
    wind_speed: { label: "Wind Speed", min: 0, max: 200, unit: "km/h", thresholds: [30, 60, 100, 150] },
    avalanche_risk: { label: "Avalanche Risk", min: 1, max: 5, unit: "level", thresholds: [1, 2, 3, 4] },
    temperature: { label: "Temperature", min: -60, max: 10, unit: "C", thresholds: [-10, -25, -40, -50] },
    ice_thickness: { label: "Ice Thickness", min: 0, max: 300, unit: "cm", thresholds: [30, 80, 150, 250] },
    crevasse_detection: { label: "Crevasse Detection", min: 0, max: 20, unit: "count", thresholds: [2, 5, 10, 15] },
    oxygen_level: { label: "Oxygen Level", min: 30, max: 100, unit: "%", thresholds: [80, 65, 50, 40] },
  }

  const cfg = configs[type]
  const value = Math.round((Math.random() * (cfg.max - cfg.min) + cfg.min) * 10) / 10

  // For visibility and oxygen, lower is worse (reverse thresholds)
  const isInverse = type === "visibility" || type === "oxygen_level"
  let level: SafetyLevel
  if (isInverse) {
    if (value >= cfg.thresholds[0]) level = "safe"
    else if (value >= cfg.thresholds[1]) level = "caution"
    else if (value >= cfg.thresholds[2]) level = "warning"
    else if (value >= cfg.thresholds[3]) level = "danger"
    else level = "critical"
  } else {
    level = randomLevel(value, cfg.thresholds)
  }

  return {
    id: `test-${type}-${Date.now()}-${Math.random().toString(36).slice(2, 7)}`,
    type,
    label: cfg.label,
    value,
    unit: cfg.unit,
    level,
    timestamp,
  }
}

const TEST_TYPES: SafetyTest["type"][] = [
  "snow_depth", "visibility", "wind_speed", "avalanche_risk",
  "temperature", "ice_thickness", "crevasse_detection", "oxygen_level",
]

export function generateMission(mountainId: string, status: DroneMission["status"] = "completed"): DroneMission {
  const mountain = MOUNTAINS.find(m => m.id === mountainId) || MOUNTAINS[0]
  const now = new Date()
  const startedAt = new Date(now.getTime() - Math.random() * 3600000 * 24).toISOString()
  const completedAt = status === "completed"
    ? new Date(new Date(startedAt).getTime() + Math.random() * 3600000).toISOString()
    : undefined

  return {
    id: `mission-${Date.now()}-${Math.random().toString(36).slice(2, 7)}`,
    mountainId: mountain.id,
    mountainName: mountain.name,
    status,
    droneId: `DRONE-${String(Math.floor(Math.random() * 999)).padStart(3, "0")}`,
    altitude: Math.round(mountain.elevation * (0.6 + Math.random() * 0.4)),
    startedAt,
    completedAt,
    tests: status === "completed"
      ? TEST_TYPES.map(t => generateTest(t, completedAt!))
      : [],
    coordinates: {
      lat: mountain.location.lat + (Math.random() - 0.5) * 0.05,
      lng: mountain.location.lng + (Math.random() - 0.5) * 0.05,
    },
  }
}

// Pre-generate a set of missions for the dashboard
export function generateMissionSet(): DroneMission[] {
  const missions: DroneMission[] = []
  for (const mountain of MOUNTAINS) {
    const count = Math.floor(Math.random() * 3) + 1
    for (let i = 0; i < count; i++) {
      const statuses: DroneMission["status"][] = ["completed", "completed", "in_progress", "queued"]
      const status = statuses[Math.floor(Math.random() * statuses.length)]
      missions.push(generateMission(mountain.id, status))
    }
  }
  return missions.sort((a, b) => new Date(b.startedAt).getTime() - new Date(a.startedAt).getTime())
}

export const DEV_USER: User = {
  id: "dev-user",
  email: "dev@alpinesafety.io",
  name: "Dev User",
  role: "admin",
  organization: "Alpine Safety Intelligence",
}
```

**Step 3: Create API route - GET /api/mountains**

Create `alpine-safety/src/app/api/mountains/route.ts`:
```ts
import { NextResponse } from "next/server"
import { MOUNTAINS } from "@/lib/mock-data"

export async function GET() {
  return NextResponse.json(MOUNTAINS)
}
```

**Step 4: Create API route - GET/POST /api/missions**

Create `alpine-safety/src/app/api/missions/route.ts`:
```ts
import { NextResponse } from "next/server"
import { generateMissionSet, generateMission } from "@/lib/mock-data"

// In-memory store (resets on server restart)
let missions = generateMissionSet()

export async function GET() {
  return NextResponse.json(missions)
}

export async function POST(request: Request) {
  const body = await request.json()
  const { mountainId } = body

  if (!mountainId) {
    return NextResponse.json({ error: "mountainId is required" }, { status: 400 })
  }

  const mission = generateMission(mountainId, "queued")
  missions.unshift(mission)

  // Simulate mission progressing: after 2s -> in_progress, after 5s -> completed with tests
  setTimeout(() => {
    const m = missions.find(x => x.id === mission.id)
    if (m) m.status = "in_progress"
  }, 2000)

  setTimeout(() => {
    const m = missions.find(x => x.id === mission.id)
    if (m) {
      const completed = generateMission(m.mountainId, "completed")
      m.status = "completed"
      m.completedAt = completed.completedAt
      m.tests = completed.tests
    }
  }, 5000)

  return NextResponse.json(mission, { status: 201 })
}
```

**Step 5: Create API route - GET /api/missions/[id]**

Create `alpine-safety/src/app/api/missions/[id]/route.ts`:
```ts
import { NextResponse } from "next/server"
import { generateMissionSet } from "@/lib/mock-data"

// Note: In production this would share state with the missions route.
// For the mock, we re-import. The POST mutations won't persist here,
// but GET by ID will work for pre-seeded data.

export async function GET(
  request: Request,
  { params }: { params: Promise<{ id: string }> }
) {
  const { id } = await params
  // For mock purposes, return a generated mission
  const missions = generateMissionSet()
  const mission = missions.find(m => m.id === id)

  if (!mission) {
    return NextResponse.json({ error: "Mission not found" }, { status: 404 })
  }

  return NextResponse.json(mission)
}
```

**Step 6: Create API route - POST /api/auth/login**

Create `alpine-safety/src/app/api/auth/login/route.ts`:
```ts
import { NextResponse } from "next/server"
import { DEV_USER } from "@/lib/mock-data"

export async function POST(request: Request) {
  const body = await request.json()
  const { email, password, devBypass } = body

  // Dev bypass - skip auth entirely
  if (devBypass) {
    return NextResponse.json({
      user: DEV_USER,
      token: "dev-token-alpine-safety",
    })
  }

  // Mock auth - accept any email with password "alpine123"
  if (email && password === "alpine123") {
    return NextResponse.json({
      user: {
        id: `user-${Date.now()}`,
        email,
        name: email.split("@")[0],
        role: "researcher",
        organization: "Research Institute",
      },
      token: `mock-token-${Date.now()}`,
    })
  }

  return NextResponse.json({ error: "Invalid credentials" }, { status: 401 })
}
```

**Step 7: Verify API routes work**

Run:
```bash
cd /Users/jackson/Desktop/claudeprojects/alpine-safety && npm run dev &
sleep 5
curl -s http://localhost:3000/api/mountains | head -c 200
echo ""
curl -s http://localhost:3000/api/missions | head -c 200
echo ""
curl -s -X POST http://localhost:3000/api/auth/login -H "Content-Type: application/json" -d '{"devBypass": true}' | head -c 200
```

Expected: JSON responses for each endpoint.

**Step 8: Commit**

Run:
```bash
cd /Users/jackson/Desktop/claudeprojects/alpine-safety && git add -A && git commit -m "feat: add mock data layer and API routes for mountains, missions, and auth"
```

---

## Task 5: Create Auth Context and Login Page

> **Agent:** Use @frontend-designer agent for the login page UI.

**Files:**
- Create: `alpine-safety/src/lib/auth-context.tsx`
- Create: `alpine-safety/src/app/login/page.tsx`

**Step 1: Create auth context**

Create `alpine-safety/src/lib/auth-context.tsx`:
```tsx
"use client"

import { createContext, useContext, useState, useCallback, ReactNode } from "react"
import { User } from "./types"

interface AuthState {
  user: User | null
  token: string | null
  isAuthenticated: boolean
  login: (email: string, password: string) => Promise<boolean>
  devLogin: () => Promise<void>
  logout: () => void
}

const AuthContext = createContext<AuthState | null>(null)

export function AuthProvider({ children }: { children: ReactNode }) {
  const [user, setUser] = useState<User | null>(null)
  const [token, setToken] = useState<string | null>(null)

  const login = useCallback(async (email: string, password: string): Promise<boolean> => {
    const res = await fetch("/api/auth/login", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ email, password }),
    })
    if (!res.ok) return false
    const data = await res.json()
    setUser(data.user)
    setToken(data.token)
    return true
  }, [])

  const devLogin = useCallback(async () => {
    const res = await fetch("/api/auth/login", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ devBypass: true }),
    })
    const data = await res.json()
    setUser(data.user)
    setToken(data.token)
  }, [])

  const logout = useCallback(() => {
    setUser(null)
    setToken(null)
  }, [])

  return (
    <AuthContext.Provider value={{ user, token, isAuthenticated: !!user, login, devLogin, logout }}>
      {children}
    </AuthContext.Provider>
  )
}

export function useAuth() {
  const ctx = useContext(AuthContext)
  if (!ctx) throw new Error("useAuth must be used within AuthProvider")
  return ctx
}
```

**Step 2: Create login page**

Create `alpine-safety/src/app/login/page.tsx`:
```tsx
"use client"

import { useState } from "react"
import { useRouter } from "next/navigation"
import { useAuth } from "@/lib/auth-context"
import { Button } from "@/components/ui/button"
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from "@/components/ui/card"
import { Input } from "@/components/ui/input"
import { Label } from "@/components/ui/label"
import { Separator } from "@/components/ui/separator"

export default function LoginPage() {
  const [email, setEmail] = useState("")
  const [password, setPassword] = useState("")
  const [error, setError] = useState("")
  const [loading, setLoading] = useState(false)
  const { login, devLogin } = useAuth()
  const router = useRouter()

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault()
    setError("")
    setLoading(true)
    const success = await login(email, password)
    setLoading(false)
    if (success) {
      router.push("/dashboard")
    } else {
      setError("Invalid credentials. Try password: alpine123")
    }
  }

  const handleDevBypass = async () => {
    setLoading(true)
    await devLogin()
    setLoading(false)
    router.push("/dashboard")
  }

  return (
    <div className="flex min-h-screen items-center justify-center bg-background p-4">
      <Card className="w-full max-w-md">
        <CardHeader className="text-center">
          <div className="mx-auto mb-4 flex h-12 w-12 items-center justify-center rounded-xl bg-primary/10">
            <svg className="h-6 w-6 text-primary" fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
              <path strokeLinecap="round" strokeLinejoin="round" d="M3 21l7.5-7.5m0 0L18 6m-7.5 7.5L3 6m7.5 7.5L18 21" />
            </svg>
          </div>
          <CardTitle className="text-2xl font-bold">Alpine Safety Intelligence</CardTitle>
          <CardDescription>Sign in to access the drone safety testing dashboard</CardDescription>
        </CardHeader>
        <CardContent>
          <form onSubmit={handleSubmit} className="space-y-4">
            <div className="space-y-2">
              <Label htmlFor="email">Email</Label>
              <Input
                id="email"
                type="email"
                placeholder="researcher@institute.org"
                value={email}
                onChange={(e) => setEmail(e.target.value)}
                required
              />
            </div>
            <div className="space-y-2">
              <Label htmlFor="password">Password</Label>
              <Input
                id="password"
                type="password"
                value={password}
                onChange={(e) => setPassword(e.target.value)}
                required
              />
            </div>
            {error && <p className="text-sm text-destructive">{error}</p>}
            <Button type="submit" className="w-full" disabled={loading}>
              {loading ? "Signing in..." : "Sign In"}
            </Button>
          </form>

          <div className="my-6">
            <Separator />
          </div>

          <Button
            variant="outline"
            className="w-full border-dashed border-alpine-warning text-alpine-warning hover:bg-alpine-warning/10"
            onClick={handleDevBypass}
            disabled={loading}
          >
            Dev Bypass (Skip Auth)
          </Button>
          <p className="mt-2 text-center text-xs text-muted-foreground">
            Bypass authentication for development and demo purposes
          </p>
        </CardContent>
      </Card>
    </div>
  )
}
```

**Step 3: Wrap app layout with AuthProvider**

Update `alpine-safety/src/app/layout.tsx` to include `AuthProvider` inside `ThemeProvider`:
```tsx
import { AuthProvider } from "@/lib/auth-context"
// ...existing imports...

// In the JSX, wrap children:
<ThemeProvider ...>
  <AuthProvider>
    {children}
  </AuthProvider>
</ThemeProvider>
```

**Step 4: Commit**

Run:
```bash
cd /Users/jackson/Desktop/claudeprojects/alpine-safety && git add -A && git commit -m "feat: add auth context, login page with dev bypass"
```

---

## Task 6: Build Hero Landing Page

> **Agent:** Use @frontend-designer agent for this task. The hero page is the most important visual element.

**Files:**
- Create: `alpine-safety/src/components/hero-section.tsx`
- Create: `alpine-safety/src/components/navbar.tsx`
- Create: `alpine-safety/src/components/features-section.tsx`
- Modify: `alpine-safety/src/app/page.tsx`

**Step 1: Create the Navbar**

Create `alpine-safety/src/components/navbar.tsx`:
```tsx
"use client"

import Link from "next/link"
import { Button } from "@/components/ui/button"

export function Navbar() {
  return (
    <nav className="fixed top-0 z-50 w-full border-b border-white/10 bg-background/60 backdrop-blur-xl">
      <div className="mx-auto flex h-16 max-w-7xl items-center justify-between px-6">
        <Link href="/" className="flex items-center gap-2">
          <div className="flex h-8 w-8 items-center justify-center rounded-lg bg-primary">
            <svg className="h-4 w-4 text-primary-foreground" fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2.5}>
              <path strokeLinecap="round" strokeLinejoin="round" d="M3 21l7.5-7.5m0 0L18 6m-7.5 7.5L3 6m7.5 7.5L18 21" />
            </svg>
          </div>
          <span className="text-lg font-semibold tracking-tight">Alpine Safety</span>
        </Link>
        <div className="flex items-center gap-4">
          <Link href="/login">
            <Button variant="ghost" size="sm">Sign In</Button>
          </Link>
          <Link href="/login">
            <Button size="sm">Get Started</Button>
          </Link>
        </div>
      </div>
    </nav>
  )
}
```

**Step 2: Create the HeroSection with Globe + DottedSurface**

Create `alpine-safety/src/components/hero-section.tsx`:
```tsx
"use client"

import { Globe } from "@/components/ui/globe"
import { DottedSurface } from "@/components/ui/dotted-surface"
import { Button } from "@/components/ui/button"
import Link from "next/link"

export function HeroSection() {
  return (
    <section className="relative flex min-h-screen items-center justify-center overflow-hidden">
      {/* Animated dotted particle background */}
      <DottedSurface className="opacity-30" />

      {/* Gradient overlays */}
      <div className="pointer-events-none absolute inset-0 bg-gradient-to-b from-background via-transparent to-background" />
      <div className="pointer-events-none absolute inset-0 bg-[radial-gradient(ellipse_at_center,transparent_20%,hsl(var(--background))_80%)]" />

      {/* Globe */}
      <div className="absolute inset-0 flex items-center justify-center">
        <div className="relative h-[600px] w-[600px] opacity-60">
          <Globe />
        </div>
      </div>

      {/* Content */}
      <div className="relative z-10 mx-auto max-w-4xl px-6 text-center">
        <div className="mb-6 inline-flex items-center gap-2 rounded-full border border-primary/20 bg-primary/5 px-4 py-1.5 text-sm text-primary">
          <span className="relative flex h-2 w-2">
            <span className="absolute inline-flex h-full w-full animate-ping rounded-full bg-primary opacity-75" />
            <span className="relative inline-flex h-2 w-2 rounded-full bg-primary" />
          </span>
          Drone Fleet Active
        </div>

        <h1 className="mb-6 text-5xl font-bold leading-tight tracking-tight md:text-7xl">
          Alpine Safety{" "}
          <span className="bg-gradient-to-r from-primary via-alpine-sky to-alpine-ice bg-clip-text text-transparent">
            Intelligence
          </span>
        </h1>

        <p className="mx-auto mb-10 max-w-2xl text-lg text-muted-foreground md:text-xl">
          Autonomous drone-powered safety assessments for alpine mountaineering.
          Snow surveys, avalanche risk, visibility testing, and summit readiness — all before anyone sets foot on the mountain.
        </p>

        <div className="flex flex-col items-center justify-center gap-4 sm:flex-row">
          <Link href="/login">
            <Button size="lg" className="px-8 text-base">
              Launch Dashboard
            </Button>
          </Link>
          <Link href="#features">
            <Button variant="outline" size="lg" className="px-8 text-base">
              Learn More
            </Button>
          </Link>
        </div>

        {/* Stats */}
        <div className="mt-16 grid grid-cols-3 gap-8 border-t border-border/50 pt-8">
          <div>
            <div className="text-3xl font-bold text-primary">10+</div>
            <div className="text-sm text-muted-foreground">Mountain Ranges</div>
          </div>
          <div>
            <div className="text-3xl font-bold text-primary">8</div>
            <div className="text-sm text-muted-foreground">Safety Tests</div>
          </div>
          <div>
            <div className="text-3xl font-bold text-primary">24/7</div>
            <div className="text-sm text-muted-foreground">Monitoring</div>
          </div>
        </div>
      </div>
    </section>
  )
}
```

**Step 3: Create the Features Section**

Create `alpine-safety/src/components/features-section.tsx`:
```tsx
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card"

const features = [
  {
    title: "Snow Depth Analysis",
    description: "Real-time snow depth measurements across multiple elevation bands using LiDAR-equipped drones.",
    icon: "M12 2L2 19h20L12 2zm0 4l6.5 11h-13L12 6z",
  },
  {
    title: "Avalanche Risk Detection",
    description: "AI-powered analysis of snowpack layers, slope angles, and recent precipitation to assess avalanche probability.",
    icon: "M13 10V3L4 14h7v7l9-11h-7z",
  },
  {
    title: "Visibility & Weather",
    description: "Continuous monitoring of visibility conditions, cloud cover, wind speed, and temperature at summit altitudes.",
    icon: "M15 12a3 3 0 11-6 0 3 3 0 016 0z M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z",
  },
  {
    title: "Crevasse Mapping",
    description: "Thermal imaging and ground-penetrating radar to detect hidden crevasses and ice bridges along climbing routes.",
    icon: "M9 20l-5.447-2.724A1 1 0 013 16.382V5.618a1 1 0 011.447-.894L9 7m0 13l6-3m-6 3V7m6 10l5.447 2.724A1 1 0 0021 18.382V7.618a1 1 0 00-.553-.894L15 4m0 13V4m0 0L9 7",
  },
  {
    title: "Oxygen Level Monitoring",
    description: "Atmospheric oxygen percentage readings at various altitudes to plan acclimatization schedules.",
    icon: "M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z",
  },
  {
    title: "Ice Thickness Scanning",
    description: "Ultrasonic sensors measure ice formation thickness on ridges and traverses for safe route planning.",
    icon: "M20 7l-8-4-8 4m16 0l-8 4m8-4v10l-8 4m0-10L4 7m8 4v10M4 7v10l8 4",
  },
]

export function FeaturesSection() {
  return (
    <section id="features" className="relative py-24">
      <div className="mx-auto max-w-7xl px-6">
        <div className="mb-16 text-center">
          <h2 className="mb-4 text-3xl font-bold md:text-4xl">Comprehensive Safety Testing</h2>
          <p className="mx-auto max-w-2xl text-muted-foreground">
            Our autonomous drone fleet runs every critical safety assessment so researchers and climbers don&apos;t have to risk human lives for preliminary data.
          </p>
        </div>
        <div className="grid gap-6 md:grid-cols-2 lg:grid-cols-3">
          {features.map((feature) => (
            <Card key={feature.title} className="border-border/50 bg-card/50 backdrop-blur-sm transition-colors hover:border-primary/30">
              <CardHeader>
                <div className="mb-2 flex h-10 w-10 items-center justify-center rounded-lg bg-primary/10">
                  <svg className="h-5 w-5 text-primary" fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={1.5}>
                    <path strokeLinecap="round" strokeLinejoin="round" d={feature.icon} />
                  </svg>
                </div>
                <CardTitle className="text-lg">{feature.title}</CardTitle>
              </CardHeader>
              <CardContent>
                <p className="text-sm text-muted-foreground">{feature.description}</p>
              </CardContent>
            </Card>
          ))}
        </div>
      </div>
    </section>
  )
}
```

**Step 4: Update the home page**

Replace `alpine-safety/src/app/page.tsx`:
```tsx
import { Navbar } from "@/components/navbar"
import { HeroSection } from "@/components/hero-section"
import { FeaturesSection } from "@/components/features-section"

export default function Home() {
  return (
    <main className="min-h-screen bg-background">
      <Navbar />
      <HeroSection />
      <FeaturesSection />
    </main>
  )
}
```

**Step 5: Verify page renders**

Run dev server and check http://localhost:3000 renders without errors.

**Step 6: Commit**

Run:
```bash
cd /Users/jackson/Desktop/claudeprojects/alpine-safety && git add -A && git commit -m "feat: build hero landing page with globe, dotted surface, and features section"
```

---

## Task 7: Build Dashboard Layout and Map View

> **Agent:** Use @frontend-designer agent for the dashboard UI and @backend-architect agent for the data fetching hooks.

**Files:**
- Create: `alpine-safety/src/app/dashboard/layout.tsx`
- Create: `alpine-safety/src/app/dashboard/page.tsx`
- Create: `alpine-safety/src/components/dashboard/sidebar.tsx`
- Create: `alpine-safety/src/components/dashboard/mission-map.tsx`
- Create: `alpine-safety/src/components/dashboard/stats-bar.tsx`
- Create: `alpine-safety/src/hooks/use-missions.ts`
- Create: `alpine-safety/src/hooks/use-mountains.ts`

**Step 1: Create data fetching hooks**

Create `alpine-safety/src/hooks/use-missions.ts`:
```ts
"use client"

import { useState, useEffect, useCallback } from "react"
import { DroneMission } from "@/lib/types"

export function useMissions() {
  const [missions, setMissions] = useState<DroneMission[]>([])
  const [loading, setLoading] = useState(true)

  const fetchMissions = useCallback(async () => {
    const res = await fetch("/api/missions")
    const data = await res.json()
    setMissions(data)
    setLoading(false)
  }, [])

  const dispatchMission = useCallback(async (mountainId: string) => {
    const res = await fetch("/api/missions", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ mountainId }),
    })
    const mission = await res.json()
    setMissions(prev => [mission, ...prev])
    return mission
  }, [])

  const refresh = useCallback(async () => {
    setLoading(true)
    await fetchMissions()
  }, [fetchMissions])

  useEffect(() => {
    fetchMissions()
    // Poll every 10s for status updates
    const interval = setInterval(fetchMissions, 10000)
    return () => clearInterval(interval)
  }, [fetchMissions])

  return { missions, loading, dispatchMission, refresh }
}
```

Create `alpine-safety/src/hooks/use-mountains.ts`:
```ts
"use client"

import { useState, useEffect } from "react"
import { Mountain } from "@/lib/types"

export function useMountains() {
  const [mountains, setMountains] = useState<Mountain[]>([])
  const [loading, setLoading] = useState(true)

  useEffect(() => {
    fetch("/api/mountains")
      .then(res => res.json())
      .then(data => {
        setMountains(data)
        setLoading(false)
      })
  }, [])

  return { mountains, loading }
}
```

**Step 2: Create Dashboard Sidebar**

Create `alpine-safety/src/components/dashboard/sidebar.tsx`:
```tsx
"use client"

import Link from "next/link"
import { useAuth } from "@/lib/auth-context"
import { Button } from "@/components/ui/button"
import { Badge } from "@/components/ui/badge"

const navItems = [
  { label: "Map Overview", href: "/dashboard", icon: "M9 20l-5.447-2.724A1 1 0 013 16.382V5.618a1 1 0 011.447-.894L9 7m0 13l6-3m-6 3V7m6 10l5.447 2.724A1 1 0 0021 18.382V7.618a1 1 0 00-.553-.894L15 4m0 13V4m0 0L9 7" },
  { label: "Missions", href: "/dashboard/missions", icon: "M12 19l9 2-9-18-9 18 9-2zm0 0v-8" },
  { label: "Mountains", href: "/dashboard/mountains", icon: "M3 21l7.5-7.5m0 0L18 6m-7.5 7.5L3 6m7.5 7.5L18 21" },
]

export function Sidebar() {
  const { user, logout } = useAuth()

  return (
    <aside className="flex h-screen w-64 flex-col border-r border-border bg-card">
      <div className="flex h-16 items-center gap-2 border-b border-border px-4">
        <div className="flex h-8 w-8 items-center justify-center rounded-lg bg-primary">
          <svg className="h-4 w-4 text-primary-foreground" fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2.5}>
            <path strokeLinecap="round" strokeLinejoin="round" d="M3 21l7.5-7.5m0 0L18 6m-7.5 7.5L3 6m7.5 7.5L18 21" />
          </svg>
        </div>
        <span className="text-sm font-semibold">Alpine Safety</span>
      </div>

      <nav className="flex-1 space-y-1 p-3">
        {navItems.map((item) => (
          <Link key={item.href} href={item.href}>
            <Button variant="ghost" className="w-full justify-start gap-3 text-sm">
              <svg className="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={1.5}>
                <path strokeLinecap="round" strokeLinejoin="round" d={item.icon} />
              </svg>
              {item.label}
            </Button>
          </Link>
        ))}
      </nav>

      <div className="border-t border-border p-4">
        {user && (
          <div className="mb-3 space-y-1">
            <p className="text-sm font-medium">{user.name}</p>
            <div className="flex items-center gap-2">
              <Badge variant="secondary" className="text-xs">{user.role}</Badge>
              <span className="text-xs text-muted-foreground">{user.organization}</span>
            </div>
          </div>
        )}
        <Button variant="ghost" size="sm" className="w-full" onClick={logout}>
          Sign Out
        </Button>
      </div>
    </aside>
  )
}
```

**Step 3: Create StatsBar component**

Create `alpine-safety/src/components/dashboard/stats-bar.tsx`:
```tsx
"use client"

import { Card, CardContent } from "@/components/ui/card"
import { DroneMission } from "@/lib/types"

interface StatsBarProps {
  missions: DroneMission[]
}

export function StatsBar({ missions }: StatsBarProps) {
  const completed = missions.filter(m => m.status === "completed").length
  const inProgress = missions.filter(m => m.status === "in_progress").length
  const queued = missions.filter(m => m.status === "queued").length

  const dangerTests = missions
    .flatMap(m => m.tests)
    .filter(t => t.level === "danger" || t.level === "critical").length

  const stats = [
    { label: "Total Missions", value: missions.length, color: "text-foreground" },
    { label: "Completed", value: completed, color: "text-alpine-safe" },
    { label: "In Progress", value: inProgress, color: "text-primary" },
    { label: "Queued", value: queued, color: "text-muted-foreground" },
    { label: "Danger Alerts", value: dangerTests, color: "text-alpine-danger" },
  ]

  return (
    <div className="grid grid-cols-5 gap-4">
      {stats.map(stat => (
        <Card key={stat.label} className="border-border/50">
          <CardContent className="p-4">
            <p className="text-xs text-muted-foreground">{stat.label}</p>
            <p className={`text-2xl font-bold ${stat.color}`}>{stat.value}</p>
          </CardContent>
        </Card>
      ))}
    </div>
  )
}
```

**Step 4: Create MissionMap component (Leaflet)**

Create `alpine-safety/src/components/dashboard/mission-map.tsx`:
```tsx
"use client"

import { useEffect, useRef, useState } from "react"
import { DroneMission } from "@/lib/types"
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card"
import { Badge } from "@/components/ui/badge"

interface MissionMapProps {
  missions: DroneMission[]
  onMissionSelect?: (mission: DroneMission) => void
}

const statusColors: Record<string, string> = {
  completed: "#22c55e",
  in_progress: "#3b82f6",
  queued: "#94a3b8",
  failed: "#ef4444",
}

const levelColors: Record<string, string> = {
  safe: "bg-alpine-safe text-white",
  caution: "bg-yellow-500 text-black",
  warning: "bg-alpine-warning text-black",
  danger: "bg-orange-600 text-white",
  critical: "bg-alpine-danger text-white",
}

export function MissionMap({ missions, onMissionSelect }: MissionMapProps) {
  const mapRef = useRef<HTMLDivElement>(null)
  const mapInstanceRef = useRef<any>(null)
  const [selectedMission, setSelectedMission] = useState<DroneMission | null>(null)

  useEffect(() => {
    if (!mapRef.current || mapInstanceRef.current) return

    // Dynamic import for Leaflet (SSR safe)
    import("leaflet").then((L) => {
      import("leaflet/dist/leaflet.css")

      const map = L.map(mapRef.current!, {
        center: [30, 60],
        zoom: 3,
        zoomControl: true,
        attributionControl: false,
      })

      L.tileLayer("https://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}{r}.png", {
        maxZoom: 19,
      }).addTo(map)

      mapInstanceRef.current = map

      // Add mission markers
      missions.forEach((mission) => {
        const color = statusColors[mission.status] || "#94a3b8"
        const icon = L.divIcon({
          className: "custom-marker",
          html: `<div style="width:14px;height:14px;border-radius:50%;background:${color};border:2px solid white;box-shadow:0 0 8px ${color}80;"></div>`,
          iconSize: [14, 14],
          iconAnchor: [7, 7],
        })

        const marker = L.marker([mission.coordinates.lat, mission.coordinates.lng], { icon })
          .addTo(map)
          .on("click", () => {
            setSelectedMission(mission)
            onMissionSelect?.(mission)
          })

        marker.bindTooltip(
          `<strong>${mission.mountainName}</strong><br/>
           Drone: ${mission.droneId}<br/>
           Status: ${mission.status}<br/>
           Alt: ${mission.altitude}m`,
          { className: "!bg-card !text-card-foreground !border-border !rounded-lg !text-xs" }
        )
      })
    })

    return () => {
      if (mapInstanceRef.current) {
        mapInstanceRef.current.remove()
        mapInstanceRef.current = null
      }
    }
  }, [missions, onMissionSelect])

  return (
    <div className="grid gap-4 lg:grid-cols-3">
      <div className="lg:col-span-2">
        <Card className="overflow-hidden border-border/50">
          <div ref={mapRef} className="h-[500px] w-full" />
        </Card>
      </div>

      <div>
        <Card className="border-border/50">
          <CardHeader>
            <CardTitle className="text-base">
              {selectedMission ? `Mission: ${selectedMission.droneId}` : "Select a Mission"}
            </CardTitle>
          </CardHeader>
          <CardContent>
            {selectedMission ? (
              <div className="space-y-4">
                <div>
                  <p className="text-sm text-muted-foreground">Mountain</p>
                  <p className="font-medium">{selectedMission.mountainName}</p>
                </div>
                <div>
                  <p className="text-sm text-muted-foreground">Altitude</p>
                  <p className="font-medium">{selectedMission.altitude}m</p>
                </div>
                <div>
                  <p className="text-sm text-muted-foreground">Status</p>
                  <Badge variant="secondary">{selectedMission.status}</Badge>
                </div>
                {selectedMission.tests.length > 0 && (
                  <div>
                    <p className="mb-2 text-sm text-muted-foreground">Safety Tests</p>
                    <div className="space-y-2">
                      {selectedMission.tests.map(test => (
                        <div key={test.id} className="flex items-center justify-between rounded-md border border-border/50 px-3 py-2 text-sm">
                          <span>{test.label}</span>
                          <div className="flex items-center gap-2">
                            <span className="font-mono">{test.value} {test.unit}</span>
                            <Badge className={`text-xs ${levelColors[test.level]}`}>
                              {test.level}
                            </Badge>
                          </div>
                        </div>
                      ))}
                    </div>
                  </div>
                )}
              </div>
            ) : (
              <p className="text-sm text-muted-foreground">
                Click a marker on the map to view mission details and safety test results.
              </p>
            )}
          </CardContent>
        </Card>
      </div>
    </div>
  )
}
```

**Step 5: Create Dashboard Layout**

Create `alpine-safety/src/app/dashboard/layout.tsx`:
```tsx
"use client"

import { useAuth } from "@/lib/auth-context"
import { useRouter } from "next/navigation"
import { useEffect } from "react"
import { Sidebar } from "@/components/dashboard/sidebar"

export default function DashboardLayout({ children }: { children: React.ReactNode }) {
  const { isAuthenticated } = useAuth()
  const router = useRouter()

  useEffect(() => {
    if (!isAuthenticated) {
      router.push("/login")
    }
  }, [isAuthenticated, router])

  if (!isAuthenticated) return null

  return (
    <div className="flex h-screen bg-background">
      <Sidebar />
      <main className="flex-1 overflow-auto p-6">
        {children}
      </main>
    </div>
  )
}
```

**Step 6: Create Dashboard Page**

Create `alpine-safety/src/app/dashboard/page.tsx`:
```tsx
"use client"

import { useMissions } from "@/hooks/use-missions"
import { useMountains } from "@/hooks/use-mountains"
import { StatsBar } from "@/components/dashboard/stats-bar"
import { MissionMap } from "@/components/dashboard/mission-map"
import { Button } from "@/components/ui/button"
import {
  Dialog,
  DialogContent,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from "@/components/ui/dialog"
import { useState } from "react"

export default function DashboardPage() {
  const { missions, loading, dispatchMission, refresh } = useMissions()
  const { mountains } = useMountains()
  const [dispatching, setDispatching] = useState(false)

  const handleDispatch = async (mountainId: string) => {
    setDispatching(true)
    await dispatchMission(mountainId)
    setDispatching(false)
  }

  return (
    <div className="space-y-6">
      <div className="flex items-center justify-between">
        <div>
          <h1 className="text-2xl font-bold">Mission Control</h1>
          <p className="text-sm text-muted-foreground">Monitor drone safety missions across alpine regions</p>
        </div>
        <div className="flex gap-2">
          <Button variant="outline" size="sm" onClick={refresh} disabled={loading}>
            Refresh
          </Button>
          <Dialog>
            <DialogTrigger asChild>
              <Button size="sm">Dispatch Drone</Button>
            </DialogTrigger>
            <DialogContent>
              <DialogHeader>
                <DialogTitle>Dispatch Safety Drone</DialogTitle>
              </DialogHeader>
              <div className="grid gap-2 py-4">
                {mountains.map(mountain => (
                  <Button
                    key={mountain.id}
                    variant="outline"
                    className="justify-between"
                    disabled={dispatching}
                    onClick={() => handleDispatch(mountain.id)}
                  >
                    <span>{mountain.name}</span>
                    <span className="text-xs text-muted-foreground">{mountain.elevation}m — {mountain.country}</span>
                  </Button>
                ))}
              </div>
            </DialogContent>
          </Dialog>
        </div>
      </div>

      <StatsBar missions={missions} />

      {loading ? (
        <div className="flex h-[500px] items-center justify-center text-muted-foreground">
          Loading mission data...
        </div>
      ) : (
        <MissionMap missions={missions} />
      )}
    </div>
  )
}
```

**Step 7: Commit**

Run:
```bash
cd /Users/jackson/Desktop/claudeprojects/alpine-safety && git add -A && git commit -m "feat: build dashboard with interactive map, mission control, and stats"
```

---

## Task 8: Build Missions List and Mountain Detail Pages

**Files:**
- Create: `alpine-safety/src/app/dashboard/missions/page.tsx`
- Create: `alpine-safety/src/app/dashboard/mountains/page.tsx`

**Step 1: Create Missions list page**

Create `alpine-safety/src/app/dashboard/missions/page.tsx`:
```tsx
"use client"

import { useMissions } from "@/hooks/use-missions"
import { Card, CardContent } from "@/components/ui/card"
import { Badge } from "@/components/ui/badge"

const statusVariants: Record<string, string> = {
  completed: "bg-alpine-safe/10 text-alpine-safe border-alpine-safe/20",
  in_progress: "bg-primary/10 text-primary border-primary/20",
  queued: "bg-muted text-muted-foreground border-border",
  failed: "bg-alpine-danger/10 text-alpine-danger border-alpine-danger/20",
}

export default function MissionsPage() {
  const { missions, loading } = useMissions()

  if (loading) return <div className="text-muted-foreground">Loading...</div>

  return (
    <div className="space-y-6">
      <div>
        <h1 className="text-2xl font-bold">All Missions</h1>
        <p className="text-sm text-muted-foreground">{missions.length} missions tracked</p>
      </div>
      <div className="space-y-3">
        {missions.map(mission => (
          <Card key={mission.id} className="border-border/50 transition-colors hover:border-primary/30">
            <CardContent className="flex items-center justify-between p-4">
              <div className="flex items-center gap-4">
                <div>
                  <p className="font-medium">{mission.mountainName}</p>
                  <p className="text-xs text-muted-foreground">
                    {mission.droneId} &middot; {mission.altitude}m altitude
                  </p>
                </div>
              </div>
              <div className="flex items-center gap-3">
                {mission.tests.length > 0 && (
                  <span className="text-xs text-muted-foreground">
                    {mission.tests.length} tests
                  </span>
                )}
                <Badge className={statusVariants[mission.status]}>
                  {mission.status.replace("_", " ")}
                </Badge>
                <span className="text-xs text-muted-foreground">
                  {new Date(mission.startedAt).toLocaleDateString()}
                </span>
              </div>
            </CardContent>
          </Card>
        ))}
      </div>
    </div>
  )
}
```

**Step 2: Create Mountains list page**

Create `alpine-safety/src/app/dashboard/mountains/page.tsx`:
```tsx
"use client"

import { useMountains } from "@/hooks/use-mountains"
import { useMissions } from "@/hooks/use-missions"
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card"
import { Badge } from "@/components/ui/badge"
import { Button } from "@/components/ui/button"

export default function MountainsPage() {
  const { mountains, loading: mLoading } = useMountains()
  const { missions, dispatchMission } = useMissions()

  if (mLoading) return <div className="text-muted-foreground">Loading...</div>

  return (
    <div className="space-y-6">
      <div>
        <h1 className="text-2xl font-bold">Mountain Registry</h1>
        <p className="text-sm text-muted-foreground">Monitored mountain peaks and their mission history</p>
      </div>
      <div className="grid gap-4 md:grid-cols-2">
        {mountains.map(mountain => {
          const mountainMissions = missions.filter(m => m.mountainId === mountain.id)
          const latestMission = mountainMissions[0]
          return (
            <Card key={mountain.id} className="border-border/50">
              <CardHeader className="flex flex-row items-start justify-between pb-3">
                <div>
                  <CardTitle className="text-lg">{mountain.name}</CardTitle>
                  <p className="text-xs text-muted-foreground">{mountain.region} &middot; {mountain.country}</p>
                </div>
                <Badge variant="outline">{mountain.elevation}m</Badge>
              </CardHeader>
              <CardContent className="space-y-3">
                <div className="flex items-center justify-between text-sm">
                  <span className="text-muted-foreground">Missions</span>
                  <span className="font-medium">{mountainMissions.length}</span>
                </div>
                {latestMission && latestMission.tests.length > 0 && (
                  <div className="space-y-1">
                    <p className="text-xs text-muted-foreground">Latest Safety Summary</p>
                    <div className="flex flex-wrap gap-1">
                      {latestMission.tests.slice(0, 4).map(test => (
                        <Badge key={test.id} variant="secondary" className="text-xs">
                          {test.label}: {test.value}{test.unit}
                        </Badge>
                      ))}
                    </div>
                  </div>
                )}
                <Button
                  size="sm"
                  variant="outline"
                  className="w-full"
                  onClick={() => dispatchMission(mountain.id)}
                >
                  Dispatch Drone
                </Button>
              </CardContent>
            </Card>
          )
        })}
      </div>
    </div>
  )
}
```

**Step 3: Commit**

Run:
```bash
cd /Users/jackson/Desktop/claudeprojects/alpine-safety && git add -A && git commit -m "feat: add missions list and mountains registry pages"
```

---

## Task 9: Final Integration, Polish, and Verification

**Files:**
- Modify: Various files for final fixes
- Verify: Full application flow

**Step 1: Verify the full application flow**

Run:
```bash
cd /Users/jackson/Desktop/claudeprojects/alpine-safety && npm run build
```

Expected: Build succeeds.

**Step 2: Test the complete user journey**

1. Start dev server: `npm run dev`
2. Visit `http://localhost:3000` - See hero page with globe + dotted surface
3. Click "Launch Dashboard" or "Get Started" - Goes to `/login`
4. Click "Dev Bypass" button - Redirects to `/dashboard`
5. See stats bar + map with markers
6. Click a marker - See mission details + safety tests
7. Click "Dispatch Drone" - Select a mountain - See new mission appear
8. Navigate to Missions page - See all missions listed
9. Navigate to Mountains page - See all mountains with dispatch buttons

**Step 3: Fix any build or runtime errors found**

Address issues as discovered.

**Step 4: Final commit**

Run:
```bash
cd /Users/jackson/Desktop/claudeprojects/alpine-safety && git add -A && git commit -m "feat: final integration and polish for Alpine Safety Intelligence"
```

---

## Summary of File Structure

```
alpine-safety/
├── src/
│   ├── app/
│   │   ├── api/
│   │   │   ├── auth/login/route.ts
│   │   │   ├── missions/route.ts
│   │   │   ├── missions/[id]/route.ts
│   │   │   └── mountains/route.ts
│   │   ├── dashboard/
│   │   │   ├── layout.tsx
│   │   │   ├── page.tsx
│   │   │   ├── missions/page.tsx
│   │   │   └── mountains/page.tsx
│   │   ├── login/page.tsx
│   │   ├── globals.css
│   │   ├── layout.tsx
│   │   └── page.tsx
│   ├── components/
│   │   ├── dashboard/
│   │   │   ├── mission-map.tsx
│   │   │   ├── sidebar.tsx
│   │   │   └── stats-bar.tsx
│   │   ├── ui/  (shadcn + custom)
│   │   │   ├── globe.tsx
│   │   │   ├── dotted-surface.tsx
│   │   │   ├── button.tsx
│   │   │   ├── card.tsx
│   │   │   ├── input.tsx
│   │   │   ├── label.tsx
│   │   │   ├── badge.tsx
│   │   │   ├── separator.tsx
│   │   │   ├── dialog.tsx
│   │   │   ├── sheet.tsx
│   │   │   └── tabs.tsx
│   │   ├── features-section.tsx
│   │   ├── hero-section.tsx
│   │   ├── navbar.tsx
│   │   └── theme-provider.tsx
│   ├── hooks/
│   │   ├── use-missions.ts
│   │   └── use-mountains.ts
│   └── lib/
│       ├── auth-context.tsx
│       ├── mock-data.ts
│       ├── types.ts
│       └── utils.ts
├── components.json
├── package.json
├── tailwind.config.ts
├── tsconfig.json
└── next.config.ts
```

## Agent Delegation Plan

| Task | Agent | Responsibility |
|------|-------|---------------|
| Task 1 | Main session | Project scaffolding |
| Task 2 | Main session | Theme & configuration |
| Task 3 | Main session | Component integration |
| Task 4 | @backend-architect | API routes, mock data, types |
| Task 5 | @frontend-designer | Login page UI |
| Task 6 | @frontend-designer | Hero page (most important visual) |
| Task 7 | @frontend-designer + @backend-architect | Dashboard layout + data hooks |
| Task 8 | @frontend-designer | Sub-pages |
| Task 9 | Main session | Integration testing & polish |
