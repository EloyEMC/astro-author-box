# Author Box for Astro

This project provides an **Author Box** component for Astro, designed to automatically display author information for blog posts. The component fetches author data based on the `author` field in your Markdown post's frontmatter and displays it in a custom box on your site.

## Features

- Fetches author data dynamically from a JavaScript file (`authors.js`) based on the `author` field in the Markdown frontmatter.
- Displays author image, bio, and social media links (Twitter, GitHub, LinkedIn, YouTube, etc.).
- Easily customizable to add more fields or social media platforms.

## How to Use

### Step 1: Add Author Data in Markdown Files

Ensure your Markdown files (blog posts) have an `author` field in the frontmatter. For example:

```md
---
title: "My Blog Post"
author: "eloymartinez"
publishedDate: "2025-03-15"
---
Content of the post goes here...
```

The author field corresponds to the key used in the authors.js file to fetch the author's data.

### Step 2: Access the Author Field in the Post Page

In your Astro page or component that renders the post, access the author field from the frontmatter by using Astro.props.author. For example, in the post page (`post.astro`):

```astro
---
import AuthorBox from "../components/AuthorBox.astro";
const { author } = Astro.props;
---

<article>
  <h1>{Astro.props.title}</h1>
  <p>{Astro.props.publishedDate}</p>
  <div>
    <!-- The content of the post -->
    <slot />
  </div>

  <!-- Render the Author Box and pass the author prop -->
  <AuthorBox author={author} />
</article>
```

### Step 3: Create the Author Box Component

In your `AuthorBox.astro` component, receive the author prop and use it to fetch the author's details from the `authors.js` file. Here’s an example:

```astro
---
import { authors } from "../data/authors.js";
const { author } = Astro.props;
const authorData = authors[author] || null;
---

{authorData && (
  <section class="author-box border-2 rounded-lg p-6 flex gap-8 bg-neutral-900 text-white">
    <img src={authorData.image} alt={authorData.name} class="author-img rounded-full w-24 h-24 object-cover" loading="lazy" decoding="async" />
    <div class="author-info">
      <h3 class="text-xl font-semibold">{authorData.name}</h3>
      <p class="text-sm text-gray-400">{authorData.bio}</p>
      <div class="social-links mt-4 flex gap-3">
        {Object.entries(authorData.social).map(([network, url]) => (
          <a href={String(url)} class="text-blue-400 hover:text-blue-500 capitalize" target="_blank">
            {network}
          </a>
        ))}
      </div>
    </div>
  </section>
)}
```

## How It Works

- The Markdown file has an `author` field in its frontmatter, which corresponds to the key in the `authors.js` file.
- The Astro page or component that renders the Markdown file can access the `author` field through `Astro.props`.
- The `AuthorBox` component takes the `author` prop, looks up the author data in `authors.js`, and renders the author’s image, bio, and social links.

## Example Author Data

Here’s an example of how the `authors.js` file should look:

```js
/** @type {import('../types/authors').Authors} */
export const authors = {
  "eloymartinez": {
    name: "Eloy Martinez",
    image: "/eloy_feria_iluminacion.webp",
    bio: "Web and mobile developer. Always learning.",
    social: {
      twitter: "https://twitter.com/eloy_emc",
      github: "https://github.com/EloyEMC",
      linkedin: "https://www.linkedin.com/in/eloymartinezemc/",
      youtube: "https://youtube.com/@eloymartinez"
    }
  },
  "janedoe": {
    name: "Jane Doe",
    image: "/images/authors/janedoe.jpg",
    bio: "Frontend designer and developer passionate about accessibility.",
    social: {
      twitter: "https://twitter.com/janedoe",
      github: "https://github.com/janedoe"
    }
  }
};
```

You can add more authors and their social links as needed.

## Installation

Clone this repository into your Astro project:

```bash
git clone https://github.com/your-username/astro-author-box.git
```

Install the dependencies:

```bash
npm install
```

Import and use the `AuthorBox` component in your Astro project as shown above.

## Customization

- **Add more fields**: You can easily extend the `authors.js` file to include more data such as additional social links, websites, or even email addresses.
- **Styling**: The component comes with basic styling using Tailwind CSS. Feel free to customize the styles in the component as per your design preferences.
