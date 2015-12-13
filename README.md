# Override OMAPI SE AccessControl
### Xposed Module 

### Module to be used with OpenMobile API 
Tested with Android 6.0.0 and OMAPI v2.05
<br/><br/>


### What's this ##

- This is a module for the [Xposed Framework](http://repo.xposed.info/). You need to have installed the Xposed framework (which is not made by me) on your phone. You need to have root for the installation of the Xposed framework (but not for this module).

### Some words to the OpenMobile API (OMAPI):
- The [Open Mobile API](http://simalliance.org/wp-content/uploads/2015/03/SIMalliance_OpenMobileAPI2_05_release-Feb142.pdf) is a 3rd party API which gives apps a way to access different types of secure elements (SE) within a device (as this is not possible with current stock Android API).
- Security: An issuer of a Secure Element can define **access rules** within the Secure Element (either in a specific AID applet called ARA (access rule application) or via a special file ARF (access rule file) placed on the Secure Element
- access rules are based on an app's signing certificate (hash)
- this rules together with the certificate hashes are **stored WITHIN the Secure Element**
- However, the component which enforces this access rules, **runs in software** on the Android platform (to be exact: within the SmartcardService.apk, which is the Android implementation of the Open Mobile API) and therefore can be targeted with the Xposed framework

### What this module can do for you?

- This module intercepts the `AccessControlEnforcer` class which is responsible for enforcing the access rules stored within the SE
- Currently this module simply contains a hardcoded constant `TARGET_APPLICATION_PACKAGE_NAME` which you should set to the package name of your own OMAPI client app *(sorry, no GUI or settings page)*
- any calls to the Open Mobile API from the app specified in this constant, will always be granted  access to the SE no matter what the access rules in the SE itself say


### Reasons why it might fail:
- your device doesn't have OpenMobile API (SmartcardService.apk) installed
- the OMAPI version in your device uses code obfuscation (proguard, or similiar)
- the OMAPI version in your device is a different one, than the one I tested with (3.0, 2.05)
- However: in all of these cases it still will be possible to circumvent the access rules from the SE (it just might be a little bit more work but the principal problem will be the same.)

So as a reminder to all developers of Secure Element applets: **Keep in mind that Secure Element Access Control always can be overriden by the device owner! The security rules are not enforced in the SE but on the (maybe insecure) host system!**

### Logs:
- this module logs into the Xposed Logfile which can bei either accessed via the Xposed installer app or (as root) on the device under: `/data/data/de.robv.android.xposed.installer/log/error.log`
