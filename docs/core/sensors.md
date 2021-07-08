---
title: "Sensors"
id: 'sensors'
---

Along with providing [location services](location.md), the companion app also adds several additional sensors to Home Assistant. If you don't want the `device_tracker` entity but still want sensors to update then just disable the entity in the [entity registry](https://www.home-assistant.io/integrations/config/#entity-registry) to stop location updates and keep sensor updates.

The sensors provided by the companion app are:

![iOS](/assets/iOS.svg)and ![macOS](/assets/macOS.svg)Sensor List

:::note Sensor Updates
On iOS, sensors update in limited situations: when your location changes, periodically when the app is running in the foreground, when you pull-to-refresh the web view, in the background at a rate determined by iOS, and when performing an "Update Sensors" or via "Send Location" shortcut or push notification.

On macOS, sensors update in the same situations as above as well as immediately when some sensors change.
:::

| Sensor | Attributes | Description |
| --------- | --------- | ----------- |
| `sensor.battery_level` | None | The current battery level of the device. |
| `sensor.battery_state` | `Low Power Mode` | The current charging state (either `Charging`, `Not Charging`, or `Full`) of the device. |
| `sensor.bssid` | None |  The MAC address of the wireless access point your phone is connected to. When off Wi-Fi, this sensor will report `Not Connected`. |
| `sensor.connection_type` | iOS: `Cellular Technology`<br />macOS: `Name`, `Hardware Address` | The current data connection being used by the device. On macOS, this requires app version 2021.2 or later. |
| `binary_sensor.focus` | None | Whether focus is currently enabled. Requires focus permission and iOS 15/macOS 12 update later this year. |
| `sensor.geocoded_location` | [See Below](#geocoded-location-sensor) | Calculated address based on GPS data. |
| `sensor.last_update_trigger` | None | The cause of the last update of location and sensor data from the device to Home Assistant |
| `sensor.ssid` | None | The human-readable name of the Wi-Fi network the device is currently connected to. When off Wi-Fi, this sensor will report `Not Connected`. |
| `sensor.storage` | [See Below](#storage-sensor) | The amount of total and available storage on your device. |

![iOS](/assets/iOS.svg)Specific Sensors

| Sensor | Attributes | Description |
| --------- | --------- | ----------- |
| `sensor.activity` | `confidence`, `types` | The current activity type as computed by iOS. Requires motion permissions to be enabled. |
| `sensor.average_active_pace` | None | The averaged pace calculated by iOS from pedometer data. Units: meters per second, m/s |
| `sensor.distance` | None | The estimated distance walked by the user since midnight local time. Units: meters, m |
| `sensor.floors_ascended` | None | The approximate number of floors ascended by walking since midnight local time. |
| `sensor.floors_descended` | None | The approximate number of floors descended by walking. Since |
| `sensor.sim_1` | [See Below](#cellular-provider-sensor) | Name of your cellular provider. |
| `sensor.sim_2` | [See Below](#cellular-provider-sensor) | Name of your cellular provider. |
| `sensor.steps` | None | The number of steps taken by the user. |

![macOS](/assets/macOS.svg)Specific Sensors

| Sensor | Attributes | Description |
| --------- | --------- | ----------- |
| `binary_sensor.active` | [See Below](#active-sensor) | Whether the device is actively being used. |
| `sensor.active_camera` | `All`, `Active` | The name of the active camera, or `Inactive` if not in use. |
| `sensor.active_microphone` | `All`, `Active` | The name of the active microphone, or `Inactive` if not in use. |
| `sensor.frontmost_app` | [See Below](#frontmost-app-sensor) | Requires app version 2021.2 or later. The name of the current frontmost app. |
| `binary_sensor.camera_in_use` | None | Whether a camera on the system is currently in use. |
| `binary_sensor.microphone_in_use` | None | Whether a microphone on the system is currently in use. |
| `sensor.displays` | `Display IDs`, `Display Names` | Requires app version 2021.2 or later. Number of displays connected to the device. |
| `sensor.primary_display_id` | None | Requires app version 2021.2 or later. ID of the current primary display, which is the display with the menu bar. In the form of a UUID, for example `BE82E2E6-EA40-4963-93AD-A0BDC9D2F18F`. |
| `sensor.primary_display_name` | None | Requires app version 2021.2 or later. Name of the current primary display, which is the display with the menu bar. |

Attributes such as `Cellular Technology` can be accessed with a template such as:

```
{{ states.sensor.connection_type.attributes['Cellular Technology'] }}
```

![Android](/assets/android.svg) Android Sensor List

Each ![Android](/assets/android.svg) sensor below can be enabled by navigating to the `App Configuration` page then selecting `Manage Sensors`. By default, most are disabled with the exception of the [battery sensors](#battery-sensors) and any that were given permission during onboarding. Once enabled the sensor will begin to send data to your Home Assistant server, if you chose to disable it later on the sensor will stop updating. Upon enabling a sensor the app will request for permissions, if required. If you do not see a sensor listed below then your device does not support it. Some of the sensors below offer custom settings for each of their own needs, read about each one to see what it offers. These settings can be found in the same location where you enable the sensor.

All sensors update during a periodic 15-minute interval and they will also update if other certain conditions are met. Read about each sensor below to understand how often to expect updates. During the 15-minute update interval a low priority foreground notification is temporarily created to prevent the Android system from halting the worker. This notification does not make a sound unless the user has installed a third-party app that intercepts notifications and decides to make a sound. If you are on Android 8.0+ you are free to minimize and/or turn off the notification channel for the `SensorWorker`.

| Sensor | Attributes | Description |
| --------- | --------- | ----------- |
| `binary_sensor.doze` | [See Below](#doze-sensor) | Whether or not the device is in doze mode. |
| `binary_sensor.interactive` | None | Whether or not the device is in an interactive state. |
| `binary_sensor.power_save` | None | Whether or not the device is in power saving mode. |
| [Activity Sensors](#activity-sensors) | See Below | The current activity type, sleep confidence and sleep segment as computed by Google. Requires activity recognition permissions on supported devices. |
| [App Data Sensors](#app-data_sensors) | None | Sensors that show how much data was sent or received by the app. |
| [App Importance Sensor](#app-importance-sensor) | None | The current importance of the app to determine if its in the foreground or cached. |
| `sensor.app_memory` | [See Below](#app-memory-sensor) | Information about the memory that is available for the app. |
| [App Usage Sensors](#app-usage-sensors) | None | Sensors that represent how the app is treated based on its usage. |
| [Audio Sensors](#audio-sensors) | None | Several different sensors around different types of audio detection from the device. |
| [Battery Sensors](#battery-sensors) | None | Several different sensors around the state of the devices battery. |
| `sensor.bluetooth_connection` | [See Below](#bluetooth-sensor) | The state of the sensor will reflect the total number of connected bluetooth devices. |
| `sensor.current_time_zone` | [See Below](#current-time-zone-sensor) | The current time zone the device is in. |
| `sensor.current_version` | None | The current installed version of the application. |
| `sensor.do_not_disturb` | None | The state of do not disturb on the device. |
| `sensor.geocoded_location` | [See Below](#geocoded-location-sensor) | Calculated address based on GPS data. |
| [Keyguard Sensors](#keyguard-sensors) | None | Sensors that represent various states about the device being locked or secured. |
| [Mobile Data Sensors](#mobile-data-sensors) | None | Several different sensors around the state of mobile data. |
| [Notification Sensors](#notification-sensors) | See Below | Details about the notifications on the device. |
| `sensor.last_reboot` | [See Below](#last-reboot-sensor) | The timestamp of the device's last reboot. |
| `sensor.last_update` | None | The state will reflect the intent that caused the last update to get sent. |
| `sensor.light` | None | The current level of illuminance the device detects. |
| `sensor.phone_state` | None | The only tracked states are `idle`, `ringing` or `offhook`, no other information is accessed. |
| `sensor.pressure` | None | The pressure reading from the device. |
| `sensor.proximity` | None | The current proximity reading from the device, certain devices will only show boolean value of `near` or `far`. |
| `sensor.public_ip` | None | The public IP address of the device as generated by ipify API. |
| `sensor.next_alarm` | [See Below](#next-alarm-sensor) | Date of the next scheduled alarm. |
| `sensor.sim_1` | [See Below](#cellular-provider-sensor) | Name of your cellular provider. |
| `sensor.sim_2` | [See Below](#cellular-provider-sensor) | Name of your cellular provider. |
| `sensor.steps` | None | The number of steps taken from the user since the last device reboot. Requires activity recognition permissions on supported devies. |
| [Storage Sensors](#storage-sensor) | [See Below](#storage-sensor) | The amount of total and available internal & external storage on your Android device. |
| [Traffic Stats Sensor](#traffic-stats-sensor) | None | Amount of data transmitted and received from mobile and total device usage since last reboot. |
| [WiFi Sensors](#connection-type-sensor) | None | Several different sensors around the state of WiFi. |

## Active Sensor
![macOS](/assets/macOS.svg) `sensor.active` provides whether the device is currently being used, based on a few different inputs which are provided as attributes to be informative.

| Attribute | Description |
| --------- | --------- |
| `Idle` | `true` when the machine is not any of the following attributes, but input devices haven't been used in a number of minutes. |
| `Screensaver` | `true` when the screensaver began playing to turn inactive |
| `Locked` | `true` when the device is showing the login screen |
| `Screen Off` | `true` when the screens have been turned off |
| `Fast User Switched` | `true` when switched to another user |
| `Sleeping` | `true` when the device is sleeping |
| `Terminating` | `true` when the app was quit available. Requires app version 2021.2 or later. |

This sensor has a setting to decide the duration that is considered 'idle'.

## Activity Sensors
![iOS](/assets/iOS.svg) `sensor.activity` provides the current motion activity as calculated by iOS along with the confidence of the calculations. Activities known by iOS and given by `sensor.activity` are:
*   `Stationary`
*   `Walking`
*   `Running`
*   `Automotive`
*   `Cycling`

If iOS is unable to calculate an activity from motion data, `Unknown` will be given.

It is possible for multiple activities to be returned, such as `Cycling` and `Stationary` (if you are cycling but at a stop light), the state of the sensor is simply the first of these return by iOS (not necessarily the most likely). A complete list of calculated activities is given by the `types` attribute. See [this post](https://nshipster.com/cmmotionactivity/#traveling-without-moving) by [@Mattt](https://twitter.com/mattt) over at [nshipster](https://nshipster.com/) for a description of how different scenarios yield multiple activities.

The `confidence` attribute corresponds how accurate iOS believes the report of the current activity is. Possible values are:
*   `Low`
*   `Medium`
*   `High`

![Android](/assets/android.svg) This sensor is only available on the full flavor of the Android app that is found in the Google Play Store, it is not available for the minimal flavor. For android the user will have a different set of states to go by:
*   `in_vehicle`
*   `on_bicycle`
*   `on_foot`
*   `running`
*   `still`
*   `tilting`
*   `walking`
*   `unknown`

The attribute for the state will reflect the `confidence` rating from the [Activity Recognition API](https://developers.google.com/location-context/activity-recognition). This sensor requires the [Activity Recognition permission](https://developer.android.com/reference/android/Manifest.permission#ACTIVITY_RECOGNITION).

![Android](/assets/android.svg)
The Sleep Confidence and Sleep Segment sensors utilize the new [Sleep API](https://developers.google.com/location-context/sleep) from Google services. Sleep Segment updates about once a day and Sleep Confidence will update about every 10 minutes. All data is provided by Google.

## App Data Sensors
![Android](/assets/android.svg)
These sensors will represent how much data was transmitted and received by the Home Assistant Android app, since the last device reboot. These sensors make use of the [Traffic Stats API](https://developer.android.com/reference/kotlin/android/net/TrafficStats).


## App Importance Sensor
![Android](/assets/android.svg)
This sensor will represent the state of the app to reflect if its in the `foreground` or `service` or any other state it can be. This sensor will update any time any other sensor has an update. See all of the Importance variables in [ActivityManager](https://developer.android.com/reference/android/app/ActivityManager.RunningAppProcessInfo) to see what they mean.

Possible states are:

*   `cached`
*   `cant_save_state`
*   `foreground`
*   `foreground_service`
*   `gone`
*   `not_running`
*   `perceptible`
*   `service`
*   `top_sleeping`
*   `visible`


## App Memory Sensor
![Android](/assets/android.svg)
This sensor will represent how much memory is being used by the application. The attributes will include how much memory is free and available for the application. This sensor makes use of the [Runtime API](https://developer.android.com/reference/java/lang/Runtime).


## App Usage Sensors
![Android](/assets/android.svg)
These sensors will represent how the Android system is treating the app based on its usage. There is one binary sensor `app_inactive` which will report whether or not the system currently considers the app to be inactive. The other sensor `app_standby_bucket` will reflect the current standby bucket that the Android system considers for the app. Standby buckets determine how much an app will be restricted from running background tasks such as jobs and alarms. Both of these sensors make use of the [UsageStatsManager API](https://developer.android.com/reference/android/app/usage/UsageStatsManager).

Possible states for `app_standby_bucket` sensor (please refer to the API linked above for their definitions):

*   `active`
*   `frequent`
*   `rare`
*   `restricted`
*   `working_set`
*   `never`


## Audio Sensors
![Android](/assets/android.svg) <br />
These sensors use the [AudioManager](https://developer.android.com/reference/kotlin/android/media/AudioManager?hl=en) so the state will represent the ringer mode on the device, possible values are `normal`, `vibriate` or `silent`. The sensor will update any time the ringer mode on the device has changed. There are also additional attributes that trigger updates as mentioned below:

| Sensor | Description |
| --------- | --------- |
| `audio_mode` | The current audio mode of the device can be either: `normal`, `ringing` (identical to [phone sensor](#phone-state-sensor)), `in_call`, `in_communication` or `unknown` |
| `is_headphones` | Boolean value if headsets or headphones are plugged in, will update if device detects them. |
| `is_mic_muted` | Boolean value if the microphone is currently muted, Android 6.0+ will update as this value changes. |
| `is_music_active` | Boolean value if the device is actively playing music. |
| `is_speakerphone_on` | Boolean value if the device speakerphone is enabled, Android 6.0+ will update as this value changes. |
| `ringer_mode` | The ringer mode on the device, possible values are `normal`, `vibriate` or `silent` |
| `volume_level_*` | The current device volume level for the given volume attributes: `alarm`, `call`, `music`, `ring` |


## Battery Sensors
![iOS](/assets/iOS.svg) <br />
The Battery State sensor (`sensor.battery_state`) provides information on the current status of the devices battery. The three possible values are `Charging`, `Not Charging`, or `Full` when the device is 100 % charged. The Battery Level sensor (`sensor.battery_level`) reports the current battery level of the device from 0–100 %. The charge level is reflected in the sensor icon. Additionally there is a "Low Power Mode" attribute that reports `true` or `false` depending on whether your iOS device is in [Low Power Mode](https://support.apple.com/en-us/HT205234) or not.

![Android](/assets/android.svg)<br />
The battery sensors listed below describe the state of the battery for a few different data points. The sensor's icon reflects the charging status, and type of charging being used. The `battery_state`, `charger_type` and `is_charging` sensor will be updated when the device has a charger connected or disconnected. The `battery_health`, `battery_level` and `battery_temperature` sensors will be updated any time any of the other sensors get an update as well as when the device reports low battery or when it has recovered from the low battery alert. All of these sensors make use of [BatteryManager](https://developer.android.com/reference/android/os/BatteryManager).

| Sensor | Description |
| --------- | --------- |
| `battery_health` | The health of the battery |
| `battery_level` | The percentage of battery remaining |
| `battery_state` | The state of charging on the device |
| `battery_temperature` | The current battery temperature |
| `charger_type` | The type of charger being used on the device |
| `is_charging` | Whether or not the device is actively charging |


## Bluetooth Sensors
![Android](/assets/android.svg)<br />
This Bluetooth Connection state will be the total number of connected bluetooth devices. The sensor will update as soon as the bluetooth state of the device changes. This sensor makes use of Android's [Bluetooth](https://developer.android.com/reference/android/bluetooth/package-summary?hl=en) package.

| Attribute | Description |
| --------- | --------- |
| `Connected Paired Devices` | The list of paired devices that are currently connected. |
| `Connected Not Paired Devices` | The list of devices that are connected but not paired. |
| `Paired Devices` | The list of devices that are paired. |

There will also be a binary sensor for the `bluetooth_state` that will represent whether or not bluetooth is turned on for the device. This sensor will update anytime the state of bluetooth changes.

![Android](/assets/android.svg)
A BLE Transmitter sensor allows your device to transmit a BLE iBeacon.  This is useful in conjunction with projects like [roomassistant](https://www.room-assistant.io/) and [esp32-mqtt-room ](https://jptrsn.github.io/ESP32-mqtt-room/) to allow room level tracking.  The current transmitting ID (UUID-Major-Minor) is reported as an attribute that can be copied for use with these systems.

:::caution
This sensor can impact battery life, particularly if used wih Transmit Power set to High. The iBeacon is transmitted every second (low latency to save battery, but sufficient for room presence).
:::

Settings are available to change the UUID, Major and Minor masks. These can be used to change the overall identifier, as well as to allow groups, e.g. family phone devices can have particular Major value which can be whitelisted in apps like roomassistant. These settings are validated: UUID should be the [standard format](https://en.wikipedia.org/wiki/Universally_unique_identifier), Major and Minor need to be within 0 and 65535. There are also settings to change the Transmit power (between Ultra Low, Low, Medium and High) as well as as toggle to allow this sensor to be turned on when the Enable all sensors toggle is activated. This is set to false, to prevent this sensor draining battery unnecessarily.

A Transmit setting toggle will start or stop the BLE transmissions - this setting is also toggled via the notification command that can turn the services on and off. 


## Cellular Provider Sensor
The cellular provider sensor displays information about the user’s cellular service provider, such as its unique identifier and whether it allows VoIP calls on its network. `sensor.sim_1` corresponds to the physical SIM card installed and `sensor.sim_2` corresponds to the eSIM (this is only shown if the eSIM is enabled).

![Android](/assets/android.svg) Android users will see these sensors update anytime the network has changed, which makes use of [SubscriptionManager](https://developer.android.com/reference/android/telephony/SubscriptionManager?hl=en). These sensors require the [Read Phone State permission](https://developer.android.com/reference/android/Manifest.permission#READ_PHONE_STATE).

| Attribute | Description |
| --------- | --------- |
| `Carrier Name` | The name of the user’s home cellular service provider. |
| `Current Radio Technology` | ![iOS](/assets/iOS.svg) only. |
| `ISO Country Code` | The ISO country code for the user’s cellular service provider. |
| `Mobile Country Code` | The mobile country code (MCC) for the user’s cellular service provider. |
| `Mobile Network Code` | The mobile network code for the user’s cellular service provider. |
| `Carrier ID` |  |
| `Allows VoIP` | Indicates if the carrier allows making VoIP calls on its network. ![iOS](/assets/iOS.svg) |
| `Is Opportunistic` | An opportunistic subscription connects to a network that is limited in functionality and / or coverage. ![Android](/assets/android.svg) |
| `Data Roaming` | Is data roaming enabled for the device. ![Android](/assets/android.svg) |


## Connection Type Sensor
![iOS](/assets/iOS.svg)<br />
The following connection types are known by the companion app:
*   `Wi-Fi`
*   `Cellular`
*   `No Connection`

A more specific description of the data connection can be found in the `Cellular Technology` attribute of the sensor (which only appears when on cellular). Possible values for this attribute are:

*   `4G`
*   `3G`
*   `2G`
*   `Cellular`
*   `No Connection`

If the connection type is not recognized, either `Unknown` or `Unknown Technology` will be returned.

![Android](/assets/android.svg)<br />
For Android several different types of connection sensors are available and they will update when a network state change has been detected:

| Sensor | Description |
| --------- | --------- |
| `wifi_connection` | The name of the current connected network |
| `bssid` | The mac address of the current connected network |
| `frequency` | The frequency band of the connected network |
| `wifi_ip_address` | The current IP address of the device on the network |
| `link_speed` | The current link speed of the device to the connected network |
| `signal_strength` | The signal strength of the device to the WiFi network |
| `wifi_state` | Whether or not WiFi is turned on for the device |

![Android](/assets/android.svg) The `bssid` sensor offers settings to let you rename the current mac address to help avoid the need for templates and secret usage in automations and the front end. This is generally useful if you have multiple access points and want an easy way to differentiate between them. These settings are turned off by default. These sensors require either [Background Location](https://developer.android.com/reference/android/Manifest.permission#ACCESS_BACKGROUND_LOCATION) or [Fine Location](https://developer.android.com/reference/android/Manifest.permission#ACCESS_FINE_LOCATION) permissions, depending on what version of Android you run.

## Current Time Zone Sensor
![Android](/assets/android.svg)
This sensor will represent the current time zone the device is in. There are also a few attributes to help describe this time zone. Data is provided by the [TimeZone API](https://developer.android.com/reference/java/util/TimeZone.html).

| Attribute | Description |
| --------- | ----------- |
| `in_daylight_time` | If the  time zone is currently observing daylight time. |
| `time_zone_id` | The display name of the time zone. |
| `time_zone_short` | The short name of the time zone. |
| `uses_daylight_time` | If the current time zone observes daylight time. |

## Current Version Sensor
![Android](/assets/android.svg)
This sensor will represent the current installed version of the Android app.

## Do Not Disturb Sensor
![Android](/assets/android.svg)<br />
This sensor will represent the state of Do Not Disturb (DND) on the device. The functionality of DND depends on the version of Android. Possible state values are `off`, `priority_only`, `total_silence`, `alarms_only`, `unavailable` or `unknown`. Not all states will show up on all versions of Android, for example a Pixel 4 XL will only show `off` or `priority_only`. If you never used DND you may see `unavailable` until you change the setting on your device. This sensor will update as soon as the state of DND changes. This sensor uses a [Global](https://developer.android.com/reference/kotlin/android/provider/Settings.Global?hl=en) variable that is not officially documented but has been available since Android 5.0.


## Doze Sensor
![Android](/assets/android.svg)<br />
This sensor is only available on devices running Android 6.0+. The state will reflect whether or not the device is in doze mode. The state will update immediately upon a state change and data is provided by [PowerManager](https://developer.android.com/reference/android/os/PowerManager.html). There is one attribute `ignoring_battery_optimizations` which will show `true` or `false` if the Companion app is ignoring battery optimizations. If you are curious about how the state actually changes you may test it by following these [outlined steps](https://developer.android.com/training/monitoring-device-state/doze-standby#testing_doze).


## Frontmost App Sensor
![macOS](/assets/macOS.svg)<br />
This sensor updates immediately when the frontmost app changes.

| Attribute | Description |
| --------- | --------- |
| `Bundle Identifier` | The bundle identifier of the app. For example, `io.home-assistant.example`. |
| `Is Hidden` | Whether the application is hidden. |
| `Launch Date` | The date (in ISO 8601, RFC 3339 format) the app was launched. For example, `2021-01-06T22:17:30-08:00`. |
| `Owns Menu Bar` | Whether the application "owns" the menu bar. For example, a menu-bar-only app will not change the contents of the menu bar, even when it is frontmost it is not necessarily as primary. |

## Geocoded Location Sensor
The [geocoded](https://.wikipedia.org/wiki/Geocoding) location sensor provides a user-friendly description of a users current location coordinates, often containing the name of the place, its address, and other relevant information. This sensor reports many detailed attributes allowing you to create useful [template sensors](https://www.home-assistant.io/components/template/).

Geocoding is handled directly by iOS's [MapKit](https://developer.apple.com/documentation/mapkit) and [Core Location](https://developer.apple.com/documentation/corelocation/converting_between_coordinates_and_user-friendly_place_names) services. In Android geocoding is handled by the internal [Geocoder](https://developer.android.com/reference/android/location/Geocoder).

| Attribute | Description |
| --------- | --------- |
| `Location` | The latitude and longitude coordinates of the placemark. |
| `Name` | The name of the placemark. ![iOS](/assets/iOS.svg) iOS only, for Android check the state of the sensor. |
| `Country` | The name of the country associated with the placemark. |
| `ISOCountryCode` | The abbreviated country name. |
| `TimeZone` | The time zone associated with the placemark. ![iOS](/assets/iOS.svg) iOS only |
| `AdministrativeArea` | The state or province associated with the placemark. |
| `SubAdministrativeArea` | Additional administrative area information for the placemark. |
| `PostalCode` | The postal code associated with the placemark. |
| `Locality` | The city associated with the placemark. |
| `SubLocality` | Additional city-level information for the placemark. |
| `Thoroughfare` | The street address associated with the placemark. |
| `SubThoroughfare` | Additional street-level information for the placemark. |
| `AreasOfInterest` | The relevant areas of interest associated with the placemark. ![iOS](/assets/iOS.svg) iOS only |
| `Ocean` | The name of the ocean associated with the placemark. ![iOS](/assets/iOS.svg) iOS only |
| `InlandWater` | The name of the inland water body associated with the placemark. ![iOS](/assets/iOS.svg) iOS only |

![Android](/assets/android.svg) Android users will have a sensor setting for the minimum required accuracy, that defaults to 200m. Users may adjust this to fit their own needs if they find inaccurate reports or not enough reports. This sensor requires either [Background Location](https://developer.android.com/reference/android/Manifest.permission#ACCESS_BACKGROUND_LOCATION) or [Fine Location](https://developer.android.com/reference/android/Manifest.permission#ACCESS_FINE_LOCATION) permissions, depending on what version of Android you run.

![iOS](/assets/iOS.svg) and ![macOS](/assets/macOS.svg) users will have a sensor setting for whether to use the name of an active Zone if present instead of the geocoded state, defaulting to not using it.

## Interactive Sensor
![Android](/assets/android.svg) This sensors state will reflect if the device is in an interactive state. This is typically when the screen comes on and off but may vary from device to device. This sensor will update as soon state changes are detected, data is provided by [PowerManager](https://developer.android.com/reference/android/os/PowerManager.html).

Using the [History Stats Integration](https://www.home-assistant.io/integrations/history_stats/), it is possible to monitor both the daily screen time `type: time` as well as the amount of times the screen has been turned on that day `type: count`.


## Keyguard Sensors
![Android](/assets/android.svg)

These sensors will reflect various states from the [Keyguard Manager](https://developer.android.com/reference/android/app/KeyguardManager). You will be able to determine if the device is actively locked, has a password setup or even if the device requires a password to unlock. These sensors will update with the periodic sensor interval.


## Notification Sensors
![Android](/assets/android.svg)<br />

### Last Notification

This sensor will reflect the last notification posted on the device. This sensor requires a special permission that the app will take the user to so they can grant access to notifications. This sensors state will default to the text of the notification or if not available the posting package name. This sensor offers a setting to enable an Allow List to let the user select which packages they wish to get notification data from, notifications sent by Home Assistant are always ignored. We recommend that users make use of this setting as by default all notifications including invisible ones get sent and might cause unintentional battery drain if the setting is left untouched. This can be very useful to integrate any app that sends a notification but does not offer direct integration (ex: food delivery apps or 2FA SMS codes). There are several attributes a user can expect to see, although not all attributes will contain data. This sensor makes use of the [NotificationListenerService API](https://developer.android.com/reference/android/service/notification/NotificationListenerService#onNotificationRemoved(android.service.notification.StatusBarNotification)).  More details on each attribute can be found in the [Notification Extras](https://developer.android.com/reference/android/app/Notification).

### Last Removed Notification

This sensor is similar to Last Notification except that it will update when a notification has been removed from the device, either by the user or an application. You can expect to see similar attributes for this sensor, some of which are outlined below. This sensor requires the same permission as mentioned up above. This sensor also has an allow list that functions similar to Last Notification.

### Active Notification Count

This sensor will reflect the total active notifications on the device. This count will include notifications that are persistent and/or silent. At times it may even include the Sensor Worker notification. This sensor will update whenever any of the other sensors have an update. This sensor requires the same permissions as mentioned in Last Notification. There is no allow list for this sensor.<br /><br />


Below you can find some details that can be given with some notifications.


| Attribute | Description |
| --------- | --------- |
| `android.appInfo` | App Info that contains the package name. |
| `android.infoText` | Text that is informative to the notification. |
| `android.largeIcon` | The large icon of the notification. |
| `android.progress` | The progress of the notification, if it has a progress bar. |
| `android.progressIndeterminate` | Whether or not the progress can be determined. |
| `android.progressMax` | The max position of the progress (ex: 100 for 100%). |
| `android.reduced.images` | If images on the notification were reduced. |
| `android.remoteInputHistory` | The most recent input for the notification. |
| `android.showChronometer` | If the chronometer is shown. |
| `android.showWhen` | If the notification should be shown at a specific time. |
| `android.subText` | The subtitle of the notification. |
| `android.text` | The text of the notification. |
| `android.title` | The title of the notification. |
| `is_clearable` | If the notification can be cleared. |
| `is_ongoing` | If the notification is persistent on the device. |
| `package` | The package that posted the notification. |
| `post_time` | The time the notification was posted on the device. |


### Media Session Sensor
![Android](/assets/android.svg)<br />
This sensor requires notification permissions in order to be enabled and send data. The state will be the playback state of the primary media session. If no media sessions are active then the state will be `unavailable`. Attributes will include a total count of active sessions and media data from all active sessions separated by package name. This sensor will update during the normal sensor update interval. To get the most out of this sensor we recommend using [Last Notification](#last-notification) to hook into your media apps to send faster updates. This sensor uses the [MediaController](https://developer.android.com/reference/android/media/session/MediaController) and [MediaSessionManager](https://developer.android.com/reference/android/media/session/MediaSessionManager) APIs to get the data.


## Last Reboot Sensor
![Android](/assets/android.svg)<br />
This sensors state will be the date and time of the last reboot from the device in UTC format. The sensor will update during the normal sensor update interval. The state will be `unavailable` if the timestamp cannot be determined. This sensor uses the [SystemClock](https://developer.android.com/reference/android/os/SystemClock?hl=en) and current [System](https://developer.android.com/reference/java/lang/System?hl=en) time to calculate the timestamp. This sensor offers a deadband setting, that defaults to 1 minute, to account for time calculation issues seen over certain carriers.

| Attribute | Description |
| --------- | --------- |
| `Local Time` | The date and time of the last reboot in local time. |
| `Time in Milliseconds` | The time date and time of the last reboot in milliseconds. |


## Last Update Trigger Sensor
![Android](/assets/android.svg)

For android this sensors state will reflect the [intent](https://developer.android.com/reference/android/content/Intent) of the most recent update sent. Additionally the sensor offers settings to allow the user to receive [app events](../integrations/app-events.md) from other Android apps that broadcast an intent. Users can register for as many intents as they like, an event will be sent to Home Assistant once the intent has been received. Once you save an intent be sure to restart the application to register for the intent.

![iOS](/assets/iOS.svg)<br />
This sensor displays exactly what caused the last update of location and sensor data from the device to Home Assistant.

| State | Description |
| --------- | --------- |
| Manual | A manual update is triggered when the user pulls to refresh. |
| Launch | Sensors are updated upon initial app launch. |
| Periodic | Updates periodically according to your settings in App Configuration -> Sensors. |
| Significant Location Change | Triggers when there has been a significant change in the device’s location, such as 500 meters or more. See [location](location.md) for additional details. |
| Geographic Region Entered | Triggered when entering any user-specified Home Assistant [zone](https://www.home-assistant.io/components/zone/) (also known as geofencing). |
| Geographic Region Exited | Triggered when exiting any user-specified Home Assistant [zone](https://www.home-assistant.io/components/zone/) (also known as geofencing). |
| Push Notification | [Requesting location updates](../notifications/notification-commands#request-location-updates) via push notification.  |
| Background Fetch | When the app refreshes sensor information in the background. |
| Siri | Location updates triggered via the [Siri Shortcuts](../integrations/siri-shortcuts.md) "Send Location" shortcut. |
| iBeacon Region Entered | Triggered when an iBeacon is seen that corresponds to a known zone. |
| Registration | Triggered once when the app is first connected to your Home Assistant instance. |
| Signaled | Triggered when the app detects a change, such as battery state changes, while running. |

## Light Sensor
![Android](/assets/android.svg)<br />
This sensor will reflect the current level of illuminance the device detects. The sensor updates during the normal sensor update interval or with the other sensor updates and makes use of [Environment Sensors](https://developer.android.com/guide/topics/sensors/sensors_environment).

## Mobile Data Sensors
![Android](/assets/android.svg)<br />
Several different sensors around the state of mobile data. These sensors make use of [Settings.Global](https://developer.android.com/reference/kotlin/android/provider/Settings.Global?hl=en) and [TelephonyManager](https://developer.android.com/reference/android/telephony/TelephonyManager?hl=en) to get the mobile data states.

| Sensor | Description |
| ------ | ----------- |
| `mobile_data` | Whether or not mobile data is turned on for the device. |
| `mobile_data_roaming` | Whether or not mobile data roaming is turned on for the device. |

## Next Alarm Sensor
![Android](/assets/android.svg)<br />
This sensors state will be the date and time of the next alarm in UTC format. The sensor will update as soon as the next alarm is scheduled. The state will be `unavailable` when there is no next alarm. This sensor makes use of [AlarmManager](https://developer.android.com/reference/android/app/AlarmManager?hl=en) to get the next scheduled alarm which can be set by any app at any time. This sensor has settings that will let you create an allow list by selecting the packages you want to get alarm events from, just keep in mind the API is only able to get the next scheduled alarm. This setting is turned off by default.

| Attribute | Description |
| --------- | --------- |
| `Local Time` | The date and time of the next alarm in local time. |
| `Package` | The package that scheduled the next alarm. |
| `Time in Milliseconds` | The time date and time of the next alarm in milliseconds. |


## Pedometer Sensors
![iOS](/assets/iOS.svg)<br />
The pedometer sensors provide step-counting data from the devices built-in motion processor. They keep a tally of your daily on-foot activity, and reset at midnight. These sensors require motion permissions to be enabled.

| Sensor | Description |
| --------- | --------- |
| `sensor.steps` | The number of steps taken by the user. |
| `sensor.distance` | The estimated distance (in meters) traveled by the user. |
| `sensor.average_active_pace` | The average pace of the user, measured in seconds per meter. |
| `sensor.floors_ascended` | The approximate number of floors ascended by walking. |
| `sensor.floors_descended` | The approximate number of floors descended by walking. |

![Android](/assets/android.svg) Android users will only have a `sensor.steps` entity which will represent the total number of steps taken since the last device reboot. A recommended approach to getting your daily step count is to use the [Utility Meter integration](https://www.home-assistant.io/integrations/utility_meter) with `cycle: daily`. This sensor will update during the normal sensor update interval and makes use of the [Motion Sensor](https://developer.android.com/guide/topics/sensors/sensors_motion?hl=en). This sensor requires the [Activity Recognition permission](https://developer.android.com/reference/android/Manifest.permission#ACTIVITY_RECOGNITION).


## Phone State Sensor
![Android](/assets/android.svg)<br />
This sensor will only show up if a user explicitly grants the `Phone` permission for the app in your devices `App Info` screen. The only data tracked for this sensor are the following states: `idle`, `ringing`, `offhook`. This sensor will update anytime a phone state change is detected and makes use of [TelephonyManager](https://developer.android.com/reference/android/telephony/TelephonyManager?hl=en). This sensor requires the [Read Phone State permission](https://developer.android.com/reference/android/Manifest.permission#READ_PHONE_STATE).


## Power Save Sensor
![Android](/assets/android.svg)<br />
This sensor will show the state of power save mode on the device. Depending on the device this is usually a user configurable option to indicate when the device should enter a special power saving mode. The state will update as soon as a state change is detected and the sensor makes use of [PowerManager](https://developer.android.com/reference/android/os/PowerManager.html).


## Pressure Sensor
![Android](/assets/android.svg)<br />
This sensor will show the current pressure reading from the device. This sensor will update during the normal sensor update interval and makes use of [Environment Sensors](https://developer.android.com/guide/topics/sensors/sensors_environment).


## Proximity Sensor
![Android](/assets/android.svg)<br />
This sensor will show the current proximity reading from the device. This sensor will update during the normal sensor update interval. Not all devices report an actual reading so those devices will show either `near` or `far` depending if the sensors maximum range is `5`. This sensor makes use of [Position Sensors](https://developer.android.com/guide/topics/sensors/sensors_position?hl=en).


## Public IP Sensor
![Android](/assets/android.svg)<br />
This sensor uses the [ipify API](https://www.ipify.org/) in order to determine the devices public IP address. This sensor will update during the normal sensor update interval.


## Storage Sensor
![iOS](/assets/iOS.svg)<br />
This sensor displays information on the device storage. The file sizes reported are in Base-10.

| Attribute | Description |
| --------- | --------- |
| `Available` | The amount of available storage remaining on your device. |
| `Available (Important)` | The volume’s available capacity in bytes for storing important resources. |
| `Available (Opportunistic)` | The volume’s available capacity in bytes for storing nonessential resources. |
| `Total` | The total storage capacity of your device. |

![Android](/assets/android.svg)<br />
For Android the behavior is slightly different due to the differences in the 2 operating systems. The state will be the same as iOS where we show the percentage of free space, the attributes will not be identical. These sensors will update during the normal sensor update interval, calculations are done with the help of [StatFs](https://developer.android.com/reference/android/os/StatFs?hl=en).

`sensor.internal_storage`

| Attribute | Description |
| --------- | --------- |
| `Free internal storage` | The amount of free internal storage space remaining on your device. |
| `Total internal storage` | The total internal storage capacity of your device. |

`sensor.external_storage`

| Attribute | Description |
| --------- | --------- |
| `Free external storage` | The amount of free external storage remaining on your SD card, for devices without a SD card it will reflect `No SD Card`. |
| `Total external storage` | The total external storage of your SD card, for devices without a SD card it will reflect `No SD Card`. |


## Traffic Stats Sensor
![Android](/assets/android.svg)<br />
These sensors will show the total data transmitted and received by the device. There are both total and mobile sensors to use and the statistics reset on device reboot. These sensors use the [Traffic Stats API](https://developer.android.com/reference/android/net/TrafficStats).
