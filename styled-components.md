# Styled Components

Styled components allow us to encapsulate CSS into their own components. They provide all of the power of CSS combined with the ability to add logic to styles through props passed to the component as we might with a regular React component.

An example might look like:

```javascript
// src/components/Button/index.js
import styled from 'styled-components'

const Button = styled.button`
  background-color: darkgray;
  
  &:hover {
    background-color: gray;
  }
`

Button.displayName = 'Button'

export default Button
```

## How we will use them

In order to keep our files clean we can separate the styled components into their own directories at the highest possible level that they are required. This simply means that if a styled component is only required by one component then it can sit underneath that components directory in the file structure or if it is required across the app then it can sit at the top level.

For example if we have an `Organization` component:

```javascript
// src/components/Organization/index.js
import * as React from 'react'

export default function Organization ({ image, title }) {
  return (
    <div>
      <img src={image} alt='organization' />
      <div>{title}</div>
    </div>
  )
}
```

And we wanted to add some CSS styling to this component we could create a styled component to wrap this:

```javascript
// src/components/Organization/OrganizationWrapper/index.js
import styled from 'styled-components'

const OrganizationWrapper = styled.div`
  background-color: lightgray;
  
  > div {
    font-weight: bold;
  }
`

OrganizationWrapper.displayName = 'OrganizationWrapper'

export default OrganizationWrapper
```

A couple of things to notice here, firstly the styled component is in its own file and secondly this is nested within the `Organization` component.

The reason for having styled components in their own directories is that we are able to snapshot test them and any conditional styling separately of the components that they are used in.

Because this styled component relates only to the Organization component there is no need for it to be any higher in the component hierarchy so can be located within the component it relates to. However if the styled component was used across multiple components it would make sense to sit higher in the hierarchy. For instance the `Button` styled component example above might be used across our app so having it at the top level is appropriate.

The other thing to note is that with styled components we will explicitly set the `displayName` to name of the styled component. The reason we need to do this explicitly for styled components and not regular function components is because React infers the `displayName` from the name of the function. With styled components this comes from the library so we override this to ensure more meaningful debugging messages.

Now using our new styled component the `Organization` looks like this:

```javascript
// src/components/Organization/index.js
import * as React from 'react'

import OrganizationWrapper from './OrganizationWrapper'

export default function Organization ({ image, title }) {
  return (
    <OrganizationWrapper>
      <img src={image} alt='organization' />
      <div>{title}</div>
    </OrganizationWrapper>
  )
}
```

And we can unit test our styled component in isolation:

```javascript
// src/components/Organization/OrganizationWrapper/index.js
import * as React from 'react'

import { shallow } from 'enzyme'

import OrganizationWrapper from '.'

describe('components:Organization:OrganizationWrapper', () => {
  it('renders the OrganizationWrapper component CSS properly', () => {
    expect(
      toJson(
        shallow(
          <OrganizationWrapper />
        ).first().render()
      )
    ).toMatchSnapshot()
  })
})
```

Note the `.first().render()` on the `shallow` call which pulls out the HTML and CSS which helps keep our snapshots clean.
