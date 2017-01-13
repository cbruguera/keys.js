This is a JavaScript client library to facilitate interaction with an [Eris Keys](https://github.com/eris-ltd/eris-keys) server.

Note:  Eris Keys does not expose an interface to get private keys.

# Installation

`$ npm install eris-keys`

# Usage

First start a key server:

`$ eris services start keys --publish`

```JavaScript
'use strict'

const assert = require('assert')
const keys = require('../../lib')

describe('a client for eris-keys', function () {
  it('generates a key, signs a message, and verifies the signature',
    function () {
      this.timeout(10 * 1000)

      // Open a connection to the server.
      return keys.serviceUrl('services', 'keys', 4767).then((url) => {
        const server = keys.open(url)

        // Generate a new key pair.
        return server.generateKeyPair().then((keyPairId) => {
          const message = 'a message in a bottle'

          return Promise.all([
            // Get the public key of the key pair.
            server.publicKeyFor(keyPairId),

            // Sign the message.
            server.sign(message, keyPairId)
          ]).then(([publicKey, signature]) =>
            server.verifySignature(message, signature, publicKey)
              .then((valid) => {
                assert(valid)
              })
          )
        })
      })
    })
})
```

# Documentation

You can generate documentation in the `doc` subdirectory with the command `npm run doc`.

# Copyright

Copyright 2016 Monax

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.
