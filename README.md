# Sell Smart — AI-Powered Resale Listings

> Built for the GDG London IWD 2026 Hackathon — "Build with AI"

This is Sell Smart...

Sell Smart helps people turn product photos into ready-to-post second-hand listings using AI.

A user uploads photos, the app automatically groups images of the same item, analyses the product, and generates a title, description, suggested resale price, tags, and category in seconds.

It also adapts the listing style for platforms like Vinted, eBay, Depop, and Facebook Marketplace, and gives a price confidence rating with a brief explanation.

So instead of manually writing every listing, users get polished, platform-ready resale content almost instantly.”


## What it does

Upload photos of your second-hand items and Sell Smart generates a complete, ready-to-post listing for you:

- **Title** — catchy and searchable
- **Description** — detailed and honest
- **Suggested resale price**
- **Estimated RRP**
- **Tags** for discoverability
- **Category** matched to the selected platform

Supports **Vinted, eBay, Depop, and Facebook Marketplace** formatting.

## The smart part: automatic photo grouping

Upload up to **10 photos** across multiple items and Sell Smart works out which photos belong to the same product.

Instead of generating one request per image and one more request per grouped item, the app now uses a more efficient two-step flow:

1. **Batch image analysis**
    - All uploaded images are compressed once in the browser
    - Gemini analyses them together in a single batch
    - It returns grouped items with structured metadata such as:
        - brand
        - model
        - product type
        - colour
        - condition
        - gender
        - size
        - product signature

2. **Listing generation from metadata**
    - Each grouped item is then turned into a final listing using its metadata
    - This avoids re-uploading the same images repeatedly
    - The result is faster generation, lower quota usage, and better reliability

Each grouped item becomes one listing.

## Future Improvements
* Chatbot for customer support and onboarding guidance
* Explore whether the information / feedback section should remain a modal or evolve into a conversational help interface
* Create a mobile app version of Sell Smart for faster photo capture and listing creation
* Improve privacy controls around uploaded images (e.g., automatic deletion, clearer messaging that images are compressed client-side)
* Enhance pricing suggestions by incorporating external resale signals such as marketplace demand or sold-listing data
* Integrate a live foreign exchange API so currency switching reflects real-time exchange rates instead of static values
* Improve product recognition accuracy (brand detection, logo recognition, model identification)
* Provide listing quality scoring to help users optimise their resale listings
* Enable bulk export formats such as CSV for marketplace bulk uploads
* Explore direct integrations with resale platforms (eBay, Vinted, etc.) for one-click listing publishing

## Architecture

![Sell Smart Architecture](images/sell-smart-architecture.svg)

## Tech stack

| Layer | Tool |
|---|---|
| AI image analysis | **Google Gemini 2.5 Flash Lite** |
| AI listing generation | **Google Gemini 2.5 Flash** |
| Frontend | Vanilla HTML/CSS/JS (single file, no build step) |
| Backend proxy | Vercel Serverless Function |
| Hosting | GitHub Pages + Vercel |

---

## Quickstart for judges

> Everything runs in the browser. No database. No login. No data stored.

**Option 1 — Use the live demo (fastest)**
```
https://kanke.github.io/sell-smart/
```

**Option 2 — Run locally**
```bash
git clone https://github.com/kanke/sell-smart.git
cd sell-smart
# Open index.html directly in your browser — no build step needed
open index.html
```
> Note: Running locally requires the API proxy to be deployed (see below), or point `API_BASE` in `index.html` to your own Vercel/Cloud Run deployment.

**Option 3 — Deploy your own copy**

1. Fork this repo
2. Deploy to Vercel — `api/gemini.js` auto-deploys as a serverless function
3. Set environment variable in Vercel: `GEMINI_API_KEY=your_key_here`
4. Enable GitHub Pages for `index.html`
5. Update `API_BASE` in `index.html` to your Vercel URL

---

## Google Cloud integration

Sell Smart uses the **Google AI REST API** (`generativelanguage.googleapis.com`) — the same underlying interface as the official Google GenAI SDK — to call **Gemini 2.5 Flash**, a Google Cloud AI service.

The API is called server-side via a serverless proxy to keep the key secure. The proxy is deployable to both **Vercel** and **Google Cloud Run**.

**Relevant code:**
- Frontend API call: [`index.html#L684`](https://github.com/kanke/sell-smart/blob/d664b230e532cc715d671fb1972f2af12fb9e79d/index.html#L684)
- Server-side proxy: [`api/gemini.js`](https://github.com/kanke/sell-smart/blob/main/api/gemini.js)

---

## How the AI flow works

```text
1. User uploads up to 12 photos
   → Images are compressed once in the browser and cached

2. Batch analysis request
   → All images are sent together to Gemini
   → Gemini returns grouped item metadata:
      {
        groupId,
        photoIndexes,
        brand,
        model,
        productType,
        color,
        condition,
        gender,
        size,
        productSignature
      }

3. Listing generation request
   → Each grouped item is sent as metadata only
   → Gemini returns listing JSON:
      {
        title,
        description,
        suggestedPrice,
        rrp,
        condition,
        category,
        tags
      }

4. UI renders one final resale listing per grouped item

```
---

## Privacy

All image processing happens **in the browser**. Photos are compressed using the Canvas API and sent only to the Gemini API via the secure proxy. No images are stored anywhere. No user data is collected.

---