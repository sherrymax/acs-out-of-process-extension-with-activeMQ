# Create a Node Event Listener Project

## Initial Steps
For this activity, you must use **your environment from the end state of activity 1 from this git project** or **download and extract the _finished-state.zip_ project linked here: https://github.com/GBHyland/alfresco-out-of-process-extension-with-activeMQ/blob/main/activity1-helloworld-oop-skeleton/completed-state.zip**.
* If using the zip file, complete the next section _(Steps to start from zip file)_ steps.

### Steps to start from the zip file.
* Download and extract the zip archive linked above to an accessible location.
* In your JDE, select **File > Open** and navigate to the POM.xml file created in the previous step. Once selected, press the **Open as Project** button on the next popup.
* When the project is opened and any dependency errors are resolved, continue to the next section _(Create a _NodeUpdatedHandler_ Java Class)_.

## Create a _NodeUpdatedHandler_ Java Class
### Imports and Implements
* Create a new Java class file titled ```NodeUpdatedHandler``` in the following path: _src > main > java > org.alfresco > handler_.
* Add the following import:
  ```
      import org.alfresco.event.sdk.handling.handler.OnNodeUpdatedEventHandler;
  ```
* Change the class function to this:
  ```

  ```
* You may need to implement the methods for this class by holding the mouse cursoer over the implement statement and select _Implement methods_ from the popup (implement the handle event).
* Add ```@Component``` just before the class statement.

### Create a LOGGER
* Add the following line of code to the class:
  ```
    private static final Logger LOGGER = LoggerFactory.getLogger(NodeUpdatedHandler.class);
  ```
* If the code shows errors, hold the mouse cursor over the error and choose to _add imports_. You should have the _Logger_ and _LoggerFactory_ imports.
  
### Code the Handler
* Add the following script to the _handleEvent_ function:
  ```
      final NodeResource nodeResource = (NodeResource) event.getData().getResource();
  ```
* Add a Logger command to the _handleEvent_ just after the _nodeResource_ variable:
  ```
    LOGGER.info("\n\n ** DATA: "+event.getData());
  ```
* Add the following _if statement_ to single out events that contain content.
  ```
    if (((NodeResource) event.getData().getResourceBefore()).getContent() != null) {

    }
  ```
* **Note:** A document node in ACS has associated nodes that will update when a change is made to the document, causing the Updated handler event to execute mutliple times. By ensuring that the _getContent_ attribute of the _getResourceBefore_ attribute of the edited node is **not** null, we can single out responses that are referring to the content node.
* Add the following line of code inside the if statement:
  ```
    System.out.println("This content has been modified. ");
  ```
* Add the following function to your )handleEvent_ class below the "if" statement:
  ```
    @Override
    public EventFilter getEventFilter() {
        return IsFileFilter.get();
    }
  ```
* **Note:** The _EventFilter_ function will override and filter out all responses that are **not** files. This will ensure that we only get file-related responses.

### Deploy and test the Java App to ensure the 
* With your alfresco environment running, execute both of these commands in your Terminal window to run your java application:
  ```
    mvn package
  ```
  ```
    java -jar target/oop-*.jar
  ```
* In your Alfresco environment, create a Site if you do not have one already. 
* Create a plain text file in the Document Library of your site and save it.
* Edit the file's content within Share.
* Go back to your Terminal window in your JDE to view the printed results.
    * **Note:** You should see each returned data block from the _LOGGER_ command that is printing the event data, however only one of those blocks should be followed by the _"This content has been modified."_ system print comment added within the "if" statement.
* Stop the Java application by using ```CTRL+C``` inside the Terminal window.

### Apply logic that checks if document was edited by someone other than creator
