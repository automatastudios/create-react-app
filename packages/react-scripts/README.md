# react-scripts

This package includes scripts and configuration used by [Create React App](https://github.com/facebookincubator/create-react-app).<br>
Please refer to its documentation:

* [Getting Started](https://github.com/facebookincubator/create-react-app/blob/master/README.md#getting-started) – How to create a new app.
* [User Guide](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) – How to develop apps bootstrapped with Create React App.


# Automata Usage

Much of this plan is based on [Maintaining a fork of create-react-app as an alternative to ejecting](https://medium.com/@denis.zhbankov/maintaining-a-fork-of-create-react-app-as-an-alternative-to-ejecting-c555e8eb2b63).

## Including in project

Initialize a Create React App as you would without any customizations

1. Make sure to have create-react-app installed (e.g. `npm install -g create-react-app`)
2. Run `create-react-app my-app`

Update dependencies to point to our customized copy of react-scripts

NPM

3. `npm uninstall react-scripts`
4. `npm install --save-dev @automatastudios/react-scripts`

YARN

3. `yarn delete react-scripts`
4. `yarn add --dev @automatastudios/react-scripts`


NOTE: Ejecting from our custom react scripts may be untested and unstable. If you plan to eject for further customizations it may be wise to eject from the latest release from the upstream Facebook project.

## Contributing


### Syncing from Upstream with Backstroke

`automatastudios/create-react-app@master` should always mirror the upstream `facebookincubator/create-react-app@master`. To keep these in sync we've configured a bot via [backstroke](https://backstroke.co/) to submit PRs to the master branch in our project fork.

### Updating `automata-react-scripts` branch with releases

To then incorporate those updates in our `custom-react-scripts` branch we can rebase from master.


IMPORTANT: Since we're only publishing a single pacakge from the create-react-app monorepo we need to be careful about dependencies. Therefore we should ONLY rebase against a release commit / version change so that we don't have `react-scripts` package dependencies that are ahead of other published packages. (Scneario: react-scripts on master is 14 commits ahead of the last published version and relies on features of react-dev-utils that have not yet been published)

### Local development of `@automatastudios/react-scripts`

TODO: document best way to set up a demo project / do local dev against and test react-scripts from git codebase

### Publishing a new version of `@automatastudios/react-scripts`

1. Increment the version number in package.json
2. From the react-scripts folder `npm publish --access public`

# Automata Customization & Opinions

## Browserslist Targets & package.json Requirements

I've removed any specified `browserslist` config (driving autoprefixer, other postcss plugins, babel) in favor of configuring on a project by project basis via a `browserslist` object in `package.json`. For example:

```
{
  "browserslist": [
    "> 1%",
    "last 2 versions"
  ]
}
```

If no browserslist config is present in package.json each tool will run with their default setup (may have unintended consequences).

## ESLint & Babel

We're going to use ESLint to enforce a strict approach to coding errors, best practices/organization, and formatting. By extending AirBnB rules we can catch a lot of of sloppiness, be a bit more uniform while sharing code, as well as be less prone to errors.

Customizations from AirBnB guidelines include:

* Allow JSX in JS files
* PropTypes: Only ERROR on undeclared PropTypes if file already contains PropTypes
* Console: Warn on console.log
* Stateless Functions: Warn when a stateless function could be used
* Bind: Warn on using bind in JSX handlers
* Keys: Warn on using array index as key
* Code Complexity: Warn if complexity is too great


Review the `eslintOptions` object in webpack configs for full linting customization.

### ESLint rules documentation:

* https://eslint.org/docs/rules/
* https://github.com/evcohen/eslint-plugin-jsx-a11y
* airbnb JS style guide (informs eslint-config-airbnb) https://github.com/airbnb/javascript


## Inline SVGS

Inline SVG content was not enabled by default in CRA due to it's incompatibily with SVG sprites which some projects might use. Because we don't tend to use sprites we've confirgured loading of SVG source by default.


## Future Enhancements

* Investigate Prettier for formatting instead of ESLint