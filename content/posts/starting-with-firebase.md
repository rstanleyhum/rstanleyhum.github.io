---
title: "Starting With Firebase"
date: 2017-08-18T14:50:49-04:00
tags: []
draft: true
---

I have decided to use a blog to chronical my adventures in using Firebase and other technologies.

So I wanted to start with *Firebase Hosting*. From the documentation at:

https://firebase.google.com/docs/hosting/quickstart 

First, install the Firebase CLI:

```shell
npm install -g firebase-tools
```

Now the full documentation for the Firebase CLI is at:

https://firebase.google.com/docs/cli/

To login to the firebase account use the following command:

```shell
firebase login
```

This will bring up a webbrowser so that you can login into your account.

It should replay with a "Success! Logged in as <email address>"

Now I can check the projects that I have using:

```shell
firebase list
```

Also we can start a development server using:

```shell
firebase serve
```

This will start a local web server with my Firebase Hosting configuration.


Now I already had a firebase hosting site made so I will clone the repository using:

```shell
git clone git@github.com:rstanleyhum/mdhandbookwebapp.git
```

Then I need to load the node modules that it depends on and start the javascript/react project. I use yarn.

```shell
yarn install
yarn start
```

You need to have the *app/assets/firebase.secrets.js* file in the following format:

```
const firebaseConfig = {
        apiKey: "<apiKey>",
        authDomain: "<authDomain>",  
        databaseURL: "<databaseURL>",  
        projectId: "<projectId>",  
        storageBucket: "<storageBucket>",
        messagingSenderId: "<messagingSenderId>"
        };
export default firebaseConfig;
```

Next I "want" to do a `firebase serve` which should bring up a webbrowser and point to the home page.

