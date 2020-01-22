## Fix for React Native apps using Ziggeo on Web

If you are using `react-ziggeo` node module on for web code that also needs to run on native mobile using React Native, you'll run into problems.

This module allows you to create a placeholder and then using `webpack.config` you can create an alias module to the real `react-ziggeo` while your mobile build has no idea :)

## Example

Make sure you run `npm install react-ziggeo` so it's in your node_modules, but don't reference it directly in `SomeComponent.tsx`. Instead:

```js
// SomeComponent.tsx
import { ZiggeoRecorder } from 'react-ziggeo-mobile-friendly';
import { RegularMobileCamera } from '...';
const Camera = Platform.OS === 'web' ? ZiggeoRecorder : RegularMobileCamera;
//... profit .../

// webpack.config
config.resolve.alias['react-ziggeo-mobile-friendly'] = path.resolve(
    __dirname,
    './mocked_modules/react-ziggeo-mobile-friendly'
);
  
// ./mocked_modules/react-ziggeo-mobile-friendly/index.js
import * as Ziggeo from 'react-ziggeo';
module.exports = Ziggeo;
```