# React Chciago

## Books to Read

* Don't make me think

## April 2018

### What's new in React 16.04

* portals -- render react element outside react tree
* fragments -- instead of rendering in `<div>.</div>`, we can render in `<React.Fragement>...</React.Fragement>`
    * [docs](https://reactjs.org/docs/fragments.html)

* async rendering
    * lifecycle hooks are being depreccated and replaced with better methods for async
        * `componentWillMount`  — please use `componentDidMount` instead
        * `componentWillUpdate ` — please use `componentDidUpdate` instead
        * `componentWillReceiveProps ` — a new function, `static getDerivedStateFromProps` is introduced

* context api
    * [more details](https://medium.freecodecamp.org/reacts-new-context-api-how-to-toggle-between-local-and-global-state-c6ace81443d0)

### React Native

* react-native-sensitive-info
* react-native-debugger
* `detox` for testing
* `app center` to do continuous builds for release in app store

* build native components for iOS and Android and pull them into React Native as components

## March 2018

### Testing React Apps

* Jest 101
	* jest comes with `create-react-app`
	* react-test-renderer vs enzyme
	* tests via snapshots... look into
	* deep render vs shallow render
	* snapshot-diff
	* jest-image-snapshot
	* jest-fetch-mock
	* vscode-jest
	* awesome-jest on Github
	* test interactions with enzyme
