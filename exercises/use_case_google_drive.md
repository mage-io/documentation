# Mage Introduction - Create Google Drive

## Prerequisites
1. It is highly recommended to read the user manual for Mage by checking ![magedev.xyz/docs](https://magedev.xyz/docs).
2. Users are also encouraged to try and check ![Mage's Landing page](https://magedev.xyz/) before beginning this exercise to understand what mage is expected to achieve.

## Useful Links
1. ![Mage Console](https://console.magedev.xyz) - For actually creating code by dragging-and-dropping components
2. ![Documentation](https://magedev.xyz/docs) - for reading through the documentation to understand how mage works
3. ![Discord Group](LINK) - for useful discussions and resolving doubts in case you are stuck

## Exercise Requirements
In this exercise, we'll be creating the backend application for a Basic Google Drive application which allows users to:
1. Store files in a pre-configured S3 bucket.
2. Persist and Retrieve metadata in SQL database.
3. Fasten data retrieval by adding a redis caching layer over get metadata endpoint.
4. Adding platform-wide monitoring for all API requests.
5. Generating code for the created application.

## High Level Steps
1. Login to the Mage console using google credentials.
2. Create a new microservice using the New microservice card or by clicking new microservice from the title bar. Give it any name of your choice (example: GoogleDriveService).
3. From Components section on the left, add a few components to the canvas. The following components will be required to complete the exercise.
    1. gRPC component to read user input
    2. orchestrator to configure middlewares (monitoring)
    3. Grafana/Monitoring middleware to add monitoring requirements
    4. SQL to read/write metadata
    5. Redis to read metadata faster
    6. Look Aside cache to connect SQL method with a cached redis method
    7. File Storage component to read/write Blob data
4. You can read more about these components under the ![Docs](https://magedev.xyz/docs) page.
5. Click on Save button in title bar to save the configurations.
6. For configuring SQL, Redis and S3 components, we'll need to connect to appropriate data stores. For doing this, navigate to the Infrastructure Management section and then add relevant data store configurations.
7. While it is ok for this exercise's use case to keep the connection URLs as default values, you'll need to create SQL schemas. After creating an SQL placeholder, configure schema for the database based on the requirement (like file name, file ID, etc.)
8. Once satisfied with infrastructure, click on save button in the title bar to save infrastructure components.
9. Navigate back to the Editor Page to resume from where we initially left.
10. Double click every component to add connection methods. These are the methods which are used for reading/writing data from every mage's component.
11. Connect the components appropriately and configure them as per the requirement (Read the documentation for more details on how this is generally done).
12. Once satisfied, save the UI using the save button and navigate to builds section.
13. In the builds page, configure your gitlab repository credentials. Check ![Builds](https://magedev.xyz/docs/docs/Builds/) documentation on how to achieve this.
14. Once done, click on `Save` button in the builds config section to save these credentials.
15. Select the microservice created and then create a new build by selecting the New build button.

## Output Expected
1. The build should be in `SUCCESS` state at the end of this exercise.
2. The repository created should have a Merge request created with the code for Google Drive application.
3. There should at least be the following methods in the application:
    1. GetUserFiles - returns a list of all data from the files table. Uses redis for look aside cache
    2. CreateFileMeta - creates a new entry in the files table
    3. GetFileMeta - returns a single row for the given file name.
    4. GetFile - returns S3 blob
    5. SetFile - writes file data to S3

## Help
In case of any troubles, please go through the Mage's documentation to find more details. If you need help regarding something not listed in the documentation, please reach out to us via our discord channel.

