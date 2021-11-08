# Take home exercise - Sniffing Service

## Prerequisites
It is highly recommended to go through the getting [started page](https://github.com/mage-io/documentation/blob/main/getting-started.md) before jumping to this exercise.

## Requirements
This exercise will be divided into five parts, and each individual will be evaluated based upon how well they perform in each step.
The goal of this exercise will be to build a sniffing service which would read any request it receives and emits metrics for the same. We will use Prometheus to aggregate and visualize these metrics.

## Some Useful Tips
1. Always code to an interface, wherever possible. This allows us to have better test coverage for codebase and have mutually detacheable items. If you do not understand what this means, Take out some time to go through [this](https://medium.com/rungo/interfaces-in-go-ab1601159b3a).
2. Follow the strategy of "accept interfaces, return structs" in your code wherever possible. Read more about it if you aren't sure what this means.
3. It is important to follow the branching structure mentioned in the steps below (and the squash step) as it allows the reviewer to review every commit as an individual entity, rather than a plethora of code.
4. Document whatever isn't obvious. Anything which is specific to a step (like setting some config for redis, or how to run the server, etc) must be documented in `Part 5`.


### Part 1 - Initialize empty repository
Initialize an empty repository in your space on Github. Name this repository as `cache-service`. The repository must be initialized with an Apache license and a readme file.

Going forward, every part must be committed to the repository in its own commit - one commit per part. In order to do that, it is recommended to use a branching strategy with git, and merge your branch to master when you believe the section is complete.

### Part 2 - Creating RPC service
For this section, you'll have to use `protocol buffers` and `protobuf generators`. If you are not aware of what these words mean, please have a look here: [Link](https://developers.google.com/protocol-buffers/docs/reference/go-generated).

* Create a branch named `init-rpc-service`.
* Create an RPC service which acts as an interface for receiving and returning user requests. The interface must be built using protobuf and gRPC, and should be able to support the following methods:
    ```
    // Function to showcase metrics integration for the service
    func Increment(ctx context.Context, name string) error
    ```
    Note that the interface can be tweaked a little to support the actual application, provided the service is appropriately documented.
* Generate the server and client stub using protobuf generator.
* Return hardcoded response for `Increment`:
    ```
    // Increment Method response
    func Increment(ctx context.Context, name string) error {
        return errors.New("not implemented yet. <yourname> will implement me")
    }
    ```
* Create an RPC client and store this in a separate `client` folder. Run the server on a port and use the generated RPC client code to read response from the server.
   Check [this](https://grpc.io/docs/languages/go/basics/) for hints on how you can use protobuf for the requirements above.
* Once you are satisfied with your changes, squash all your commits in the branch and merge it to master (Very important to squash commits, as it will help for better evaluation and review).

### Part 3 - Implementing Metric emission
In this section, we will implement the method created above and actually allow users to emit metrics for RPC method.
We will be using `Prometheus` integration to support this. If you don't understand what prometheus is, don't worry! There is a detailed documentation by prometheus team regarding the same. [Link](https://prometheus.io/docs/introduction/overview/) 
* Create another branch named `prometheus-impl`.
* Install and setup prometheus on your local. Use [this](https://prometheus.io/docs/prometheus/latest/getting_started/) to set this up quickly and run it on your system. Prometheus must be configured to run on port `9090` (default port).
* Tweak the gRPC server to emit metrics for every request received. The server must have the capability to emit historgrams, counters, errors and any other metrics available. Use [this](https://github.com/grpc-ecosystem/go-grpc-prometheus) to help yourself in doing the same.
* Update prometheus scrape target to make sure it is scraping your http endpoint exposed by the gRPC service. Query the prometheus' `graph` dashboard to check if the metrics are being scraped (check the getting started doc on the top for instructions).
* Once satisfied with the changes, SQUASH all commits into one and merge the branch to master.

### Part 4 - Creating custom metrics endpoint
The sections so far have navigated and helped in setting up simple metric emission and capture clients. Now, we will create custom RPC methods catered to capturing metrics based on given inputs.

* Create a branch named `custom-metrics-endpoint`.
* Update the `Increment` rpc method to have the following contract:
    ```
    func Increment(ctx context.Context, name string, tags []string, val int64) error
    ```
    * If the val is passed as 0, treat it as 1. Else, use the value passed in the input.
    * The input is used as a counter which is supplied as custom metric to prometheus. Check [here](https://prometheus.io/docs/guides/go-application/) for a tutorial on how to achieve this. 
    * The logic works by checking if there is a counter already created for the given name. If not, it creates a new counter for the metrics and starts keeping a track of these new metrics.

*  Make sure the custom metrics are being emitted by making a call to the RPC service using the client created. Capture screenshots for this as it would be required for documentation.
* Once satisfied with code, SQUASH all changes and merge to master.

### Part 5 - Documentation
As with every production ready codebase, provide a small documentation on how to run the service and test it locally. The steps must be reproducible and should be written in a way so that any evaluator is able to execute the setup locally. Do not include how to setup go, protobuf, redis, etc).

Make the access for the repository public and reach out to the interviewer with a link to github repository once done. It would also be useful to provide the following information in the documentation:
1. Operating System used for Development (mac, windows, linux (recommended)).
2. `GOPATH` and repository path on local.

Additionally, add a few screenshots from prometheus which you think are relevant to every step, and include them along with the documentation in your repository.
