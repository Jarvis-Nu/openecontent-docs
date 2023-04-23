---
description: You are on your way to content creation freedom, let's go🎉🎉🎉
---

# Quick Start

{% hint style="info" %}
**Goal:** To get OpenContent up and running in your local project
{% endhint %}

## Make sure you have node installed

You can verify this by running the code below in your command prompt

```
// Code to verify node is installed on this computer
node -v
```

You should get a response like

```
// Sample value returned by running node -v
v18.12.0
```

If not you can download node on your computer from the official website [here](https://nodejs.org/en/download)

## Make sure you have git installed

Verify you have git installed by running the code below in your command prompt

```
// Code to verify git is installed on your computer
git -v
```

You should  get a response like

```
// Sample value returned by running git -v
git version 2.38.1.windows.1
```

else you can get it by following one of the steps [here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

## Clone the studio repository

You can do this by running

```
// Clone OpenContent studio into your project
git clone https://github.com/Jarvis-Nu/opencontent-studio.git
```

## Go into the ui directory

To can do this by running

{% hint style="info" %}
How you do this depends on your package manager, the following assumes you are using npmcode
{% endhint %}

```
// Enter ui directory
cd ui
```

## Set your Web3 Storage Api key

You can get this by creating an account from web3 storage [here](https://web3.storage)

Create a .env file in the studio root and add

```
NEXT_PUBLIC_WEB3STORAGE_TOKEN=YOURAPIKEY
```

## Install the studio node modules

This varies based on you package manager

{% hint style="info" %}
**Note:** How you do this depends on your package manager
{% endhint %}

Because you already have node you can run

```
// Install all node modules
cd opencontent
npm install
```

## Spin up the studio

{% hint style="info" %}
**Note:** How you do this depends on your package manager
{% endhint %}

You can do this by running

```
// Spin up the studio
npm run dev
```

## Congratulations you have just gotten yourself a decentralize CMS in your project!!!🎉🎉🎉
