# marketo-webhook-integration

### WhatsApp

The following document is a guide for building and testing a Marketo Webhook that will send
messages via Whatsapp using Nexmo Messages API.

### Building the Webhook in Marketo

##### Step 1

Log in to Marketo and navigate to Admin --> My Account
![Admin console](img/admin_console.png)

##### Step 2

Once in the Admin section, click on the Webhooks folder on the left hand side. Create a new
Webhook by clicking New Webhook

![Webhook console](img/webhooks_select.png)

##### Step 3

Compile the name and description field.

Put the following parameters:

- URL: `https://sandbox.nexmodemo.com/v0.1/messages/`
- Request Type: `POST`
- Example body: 

```json
{
	"to": {"type": "whatsapp", "number": "3912345678"},
	"from": {"type": "whatsapp", "number": "from_number"},
	"message": {"content": {"type": "template", "template":{
            "name":"whatsapp:hsm:technology:nexmo:simplewelcome",
            "parameters":[
               {
                  "default":"Nexmo"
               },
               {
                  "default":"interact with us over whatsapp. The campaignID is {{campaign.id:default=Campaign Id}}"
               }
            ]
         }}}
}

```
- Request Token Encoding: JSON

![Webhook console](img/edit_webhook.png)

**Note**: Using the `INSERT TOKEN` button you can also use tokens in the messages body sent via Whatsapp. In the example above, I used campaign id in the message body.

Lastly, select `Webhooks Actions` --> `Set Custom Header` and put `Authorization`: `Bearer {JWT}`, with JWT value. For example, `Authorization: Bearer asd12456789`.



Great! Now we need to test the Webhook in Marketo.


#### Smart Campaign

Go to `Marketing Activities`.

![Admin console](img/marketing_activities.png)

Create a New Campaign Folder. Then, select the folder and create a new Program.

![Admin console](img/new_program.png)

Create a new Smart Campaign. Select the `Smart List` tab and pick a condition that will trigger the Webhook. 

![Admin console](img/smart_list.png)

In this case we will send a message any time a lead fills out a form we have placed on a Marketo landing page (More info here: [LandingPage](https://docs.marketo.com/display/public/DOCS/Landing+Pages).



Select Flow tab and pick the `Call Webhook` action.


Finally, go to the `Schedule` tab and activate the campaign. 

**Congratulations**! Now visit the landing page and try out the webhook!
