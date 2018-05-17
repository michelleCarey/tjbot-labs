# Translate What I Hear

![](assets/tjbot.png)

## Requirements

In this lab, we'll use the listen, translate, and speak methods to train TJBot to listen to utterances, translate to French, and speak out the translation.

You can run this lab on a physical TJBot or use the [TJBot simulator](https://ibm.biz/meet-tjbot).

If you run this lab on a physical TJBot, you will need to connect a microphone and speaker to the TJBot for this lab. 

## IBM Cloud Account

You will need a IBM Cloud account to create the IBM Watson services used in this lab. Please visit [ibm.biz/start-tjbot-lab](https://ibm.biz/start-tjbot-lab) to register for and log into your IBM Cloud.

## Train TJBot to Listen and Translate

1. Create a file named `app.js`. Copy the following code. In the following steps, replace the `/* Step # */` placeholders with the code provided. 

    ```
    var TJBot = require("tjbot");
    
    var tj = new TJBot(
      [/* Step #2 *//* Step #20 */],
      {
        /* Step #19 */
      },
      {
        /* Step #8 */
        /* Step #13 */
        /* Step #18 */
      }
    );
    
    /* Step #9 */
    ```
    
2. In order for TJBot to listen and transcribe audio, we first need to configure it with a microphone. The first argument to the TJBot constructor is an array of hardware available. Add `"microphone"` to this array.

    ```
    var tj = new TJBot(
      ["microphone"],
    ```

3. TJBot uses the Watson Speech to Text service from IBM Cloud to transcribe the audio. If you don't have an IBM Cloud account, sign up at [ibm.biz/start-tjbot-lab](https://ibm.biz/start-tjbot-lab). Sign into your account. 

4. Click on the **Catalog** link in the top right corner of the IBM Cloud dashboard. 

    ![](assets/1.1.png)

5. Click on the **Watson** category on the left. Click on the **Speech to Text** tile.

    ![](assets/1.2.png)

6. Leave the service name as is and click **Create**.

    ![](assets/1.3.png)

7. Click on **Service Credentials** in the menu on the left. If there are no credentials in the list, click **New credential** and **Add** to create a set of credentials. Click on **View Credentials** to display the service credentials.

    ![](assets/1.4.png)	    
    ![](assets/1.5.png)	        

8. Replace the placeholder `/* Step #8 */` with the following code. Use your own username and password credentials from the previous step. 

    ```
        speech_to_text: { 
          username: "cf63b1f3-ef18-4628-86c8-6b1871e076b9",
          password: "MWNwz3qcdIab"
        }, 
    ```
    
    ![](assets/1.6.png)    

9. Replace the placeholder `/* Step #9 */` with the following code. 
This code will instruct TJBot to start transcribing what is heard and call this function after the first chunk of audio is transcribed.
    
    ```    
    tj.listen(text => {
      tj.stopListening();
      /* Step #14 */
    });
    ```

10. Next, we will train TJBot to translate the text using the Watson Language Translator service, which requires service credentials from IBM Cloud. Return to the IBM Cloud dashboard catalog. Click on the **Watson** category on the left. Click on the **Watson Language Translator** tile.

    ![](assets/1.7.png)

11.	Leave the service name as is and click **Create**.

    ![](assets/1.8.png)

12.	Click on **Service Credentials** in the menu on the left. If there are no credentials in the list, click **New credential** and **Add** to create a set of credentials. Click on **View Credentials** to display the service credentials.

    ![](assets/1.9.png)	
    ![](assets/1.10.png)    

13. Replace the placeholder `/* Step #13 */` with the following code. Use your own username and password credentials from the previous step. 

    ```
      language_translator: {
        "username": "6853aacd-963e-49ea-a911-3241521d7c03",
        "password": "Nt2xiUbcOUeR"
      },
    ```

    ![](assets/1.11.png)   

14. Replace the placeholder `/* Step #14 */` with the following code. This code translates the text from English (en) to French (fr).

    ```
      tj.translate(text, "en", "fr")
        .then((response) => {
          /* Step #21 */
        });
    ```     

15. Next, we will train TJBot to speak out this translation using the Watson Text to Speech service, which requires service credentials from IBM Cloud. Return to the IBM Cloud dashboard catalog. Click on the **Watson** category on the left. Click on the **Watson Text to Speech** tile.

    ![](assets/1.12.png)
    
16.	Leave the service name as is and click **Create**.

    ![](assets/1.13.png)

17.	Click on **Service Credentials** in the menu on the left. If there are no credentials in the list, click **New credential** and **Add** to create a set of credentials. Click on **View Credentials** to display the service credentials.

    ![](assets/1.14.png)	
    ![](assets/1.15.png)    

18. Replace the placeholder `/* Step #18 */` with the following code. Use your own username and password credentials from the previous step. 

    ```
    text_to_speech: {
      username: "dec28251-d359-4f88-a714-9f36694c4218",
      password: "5ZwSwrciqoHG"
    }
    ```

    ![](assets/1.16.png)   

19. Replace the placeholder `/* Step #19 */` with the following code. Configure TJBot with the gender of the voice (`male` or `female`) and what language to use (`fr-FR` is the language code for French). In this example, we'll use the French feminine voice model to synthesize the translation.

    ```
        robot: {
          gender: "female"
        },
        speak: {
          language: "fr-FR"
        }
    ```     

20. In order for TJBot to play audio, we need to configure it with a speaker. Add `"speaker"` to the array of the first argument to the TJBot constructor. If you're using a physical TJBot, please refer to the section below titled **Running on the Raspberry Pi** for more information about the speaker device ID.

    ```
    var tj = new TJBot(
      ["microphone","speaker"], 
    ```
    
21. Replace the placeholder `/* Step #21 */` with the following code. This will instruct TJBot to speak out the translation returned from the Watson Language Translator service.

    ```
          tj.speak(response.translations[0].translation);
    ```

22. Run the code. Speak a phrase, such as `Hello`. TJBot will transcribe the utterance captured via the microphone using the Watson Speech to Text service, translate the phrase using the Watson Language Translator service, and then speak out the translation using the Watson Text to Speech service via the speaker.

    An example translation is:

    `Bonjour`

## Running on the Raspberry Pi

Depending on the speaker you use, you may need to specify the speaker device ID. Determine the Speaker Device ID by running the command `aplay -l` on the Raspberry Pi. In the example output shown below, the USB speaker attached is accessible on card `2`, device `0`.

![](assets/1.17.png)

In the TJBot configuration, use the applicable speaker device ID, with the format `plughw:<card>,<device>`

```
    speak: {
      language: "fr-FR",
      speakerDeviceId: "plughw:2,0" 
    }
```

## Completed Code

The completed Node.js program is shown below, except for Watson service credentials. 

```
var TJBot = require("tjbot");

var tj = new TJBot(
  ["microphone","speaker"],
  {
    robot: {
      gender: "female"
    },
    speak: {
      language: "fr-FR"
    }
  },
  {
    speech_to_text: {
      "username": "",
	  "password": ""
    },
    language_translator: {
      "username": "",
      "password": ""
    },
    text_to_speech: {
      username: "",
      password: ""
    }    
  }
);

tj.listen(text => {
  tj.stopListening();
  tj.translate(text, "en", "fr")
    .then((response) => {
      tj.speak(response.translations[0].translation);
    });
});
```