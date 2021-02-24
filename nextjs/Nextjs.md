# Next.js

To build a complete web application with React, you need to follow a few steps:

 	1.	Code needs to be bundled and compiled
 	2.	you need to do production optimizations
 	3.	You might need to statically pre-render some content for performance and SEO
 	4.	you might have client and server side rendering
 	5.	you might need to create server side code to connect your app to a data storage

Next.js can solve all that and more:

 	1. it has an intuitive page based routing system
 	2. Pre-rendering static generation and server-side generation are supported on a per page basis
 	3. it has automatic code splitting for faster page loads
 	4. Client-side routing with optimized prefetching
 	5. built in css and sass support
 	6. dev environment with fast refresh support
 	7. api routes to build api endpoints with serverless function
 	8. and its fully extendable

## Background Info

### Two Forms of Pre-rendering

Next.js has two forms of pre-rendering:

- **Static Generation** is the pre-rendering method that generates the HTML at <u>build time</u>. Then it is reused on each request
- **Server-side Rendering** is the pre-rendering method that henerates HTML on <u>each request.</u>

Next.js lets you choose which pre-rendering form to use for each page, creating a hybrid app. Static generation is recommended whenever possible because your page can be built once and served by CDN, making it faster. Pages that might use it could include: marketing pages, blog posts, e-commerce product listings, help and documentation pages. Aything where the data isn't typically changing every time you visit.

The question you should ask yourself is "Can I pre-render this page **ahead** of a user's request?" If yes, Static generation is the way to go.

It would not be a good idea to use it if the page frequently shows updated information or the page changes on every request.





## Creating a Next.js App

1. Create a next.js app

   1. ```
      npx create-next-app nextjs-blog --use-npm --example "https://github.com/vercel/next-learn-starter/tree/master/learn-starter"
      ```

   2. This creates an app with the example template.

### Creating Pages

In Next, a page is a component exported from the pages directory. Pages are associated with a route based on their <u>file name</u>.

- 'pages/index.js' is associated with the '/' route.
- 'pages/posts/first-post.js' is associated with the '/posts/first-post' route.

```javascript
export default function FirstPost(){
return <h1>First Post</h1>
}
```

- Components can have any name but you must export as a default export.

### Linking Pages

When linking between pages, use the 'Link' component. 

1.  Import Link

   1. ```javascript
      import Link from 'next/link'
      ```

2. Use <Link> to wrap around your <a> tag and move the href from the <a> to the <Link>.

   1. ```javascript
      <h1 className="title">
        Read{' '}
        <Link href="/posts/first-post">
          <a>this page!</a>
        </Link>
      </h1>
      ```

   2. '{' '}' adds an empty space which is used to divide text over multiple lines.

The Link component allows for client-side navigation between two pages within the same app. If wanting to link externally, just use the <a> tag.

### Code splitting and prefetching

Next.js does code splitting automatically. This means only pages necessary will load, ensuring faster load times. This process also isolates code. So if one page throws an error, the rest of the site can still function. Furthermore, in production, whenever a Link component appears, Next automatically prefetches the code for the linked page. So if and when it is clicked, the page transition will be near instant. 

### Assets

Next.js can serve static files like images under the top-level public directory. With a normal <img/> tag, you have to make sure your image is responsive, optimized and only loads when they enter the viewport.

Instead, Next used an 'Image' component to handle all this for you.

This <Image> tag has optimization out of the box. it allows for resizing images, optimizing and serving images in modern formats like WebP. 

Images will be **lazy-loaded** by default so page speed isnt penalized for images outside the viewport. They load as they are scrolled over. 

```javascript
import Image from 'next/image'

const YourComponent = () => (
  <Image
    src="/images/profile.jpg" // Route of the image file
    height={144} // Desired size with correct aspect ratio
    width={144} // Desired size with correct aspect ratio
    alt="Your Name"
  />
)
```

### Metadata

`<Head>` is a React Component that is built into Next.js. It allows you to modify the `<head>` of a page. 

```javascript
export default function FirstPost() {
  return (
    <>
      <Head>
        <title>First Post</title>
      </Head>
      <h1>First Post</h1>
      <h2>
        <Link href="/">
          <a>Back to home</a>
        </Link>
      </h2>
    </>
  )
}
```



### CSS Styling

**CSS Modules** locally scopes CSS by automatically creating a unique class name. This allows you to use the same CSS class name in different files without worrying about name collisions. It can also be imported anywhere in your application.

