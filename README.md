# Grandsquare — AR Shirt Commerce

A responsive fashion storefront starter with an admin product uploader, product catalog, sizing, wishlist/cart-ready UI, and an AR try-on entry point.

## Current capabilities
- Editorial Grandsquare storefront and responsive product cards
- Admin-style product upload form for shirt images and details
- Product catalog data stored locally for the demo; a Supabase adapter is documented for persistence
- Try-on preview stage with a camera/AR access prompt and fallback preview
- Size guide, color selection, wishlist and cart drawer UI
- Accessible, mobile-first design

## Important technical boundary
A single product photo cannot reliably produce a production-quality, cloth-simulating 3D garment. This starter accepts front/back/detail images and offers a visual preview workflow. True garment 3D needs multi-view imagery or a 3D asset (GLB/USDZ) plus a garment reconstruction pipeline; image-based overlay is not represented as physically accurate fit. Browser AR support varies by device, and iOS/Android support must be tested on real devices.

## Stack
- Vite + React + TypeScript
- Tailwind CSS
- Supabase Auth, Postgres, and Storage (optional; connect a dedicated Grandsquare project, not the Ramanujan Academy database)
- Three.js / model-viewer integration seam for GLB/USDZ assets
- WebXR / Quick Look / Scene Viewer paths as supported by device

## Run locally
```bash
npm install
npm run dev
```

## Supabase setup
Create a **separate Supabase project for Grandsquare**. Do not reuse the existing Ramanujan Academy project, which contains student and finance information.

Set environment variables:
```env
VITE_SUPABASE_URL=
VITE_SUPABASE_PUBLISHABLE_KEY=
```

Create a public `products` table with fields such as:
`id, slug, name, description, price, compare_at_price, sizes, colors, image_urls, model_glb_url, model_usdz_url, published, created_at`.

Enable RLS. Public users may read published products; only authenticated users with an explicit admin role may create/update/delete products. Store product imagery and models in Supabase Storage with deliberate bucket policies.

## Recommended image-to-3D publishing workflow
1. Upload front, back, side and detail photos; plain background and even lighting work best.
2. Create an image preview and product metadata.
3. Generate/reconstruct the garment mesh using a suitable external 3D/photogrammetry service or a garment-specific workflow.
4. Review and correct the resulting model, UV/materials, and scale.
5. Upload a verified GLB for in-browser 3D and USDZ for iOS Quick Look where required.
6. Enable the AR button only when a valid model is available; otherwise show an honest preview state.

## Deployment
Connect this repo to Vercel; set the environment variables above. This repository is a starter, not a claim that image-to-3D reconstruction or production-grade garment fit is already hosted.
