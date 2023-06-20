# SimpleConfig
UiPath excel config is a pain. This activity allows you to read/initiate a config dictionary from a simple flat file (e.g. txt, cfg).

[![image](https://i.postimg.cc/Wb0rkzSk/2023-06-15-14-43-12-Blank-Process1-Ui-Path-Studio-Community.png)](https://postimg.cc/Ln6ndHc9)

# Problems with standard excel config
* Speed! Initiating config from excel file is unreasonably slow. Reading Config with SimpleConfig is instantaneous.
* It doesn't allow you to keep it open at runtime, during development. As a developer, you always put things in config file as you develop/test. UiPath always yells at you if you forget the excel open at runtime.
* It doesn't allow to view git changes. If you check git "show changes" of an excel file, all you see is crypted data. Flat files allow you to view changes easily.
* Usual alternative to excel config in UiPath world, is the json format config. But json doesn't allow comments, and you need an extra dependency (UiPath.WebApi) to make it work. As well, json format doesn't allow easily to unpack assets or credentials
* You have to carry needlessly a lot of config objects throught the project, even if you use it only in select cases. With new activity, you can easily read any config (e.g. emails.txt, localization.txt) exactly where you need it


# How to install
* Download the desired nupkg to a local folder (for non-legacy projects, it requires both runtime/design nupkg).
* Add local folder to the packages feed inside UiPath Manage Packages.
* Drag and drop the SimpleConfig activity inside your project from Activities tab.

# How to use and couple specifications
* create a .txt file where you'll keep your config text (you can have multiple files where you store various configs, e.g. emails.txt, localization.txt), which you can read separately
* output of the activity is a Dictionary<string,object>
* "#" at the start of a line marks a comment line
* blank lines are ignored
* config lines are marked as "key = value" pairs. Only first "=" sign is used as a delimiter, it doesn't affect if your values will contain more "=" signs
* the key will be the Config("key"), you'll use in project to call the value object
* there are 3 special cases you can use in your config:
  1) reading credentials from Windows Credential. Prefix your generic windows credential key with "c_". Example: "c_winCred = NameOfCredential", will unpack into following config keys: "winCred_user" and "winCred_pass"
  2) reading assets from Orchetrator. Prefix your assets key with "a_". Example: "a_projectAssetName = NameOfAsset", will unpack into config key: "projectAssetName" with value of "NameOfAsset" from orchestrator
  3) reading credential asset from Orchestrator. Prefix your assets with "ca_". Example: "ca_credFromOrch = OrchCreds", will unpack into config keys: "credFromOrch_user" and "credFromOrch_pass"


# Example of a usable config.txt file
[![2023-06-20-19-16-18-E-Documents-config-txt-Notepad.png](https://i.postimg.cc/vZHjxjfF/2023-06-20-19-16-18-E-Documents-config-txt-Notepad.png)](https://postimg.cc/xchgW66t)