Next.js has you use '[name].module.css' naming convention. For example, if you have a layout component and want to add a css module, you would name it 'layout.module.css'.

In production, all CSS module files will be automatically concatenated into many minified and code-split '.css' files. This files ensure minimal css loading for your application. 

### Global Styles

CSS modules are useful for component level styling, but if you want some CSS to be loaded by every page, Next has support for that as well. To load global CSS files, create a file called **_app.js** in your pages directory.

```javascript
export default function App({ Component, pageProps }) {
  return <Component {...pageProps} />
}
```

This will be a top-level component which will be common across all the different pages. You can use it to keep state when navigating bwtn pages.

** **After creating this App component, you will need to stop and start the server again** **

Unlike CSS modules, you cannot import global CSS from anywhere else. This is bc it affects all the elements on the page. If you were to navigate from the homepage to the first post page, global styles from the homepage would affect first post page unintentionally.



## Pre-rendering and Data Fetching

By default, Next.js pre-renders every page. This means, it generates the HTML in advance which results in better performance and SEO.

Pre-rendering with Nextjs displays HTML on initial load, then after **hydration** (each generated HTML is associated with minimal JS code necessary for that page. When the page is loaded, JS runs code and makes the page interactive) components become initialized and the app becomes interactive.

No pre-rendering in a react app: On initial load, the app is not rendered. Once JS loads, the components are then initialized and app becomes interactive.

**Static Generation vs Server-side Rendering**

