# Gatsby Theme Contentful Blog

A core, data-only theme for adding a blog to your site, with content from Contentful.

## Get started

Install the theme and add it to your Gatsby site's `gatsby-config.js`.

```sh
npm i gatsby-theme-contentful-blog
```

```js
// gatsby-config.js
require("dotenv").config({
  path: `.env.${process.env.NODE_ENV}`,
})

module.exports = {
  plugins: [
    "gatsby-theme-contentful-blog",
    // ...
  ],
}
```

Add `.env.development` and `.env.production` files with the following environment variables defined:

```sh
// .env example
CONTENTFUL_SPACE_ID=""
CONTENTFUL_ACCESS_TOKEN=""
```

### Add templates

This theme requires that your site include two templates in its `src` directory.
By default it expects these templates to exist:

- `src/templates/blog-index.js`
- `src/templates/blog-post.js`

Add these two templates to your site.

```js
// src/templates/blog-index.js
import * as React from "react"
import { graphql, Link } from "gatsby"

export default function BlogIndex(props) {
  const posts = props.data.allBlogPost.nodes

  return (
    <div>
      <h1>Blog</h1>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>
            <Link to={`/blog/${post.slug}`}>{post.title}</Link>
          </li>
        ))}
      </ul>
    </div>
  )
}

export const query = graphql`
  query {
    allBlogPost {
      nodes {
        id
        slug
        title
      }
    }
  }
`
```

```js
// src/templates/blog-post.js
import * as React from "react"
import { graphql } from "gatsby"

export default function BlogPost(props) {
  const post = props.data.blogPost

  return (
    <div>
      <h1>{post.title}</h1>
      <div
        dangerouslySetInnerHTML={{
          __html: post.html,
        }}
      />
    </div>
  )
}

export const query = graphql`
  query ($id: String!) {
    blogPost(id: { eq: $id }) {
      id
      slug
      title
      html
    }
  }
`
```

### Add a content model and content

In your Contentful space, add a new content model for `BlogPost`.

The content model requires the following fields:

- `slug`: Unique Short text with the custom validation: `^[a-zA-Z0-9\/_-]*$` - this field is used for the URL path
- `title`: Short text
- `body`: Rich text

## Options

```js
// example gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: "gatsby-theme-contentful-blog",
      options: {
        postPath: "src/templates/blog-post.js",
        indexPath: "src/templates/blog-index.js",
      },
    },
  ],
}
```

- `postPath`: relative path to template for the blog post pages
- `indexPath`: relative path to template for the blog index page
