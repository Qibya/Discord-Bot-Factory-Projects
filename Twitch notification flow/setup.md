# Twitch notification flow setup guide

## Twitch application

In this part I will guide you in making and setting up a Twitch application which is needed to make the flow work.

### Making the application

1. go to the [Twitch developers console](https://dev.twitch.tv/console) and login with your twitch account
2. click on "Register Your Application"
3. enter a name for the application, this can be anything but i would suggest giving this the name of your bot, because it does have to be unique
4. set "OAuth Redirect URLs" to "http://localhost:3000", you do not need to click on "Add"
5. set "Category" to "Application Integration"
6. set "Client Type" to "Confidential", it should be selected by default
7. click on "Create"

### Getting all the relevant information

I would suggest you open a text editor on your device so you can paste all them there for use later on in this guide.
I would suggest to do it like this, but this is entirely your choice.

`Client ID = xxx`

#### Client ID and Secret

These strings you can easily get in the [Twitch developers console](https://dev.twitch.tv/console) by going to the application that you just created.
The Client ID is easily visible, so paste that in your notepad.
For the Client Secret you just need to click on "New Secret" and then you can also paste that one in your notepad.

#### Refresh Token and Acces Token

These two aren't as easily accessible and require different steps depending on your operating system.

##### <u>Windows</u>

We will be using Windows PowerShell. You can open Windows PowerShell by clicking on the windows icon and searching for it. I will be providing code to paste, but you will need change placeholders with your own values.
Placeholders will look like this: `"YOUR_CLIENT_ID"`. Only change the text and leave the double quotation marks. I suggest you paste them first in a text editor to change the the placeholders and paste then paste them into powershell.
If powershell gives a warning because you are going to paste multiple lines of text click: "Paste anyway".

First we need to authorize the app. Because we can't leave the scope empty, we use the permission that allows the app the read your email address. However this doesn't cause any harm since we don't need it and it will never be sent to DBF.

```
$clientId = "YOUR_CLIENT_ID"
$redirectUri = "http://localhost:3000"
$scope = [uri]::EscapeDataString("user:read:email")
$state = [guid]::NewGuid().ToString()

$authorizeUrl = "https://id.twitch.tv/oauth2/authorize?client_id=$clientId&redirect_uri=$([uri]::EscapeDataString($redirectUri))&response_type=code&scope=$scope&state=$state"

Start-Process $authorizeUrl
```

You might need to press enter after `Start-Process $authorizeUrl`.
Now a browser window should open and you should be asked to authorize this app, click authorize. redirected, your browser may say "This site can't be reached", but this doesn't matter.

Click on the address bar, the address should look like this: `http://localhost:3000/?code=THE_AUTHORIZATION_CODE&scope=...&state=...`, copy THE_AUTHORIZATION_CODE (without the &) and paste it into your notepad.
You can now close the tab.

##### <u>MacOS and Linux</u>
