# Map

It is a common development scenario where you will encounter having a list of things and wanting to change the items in that list in some way.

The `map` function allows you to achieve this by taking a function (used to change the items somehow) and a list then returns a new list with the function applied to each element.

A simple example would look like the below:

```
import { map } from 'ramda'

// function used to "map" each item from the orginal list to the new one
const addOne = num => num + 1

// our list we want to "map" through
const list = [1, 2, 3]

// "map" the addOne function against each item in list
const result = map(addOne, list)

result // [2, 3, 4]

// note that this doesn't mutate the original list
list // [1, 2, 3]
```

Using Ramda's `map` over JavaScript's built in implementation gives us access to the functional power of Ramda which include concepts like currying and composibility. Having access to these concepts becomes hugely beneficial as the nature and complexity of the data and problems we will work with grows.

## Map in React

UI development is no different we often encounter lists of data we want to manipulate and / or display on a page somewhere.

We will generally receive data via APIs or our client state and then want to turn them into consumable components our application knows how to handle.

In React we are more often than not wanting to turn (or "map") the list of data into a list of components for our application to render into the DOM and this is where `map` comes in.

So given a list of data, for example `organizations` (this is a pretty standard structure we will work with, an array of objects):

```
const organizations = [
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
  organizations
)
```

### React lists and the `key` prop

Also note the additional `key` prop we give to the component returned by the function used in `map`. This is an important performance consideration for React. The `key` is used by React to ensure only the components that need to change are when it updates the DOM.

Using a unique `key` value for each component ensures this so avoiding the use of the `index` of that item in the list would be the preferred approach.

### Putting it all together

The full React component might look like:

```
// src/components/OrganizationList/index.js
import * as React from 'react'
import { map } from 'ramda'

import Organization from '../Organization'

export default function OrganizationList (organizations) {
  return (
    <div>
      {map(
        ({ id, image, title }) => <Organization key={id} title={title} image={image} />,
        organizations
      )}
    </div>
  )
}
```

A note on the example above and the function we give to `map` to apply to each list item. Here we defined the function inline using an arrow function but depending on the complexity of what the function does it might make more sense to define it elsewhere.

For example we could pull the function out on its own as `mapOrganization`:

```
// src/components/OrganizationList/index.js
import * as React from 'react'
import { map } from 'ramda'

import Organization from '../Organization'

function mapOrganization ({ id, image, title }) {
  return <Organization key={id} title={title} image={image} />
}

export default function OrganizationList (organizations) {
  return (
    <div>
      {map(
        mapOrganization,
        organizations
      )}
    </div>
  )
}
```
