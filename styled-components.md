# Styled Components

Styled components allow us to encapsulate CSS into their own components. They provide all of the power of CSS combined with the ability to add logic to styles through props passed to the component as we might with a regular React component.

```javascript
// src/components/Button
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

The reason for having styled components in their own directories is that we are able to snapshot test them and any conditional styling separately of the components that they are used in.

The other thing to note is that with styled components we will explicitly set the `displayName` to name of the styled component. The reason we need to do this explicitly for styled components and not regular function components is because React infers the `displayName` from the name of the function. With styled components this comes from the library so we override this to ensure more meaningful debugging messages.

The way that we will use them:
- separate them into their own directories
- this ensure that snapshot tests can be run on them individually
- the directories will be subdirectories (at the highest possible level of the hierarchy) of the components that require the styles
- this is so they are esily accesible to the components that require them (note a styled component used across the app and at the highest level could be a `Link` for example while a particular component wrapper would only be inside of that components directory)
- ensure that the `displayName` is set on them
