# react-native-braintree

A react native interface for integrating payments using [Braintree's v.zero SDK](https://developers.braintreepayments.com/start/overview).
![Accepts Credit/Debit Cards](/Screenshots/vzero.png)

## Usage

### Setup
```js
var BTClient = require('react-native-braintree');
BTClient.setup(<token>);
```
You can find a demo client token [here](https://developers.braintreepayments.com/start/hello-client/ios/v3).

To take advantage of [One Touch](https://developers.braintreepayments.com/guides/one-touch/overview/ios/v3), there are additional setup required:

1. Register a URL scheme in Xcode (should always start with your Bundle ID)
  ![Register URL Scheme](/Screenshots/urlscheme.png)

2. Use setupWithURLScheme instead, passing the url scheme you have registered in previous step
```js
var BTClient = require('react-native-braintree');
BTClient.setupWithURLScheme(<token>, <url scheme>);
```

3. Add this delegate method (callback) to your AppDelegate.m
```objc
  #import <RNBraintree.h>

  - (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation {
    return [[RNBraintree sharedInstance] application:application openURL:url sourceApplication:sourceApplication annotation:annotation];
  }
```

4. Optionally, install the [fake PayPal wallet app](https://github.com/braintree/fake-wallet-app-ios) in your simulator to simulate and test responses

### Show Payment Screen
v.zero
```js
BTClient.showPaymentViewController(function(err, nonce) {
  //payment succeeded, pass nonce to server
});
```

PayPal only
```js
BTClient.showPayPalViewController(function(err, nonce) {
  //payment succeeded, pass nonce to server
});
```

## Installation
1. `react-native init BTRNSample` (skip for existing projects)
2. Run `npm install react-native-braintree --save` to add the package
3. Inside the ios directory, create a Podfile:

  ```ruby
  source 'https://github.com/CocoaPods/Specs.git'
  pod 'React', :subspecs => ['Core', 'RCTImage', 'RCTNetwork', 'RCTText', 'RCTWebSocket'], :path => '../node_modules/react-native'
  pod 'react-native-braintree', :path => '../node_modules/react-native-braintree'
  ```

4. Run `pod install`.  This installs the Braintree iOS SDK and a new workspace is created.

5. Open `BTRNSample.xcworkspace`

6. Under your app target -> build settings, look for `Other Linker Flags` and add `$(inherited)`

  ![Accepts Credit/Debit Cards](/Screenshots/linker.png)

7. Build and run project!  If it fails the first time, clean and rebuild.

## Requirements

Tested with:
* Node 4.1.0
* npm 2.14.3
* react-native 0.8.0

## License

react-native-braintree is available under the MIT license. See the LICENSE file for more info.
