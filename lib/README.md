# IDWise Document Capture SDK

This SDK allows you to integrate the IDWise Digital Identity Verification technology in your web app with minimal effort
It presents a highly customisable UI that guides the user through a series of steps that prompts them for their ID documents and/or biometrics depending on how you have configured your journey flow in IDWise backend system. At the end of this process your app will receive a callback with information about the completed journey which your backend code can use to securely get the results of this journey. It's that simple!

Please follow these steps to integrate with the document capturing SDK:
1. Add IDWise library files
Copy the files `idwise.min.js` and `idwise.min.css` as assets/static files to your web server distribution files or your CDN to make them available to be served to your web users along your other web assets

2. Add a reference to the style sheet file “idwise.min.css” in the web page that will host IDWise Web SDK. For example:

    ```
    <link href="./lib/idwise.min.css" rel="stylesheet">
    ```

3. Add a reference to the script file `idwise.min.js` in the web page that will host IDWise Web SDK. For example:

    ```
    <script src="./lib/idwise.min.js"></script>
    ```

4. Decide which element that will host the IDWise UI. This can be any block element and can be as simple as this example:

    ```
    <div id="idwise-mount"></div>
    ```

5. Start the initialization of IDWise SDK library using `initialize` function. This function takes three parameters which identify your business and are shared with you during your registration process with IDWise.

    Example code for the inialization:

    ```
    <script>
      let idwise = IDWise.initialize({
        businessId: 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx',
        apiKey: 'xxxxxxxx',
        apiSecret: 'xxxxxxxx'
      })
    </script>
    ```

    This call returns a promise to a session instance. The session instance will be used to access the functionality of IDWise system in next steps

6. Use the IDWise session instance to start a new journey. To do so you can call `startJourney` function. The function takes three parameters:
    * `journeyDefinitionId` Identifies the steps of the journey to be started. This is provided to you by IDWise team based on your requirements and what documents or biometrics you want to ask your users for
    * `mount` The HTML DOM element where IDWise UI elements should be added. This was specified in Step 4 above
    * `eventHandlers` An object with list of callbacks to invoke to handle the
different events fired by IDWise SDK. There are currently two events supported: onStarted and onFinished. Which indicate the start and completion of the journey

    The following example code shows an overall script covering steps 5 and 6 above:

    ```
    <script>
      let idwise = IDWise.initialize({
        businessId: 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx',
        apiKey: 'xxxxxxxx',
        apiSecret: 'xxxxxxxx'
      })
      .then((idwise) => {
        idwise.startJourney({
          mount: '#idwise-mount',
          journeyTemplateId: 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx',
          eventHandlers: {
            onFinished: function(details) {
              alert('Thanks for completing the registration')
            }
          }
        })
      })
    </script>
    ```

You can access the identifier of the journey that was completed by accessing the parameter provided on the `onFinished` event callback. This identifier can be used to retrieve the data associated with this journey from IDWise Data Fetch API.

Note that IDWise SDK will automatically remove the UI elements it created and clean up the used resources when a journey completes and after triggering the `onFinished` event.

And that’s it! This is all you need to get core integration up and running and have IDWise Web SDK to enable streamlined user onboarding journeys.