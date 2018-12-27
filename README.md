### Code style

Our projects use [`standard`][standard-url] to maintain code style and consistency and avoid style arguments. `npm test` should also run `standard` so that CI fails when style does not meet the requirements.

Other style points:

- Code should be simple and explicit.
- [Callbacks are preferred over Promises.](http://callbackhell.com/)
- Functional style is preferred to OOP.
- Prefer small modules that do one thing well and avoid large frameworks.
- If a module exports a single function use `module.exports = ...` not `module.exports.thing = ...`.
- Line length should be limited to 80 characters.
- When writing a module use this order for your code:
    - External dependencies (e.g. `require('http')`)
    - Internal dependencies (e.g. `require('./api')`)
    - Any declarations or actions absolutely needed before `module.exports`
    - `module.exports`
    - Everything else should be ordered "high-level" to "low-level" and "first-use" to "last-use" -- low-level helper functions should be at the bottom.
- Use descriptive variable names.
- Prefer `.forEach()` over for loops.
- Default name for a callback is `cb`.
- Functions should be short enough to fit on a single page (max 30-40 lines).
- Functions should generally not accept more than 3-4 arguments, consider using an "options" object instead.
```js
function foo (a, b, c, d, cb) { cb(a, b, c, d) } // wrong
function foo (opts, cb) { cb(opts.a, opts.b, opts.c, opts.d) } // right
```
- Use single line conditionals when possible.

  ```js
  // wrong
  if (thing) {
    something()
  }
  // right
  if (thing) something()
  ```

- [Use early returns when possible.](http://blog.timoxley.com/post/47041269194/avoid-else-return-early)
```js
function getAsyncThing (cb) {
    someAsyncWork(function (err, result) {
      if (err) return cb(err) // like this
      if (!result) return cb(new Error('No result found.')) // and this
      cb(null, result)
    })
}
```

### Git

- Never commit passwords or access tokens into version control. If you do, you must tell your manager or team lead immediately.
- [Each PR should be as small and simple as possible](git.md) to prevent bugs and make code review as quick as possible. Do not include unrelated changes. Open a separate issue/PR for those.
- Do not modify a project's `.gitignore` to add files related to your environment or editor. Use your own profile's `~/.gitignore` for that.
- Use present tense in commits "upgrade dependencies" or "introduce custom error object"
- Use [Conventional Commits][conventional-commits-url]:
```
feat: add hat wobble (#227)
^--^  ^------------^ ^----^
|     |              |
|     |              +-> Issue number
|     |
|     +-> Summary in present tense.
|
+-------> Type
```

#### Allowed types
* **feat**: A new feature
* **fix**: A bug fix
* **docs**: Documentation only changes
* **style**: Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
* **refactor**: A code change that neither fixes a bug nor adds a feature
* **perf**: A code change that improves performance
* **test**: Adding missing tests
* **chore**: Changes to the build process or auxiliary tools and libraries such as documentation generation

### Github Workflow

1. If one does not already exist, create an issue for your feature/fix/refactor/cleanup. Provide a clear title and description for the change that is needed (provide links and screenshots). For example:
  - Title:
    - Advertiser Form Should Autofocus Advertiser Name Field On Load
  - Description:
    - When a user loads the advertiser edit form (https://app.com/advertiser/form) the wrong field has autofocus. It should be the "Advertiser Name" field.
    - [ Insert Screenshot / Animated Gif Demonstrating the Problem ]
2. Create a new branch to work on the issue. Your branch should be prefixed with the issue number (e.g. 197-advertiser-form-autofocus-name).
3. After you begin work on your new branch, create a new pull request and add it to the *In Progress* column on the appropriate Project Board. Be sure that your PR describes your change, includes screenshots or animated gifs showing the change, and references the issue so that Github will automatically close it:
  - Title:
    - Change Advertiser Form to Autofocus Advertiser Name Field On Load
  - Description:
    - Text description of change
    - [ Insert Screenshot / Animated Gif] (if this is a UI change and visible)
    - Closes #197
4. After you are finished working on your PR:
  - Squash all commits to a single commit. You may use multiple commits while working, but may only have a single commit when finished / ready for review.
  - Be sure that all tests pass: run `npm test` and see the *Testing* section in this guide.
  - Be sure that your commit has the issue or PR number added to the end (e.g. "feat: add autofocus for advertiser name (#197)"). This way it is easy to find the context for a change in the git log.
  - Assign reviewers from your team.
  - Move your PR to the *Needs Review* column from the *In Progress* column on the project board.

### Testing

Tests are run with `npm test`. We use [Travis CI](http://travis-ci.com) on our projects to run tests on all commits. Please make sure that `npm test` runs without errors or failures before pushing.

We use [tape](https://github.com/substack/tape) for testing. All test files should start with `process.env.NODE_ENV = 'test'` to ensure the correct environment for database and other service usage.

### Logging

Application logging at several levels for ease of filtering in production: `{fatal: 60, error: 50, warn: 40, info: 30, debug: 20, trace: 10}` and `info` is the default for `console.log()`.

All API requests should be logged at level 20 (debug) so that we can see how the app is being used and identify bad behavior.

Critical errors (anything we should be notified about) should be logged with `console.error(err)` (level 50) where `err` is an Error object (strings do not have stacktraces). Attach the request id to the error so that it easy to see the context when diagnosing an issue.

It is useful to use logging in development, however you must have a good reason to merge logging to a production branch. Logging in production should be used only if it will be helpful in production. If a certain type of logging is useful in development, but not production, and you would like to commit it, use [`debug`](debug-url).

For client-side code, never leave `console.log()` in a commit to production. However, you may use [`debug`](debug-url).

### App Structure

* See [Interlincx/example-server](https://github.com/Interlincx/example-server) for an example HTTP API server.
* See [Interlincx/example-react-app](https://github.com/Interlincx/example-react-app) for an example React app.
* API urls should be kebab-case e.g. : `/api/route-name` (NOT `/api/routeName`)
* package.json scripts:
  * `npm start` to run the app
  * `npm test` to run the tests (e.g. `node test/index.js`)
  * `npm run dev` to run the app with development helpers (reloading etc...)
* client-side / static apps:
  * Anything public/static goes in `/public` (`index.html`, built JS, images, etc...)
  * running `npm run build` should output bundle file into aforementioned `/public` directory

### Authentication and Authorization

Our projects use [Authentic](http://npm.im/authentic) for authentication. Please review [Intro to Authentic](http://dry.ly/authentic) for an explanation of how it works.

### Reporting bugs

Make a bug report about any part of a project that didn't work as expected.
Try also to give some context to a problem you are encountering, and give as much details as possible like steps to reproduce, and the manifestation on it, like logs screenshots or such.

Ideally bug report should contain following:

1. Any important context info that might matter to behavior/manifestation (like OS, node version, npm version, NODE_ENV)
2. Exact steps to reproduce
3. Expected behavior
4. Actual behavior
5. Manifestation record if possible (logs/screenshots)

- When writing about logs/code use markdown syntax.
- Open an issue for that particular project on github.

### Suggesting Enhancements

Should you think that we could make a project work better, please feel free to suggest a way. We love feedback and we wish to keep things simple and improve them over time. We are building this bottom up and trying to evolve it and make more performant over time.

[debug-url]: http://npm.im/debug
[standard-image]: https://cdn.rawgit.com/feross/standard/master/badge.svg
[standard-url]: https://github.com/feross/standard
[conventional-commits-url]: https://conventionalcommits.org/
