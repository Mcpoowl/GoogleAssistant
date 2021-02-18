# Google Assistant Module for Mendix

This module will help you get started implementing Google Assistant in your Mendix project. It can be quite a handful if you're doing this for the first time, but in essence, it's not that difficult. Feel free to send me a message if you can't figure it out though!


### Typical Usage scenario

Typing can get quite boring, so why not talk instead?

### Features and limitations

Current features

* Text to speech responses
* Basic card responses
* Carousel responses
* Table response
* Account linking with JWT tokens

Google JSON is very specific, so you'll probably need to adjust the JSON snippets and mappings to suit your need

NB: Your app needs to run on cloud v4!

### Configuration

Using the module isn't that difficult. The hard part is setting up Dialogflow and Actions on Google. Ready? Here we go!

1. Create a new Dialogflow Project
2. Create your intents
3. Import the module in your application, create the GAUserId attribute on your Account entity and set the correct role for the GoogleAgent Module role. Make note of the REST URL in the published REST service
4. Go back to Dialogflow, and enter the correct information in the ‘fullfillment’ tab of your project. This will make sure that any intent that is set to use fulfillment, will call your REST service
5. For every intent that you want to call your Mendix app, in your Afterstartup, call the AddActionToRegistry Java action, and add the IntentName (String match, so case sensitive) and the microflow you want to execute (always duplicate the Action_Template microflow, and build your logic there)
6. In Dialogflow, click the integration tab, and add Google Assistant, also check the ‘Auto-preview changes’ slider
7. Create a simple intent in dialogflow, select the ‘use fulfillment for this reponse’ and create the response in Mendix, and test! You can always look at the ‘Action_Fulfill’ microflow to get a feel on how to create responses.
 
New in this version is the addition of JWT tokens: we now have to enable Account Linking if we want to have user specific information.

To do this, you need to add Account Linking to your Actions on Google Project, and copy the ClientID you get when setting this up. Add this to the GoogleAccountClientID constant.

You should also make a new intent with the following JSON as a custom payload response:

```json 
{
  "google": {
    "expectUserResponse": true,
    "systemIntent": {
      "intent": "actions.intent.SIGN_IN",
      "data": {
        "@type": "type.googleapis.com/google.actions.v2.SignInValueSpec"
      }
    }
  }
}
```

Call this intent something like: Sign me in, so it can request access to the User’ profile

Make sure to fill your GAUserID attribute with the correct Google Email, since it'll to a retrieve based on that. NB: This is only need if you have user specific questions, and need to retrieve data for a specific user instead of general information.

### Frequently Asked Questions

Q: Halp?

A: Hit me up on Slack! (or send me an email through the dev portal!

Q: It's not working?

A: Check to see if your mapping is correct, and check the raw response in Actions on Google

Q: I don't get a User in my requests

A: Google only sends user data from actual devices, or through the Actions on Google test suite, NOT the Dialogflow test suite.

Q: I'm missing something in this documentation

A: That could very well be, feel free to hit me up and I'll make sure it gets updated
