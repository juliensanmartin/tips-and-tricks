Tips Cypress

npx cypress open

eslint
npm i -D eslint-plugin-cypress
add in .eslintrc
plugins: [‘eslint-plugin-cypress’]
env: {‘cypress/global’: true}

in .gitignore add
cypress/videos
cypress/screenshots

describe(‘’, ()=> {
it(‘’, ()=> {
})
})

cy.visit(‘http://localhost:8080’)

better to do
cy.visit(‘/’) and in cypress.json
{
“baseUrl”: “http://localhost:8080”,
“integrationFolder”: “cypress/e2e”,
“viewportHeight”: 900,
“viewportWidth”: 400,
}

npm i -D cypress-testing-library

in /cypress/support/index.js
import ‘cypress-testing-library/add-commands’

and now we can use getByText etc. instead of get

npm i -D start-server-and-test
in package.json
“cy:run”: “cypress run” //run cypress in headless mode not like cypress open
“cy:open”: “cypress open”
“test:e2e”: “is-ci \”test:e2e:run\” \”test:e2e:dev\””
“pretest:e2e:run”: “npm run build” // used by start-server-and-test start under the hood
“test:e2e:run”: “start-server-and-test start http://locahost:8080 cy:run”
“test:e2e:dev”: “start-server-and-test dev http://locahost:8080 cy:open”

update if make sense
“precommit”: “lint-staged && npm run test:e2e:run”
“validate”: “npm run test:coverage && npm run test:e2e:run”

to help debug add that between chain
.then(subject => {
debugger
return subject
})

in the actual file you can do:
if (window.Cypress) {
window.state = this.state;
window.setState = this.setState
}

to create fake data we can add a generate.js file into /cypress/support/
import {build, fake} from ‘test-data-bot’
const userBuilder = … see Tips rtl

then in the test:
const user = userBuilder();
cy.visit(‘/‘)
.getByText(/register/i)
.click()
.getByLabelText(/username/i)
.click()
…
.url()
.should(‘eq’, `${Cypress.config().baseUrl}/`)
.window()
.its(‘localStorage.token’)
.should(‘be.a’, ’string’)

Mock
cy.server().
cy.route({
method: ‘POST’,
url: ‘http://localhost:3000/register’
status: 200,
response: {}
})

Send request with cypress to go faster
cy.request({
url: ‘http://localhost:3000/register’,
method: ‘POST’,
body: user
})

create a custom command
in /cypress/support/command.js

Cypress.Commands.add(‘createUser’, ovverides => {
const user = userBuilder(ovverides)
cy.request(…)
.then(response => response.body.user)
})

Cypress.Commands.add(‘login’, user => {
cy.request(…)
.then({body} => {
window.localStorage.setItem(‘token’, body.data.token)
return body.user //the user will be the subject
})
})

Cypress.Commands.add(‘loginAsNewUser’, () => {
cy.createUser().then(user => {
cy.login(user)
})
})

and then in the test jsut do
cy.createUser().then(user => {
add all needed test
})

To use react dev tool on the cypress browser add to the <head> tag on index.html

<script>
	if (window.Cypress) { //because cypress itself uses React so the dev tool won’t  show us the good component
		window.__REACT_DEVTOOL_GLOBAL_HOOK__ = window.parent.__REACT_DEVTOOL_GLOBAL_HOOK__
	}
</script>
