[![Pub Version](https://img.shields.io/pub/v/place_picker_google?color=8A2BE2)](https://pub.dev/packages/place_picker_google)
[![popularity](https://img.shields.io/pub/popularity/place_picker_google?color=8A2BE2)](https://pub.dev/packages/place_picker_google/score)
[![likes](https://img.shields.io/pub/likes/place_picker_google?color=8A2BE2)](https://pub.dev/packages/place_picker_google/score)

# Place Picker Google

A comprehensive, cross-platform google maps place picker library for Flutter.

The package provides common operations for Place picking, 
Places autocomplete, My Location, and Nearby places from google maps given you have enabled 
`Places API`, `Maps SDK for Android`, `Maps SDK for iOS` and `Geocoding API` for your API key.

## A key difference

Different than Google's Place Picker, `Place Picker` by default **doesn't** search for places according to where the user is pointing the map to. Instead, it shows only the nearby places in the **current** location.

This was intentional and the reason is simple. By using the **/nearbysearch** from [Google Places Web API](https://developers.google.com/places/web-service/search#PlaceSearchRequests) we are going to be charged *a lot* for each map movement.

![NearbySearch warning](https://github.com/rtchagas/pingplacepicker/blob/master/images/nearby_search_warning.png?raw=true)

According to [Nearby Search pricing](https://developers.google.com/maps/billing/understanding-cost-of-use#nearby-search) each request to the API is going to cost 0.04 USD per each (40.00 USD per 1000).

To avoid the extra cost of **/nearbysearch**, `Place Picker` relies on Place API's **findCurrentPlace()** that is going to cost 0.030 USD per each  (30.00 USD per 1000).

Moreover, we don't fire a new request each time the user moves the map.

### Enabling nearby searches

If you do want to fetch places from a custom location or refresh them when the user moves the map, you must enable /nearbysearch queries in `Place Picker`.

To do that, enable `enableNearbyPlaces` flag in the package.

## Screenshots

### iOS & Android

| <img src="https://github.com/joafc96/place_picker_google/raw/main/assets/iOS_place_picker_google_basic.png" alt="iOS basic" width="210"> | <img src="https://github.com/joafc96/place_picker_google/raw/main/assets/Android_place_picker_google_basic.png" alt="Android basic" width="210"> | 
|:----------------------------------------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------------------------------------------------------------------:|

### Places Autocomplete & Nearby Places

| <img src="https://github.com/joafc96/place_picker_google/raw/main/assets/iOS_place_picker_google_autocomplete.png" width="210"> | <img src="https://github.com/joafc96/place_picker_google/raw/main/assets/iOS_place_picker_google_nearby_places.png" width="210"> | 
|:-------------------------------------------------------------------------------------------------------------------------------:|:--------------------------------------------------------------------------------------------------------------------------------:|

### My Location &&  Preview
| <img src="https://github.com/joafc96/place_picker_google/raw/main/assets/iOS_place_picker_google_my_location.png" width="210"> | <img src="https://github.com/joafc96/place_picker_google/raw/main/assets/place_picker_google.gif" width="210"> |
|:------------------------------------------------------------------------------------------------------------------------------:|:--------------------------------------------------------------------------------------------------------------:|

## Getting Started

* Get an API key at <https://cloud.google.com/maps-platform/>.

* Enable Google Map SDK for each platform.
    * Go to [Google Developers Console](https://console.cloud.google.com/).
    * Choose the project that you want to enable Google Maps on.
    * Select the navigation menu and then select "Google Maps".
    * Select "APIs" under the Google Maps menu.
    * To enable Google Maps for Android, select "Maps SDK for Android" in the "Additional APIs" section, then select "ENABLE".
    * To enable Google Maps for iOS, select "Maps SDK for iOS" in the "Additional APIs" section, then select "ENABLE".
    * Make sure the APIs you enabled are under the "Enabled APIs" section.

* You can also find detailed steps to get started with Google Maps Platform [here](https://developers.google.com/maps/gmp-get-started).

### Android

Specify your API key in the application manifest `android/app/src/main/AndroidManifest.xml`:

1. Set the `minSdkVersion` in `android/app/build.gradle`:

```groovy
android {
    defaultConfig {
        minSdkVersion 21
    }
}
```

This means that app will only be available for users that run `Android SDK - 21` or higher.

2. Specify your API key in the application manifest `android/app/src/main/AndroidManifest.xml`:

```xml
<manifest ...
    <application ...
        <!-- TODO: Add your Google Maps API key here -->
        <meta-data android:name="com.google.android.geo.API_KEY"
               android:value="YOUR ANDROID KEY HERE"/>
        <activity ..../>
    </application>
</manifest>
```

### Note

The following permissions are not required to use Google Maps Android API v2, but are recommended.

`android.permission.ACCESS_COARSE_LOCATION` Allows the API to use WiFi or mobile cell data (or both) to determine the
device's location. The API returns the location with an accuracy approximately equivalent to a city block.

`android.permission.ACCESS_FINE_LOCATION` Allows the API to determine as precise a location as possible from the
available location providers, including the Global Positioning System (GPS) as well as WiFi and mobile cell data.

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
```

### iOS

Specify your API key in the application delegate `ios/Runner/AppDelegate.m`:

```objectivec
#include "AppDelegate.h"
#include "GeneratedPluginRegistrant.h"
#import "GoogleMaps/GoogleMaps.h"
@implementation AppDelegate
- (BOOL)application:(UIApplication *)application
    didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
  // TODO: Add your Google Maps API key
  [GMSServices provideAPIKey:@"YOUR KEY HERE"];
  [GeneratedPluginRegistrant registerWithRegistry:self];
  return [super application:application didFinishLaunchingWithOptions:launchOptions];
}
@end
```

Or in your swift code, specify your API key in the application delegate `ios/Runner/AppDelegate.swift`:

```swift
import UIKit
import Flutter
import GoogleMaps
@UIApplicationMain
@objc class AppDelegate: FlutterAppDelegate {
  override func application(
    _ application: UIApplication,
    didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?
  ) -> Bool {
    // TODO: Add your Google Maps API key
    GMSServices.provideAPIKey("YOUR KEY HERE")
    GeneratedPluginRegistrant.register(with: self)
    return super.application(application, didFinishLaunchingWithOptions: launchOptions)
  }
}
```

### Note

On iOS you'll need to add the following entries to your Info.plist file (located under ios/Runner) in order to access the device's location.

Simply open your Info.plist file and add the following:

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>This app needs access to location when open.</string>
<key>NSLocationAlwaysUsageDescription</key>
<string>This app needs access to location when in the background.</string>
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>This app needs access to location when open and in the background.</string>
```

### Web

You'll need to modify the `web/index.html` file of your Flutter Web application to include the Google Maps JS SDK.

Get an API Key for Google Maps JavaScript API. 
Get started [here](https://developers.google.com/maps/documentation/javascript/get-api-key).

Modify the `<head>` tag of your `web/index.html` to load the Google Maps JavaScript API, like so:

```html

<head>
    <!-- // Other stuff -->
  <!-- TODO: Add your Google Maps API key -->
    <script src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY"></script>
</head>
```

Check [the `google_maps_flutter_web` README](https://pub.dev/packages/google_maps_flutter_web)
for the latest information on how to prepare your App to use Google Maps on the
web.

## Setup

Add this to your package's `pubspec.yaml` file:

```yaml
dependencies:
  place_picker_google: ^0.0.6
```

Now in your `Dart` code, you can use:
```dart
import 'package:place_picker_google/place_picker_google.dart';
```

### Basic usage

You can use PlacePicker by pushing to a new page using Navigator, OR put as a child of any widget.  
When the user picks a place on the map, it will return result to `onPlacePicked` with `LocationResult` type.
Alternatively, you can build your own way with `selectedPlaceWidgetBuilder` and fetch result from it (See the instruction below).

```dart
PlacePicker(
        apiKey: Platform.isAndroid
            ? 'GOOGLE_MAPS_API_KEY_ANDROID'
            : 'GOOGLE_MAPS_API_KEY_IOS',
        onPlacePicked: (LocationResult result) {
          debugPrint("Place picked: ${result.formattedAddress}");
        },
        initialLocation: const LatLng(
          29.378586,
          47.990341,
        ),
        searchInputConfig: const SearchInputConfig(
          padding: EdgeInsets.symmetric(
            horizontal: 16.0,
            vertical: 8.0,
          ),
          autofocus: false,
          textDirection: TextDirection.ltr,
        ),
        searchInputDecorationConfig: const SearchInputDecorationConfig(
          hintText: "Search for a building, street or ...",
        ),
    ),
```

### Customizing selected place UI

By default, when a user selects a place by using auto complete search or dragging/tapping the map, 
we display the information at the bottom of the map.

However, if you don't like this UI/UX, simply override the builder using `selectedPlaceWidgetBuilder`. 

**Note that using this customization WILL NOT INVOKE `onPlacePicked` callback**

## Packages Used

Below are the information about the packages used.

PACKAGE | INFO
---|---
[http](https://pub.dev/packages/http) | To consume HTTP resources
[uuid](https://pub.dev/packages/uuid) | Generate unique id's
[geolocator](https://pub.dev/packages/geolocator) | Access to location services
[google_maps_flutter](https://pub.dev/packages/google_maps_flutter) | Access to Google Maps widget

## Feature Requests and Issues

Please file feature requests and bugs at the [issue tracker][tracker].

[tracker]: https://github.com/joafc96/place_picker_google/issues/new

## Contributing

Issues and PRs welcome. Unless otherwise specified, all contributions to this lib will be under MIT license.