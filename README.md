# Next JS

A Next.js cheat sheet repository

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
