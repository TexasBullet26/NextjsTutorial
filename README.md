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

### 4. Create Dynamic Pages

**Introduction**

Now we know how to create a basic Next.js app with multiple pages. In order to create a page, we have to create an actual file on the disk.

However, in a real app, we need to create pages dynamically in order to display dynamic content. There are many approaches to do that with Next.js.

We are starting with creating dynamic pages by using **query strings**.

We'll be creating a simple blog app. It has a list of all the posts on the home (index) page.

![](https://cloud.githubusercontent.com/assets/50838/24542722/600b9ce8-161a-11e7-9f1d-7ed08ff394fd.png)

When you click on a post title, you'll be able to see the individual post on its own view.

![](https://cloud.githubusercontent.com/assets/50838/24542721/5fdd9c26-161a-11e7-9b10-296d4cb6912d.png)

Let's build that app.

**Adding a list of posts**

First of all, let's add the list of post titles in the home page. Add the following content to the `pages/index.js`:

```js
import Layout from '../components/MyLayout.js'
import Link from 'next/link'

const PostLink = (props) => (
  <li>
    <Link href={`/post?title=${props.title}`}>
      <a>{props.title}</a>
    </Link>
  </li>
)

export default () => (
  <Layout>
    <h1>My Blog</h1>
    <ul>
      <PostLink title="Hello Next.js"/>
      <PostLink title="Learn Next.js is awesome"/>
      <PostLink title="Deploy apps with Zeit"/>
    </ul>
  </Layout>
)
```

Once you add this content, you will see a page like this:

![](https://cloud.githubusercontent.com/assets/50838/24542722/600b9ce8-161a-11e7-9f1d-7ed08ff394fd.png)

Click the first link. You'll get a 404 page. That's fine. What's the URL of that page?

`/post?title=Hello%20Next.js`

**Passing Data via Query Strings**

We are passing data via a query string parameter (a query param). In our case, it's the "title" query param. We do this with our `PostLink` component as shown below:

```js
const PostLink = (props) => (
  <li>
    <Link href={`/post?title=${props.title}`}>
      <a>{props.title}</a>
    </Link>
  </li>
)
```

(Check the `href` prop of the `Link` component.)

Just like that, you can pass any kind of data you like with query strings.

**Create the "post" page**

Now we need to create the post page to show the blog post. In order to do that, we need to get the title from the query strings. Let's see how to do that:

Create a file called `pages/post.js` and add the following content:

```js
import Layout from '../components/MyLayout.js'

export default (props) => (
  <Layout>
    <h1>{props.url.query.title}</h1>
    <p>This is the blog post content.</p>
  </Layout>
)
```

Now our page will look like this:

![](https://cloud.githubusercontent.com/assets/50838/24542721/5fdd9c26-161a-11e7-9b10-296d4cb6912d.png)

Here's what's happening in the above code.
  * Every page will get a prop called "URL" which has some details related to the current URL.
  * In this case, we are using the "query" object, which has the query string params.
  * Therefore, we get the title with `props.url.query.title`.

---

Let's do a simple modification to our app. Replace the content of the "pages/post.js" with the following:

```js
import Layout from '../components/MyLayout.js'

const Content = (props) => (
  <div>
    <h1>{props.url.query.title}</h1>
    <p>This is the blog post content.</p>
  </div>
)

export default () => (
    <Layout>
       <Content />
    </Layout>
)
```

What'll happen when you navigate to this page?
http://localhost:3000/post?title=Hello%20Next.js
