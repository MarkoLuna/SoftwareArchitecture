# Rendering Strategies

Deciding where to render HTML—on the user's browser, on the server dynamically, or pre-built at compile time—directly affects performance, SEO capability, and operational costs.

> [!TIP]
> Want to weigh SSR/SSG against a client-only setup at a higher level? See [React.js vs Next.js](./react-vs-nextjs.md).

---

## Client-Side Rendering (CSR)

In a CSR architecture, the server delivers a nearly empty shell HTML file along with a JavaScript script bundle. The browser downloads the JS, boots up the framework engine (React, Angular, Vue), executes API calls to fetch data, and builds the DOM directly inside the client's browser.

- **Use Case**: Rich dashboards, internal SaaS tooling, interactive canvas editors, or post-login user accounts where search engine crawlers do not need indexation.
- **Pros**:
  - 🟢 **Instant Transitions**: Once the application has loaded, page switches are virtually instantaneous because no new HTML pages need fetching.
  - 🟢 **Decoupled Server**: Low server workload—servers just serve static JS/CSS assets, which can be entirely cached on a CDN.
- **Cons**:
  - 🔴 **Slow Initial Load**: The user sees a blank screen (first contentful paint is delayed) while massive JS bundles download and execute.
  - 🔴 **SEO Challenges**: Search engine bots that cannot execute JS reliably will index a blank page.

---

## Server-Side Rendering (SSR)

With SSR, every user request to the server triggers a dynamic page build. The server intercepts the request, runs database or API fetches, compiles the data into HTML, and streams the finished page back to the browser. The browser instantly displays the visual HTML, and then downloads a companion JS bundle to **hydrate** the page (attaching JS event listeners to make static HTML interactive).

- **Use Case**: E-commerce catalog pages, public marketing blogs, or social networks where content changes rapidly and SEO indexation is mandatory.
- **Pros**:
  - 🟢 **Excellent SEO**: Crawlers receive pre-rendered HTML immediately.
  - 🟢 **Fast Initial Load**: Users see content rapidly (FCP) because they don't have to wait for client-side JS to execute.
- **Cons**:
  - 🔴 **Server Overhead**: High server CPU loads since every page request requires dynamic server-side rendering.
  - 🔴 **Hydration Gap**: Users can see the content but cannot click buttons or interact with menus until the hydration JS executes (interactive lock).

---

## Static Site Generation (SSG)

SSG compiles all application pages into static HTML, JS, and CSS files **at build-time** (when building the project for deployment). These static files are uploaded directly to a CDN for instant edge-delivery.

- **Use Case**: Documentation portals, marketing homepages, static blogs, or portfolios where data changes infrequently.
- **Pros**:
  - 🟢 **Lightning Speed**: Edge CDN delivery yields sub-100ms TTFB (Time to First Byte).
  - 🟢 **Operational Zero-Ops**: Minimal host overhead, immune to high-traffic database connection exhaustion.
- **Cons**:
  - 🔴 **Build Bottlenecks**: A site with 10,000 products will require hours to compile at build time.
  - 🔴 **Stale Data**: Updating a spelling error requires rebuilding and redeploying the entire project.

---

## Incremental Static Regeneration (ISR)

ISR is a hybrid strategy designed to bring the speed of SSG to massive sites. Pages are initially generated at build-time. However, when a request is made for a stale page, the CDN serves the cached static page instantly, while initiating a background rebuild of that single page. Once compiled, the edge cache is refreshed.

- **Use Case**: Large e-commerce catalogs or news sites containing millions of pages.
- **Pros**:
  - 🟢 **Fast & Scalable**: Retains CDN delivery speeds while allowing millions of dynamic pages to exist without rebuild bottlenecks.
  - 🟢 **Self-Healing State**: Background generation ensures data remains fresh without global deployment runs.
- **Cons**:
  - 🔴 **Stale-While-Revalidate**: The very first user to request a modified page will still see old, stale data.

---

## Rendering Performance Matrix

Architects should evaluate rendering choices using core performance indicators:

| Strategy | TTFB | FCP | LCP | TTI | SEO Profile | Server Load | Data Freshness |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **CSR** | 🟢 Fast (Static CDN) | 🔴 Slow (Blank screen) | 🔴 Slow (JS dependant) | 🔴 Slow (Hydration/boot) | 🔴 Poor | 🟢 Low (Client-side) | 🟢 Real-time (API calls) |
| **SSR** | 🔴 Slow (Server compile) | 🟢 Fast (Visual HTML) | 🟢 Fast (Visual HTML) | 🟡 Medium (Hydration gap) | 🟢 Excellent | 🔴 High (On-demand CPU) | 🟢 Real-time (On-request) |
| **SSG** | 🟢 Extremely Fast (CDN) | 🟢 Extremely Fast (CDN) | 🟢 Extremely Fast (CDN) | 🟢 Extremely Fast | 🟢 Excellent | 🟢 Extremely Low | 🔴 Stale (Build bound) |
| **ISR** | 🟢 Extremely Fast (CDN) | 🟢 Extremely Fast (CDN) | 🟢 Extremely Fast (CDN) | 🟢 Extremely Fast | 🟢 Excellent | 🟡 Low-Medium (Background) | 🟡 Near Real-time (Lazy) |

---

## CSR vs. SSR Execution Lifecycles

The sequence diagrams below illustrate the differing client-server lifecycles between CSR and SSR, highlighting the "Hydration Gap" where the user can see elements but cannot interact with them.

```mermaid
sequenceDiagram
    autonumber
    actor User as 👤 User Browser
    participant Server as 🌐 CDN / static server
    participant API as 🔌 Backend API

    Note over User, Server: Client-Side Rendering (CSR) Lifecycle
    
    User->>Server: 1. Request Page (GET /dashboard)
    Server-->>User: 2. Return Empty HTML Shell + JS Bundle Links
    Note over User: User sees a completely blank screen!
    
    rect rgb(220, 240, 255)
        Note over User: 3. Download & Parse JavaScript Bundle
        User->>API: 4. Request App Data (GET /api/user)
        API-->>User: 5. Return JSON payload
        Note over User: 6. Mount DOM & Render Page Content
    end
    
    Note over User: Page becomes fully visible and interactive!
```

```mermaid
sequenceDiagram
    autonumber
    actor User as 👤 User Browser
    participant Server as 🌐 Dynamic Server (Node/SSR)
    participant API as 🔌 Backend API

    Note over User, Server: Server-Side Rendering (SSR) Lifecycle
    
    User->>Server: 1. Request Page (GET /product-xyz)
    
    rect rgb(255, 235, 235)
        Note over Server: 2. Fetch Data before rendering
        Server->>API: 3. Fetch Product Details (GET /api/product/xyz)
        API-->>Server: 4. Return Product JSON
        Note over Server: 5. Compile Product + Data into Complete HTML
    end
    
    Server-->>User: 6. Stream Complete HTML Page
    Note over User: FCP: User instantly sees full page layout!
    
    rect rgb(255, 248, 220)
        Note over User: [Hydration Gap] User clicks buttons but nothing happens.<br/>Downloading Hydration JavaScript in background...
        User->>Server: 7. Fetch Hydration JS
        Server-->>User: 8. Return JS Bundle
        Note over User: 9. Execute JS & Attach Event Listeners to DOM
    end
    
    Note over User: Hydration Complete: Page becomes interactive!
```

---

[⬅️ Back to Frontend Architecture](./README.md)
