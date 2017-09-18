---
layout: blog
title: WebDriver notes
date: 2017-09-18T08:31:57.645Z
thumbnail: /images/lost-places.jpg
rating: '5'
---
Edge not start clean session :(

workarround for cookies:

Hello Jason Juang

Thank you for taking the time to give us your feedback. We will be investigating this issue further. In the meantime, as a workaround please try to remove cookies before creating your new session. i.e. 
```
driver.Manage().Cookies.DeleteAllCookies(); see if this helps. 
```
Best regards,
The MS Edge Team

Specifics for IE:
https://github.com/nightwatchjs/nightwatch/wiki/Internet-Explorer-Setup
```
"selenium" : {
  ...
  "cli_args" : {
    "webdriver.ie.driver" : "C:/path/to/InternetExplorerDriver.exe"
  }
}
```
IE 11:

For 32-bit Windows installations, the key you must examine in the registry editor is 
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Internet Explorer\Main\FeatureControl\FEATURE_BFCACHE.
```
For 64-bit Windows installations, the key is
```
HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Internet Explorer\Main\FeatureControl\FEATURE_BFCACHE
```
Please note that the FEATURE_BFCACHE subkey may or may not be present, and should be created if it is not present. Inside this key, create a DWORD value named iexplore.exe with the value of 0.
```

Example config:
```
module.exports = {
  "src_folders": [
    "scripts/test"// Where you are storing your Nightwatch e2e tests
  ],
  "output_folder": "./reports", // reports (test outcome) output by nightwatch
  "selenium": { // downloaded by selenium-download module (see readme)
    "start_process": true, // tells nightwatch to start/stop the selenium process
    "server_path": "./node_modules/nightwatch/bin/selenium.jar", // the standard alone selenium server jar
    "host": "127.0.0.1",
    "port": 4444, // standard selenium port
    "cli_args": { // chromedriver is downloaded by selenium-download (see readme)

      "webdriver.chrome.driver" : "C:/chrome-win32/chromedriver.exe",
       "webdriver.gecko.driver" : "geckodriver.exe", //firefox driver location 
       "webdriver.ie.driver"    : "IEDriverServer.exe"
    }
  },
  "test_settings": {
    "default": {
      "screenshots": {
        "enabled": false, // if you want to keep screenshots
        "path": "" // save screenshots here
      },
      "globals": {
        "waitForConditionTimeout": 5000 // sometimes internet is slow so wait.
      },
      "desiredCapabilities": { // use Chrome as the default browser for tests
        "browserName": "chrome",
      },
    },
    "chrome": {
      "desiredCapabilities": {
        "browserName": "chrome",
        "javascriptEnabled": true, // turn off to test progressive enhancement
        "chromeOptions" :{
//        "args": [
//               'headless',
//                // Use --disable-gpu to avoid an error from a missing Mesa
//                // library, as per
//                // https://chromium.googlesource.com/chromium/src/+/lkgr/headless/README.md
//                'disable-gpu',
//            ],
        "binary": 'C:/chrome-win32/chrome.exe'
        },
        "selenium": {
            "cli_args": {
              "webdriver.chrome.driver" : "C:/chrome-win32/chromedriver.exe",
            },
        },
      },
    },
    "firefox":{
     "desiredCapabilities": {
        "browserName": 'firefox',
        "javascriptEnabled": true,
        "marionette": false,
        "acceptSslCerts": true,
        "acceptInsecureCerts" :true        
      },
        "selenium": {
                "cli_args": {
                    "webdriver.gecko.driver" : "geckodriver.exe" ,
                    //"webdriver.firefox.bin" : 'C:/tools/firefox_v39/firefox.exe'
                },
        },      
    },
    "ie" :{
        "desiredCapabilities": {
        "browserName": 'internet explorer',
        "javascriptEnabled": true,
        "acceptSslCerts": true
      },  
    }
  }
}
```

After initial failures of IE, this setup makes them work:
```
{
  "src_folders" : ["./test/src"],
  "test_workers" : false,
  "selenium" : {
    "start_process" : true,
    "server_path" : "selenium/selenium-server-standalone-3.4.0.jar",
    "port" : 4444,
    "cli_args" : {
        "webdriver.ie.driver" : "selenium/IEDriverServer.exe",
        "webdriver.ie.driver.logfile": "ielog.txt",
        "webdriver.ie.driver.loglevel": "DEBUG"
    }
  },
  "test_settings" : {
    "default" : {
      "desiredCapabilities" : {
        "browserName": "internet explorer",
        "javascriptEnabled": true,
        "acceptSslCerts": true,
        "requireWindowFocus": true,
        "nativeEvents": true,
        "elementScrollBehavior": 1
      }
    }
  }
}
```
Critical options was requireWindowFocus and elementScrollBehavior
