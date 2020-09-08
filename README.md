# emblem-generator-vue

Vue component with UI for [Emblem Generator](https://github.com/kiplin-dev/emblem-generator).

## Demo

https://www.pochworld.com/emblem-generator-vue/

## Installation

- NPM

`npm install emblem-generator-vue`

- Yarn

`yarn add emblem-generator-vue`

## Usage

Import `EmblemGenerator` component from node_modules and one of the following themes:
  - light-theme.css
  - dark-theme.css

Or you can create your own custom theme.

```javascript
<template>
  <div>
    <EmblemGenerator
      :assets="assets"
      :emblemData="emblemData"
    />
  </div>
</template>  

<script>
import { EmblemGenerator } from 'emblem-generator-vue'
import './node_modules/emblem-generator-vue/css/light-theme.css';

export default {
  name: 'my-component',
  components: {
    EmblemGenerator
  },
  data() {
    return {
      assets: {
        // See assets section below
      },
      emblemData: {
        // See emblemData section below
      },
    }
  }
}

</script>
```

### Assets
Assets are SVGs and colors definitions that will be used by the EmblemGenerator to generate emblems.

Its a javascript object that looks like that:
```javascript
const assets = {
  defs: {}, // foregrounds
  bg_defs: {}, // backgrounds
  color_defs: {} // colors
}
```

You can take a look at `src/customAssets.js` and `src/customColors.js` to see how they are defined.

To use these default assets, for example, you can do this:
```javascript
<script>
...

import assets from './customAssets';
import colors from './customColors';

...

data() {
  return {
    assets: {
      defs: assets.defs,
      bg_defs: assets.bg_defs,
      color_defs: colors,
    }   
  }
}

...
```


#### Foregrounds and Backgrounds

To use custom foregrounds and backgrounds, you will need to use the AssetGenerator provided by `emblem-generator` module.

Add the SVG files you want to use in `some-folder/backgrounds` and `some-folder/emblem` respectively.

NOTE: Check the specifications for the SVG files in the README of `emblem-generator` module

Then generate the assets with this command:

`node node_modules/emblem-generator/util/AssetGenerator.js some-folder some-output-folder`

You'll find the generated assets in the `some-output-folder` directory.


### EmblemData

EmblemData is an object that contains the options to generate an emblem.
It contains the IDs of the selected foregrounds, backgrounds, colors and flip flags.

example:
```javascript
<script>
...

data() {
  return {
    emblemData: {
      background_id: 1,
      foreground_id: 2,
      flags: [],
      background_color_id: 3,
      foreground_primary_color_id: 4,
      foreground_secondary_color_id: 5,
    }   
  }
}

...
```
The IDs are those defined in the assets files.

If you pass an empty object, it will generate a random emblem.

## Options

You can pass some props to EmblemGenerator to customize the usage.

Most of the props are to override the default texts:

| Props | Type | Default value | Description |
| ------ | ---- | ------------- | ----------- |
| backgroundTxt | String | Background | Text for the background. |
| foregroundTxt | String | Foreground | Text for the foreground. |
| primaryColorTxt | String | Primary Color | Text for the primary color. |
| secondaryColorTxt | String | Secondary Color | Text for the secondary color. |
| flipVerticalTxt | String | Flip Vertically | Text for the flip vertically. |
| flipHorizontalTxt | String | Flip Horizontally | Text for the flip horizontally. |
| randomizeTxt | String | Randomize | Text for the randomize. |
| generatingTxt | String | Generating... | Text for the generating. |
| :useBackground | Boolean | true | Use background. Set this option to `false` to use only the foreground. |
| :displayFlip | Boolean | true | Display flip options.  Set this option to `false` to hide them.|
| :displayGeneratingLoader | Boolean | true | Generates a random emblem when an empty `emblemData` object is pass to the EmblemGenerator. For a better user experience, we add a fake 2 seconds loader so that user understand that the emblem he will see is a random generated one. Set this option to `false` to skip this feature. |

Example:
```javascript
<EmblemGenerator
  :assets="assets"
  :emblemData="emblemData"
  backgroundTxt="My custom text"
  foregroundTxt="My custom text"
  primaryColorTxt="My custom text"
  secondaryColorTxt="My custom text"
  flipVerticallyTxt="My custom text"
  flipHorizontallyT="My custom text"
  randomizeTxt="My custom text"
  generatingTxt="My custom text"
  :useBackground="false"
  :displayFlip="false"
  :displayGeneratingLoader="false"
/>
```

## EmblemDisplayer

As we pass the `emblemData` object to the EmblemGenerator component, when generating the emblem, the parent will get the data of the corresponding emblem into this object.

You can then do whatever you want with this data, for example store it in a database.

Then if you want to display your emblem from this data, you can use the EmblemDisplayer component:

```javascript
<template>
  <div>
    <EmblemDisplayer
      :assets="assets"
      :emblemData="emblemData"
    />
  </div>
</template>  

<script>
import { EmblemDisplayer } from 'emblem-generator-vue'

export default {
  name: 'my-component',
  components: {
    EmblemDisplayer
  },
  data() {
    return {
      assets: {
        // See assets section above
      },
      emblemData: {
        // See emblemData section above
      },
    }
  }
}

</script>
```

You can pass a `width` props to define the size of the displayed emblem (the emblem is a square).

To be able to display different emblems in the same page, the divs where the emblems are generate must have a different id. To achieve this, a random id is generate for the containing divs. But if you need to have a specific id (for CSS purpose or something else) you can pass a `divId` props.

Example:
```javascript
<EmblemDisplayer
  :assets="assets"
  :emblemData="emblemData"
  :width="512"
  divId="customId" // The generated id will be "emblem-displayer-customId"
/>
```

### Authors

Benoît Ripoche - Kiplin
benoit.ripoche@kiplin.com

Aurélien Ruiz-Minano - Kiplin
aurelien.ruiz-minano@kiplin.com
