![Travis](https://travis-ci.org/gustawdaniel/blog.gustawdaniel.com.svg?branch=master)

# Development

Project is written with VuePress. To run locally use:

```
npm install
npm run dev
```

# Testing

If you want to test manually run both application and tests

```
npm run dev
npm run cy:open
```

To run tests in command line make sure that local server is down and run

```
npm run cy:run
```


# Deployment

To deploy, simply push code to master branch

```
git push origin master
```

This is connected by travis with firebase deployment.

# Files structure

Directories

- .vuepress - VuePress configuration, theming and plugins
- _drafts - blog posts that are not published
- cypress - tests of application
- dist - final build
- posts - main content - published articles

Files

- .firebaserc, firebase.json - hosting configuration
- travis.yml - configuration of building, testing and deployment 
- index.md - content of main page
- package.json - packages installed by npm