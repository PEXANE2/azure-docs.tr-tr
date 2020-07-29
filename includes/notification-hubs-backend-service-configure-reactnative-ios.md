---
author: alexeystrakh
ms.author: alstrakh
ms.date: 06/11/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: e67f53e26f2081e1aa12f7c0e6a219c240f5027a
ms.sourcegitcommit: bcb962e74ee5302d0b9242b1ee006f769a94cfb8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86060519"
---
### <a name="configure-required-ios-packages"></a>Gerekli iOS paketlerini yapılandırma

Paket, uygulama oluşturulurken [otomatik olarak bağlanır](https://github.com/react-native-community/cli/blob/master/docs/autolinking.md) . Tüm yapmanız gereken, yerel pods 'leri yüklemektir:

```bash
npx pod-install
```

### <a name="configure-infoplist-and-entitlementsplist"></a>Info. plist ve yetkilendirmeler. plist yapılandırma

1. "PushDemo/iOS" klasörünüze gidin ve "PushDemo. xcworkspace" çalışma alanını açın, en üstteki projeyi "PushDemo" seçin ve "Imzalama & özellikleri" sekmesini seçin.

1. Paket tanımlayıcısını, sağlama profilinde kullanılan değerle eşleşecek şekilde güncelleştirin.

1. -"+" Düğmesini kullanarak iki yeni özellik ekleyin:

    - Arka plan modu özelliği ve işaret uzak bildirimleri.
    - Anında iletme bildirimleri özelliği

### <a name="handle-push-notifications-for-ios"></a>İOS için anında iletme bildirimlerini işle

1. "AppDelegate. h" öğesini açın ve aşağıdaki içeri aktarmayı ekleyin:

    ```objective-c
    #import <UserNotifications/UNUserNotificationCenter.h>
    ```

1. "AppDelegate" tarafından desteklenen protokollerin listesini, şunu ekleyerek güncelleştirin `UNUserNotificationCenterDelegate` :

    ```objective-c
    @interface AppDelegate : UIResponder <UIApplicationDelegate, RCTBridgeDelegate, UNUserNotificationCenterDelegate>
    ```

1. "AppDelegate. d" öğesini açın ve gerekli tüm iOS geri çağırmaları yapılandırın:

    ```objective-c
    #import <UserNotifications/UserNotifications.h>
    #import <RNCPushNotificationIOS.h>

    ...

    // Required to register for notifications
    - (void)application:(UIApplication *)application didRegisterUserNotificationSettings:(UIUserNotificationSettings *)notificationSettings
    {
     [RNCPushNotificationIOS didRegisterUserNotificationSettings:notificationSettings];
    }

    // Required for the register event.
    - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
    {
     [RNCPushNotificationIOS didRegisterForRemoteNotificationsWithDeviceToken:deviceToken];
    }

    // Required for the notification event. You must call the completion handler after handling the remote notification.
    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo
    fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
    {
      [RNCPushNotificationIOS didReceiveRemoteNotification:userInfo fetchCompletionHandler:completionHandler];
    }

    // Required for the registrationError event.
    - (void)application:(UIApplication *)application didFailToRegisterForRemoteNotificationsWithError:(NSError *)error
    {
     [RNCPushNotificationIOS didFailToRegisterForRemoteNotificationsWithError:error];
    }

    // IOS 10+ Required for localNotification event
    - (void)userNotificationCenter:(UNUserNotificationCenter *)center
    didReceiveNotificationResponse:(UNNotificationResponse *)response
             withCompletionHandler:(void (^)(void))completionHandler
    {
      [RNCPushNotificationIOS didReceiveNotificationResponse:response];
      completionHandler();
    }

    // IOS 4-10 Required for the localNotification event.
    - (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification
    {
     [RNCPushNotificationIOS didReceiveLocalNotification:notification];
    }

    //Called when a notification is delivered to a foreground app.
    -(void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      completionHandler(UNAuthorizationOptionSound | UNAuthorizationOptionAlert | UNAuthorizationOptionBadge);
    }
    ```
