This guide is based on https://medium.com/@dubistkomisch/set-up-2fa-two-factor-authentication-for-paypal-with-google-authenticator-or-other-totp-client-60fee63bfa4f - but some information is outdated.

There's a simpler method mentioned in the article, however I wouldn't really rely on a random website to touch any of my financial data.

What you'll need:  
- a TOTP generating app/program (e.g. KeePassXC for desktop, andOTP on Android)
- A running linux machine connected to the internet (A VM or a LiveCD works great since you won't have to clean up)

- You need python-pip and git - the command to install them depends on your OS, on Ubuntu it's:
`sudo apt install python-pip git qrencode`
![Install pip and git](https://github.com/Datenschutz/PayPal-2FA/blob/master/screenshots/1-installdeps.png)
- Clone this repository - it includes fixes that prevent the original package "vipaccess" from working and has some other improvements:  
![get vipaccess](https://github.com/Datenschutz/PayPal-2FA/blob/master/screenshots/2-getsource.png)
`git clone https://github.com/dlenski/python-vipaccess.git`
- Enter the directory that got pulled:  
![change dir](https://github.com/Datenschutz/PayPal-2FA/blob/master/screenshots/3-cd.png)
`cd python-vipaccess`
- Install vipaccess, this will install all dependencies via pip as well:  
![install vipaccess](https://github.com/Datenschutz/PayPal-2FA/blob/master/screenshots/4-installvip.png)
`pip install .`  
Since we are not installing as superuser, vipaccess won't be included in PATH, but that's fine.
- Go back to your home folder:
![go back home](https://github.com/Datenschutz/PayPal-2FA/blob/master/screenshots/5-gohome.png)
- Generate your Tokens - you need an active internet connection:  
![generate tokens](https://github.com/Datenschutz/PayPal-2FA/blob/master/screenshots/6-generatetokens.png)
`.local/bin/vipaccess provision -p -t VSMT`
- The command should succeed:
![success](https://github.com/Datenschutz/PayPal-2FA/blob/master/screenshots/7-success.png)
- Copy the otpauth:// URL:
![copy otpauth](https://github.com/Datenschutz/PayPal-2FA/blob/master/screenshots/8-copyotpauth.png)
- For mobile use: Generate a QR code, replace otpauth://XXX with the otpauth:// URL you just copied:
![generate QR](https://github.com/Datenschutz/PayPal-2FA/blob/master/screenshots/9-genqr.png)
`qrencode -o qr.png 'otpauth://XXX'`
This will produce qr.png sitting in your home directory. You can scan this QR code with any TOTP compatible app, I recommend andOTP for Android.
If you want to generate the tokens on a desktop PC, you could use KeePassXC. The secret is included in the othauth:// URL, it's what follows `secret=`
- Copy the ID that starts with VSMT:
![copy ID](https://github.com/Datenschutz/PayPal-2FA/blob/master/screenshots/10-copyid.png)
- Log into your PayPal account, then visit: https://www.paypal.com/webscr?cmd=_setup-security-key
- Select "Activate your security key".
- Paste the ID that starts with VSMT into the *serial number* field.
- Paste two Tokens that got generated by your TOTP app into the following fields.
- Be sure to reset the VM or shutdown the Live disk, since your private keys are in the clipboard and shell history.

If you have any suggestions, feel free to file an issue.
