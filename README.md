![npm bundle size](https://img.shields.io/bundlephobia/minzip/@swoga/vue-collapsible-panel?style=flat-square)
![npm](https://img.shields.io/npm/dt/@swoga/vue-collapsible-panel?style=flat-square)
![GitHub package.json version](https://img.shields.io/github/package-json/v/swoga/vue-collapsible-panel?style=flat-square)

# @swoga/vue-collapsible-panel
Easy to use, customisable collapsible panel / accordion library built using Vue3.  
This is a fork of [dafcoe/vue-collapsible-panel](https://github.com/dafcoe/vue-collapsible-panel).
<br>
See it action on [demo](https://swoga.github.io/vue-collapsible-panel) page.

## Installation
Install the package as a project dependency using `yarn` or `npm`:
```
$ yarn add @swoga/vue-collapsible-panel

--- or ---

$ npm install --save @swoga/vue-collapsible-panel
```

## Usage
Import `VueCollapsiblePanelGroup` and `VueCollapsiblePanel` components, either globally (on your main.js / main.ts file) or locally (on your component):
```js
// Globally
import VueCollapsiblePanel from '@swoga/vue-collapsible-panel'
const app = createApp(App)
app.use(VueCollapsiblePanel).mount('#app')

// Locally
import {
  VueCollapsiblePanelGroup,
  VueCollapsiblePanel,
} from '@swoga/vue-collapsible-panel'
```

Import default styles (optional - you can define your own styling):
```js
import '@swoga/vue-collapsible-panel/dist/vue-collapsible-panel.css'
```

Use it in the template as follows:
```html
<vue-collapsible-panel-group>
    <vue-collapsible-panel>
        <template #title>
            Panel A Title
        </template>
        <template #content>
            Panel A Content
        </template>
    </vue-collapsible-panel>
    <vue-collapsible-panel>
        <template #title>
            Panel B Title
        </template>
        <template #content>
            Panel B Content
        </template>
    </vue-collapsible-panel>
    ...
</vue-collapsible-panel-group>
```

## Options
#### Accordion
By default, the accordion behaviour is not used. To use it, add the `accordion` on `vue-collapsible-panel-group`:
```html
<vue-collapsible-panel-group accordion>
    ...
</vue-collapsible-panel-group>
```

#### Expanded
By default, all the panels are expanded. To prevent this, add `:expanded="false"` on `vue-collapsible-panel`:
```html
<vue-collapsible-panel-group>
    <vue-collapsible-panel :expanded="false">
        <template #title>
            Panel A Title
        </template>
        <template #content>
            Panel A Content
        </template>
    </vue-collapsible-panel>
    ...
</vue-collapsible-panel-group>
```

#### Colors
The default base hex color is `#333`. To change this, add `base-color="#<color>"` on `vue-collpasible-panel-group`:
```html
<vue-collapsible-panel-group base-color="#0035a0">
    ...
</vue-collapsible-panel-group>
```
For granular control over the colors, the following css variables can be overridden:
````css
--base-color
--border-color
--bg-color-header
--bg-color-header-hover
--bg-color-header-active
--bg-color-body
````

## License
[MIT License](https://opensource.org/licenses/MIT) © [Daf Coe](mailto:dafcoe@gmail.com), swoga
