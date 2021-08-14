# Take home exercise - Orchestrator Service

## Prerequisites
It is highly recommended to go through the getting [started page](main/getting-started.md) before jumping to this exercise.

## Requirements
* This exercise will be divided into five parts, and each individual will be evaluated based upon how well they perform in each step.
* The exercise is expected to be written in golang, which is a pretty easy to learn language developed by google, and adopted by the fast-growing tech industry. If you have never worked with go before, it is recommended to go through the prerequisites before jumping into the exercise.
* The goal of this exercise will be to build an orchestrator service which would read any request it receives and forwards it to other orchestrator services and data services.
* The final output of the exercise must look like:
    ```
    client ---RPC--> orchestrator_1(:9000) ---RPC--> orchestrator_2(:9001) ---RPC--> mock_data_service(:10000)
    ```
    Do not worry a lot about what these mean, as it would be explained in the exercise below, but it is important to keep the higher picture in mind while implementing the code below.

## Some Useful Tips
1. Always code to an interface, wherever possible. This allows us to have better test coverage for codebase and have mutually detacheable items. If you do not understand what this means, Take out some time to go through [this](https://medium.com/rungo/interfaces-in-go-ab1601159b3a).
2. Follow the strategy of "accept interfaces, return structs" in your code wherever possible. Read more about it if you aren't sure what this means.
3. It is important to follow the branching structure mentioned in the steps below (and the squash step) as it allows the reviewer to review every commit as an individual entity, rather than a plethora of code.
4. Document whatever isn't obvious. Anything which is specific to a step (like setting some config for redis, or how to run the server, etc) must be documented in `Part 5`.


### Part 1 - Initialize empty repository
Initialize an empty repository in your space on Github. Name this repository as `orchestrator-service`. The repository must be initialized with an Apache license and a readme file.

Going forward, every part must be committed to the repository in its own commit - one commit per part. In order to do that, it is recommended to use a branching strategy with git, and merge your branch to master when you believe the section is complete.

### Part 2 - Creating RPC service
For this section, you'll have to use `protocol buffers` and `protobuf generators`. If you are not aware of what these words mean, please have a look here: [Link](https://developers.google.com/protocol-buffers/docs/reference/go-generated).

* Create a branch named `init-orchestrator-service`.
* Create an RPC service which acts as an interface for receiving and returning user requests. The interface must be built using protobuf and gRPC, and should be able to support the following methods:
    ```
    // Function to showcase metrics integration for the service
    func GetUserByName(ctx context.Context, name string) (*User, error)
    
    type User struct {
        name string
        class string
        roll int64
    }
    ```
    Note that the interface can be tweaked a little to support the actual application, provided the service is appropriately documented.
* Generate the server and client stub using protobuf generator.
* Return hardcoded response for `GetUserByName`:
    ```
    // GetUserByName Method response
    func GetUserByName(ctx context.Context, name string) (*User, error) {
        return nil, errors.New("not implemented yet. <yourname> will implement me")
    }
    ```
* Create an RPC client and store this in a separate `client` folder. Run the server on a port and use the generated RPC client code to read response from the server.
   Check [this](https://grpc.io/docs/languages/go/basics/) for hints on how you can use protobuf for the requirements above.
* Once you are satisfied with your changes, squash all your commits in the branch and merge it to master (Very important to squash commits, as it will help for better evaluation and review).

### Part 3 - Implementing Dummy data service
In this section, we will be implementing a dummy service which receives a request and based on the request, returns a hardcoded response.
* Create another branch named `dummy-service-impl`.
* Create a folder named `datamock` and create another RPC service inside this folder.
    ```
    func GetMockUserData(ctx context.Context, name string) (*User, error)
    ```
* This method works with a simple logic:
    * The service returns an error when user's name is `<6` characters.
    * The service returns a hardcoded `User` object, with `name` as the requested name, `class` as `len(name)` and `roll` as name.length * 10.
* Make sure the new dummy data service is capable of running as a separate microservice on port `10000`.
* Once satisfied with the changes, SQUASH all commits into one and merge the branch to master.

### Part 4 - Implementing Orchestration and connections
In this part, we will actually implement orchestration, and would allow the services to communicate to each other.
* Create another branch named `orchestration-impl`.
* Create another RPC method in the orchestrator, named `GetUser`.
* Create a folder named `logic`, where all the orchestration logic resides. 
* Run two orchestrator instances on ports `9000` and `9001`.
* Allow orchestrators to make RPC calls other orchestrators and data source such that:
    * Client calls the method `GetUserByName` on one orchestrator running on port `9000`.
    * This orchestrator is able to forward the call to the orchestrator instance on port `9001` and call it's `GetUser` method.
    * The second orchestrator makes a call with received parameters to the data server running on port `10000` and call the `GetMockUserData` method.
    * The `GetMockUserData` returns dummy response/error based on input, which is propagated to orchestrator 2, then to orchestrator 1 and finally back to client.
* Once satisfied with the changes, SQUASH all commits into one and merge the branch to master.

### Part 5 - Documentation
As with every production ready codebase, provide a small documentation on how to run the service and test it locally. The steps must be reproducible and should be written in a way so that any evaluator is able to execute the setup locally. Do not include how to setup go, protobuf, redis, etc).

Make the access for the repository public and reach out to the interviewer with a link to github repository once done. It would also be useful to provide the following information in the documentation:
1. Operating System used for Development (mac, windows, linux (recommended)).
2. `GOPATH` and repository path on local.
