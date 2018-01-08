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

**Excel**
  1. Export into a static HTML App.
  2. Lazy Loading Modules.
  3. Lazy Loading Components.

### 1. Getting Started

---

### 2. Navigate Between Pages

---

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

Change the `components` directory to `comp` and change the appropriate routes. Will this work the same as the directory being called `components`? Yes.

**The Component Directory**

Yes. It will work as expected.

We don't need to put our components in a special directory; the directory can be named anything. *The only special directory is the `pages` directory.*

You can even create the Component inside the `pages` directory.

Here we didn't do that because we don't need a direct URL to our Header component.

**The Layout Component**

In our a;;, we'll use a common style across various pages. For this purpose, we can create a common Layout component and use it for each of our pages. Here's an example:

Add this content to `components/MyLayout.js`:

```js
import Header from './Header'

const layoutStyle = {
  margin: 20,
  padding: 20,
  border: '1px solid #DDD'
}

const Layout = (props) => (
  <div style={layoutStyle}>
    <Header />
    {props.children}
  </div>
)

export default Layout
```

Once we've done that we can use this Layout in our pages as follows:

```js
// pages/index.js

import Layout from '../components/MyLayout.js'

export default () => (
    <Layout>
       <p>Hello Next.js</p>
    </Layout>
)
```

```js
// pages/about.js

import Layout from '../components/MyLayout.js'

export default () => (
    <Layout>
       <p>This is the about page</p>
    </Layout>
)
```

Let's say you remove `{props.children}` from the Layout, what happens to the app?

The content of the pages being displayed will be removed.

**Rendering Child Components**

If you remove {props.children}, the Layout cannot render the content we put inside the `Layout` element, as shown below:

```js
export default () => (
  <Layout>
    <p>This is the about page</p>
  </Layout>
)
```

This is just one way to create a Layout component. Here are some other methods of creating a Layout component:

```js
import WithLayout from '../lib/layout'

const Page = () => (
  <p>This is the about page</p>
)

export default withLayout(Page)
```

```js
const Page = () => (
  <p>This is the about page</p>
)

export default () => (<Layout page={Page}/>)
```

```js
const content = (<p>This is the about page</p>)

export default () => (<Layout content={content}/>)
```

**Using Components**

We've mentioned two use cases for shared components:
  1. As common header components.
  2. As Layouts.

You can us components for styling, page layouts, and any other tasks you like. Additionally, you can import components from NPM modules and use them.

---
