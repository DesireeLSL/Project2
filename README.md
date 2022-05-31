Getting updates from GitHub via Telegram Bot
----------------------------------------------
This is an example on getting a Telegram notification when there is a commit/pull request with *Github Actions*.

**Prerequisites:**
- Github Account
- Telegram account

You will need to have a Github account and a Telegram account, with Telegram installed in either your smartphone or computer.

**_Step 1: Create a Telegram Bot_**

- To create a new bot, we would have to "talk" with BotFather. To do so, open your Telegram App via this link in a new window -> https://t.me/botfather.
- Type in "/newbot" and select the first option. Enter the following details as the bot prompts:

	Name: Github Notifier Tutorial
	
	Username: gh_notify[random number]_bot        (For example, gh_notify3288_bot)

(Note: The username of the bot must be unique.)

**_Step 2: Get the Token_**

Once all the details have been entered, you’ll recieve the confirmation message for your bot along with the details of the token. Copy the token key as it would be required later on. (It would be in the ##########:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx format, where # are numbers and x are alphanumeric characters.)

NEVER share your token with anyone, as it could be used to control your bot!

![image](https://user-images.githubusercontent.com/93770122/171029567-8e7bc150-5ba6-40c6-89b2-fb76a54087ce.png)


After this, open the link of your bot in the message and press **start**. (There should be no response) This is to get the unique chat id, which tells the Telegram bot to send notifications to only you.

With the Telegram bot created, let’s proceed with the Github part.

**_Step 3: Select a Repository_**

You could either pick an existing repository to implement the actions or create a new repository. For this exercise, we’ll be creating a new repository. To create a new repository, click on the plus icon beside the profile icon, and then "New repository".

![image](https://user-images.githubusercontent.com/93770122/171030386-96019f18-ccbc-4fd3-a82a-2d572c1a3fed.png)

Let’s name it "gh-telegram-notifier", check the checkbox "Initialize this repository with a README" and create the repository. You’ll be redirected to the new repository once it is created.

![image](https://user-images.githubusercontent.com/93770122/171030459-b7b06fbd-3c0b-4480-9aea-41d36d1839eb.png)

**_Step 4: Create Secret Variables_**

We’ll be using the secrets variable to store our Telegram bot token and our chat identifier key (in order to know who to notify when there is a change in the GitHub repo status). 

- Visit the following URL on your web browser: https://api.telegram.org/bot##########:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/getUpdates replacing ##########:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx with your bot token key. This would return a JSON response, and we can get the unique chat id from the "from" entry (see highlighted in yellow in the screenshot below):

![image](https://user-images.githubusercontent.com/93770122/171032960-eb045c84-714a-47bd-a789-c1bab4a0e7f4.png)

- Returning to your repository, click on the "Settings" tab, followed by the "Secrets" and "Actions" on the bottom left of the menu. We would be adding the variables here.

![image](https://user-images.githubusercontent.com/93770122/171033663-988755de-7555-4061-b494-e3780819dd4a.png)

- Click on "New repository secret".

- Create the first variable: Key in "TELEGRAM_TO" as Name and the unique chat ID as the Value, then click on Add secret.

![image](https://user-images.githubusercontent.com/93770122/171034249-65169ee7-13dc-420c-a3e9-88a682d642ef.png)

- Create the second variable: Key in "TELEGRAM_TOKEN" as Name and the Token Key as Value, then click on Add secret.

(For example, the TELEGRAM_TO would only consists of numbers (0000000001) & TELEGRAM_TOKEN follows the ##########:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx format.)

![image](https://user-images.githubusercontent.com/93770122/171034535-09f5445e-91e7-4248-aaa9-24561b97de2d.png)

**_Step 5: Setup the Workflow_**

- Click on the "Actions" tab below the title of the new repository, and then click on the "set up a workflow yourself" link.
![image](https://user-images.githubusercontent.com/93770122/171080042-abb8567c-97e4-4979-95ed-e790422d2508.png)


- A default template would be initialized by default, and we would want to change the setup accordingly. Update the name from "main.yml" to "tg-notify.yml", and replace the code with the one as shown below. (We’ll be using appleboy/telegram-action in this exercise.) 

````
name: tg-notify
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - name: Update status
      uses: appleboy/telegram-action@master
      with:
        to: ${{ secrets.TELEGRAM_TO }}
        token: ${{ secrets.TELEGRAM_TOKEN }}
        message: |  #https://help.github.com/en/actions/reference/contexts-and-expression-syntax-for-github-actions#github-context
          ${{ github.event_name }} commit in ${{ github.repository }} by "${{ github.actor }}". [${{github.sha}}@${{ github.ref }}]

````

- What the above workflow does is that it sends the user a notification via Telegram when the repository has a new push commit or pull request. The message would contain the commit status (either a push or pull request) with the repository name, followed by the username of the person who triggered the workflow, the commit SHA & the branch/tag reference that triggered the workflow run.
- Click on "Start Commit" and select "Commit New File". This would create the Workflow and start the Github Action. After a few seconds, you should recieve a Telegram message from your Telegram Bot as shown below, which means that the Actions are working!

![image](https://user-images.githubusercontent.com/93770122/171035850-0e679887-2b93-4409-9e63-41d329335902.png)

------------------------------------------------

Source: https://cyaninfinite.com/getting-updates-from-github-via-telegram-bot/