![Static Generation](https://nextjs.org/static/images/learn/data-fetching/static-generation.png)

![Server-side Rendering](https://nextjs.org/static/images/learn/data-fetching/server-side-rendering.png)

Again, Next.js allows you to choose which rendering you want on a per-page basis but static generation should be the preferred method unless the data needs to be updated on every request. 

### Static Generation

can be done with and without data. 

![Static Generation without Data](https://nextjs.org/static/images/learn/data-fetching/static-generation-without-data.png)

But some pages you might not be able to render the HTML without some sort of data fetching, whether its accessing a file system, external API or querying your database. 

![Static Generation with Data](https://nextjs.org/static/images/learn/data-fetching/static-generation-with-data.png)

**Static Generation with Data using 'getStaticProps'**

This works  when you export a page component, you can also export an 'async' function called **getStaticProps**. It runs at build time and inside the function, you can fetch the external data  and send it as props to the page:

```javascript
export default function Home(props) { ... }

export async function getStaticProps() {
  // Get external data from the file system, API, DB, etc.
  const data = ...

  // The value of the `props` key will be
  //  passed to the `Home` component
  return {
    props: ...
  }
}
```

This tells Next that this page has some data dependencies and when you pre-render, to resolve these first.

![Index Page](https://nextjs.org/static/images/learn/data-fetching/index-page.png)

To implement getStaticProps:

1. install **gray-matter** which lets us parse the metadata(YAML) in each markdown file.

   1. npm install gray-matter

2. create a top-level directory called lib

   1. create a file called posts.js (or whatever it is related to) inside it
   2. here you will create you function that gets your data

3. back in your pages/index.js

   1. import your getdata function

   2. and call it within getStaticProps:

      1. ```javascript
         import { getSortedPostsData } from '../lib/posts'
         
         export async function getStaticProps() {
           const allPostsData = getSortedPostsData()
           return {
             props: {
               allPostsData
             }
           }
         }
         ```

   3. By returning allPostsData inside of the props object, the blog posts will be passed ot the Home component as a prop. 

      1. and now you can access the blog posts like such: 

         1. ```javascript
            export default function Home ({ allPostsData }) { ... }
            ```



**getStaticProps Details**

1. Fetch External API or query database

   1. we fetched data from our file system, but you can also fetch from an external API endpoint:

      1. ```javascript
         export async function getSortedPostsData() {
           // Instead of the file system,
           // fetch post data from an external API endpoint
           const res = await fetch('..')
           return res.json()
         }
         ```

   2. or query a database directly:

      1. ```javascript
         import someDatabaseSDK from 'someDatabaseSDK'
         
         const databaseClient = someDatabaseSDK.createClient(...)
         
         export async function getSortedPostsData() {
           // Instead of the file system,
           // fetch post data from a database
           return databaseClient.query('SELECT posts...')
         }
         ```

      2. this only runs on the server-side, never the client-side. And it wont be included in the JS bundle for the browser. That means you can write code as such as direct DB queries without them being sent to the browsers.

2. Development vs production

   1. in development, getStaticProps runs every request
   2. in production it runs at build time. 

3. Only allowed in a page

   1. getStaticProps can only be expoerted from a 'page'. Not from non-page files. This is so react can have all the required data before the page is rendered.

### Fetching Data at Request Time

![Server-side Rendering](https://nextjs.org/static/images/learn/data-fetching/server-side-rendering-with-data.png)

1. To use server-side rendering, you need to export 'getServerSideProps' instead of getStaticProps.

   1. ```javascript
      export async function getServerSideProps(context) {
        return {
          props: {
            // props for your component
          }
        }
      }
      ```

**Client-Side Rendering**

If you do not need to pre-render the data, you can use this strategy:

	1.	statically generate parts of the page that do not require external data
 	2.	when page loads, fetch external data from client using JS and populate the remaining parts

This works well for dashboard pages. Because a dashboard is a private, user specific page, SEO is not relevant and the page doesnt need to be pre-rendered.

**SWR**

This react hook is highly recommended if youre fetching data on the client side. It handles caching, revalidation, focus tracking, refetching on interval and more. 

Example: 

```javascript
import useSWR from 'swr'

function Profile() {
  const { data, error } = useSWR('/api/user', fetch)

  if (error) return <div>failed to load</div>
  if (!data) return <div>loading...</div>
  return <div>hello {data.name}!</div>
}
```



## Dynamic Routes

### Page Path Depends on External Data

![Page Path Depends on External Data](https://nextjs.org/static/images/learn/dynamic-routes/page-path-external-data.png)

![How to Statically Generate Pages with Dynamic Routes](https://nextjs.org/static/images/learn/dynamic-routes/how-to-dynamic-routes.png)

### Implement getStaticPaths

1. create a file called '[id].js' inside your (posts) directory

2. create a function in lib/posts called getallpostids

   1. ```javascript
      export function getAllPostIds() {
        const fileNames = fs.readdirSync(postsDirectory)
      
        return fileNames.map(fileName => {
          return {
            params: {
              id: fileName.replace(/\.md$/, '')
            }
          }
        })
      }
      ```

3. import the function into [id].js and use it inside the getStaticPaths:

   1. ```javascript
      import { getAllPostIds } from '../../lib/posts'
      
      export async function getStaticPaths() {
        const paths = getAllPostIds()
        return {
          paths,
          fallback: false
        }
      }
      ```


### Rendering Markdown

To render markdown, use the 'remark' library 

```javascript
npm install remark remark-html
```

1. download

2. Add the following imports to 'lib/posts'

   1. ```javascript
      import remark from 'remark'
      import html from 'remark-html'
      ```

3. Get the getPostData() function in the same file to use remark:

   1. ```javascript
      export async function getPostData(id) {
        const fullPath = path.join(postsDirectory, `${id}.md`)
        const fileContents = fs.readFileSync(fullPath, 'utf8')
      
        // Use gray-matter to parse the post metadata section
        const matterResult = matter(fileContents)
      
        // Use remark to convert markdown into HTML string
        const processedContent = await remark()
          .use(html)
          .process(matterResult.content)
        const contentHtml = processedContent.toString()
      
        // Combine the data with the id and contentHtml
        return {
          id,
          contentHtml,
          ...matterResult.data
        }
      }
      ```

4. Since we updated getpostdata to be an asynchronous function, we need to update getstaticprops to use await when calling getpostdata.

   1. Updated:

      1. ```javascript
         export async function getStaticProps({ params }) {
           const postData = await getPostData(params.id)
           return {
             props: {
               postData
             }
           }
         }
         ```

5. finally, updated the post component to render contenthtml using 'dangerouslySetInnerHTML'

   1. ```javascript
      export default function Post({ postData }) {
        return (
          <Layout>
            {postData.title}
            <br />
            {postData.id}
            <br />
            {postData.date}
            <br />
            <div dangerouslySetInnerHTML={{ __html: postData.contentHtml }} />
          </Layout>
        )
      }
      ```

**Polishing Up**

- add a title tag to the post page

- Format the date

  - Use 'date-fns'

    - ```javascript
      npm i date-fns
      ```

  - create a date component

    - ```javascript
      import { parseISO, format } from 'date-fns'
      
      export default function Date({ dateString }) {
        const date = parseISO(dateString)
        return <time dateTime={dateString}>{format(date, 'LLLL d, yyyy')}</time>
      }
      ```

  - import date into your [id].js and use it over your postData.date



## API Routes

API routes let you create an API endpoint in a next.js app. You can do so by creating an funciton inside the 'pages/api' directory.

```javascript
export default function handler(req, res) {
  res.status(200).json({ text: 'Hello' })
}
```

- `req` is an instance of [http.IncomingMessage](https://nodejs.org/api/http.html#http_class_http_incomingmessage), plus some pre-built middlewares you can see [here](https://nextjs.org/docs/api-routes/api-middlewares).
- `res` is an instance of [http.ServerResponse](https://nodejs.org/api/http.html#http_class_http_serverresponse), plus some helper functions you can see [here](https://nextjs.org/docs/api-routes/response-helpers).

Important info about APIs:

- Do not fetch an API route from 'getStaticProps' or 'getStaticPaths'
  - instead, write your server-side code directly in getStaticprops/paths or use a helper function
  - getStaticProps/Paths run only on the server-side, never the client side. it wont be included in the JS bundle for the browser
- Handling Form inputs
  - a good use case for API routes is handling form input
    - you can have a form and have it send a 'POST' request to your API routes. then write your code to directly save it to your DB.
- Preview Mode
  - [Static Generation](https://nextjs.org/docs/basic-features/pages#static-generation-recommended) is useful when your pages fetch data from a headless CMS. However, it’s not ideal when you’re writing a draft on your headless CMS and want to **preview** the draft immediately on your page. You’d want Next.js to render these pages at **request time** instead of build time and fetch the draft content instead of the published content. You’d want Next.js to bypass Static Generation only for this specific case.
  - Next.js has a feature called **Preview Mode** to solve the problem above, and it utilizes [API Routes](https://nextjs.org/docs/api-routes/introduction). To learn more about it take a look at our [Preview Mode](https://nextjs.org/docs/advanced-features/preview-mode) documentation.











### Other

**Fast Refresh** is enabled on a Next.js dev server. This means when you make a change to the code, Next automatically applies the changes. No refresh or Nodemon necessary.

**Code splitting** is a feature supported by bundlers which can create multiple bundles that can be dynamically loaded at runtime. Code splitting your app can help you "lazy-load" just the things that are currently needed by the user, which can dramatically improve the performance of your app. 

**YAML** is a human-readable data-serialization language. It is commonly used for configuration files and in applications where data is being stored or transmitted. It targets many of the same communications applications as extensible markup language but has minimal syntax.

**YAML Front Matter** is a block placed as the first thing in a file and must take a valid YAML format, a set of triple dashed lines. Between the lines are predefined variables.

```yaml
---
layout: post
title: Blogging Like a Hacker
---
```

**dangerouslySetInnerHTML** is react's replacement for using innerHTML in the browser DOM. Generally setting html from code is risky because its easy to inadvertently expose your users to cross-site scripting (XSS) attacks. So use dangerouslySetInnerHTML and pass an object with a '__html' key to remind yourself that its dangerous

### Fallback

Recall that we returned `fallback: false` from [`getStaticPaths`](https://nextjs.org/docs/basic-features/data-fetching#getstaticpaths-static-generation). What does this mean?

If [`fallback` is `false`](https://nextjs.org/docs/basic-features/data-fetching#fallback-false), then any paths not returned by [`getStaticPaths`](https://nextjs.org/docs/basic-features/data-fetching#getstaticpaths-static-generation) will result in a **404 page**.

If [`fallback` is `true`](https://nextjs.org/docs/basic-features/data-fetching#fallback-true), then the behavior of [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching#getstaticprops-static-generation) changes:

- The paths returned from [`getStaticPaths`](https://nextjs.org/docs/basic-features/data-fetching#getstaticpaths-static-generation) will be rendered to HTML at build time.
- The paths that have not been generated at build time will **not** result in a 404 page. Instead, Next.js will serve a “fallback” version of the page on the first request to such a path.
- In the background, Next.js will statically generate the requested path. Subsequent requests to the same path will serve the generated page, just like other pages pre-rendered at build time.

If [`fallback` is `blocking`](https://nextjs.org/docs/basic-features/data-fetching#fallback-blocking), then new paths will be server-side rendered with `getStaticProps`, and cached for future requests so it only happens once per path.

This is beyond the scope of our lessons, but you can learn more about `fallback: true` and `fallback: 'blocking'` in the [`fallback` documentation](https://nextjs.org/docs/basic-features/data-fetching#the-fallback-key-required).























