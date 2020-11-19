# testing-library

```js
import {render, cleanup} from ‘react-testing-library’;
// cleanup unmount component that are being tested after the test
afterEach(() => {
cleanup();
});
```

OR

```js
import ‘react-testing-library/cleanup-after-each’;
```

> debug from render method
> debug() or debug particular node debug(node)

```js
import {fireEvent} from ‘rtl’;

fireEvent.change(input, {target: { value: 10}})
```

to test this method handleChange(event) => event.target.value in our component

to access a div add the attribute `data-testid=“my-div”` to the div

```js
{ getByTestId } = render(…)
getByTestId(‘my-div’) …
```

other accessor `getbyLabelText`, `getByText` etc.

can call rerender from the render function to rerender a component
`rerender(<MyComponent />);`

to check that somethign doesn’t exit use `queryByTestId` instead of `getByTestId`
the first one will return null if not found and the second will throw an error

asynchronous action:

```js
import {wait, fireEvent, render} from ‘rtl’;
import {huhuhuhu as mockHuhuhu} from ‘../api’;

jest.mock(‘../api’, ()=> {
    return {
        huhuhuhu: jest.fn( subject => Promise.resolve({data: {uhuhuh: ‘huhuhu’}}))
}
})

async() => {
    render(<mycomponent />);
    fireEvent.click(button);
    await wait(() => expect(getbyTestId(‘huhu’).toBe(…)))
    expect(mockHuhuhu).toHaveBeenCalledOnce();
}
```

another technique to mock api call is to pass the mock as props to the component
and use defaultProps to default to the real api function. In our test we just pass the mock api call as a props

```js
const mockHuhu = jest.fn(….);

// to mock a component:
jest.mock(‘library to mock’, () => {
    return{
        ComponentToMock: props => (props.huhu ? props.children : null)
    }
})
```

to remove the console.error from jest-dom during test:

```js
beforeEach(() => {
    jest.spyOn(console, ‘error’).mockImplementation(() => {})
})
afterEach(() => {
    console.error.mockRestrore()
})

afterEach(() => {
    mockSavePost.mockClear() //mockSavePost is the function api call
})
```

using fake data:
`npm i -D test-data-bot`

```js
import {build, fake, sequence} from ‘test-data-bot’;
const userBuilder = build(‘User’).fields({
    title: fake(f => f.lorem.words()),
    id: sequence(s => `user-{s}`)
})
```

to reject a mock one time in a test:

```js
mockMyMock.mockRejectedValueOnce({data: {error; ‘huhu’}})

const postError = await waitForElement(() => getTestById(‘jhsjdh’))
waitForElement is coming from react testing library
```

react router test

```js
import {Router} from ‘react-router-dom’
import {createMemoryHistory} from ‘history’
// in test
const history = createMemoryHistory({initialEntries: [‘/‘]})
render(<Router history={history}><MyComponent/></Router>)
```

nice util render for react router:

```js
import {render as rtlRender} from ‘rtl’;
import {createMemoryHistory} from ‘history’

function render(
ui,
{
route = ‘/‘,
history = createMemoryHistory({initialEntries: [route]}),
…options
} = {}
) {
return {
…rtlRender(<Router history=history>{ui}</Router>),
history
}
}
```

react redux

```js
import {createStore} from ‘redux’
import {Provider} from ‘react-redux’
import {myReducer} from ‘../redux/reducer’

const store = createStore(myReducer, {})
render(<Provider store={store}><MyComponent></Provider>)
```

to simplify we can use an utility function

```js
function render(ui, {initialState, store = createStore(myReducer, intialState), …options} = {}){
return rtlRender(<Provider store={store}>{ui}</Provider>, options)
}
```

Test React.Portal

```js
import {…, within} from ‘rtl’
import {MyModal} from ‘../Modal’

test(‘modal shows the children’, () => {
render(
<Modal><div>test</div></Modal>
)
const {getByText} = within(document.getElementById(‘modal-root’))
expect(getByText(‘test’)).toBeInTheDocument()
})
```

to test setTimeout or setInterval we can use
jest.useFakeTimers() // on top of the test file
and in the test use (just before the expect)
jest.runOnlyPendingTimers()
