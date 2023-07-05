# Integration Of Appylar For iOS
  Appylar is an SDK framework created by Appylar that provides ad-serving capabilities for iOS mobile applications.
 <a name="readme-top"></a>
 <!-- TABLE OF CONTENTS -->
  <p>Implementation Guide for Developers: </p>
  <ol>
    <li>
      <a href="#about-appylar">About Appylar</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#ad-types">Ad Types</a>
    </li>
    <li><a href="#general-requirements">General Requirements</a></li>
    <li>
    <a href="#implementation">Implementation</a>
     <ul>
	<li><a href="#pod-installation">Pod Installation</a></li>
        <li><a href="#add-appylar">Add Appylar</a></li>
	<li><a href="#setup-the-configuration-for-your-app-and-listeners">Setup the configuration for your App and Listeners</a></li>
	<li><a href="#implementation-of-banner">Implementation of Banner</a></li>
	<li><a href="#add-interstitial-ads">Add Interstitial Ads</a></li>   
      </ul>
    </li>
    <li><a href="#sample-codes">Sample Codes</a></li>
  </ol>

## About Appylar
Appylar is a lightweight and user-friendly SDK for integrating ads into iOS applications, developed by Appylar. With this SDK, developers can easily integrate Appylar Ads into any type of iOS app

