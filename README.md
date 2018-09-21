# node-red-contrib-later
A node red node for triggering using [later.js](http://bunkat.github.io/later/index.html).

## What’s new
* Added function to cancel all timers

## Installation
```bash
$cd
$git clone https://github.com/pauldeng/node-red-contrib-later.git
$cd .node-red/
$npm install ~/node-red-contrib-later/
$cd
```
## Usage

  This node accepts a `msg` and sends it on according to a schedule defined by a string parsed through `later.parse.text()`. In all cases the schedule string in the node schedule string property is used if set.

  If the schedule fails to parse `node.warn()` is called with details of the string and error as reported by the parser, and the `msg.payload` is sent on immediately.

  If the schedule is empty, the `msg.payload` is passed straight through.

  To cancel a running schedule pass in a message with an empty payload with the `msg.later` property from a previously sent sceduled message.

  To cancel all running schedules, pass in a message with an empty payload.

### Payload as an object

  If the payload is an object the property `msg.payload.later` is used as the schedule string if the node doesn't have a schedule property set.

## Examples

  See the [documentation of later.parse.text()](http://bunkat.github.io/later/parsers.html#text) for the format of the schedule string.

  `every 10 seconds` or `at 10:15 am` are simple examples.

### Example flow files

  Try pasting in the flow file below that sends the message every 10th second twice.

  ```json
  [{"id":"b9995d6c.4666a","type":"later","name":"The schedule","schedule":"every 10 seconds","x":384,"y":129,"z":"80e3898c.7f1c78","wires":[["b8f9cfce.47063","4a71efdd.b58e1"]]},{"id":"b9335ee3.46cca","type":"inject","name":"","topic":"","payload":"comes out later","payloadType":"string","repeat":"","crontab":"","once":true,"x":124,"y":130,"z":"80e3898c.7f1c78","wires":[["b9995d6c.4666a"]]},{"id":"b8f9cfce.47063","type":"debug","name":"","active":true,"console":"false","complete":"true","x":620,"y":129,"z":"80e3898c.7f1c78","wires":[]},{"id":"4a71efdd.b58e1","type":"function","name":"Run schedule 2 times","func":"if (msg.later.count >= 2) {\n    delete msg.payload; \n    return msg;\n} else {\n    return;\n}\n","outputs":1,"valid":true,"x":382,"y":248,"z":"80e3898c.7f1c78","wires":[["b9995d6c.4666a"]]}]
  ```

  This flow does essentially the same, but uses a schedule sent in as a property in `msg.payload` as an object.

  ```json
  [{"id":"e28ebddd.1d714","type":"inject","name":"","topic":"","payload":"{\"later\":\"every 10 sec\"}","payloadType":"string","repeat":"","crontab":"","once":true,"x":115,"y":196,"z":"cb2e612f.34d1a","wires":[["64e96677.9b1698"]]},{"id":"64e96677.9b1698","type":"json","name":"","x":262,"y":196,"z":"cb2e612f.34d1a","wires":[["507261e9.af8da"]]},{"id":"507261e9.af8da","type":"later","name":"","schedule":"","x":436,"y":196,"z":"cb2e612f.34d1a","wires":[["d8bf725e.27409","db2fa266.24d06"]]},{"id":"d8bf725e.27409","type":"debug","name":"","active":true,"console":"false","complete":"false","x":593,"y":196,"z":"cb2e612f.34d1a","wires":[]},{"id":"db2fa266.24d06","type":"function","name":"Run schedule 2 times","func":"if (msg.later.count >= 2) {\n    delete msg.payload; \n    return msg;\n} else {\n    return;\n}\n","outputs":1,"valid":true,"x":442,"y":304,"z":"cb2e612f.34d1a","wires":[["507261e9.af8da"]]}]
  ```

  This flow below shows how to set a schedule, cancel all schedules and set new schedule.
  ```json
  [{"id":"369c5e41.915df2","type":"inject","z":"f5db7e61.e22758","name":"","topic":"","payload":"{\"later\":\"every 1 minute\"}","payloadType":"str","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":331,"y":395,"wires":[["c01613a5.ac1d9"]]},{"id":"c01613a5.ac1d9","type":"json","z":"f5db7e61.e22758","name":"","property":"payload","action":"","pretty":false,"x":501,"y":315,"wires":[["e98e17a0.bde9a"]]},{"id":"e98e17a0.bde9a","type":"later","z":"f5db7e61.e22758","name":"","schedule":"","x":671,"y":315,"wires":[["33bcea6.b262296"]]},{"id":"33bcea6.b262296","type":"debug","z":"f5db7e61.e22758","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":851,"y":315,"wires":[]},{"id":"4be6be86.04f38","type":"inject","z":"f5db7e61.e22758","name":"Cancel all timers","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":121,"y":235,"wires":[["d921bf59.289908"]]},{"id":"d921bf59.289908","type":"function","z":"f5db7e61.e22758","name":"del payload","func":"delete msg.payload;\nreturn msg;","outputs":1,"noerr":0,"x":311,"y":235,"wires":[["c01613a5.ac1d9"]]}]
  ```

## Author

  - Dean Sellers
  - Paul Deng

## License

The MIT License (MIT)

Copyright (c) 2015 Lighthouse Automation &lt;dean@lighthouseautomation.com.au&gt;

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
