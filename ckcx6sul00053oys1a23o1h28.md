## ReactJS: A Simple Custom Hook

React Hooks have changed the way we write components. Hooks have mentally pushed us to write more Functional than Classical Components.

Though once you start making your app with Hooks, suddenly you have 10s of different hooks and even though they are managing a related state, it becomes hard to manage them. 

They start feeling like clutter in the good-old Functional Components.

Looks unrelated? Have a look at this Component:
```jsx
import React from 'react';

const DataTable = ({ movies = []}) => (
  <div>
    {movies.map(({Poster, Title, imdbID, Year}) => (
      <div key={imdbID}>
        <img src={Poster} height="100" alt={Title} />
        <p>
          <a href={`/?t=${imdbID}`}>{Title}</a>
          <br />{Year}
        </p>
      </div>
    ))}
  </div>
)

export default DataTable
```

Now if we were to add the Data Loading requests and building the profile links, there are two ways to do it:
1. Add all the requests and function in the same component
2. Create a wrapper component to
* Make a request and build links
* Pass all the required Data and Functions as Props

Let’s try to see both ways and how our computer develops in size and functionalities. 


Loading Data, building event handlers and Markup in the same component:

```jsx
import React, { useEffect, useState, useContext } from 'react';
import KEY from './KeyContext';

const url = 'http://www.omdbapi.com/?s='

const DataTable = ({ query = 'Harry Potter' }) => {
  const key = useContext(KEY);
  const [movies, setMovies] = useState([])
  useEffect(() => {
    fetch(`${url}${query}&apikey=${key}`)
      .then(r => r.json()).then(res => setMovies(res.Search.sort((a,b) => (a.Year-b.Year))))
  }, [key, query])

  return (
    <div>
      {movies.map(({Poster, Title, imdbID, Year}) => (
        <div key={imdbID}>
          <img src={Poster} height="100" alt={Title} />
          <p>
            <a href={`/?t=${imdbID}`}>{Title}</a>
            <br />{Year}
          </p>
        </div>
      ))}
    </div>
  )
}

export default DataTable
```

And if we make a Wrapper Component to wrap the data table and pass the data as props; it will look like the following:

```jsx
import React, { useEffect, useState, useContext } from 'react';
import KEY from './KeyContext';

const url = 'http://www.omdbapi.com/?s='

const DataTable = ({ movies = []}) => (
  <div>
    {movies.map(({Poster, Title, imdbID, Year}) => (
      <div key={imdbID}>
        <img src={Poster} height="100" alt={Title} />
        <p>
          <a href={`/?t=${imdbID}`}>{Title}</a>
          <br />{Year}
        </p>
      </div>
    ))}
  </div>
)

const DataContainer = ({ query = 'Harry Potter' }) => {
  const key = useContext(KEY);
  const [movies, setMovies] = useState([])
  useEffect(() => {
    fetch(`${url}${query}&apikey=${key}`)
      .then(r => r.json()).then(res => setMovies(res.Search.sort((a,b) => (a.Year-b.Year))))
  }, [key, query])

  return <DataTable movies={movies} />
}

export default DataContainer
```

Now here comes the custom hooks. 

As we saw firstly, we can take the loading of data and related functions in separate functions which will trigger the same thing through that function. 

Additionally, we can have a context to initialize the defaults and some common data to share among apps

First of all, we want to separate the data loading. Let's make out new hook called `useMovies`

```js
const useMovies = (query = null) => {
	return fetch(`${url}${query}&apikey=${key}`)
    .then(r => r.json())
    .then(r => r.Search.sort((a,b) => (a.Year-b.Year)))
}
```

Now that our function is doing data loading, let's add some persistence to it with state hooks
```js
import {useState} from 'react';

const useMovies = (query = null) => {
  const [movies, setMovies] = useState([])
	fetch(`${url}${query}&apikey=${key}`)
    .then(r => r.json())
    .then(r => r.Search.sort((a,b) => (a.Year-b.Year)))
    .then(setMovies)
	return movies;
}
```


But we want to load the movies on the first call, not every call; and then get the new data when there is a change in the query.

Along with that, let’s separate the fetching/AJAX code in a separate file.

With above mentioned separation of concerns in the code; we have the following `useMovies` hook and `request` module respectively:

```js
// useMovies.js
import { useState, useEffect, useContext } from 'react';
import KeyContext from './KeyContext';
import request from './request';
import queryString from 'query-string';

const url = 'http://www.omdbapi.com/'

const sortMovies = (movies = []) => movies.sort((a, b) => (a.Year - b.Year))

const getUrl = (params) => [url, queryString.stringify(params)].join('?')

const useMovies = (query = null) => {
  const [q, setQuery] = useState(query)
  const [movies, setMovies] = useState([]);
  const apikey = useContext(KeyContext);

  useEffect(() => {
    q && request(getUrl({ apikey, s: q }))
    .then(r => r.Search)
    .then(sortMovies)
    .then(setMovies)
  }, [q, apikey])

  return [movies, setQuery];
}

export default useMovies;
```

```js
// request.js
export default (url, params) => fetch(url, params)
  .then(response => {
    if (response.status === 200) {
      try {
        return response.json()
      } catch (e) {
        return response.text()
      }
    }
    return response
  })
```

In the above function of our custom hook we did the following:

* Receive the First Query & initialize a state to receive changes in Query
* Movies data with useState Hook
* API key from the Context and useContext Hook
* Use useEffect to 
1. Trigger the first request for First Query
2. Request changes to API on change of Query
3. As API Key is coming from Context, it is prone to change and hence keep it in the Dependency of `useEffect` hook
4. Return the Data (i.e. `movies`) and Function to change Query (i.e. `setQuery`)

---

Though while creating or using hooks, there are two rules that you need to keep in mind
1. Only Call Hooks at the Top Level
2. Only Call Hooks from React Functions

The name of the rules state enough, though you can read more about them here: [Rules of Hooks – React](https://reactjs.org/docs/hooks-rules.html)

---

Moreover, if you just wanna use Hooks in most of the cases, you can check	out the following repository; it is a collection of Custom Hooks for almost everything: *https://github.com/streamich/react-use* 

---

## Conclusion

Hooks have simplified the code a lot in terms of writing and reading.

I am personally trying to use Hooks as much as possible.

I would like to know, Did you make your custom hook? And How?

Let me know through comments 💬 or on Twitter at  [@patel\_pankaj\_](https://twitter.com/patel_pankaj_)  and [@time2hack](https://twitter.com/time2hack)

If you find this article helpful, please share it with others 🗣

Subscribe to the blog to receive new posts right to your inbox.

---

#### Credits
Icon from [IconFinder](https://www.iconfinder.com/icons/5172985/chain_connect_hyper_internet_link_security_web_icon)
Photo by  [Ferenc Almasi](https://unsplash.com/@flowforfrank?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)  on  [Unsplash](https://unsplash.com/s/photos/hooks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 

---

Originally published at [https://time2hack.com](https://time2hack.com/reactjs-simple-custom-hook/) on July 21, 2020.
