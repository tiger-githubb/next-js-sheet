# Next JS

A Next.js cheat sheet repository

## Table of Contents

- [Create a new Next.js app](#create-a-new-nextjs-app)
- [Manual Installation](#manual-installation)
- [Folder Structure](#folder-structure)
- [Routing](#routing)
- [Meta tags](#meta-tags)
- [The `_app.js` file](#the-_appjs-file)
- [The `Layout` component](#the-layout-component)
- [Sass](#sass)
- [Tailwind CSS](#tailwind-css)
- [Styled JSX](#styled-jsx)
- [The `_document.js` file](#the-_documentjs-file)
- [Fetch data](#fetch-data)
- [Dynamic routes](#dynamic-routes)
- [Export Static Site](#export-static-site)
- [API Routes](#api-routes)
- [Check for `development` mode or `production` mode](#check-for-development-mode-or-production-mode)
- [Custom Meta Component](#custom-meta-component)

## Create a new Next.js app

```bash
npx create-next-app
```

### Use TypeScript, ESLint and npm

```bash
npx create-next-app --typeScript --eslint --use-npm
```

## Manual Installation

- Add Next.js to your project

```bash
npm install next react react-dom
```

- Add the following scripts to your package.json

```json
"scripts": {
  "dev": "next dev",
  "build": "next build",
  "start": "next start",
  "lint": "next lint"
}
```

## Folder Structure

**Pages folder** - is the only required folder in a Next.js app. All the React components inside pages folder will automatically become routes

> Note: The name of the file will be the route name, use lowercase for the file name and PascalCase for the component name

**Public folder** - contains static assets such as images, files, etc. The files inside public folder can be accessed directly from the root of the application

**Styles folder** - contains stylesheets, here you can add global styles, CSS modules, etc

> Usually `globals.css` is imported in the `_app.js` file

**Components folder** - contains React components

### The `@` alias

The `@` alias is used to import files from the root of the project

```jsx
import Header from '@/components/Header'
```

To use the `@` alias, add the following to the `jsconfig.json` file at the root of the project

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["*"]
    }
  }
}
```

## Routing

- **Link** - is used for client-side routing. It is similar to the HTML `<a>` tag

```jsx
import Link from 'next/link'

export default function Home() {
  return (
    <div>
      <Link href='/about'>About</Link>
    </div>
  )
}
```

## Meta tags

- **Head** - is used to add meta tags to the page

```jsx
import Head from 'next/head'

export default function Home() {
  return (
    <div>
      <Head>
        <title>My page title</title>
        <meta name='description' content='Generated by create next app' />
        <link rel='icon' href='/favicon.ico' />
      </Head>
    </div>
  )
}
```

> The `Head` component should be placed inside the `Layout` component or inside the `_app.js` file

### Give a different title to each page

- Import the `Head` component and put the `title` tag inside it

## The `_app.js` file

Wrap around each page and here is where you would import global styles and put header and footer components

> Note: You could also put the header and footer components inside the `Layout` component

## The `Layout` component

- Create a `Layout` component and wrap around each page with children prop

```jsx
import Header from '@/components/Header'
import Footer from '@/components/Footer'

export default function Layout({ children }) {
  return (
    <div>
      <Header />
      {children}
      <Footer />
    </div>
  )
}
```

- Import the `Layout` component in the `_app.js` file

```jsx
import Layout from '@/components/Layout'

function MyApp({ Component, pageProps }) {
  return (
    <Layout>
      <Component {...pageProps} />
    </Layout>
  )
}

export default MyApp
```

## Sass

Next.js has built-in support for Sass

- Install `sass`

```bash
npm i -D sass
```

## Tailwind CSS

- Install `tailwindcss`

```bash
npm install -D tailwindcss autoprefixer postcss
```

- Create a `tailwind.config.js` file at the root of the project

```bash
module.exports = {
  content: [
    './pages/**/*.{js,ts,jsx,tsx}',
    './components/**/*.{js,ts,jsx,tsx}',
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

- Create a `postcss.config.js` file at the root of the project

```bash
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
```

- Add the following to `globals.css`

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

- Import `globals.css` in the `_app.js` file

```jsx
import '@/styles/globals.css'
```

## Styled JSX

Styled JSX is a CSS-in-JS library that allows you to write CSS inside a React component

It has two modes: global and scoped

- **Global** - styles are applied globally to the entire application

```jsx
export default function Home() {
  return (
    <>
      Your JSX here
      <style jsx global>{`
        p {
          color: red;
        }
      `}</style>
    </>
  )
}
```

- **Scoped** - styles are applied only to the component

```jsx
export default function Home() {
  return (
    <>
      Your JSX here
      <style jsx>{`
        p {
          color: red;
        }
      `}</style>
    </>
  )
}
```

> Note: If in vs-code the syntax highlighting for the `style` tag is not working, you can install the `vscode-styled-components` extension to fix this
>
> Be sure that the curly braces are on the same line as the `style` tag: `<style jsx>{`
>
> No need to use styled jsx if you use other methods like CSS modules or styled components

## The `_document.js` file

Here you can customize the `html` and `body` tags

> For instance you can add a `lang` attribute to the `html` tag

```jsx
import Document, { Html, Head, Main, NextScript } from 'next/document'

export default function Document() {
  return (
    <Html lang='en'>
      <Head />
      <body>
        <Main />
        <NextScript />
      </body>
    </Html>
  )
}
```

> Note: This file will be created if you create a new Next.js app with `npx create-next-app`

## Fetch Data

- **getStaticProps** - is used to fetch data at build time

```jsx
export async function getStaticProps() {
  const res = await fetch('https://.../posts')
  const posts = await res.json()

  return {
    props: {
      posts,
    },
  }
}
```

> `posts` will be passed to the component as a prop:

```jsx
export default function Home({ posts }) {
  return (
    <div>
      {posts.map((post) => (
        <h3>{post.title}</h3>
      ))}
    </div>
  )
}
```

- **getStaticPaths** - is used to specify dynamic routes to pre-render pages based on data

```jsx
export async function getStaticPaths() {
  const res = await fetch('https://.../posts')
  const posts = await res.json()

  const paths = posts.map((post) => ({
    params: { id: post.id },
  }))

  return { paths, fallback: false }
}
```

- **getServerSideProps** - is used to fetch data on the server on each request

```jsx
export async function getServerSideProps(context) {
  return {
    props: {
      // props for your component
    },
  }
}
```

> `getStaticProps` and `getServerSideProps` have a `context` parameter that contains the url `params` object
>
> You can use this to fetch data for a specific post (e.g. `context.params.id`)

## Fetch Data on the client

- **useEffect** - is used to fetch data on the client

```jsx
import { useEffect, useState } from 'react'

export default function Home() {
  const [posts, setPosts] = useState([])

  useEffect(() => {
    fetch('https://.../posts')
      .then((res) => res.json())
      .then((data) => setPosts(data))
  }, [])

  return (
    <div>
      {posts.map((post) => (
        <h3 key={post.id}>{post.title}</h3>
      ))}
    </div>
  )
}
```

## Dynamic Routes

- Create a folder inside the `pages` folder with the name of the dynamic route in square brackets (e.g. `[id]`)

- Create an `index.js` file inside the dynamic route folder

### Dynamic Links

- Create a link with that points to the dynamic route and pass the dynamic value as a prop

```jsx
import Link from 'next/link'

export default function Post({ post }) {
  return (
    <div>
      <Link href='/posts/[id]' as={`/posts/${post.id}`}>
        <a>{post.title}</a>
      </Link>
    </div>
  )
}
```

> Note: this is usually done inside a `map` function

## Export Static Site

Export a static site with `next export`

> Add an npm script to the `package.json` file:

```json
"scripts": {
  "export": "next build && next export"
}
```

> Run the script:

```bash
npm run export
```

> The static site will be exported to the `out` folder
>
> You can deploy this folder to any static site host such as GitHub Pages

#### Build a local server to test the static site

- Install `serve`

```bash
npm i -g serve
```

- Run the server

```bash
serve -s out -p 8000
```

## API Routes

You can work with any database in the `pages/api/` folder

> Note: Any API route that is placed inside this folder will be accessible like any other page in Next.js

- Create a folder inside the `pages` folder with the name of the API route (e.g. `api/posts`)

### Work with local data

- Create a `data.js` file at the root of the project

```js
const posts = [
  {
    id: 1,
    title: 'Post 1',
  },
  {
    id: 2,
    title: 'Post 2',
  },
  {
    id: 3,
    title: 'Post 3',
  },
]
```

- Import the data in the API route

```js
import { posts } from '@/data'
```

- Get the data

```js
export default function handler(req, res) {
  res.status(200).json(posts)
}
```

> You can now fetch the data as you would with any other API
>
> Note: Next.js needs absolute paths when fetching data

## Check for `development` mode or `production` mode

Since Next.js needs absolute paths when fetching data, you can check if you are in `development` mode or `production` mode

- Create a `config.js` folder at the root of the project with an `index.js` file inside

```js
const dev = process.env.NODE_ENV !== 'production'

export const server = dev ? 'http://localhost:3000' : 'https://yourwebsite.com'
```

- Now you can use `server` as a variable in your code as an absolute path when fetching data

```js
import { server } from '@/config'

export default function handler(req, res) {
  fetch(`${server}/api/posts`)
    .then((res) => res.json())
    .then((data) => res.status(200).json(data))
}
```

## Custom Meta Component

> Note: There is no need to create a custom meta component since we can use the `Head` component from Next.js

A meta component is used to add meta tags to the `head` of the document

- Create a `Meta.js` file inside the `components` folder

```jsx
import Head from 'next/head'

export default function Meta({ title, keywords, description }) {
  return (
    <Head>
      <meta charSet='utf-8' />
      <meta name='viewport' content='width=device-width, initial-scale=1' />
      <link rel='icon' href='/favicon.ico' />
      <meta name='keywords' content={keywords} />
      <meta name='description' content={description} />
      <title>{title}</title>
    </Head>
  )
}
```

> Tip: You can also use packages such as `next-seo` for this

- Add `defaultProps` to the `Meta` component so that you don't need to add props to it every time you use it

```jsx
Meta.defaultProps = {
  title: 'WebDev News',
  keywords: 'web development, programming',
  description: 'Get the latest news in web dev',
}
```

- Now you can use the `Meta` component in any page (it is common to use it in the `Layout` component)

```jsx
import Meta from '@/components/Meta'

export default function Layout({ children }) {
  return (
    <div>
      <Meta />
      {children}
    </div>
  )
}
```

### Add a specific title to a page

- Import the `Meta` component in the specific page and pass the title as a prop

```jsx
import Meta from '@/components/Meta'

export default function About() {
  return (
    <div>
      <Meta title='About' />
      <h1>Welcome to the About Page</h1>
    </div>
  )
}
```
