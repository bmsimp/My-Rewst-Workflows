# Teams Notification Bot without Database

I'd like to give full credit to Brandon/Giga for his blog on how to set up the Azure Bot Framework to work with Teams. I highly, highly recommend you read his blog on the basics of setup, interacting with the bot, and basic messaging. [Click here to read:](https://blog.gigacode.dev/technology/cloud-concepts/microsoft-azure/bot-framework) 
![image](https://github.com/bmsimp/My-Rewst-Workflows/assets/50429915/50d30b0d-5bd0-43ac-8cac-1f1559d57b15)

## The Basics

Assuming that you've read all of Giga's code... here's how I got started. Unlike his bot, I was looking to create a notification only bot. Something that would send a notice to Teams based on ticket activity. These notices are going to different Teams channels based on the type of activity. I don't currently have in my scope to create a chat bot that would have a need to identify and respond to potentially dozens or even hundreds of user chat installs. I also didn't want to have to learn how to use Azure Tables or bug our team to create new SQL tables for me. When I began to review how to design the adaptive cards, I noticed that the submit action could include a JSON object with the return.

Given all of this, I got to work on the setup of my bot: 
1. I installed it in our dev tenant using Giga's handy guide.
2. I set up a basic one noop workflow in Rewst with a webhook trigger to be the messaging endpoint. If you've followed Giga's blog, he used webhook.site to test.
3. Using the very easy to follow package instructions Giga provided, I created my KarpelBot zip file and uploaded it to my dev tenant and waited for it to propogate.
4. Instead of using the custom integration, I set up a workflow that will refresh the token and keep an organization variable named `karpelbot_access_token` so if you utilize any of my workflows, you'll need to replace that token variable name with your own.
6. When I installed the bot on a Teams channel, I noted that the JSON payload sent to the Rewst workflow included quite a lot of information: ![image](https://github.com/bmsimp/My-Rewst-Workflows/assets/50429915/46c7a57f-1908-4ad0-b1ba-1916a4958bf1)
7. Using this information, I was able to begin to see that much of the information I would need to handle action responses was going to be contained in the response.
8. Utilizing the [Actionable Message Designer](https://amdesigner.azurewebsites.net/), I created an adaptive card that would include the approve/deny actions.
9. Sending that card to the bot showed that the response would also be included in a JSON object: ![image](https://github.com/bmsimp/My-Rewst-Workflows/assets/50429915/b7a4e113-ee87-4b9b-9961-23b3db1aea1a)
10. I could even add additional fields to the JSON object: ![image](https://github.com/bmsimp/My-Rewst-Workflows/assets/50429915/4f80a3fb-48f0-4b90-8fa1-902ddacac817)
11. Looking at the response to the action that sends the card to the Teams channel, the message id and activity id are returned: ![image](https://github.com/bmsimp/My-Rewst-Workflows/assets/50429915/6291a2ec-b853-4370-8811-ca7bf505e1e5)

