# GTNetMon Swift Package

### Integrate network monitoring capabilities in Swift projects! 

![Platform iOS](https://img.shields.io/badge/Platform-iOS-informational)
![Platform macOS](https://img.shields.io/badge/Platform-macOS-informational)
![Language](https://img.shields.io/badge/Language-Swift-orange)
![License](https://img.shields.io/github/license/gabrieltheodoropoulos/GTRest.svg)

## About

GTNetMon is a lightweight Swift library that detects whether a device is connected to Internet, it identifies the connection type (wifi, cellular, and more), and monitors for changes in the network status. For both iOS & macOS.

> ***Do you want GTNetMon as a pod? [Check this out](https://github.com/gabrieltheodoropoulos/GTNetMon).***

## Integrating GTNetMon

To integrate `GTNetMon` into your projects follow the next steps:

1. Copy the repository's URL to GitHub.
2. Open your project in Xcode.
3. Go to menu **File > Swift Packages > Add Package Dependency...**.
4. Paste the URL, select the package when it appears and click Next.
5. In the *Rules* leave the default option selected (*Up to Next Major*) and click Next.
6. Finally select the *GTNetMon-Swift-Package* package and select the *Target* to add to; click Finish.

## Using GTNetMon

Use **`GTNetMon`** singleton class to access all publicly available properties and methods through its **`shared`** instance.

Available properties are:

 * **`isConnected`**: Indicates whether the device is connected to Internet or not.
 * **`connectionType`**: The connection type as a `GTNetMon.ConnectionType` value (see next).
 * **`availableConnectionTypes`**: A collection of the available connection types to the device at a given moment.
 * **`isExpensive`**: An indication that the connection is expensive while the device is connected to Internet through cellular network. Note that this flag is more accurate when used in iOS versions >= 12.0.
 * **`isMonitoring`**: It indicates whether network status changes are being monitored or not.

 In addition to the above, there are the following two methods:

 * **`startMonitoring()`**: It starts monitoring for network status changes.
 * **`stopMonitoring()`**: It stops monitoring for network status changes.


### Important

1. It is recommended to start and stop monitoring in the `applicationDidBecomeActive(_:)` and `applicationWillResignActive(_:)` methods of the AppDelegate class respectively.
2. Classes that want to be notified about monitored network status changes should observe for the **`GTNetMonNetworkStatusChangeNotification`** notification. It is posted whenever a change in the connection occurs.


### Usage Example in iOS

Start and stop monitoring in the *AppDelegate.swift* file:

```swift
func applicationDidBecomeActive(_ application: UIApplication) {
    GTNetMon.shared.startMonitoring()
}

func applicationWillResignActive(_ application: UIApplication) {
    GTNetMon.shared.stopMonitoring()
}
```

A simple view controller sample implementation that monitors for network changes:

```swift
import UIKit
import GTNetMon

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        
        // Observe for the GTNetMonNetworkStatusChangeNotification notification.
        NotificationCenter.default.addObserver(self, selector: #selector(handleStatusChange(notification:)), name: .GTNetMonNetworkStatusChangeNotification, object: nil)
    }

    
    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)
        showConnectionInfo()
    }
    
    
    deinit {
        NotificationCenter.default.removeObserver(self)
    }
    
    
    @objc func handleStatusChange(notification: Notification) {
        self.showConnectionInfo()
    }
    
    
    func showConnectionInfo() {
        print("Is connected: \(GTNetMon.shared.isConnected)")
        
        "\nAvailable Connection Types:"
        for type in GTNetMon.shared.availableConnectionTypes {
            print(type.toString())
        }
        
        print("\n\nCurrent Connection Type: \(GTNetMon.shared.connectionType.toString())")
        print("Is Expensive: \(GTNetMon.shared.isExpensive)")
    }
    
}
```

### About `GTNetMon.ConnectionType`

`GTNetMon.ConnectionType` is an *enum* with the following cases:

* wifi
* cellular
* wiredEthernet
* other
* undefined


## Other

In iOS >= 12.0, the new `Network` framework of the iOS SDK is used to retrieve network information. In older iOS versions, `SCNetworkReachability` API is used instead.


## Version

Current up-to-date version is 1.0.3.


## License

GTNetMon is licensed under the MIT license.
