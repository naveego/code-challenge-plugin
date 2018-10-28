# Code Challenge: Toy Plugin

In this code challenge you will construct a simplified version of the kind of plugin that Naveego uses to discover and collect data. The plugin will be responsible for discovering and collecting data from CSV files.

This challenge is intended to be a realistic test of your ability
to write production-ready code given a description of the problem.
Your solution will be assessed based on

### Scenario

Our plugin framework uses gRPC for communication. In reality we use the Hashicorp [go-plugin](https://github.com/hashicorp/go-plugin) framework, but
for this challenge we will just use plain gRPC.

Using a language with [gRPC support](https://grpc.io/docs/), you must produce a CLI app which will satisfy the plugin protocol defined below.

This directory contains a fake plugin host which will run your plugin. The
host is a CLI app which you can run using Go (Go 1.11 required, see here: https://golang.org/doc/install)

```
go run host.go {command to start your program}
```

The host will run your app using the command you pass to it, and will exercise it by making calls over gRPC.

### Plugin Protocol

The plugin must claim a port and start a gRPC server on that port. After claiming the port,
the plugin must write the port number to stdout, followed by a carriage return. The plugin
must not write anything else to stdout before writing the port number. After it writes the port 
number, it can emit logging messages to stdout or stderr.

The plugin should allow insecure connections.

The plugin must stop streaming data and exit with code 0 if it receives an
SIGINT or SIGKILL.

The gRPC server the plugin starts must fulfil the contract defined in [./plugin.proto](./plugin.proto). The host will first call the Discover service and will expect to get back a listing of schemas. Then it will
call the Publish service for each schema and will expect to be streamed
the data from the files for that schema.

For details about the contract, see the comments in [./plugin.proto](./plugin.proto).
