# use-visibility-change

A custom React Hook that provides easy access to the visibilitychange event.
Know how long it's been since a user has "seen" your app.

[![npm version](https://badge.fury.io/js/use-visibility-change.svg)](https://badge.fury.io/js/use-visibility-change) [![All Contributors](https://img.shields.io/badge/all_contributors-1-orange.svg?style=flat-square)](#contributors)

## Features

🕐 Know how long the user has been away from your page.

🕑 `onHide` called when the user changes tabs, closes the current tab, or minimizes the browser.

🕒 `onShow` when the page is once again visible to the user (passing the `lastSeenDate`).

🕓 Persists `lastSeenDate` to `localStorage`.

![use-visibility-change](https://user-images.githubusercontent.com/887639/55264178-c34caa00-5230-11e9-8351-f1983c62624c.gif)

This hook was inspired by this piece on [Web Platform News](https://webplatform.news/issues/2019-03-27#web-pages-can-now-detect-when-chrome-s-window-is-covered-by-another-window).


## Installation

```bash
$ npm i use-visibility-change
```

or

```bash
$ yarn add use-visibility-change
```

## Usage

Here is a basic setup.

```js
const result = useVisibilityChange(config);
```

### Parameters

You pass a single config object with the following properties.

| Parameter   | Description                                                                                     |
| :---------- | :---------------------------------------------------------------------------------------------- |
| `onHide` | An optional callback function that is called upon closing or navigation away from the current tab, or minimizing the browser. You could use this callback to save the application state. |
| `onShow` | An optional callback function that is called with the same thing as the returned object when the view is restored. You could use this callback to restore the application state, or reset the user's experience if they have been gone for some time. |
| `storageKey` | A string that will be used as the key when persisting the last seen date. Default = "useSaveRestoreState.lastSeenDateUTC" |
| `shouldReturnResult` | A boolean that, if true, will cause `useVisibilityChange` to return a result object, otherwise it will return `undefined` and not update its internal state. This will help to decrease unnecessary re-renders. Default = `false` if `onHide` and `onShow` are specified, else `true`. You should normally never have to set this unless you have `onHide` and/or `onShow` callbacks _and_ wish to have a result returned.  |


### Return

This hook returns a result object with the following keys.

| Property   | Description                                                                                     |
| :---------- | :---------------------------------------------------------------------------------------------- |
| `lastSeenDate` | A Date object representing the date that the user was last "seen". Returns `null` if this is the first time the user visited your site with this browser. |

## Example 1

Here we have a simple case where we render how long it's been since the user has "seen" your app.

```js
const App = () => {
  const { lastSeenDate } = useVisibilityChange();
  return (
    <div>
      <p>Change tabs, navigate away, or close the tab. Then come back.</p>
      {lastSeenDate ? (
        <>
          <p className="hidden-for">
            This page was hidden for{' '}
            <em>{((new Date() - lastSeenDate) / 1000).toFixed(2)} secs</em>
          </p>
          <p>Last seen on: {lastSeenDate.toISOString()}</p>
        </>
      ) : (
        'Welcome new user!'
      )}
    </div>
  );
};
```

### Live demo

You can view/edit the sample code above on CodeSandbox.

[![Edit demo app on CodeSandbox](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/vm6l68k427)

## Example 2

Here is another example that uses the `onHide` and `onShow` callbacks.

![use-visibility-change-2](https://user-images.githubusercontent.com/887639/55264260-04dd5500-5231-11e9-97e4-024eb711a75f.gif)

```js
let savedTitle;

const onHide = () => {
  savedTitle = document.title;
  document.title = '👋 Bye. See ya later!';
};

const onShow = () => {
  document.title = savedTitle;
};

const App = () => {
  useVisibilityChange({ onShow, onHide });
  return (
    <>
      <h1>useVisibilityChange demo.</h1>
      <p>Change tabs and notice the title.</p>
    </>
  );
};
```

### Live demo

You can view/edit the sample code above on CodeSandbox.

[![Edit demo app on CodeSandbox](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/xj6vz7wn1z)


## License

**[MIT](LICENSE)** Licensed

## Contributors

Thanks goes to these wonderful people ([emoji key](https://allcontributors.org/docs/en/emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore -->
<table><tr><td align="center"><a href="http://donavon.com"><img src="https://avatars3.githubusercontent.com/u/887639?v=4" width="100px;" alt="Donavon West"/><br /><sub><b>Donavon West</b></sub></a><br /><a href="#ideas-donavon" title="Ideas, Planning, & Feedback">🤔</a> <a href="#infra-donavon" title="Infrastructure (Hosting, Build-Tools, etc)">🚇</a> <a href="#maintenance-donavon" title="Maintenance">🚧</a> <a href="#review-donavon" title="Reviewed Pull Requests">👀</a> <a href="https://github.com/donavon/use-visibility-change/commits?author=donavon" title="Code">💻</a> <a href="#design-donavon" title="Design">🎨</a></td></tr></table>

<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/all-contributors/all-contributors) specification. Contributions of any kind welcome!
