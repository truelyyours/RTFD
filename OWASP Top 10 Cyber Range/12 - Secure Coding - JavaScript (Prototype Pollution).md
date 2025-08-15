# Exploit hoek

In _lab_ directory, there is a _package.json_ file. It installs an outdated version of _hoek_.Lets try to exploit the prototype pollution in it. Open the _hoek.js_ file, with the following command:
`nano hoek.js

The _Hoek_merge_ method is vulnerable to prototype pollution. It means that if an object is passed to it as a second argument, it will overwrite properties on the object passed as first argument recursively.

By abusing the _"**proto**"_ property on JavaScript object, try to overwrite the value of _a.hasOwnProperty_ by just changing the value of the _malicious_payload_ string.The new value of _a.hasOwnProperty_ should be _true_.To modify the script run the following commands:

`const malicious_payload= '{ "__proto__": { "hasOwnProperty": true } }';`

To execute the script, enter the following command:
`node hoek.js
# Exploit lodash

Now let's do the same thing in _lodash.js_:
`nano lodash.js

Here, we won't abuse the _"**proto**"_.This version of _lodash_ is vulnerable but through another path.We will try to make the second console log print _true_.To do this, enter the following commands:
`const malicious_payload = { hasOwnProperty: true };

And change the __.merge{}_ line with the following line:
`Map.prototype.hasOwnProperty = malicious_payload.hasOwnProperty;

Run the command to execute the script:
`node lodash.js