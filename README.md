# Bypass [disable-devtool](https://github.com/theajack/disable-devtool)

(Working as of 2025-02-09)

There are websites that use [disable-devtool](https://github.com/theajack/disable-devtool) to prevent you from opening or using devtools. They typically prevent you from right clicking or using the keyboard shortcut to open devtools. Even if you successfully do so, they detect it and redirect you elsewhere. You can bypass this by using one of the following ways.


## Opening devtools

If the shortcut **F12** on Windows or **Option + ⌘ + I** on Mac do not work. Press the three vertically aligned dots in the top right corner of your Google Chrome or Microsoft Edge window. Under the section "More Tools", you'll see the option to select "Developer Tools" which opens the toolkit in your window.

Once devtools is open, the script may also have `debugger` statements that may interrupt your browsing experience. You can disable all debugger statements by going to "Sources", and clicking on "Deactivate breakpoints"

<img width="259" alt="Screenshot 2025-02-09 at 3 03 43 PM" src="https://gist.github.com/user-attachments/assets/93936ef3-084f-491e-99cd-bf2344650938" />

<img width="428" alt="Screenshot 2025-02-09 at 3 05 10 PM" src="https://gist.github.com/user-attachments/assets/ecd784bb-1fda-4807-ace3-7625c8004f66" />


## Bypass external script

When [disable-devtool](https://github.com/theajack/disable-devtool) is included as an external script, it can be disabled with a single line of javascript or using an url blocker extension.

- First try executing this line of javascript from your address bar as suggested here on [reddit](https://www.reddit.com/r/DataHoarder/comments/17jx5ia/comment/kycv2kj/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button)

    ```
    javascript:DisableDevtool.isSuspend = true
    ```

- Or you can bypass it by finding the url in the page source, and blocking it using an url blocker extension, as suggested here on [reddit](https://www.reddit.com/r/DataHoarder/comments/17jx5ia/comment/k73yybv/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button)

    1. If the website is `thewebsite.com`, go to `view-source:https://thewebsite.com`
    2. Find the url for the external script by searching for `disable-devtool`. It'll typically be 
        ```
        //cdn.jsdelivr.net/npm/disable-devtool
        ```
    4. Block the url using ublock or any browser url blocking extension of your choice


## Bypass bundled script

If [disable-devtool](https://github.com/theajack/disable-devtool) is bundled into the application, the solutions above will not work.

To bypass this, we'll have to modify the bundled script and disable it.

### Prerequisites

We are going to make some changes to the script. To do this, we have to enable overriding scripts.

Go to a new tab, and open devtools. In "Sources", under "Overrides" tab, check "Enable Local Overrides" and pick a folder to save the modified scripts locally.

<img width="284" alt="Screenshot 2025-02-09 at 3 18 03 PM" src="https://gist.github.com/user-attachments/assets/d429703b-2e1c-4747-afff-33f14b14bd9e" />

### Steps

1. We'll look for a literal from the source to find it's location in the bundle, I'm going to use `already running`

    <img width="597" alt="Screenshot 2025-03-10 at 1 20 31 PM" src="https://gist.github.com/user-attachments/assets/3fdeba5d-040c-4739-b1e9-f03469e6e73a" />

2. If the website is `thewebsite.com`, go to `view-source:http://thewebsite.com`
3. Search for the string literal `already running`.
4. If there is no match, find all included scripts by searching for `.js`
    
    <img width="601" alt="Screenshot 2025-02-09 at 2 52 06 PM" src="https://gist.github.com/user-attachments/assets/8f28dd5e-066d-4c5b-9c9b-c086cc50a75c" />

4. Follow the links to the bundled scripts and search within them, you'll usually find them in a script called `main` or `app`. I found mine in a script starting with `_app-`

    <img width="509" alt="Screenshot 2025-03-10 at 1 31 05 PM" src="https://gist.github.com/user-attachments/assets/644b5ce2-79cd-43a1-9610-922c18c0afaf" />

5. Open devtools (there should be no issue here since we are viewing the source, not the actual page). In "Search", find `already running` again and click on the result
    
    <img width="415" alt="Screenshot 2025-03-10 at 1 27 19 PM" src="https://gist.github.com/user-attachments/assets/8efc38f7-375c-46f4-a7ca-8e07c664e3e6" />
    
    <img width="354" alt="Screenshot 2025-03-10 at 1 37 54 PM" src="https://gist.github.com/user-attachments/assets/9ce3778c-b515-4f3e-bc02-75034eb1fd65" />

6. Add a return statement after this line so that the package is never initialized. Make sure to hit save (Ctrl/Cmd + S) after modifying the file

    <img width="415" alt="Screenshot 2025-03-10 at 1 20 00 PM" src="https://gist.github.com/user-attachments/assets/f84e330f-c913-4ee9-9966-b16a32eb06a5" />

Now, if you open devtools using the methods described under the [opening devtools](#opening-devtools) section, you'll find that [disable-devtool](https://github.com/theajack/disable-devtool) has been successfully disabled
