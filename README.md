# NextJS Big Project

This project is a practice project from [React - The Complete Guide (incl Hooks, React Router, Redux)](https://www.udemy.com/course/react-the-complete-guide-incl-redux/)

## Key Concepts

in `_app.js`, the default returns the component of the page that is rendered, and the specific props for that page
```js
export default function MyApp({ Component, pageProps }) {
  return <Component {...pageProps} />
}
```
This file will hold all of the information/components/styling that will be applied to *every* page
- Good place to put the layout if that layout will be applied to every page
- Apply **general** components to the app

***reminder***: only use hooks at the root level of a component, never in a nested function

imports that are only used in getStaticProps or getServerSideProps wont' be bundled to the client

***important*** add metadata to pages

## Page Pre-Rendering

SEO will only return the first render cycle, so fetching data from a DB in useEffect() inside of a component will always show the data as empty in the returned html

Next.js has two forms of pre-rendering:
- Static Generation
  - page pre-rendered when project is built
- Server-side Rendering

### Static Generation

```js
export function getStaticProps(context) {
  // fetch data from API
  return {
    props: {
      propKey: propKeyValue
    }
  }
}
```
can only be used within Pages, not in any other components

will be called before the component is rendered

can use it to get the data that is needed, also is/can be async

load the data, then return that as props to the component and render with the data

can execute any code that normally runs on a server

never loads on client side and never executes on the client side

must return an object

can have problems with stale data, if app is built, then more data is added to the backend, the data will not get updated until the project is built again
- can add revalidate to the return props 
```js
return {
  props: {},
  revalidate: <time_in_seconds>
}
```
advantage: built ahead of time and will be faster
uses: data doesn't change too often

### Server-Side Rendering

want to rebuild page on every request, not on build or deployment
gives full access to the request and response objects

```js
export async function getServerSideProps(context) {
  const req = context.req
  const res = context.res

  // fetch data from an API

  return {
    props: {}
  }
}
```
disadvantage: have to wait on page to build on every request
uses: need access to complete request object or if you have data that changes multiple times every second where revalidate won't help

### getStaticPaths()

```js
export async function getStaticPaths() {
  return {
    fallback: false,
    paths: [
      {
        params: {
          id: 1,
          paramKey: paramKeyValue,
        }
      },
      {
        params: {
          id: 2,
          paramKey: paramKeyValue,
        }
      }
    ]
  }
}
```
must have fallback key
- set to false it only allows the supplied paths, and shows 404 for the rest
- set to true it allows missing paths to be dynamically generated when requests come in


## API Routes

don't return HTML code
accept incoming HTTP requests with JSON data attached to them
then can do whatever is needed

allows to build own API endpoints as part of project/app

don't create a react component function
define functions which contain server-side code

```js
export default async function handler(req, res) {
  // do stuff on server side with req

  // send back res
}
```

