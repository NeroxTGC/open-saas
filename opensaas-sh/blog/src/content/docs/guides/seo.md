---
title: SEO
banner:
  content: |
    🆕 Open SaaS is now running on <b><a href='https://wasp-lang.dev'>Wasp v0.15</a></b>! <br/>⚙️<br/>If you're running an older version and would like to upgrade, please follow the <a href="https://wasp-lang.dev/docs/migration-guides/migrate-from-0-14-to-0-15">migration instructions.</a>
---

This guides explains how to improve SEO for of your app

## Landing Page Meta Tags

Wasp gives you the ability to add meta tags to your landing page HTML via the `main.wasp` file's `head` property:

```js {8-11}
app SaaSTemplate {
  wasp: {
    version: "^0.13.0"
  },
  title: "Open SaaS",
  head: [
        "<meta property='og:type' content='website' />",
        "<meta property='og:url' content='https://opensaas.sh' />",
        "<meta property='og:title' content='Open SaaS' />",
        "<meta property='og:description' content='Free, open-source SaaS boilerplate starter for React & NodeJS.' />",
        "<meta property='og:image' content='https://opensaas.sh/public-banner.png' />",
        //...
  ],
  //...
```

Change the above highlighted meta tags to match your app. Wasp will inject these tags into the HTML of your `index.html` file, which is the Landing Page (`app/src/client/landing-page/LandingPage.tsx`), in this case.

This means you **do not** need to rely on a separate app or framework to serve your landing page for SEO purposes.

## Custom Meta Tags for Other Pages

If you want to customize the meta tags of a page other than the landing page, you can do this with [React Helmet Async](https://github.com/staylor/react-helmet-async).

Is a thread-safe fork of React Helmet, (serving as the React equivalent to [Next SEO](https://github.com/garmeeh/next-seo) for Next.js applications). If you're familiar with Next SEO, you'll find `react-relmet-async` follows similar patterns for managing meta tags in React applications.

### Installation

```bash
# Using npm
npm install react-helmet-async

# Using yarn
yarn add react-helmet-async

# Using pnpm
pnpm add react-helmet-async
```

### Basic Setup

First, wrap your main App (`@src/client/app.tsx` by default) component with `HelmetProvider`:

```jsx
import { HelmetProvider } from 'react-helmet-async';

export default function App({ children }) {
  return (
    <HelmetProvider>
      <div className='min-h-screen'>
        {children}
      </div>
    </HelmetProvider>
  );
}
```

Then, you can set page-specific meta tags in any component:

```jsx
import { Helmet } from 'react-helmet-async';

export function PageComponent() {
  return (
    <div>
      <Helmet>
        <title>Page Title | Your App</title>
        <meta name="description" content="Page description" />
        
        {/* Open Graph / Facebook */}
        <meta property="og:type" content="website" />
        <meta property="og:url" content="https://yourapp.com/page" />
        <meta property="og:title" content="Page Title" />
        <meta property="og:description" content="Page description" />
        
        {/* Twitter */}
        <meta name="twitter:card" content="summary_large_image" />
        <meta name="twitter:title" content="Page Title" />
        <meta name="twitter:description" content="Page description" />
        
        {/* Additional SEO tags */}
        <link rel="canonical" href="https://yourapp.com/page" />
        <meta name="keywords" content="relevant, keywords, here" />
      </Helmet>
      
      {/* Page content */}
    </div>
  );
}
```

## Structured Data with JSON-LD

JSON-LD (JavaScript Object Notation for Linked Data) helps search engines better understand your content by providing structured data. Here are some common examples:

### Basic Page Schema

```jsx
<script type="application/ld+json">
  {`
    {
      "@context": "https://schema.org",
      "@type": "WebPage",
      "name": "Page Title",
      "description": "Page description"
    }
  `}
</script>
```

### FAQ Schema

```jsx
<script type="application/ld+json">
  {`
    {
      "@context": "https://schema.org",
      "@type": "FAQPage",
      "mainEntity": [{
        "@type": "Question",
        "name": "What is CGPA?",
        "acceptedAnswer": {
          "@type": "Answer",
          "text": "CGPA stands for Cumulative Grade Point Average..."
        }
      },
      {
        "@type": "Question",
        "name": "How is CGPA calculated?",
        "acceptedAnswer": {
          "@type": "Answer",
          "text": "CGPA is calculated by taking the weighted average..."
        }
      }]
    }
  `}
</script>
```

### Product Reviews Schema

```jsx
<script type="application/ld+json">
  {`
    {
      "@context": "https://schema.org",
      "@type": "Product",
      "name": "Your Product",
      "aggregateRating": {
        "@type": "AggregateRating",
        "ratingValue": "4.8",
        "reviewCount": "1455"
      },
      "review": [
        {
          "@type": "Review",
          "author": {"@type": "Person", "name": "John Doe"},
          "reviewRating": {
            "@type": "Rating",
            "ratingValue": "5"
          },
          "reviewBody": "Great product, highly recommended!"
        }
      ]
    }
  `}
</script>
```

### Validating Your Structured Data

Always validate your JSON-LD implementation using these tools:

1. [Google's Rich Results Test](https://search.google.com/test/rich-results) - Test how your structured data appears in Google Search
2. [Schema.org Validator](https://validator.schema.org/) - Verify your JSON-LD syntax and structure

These tools will help you identify any errors and ensure your structured data is properly formatted for search engines.

:::tip[Star our Repo on GitHub! 🌟]
We've packed in a ton of features and love into this SaaS starter, and offer it all to you for free!

If you're finding this template and its guides useful, consider giving us [a star on GitHub](https://github.com/wasp-lang/wasp)
:::

## Docs & Blog Meta Tags

Astro, being a static-site generator, will automatically inject relevant information provided in the `blog/astro.config.mjs` file, as well as in the frontmatter of `.md` files into the pages HTML:

```yaml
---
title: 'My First Blog Post'
pubDate: 2022-07-01
description: 'This is the first post of my new Astro blog.'
author: 'Astro Learner'
image:
    url: 'https://docs.astro.build/assets/full-logo-light.png'
    alt: 'The full Astro logo.'
tags: ["astro", "blogging", "learning in public"]
---
```

Improving your SEO is as simple as adding these properties to your docs and blog content!

## A Word on SSR & SEO

Open SaaS and Wasp do not currently have a SSR option (although it is coming soon!), but that does not mean that Open SaaS apps are at a disadvantage with regards to SEO.

That's because the meta tags for the landing page (described above), plus the Astro docs/blog provided with Open SaaS are more than enough! Not to mention, Google is also able to crawl websites with JavaScript activated, making SSR unnecessary. 

For example, try searching "Open SaaS" on Google and you'll see this App, which was built with this template, as the first result! 
![open-saas-google-search](/seo/open-saas-google.png)
