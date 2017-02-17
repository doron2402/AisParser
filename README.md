# AisParser
A Parser for NMEA0183  AIS messages.
The parser is written using [flow](https://flowtype.org/). It can be run from the src directory with babel-node or in the transpiled version from the lib directory. The code should get transpiled in install (I am working on that currently) otherwise it can be transpiled calling ```npm install``` and ```npm run-script prepublish```.

Usage:
```javascript
var parser = new AisParser({ checksum : true });

var sentences = [
  '!AIVDM,1,1,,B,14`c;d002grD>PH50hr7RVE000SG,0*74',
  '$GPVTG,222.30,T,,M,0.30,N,0.6,K,A*09',
  '!AIVDM,1,1,,B,14`a`N001WrD12J4sMnWpV8l2<4`,0*0D',
  '!AIVDM,1,1,,B,15Bs:H8001JCUE852dB<FP1p2PSe,0*54',
  '!AIVDM,1,1,,B,3DSegB1uh2rCs6b54VuG417b0000,0*7C'
]

sentences.forEach(function(sentence) {
  var result = parser.parse(sentence);
  switch(result.valid) {
    case 'VALID':
      console.log('values for message:' + sentence);
      result.supportedValues.forEach(
        function(field) {
          console.log(' ' + field + ':' + result[field] +
                      ' ' + result.getUnit(field));
        });
      break;
    case 'UNSUPPORTED':
      console.log('unsupported message :' + sentence);
      console.log('error message: :' + result.errMsg);
      break;
    case 'INVALID':
      console.log('invalid message :' + sentence);
      console.log('error message: :' + result.errMsg);
      break;
    case 'INCOMPLETE':
      console.log('incomlete message, waiting for more');
      break;
    }
  });
```
Test:
