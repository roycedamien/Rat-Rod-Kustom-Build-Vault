# Rat Rod & Kustom Build Vault: Automotive Restoration Gallery & AI Classifier

An Ed Roth Kustom Kulture-styled automotive build gallery and workshop management system that ingests vehicle project photos via drag-and-drop, runs multimodal Gemini visual recognition to identify vehicle type, make, model, year, and restoration stage, auto-clusters photos into project timelines, provides rich user-fillable metadata fields, and enables aspect-ratio-controlled AI concept rendering.

## User Review & Critical Decisions

> [!IMPORTANT]
> The following key decisions were confirmed during the interactive Phase 1 clarification:
> - **Build Stage Workflow**: Structured around the 5 standard automotive restoration stages: `Teardown`, `Metalwork`, `Paint`, `Assembly`, and `Completed`.
> - **Upload Organization**: Automatic grouping into vehicle projects and restoration stages with an instant batch triage and review drawer.
> - **Visual Theme**: Big Daddy Ed Roth "Kustom Kulture" garage aesthetic—asphalt charcoal foundations, monster slime green and hot flame orange accents, vintage speed shop badges, pinstripe hairline dividers, and bold industrial typography.
> - **Multi-Craft Scope**: Robust recognition taxonomy accommodating cars, trucks, hot rods, vans, buses, boats, planes, motorcycles, and bespoke coachbuilds.
> - **Aspect Ratio & Concept Studio**: Integrated controls supporting all 8 aspect ratios (1:1, 2:3, 3:2, 3:4, 4:3, 9:16, 16:9, 21:9) for generating restoration concept renders and modification previews.

---

## 1. Overview & Core Concept

- **What It Does**:
  Provides custom builders, fabricators, restorers, and hobbyists with a high-octane workshop gallery. Users drag and drop photo batches from any restoration or build job. Gemini Vision instantly inspects each photo, identifying the craft type (car, truck, hot rod, van, boat, aircraft), estimated make, model, year, body style, detected parts/assemblies (e.g., bare chassis, block & heads, primer sheet metal, wiring harness), and pinpoints the restoration stage. Images are auto-grouped into project builds and staged chronologically. Users can freely edit metadata, filter and reorder stages, compare before/after shots, export build sheets, and generate concept modifications in custom aspect ratios.

- **Target Audience / Persona**:
  Professional automotive restorers, custom hot rod and chopper shops, vintage marine and aviation mechanics, and vehicle restoration enthusiasts cataloging hundreds of high-resolution in-progress photos.

- **Key Value**:
  Eliminates disorganized folders of unclassified garage photos by combining automated visual classification, restoration stage timelines, flexible custom metadata editing, and concept generation into a cohesive, expressive interface.

---

## 2. User Experience & Visual Design

### Key User Flows

1. **Drag-and-Drop Ingestion & Auto-Triage**:
   - The user drops one or dozens of vehicle photos onto the dynamic drag zone or clicks to browse.
   - A batch upload queue shows instant thumbnails, file sizes, and analysis progress.
   - Gemini server proxy analyzes each image for: craft type, make, model, year range, restoration stage (`Teardown`, `Metalwork`, `Paint`, `Assembly`, `Completed`), confidence score, dominant colors, and detected components.
   - Auto-clustering proposes either assigning images to an existing active build (e.g., `'69 Chevy C10 Shop Truck`) or creating a new build workspace.

2. **Vehicle Build Timeline & Stage Kanban/Grid**:
   - The user explores builds via a multi-view switch (Stage Timeline, Stage Kanban Columns, or High-Density Photo Masonry).
   - Each restoration stage displays photos chronologically with status indicators, part tags, notes, and timestamp metadata.
   - Drag-and-drop between stages allows fast manual reclassification if work transitions or requires adjustments.

3. **High-Resolution Inspection & Metadata Editor (Lightbox)**:
   - Clicking any photo opens a high-contrast inspection lightbox.
   - The side-inspector exposes user-fillable fields: Vehicle Title, Craft Type, Make, Model, Year, VIN/Serial #, Build Stage, Component/Subsystem (Engine, Suspension, Body, Interior, Electrical), Date Taken, Work Performed Log, Estimated Hours, Cost/Parts Incurred, and Custom Tags.
   - Changes save instantly to persistent client storage (IndexedDB/localStorage).

4. **Aspect Ratio & Kustom Concept Studio**:
   - An integrated concept generator allows fabricators to visualize the next stage of work (e.g., "Add custom ghost flames and lowered stance" or "Chopped top with dual chrome velocity stacks").
   - Dedicated aspect ratio selector with active pills/buttons: `1:1`, `2:3`, `3:2`, `3:4`, `4:3`, `9:16`, `16:9`, `21:9`.
   - Generates concept artwork directly into the project's concept stage for visual comparison.

5. **Filtering, Search & Build Sheet Export**:
   - Fast instant search across vehicle make, model, year, stage, parts, and custom notes.
   - Filter chips for craft types (Cars, Trucks, Vans, Boats, Planes, Custom/Bespoke).
   - One-click "Export Build Dossier" generates a structured JSON/printable summary sheet with all categorized stages and metadata.

