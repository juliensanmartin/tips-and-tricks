Tips Jest

foo.bar = jest.fn()
// mock the function bar. can also pass a fn to jest.fn that will mock its implementation

//after that we can check a lot of things with this mock function
foo.bar.mock.calls //etc…

// with jes.spyOn no need to keep track of original function that we want ot mock
// it does it automatically for us but we need to use mockImplementation
jest.spyOn(foo, ‘bar’);
foo.bar.mockImplementation(//new fn implementation);
…
foo.bar.mockRestore()

// less monkey patching
inport Foo from ‘../foo’;

jest.mock(‘../foo’, () => {
return {
bar: jest.fn(…)
}
});
….
Foo.bar.mockReset()

//Mock in a more global place
1- create a **mocks** directory
2- inside create a file called the same as the file you want to mock (foo.js)

module.exports = {
bar: jest.fn(…)
}

3- and then in the test just use
jest.mock(‘../foo’)
…
foo.bar.mockReset()

to support ES Module in test set .babelrc to presets {modules: ‘commonjs’}

Jest runs on Node but by using jsdom it can simulate a browser env (window etc.)

to be able test file import css file:
1- in jest.config.js add (in this order)
moduleNameMapper: {
‘\\.module\\.css$’: ‘identity-obj-proxy’,
		‘\\.css$’: require.resolve(‘./test/style-mock.js’)
}
2- in test/style-mock.js:
module.exports = {}

to be able to simulate className:
1- npm install -D identity-obj-proxy

to use the real css property for snapshot component test
1- npm i -D jest-emotion
2- in Test add:
import {createSerializer} from ‘jest-emotion’;
import \* as emotion from ‘emotion’;
expect.addSnapshotSerializer(createSerializer(emotion));
3- possible as well- >
in jest.config.js:
snapshotSerializers: [whatever serializer you installed]

jest -u
// update snapshot

for dynamic import:
npm i -D babel-plugin-dynamic-import-node
in .babelrc:
plugin: [
…,
isTest ? ‘babel-plugin-dynamic-import-node’: null
]

const isTest = String(process.env.Node_ENV) === ‘test’

To have all common setup for jest tests:
in /test/setup-tests.js

import whatever is always imported in your test;
import ‘react-testing-library/cleanup-after-each’;
import {createSerializer} from ‘jest-emotion’;
import \* as emotion from ‘emotion’;
expect.addSnapshotSerializer(createSerializer(emotion));

in jest.config.js:
//before Jest is loaded
setupFiles: []
//after Jest is loaded
setupFilesAfterEnv: ‘require.resolve(‘./test/setup-tests.js’)’

Always useful to copy resolve.modules from webpack.config.js to moduleDirectories in jest.config.js to be able to use local module folder that will be resolve as node modules with jest

to be able to test component that use Provider (themeProvider etc.)
add a new file to /test/{anyfilename}.js
import React …
import {anyKindOfProvider} …
import …

function renderWithProviders(ui, options) {
return render(<AnyKindOfProvider whateverProps={huhu}>{ui}</…>, options)
}

for instance we can have in this file:
<Router>
<ReduxProvider store={store || options.store}>
<ThemeProvider>…

export \* from ‘react-testing-library’; // export everything from RTL
export {renderWithProviders as render}; // override render from ‘react-testing-library’

add the /test folder to jest.config.js in

import {render} from ‘anyfilename’;

And finally to have eslint understand module exported by jest
npm i -D eslint-import-resolver-jest
in .eslintrc add:
overrides: [
{
files: [‘**/__tests__/**’],
settings: {
‘import/resolver’: {
jest: {
jestConfigFile: path.join(\_\_dirname, ‘./jest.config.js’)
}
}
}
}
]

to use debugger in test file
in package.json add this script
“test:debug”: “node —inspect-brk ./node_modules/jest/bin/jest.js —runInBand”
npm run test:debug
open chrome://inspect/#devices and then click inspect

coverage:
in package.json script
“jest:debug”: “jest —coverage”

in jest.config.js
collectCoverageFrom: [‘**/src/**/*.js’]
// all js file inside src directory and not only the file that are used in tests

we can add a threshold to be sure the coverage does not go below a certain point
jest.config.js

coverageThreshold: {
global: {
statements: anyPercentage,
branches: ..
lines: …
functions: ..
}
}

codecov platform for code coverage
npm i-D codecov
and then in CI/CD run
npx codecov@3
go to codecov.io

test(‘test title’, () => {
expect()….
})

to be able to run `npm t` that goes to `test:watch` locally and `test:coverage` not locally:
npm i -D is-ci-cli
in package.json replace the test script with
“test: is-ci \”test:covergae\” \“test:watch\””,

to help with watch mode:
npm i -D jest-watch-typeahead
in jest.config.js
watchPlugins: [
‘jest-watch-typeahead/filename’,
‘jest-watch-typeahead/testname’
]

multi jest config:
in jest.config.js add
projects: [‘./test/jest.client.js’, ‘./test/jest.server.js’

npm i -D watch-select-projects
in jest.config.js watchPlugins add
‘jest-watch-select-projects’

eslint running inside jest runner during `npm t`
npm i -D jest-runner-eslint
create a custom config ./test/jest.lint.js:
const {roodDir} = require(‘./jest-common’)

module.exports = {
rootDir,
displayName: ‘lint’,
runner: ‘jest-runner-eslint’,
testMatch: [‘<rootDir>/**/*.js’],
testPathIgnorePatterns: [‘/node_modules/’, ‘/coverage/‘, ‘/dist/’, …]
}

in jest.config.js:
projects: [
‘./test/jest.lint.js’
]

in package.json:
“lint”: “jest —config test/test.lint.js”

npm i -D husky lint-staged
in package.json script:
“precommit”: “lint-staged”
create a new file lint-staged.config:
module.exports = {
linters: {
‘\*_/_.js’: [‘jest —findRelatedTests’]
}
}
