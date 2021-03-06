# @nib-components/react-html

A common layout component for use in nib apps.

**Features:**

- metadata including; title
- Includes Modernizr (for the `flexbox`/`no-flexbox` classes required by the [grid](https://github.com/nib-styles/sass-grid))
- Includes nib fonts
- Includes multiple style and script files (they may be revisioned)
- Serialises and passes app config to the client
- Serialises and passes `redux` state to the client
- New Relic monitoring
- Google Tag Manager
- Visual Website Optimizer


## Installation

    npm install -S @nib-components/react-html

## Usage

```javascript

import express from 'express';
import React from 'react';
import {renderToStaticMarkup} from 'react-dom/server';
import createHtml from '@nib-components/react-html';

const app = express();

const Html = createHtml({

    config,

    script: ['vendor.js', 'client.js'],
    style: ['client.css'],

    revManifestPath: {
      prefix: '/health-insurance/quote/public'
    },

    newRelic: {
      licensekey: process.env.NR_KEY,
      applicationId: process.env.NR_ID
    },

    googleTagManagerId: process.env.GTM_ID || 'GTM-XXXXXX',
    visualWebsiteOptimizer: process.env.VWO_ENABLED || false

});

app.get('/', (req, res) => {
  res.send(renderToStaticMarkup(
    <Html>
      <h1>Hello World!</h1>
    </Html>
  ));
});

```

> Note: This is an overly simple example, realistically you might want to use it in conjunction with something like [rechannel](https://www.npmjs.com/package/rechannel).

## API

```javascript
createHtml(options : object) : React.Component
```

Creates a React component for rendering generic meta-data and layout.

**Options:**

- `static : boolean` Optional. Render the `children` with `ReactDOM.renderToStaticMarkup()` instead of `ReactDOM.renderToString()`.  Defaults to `false`.
- `title : string` Optional. The page title. Uses `react-helmet` if unspecified.
- `description : string` Optional. The meta description.
- `canonical : string` Optional. The canonical URL.
- `script : string|array` Optional. The path(s) of a script or an array of scripts. Defaults to `'index.js'`.
- `style : string|array` Optional. The path(s) of a style or an array of styles. Defaults to `'index.css'`.
- `config : object` Optional. The application config which will be passed to the client loaded at `window.__CONFIG__`.
- `newRelic : object` Optional. The New Relic configuration. Determines whether the New Relic script is enabled.
  - `licenseKey : string` Required. The license key.
  - `applicationId : string` Required. The application ID.
- `visualWebsiteOptimizer : boolean` Optional. Whether the VWO script is enabled. Defaults to `false`.
- `googleTagManagerId : string` Optional. The GTM ID. Determines whether the GTM script is enabled.

**Returns:**

A React component.