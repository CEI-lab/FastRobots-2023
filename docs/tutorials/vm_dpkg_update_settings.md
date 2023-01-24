# Modify Ubuntu's Software Update Settings

The Ubuntu VM is configured to update and download various software updates (firefox, vscode, security updates, etc) on a daily basis. 
This may sometimes interfere when applications are to be manually installated/updated using "apt" from the terminal. 
If that happens, you may notice an error as shown: 
```bash
E: Could not get lock /var/lib/dpkg/lock-frontend - open (11: Resource temporarily unavailable)
E: Unable to acquire the dpkg frontend lock (/var/lib/dpkg/lock-frontend), is another process using it?
```
Ubuntu is probably performing background updates and is preventing you from installing/updating any software at the same time in order to prevent race conditions. 
Give it 10-15 minutes to finish what it is doing. If the issue persists or happens too often, follow the instructions below:

1. Click on "Show Applications" on the bottom left of the dock in Ubuntu.
2. Open the application "Software & Updates".
3. Navigate to the "Updates" tab. 
4. Change "Automatically check for updates" to "Weekly" (or "Never") from the drop down list.
5. Change "When there are security updates" to "Display Immediately" from the drop down list.
6. Change "When there are other updates" to "Display weekly" from the drop down list.
7. Click on the "Close" button and restart Ubuntu.

If the issue still persists and prevents you from installing any software using "apt", refer the guide below or contact Vivek on campuswire.

Reference: 
1. https://phoenixnap.com/kb/fix-could-not-get-lock-error-ubuntu
