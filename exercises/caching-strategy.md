# Take home exercise - Service Caching

### Prerequisites
It is highly recommended to go through the getting [started page](main/getting-started.md) before jumping to this exercise.

### Requirements
This exercise will be divided into three parts, and each individual will be evaluated based upon how well they perform in each step.
The goal of this exercise will be to build a full-fledged RPC service which would allow to get and set data to a Redis cache.

### Some Useful Tips
1. Always code to an interface, wherever possible. This allows us to have better test coverage for codebase and have mutually detacheable items. If you do not understand what this means, Take out some time to go through [this](https://medium.com/rungo/interfaces-in-go-ab1601159b3a).
2. Follow the strategy of "accept interfaces, return structs" in your code wherever possible. Read more about it if you aren't sure what this means.
3. It is important to follow the branching structure mentioned in the steps below (and the squash step) as it allows the reviewer to review every commit as an individual entity, rather than a plethora of code.
4. Document whatever isn't obvious. Anything which is specific to a step (like setting some config for redis, or how to run the server, etc) must be documented in `Part 5`.


##### Part 1 - Initialize empty repository
Initialize an empty repository in your space on Github. Name this repository as `cache-service`. The repository must be initialized with an Apache license and a readme file.

Going forward, every part must be committed to the repository in its own commit - one commit per part. In order to do that, it is recommended to use a branching strategy with git, and merge your branch to master when you believe the section is complete.

##### Part 2 - Creating RPC service
For this section, you'll have to use `protocol buffers` and `protobuf generators`. If you are not aware of what these words mean, please have a look here: [Link](https://developers.google.com/protocol-buffers/docs/reference/go-generated).

* Create a branch named `init-rpc-service`.
* Create an RPC service which acts as an interface for receiving and returning user requests. The interface must be built using protobuf and should be able to support the following methods:
    ```
    // Function to set the data in cache using key and value.
    func Set(ctx context.Context, key string, value []bytes) error
    
    // Function to get the byte array data based on a given key
    func Get(ctx context.Context, key string) ([]bytes, error)
    ```
    Note that the interfaces can be tweaked a little to support the actual application, provided the service is appropriately documented.
* Generate the server and client stub using protobuf generator.
* Return hardcoded response for `Set` and `Get` methods as follows:
    ```
    // Set Method response
    func Set(ctx context.Context, key string, value []bytes) error {
        return errors.New("not implemented yet. <yourname> will implement me")
    }
    
    /// Get Method response
    func Get(ctx context.Context, key string) ([]bytes, error) {
        return []bytes("<yourname> will implement me"), nil
    }
    ```
* Create an RPC client and store this in a separate `client` folder. Run the server on a port and use the generated RPC client code to read response from the server.
   Check [this](https://grpc.io/docs/languages/go/basics/) for hints on how you can use protobuf for the requirements above.
* Once you are satisfied with your changes, squash all your commits in the branch and merge it to master (Very important to squash commits, as it will help for better evaluation and review).

##### Part 3 - Implementing Cache service
In this section, we will implement the methods created above and actually allow users to store and retrieve data from a `Redis` cache.
* Create another branch named `redis-impl`.
* Install and setup redis on your local. Use [this](https://redis.io/topics/quickstart) to set this up quickly and run it on your system. Redis must be configured to run on port `6379` (default port).
* Implement the set and get methods to write and read the data  to/from redis and return to user. To interact with redis, use the [`go-redis`](https://github.com/go-redis/redis) library.
* Every key in redis must be prepended with `<yourname>`. Example, if bob is completing this exercise and user reqests for setting a key called `mydata`, `yourname:mydata` must get stored in redis.
* Set the following configurations for redis:
    ```
    ttl: 300 seconds
    max memory size: 100MB
    cache strategy: LRU
    ```
    Document any redis script that needs to be executed at start in a folder called `init` and name the file as `initialize_redis.sh`.
* Once satisfied with the changes, SQUASH all commits into one and merge the branch to master.

##### Part 4 - Creating custom cache client
The sections so far have navigated and helped in setting up a simple Set and Get client. Now, we will create custom RPC methods catered to a specific business requirement.

* Create a branch named `user-client`.
* Create a new RPC method named `SetUser` and `GetUserByID` with the following schema:
    ```
    User Schema:
    {
        Name     string `json:"name"`
        Class    string `json:"class"`
        RollNum  int64  `json:"roll_num"
        Metadata []bytes `json:"metadata"
    }
    
    // Sets a user based on a given ID.
    // The logic for ID is defined in the comments below.
    func SetUser(ctx context.Context, user *User) error
    
    // Get the user based on identifiers provided.
    func GetUser(ctx context.Context, name string, roll int64) (*User, error)
    ```
* The `SetUser` method must be able to generate a key using `name`, `class` and `roll`. For example, a user named `Alice` with class `IV` and roll `15` must be saved as `<yourname>:name:class:roll` in redis. Similarly, generate the key for fetching the data based on user information.
* Refactor your code to have the following code structure (for reference only):
    ```    
    - Cache service
          - server (server logic)
            - handlers
                - user.go (for the SetUser and GetUser methods)
                - raw.go (for the original set and get methods)
            - logic
                - user.go (contains all logic to get and retrieve user data)
                - raw.go (contains logic to get and retrieve raw data)
                - base.go (contains any generic utility required across all files in logic)
          - proto (for *.proto)
          - z_generated (for generated *.go files)
          - client (client code)
              - user.go
              - raw.go
              - connect.go (client to connect to the server)
    ```
* Once satisfied with code, squash all changes and merge to master.


#### Part 5 - Documentation
As with every production ready codebase, provide a small documentation on how to run the service and test it locally. The steps must be reproducible and should be written in a way so that any evaluator is able to execute the setup locally. Do not include how to setup go, protobuf, redis, etc).

Make the access for the repository public and reach out to the interviewer with a link to github repository once done. It would also be useful to provide the following information in the documentation:
1. Operating System used for Development (mac, windows, linux (recommended)).
2. `GOPATH` and repository path on local.
