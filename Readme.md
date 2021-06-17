# Placeholder with requirements
The deilverable is an application that loads wallet.xenioscoin.com. This is the main functionality of the app. First and foremost the wallet needs to work as intended, including the QR code scanning functionality. There are some additional requirements:
#### Core Functionality (must be present)
1. **Re-Authenticating user:** After the initial authentication of the user in the app, the user is permenantly logged in (questionmark on the permenent this needs to be tested). We need to provide an extra level of assurance that the user is authorised to open the wallet app.
    * Ask the user for PIN/Biometric Authenctication
    * If the user continiously fails to provide valid PIN/Biometric we consider the phone compromised and delete ALL data. The user will have to re-log in to the web application.
    * The PIN must be stored in a secure manne as statehed in: Encrypted Secrets [MSTG-STORAGE‑14]: Example in cordova [cordova-plugin-secure-storage-echo](https://github.com/mibrito707/cordova-plugin-secure-storage-echo).
    * When the application leaves the running state and falls in the background (on Android e.g. `onPause` in the lifecycle) we need to ask for user PIN/Biometric again.
2. **Determine Network State:** The state of the network needs to be determined. If the device is off-line we need to let the user know and ask that they re-connect. 
3. **Determine Jailbrake/Root Status:** We need to determine whether the phone is rooted/jailbroken. Most banking apps will not run on rooted devices. I disagree with that. The user owns the device and they should be able to run Xenios on their device no matter how the CHOSE to configure it. Our responsibility is to:
    * Notify the user that the device has been detected jailbroken/rooted and inform them of the added risk.
    * The user Accepts the risk, and the app runs as intended.
    * This message should be presented on first run only (a counter can easily be used to determine this)
#### Added Core Security Features (we need to try our best to incorporate all of them)
1. **Certificate pinning**: This is a counter measure for man in the middle attacks. What we need to do is to check whether the SSL certificate presented by the server is the expected one. [OWASP - thorough article](https://owasp.org/www-community/controls/Certificate_and_Public_Key_Pinning), [Medium a bit more lightweight](https://medium.com/walmartglobaltech/implementation-of-ssl-pinning-7e57e280cc49). The mechanism is the followin:
    * The certificate of the server (wallet.xenioscoin.com) is retrieved
    * The certificate signing authority is checked for validity against the signature hadrcoded in the app.
    * If not valid, cache is deleted user is warned app exists.
    * If valid apps works as intended.