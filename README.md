# Instructions to create a new plugin

The process to create a new `plugin` able to manage a new capability for the `halkyon` operator is pretty forward 
and consists in a few steps :

- Git clone the project `https://github.com/halkyonio/plugin-example-capability` and rename it 
- Rename the `module name` as defined within the `go.mod` file to use your package name
- Find and replace the `example` word with the name of your `plugin`, resource, ...
- Build and deploy it under the local `plugins` folder of the `halkyon` operator to test it

The plugin allows you to encapsulate 3rd party go libraries, your dependent kubernetes resources
and to generate a secret by example containing the parameters provided by the Capability.

## Build the plugin

To build the plugin, execute the following command within a terminal
```bash
go build -o example-plugin ./cmd/example-capability/plugin.go
```
Next, copy/paste the `example-capability` binary file generated by the build under the `plugins` folder of the `halkyon` operator project cloned locally
and launch the Operator. The plugin will be automatically loaded.
```bash
2020-02-13T16:24:04.531+0100	INFO	cmd	Go Version: go1.13.7
2020-02-13T16:24:04.531+0100	INFO	cmd	Go OS/Arch: darwin/amd64
2020-02-13T16:24:04.531+0100	INFO	cmd	Version of operator-sdk: v0.8.2
2020-02-13T16:24:04.531+0100	INFO	cmd	halkyon-operator version: Unset
2020-02-13T16:24:04.531+0100	INFO	cmd	halkyon-operator git commit: HEAD
2020-02-13T16:24:04.531+0100	INFO	cmd	watching namespace test
2020-02-13T16:24:06.315+0100	INFO	cmd	Registering Halkyon resources
2020-02-13T16:24:06.315+0100	INFO	cmd	Registering 3rd party resources
2020-02-13T16:24:06.316+0100	INFO	cmd	Loading plugins
2020-02-13T16:24:06.316+0100 [DEBUG] example-plugin: starting plugin: path=/Users/dabou/Code/halkyon/operator/plugins/example-plugin args=[/Users/dabou/Code/halkyon/operator/plugins/example-plugin]
2020-02-13T16:24:06.320+0100 [DEBUG] example-plugin: plugin started: path=/Users/dabou/Code/halkyon/operator/plugins/example-plugin pid=29894
2020-02-13T16:24:06.320+0100 [DEBUG] example-plugin: waiting for RPC address: path=/Users/dabou/Code/halkyon/operator/plugins/example-plugin
2020-02-13T16:24:06.411+0100 [DEBUG] example-plugin.example-plugin: 2020-02-13T16:24:06.411+0100 [INFO ] example-plugin.PluginResource: calling GetSupportedCategory
2020-02-13T16:24:06.411+0100 [DEBUG] example-plugin.example-plugin: 2020-02-13T16:24:06.411+0100 [INFO ] example-plugin.PluginResource: calling GetSupportedTypes
2020-02-13T16:24:06.411+0100 [DEBUG] example-plugin.example-plugin: 2020-02-13T16:24:06.411+0100 [DEBUG] example-plugin: plugin address: network=unix address=/var/folders/56/dtp67r4n1hv79q2hrh_dbwcc0000gn/T/plugin731069597
2020-02-13T16:24:06.412+0100 [DEBUG] example-plugin: using plugin: version=1
```

## Debug

- If you plan to debug the `rpc` plugin, simply add the following compilation parameters to the `go build` command
  ```bash
  go build -gcflags="all=-N -l" -o example-plugin ./cmd/example-capability/plugin.go
  ```

- Next, start the Operator in Debug mode. During the launch of the operator, the plugin will be loaded and the RPC server started.
- Use the `Attach to Process` debug option of your IDE to start within this project a debugger.
- Add a breakpoint within the file that you would like to watch and deploy a capability
  ```bash
  kubectl apply -f deploy/example-capability.yml
  ```
- Enjoy !  

## Release 

To release your plugin, configure your github project to use the `.github/workflows/main.yml` file responsible to perform
the `go releases` using the configuration of the file `.goreleaser.yml`. 
You can find more information about the goreleaser action [here](https://github.com/goreleaser/goreleaser-action).

