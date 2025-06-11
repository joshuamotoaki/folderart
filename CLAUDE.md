# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

**Development:**
```bash
npm run dev          # Start development server
npm run build        # Build for production
npm run start        # Start production server
```

**Quality Assurance (run these after making changes):**
```bash
npm run lint         # Run ESLint + TypeScript checking
npm run format       # Format code with Prettier
npm run test         # Run Node.js tests (watch mode)
```

## Architecture Overview

FolderArt is a Next.js 14 application that generates custom folder icons for macOS and Windows 11. The application uses canvas-based rendering to composite folder backgrounds, custom icons, and text in real-time.

**Core Flow:**
1. User configures folder settings through Configuration component
2. Changes trigger `useUpdatePreview` hook which calls `generatePreview`
3. `generatePreview` renders composite image on HTML canvas
4. User downloads canvas as PNG (macOS) or ICO (Windows via external API)

## Key Components Architecture

**Main Component Hierarchy:**
```
FolderEditor (main container + state management)
├── Configuration (sidebar controls)
└── Folder (canvas display)
    └── useUpdatePreview hook → generatePreview → Canvas rendering
```

**Critical Files:**
- `components/FolderEditor.tsx` - Main state management and drag-and-drop handling
- `hooks/useUpdatePreview.ts` - Canvas rendering lifecycle management  
- `utils/icons/generatePreview.ts` - Core canvas composition logic
- `utils/icons/createIcon.ts` - Icon processing and color adjustment
- `components/Configuration/Configuration.tsx` - UI controls and form inputs

## Canvas Rendering System

The canvas system (`utils/icons/`) handles:
- High-resolution (Retina) rendering with 2x scale factor
- Icon color adjustment to match folder themes
- Text rendering with proper positioning and shadows
- Folder background compositing for different OS styles

**Icon Processing Pipeline:**
1. Load icon image → `loadIconImg.ts`
2. Apply color constraints → `createConstraints.ts` 
3. Format and resize → `formatIcon.ts`
4. Composite onto folder → `generatePreview.ts`

## State Management

Configuration state flows through props from FolderEditor to child components. The `useUpdatePreview` hook manages canvas rendering side effects and provides loading states.

## File Export

- macOS: Direct canvas-to-PNG download via `canvasToPng.ts`
- Windows: Canvas-to-ICO conversion via external API (`canvasToIco.ts`)

## Asset Structure

- `public/folders/` - Folder background templates by OS
- `public/icons/` - Default icon set (GitHub, React, etc.)
- `components/Configuration/defaultIcons.ts` - Icon metadata and mappings