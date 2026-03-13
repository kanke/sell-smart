# Sell Smart — AI-Powered Resale Listings

> Built for the GDG London IWD 2026 Hackathon — "Build with AI"

## What it does

Upload photos of your second-hand items and Sell Smart instantly generates a complete, ready-to-post listing for you:

- **Title** — catchy and searchable
- **Description** — detailed and honest
- **Suggested resale price** (GBP)
- **Estimated RRP**
- **Tags** for discoverability
- **Category** matched to the platform

Supports **Vinted, eBay, Depop, and Facebook Marketplace** formatting.

### The smart part: automatic photo grouping

Upload 10 photos of 3 different items and Sell Smart figures out which photos belong to the same product. It does this by asking Gemini to extract a "product signature" (brand + model + type + colour) from each photo, then clusters photos with matching signatures together. Each cluster becomes one listing.

## Tech stack

| Layer | Tool |
|---|---|
| AI vision + text | **Google Gemini 1.5 Flash** |
| Frontend | Vanilla HTML/CSS/JS (single file, no build step) |
| Hosting | GitHub Pages / Vercel |

## How to run

### Option 1 — Open locally
Just open `index.html` in your browser. No server needed.

### Option 2 — Deploy to Vercel
1. Push this repo to GitHub
2. Go to [vercel.com](https://vercel.com) → Import Project → select your repo
3. Click Deploy — done

### Option 3 — GitHub Pages
1. Push to GitHub
2. Go to repo Settings → Pages → Source: main branch / root
3. Your app will be live at `https://yourusername.github.io/sell-smart`

## Getting a Gemini API key

1. Go to [aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey)
2. Click "Create API key"
3. It's free — the free tier is more than enough for this app

Paste your key into the app's API key field. It's stored in `sessionStorage` only (never sent anywhere except Google's API).

## How the photo grouping works

```
1. For each uploaded image:
   → Gemini analyses it and returns a JSON "product signature"
      { brand, model, productType, color, condition, productSignature }

2. Clustering algorithm:
   → Compare every pair of images
   → Two images are in the same cluster if:
      - They have the same productType, AND
      - Same brand OR ≥50% overlapping signature words

3. Each cluster → one Gemini API call with all cluster images
   → Returns the full listing JSON
```

## Project structure

```
sell-smart/
├── index.html     # Entire app — single file, no dependencies
└── README.md
```

## Built with

- Google Gemini 1.5 Flash (multimodal AI)
- Zero npm packages, zero build tools
- Designed for resellers on Vinted, eBay, Depop, Facebook Marketplace