### Visual Identity & Theme (Ed Roth Kustom Kulture)

- **Aesthetic Direction**:
  Big Daddy Ed Roth & 1960s Southern California hot rod culture: gritty asphalt backgrounds, bold hand-painted pinstriping accents, speed shop tachometer styling, metallic chrome framing, and monster garage energy while maintaining modern typographic precision, WCAG AA contrast, and zero AI-slop restraint.
- **Color Palette & Atmosphere**:
  - `Canvas Dark`: `#0F1115` (Deep asphalt charcoal)
  - `Surface Panels`: `#181B22` with `#262A34` subtle hairline borders
  - `Kustom Green (Accent 1)`: `#10B981` / `#22C55E` (High-visibility toxic slime green for verified status and primary actions)
  - `Flame Orange (Accent 2)`: `#F97316` / `#FB923C` (Hot rod flame orange for in-progress work and upload highlights)
  - `Chrome White / Silver`: `#F1F5F9` and `#94A3B8` (Crisp legible text and mechanical specs)
  - `Speed Purple / Magenta`: `#A855F7` (Wild custom kustom highlights and concept badges)
- **Typography & Hierarchy**:
  - Display Font: High-impact condensed geometric sans (`Impact`, `Montserrat ExtraBold`, or `Archivo Black`) for project headlines and stage numbers.
  - Body & Data Font: Ultra-legible technical mono/sans pairing (`JetBrains Mono` / `Inter`) for VINs, years, specifications, and metadata tables.
- **Micro-Interactions**:
  - Hover zoom on build photo tiles with subtle chrome rim glow.
  - Smooth stage transitions with spring animations.
  - Real-time batch progress bars with tachometer RPM visual styling.

---

## 3. Key Product Decisions & Trade-Offs

- **Decision 1: Server-Side Gemini API Proxy**:
  - *Chosen Approach*: Route all Gemini multimodal calls through Express backend endpoints (`/api/analyze-image` and `/api/generate-concept`) using `@google/genai` and `process.env.GEMINI_API_KEY`.
  - *Why*: Keeps API credentials secure on the server, allows robust image resizing and format parsing, and conforms strictly to AI Studio security rules.
  - *Alternatives Considered*: Client-side SDK (rejected due to API key exposure risk).

- **Decision 2: Hybrid Client Persistence (IndexedDB + LocalStorage)**:
  - *Chosen Approach*: Store project metadata, classification results, and image base64/blobs in IndexedDB with fallback to localStorage.
  - *Why*: Allows large multi-image galleries (dozens of high-res photos) to persist reliably across browser reloads without exceeding localStorage's 5MB quota.

- **Decision 3: Seed Pre-Populated Sample Builds**:
  - *Chosen Approach*: Provide 3 rich realistic seed builds on first launch:
    1. `1957 Chevrolet Bel Air Gasser` (Multi-stage: Teardown -> Metalwork -> Paint -> Assembly).
    2. `1968 Ford Bronco 4x4 Trail Rig` (Chassis & Drivetrain swap).
    3. `1940 Boeing Stearman Biplane` (Radial engine overhaul & wing fabric restoration).
  - *Why*: The app is immediately engaging and interactive even before the user uploads their own photos.

---

## 4. Technical Architecture & Data Strategy

### System Architecture Diagram

```
┌────────────────────────────────────────────────────────────────────────┐
│                        Frontend React Application                      │
│                                                                        │
│  ┌───────────────────────┐  ┌────────────────────────────────────────┐ │
│  │   Ed Roth Header &    │  │       Project Selector & Filter Bar     │ │
│  │   Speed Shop Branding │  │   (Cars / Trucks / Boats / Aircraft)   │ │
│  └───────────────────────┘  └────────────────────────────────────────┘ │
│                                                                        │
│  ┌───────────────────────────────────────────────────────────────────┐ │
│  │     Multi-File Drag & Drop Dropzone + Batch Upload Triage Bar    │ │
│  └───────────────────────────────────────────────────────────────────┘ │
│                                                                        │
│  ┌───────────────────────┐  ┌────────────────────────────────────────┐ │
│  │  5-Stage Kanban Board │  │  Photo Masonry Grid & Comparison View  │ │
│  │  [Teardown]           │  │  (Chronological timeline & before/after)│ │
│  │  [Metalwork]          │  │                                        │ │
│  │  [Paint]              │  │  ┌───────────────────────────────────┐  │ │
│  │  [Assembly]           │  │  │   Aspect-Ratio AI Concept Studio  │  │ │
│  │  [Completed]          │  │  │   (1:1, 16:9, 4:3, 21:9 renders)  │  │ │
│  └───────────────────────┘  │  └───────────────────────────────────┘  │ │
│                             └────────────────────────────────────────┘ │
│                                                                        │
│  ┌───────────────────────────────────────────────────────────────────┐ │
│  │      High-Resolution Lightbox + Full Metadata Field Editor        │ │
│  │      (Make, Model, Year, VIN, Stage, Parts, Hours, Notes)         │ │
│  └───────────────────────────────────────────────────────────────────┘ │
└──────────────────────────────────┬─────────────────────────────────────┘
                                   │ Fetch JSON / Multipart
                                   ▼
┌────────────────────────────────────────────────────────────────────────┐
│                     Node.js / Express Server API                       │
│                                                                        │
│  ┌──────────────────────────────────────────────────────────────────┐  │
│  │ POST /api/analyze-image (Multimodal Gemini Vision Model)         │  │
│  │ -> Detects craft type, make, model, year, restoration stage,     │  │
│  │    detected components, confidence, and recommended tags         │  │
│  └──────────────────────────────────────────────────────────────────┘  │
│                                                                        │
│  ┌──────────────────────────────────────────────────────────────────┐  │
│  │ POST /api/generate-concept (Gemini Image Generation Model)       │  │
│  │ -> Aspect ratio controls: 1:1, 2:3, 3:2, 3:4, 4:3, 9:16, 16:9,    │  │
│  │    and 21:9 for custom hot rod concepts and modification mockups │  │
│  └──────────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────┬─────────────────────────────────────┘
                                   │ @google/genai SDK
                                   ▼
┌────────────────────────────────────────────────────────────────────────┐
│                      Google Gemini Foundation APIs                     │
│               (gemini-3.8-flash for Multimodal Vision &                │
│                gemini-3.1-flash-lite-image / nano-banana for renders)  │
└────────────────────────────────────────────────────────────────────────┘
```

