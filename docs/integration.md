# Integration

**Integration** is one of the many problems of software development for governments. Many different departments may have different systems and even different use cases for interacting with your application. It is often not feasible to try to integrate with every different system.

This is a current conversation we are having with our city partner when discussing sustainability issues.

In order to have a consistent conversation let's define two terms:

### Some Definitions
* **Provider Application** - the application that is in charge of the data the developer wants.
* **Consumer Application** - the application that wants to integrate with the provider.

*These do not denote importance or size - there are many situations where the Consumer is much larger and more important than the Provider.  Consider a scenario where various distributed news sources (providers) are pinging on aggregator site (consumer) when new information is available.*

### A Scenario
Most of this is accurate, but I might make up some facts to illustrate.
Let's imagine that I am the developer of a shiny new platform that will help business owners find financing and education opportunities from a government.  I am aggregating various opportunities provided by various organizations (in the context of Puerto Rico, we have Economic Development Bank (BDE), Tourism, Film Comission and Company of Export and Commerce (CCE) to name a few).

When someone seeking for financing fills out some basic information, my application (provider) is going to send a lead to the appropriate department (consumer), based on the programs my user indicates she/he wants to know more about.  BDE and CCE are running two different implementations of Microsoft Dynamics CRM. Let's imagine Tourism is running something sharepoint-based.

Add about 15 more departments or agencies to this scenario and you see where I'm going...

### A non viable solution.
The most obvious solution would be to develop your application (the provider) as an API server.  Each of the departments (consumers) would have to write code that runs periodically that polls your server and see if there's any new leads related to them.

While this is a worthwhile solution there are several problems:

* More work for departments (cron code, polling code is hard to write)
* More work for you (either the developers in the departments or you would have to learn how to diff what's new and what's not.  And you want it to be you for the sake of sanity).
* Heavier loads on your server - imagine lots of machines polling your server every 5 minutes.
* Documentation and API changes because you have many people depending on the exact data structure.
* Buy in is a lot more difficult - lots more work needed and departments often just don't have the resources.

### A better solution - Webhooks!
What if whenever a lead is submitted you can automatically notify the related department (consumer) with all the information in the lead, in real time?

The solution is pretty simple and the level of work in building it for us is not more than building a standard API.

When departments create a new "Business Funding Opportunity" in our application, I am going to ask them for 2 extra fields:
* A URL to hit when a lead is generated.
* A secret token to send as part of that payload.

When the lead is generated, I'm going to send a standard (documented) set of data to the URL the department specified.  The payload will contain all the lead information, plus the secret token.  The developer on the department side needs only to provide me a path to hit where she/he checks for the token, and once we're ok, she/he consumes the data I'm sending in the payload.

What does she/he do with the data?  It does not matter.  She can put it in Mongo and crunch numbers on it.  She can create a lead in the CRM.  She can do that and 30 other things.  And when she/he decides thing #29 is useless, she/he can get rid of it without picking up the phone to call me -- since all I'm doing is sending her/him the data in real time.

This concept is called webhooks and they're used all over the web.

### Github Webhooks.
Let's explore Github.  They have a very sweet implementation of web hooks.

On my repository, I can trigger a webhook to fire on certain events. If you want to follow along, go to a repo you own, then go to Settings, then Webhooks and Services.  But I'll post Screenshots.

As you can see, I currently have one webhook enabled.  This web hook will hit Slack, a messaging platform we use for communication and Slack will post various events in our chatroom.

{<1>}![Webhook Enabled](http://mrm-screen.s3.amazonaws.com/Webhooks__Services_2014-09-02_19-28-54_2014-09-02_19-29-04.png)

When I manage that webhook I see a bit more info:

{<2>}![Webhook Manage](http://mrm-screen.s3.amazonaws.com/Webhook_-_httpscfa.slack.comserviceshooksgithub_2014-09-02_19-30-46_2014-09-02_19-31-11.png)

The payload URL I specify is given to me by Slack.  On the other end, a Slack web service just sits there and listens for the incoming data.

The content type is JSON but I could pick several others.

While this webhook doesn't need it, I can specify a secret token for the receiver to verify the legitimacy of the incoming data.

The last arrow points to something specific for Github.  I can actually pick which events trigger a webhook.  Pretty sweet deal!.

As I scroll down I see a log of deliveries of the webhook:

{<3>}![Delivery Log](http://mrm-screen.s3.amazonaws.com/Webhook_-_httpscfa.slack.comserviceshooksgithub_2014-09-02_19-34-18_2014-09-02_19-34-24.png)

As you can see the URL always gets hit with the same standard set of data and the receiver knows how to process it.  In general the best practice is to send a lot of data on a webhook.  A bigger payload doesn't hurt as much as an extra network connection.