#### Built With
 * [Swift](https://docs.swift.org/assets/images/swift.svg)

## Ad Types
Appylar offers multiple ad types and provides developers with the flexibility to place ads anywhere within their application. The ad types available with Appylar are banners and interstitials.
 
## General Requirements
 * Appylar requires a minimum targeted version of iOS 12.0 or later.

## Implementation
To use Appylar Ads in your application, you need to follow these steps. Please note that additional implementation steps may be required based on your specific use case:
 #### Pod Installation
 Adding Pods to an Xcode project
 
 * Open a terminal window, and $ cd into your project directory.
 * Create a Podfile. This can be done by running $ pod init.
 * Add a CocoaPod by specifying pod '$Appylar' on a single line inside your target block.
     ```     
     target ‘App Name’ do
     pod 'Appylar'
     end
     ```
  * Save your Podfile.
  * Run $ pod install
  * Open the MyApp.xcworkspace that was created. This should be the file you use everyday to create your app.

 
 #### Add Appylar 

 Before proceeding with Appylar Ads integration, ensure that the Pods directory is available in your project. Once confirmed, you can import the necessary class into your UIViewController class

 ```swift
 import SwiftUI
 import Appylar
 ```

<p align="right"><a href="#readme-top">Back To Top</a></p>

#### Setup the configuration for your App and Listeners


1. To implement Appylar Ads in your iOS application, create an extension of App and init SDK. Additionally, implement the AppylarDelegate protocol within this extension. If you already have an application subclass in your project, you can use that instead.

```swift
import SwiftUI
import Appylar
@main
struct DemoApp: App {
    init() {
        AppylarManager.setEventListener(delegate: self, bannerDelegate: self, interstitialDelegate: self)
        AppylarManager.Init(appKey: "OwDmESooYtY2kNPotIuhiQ", adTypes: [.interstitial, .banner], testMode: true)
    }
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}

//Attach callbacks for initialization
extension DemoApp: AppylarDelegate{
    func onInitialized() {
        print("onInitialized")
    }
    
    func onError(error: String) {
        print("onError:\(error)")
    }
}
```

2. Initialize SDK with configuration:
```swift
import SwiftUI
import Appylar

@main
struct DemoApp: App {
    init() {
        AppylarManager.setEventListener(delegate: self, bannerDelegate: self, interstitialDelegate: self)
        AppylarManager.Init(                        
          		app_Key: "<YOUR_APP_KEY>"?? “”, //APP KEY provided by console for Development use
 			Adtypes: [AdType.BANNER, AdType.INTERSTITIAL],	//Types of Ads to integrate
			testmode: true // ‘True’ for development and ‘False’ for production, 
			)
          }
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```
#### Implementation of Banner 

1. To integrate the BannerView component into your design, you need to prepare a view from the contentView and set it to the BannerView type. Follow these steps:
 
```swift
    @State private var bannerView = BannerView()
```	 

2. Implement callback for banners.

```swift
//Attach callbacks for banner.
extension AppylarDemoApp: BannerViewDelegate {
    func onNoBanner() {
        print("onNoBanner()")
    }
    
    func onBannerShown() {
        print("onBannerShown()")
    }
}
```

3. Check Ad availability and show the Ad. <br>
For better performance and check the availability of ads you can `canShowAd()` function.

```swift
   if BannerView().canShowAd(){
           bannerView.showAd()
   }
```
   The parameter placement is optional, and it is up to the developer to decide whether to pass it or not.

4. For creating a BannerView as a representable, you need to define a struct that extends UIViewRepresentable.

```swift
      struct BannerViewWrapper: UIViewRepresentable {
         let bannerView: BannerView
    
         func makeUIView(context: Context) -> UIView {
              bannerView
         }
    
         func updateUIView(_ uiView: UIView, context: Context) {
         }
      }
```
5. To integrate the banner into your SwiftUI ContentView, you can use the following code:

```swift
      BannerViewWrapper(bannerView: bannerView)
```

6. To hide the banner.

```swift
   bannerView.hideBanner()
```
<p align="right"><a href="#readme-top">Back To Top</a></p>

#### Add Interstitial Ads

1. Make your `ViewController` of type `InterstitialViewController`.  
```swift
   class ViewController: InterstitialViewController
```

2. Implement callbacks for Interstitial.
```swift
//Attach callbacks for interstitial.
func onNoInterstitial(){
    	//Callback for when there is no Ad to show.  
   }
func onInterstitialShown() {
       //Callback for Ad shown.  
   }
func onInterstitialClosed() {
    	//Callback for close event of interstitial
   }
```	

3. Check Ad availablity and show the Ad.<br>
 For better performance and check the availability of ads you can `canShowAd()` function.
```swift
if  InterstitialViewController.canShowAd(){
                 // showAd function with the value of placement
  	         self.bannerView.showAd(placement: txtfieldEnterPlacement.text ?? "" )
		 // showAd function without placement parameter
		 self.bannerView.showAd()
}
```
   The parameter placement is optional, and it is up to the developer to decide whether to pass it or not.

4. For lock the orientation of interstitial in your `AppDelegate` and override a function in it.

```swift
      func application(_ application: UIApplication, supportedInterfaceOrientationsFor window: UIWindow?) -> UIInterfaceOrientationMask {
           return AppylarManager.supportedOrientation
      }
```
<p align="right"><a href="#readme-top">Back To Top</a></p>

## Sample Codes

1. For initialization
	
```swift
import SwiftUI
import Appylar
		
class ViewController: InterstitialViewController{
    	Override func viewDidLoad() {
		super.viewDidLoad()  
		//Attach callback listeners for SDK before initialization
                AppylarManager.setEventListener(delegate: self,bannerDelegate: self,interstitialDelegate: self) 
            	//Here ‘setEventListener’ is a method for AppylarManager
		//Initialization
           	AppylarManager.Init(                        
          		app_Key: "<YOUR_APP_KEY>"?? “”, //APP KEY provided by console for Development use    ["OwDmESooYtY2kNPotIuhiQ"]
 			Adtypes: [AdType.BANNER, AdType.INTERSTITIAL]	//Types of Ads to integrate
			orientations: [Orientation.PORTRAIT, Orientation.LANDSCAPE], 	//Supported orientations for Ads
			testmode: true // ‘True’ for development and ‘False’ for production, 
			)
     	}
}

```

2. For orientation lock of interstitial:

```swift
  func application(_ application: UIApplication, supportedInterfaceOrientationsFor window: UIWindow?) -> UIInterfaceOrientationMask {
           return AppylarManager.supportedOrientation
  }
```
3.  Implements for both the types:

```swift
import UIKit
import Appylar

class ViewController: InterstitialViewController { 
    	@IBAction func btnShowBannerDidTapped(_ sender: UIButton) {
            if BannerView.canShowAd(){    
	         // showAd function with the value of placement
  	         self.bannerView.showAd(placement: txtfieldEnterPlacement.text ?? "" )
		 // showAd function without placement parameter
		 self.bannerView.showAd()
            }
    	 }
    
    	@IBAction func btnHideBannerDidTapped(_ sender: UIButton) {
        	self.bannerView.hideBanner()
        	self.view.layoutIfNeeded()
    	}
    
    	@IBAction func btnShowIntersitialDidTapped(_ sender: UIButton) {
        	if  InterstitialViewController.canShowAd(){
                 // showAd function with the value of placement
  	         self.bannerView.showAd(placement: txtfieldEnterPlacement.text ?? "" )
		 // showAd function without placement parameter
		 self.bannerView.showAd()
       		} 
   	}
}
//Attach callbacks for Initialization.
extension ViewController : AppylarDelegate {
    func onInitialized() {
        Print("onInitialized() ")
    }
    
    func onError(error : String) {
        Print("onError() - \(error)")
    }
}  
//Attach callbacks for banner.
extension ViewController: BannerViewDelegate{
    func onNoBanner() {
        Print("onNoBanner()")
    }
    
    func onBannerShown() {
        Print("onBannerShown()")
    }
    
    
}
//Attach callbacks for interstitial.
extension ViewController: InterstitialDelegate{
    func onNoInterstitial() {
        Print("onNoInterstitial()")
    }
    
    func onInterstitialShown() {
        Print("onInterstitialShown()")
    }
    
    func onInterstitialClosed() {
        Print("onInterstitialClosed()")
    }
}
```
<p align="right"><a href="#readme-top">Back To Top</a></p>
