# Say What I See

![](assets/tjbot.png)

## Requirements

In this lab, we'll use the see and speak nodes to train TJBot to recognize objects and speak what is seen. You will need a Raspberry Pi camera and speaker connected to the TJBot for this lab. 

## Train TJBot to See Objects and Speak

1. In the Node-RED editor running on the Raspberry Pi, drag an ![](assets/nodes/inject.png) node onto the canvas. Double click on the node and configure as shown below.

    ![](assets/1.1.png)

2. Add a ![](assets/nodes/see.png) node as shown below. The see node has several modes: recognize text, recognize objects, and take a photo. Select **See (identify objects)** from the **Mode** dropdown menu.

    The see node uses the Watson Visual Recognition service, which requires service credentials from IBM Cloud. Click on the pencil icon to the right of the **Bot** dropdown menu. 

    ![](assets/1.2.png)

3. Click on the link icon next to the **Visual Recognition** heading to launch into the IBM Cloud console and create a Watson Visual Recognition service instance.

    ![](assets/1.3.png)
4. If you don't have an IBM account, sign up at [https://bluemix.net](https://bluemix.net). Sign into your account if prompted. Leave the service name as is and click **Create**.

    ![](assets/1.4.png)

5. Click on **Service Credentials** in the menu on the left. If there are no credentials in the list, click **New credential** and **Add** to create a set of credentials. Click on **View Credentials** to display the service credentials.

    ![](assets/1.5.png)	
    ![](assets/1.6.png)

6. Copy the API key into the field back in the Node-RED editor under the **Visual Recognition** section.

    ![](assets/1.7.png)
    ![](assets/1.8.png)
	
7. Enable the camera by ticking the checkbox labeled **Camera**. 

    ![](assets/1.9.png)

8.	The see node produces a message with names of objects and colors in the photo analyzed, with the response being passed in the `msg.payload` property. Add a ![](assets/nodes/function.png) node to loop through the results and concatenate them into a new message.

    ![](assets/1.10.png)

9.	Add a ![](assets/nodes/speak.png) node as shown below. The speak node uses the Watson Text to Speech service, which requires service credentials from IBM Cloud. Click on the pencil icon to the right of the **Bot** dropdown menu. 

    ![](assets/1.11.png)

10.	Click on the link icon next to the **Text to Speech** heading to launch into the IBM Cloud console and create a Watson Text to Speech service instance.

    ![](assets/1.12.png)

11.	Leave the service name as is and click **Create**.

    ![](assets/1.13.png)

12.	Click on Service Credentials in the menu on the left. If there are no credentials in the list, click **New credential** and **Add** to create a set of credentials. Click on **View Credentials** to display the service credentials.

    ![](assets/1.14.png)	
    ![](assets/1.15.png)    

13.	Copy the username and password into the fields back in the Node-RED editor under the **Text to Speech** section.

    ![](assets/1.16.png)
    ![](assets/1.17.png)	
14.	Determine the Speaker Device ID by running the command `aplay -l` on the Raspberry Pi. In the example output shown below, the USB speaker attached is accessible on card `2`, device `0`.

    ![](assets/1.18.png)

    In the TJBot configuration, enter the applicable speaker device ID, with the format `plughw:<card>,<device>`

    ![](assets/1.19.png)

15.	At the top of the configuration window, select **English (US dialect)** from the **Speak** dropdown menu. Enable the speaker by ticking the checkbox labeled **Speaker**. 
 
    ![](assets/1.20.png)

16.	Connect the nodes together as shown below.

    ![](assets/1.21.png)

17.	Click on the ![](assets/nodes/deploy.png) button in the top-right corner of the Node-RED editor to save and deploy the changes.

18.	Click on the tab to the left of the inject node to take a picture with TJBot's camera. When the photo is analyzed with the Watson Visual Recognition service, a message is constructed with the objects and colors recognized, and is spoken out via the speaker.

    An example is:

    `TJBot sees earphone, person, face, people, maroon color`
