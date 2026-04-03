# Rendering Patterns

## Quick Start Commands

```bash
# CSR — just open the file
open csr/index.html

# SSG — build then open
cd ssg && npm run build && open dist/index.html && cd ..

# SSR
cd ssr && npm install && npm start    # http://localhost:3001

# ISR
cd isr && npm install && npm start    # http://localhost:3002

# Island Architecture
cd island-architecture && npm install && npm start   # http://localhost:3003
```

## What to Pay Attention To

1. **View Source** — CSR shows almost nothing, the others show full content
2. **Timestamps** — SSR changes every refresh, SSG never changes, ISR changes after the revalidation window
3. **JavaScript dependency** — Disable JS and see which pages still work
4. **Islands** — Notice how only the interactive parts use JS, the rest is static

---

## Rendering Patterns Explained

### CSR (Client-Side Rendering)

**What is it?**
Imagine going to a restaurant and the waiter gives you an empty plate with ingredients on the side — you assemble the dish yourself at the table. That's what CSR does: the server sends an almost empty HTML file, and the browser's JavaScript builds the entire page. That's why if you view the page source, there's almost nothing there.

**Pros:**
- Very fast navigation between pages after initial load
- Reduces server workload (computation happens on client)
- Rich, interactive user experiences

**Cons:**
- Slow initial page load (blank screen while JS loads and executes)
- Poor SEO — search engines have difficulty indexing content
- Doesn't work without JavaScript enabled
- Higher data usage for users

---

### SSR (Server-Side Rendering)

**What is it?**
Now imagine the waiter brings the dish ready from the kitchen — you just eat. With SSR, the server builds the complete HTML for each request and sends it ready to the browser. Every time you refresh the page, the server cooks it from scratch.

**Pros:**
- Excellent SEO — content is already in the HTML
- Fast initial page load and Time to First Byte (TTFB)
- Works without JavaScript
- Always fresh content

**Cons:**
- Higher server load (server must render on every request)
- Slower page transitions compared to CSR
- More expensive to host and scale
- Can be slower if the server is far from the user

---

### SSG (Static Site Generation)

**What is it?**
Here the dish was prepared yesterday and stored in the fridge. When you order it, the waiter just grabs and serves it — no cooking needed. The build generates HTML files once, and the server just serves the static file.

**Pros:**
- Extremely fast page loads
- Very cheap to host (can be deployed to CDN)
- Excellent SEO
- Minimal server requirements
- Best security (no dynamic server-side code)

**Cons:**
- Content is stale until you rebuild
- Not suitable for frequently changing content
- Build time increases with number of pages
- Can't personalize content per user

---

### ISR (Incremental Static Regeneration)

**What is it?**
It's like the refrigerated dish having an expiration date. While it's valid, everyone gets the same dish (fast). When it expires, the next customer still gets the old dish, but the kitchen starts preparing a new one in the background. The following customer gets the fresh dish. You get SSG's speed with SSR's updates, just with a small delay.

**Pros:**
- Fast as SSG for most requests
- Content updates automatically on schedule
- No need to rebuild entire site for content changes
- Scales well — static files served from CDN
- Good balance between performance and freshness

**Cons:**
- Users might see stale content temporarily
- More complex to configure and understand
- Cache invalidation can be tricky
- Not real-time (small delay in updates)

---

### Island Architecture

**What is it?**
Imagine a printed newspaper page. The text, photos, headlines — all static, no JavaScript needed. But in the middle of the page there's a voting widget and a live clock. Only these interactive bits receive JavaScript. The rest is pure HTML. This is island architecture: a sea of static content with small "islands" of interactivity.

**Pros:**
- Sends much less JavaScript to the browser
- Much faster page loads
- Better performance on low-end devices
- Most content works without JavaScript
- Excellent SEO
- Progressive enhancement by default

**Cons:**
- Requires careful planning of interactive zones
- Not all frameworks support this pattern well
- Can be complex to implement
- May require state management between islands
- Learning curve for developers used to SPA patterns
