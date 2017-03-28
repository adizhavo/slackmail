**Slackmail** is a gmail and slack integration to receive emails in your slack channel for OSX and Linux

The system uses [jq](https://stedolan.github.io/jq/) deserialise json, so before proceeding make sure to have it.

Dont forget to make the script executable with the command ```chmod +x <script>```

#### Gmail API
* Enable the API in the [google developer console](https://console.developers.google.com)
* Create credentials as a _OAuth Client Id_ and choose _Other_ as an application type
* Copy and paste ```client_id``` and ```client_secret``` code in the [config](https://github.com/adizhavo/slackmail/blob/master/config.json)

the scope is already defined in the authorisation script as https://www.googleapis.com/auth/gmail.readonly

#### Slack app
* Create a Slack app
* Activate the ```Incoming Webhook``` functionality
* Select the channel you want the app to post your emails
* Copy and past the ```Webhook URL``` in the [config](https://github.com/adizhavo/slackmail/blob/master/config.json)

#### Gmail Authorization
Executing the ```email_authorization.bsh``` script will open the browser and let you pick up your gmail account.
After choosing, it will give back an authrization code, copy and past it to the terminal to complete the authorisation.

If its successful, it will create a file called ```token.json``` under the resources folder.
If this file is deleted another authorisation is required.

#### Cron job
The final step is to execute the ```slack_mail.bsh``` script to read and the unread emails form the gmail account and post them on slack.
This is done by setting up a cron job.

```slack_mail.bsh``` takes two arguments:
* The path where the ```resources``` exists
* The path to the jq library

The first argument is required, the second is mostly optional, since it should be able to read jq from the environment variables, but for the cron job the path to jq is required since the cron job is called by the system that doesn’t have the same environment variable of your shell.

To get the path to the jq library execute the command ```type jq```

To start a cron job execute the command ```crontab -e``` and put your cron job command in the file.

Mine is ```* * * * * /Slackmail/slack_mail.bsh /Slackmail /usr/local/bin/jq >/tmp/slackmail.log;```

## Improvements

* Support multiple gmail accounts
* Improve slack post format