### Data Model & Entities

```typescript
export type CraftType = 'Car' | 'Truck' | 'Hot Rod' | 'Van' | 'Bus' | 'Boat' | 'Plane' | 'Motorcycle' | 'Other';

export type BuildStage = 'Teardown' | 'Metalwork' | 'Paint' | 'Assembly' | 'Completed';

export interface BuildImage {
  id: string;
  projectId: string;
  url: string; // base64 data url or seeded image url
  fileName: string;
  fileSize: number;
  uploadedAt: string;
  stage: BuildStage;
  
  // AI-Recognized & User-Fillable Fields
  craftType: CraftType;
  make: string;
  model: string;
  year: string;
  vinOrSerial?: string;
  component: string; // e.g. "Front Clip", "Engine Block", "Interior Dash"
  condition: string; // e.g. "Rusted Surface", "Fresh Primer", "Show Polished"
  dateTaken: string;
  notes: string;
  technician: string;
  hoursSpent?: number;
  partsReplaced?: string[];
  tags: string[];
  
  // AI Analysis Metadata
  aiConfidence?: number;
  aiRawSummary?: string;
  isAiClassified: boolean;
}

export interface VehicleProject {
  id: string;
  title: string;
  craftType: CraftType;
  make: string;
  model: string;
  year: string;
  vin?: string;
  ownerName?: string;
  targetCompletionDate?: string;
  budget?: number;
  createdAt: string;
  coverImageId?: string;
  imageCount: number;
}
```

### Interactive State & Component Mapping

1. **`App.tsx` (Top-level container)**:
   - Manages selected project, active view mode (`stages`, `timeline`, `grid`, `concept_studio`), upload triage queue, and lightbox state.
2. **`DropzoneTriage.tsx`**:
   - Full drag-and-drop listener across entire workspace with visual hot-rod flame drag indicators.
   - Dispatches batch files sequentially to `/api/analyze-image` with visual progress indicators.
3. **`StageKanbanView.tsx`**:
   - 5 columns representing `Teardown`, `Metalwork`, `Paint`, `Assembly`, and `Completed`.
   - Supports drag-and-drop between stages to update `stage` in real time.
4. **`MetadataLightbox.tsx`**:
   - Fullscreen modal with high-res zoom, stage switcher, and two-way editable metadata form with instant auto-save.
5. **`ConceptStudio.tsx`**:
   - Text prompt input, aspect ratio segmented buttons (`1:1`, `2:3`, `3:2`, `3:4`, `4:3`, `9:16`, `16:9`, `21:9`), and style presets (Chop Top, Rat Rod Patina, Chrome Supercharger, Flame Pinstripe, Lowrider Stance).
6. **`BuildDossierModal.tsx`**:
   - Printable and exportable build sheet with complete image ledger, part receipts, and stage progression.

---

## 5. Verification Plan

1. **Upload & Vision Analysis**: Drag and drop real vehicle photos and verify Gemini returns accurate craft type, make, model, year, and restoration stage.
2. **5-Stage Pipeline**: Verify photos correctly auto-populate under Teardown, Metalwork, Paint, Assembly, or Completed.
3. **User-Fillable Metadata**: Edit Make, Model, Year, VIN, Notes, and Hours; verify persistence upon page refresh.
4. **Aspect Ratio Concept Studio**: Generate hot rod concept renders with various aspect ratios (1:1, 16:9, 21:9, 3:4) and confirm correct rendering.
5. **Zero-Slop UI & Responsiveness**: Test at 1440px desktop down to mobile viewports with WCAG AA contrast and smooth interactions.
