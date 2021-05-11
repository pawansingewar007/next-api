# Basic API routes example

Next.js works on two approaches whether you want static page generation , incremental static page generation or pure server side page generation or rendering.

Static page generation - Will be implemented by getStaticProps() and getStaticPaths()(if dynamic paths need to be generated). getStaticProps will execute during build time get all your external API request data and generate static files

export async function getStaticProps(context) {
return {
props: {}, // will be passed to the page component as props
}
}

getStaticProps should return an object with:
props - A required object with the props that will be received by the page component. It should be a serializable object
revalidate - An optional amount in seconds after which a page re-generation can occur. More on Incremental Static Regeneration
notFound - An optional boolean value to allow the page to return a 404 status and page. Below is an example of how it works:

// posts will be populated at build time by getStaticProps()
function Blog({ posts }) {
return (
<ul>
{posts.map((post) => (
<li>{post.title}</li>
))}
</ul>
)
}

// This function gets called at build time on server-side.
// It won't be called on client-side, so you can even do
// direct database queries. See the "Technical details" section.
export async function getStaticProps() {
// Call an external API endpoint to get posts.
// You can use any data fetching library
const res = await fetch('https://.../posts')
const posts = await res.json()

// By returning { props: { posts } }, the Blog component
// will receive `posts` as a prop at build time
return {
props: {
posts,
},
}
}

export default Blog

If a page has dynamic routes (documentation) and uses getStaticProps it needs to define a list of paths that have to be rendered to HTML at build time.
If you export an async function called getStaticPaths from a page that uses dynamic routes, Next.js will statically pre-render all the paths specified by getStaticPaths.
export async function getStaticPaths() {
return {
paths: [
{ params: { ... } } // See the "paths" section below
],
fallback: true or false // See the "fallback" section below
};
}

fallback: false
If fallback is false, then any paths not returned by getStaticPaths will result in a 404 page. You can do this if you have a small number of paths to pre-render - so they are all statically generated during build time. It’s also useful when the new pages are not added often. If you add more items to the data source and need to render the new pages, you’d need to run the build again.

getServerSideProps (Server-side Rendering)
Version History
If you export an async function called getServerSideProps from a page, Next.js will pre-render this page on each request using the data returned by getServerSideProps.
export async function getServerSideProps(context) {
return {
props: {}, // will be passed to the page component as props
}
}

## Deploy your own

Deploy the example using [Vercel](https://vercel.com?utm_source=github&utm_medium=readme&utm_campaign=next-example):

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/git/external?repository-url=https://github.com/vercel/next.js/tree/canary/examples/api-routes&project-name=api-routes&repository-name=api-routes)

## How to use

Execute [`create-next-app`](https://github.com/vercel/next.js/tree/canary/packages/create-next-app) with [npm](https://docs.npmjs.com/cli/init) or [Yarn](https://yarnpkg.com/lang/en/docs/cli/create/) to bootstrap the example:

```bash
npx create-next-app --example api-routes api-routes-app
# or
yarn create next-app --example api-routes api-routes-app
```

Deploy it to the cloud with [Vercel](https://vercel.com/new?utm_source=github&utm_medium=readme&utm_campaign=next-example) ([Documentation](https://nextjs.org/docs/deployment)).
