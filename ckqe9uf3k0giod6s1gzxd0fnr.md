## Today's npm package: slack-node

Today's npm package is [slack-node](https://www.npm/js.com/package/slack-node)

 Slack is the new center of all the communication in Companies.
 
 And in your JS apps, while automating tasks for the projects, it would be better to get notified of messages and status as soon as things happen.
 
**slack-node** is going to allow you doing the same by binding to one webhook URL.

Let's go ahead and create a webhook to slack channel or user https://my.slack.com/services/new/incoming-webhook


Now add it to your environment variables as key `WEBHOOK`

For locala development, we will add this ENV variable to a a`.env` file and read it via [dotenv](https://www.npm/js.com/package/dotenv) package.

And then send some test messages to the webhook bound channel.

here's how you can do that:
```js
require('dotenv').config()
const Slack = require('slack-node')
console.log(process.env.WEBHOOK)

const slack = new Slack();
slack.setWebhook(process.env.WEBHOOK);
 
// slack emoji
slack.webhook({
  channel: "#general",
  username: "webhookbot",
  icon_emoji: ":ghost:",
  text: "test message, test message"
}, function(err, response) {
  console.log(response);
});
 
// URL image
slack.webhook({
  channel: "#general",
  username: "webhookbot",
  icon_emoji: "http://icons.iconarchive.com/icons/rokey/popo-emotions/128/after-boom-icon.png",
  text: "test message, test message"
}, function(err, response) {
  console.log(response);
});
```

And it will look like as follows:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1624742624674/vPtR6T4Hc.png)

Stay tuned for tomorrow's package of the Day.