# Map

It is a common development scenario where you will encounter having a list of things and wanting to change the items in that list in some way.

The `map` function allows you to achieve this by taking a function (used to change the items somehow) and a list then returns a new list with the function applied to each element.

```
import { map } from 'ramda'

const addOne = num => num + 1

const list = [1, 2, 3]

const result = map(addOne, list)

result // [2, 3, 4]
```

Using Ramda's `map` over JavaScript's built in implementation gives us access to the functional power of Ramda which include concepts like currying and composibility.

## Map in React

UI development is no different we will often have a list of things we want to display on a page somewhere.

We could get a list of items from many sources such as a third party API or our database (probably via our own API) but in any case these are likely to be given to us in a list form (in JavaScript this will generally be an `array`).

The thing though is that this list will probably be sent as data in a JSON, XML, or other data transfer format. Not really in a form that we can readily render out onto the screen.

This is where `map` comes in, we want to take our list of data map and over each item generally returning a list of React components turning the data into a more consumable format for our app.

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

And an `Organization` component which takes `title` and `image` as `props`:

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


