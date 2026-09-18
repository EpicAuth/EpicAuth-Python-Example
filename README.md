# EpicAuth-Python-Example : Please star 🌟

EpicAuth Python example SDK for https://EpicAuth.cc license key API auth.

## Copyright License

EpicAuth is licensed under **Elastic License 2.0**

* You may not provide the software to third parties as a hosted or managed
service, where the service provides users with access to any substantial set of
the features or functionality of the software.

* You may not move, change, disable, or circumvent the license key functionality
in the software, and you may not remove or obscure any functionality in the
software that is protected by the license key.

* You may not alter, remove, or obscure any licensing, copyright, or other notices
of the licensor in the software. Any use of the licensor’s trademarks is subject
to applicable law.

Thank you for your compliance, we work hard on the development of EpicAuth and do not appreciate our copyright being infringed.

## ⚠️ Important Notice

This repository has been **modified** to only include `main.py`.  
All other example files have been removed for simplicity.  

> If you want to use the official EpicAuth Python SDK as published on PyPI, you can install it directly with:

```bash
pip install epicauth
```
## **What is EpicAuth?**

EpicAuth is an simple authentication system with cloud hosting plans as well. Client SDKs available for [C#](https://github.com/EpicAuth/EpicAuth-CSHARP-Example), [C++](https://github.com/EpicAuth/EpicAuth-CPP-Example), [Python](https://github.com/EpicAuth/EpicAuth-Python-Example), [Java](https://github.com/EpicAuth/EpicAuth-JAVA-api), [JavaScript](https://github.com/EpicAuth/EpicAuth-JS-Example), [VB.NET](https://github.com/EpicAuth/EpicAuth-VB-Example), [PHP](https://github.com/EpicAuth/EpicAuth-PHP-Example), [Rust](https://github.com/EpicAuth/EpicAuth-Rust-Example), [Go](https://github.com/EpicAuth/EpicAuth-Go-Example), [Lua](https://github.com/EpicAuth/EpicAuth-Lua-Examples), [Ruby](https://github.com/EpicAuth/EpicAuth-Ruby-Example), and [Perl](https://github.com/EpicAuth/EpicAuth-Perl-Example). EpicAuth has several unique features such as memory streaming, webhook function where you can send requests to API without leaking the API, discord webhook notifications, ban the user securely through the application at your discretion. Feel free to join https://t.me/EpicAuth if you have questions or suggestions.

## **How to compile?**

You can either use Pyinstaller or Nuitka.

Links:
- Nuitka: https://nuitka.net/
- Pyinstaller: https://pyinstaller.org/

Pyinstaller:
- Basic command: `pyinstaller --onefile main.py`

Nutika:
- Basic command: `python -m nuitka --follow-imports --onefile main.py`

## **`EpicAuthApp` instance definition**

Visit https://EpicAuth.cc/app/ and select your application, then click on the **Python** tab

It'll provide you with the code which you should replace with in the `main.py` file.

```PY
EpicAuthApp = EpicAuth(
    name = "", #App name (Manage Applications --> Application name)
    ownerid = "", #Owner ID (Account-Settings --> OwnerID)
    version = "",
    hash_to_check = getchecksum()
)
```

## **Initialize application**

```PY
EpicAuthApp.init()

handle_auto_update(EpicAuthApp)

if not EpicAuthApp.response.success:
    print("\n Status: "+ EpicAuthApp.response.message)
    sys.exit(1)
```

## **Display application information**

```py
EpicAuthApp.fetchStats()
print(f"""
App data:
Number of users: {EpicAuthApp.app_data.numUsers}
Number of online users: {EpicAuthApp.app_data.onlineUsers}
Number of keys: {EpicAuthApp.app_data.numKeys}
Application Version: {EpicAuthApp.app_data.app_ver}
Customer panel link: {EpicAuthApp.app_data.customer_panel}
""")
```

## **Check session validation**

Use this to see if the user is logged in or not.

```py
print(f"Current Session Validation Status: {EpicAuthApp.check()}")
```

## **Check blacklist status**

Check if HWID or IP Address is blacklisted. You can add this if you want, just to make sure nobody can open your program for less than a second if they're blacklisted. Though, if you don't mind a blacklisted user having the program for a few seconds until they try to login and register, and you care about having the quickest program for your users, you shouldn't use this function then. If a blacklisted user tries to login/register, the EpicAuth server will check if they're blacklisted and deny entry if so. So the check blacklist function is just auxiliary function that's optional.

```py
if EpicAuthApp.checkblacklist():
    print("You've been blacklisted from our application.")
    os._exit(1)
```

## **Login with username/password**

```py
user = input('Provide username: ')
password = input('Provide password: ')
EpicAuthApp.login(user, password)
```

## **Register with username/password/key**

```py
user = input('Provide username: ')
password = input('Provide password: ')
license = input('Provide License: ')
EpicAuthApp.register(user, password, license)
```

## **Upgrade user username/key**

Used so the user can add extra time to their account by claiming new key.

> [!Warning]
> No password is needed to upgrade account. So, unlike login, register, and license functions - you should **not** log user in after successful upgrade.

```py
user = input('Provide username: ')
license = input('Provide License: ')
EpicAuthApp.upgrade(user, license)
```

## **Login with just license key**

Users can use this function if their license key has never been used before, and if it has been used before. So if you plan to just allow users to use keys, you can remove the login and register functions from your code.

```py
key = input('Enter your license: ')
EpicAuthApp.license(key)
```

## **User Data**

Show information for current logged-in user.

```py
print("\nUser data: ")
print("Username: " + EpicAuthApp.user_data.username)
print("IP address: " + EpicAuthApp.user_data.ip)
print("Hardware-Id: " + EpicAuthApp.user_data.hwid)

subs = EpicAuthApp.user_data.subscriptions  # Get all Subscription names, expiry, and timeleft
for i in range(len(subs)):
    sub = subs[i]["subscription"]  # Subscription from every Sub
    expiry = datetime.utcfromtimestamp(int(subs[i]["expiry"])).strftime(
        '%Y-%m-%d %H:%M:%S')  # Expiry date from every Sub
    timeleft = subs[i]["timeleft"]  # Timeleft from every Sub

    print(f"[{i + 1} / {len(subs)}] | Subscription: {sub} - Expiry: {expiry} - Timeleft: {timeleft}")
print("Created at: " + datetime.utcfromtimestamp(int(EpicAuthApp.user_data.createdate)).strftime('%Y-%m-%d %H:%M:%S'))
print("Last login at: " + datetime.utcfromtimestamp(int(EpicAuthApp.user_data.lastlogin)).strftime('%Y-%m-%d %H:%M:%S'))
print("Expires at: " + datetime.utcfromtimestamp(int(EpicAuthApp.user_data.expires)).strftime('%Y-%m-%d %H:%M:%S'))
print(f"Current Session Validation Status: {EpicAuthApp.check()}")
```

## **Show list of online users**

```py
onlineUsers = EpicAuthApp.fetchOnline()
OU = ""  # KEEP THIS EMPTY FOR NOW, THIS WILL BE USED TO CREATE ONLINE USER STRING.
if onlineUsers is None:
    OU = "No online users"
else:
    for i in range(len(onlineUsers)):
        OU += onlineUsers[i]["credential"] + " "

print("\n" + OU + "\n")
```

## **Application variables**

A string that is kept on the server-side of EpicAuth. On the dashboard you can choose for each variable to be authenticated (only logged in users can access), or not authenticated (any user can access before login). These are global and static for all users, unlike User Variables which will be dicussed below this section.

```py
* Get normal variable and print it
data = EpicAuthApp.var("varName")
print(data)
```

## **User Variables**

User variables are strings kept on the server-side of EpicAuth. They are specific to users. They can be set on Dashboard in the Users tab, via SellerAPI, or via your loader using the code below. `discord` is the user variable name you fetch the user variable by. `test#0001` is the variable data you get when fetching the user variable.

```py
* Set up user variable
EpicAuthApp.setvar("varName", "varValue")
```

And here's how you fetch the user variable:

```py
* Get user variable and print it
data = EpicAuthApp.getvar("varName")
print(data)
```

## **Application Logs**

Can be used to log data. Good for anti-debug alerts and maybe error debugging. If you set Discord webhook in the app settings of the Dashboard, it will send log messages to your Discord webhook rather than store them on site. It's recommended that you set Discord webhook, as logs on site are deleted 1 month after being sent.

You can use the log function before login & after login.

```py
* Log message to the server and then to your webhook what is set on app settings
EpicAuthApp.log("Message")
```

## **Ban the user**

Ban the user and blacklist their HWID and IP Address. Good function to call upon if you use anti-debug and have detected an intrusion attempt.

Function only works after login.

```py
EpicAuthApp.ban()
```

## **Enable Two Factor Authentication (2fa)**

Enable two factor authentication (2fa) on a client account.

```py
EpicAuthApp.enable2fa()
```

## **Disable Two Factor Authentication (2fa)**

Disable two factor authentication (2fa) on a client account.

```py
EpicAuthApp.disable2fa()
```

## **Logout session**

Logout the users session and close the application. 

This only works if the user is authenticated (logged in)
```py
EpicAuthApp.logout()
```

## **Download file**

> [!NOTE]
> Read documentation for EpicAuth files here https://docs.EpicAuth.cc/website/dashboard/files

Keep files secure by providing EpicAuth your file download link on the EpicAuth dashboard. Make sure this is a direct download link (as soon as you go to the link, it starts downloading without you clicking anything). The EpicAuth download function provides the bytes, and then you get to decide what to do with those. This example shows how to write it to a file named `text.txt` in the same folder as the program, though you could execute with RunPE or whatever you want.

`675651` is the file ID you get from the dashboard after adding file.

```py
* Download Files form the server to your computer using the download function in the EpicAuth class
bytes = EpicAuthApp.file("675651")
f = open("example.exe", "wb")
f.write(bytes)
f.close()
```

## **Chat channels**

Allow users to communicate amongst themselves in your program.

Example from the form example on how to fetch the chat messages.

```py
* Get chat messages
messages = EpicAuthApp.chatGet("CHANNEL")

Messages = ""
for i in range(len(messages)):
Messages += datetime.utcfromtimestamp(int(messages[i]["timestamp"])).strftime('%Y-%m-%d %H:%M:%S') + " - " + messages[i]["author"] + ": " + messages[i]["message"] + "\n"

print("\n\n" + Messages)
```

Example on how to send chat message.

```py
* Send chat message
EpicAuthApp.chatSend("MESSAGE", "CHANNEL")
```