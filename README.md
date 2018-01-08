# learnnextjs

*A Small Tutorial on Setting-Up Next.js*

### What Is Next.js

Think about how apps are created with PHP. You create some files, write PHP code, then you deploy it. No worrying about routing that much, and the app is rendered on the server by default.

That's what [Next.js](https://github.com/zeit/next.js) does, but instead of PHP, Next build the app with JavaScript and React. Here are some of the other cool features Next.js brings us:
  - Server-rendered by default
  - Automatic code splitting for faster page loads
  - Simple client-side routing (page based)
  - Webpack-based dev environment which supports [Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/) (HMR)
  - Able to implement with Express or any other Node.js HTTP Server
  - Customizable with your own Babel and Webpack configurations

### Table of Contents

**Basics**
  1. Getting Started
  2. Navigate Between Pages
  3. Using Shared Components
  4. Create Dynamic Pages
  5. Clean URLs with Route Masking
  6. Server Side Support for Clean URLs
  7. Fetching Data for Pages
  8. Styling Components
  9. Deploying a Next.js App

### 1. Getting Started



### 2. Navigate Between Pages



### 3. Using Shared Components

We know that Next.js is all about pages. We can create a page by exporting a React component, and putting that component inside the `pages` directory. Then it will have a fixed URL based on the file name.

Since exported pages are JavaScript modules, we can import other JavaScript components into them as well.

In this lesson, we'll create a common Header component and use it in multiple pages. Finally, we'll look at implementing a Layout component and see how it can help us define the look for multiple pages.

**Create the Header Component**

Create a Header component in `components/Header.js` like this:

```js
import Link from 'next/link'

const linkStyle = {
  marginRight: 15
}

const Header = () => (
  <div>
    <Link href="/">
      <a style={linkStyle}>Home</a>
    </Link>
    <Link href="/about">
      <a style={linkStyle}>About</a>
    </Link>
  </div>
)

export default Header
```

**Using the Header Component**

Next, let's import this component and use it in our pages. For the `index.js` page, it will look like this:

```js
import Header from '../components/Header'

export default () => (
  <div>
    <Header />
    <p>Hello Next.js</p>
  </div>
)
```

You can do the same for the about.js page as well.

At this point, if you navigate to your app at http://localhost:3000, you'll be able to see the new Header and navigate between pages.
