# Take home exercise - UI Connector

## Prerequisites
It is highly recommended to go through the basics of React before getting started.

[React overview](https://reactjs.org/tutorial/tutorial.html#overview)

[freecodecamp tutorial](https://www.freecodecamp.org/news/react-beginner-handbook/)

## Requirements
This exercise will be divided into five parts, and each individual will be evaluated based upon how well they perform in each step.
The goal of this exercise will be to build a full-fledged React application which would allow the users to create customizable connected components on the UI.

## Some Useful Tips
1. Try using React hooks wherever possible - they are must easier and cleaner to use. [Link](https://reactjs.org/docs/hooks-intro.html)
2. Try to define every React components as a functional components and not a class component.
3. If using React hooks, there must be minimal requirement for lifecycle components, and therefore must be avoided in places where react hooks can be used.
4. Typescript is recommended but not must - you are free to choose whether you like typescript or not and proceed accordingly.
5. DO NOT eject the package once you have created it with `create-react-app`.

### Part 1 - Initialize empty repository
Initialize an empty repository in your space on Github. Name this repository as `app-ui`. The repository must be initialized with an Apache license and a `README.md` file.

Going forward, every part must be committed to the repository in its own commit - one commit per part. In order to do that, it is recommended to use a branching strategy with git, and merge your branch to master when you believe the section is complete.

### Part 2 - Creating Hello world react application + Mockserver
* Create a branch named `init-react-app`.
* Initialize the react application by using the `create-react-app` command. 
* The app must be created with the same name as the repository i.e. `app-ui`.
* Create a node.js server in the app, which acts like a mock server. Allow the mock server to start and stop along with the react app start (i.e. with `npm start`).
* The mock server can be a simple express application that returns pre-defined responses for given input.
* Once satisfied with the changes, SQUASH all commits into one and merge the branch to master.

### Part 3 - Using React Diagrams
In this section, we will utilize the React Diagrams library to create connected components on the UI, and capture their events.
**React Diagrams**: [Github](https://github.com/projectstorm/react-diagrams)

* Create another branch named `init-react-diagrams`.
* Use `npm` to install `react diagrams` in the application.
* Create a file named `BasicConnection.jsx` in the application and use that for rendering the component application.
* Provide application logic within the file created above to create a UI with two basic components (boxes) connected by a connector. Check the demos in React Diagram to understand what these mean. Outcome should look something similar to [this](https://projectstorm.cloud/react-diagrams/iframe.html?id=simple-usage--canvas-drag&args=&viewMode=story).
    * The connector line and boxes must be draggable.
    * The boxes must be labelled as Source and Destination. The connector must have a one-sided arrow, pointing to the destnation.
    * Every change event on canvas (line drag, component connect, disconnect, etc.) must be console logged.
* Additionally, allow custom CSS to be used with the components and connectors (a Proof of concept should be enough - no need to make it fancy).
* Once satisfied with the changes, SQUASH all commits into one and merge the branch to master.

### Part 4 - Interacting with Mock App
By now we should have a frontend application which has two basic components created on the UI, connected by a line. In this section, we will try to publish the UI changes to our mock service.
* Create a branch named `service-integration`.
* On every UI component connect/disconnect, collect the state of these two components and publish to the mock service.
    * The state must be described as:
        ```
        {
            "components": [
                {
                    "id": "c1",      // unique identifier for first box created
                    "name": "Source", // name of the box/component
                },
                {
                    "id": "c2",
                    "name": "Destination"
                }
            ],
            "links": [
                {
                    "src": "c1",    // source of the link
                    "dest": "c2"    // destination
                }
            ]
        }
        ```
        One thing to note here is the fact that the `links` region will be sent to backend as `[]` (empty array) if the UI doesn't find any components connected.
    * The application mustt send this body to the mockserver on `localhost:3000/api/state/cache` endpoint, which should return response with status `204` on every such request.
* Once satisfied with code, squash all changes and merge to master.


### Part 5 - Documentation
As with every production ready codebase, provide a small documentation on how to run the service and test it locally. The steps must be reproducible and should be written in a way so that any evaluator is able to execute the setup locally. Do not include how to setup npm, javascript, etc).
If there are any install steps required, either make them a part of `npm install` of the root app, or create a shell script called `install.sh` which serves as one stop installation.

Make the access for the repository public and reach out to the interviewer with a link to github repository once done. It would also be useful to provide the following information in the documentation:
1. Operating System used for Development (mac, windows, linux (recommended)).
2. Javascript version
3. `create-react-app` version
4. `npm` version
