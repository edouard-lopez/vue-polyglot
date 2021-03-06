[![Build Status](https://travis-ci.org/guillaumevincent/vue-polyglot.svg?branch=master)](https://travis-ci.org/guillaumevincent/vue-polyglot)

> basic translation plugin for VueJS 2+

## Vue-Polyglot v0.1.0

:construction: this plugin is in construction, so API can change.
Wait for version 1.0.0 for production use.

## Installation

    npm install vue-polyglot

## Is it like Vue-i18n or Vue-Translate?

Vue-Polyglot is also a plugin used to translate your pages. 
So yes it's like Vue-i18n or Vue-Translate but the API corresponds to my needs.

## TLDR

 * load translation asynchronously with HTTP requests (use `axios` module)
 
 * guess browser language and use it automatically
 
 * default message directly in your component
 
    `{{ $t('error684', 'User already exists') }}`
 
 * load context in message
 
    `$t('unread messages', 'you got ${unreadMessages} unread messages', {unreadMessages: 5})`

## Example

```js
import Vue from 'vue';
import App from './App.vue';
import Polyglot from 'vue-polyglot';

Vue.use(Polyglot, {
  defaultLanguage: 'en-US',
  languagesAvailable: ['zh-CN','fr-FR']
});

new Vue({
  el: '#app',
  beforeCreate() {
    this.$polyglot.$getLocales('dist/i18n');
  },
  render: h => h(App)
});

```

It will load asynchronously translations using browser's language.  
For example if browser language is `fr-FR`, `this.$polyglot.$getLocales('dist/i18n')` will get translations from `/dist/i18n/fr-FR.json` file.

In your component load translation directly `$t(id, defaultMessage)` 

in your html :

```html
{{ $t('error684', 'User already exists') }}
```

in your javascript :

```js
Vue.component({
  data(){
    return {
      hello: this.$t('Hello World')
    }
  }
});
```

## API

### $t(id)

locales:

```js
'fr-FR': {
  'hello world': 'bonjour monde'
}
```

browser lang : `en-US`

`$t('hello world')` > `'hello world'` 

browser lang : `zh-CN`

`$t('hello world')` > `'hello world'` 

browser lang : `fr-FR`

`$t('hello world')` > `'bonjour monde'` 

### $t(id, defaultMessage)

locales:

```js
'fr-FR': {
  'error684': 'L\'utilisateur existe déjà'
}
```

browser lang : `en-US`

`$t('error684', 'User already exists')` > `'User already exists'` 

browser lang : `fr-FR`

`$t('error684', 'User already exists')`  > `"L'utilisateur existe déjà"` 


### $t(id, defaultMessage, context)

locales:

```js
'fr-FR': {
  'error684': 'L\'utilisateur ${user} existe déjà'
}
```

browser lang : `en-US`

`$t('error684', 'User ${user} already exists', {user: 'Guillaume'})` > `'User Guillaume already exists'` 

browser lang : `fr-FR`

`$t('error684', 'User ${user} already exists', {user: 'Guillaume'})`  > `"L'utilisateur Guillaume existe déjà"` 


### Loading translations synchronously

Set locales with `Vue.locales` option in your app:

```js
Vue.locales({
  'fr-FR': {
    'hello world': 'bonjour monde'
  },
  'zh-CN': {
    'hello world': '你好，世界'
  }
});
```

### Changing the language to use

Use the `setLang` method of the `$polyglot` property, like this:
```js
Vue.component({
  methods: {
    showAppInChinese() {
      this.$polyglot.setLang('zh-CN');
    }
  }
});
```

## Similar plugin

Vue-Polyglot is similar to [Vue-Translate](https://github.com/javisperez/vuetranslate)
