## Today's npm package: bwip-js

Today's npm package is [bwip-js](https://www.npm/js.com/package/bwip-js)

Text based input from human to computer is one way we can introduce real world to the Computer applications.

Printed data like Barcodes allow faster input of data via Optical data reading.

BwipJS (Barcode Writer in Pure JavaScript) is  a JS implementation of [Barcode Writer in Pure JavaScript](https://github.com/bwipp/postscriptbarcode) and allows usage as a JS lib, CLI etc.

Let's see how it works:

### QR Code

```js
const bwipjs = require('bwip-js');

const pngFromBase64 = (data) => `data:image/png;base64,${data}`;

bwipjs.toBuffer({
  bcid: 'qrcode', // Barcode type
  text: '0123456789', // Text to encode
  scale: 6, // 6x scaling factor
  textxalign: 'center', // Always good to set this
}, (err, data) => {
  if (err) {
     throw err;
  }
  
  const bufferBase64 = data.toString('base64');

  const img = `<img src="${pngFromBase64(bufferBase64)}" />`;
  
  console.log(img);
});
```

Or same code as promise:
```js
const bwipjs = require('bwip-js');

const pngFromBase64 = (data) => `data:image/png;base64,${data}`;

bwipjs.toBuffer({
  bcid: 'qrcode', // Barcode type
  text: '0123456789', // Text to encode
  scale: 6, // 6x scaling factor
  textxalign: 'center', // Always good to set this
}).then((data) => {  
  const bufferBase64 = data.toString('base64');

  return `<img src="${pngFromBase64(bufferBase64)}" />`;
}).catch(err => (throw err));
```

![qrcode.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624208144405/BtYtEGoQU.png)

See it in action here: https://runkit.com/pankaj/bwip-js

%[https://runkit.com/pankaj/bwip-js]

### Barcode
```
const bwipjs = require('bwip-js');

const pngFromBase64 = (data) => `data:image/png;base64,${data}`;

bwipjs.toBuffer({
  bcid: 'code128', // Barcode type
  text: '0123456789', // Text to encode
  scale: 6, // 6x scaling factor
  textxalign: 'center', // Always good to set this
}, (err, data) => {
  if (err) {
     throw err;
  }
  
  const bufferBase64 = data.toString('base64');

  const img = `<img src="${pngFromBase64(bufferBase64)}" />`;
  
  console.log(img);
});
```

![barcode.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1624208152749/p1PfDPIdc.png)

See the barcode in action here: https://runkit.com/pankaj/bwip-js-barcode

%[https://runkit.com/pankaj/bwip-js-barcode]

Stay tuned for tomorrow's package of the Day.
