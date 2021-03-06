# Nuxt Helpers
[![npm](https://img.shields.io/npm/v/nuxt-helpers.svg)]()
[![XO code style](https://img.shields.io/badge/code_style-XO-5ed9c7.svg)](https://github.com/sindresorhus/xo)

> Collection of useful and SSR compatible vue plugins for using with [nuxt.js](https://github.com/nuxt/nuxt.js)

## Features
- Fully SSR compatible.
- Tested and well designed for using with Nuxt.js.
- Plugins can be used individually so bundle size remains small.
- Easy nuxt installation

## Getting started

Install nuxt-helpers package:
```bash
npm install nuxt-helpers
# OR
yarn add nuxt-helpers
```

Change your `nuxt.config.js`:

```js
const NuxtHelpers = require('nuxt-helpers');

module.exports = NuxtHelpers([/* packages to be installed */], {

    // Your own nuxt config

}
```
You can install any module by just adding it to first argument. You can also pass '*' as first argument to enable all modules.

## Available modules
- [axios](#axios)
- [bootstrap](#bootstrap)
- [notifications](#notifications)
- [auth](#auth-store)
- meta
- dev
- optimize
- font-awesome

# Axios
This plugin is a wrapper around [axios](https://github.com/mzabriskie/axios). It tries to resolve and make easier lot's of ajax tasks specially with SSR.
So you can use **$get('profile')** instead of `(await Axios.get('http://server/api/profile')).data`.

- Uses optionally custom URL when executing requests in server-side.
- Handles all HTTP exceptions and prevents server side unhandled promise exceptions.
- Injects `$get`,`$post`,... into vue context instances so requests can be done out-of-the-box.
- Exposes `setToken` function so we can easily and globally set authentication tokens.
- Returns empty object if request fails.
- Throws *nuxt-friendly* exceptions if needed.

**Usage**

Install 'axios' plugin, then:

```js
import {$get} from 'nuxt-helpers/plugins/axios';

async data() {
    let {profile} = await $get('profile');
    return {profile}
}
```

Or In any other function: (This does not needs importing axios plugin)

```js
mounted() {
    let {profile} = await this.$get('this');
    return {profile}
}
```

**Customization**

Customization can be done using shared environment variables.

Environment variable | Default                 | Description
---------------------|-------------------------|--------------------------------------------
API_URL   (SSR)      | http://localhost:3000   | Base url for ajax requests in server-side
API_URL   (browser)  | [nothing]               | Base url for ajax requests in client (cannot be changed, use API_PREFIX)
API_PREFIX           | /api                    | Adds this prefix before all relative urls

**Notes**

- Also see [hapi-nuxt](https://github.com/fandogh/hapi-nuxt) which is compatible with with plugin.
- More usage examples and bundles (including docker+nginx) comming soon.

# Bootstrap
With [bootstrap-vue](https://github.com/bootstrap-vue/bootstrap-vue) you can easily use standard bootstrap 4 components with your app.
(you need to add bootstrap package in your package.json too)

**Usage**

Install 'bootstrap' plugin, then:

```vue
<template>
    <b-alert show>
        Hello Bootstrap!
    </b-alert>
</template>
```

# Notifications
Easy toasts for your app powered by [vue-notifications](https://github.com/se-panfilov/vue-notifications) and
[mini-toastr](https://github.com/se-panfilov/mini-toastr).

**Usage**

Install 'notifications' plugin, then:

Then you can define notification in your routes or components:

```js
export default {
   methods:{
     async login() {
         try {
             await this.$post('auth/login');
             this.success();
         } catch(e){
             this.error();
         }
     }  
   },
   notifications: {
            success: {
                title: 'Welcome :)',
                type: 'success'
            },
            error: {
                title: 'Error while authenticating',
                type: 'error'
            }
    },

}
```

# Auth Store
Forget about writing boring auth store for your next nuxt project!
Please note that this store is written to be used with standard [bak](https://github.com/fandogh/bak) routes.
More options & customization will be added in the future.

**Usage**

Fist install 'auth' plugin, then:

`store/auth.js`:

```js
import AuthStore from 'nuxt-helpers/store/auth';

const authStore = new AuthStore({ /*opts*/ });

// Your customizations

export default authStore;
```

**Options**
- **default_user** : Default fields for `state.auth.user`. (overrides using Object.assign when logged-in).
- **token_cookie** : Token cookie opts. (see [js-cookie docs](https://github.com/js-cookie/js-cookie) for more info)

# font-awesome
Leverage [Font Awesome](http://fontawesome.io/) the iconic font and CSS toolkit


# Contributions
Any contribution,bug report or component is highly welcomed :)

# License
[MIT License](https://github.com/fandogh/nuxt-helpers/blob/master/LICENSE) / Fandogh@2017
