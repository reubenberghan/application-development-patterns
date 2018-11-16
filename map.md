# Map

It is a common development scenario where you will encounter having a list of things and wanting to change the items in that list in some way.

The `map` function allows you to achieve this by taking a function (used to change the items somehow) and a list then returns a new list with the function applied to each element.

```
import { map } from 'ramda'

// function used to "map" each item from the orginal list to the new one
const addOne = num => num + 1

// our list we want to "map" through
const list = [1, 2, 3]

// "map" the addOne function against each item in list
const result = map(addOne, list)

result // [2, 3, 4]
```

Using Ramda's `map` over JavaScript's built in implementation gives us access to the functional power of Ramda which include concepts like currying and composibility. Having access to these functional concepts becomes hugely beneficial as the nature and complexity of the data and problems we need to work with grows.

## Map in React

UI development is no different we often encounter lists of data we want to manipulate and / or display on a page somewhere.

In front end applications these lists often come via API calls or our client state and at some point we need to take these lists of data and turn them into consumable components our application knows how to handle.

This is where `map` comes in and we take our data then map over it returning a list of React components we can render out.

So given a list of data:

```
const data = [
  {
    id: '123abc',
    title: 'Jimmy's Chocolate Factory',
    image: 'https://place-hold.it/200'
  },
    {
    id: '123abd',
    title: 'Anna's Computer Emporium',
    image: 'https://place-hold.it/200'
  },
    {
    id: '123abe',
    title: 'The Black Box Corp.',
    image: 'https://place-hold.it/200'
  },
]
```

And a component, for example `Organization` which takes `title` and `image` as `props`:

```
<Organization title={...} image={...} />
```

We want to "map" each object in the `data` list to our `Organization` component:

```
map(
  ({ id, image, title }) => <Organization key={id} title={title} image={image} />,
  data
)
```

Also note the additional `key` prop we give to the component returned by the function used in `map`. This is an important performance consideration for React. The `key` is used by React to ensure only the components that need to change are when it updates the DOM.

Using a unique `key` value for each component ensures this so avoiding the use of the `index` of that item in the list would be the preferred approach.
