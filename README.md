# Introduction

At this point you have all the tools to build a full stack application.

This section of the course is dedicated to tying those tools together to build a first fulls stack web application.

We will start with the frontend app.

This lesson we will cover:

- [Setting up the router](./01-router-setup/README.md)

- [Creating a Cats component](./02-cats-component/README.md)

- [Creating a new cat form](./03-new-cat-form/README.md)

- [Connect form submit to a function](./03-new-cat-submit/README.md)

## The Project: Cat Tinder

Let's start with some wireframes for our project.

These are going to be the views we will set up using React.

![wires](https://s3.amazonaws.com/learn-site/curriculum/cat-tinder/cat-tinder-wireframe.png)

## App setup
Using create-react-app, react-router-dom and react-bootstrap, we can setup a new application:

```bash
$ create-react-app cat-tinder-frontend
$ cd cat-tinder-frontend
$ yarn add react-bootstrap react-router-dom
$ yarn add -D enzyme react-test-renderer enzyme-adapter-react-16
```


## Add a theme

I'm going to use the "United" theme from bootswatch.com, so I'll add the stylesheet to 'pubic/index.html'  You can download a theme from here: [Bootswatch](https://bootswatch.com/) and put it in the ```public/``` directory.

#### public/index.html
```
<link rel="stylesheet" href="%PUBLIC_URL%/bootstrap.min.css">
```

Once you have your app all set up, you are ready to start building your pages using `react-router`.

[Go to Setup the router](./01-router-setup/README.md)
