# Writing Form Components

Out of the box react-context-form provides a `Form` component and an `Input` component. The Input component provided is meant purely as a usage example for how to use the context
API. The intention is for the developer to write their own custom components. In this example we will re-create the `Input` component already provided by react-context-form.

The idea is that the form components hook into the API via context. This allows the developer to structure the form in any manner and still have access to Form hooks.

Lets create the base structure of our react input component that hooks into the provided form context, and return a HTML input element with given props.
```javascript
import React, { Component, PropTypes } from 'react'

export default
class Input extends Component {

    static contextTypes = {
        form: PropTypes.object
    }

    render() {
        return (
            <input
                {...this.props}>
            </input>
        )
    }
}
```

Each component needs to register itself by providing in a unique id, and some meta information like validation.
```javascript
class Input extends Component {

    /* ... */

    static propTypes = {
        id: PropTypes.string.isRequired,
        validation: propTypes.string
    }

    componentWillMount() {
        const {id, validation} = this.props
        this.context.form.register({
            id,
            validation
        })
    }

    /* ... */
}
```

And finally, we need to hook up the input element to our form. We get the inputs current value using the `select()` tool, and use the provided `onChange()` handler to update its value.
```javascript
class Input extends Component {

    /* ... */

   render() {
        const {id} = this.props
        const {form: {onChange, select}} = this.context

        return (
            <input
                {...this.props}
                onChange={({target: {value}}) => onChange(id, value)}
                value={select(id).value}>
            </input>
        )
   }
}
```

Here is the entire component
```javascript
import React, { Component, PropTypes } from 'react'

export default
class Input extends Component {

    static contextTypes = {
        form: PropTypes.object
    }

    static propTypes = {
        id: PropTypes.string.isRequired,
        validation: propTypes.string
    }

    componentWillMount() {
        const {id, validation} = this.props
        this.context.form.register({
            id,
            validation
        })
    }

    render() {
        const {id} = this.props
        const {form: {onChange, select}} = this.context

        return (
            <input
                {...this.props}
                onChange={({target: {value}}) => onChange(id, value)}
                value={select(id).value}>
            </input>
        )
    }
}
```