NoFlo Node.js runtime environment
=================================

This tool is designed to be used together with the [Flowhub](http://flowhub.io/) development environment for running [NoFlo](http://noflojs.org/) networks on [Node.js](http://nodejs.org/).

## Prepare a project folder

Start by setting up a local NoFlo Node.js project. For example:

```shell
$ mkdir my-project
$ cd my-project
$ npm init
$ npm install noflo --save
$ npm install noflo-nodejs --save
```

Continue by installing whatever [NoFlo component libraries](http://noflojs.org/library/) you need, for example:

```shell
$ npm install noflo-core --save
```

If you want, this is a great time to push your project to [GitHub](https://github.com/).

## Initializing the runtime configuration

The next step is to create the runtime configuration, which will be stored in the `flowhub.json` file in your project folder. Usually you don't want to add this file to version control.

You can either create the file by hand, or by the provided `noflo-nodejs-init` tool. See the included help information:

```shell
$ node node_modules/.bin/noflo-nodejs-init --help
```

You can also run node node_modules/.bin/noflo-nodejs and provide most of the parameters on the commandline to run without flowhub registration.

### Host address autodetection

There are situations where the IP address of the runtime will change. For example, when you're running a NoFlo runtime on a [Raspberry Pi](http://www.raspberrypi.org/) that you carry between home and the office. This runtime tool has rudimentary support for IP address autodetection.

To enable that, run:

```shell
$ node node_modules/.bin/noflo-nodejs-init --host autodetect
```

After this the runtime will report its current IP address to your Flowhub UI whenever it is started.
Autodetection will prefer external interfaces over local one (e.g. the loopback interface).
If you prefer a certain network interface you can use "autodetect(<iface>)" e.g.
"autodetect(wlan0)" to check that interface if it is online. Again you can also use the
--host option on node_modules/.bin/noflo-nodejs for unregistered usage.

### Example flowhub.json

`$ node node_modules/.bin/noflo-nodejs-init` will initialize `flowhub.json` with the `id` of the runtime. The `user` id comes from Flowhub > Home > Runtimes > Register. You'll need some more info to make the registration useful:
```json
{
  "id": "runtime-id-...",
  "user": "user-id-...",
  "host": "autodetect",
  "port": 3569,
  "label": "my-runtime"
}
```

To generate this file, you could run:

```shell
$ node node_modules/.bin/noflo-nodejs-init --host autodetect --port 3569 --user user-id --label "my-runtime"
```

In this case the UUID for the runtime (shown as `runtime-id` in the JSON above) would be autogenerated. If you want to reuse a previous one, you can provide it with the `--id some-UUID` option.

## Starting the runtime

Once you have configured the runtime, it is time to start it:

```shell
$ node node_modules/.bin/noflo-nodejs
```

This will start a WebSocket-based NoFlo Runtime server, and register it to your Flowhub account.
It should now become available in your Flowhub UI. By default the configuration will be read from the current working directory,
but you can change this by setting the `PROJECT_HOME` environment variable.

## Starting an existing graph

If you want to run an existing graph, you can use the `--graph` option.

    noflo-nodejs --graph graphs/MyMainGraph.json

If you want the process to exit when the network stops, you can pass `--batch`.


## Usage without Flowhub registration

If you don't want to register the runtime for your Flowhub account (for example, with temporary runtimes used for testing),
you can skip this by starting noflo-nodejs with the `--register=false` flag.
