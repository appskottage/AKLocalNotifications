# AKLocalNotifications

## Usage 

### Single fire notification

Notification that repeats from one Date to another with a time interval period

```swift

// The date you would like the notification to fire at
let triggerDate = Date().addingTimeInterval(300)

let firstNotification = AKNotification(identifier: "firstNotification", alertTitle: "Notification Alert", alertBody: "You have successfully created a notification", date: triggerDate, repeats: .none)

let scheduler = AkNotificationScheduler()
scheduler.scheduleNotification(notification: firstNotification)
scheduler.scheduleAllNotifications()
```

### Repeating Notification starting at a Date

The configuration of the repetition is chosen in the repeats parameter that can be [ .none, .minute, .hourly, .daily, .monthly, .yearly] .

```swift

let firstNotification = AKNotification(identifier: "firstNotification", alertTitle: "Notification Alert", alertBody: "You have successfully created a notification", date: Date(), repeats: .minute)

let scheduler = AKNotificationScheduler()
scheduler.scheduleNotification(notification: firstNotification)
scheduler.scheduleAllNotifications()
```

### Notification that repeats from one Date to another with a time interval period

This is useful to setup notifications to repeat every specific time interval for in a specific time period of the day.
```swift

let scheduler = AKNotificationScheduler()

// This notification repeats every 15 seconds from a time period starting from 15 seconds from the current time till 5 minutes from the current time

scheduler.repeatsFromToDate(identifier: "First Notification", alertTitle: "Multiple Notifications", alertBody: "Progress", fromDate: Date().addingTimeInterval(15), toDate: Date().addingTimeInterval(300) , interval: 15, repeats: .none )
scheduler.scheduleAllNotifications()

```
Note: Since this library takes care of the 64 notification limit you would want to call scheduler.scheduleAllNotifications() in your AppDelegate file as well. 

### Modifying elements of the notification

You can modify elements of the notification before scheduling. Publically accessible variables include:

repeatInterval, alertBody, alertTitle, soundName, fireDate, attachments, launchImageName, category

```swift

let firstNotification = AKNotification(identifier: "firstNotification", alertTitle: "Notification Alert", alertBody: "You have successfully created a notification", date: Date(), repeats: .minute)

// You can now change the repeat interval here
firstNotification.repeatInterval = .yearly

// You can add a launch image name
firstNotification.launchImageName = "Hello.png"

let scheduler = AKNotificationScheduler()
scheduler.scheduleNotification(notification: firstNotification)
scheduler.scheduleAllNotifications()
```
### Location Based Notification

The notification is triggered when a user enters a geo-fenced area.

```swift

let center = CLLocationCoordinate2D(latitude: 37.335400, longitude: -122.009201)
let region = CLCircularRegion(center: center, radius: 2000.0, identifier: "Headquarters")
region.notifyOnEntry = true
region.notifyOnExit = false

let locationNotification = AKNotification(identifier: "LocationNotification", alertTitle: "Notification Alert", alertBody: "You have reached work", region: region )

let scheduler = AKNotificationScheduler()
scheduler.scheduleNotification(notification: locationNotification)
scheduler.scheduleAllNotifications()
```

### Adding action buttons to a notification

```swift

 let scheduler = AKNotificationScheduler()
        
 let standingCategory = AKCategory(categoryIdentifier: "standingReminder")
        
 standingCategory.addActionButton(identifier: "willStand", title: "Ok, got it")
 standingCategory.addActionButton(identifier: "willNotStand", title: "Cannot")
        
 scheduler.scheduleCategories(categories: [standingCategory])

```
Don't forget to the set the notification category before scheduling the notification using

```swift
notification.category = "standingReminder"
```

### Cancelling a notification

```swift

 scheduler.cancelNotification(notification: notification)
      

```
