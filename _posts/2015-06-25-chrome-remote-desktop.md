---
layout: post
title:  "Chrome Remote Desktop"
date:   2015-06-25 21:57:48
categories: blog
---

Had known Parallels Access and use it with my Corporate desktop, it must say it's a good idea and solutions to by pass Corporate firewalls. Since PA use DNS traffic to control the host machine behind CF, I can use mobile or tablet to access the my computer inside my company network.

Trying Chrome Remote Desktop with my Linux Arch box is another option. And this is the how to:

1. Find latest PKG at AUR. The one in the latest comment and makepkg.
2. Install the package
3. Manually create these folder and file:
>.config/chrome-remote-desktop
>.chrome-remote-desktop-session which contain the "exec openbox-session" or whatever ... i3 ?

4. Create one group chrome-remote-desktop and add yourself to that group.
5. Install chrome remote desktop on your Chrome browser on arch.
6. Open the extension in the menu list and try to enable, it will fetch the json config into .config/chrome-remote-desktop folder. 
7. Install chrome remote desktop android app on your mobile
8. Refresh and try to connect.

Once it can connect you can use your mobile to remote control the host.

![Screenshot](https://lh4.googleusercontent.com/zSqKR3oAObwAoRdIYz7JZLlfTmWvW0aTwPmIOcz5PPPME_pSJdLIBq0OPVfDc-YGQx-_r_fqW0qBN3o=w1256-h587-rw  "Connect")