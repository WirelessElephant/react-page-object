### `.findWrapperForSelect(propValue, childrenPropValueForOption[, options]) => ReactWrapper`

Find a select box

**Default Checked Props:** `id` and `name`

#### Arguments

1. `propValue` (`String`): Value is compared with the values of the checked props to assert a match.
2. `childrenPropValueForOption` (`String`): Value is compared with the
   `children` prop value of children `option` React elements for potentially
   matching `select` React element.
3. `options` (`Object`): Optional.
  * `propToCheck` (`String`): Name of prop to check against instead of the default checked props.
  * `showDebuggingInfo` (`Boolean`): If `true`, then messages detailing the process of finding a
    `select` React element will be outputted to the console.

#### Returns

[`ReactWrapper`][react-wrapper] for a `select` React element whose:
  1. `id` or `name` prop value equals `propValue`
  2. children include only one `option` React element whose `children` prop value equals `childrenPropValueForOption`

If `options.propToCheck` is specified, then the method returns a
[`ReactWrapper`][react-wrapper] for a `select` React element whose:
  1. value for the prop specified by `options.propToCheck` equals `propValue`
  2. children include only one `option` React element whose `children` prop value equals `childrenPropValueForOption`

#### Related Methods

- [`.select(propValue[, options]) => ReactWrapper`](select.md)

[react-wrapper]: https://github.com/airbnb/enzyme/blob/master/docs/api/mount.md#reactwrapper-api

#### Example in Jest

```js
import React from 'react'
import Page from 'react-page-object'

const App = () => (
  <div>
    <select id="select-id">
      <option>option 1</option>
    </select>
    <select name="select-name">
      <option>option 1</option>
    </select>
    <select className="select-class">
      <option>option 1</option>
    </select>
  </div>
)

describe('findWrapperForSelect', () => {
  let page, wrapper

  beforeEach(() => {
    page = new Page(<App />)
  })

  afterEach(() => {
    page.destroy()
  })

  it('finds wrapper - targeting id', () => {
    wrapper = page.findWrapperForSelect('select-id', 'option that does not exist')
    expect(wrapper.exists()).toBe(false)

    wrapper = page.findWrapperForSelect('select-id', 'option 1')
    expect(wrapper.exists()).toBe(true)
  })

  it('finds wrapper - targeting name', () => {
    wrapper = page.findWrapperForSelect('select-name', 'option 1')
    expect(wrapper.exists()).toBe(true)
  })

  it('fills in the select - targeting non-default prop', () => {
    wrapper = page.findWrapperForSelect('select-class', 'option 1')
    expect(wrapper.exists()).toBe(false)

    wrapper = page.findWrapperForSelect('select-class', 'option 1', { propToCheck: 'className' })
    expect(wrapper.exists()).toBe(true)
  })
})
```
