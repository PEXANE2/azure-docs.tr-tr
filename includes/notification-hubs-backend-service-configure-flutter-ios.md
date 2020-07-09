---
author: mikeparker104
ms.author: miparker
ms.date: 07/07/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 9a8b76c0887cff498edd90aedf5cc6eb674fc7e5
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86147820"
---
### <a name="configure-the-runner-target-and-infoplist"></a>Runner hedefini ve Info. plist 'i yapılandırın

1. **Visual Studio Code** **' de,**  +  **iOS** klasörü ' ne tıklayın ve ardından **Xcode 'da aç****' ı** seçin.

1. **Xcode**'Da, **Çalıştırıcısı** ' na tıklayın (klasörde değil, en üstteki **XCODEPROJ** ) ve ardından **Çalıştırıcısı** hedefini seçin ve ardından **& yeteneklerini imzalama**. **Tüm** derleme yapılandırması seçiliyken, **Takım**için geliştirici hesabınızı seçin. "İmzalamayı otomatik olarak Yönet" seçeneğinin işaretli olduğundan ve Imzalama sertifikanızın ve sağlama profilinizin otomatik olarak seçildiğinden emin olun.

    > [!NOTE]
    > Yeni sağlama profili değerini görmüyorsanız, **Xcode**tercihleri hesabı ' nı seçerek imzalama kimliği için profilleri yenilemeyi deneyin,  >  **Preferences**  >  **Account** ardından profilleri indirmek için **el ile profilleri indir** düğmesini seçin.

1. **+ Özelliği**' ne ve ardından **anında iletme bildirimleri**' ni arayın. Bu özelliği eklemek için **anında Iletme bildirimleri** **' ne çift tıklayın** .

1. **Info. plist** ' i açın ve **En düşük sistem sürümünü** **13,0**olarak ayarlayın.

    > [!NOTE]
    > Bu öğreticinin amaçları doğrultusunda yalnızca **iOS 13,0 ve üzeri** çalıştıran cihazlar desteklenir, ancak daha eski sürümleri çalıştıran cihazları desteklemek için genişletebilirsiniz.

1. **Runner. yetkilendirmeleri** açın ve **AP ortamı** ayarının **geliştirme**olarak ayarlandığından emin olun.

### <a name="handle-push-notifications-for-ios"></a>İOS için anında iletme bildirimlerini işle

1. **Denetim**  +  **Çalıştırıcısı** klasörüne (Çalıştırıcısı projesi Içinde) tıklayın ve ardından ad olarak **Hizmetler** 'ı kullanarak **Yeni Grup** **'** u seçin.

1. **Denetim**  +  **Hizmetler** klasörüne tıklayın ve ardından **yeni dosya...** **öğesini** seçin. Ardından, **Swift dosyası** ' nı seçin ve **İleri**' ye tıklayın. Ad için **Deviceınstalservıce hizmetini** belirtin ve ardından **Oluştur**' a tıklayın.

1. Aşağıdaki kodu kullanarak **Deviceınstalservıce. Swift** ' i uygulayın.

    ```swift
    import Foundation

    class DeviceInstallationService {

        enum DeviceRegistrationError: Error {
            case notificationSupport(message: String)
        }

        var token : Data? = nil

        let DEVICE_INSTALLATION_CHANNEL = "com.<your_organization>.pushdemo/deviceinstallation"
        let GET_DEVICE_ID = "getDeviceId"
        let GET_DEVICE_TOKEN = "getDeviceToken"
        let GET_DEVICE_PLATFORM = "getDevicePlatform"

        private let deviceInstallationChannel : FlutterMethodChannel

        var notificationsSupported : Bool {
            get {
                if #available(iOS 13.0, *) {
                    return true
                }
                else {
                    return false
                }
            }
        }

        init(withBinaryMessenger binaryMessenger : FlutterBinaryMessenger) {
            deviceInstallationChannel = FlutterMethodChannel(name: DEVICE_INSTALLATION_CHANNEL, binaryMessenger: binaryMessenger)
            deviceInstallationChannel.setMethodCallHandler(handleDeviceInstallationCall)
        }

        func getDeviceId() -> String {
            return UIDevice.current.identifierForVendor!.description
        }

        func getDeviceToken() throws -> String {
            if(!notificationsSupported) {
                let notificationSupportError = getNotificationsSupportError()
                throw DeviceRegistrationError.notificationSupport(message: notificationSupportError)
            }

            if (token == nil) {
                throw DeviceRegistrationError.notificationSupport(message: "Unable to resolve token for APNS.")
            }

            return token!.reduce("", {$0 + String(format: "%02X", $1)})
        }

        func getDevicePlatform() -> String {
            return "apns"
        }

        private func handleDeviceInstallationCall(call: FlutterMethodCall, result: @escaping FlutterResult) {
            switch call.method {
            case GET_DEVICE_ID:
                result(getDeviceId())
            case GET_DEVICE_TOKEN:
                getDeviceToken(result: result)
            case GET_DEVICE_PLATFORM:
                result(getDevicePlatform())
            default:
                result(FlutterMethodNotImplemented)
            }
        }

        private func getDeviceToken(result: @escaping FlutterResult) {
            do {
                let token = try getDeviceToken()
                result(token)
            }
            catch let error {
                result(FlutterError(code: "UNAVAILABLE", message: error.localizedDescription, details: nil))
            }
        }

        private func getNotificationsSupportError() -> String {

            if (!notificationsSupported) {
                return "This app only supports notifications on iOS 13.0 and above. You are running \(UIDevice.current.systemVersion)"
            }

            return "An error occurred preventing the use of push notifications."
        }
    }
    ```

    > [!NOTE]
    > Bu sınıf kanal için platforma özgü karşılığı uygular `com.<your_organization>.pushdemo/deviceinstallation` . Bu, uygulamanın **Deviceınstalyüklemehizmeti. Dart**içindeki flutter bölümünde tanımlanmıştır. Bu durumda, çağrılar ortak koddan yerel ana bilgisayara yapılır. **<your_organization>** bu kullanıldığı her yerde kendi kuruluşunuzla değiştirdiğinizden emin olun.
    >
    > Bu sınıf, Bildirim Hub 'ı kayıt yükünün bir parçası olarak benzersiz bir KIMLIK ( [UIDevice. ıdentifierforvendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor) değeri kullanılarak) sağlar.

1. *Notificationregistrationservice*adlı **Hizmetler** klasörüne başka bir **Swift dosyası** ekleyin, ardından aşağıdaki kodu ekleyin.

    ```swift
    import Foundation

    class NotificationRegistrationService {

        let NOTIFICATION_REGISTRATION_CHANNEL = "com.<your_organization>.pushdemo/notificationregistration"
        let REFRESH_REGISTRATION = "refreshRegistration"

        private let notificationRegistrationChannel : FlutterMethodChannel

        init(withBinaryMessenger binaryMessenger : FlutterBinaryMessenger) {
           notificationRegistrationChannel = FlutterMethodChannel(name: NOTIFICATION_REGISTRATION_CHANNEL, binaryMessenger: binaryMessenger)
        }

        func refreshRegistration() {
            notificationRegistrationChannel.invokeMethod(REFRESH_REGISTRATION, arguments: nil)
        }
    }
    ```

    > [!NOTE]
    > Bu sınıf kanal için platforma özgü karşılığı uygular `com.<your_organization>.pushdemo/notificationregistration` . Bu, uygulamanın **Notificationregistrationservice. Dart**içindeki flutter bölümünde tanımlanmıştır. Bu durumda, çağrılar yerel ana bilgisayardan ortak koda yapılır. Bu, kullanıldığı her yerde **<your_organization>** kendi kuruluşunuzla değiştirmek için dikkatli olun.

1. *Notificationactionservice*adlı **Hizmetler** klasörüne başka bir **Swift dosyası** ekleyin, ardından aşağıdaki kodu ekleyin.

    ```swift
    import Foundation

    class NotificationActionService {

        let NOTIFICATION_ACTION_CHANNEL = "com.<your_organization>.pushdemo/notificationaction"
        let TRIGGER_ACTION = "triggerAction"
        let GET_LAUNCH_ACTION = "getLaunchAction"

        private let notificationActionChannel: FlutterMethodChannel

        var launchAction: String? = nil

        init(withBinaryMessenger binaryMessenger: FlutterBinaryMessenger) {
            notificationActionChannel = FlutterMethodChannel(name: NOTIFICATION_ACTION_CHANNEL, binaryMessenger: binaryMessenger)
            notificationActionChannel.setMethodCallHandler(handleNotificationActionCall)
        }

        func triggerAction(action: String) {
           notificationActionChannel.invokeMethod(TRIGGER_ACTION, arguments: action)
        }

        private func handleNotificationActionCall(call: FlutterMethodCall, result: @escaping FlutterResult) {
            switch call.method {
            case GET_LAUNCH_ACTION:
                result(launchAction)
            default:
                result(FlutterMethodNotImplemented)
            }
        }
    }
    ```

    > [!NOTE]
    > Bu sınıf kanal için platforma özgü karşılığı uygular `com.<your_organization>.pushdemo/notificationaction` . Bu, uygulamanın **Notificationactionservice. Dart**içindeki flutter bölümünde tanımlanmıştır. Çağrılar, bu durumda her iki yönde de yapılabilir. **<your_organization>** bu kullanıldığı her yerde kendi kuruluşunuzla değiştirdiğinizden emin olun.

1. **Appdelegate. Swift**içinde, daha önce oluşturduğunuz hizmetlere bir başvuru depolamak için değişken ekleyin.

    ```swift
    var deviceInstallationService : DeviceInstallationService?
    var notificationRegistrationService : NotificationRegistrationService?
    var notificationActionService : NotificationActionService?
    ```

1. Bildirim verilerini işlemek için **Processnotificationactions** adlı bir işlev ekleyin. Bu eylemi koşullu olarak tetikleyin veya eylem uygulama başlatma sırasında işleniyorsa daha sonra kullanmak üzere saklayın.

    ```swift
    func processNotificationActions(userInfo: [AnyHashable : Any], launchAction: Bool = false) {
        if let action = userInfo["action"] as? String {
            if (launchAction) {
                notificationActionService?.launchAction = action
            }
            else {
                notificationActionService?.triggerAction(action: action)
            }
        }
    }
    ```

1. **Deviceınstald hizmeti**için **belirteç** değerini ayarlayarak **Didregisterforremotenocertificate ationswithdevicetoken** işlevini geçersiz kılın. Ardından, **Notificationregistrationservice**üzerinde **refreshregistration** öğesini çağırın.

    ```swift
    override func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
      deviceInstallationService?.token = deviceToken
      notificationRegistrationService?.refreshRegistration()
    }
    ```

1. **UserInfo** bağımsız değişkenini **processnotificationactions** işlevine geçirerek **didReceiveRemoteNotification** işlevini geçersiz kılın.

    ```swift
    override func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any]) {
        processNotificationActions(userInfo: userInfo)
    }
    ```

1. Hatayı günlüğe kaydetmek için **Didfailtoregisterforremotenocertificate** işlevini geçersiz kılın.

    ```swift
    override func application(_ application: UIApplication, didFailToRegisterForRemoteNotificationsWithError error: Error) {
        print(error);
    }
    ```

    > [!NOTE]
    > Bu çok fazla yer tutucudur. Üretim senaryolarında doğru günlük kaydı ve hata işlemeyi uygulamak isteyeceksiniz.

1. **Dıdsonlandırhlaunchingwithoptions**Içinde **deviceınstalservıce hizmeti**, **Notificationregistrationservice**ve **notificationactionservice** değişkenlerini oluşturun.

    ```swift
    let controller : FlutterViewController = window?.rootViewController as! FlutterViewController

    deviceInstallationService = DeviceInstallationService(withBinaryMessenger: controller.binaryMessenger)
    notificationRegistrationService = NotificationRegistrationService(withBinaryMessenger: controller.binaryMessenger)
    notificationActionService = NotificationActionService(withBinaryMessenger: controller.binaryMessenger)
    ```

1. Aynı işlevde, koşullu olarak yetkilendirme isteyin ve uzak bildirimlere kaydolun.

    ```swift
    if #available(iOS 13.0, *) {
      UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .sound, .badge]) {
          (granted, error) in

          if (granted)
          {
              DispatchQueue.main.async {
                  let pushSettings = UIUserNotificationSettings(types: [.alert, .sound, .badge], categories: nil)
                  application.registerUserNotificationSettings(pushSettings)
                  application.registerForRemoteNotifications()
              }
          }
      }
    }
    ```

1. **Launchoptions** **remoteNotification** anahtarını Içeriyorsa, **Didsonlandırhlaunchingwithoptions** işlevinin sonundaki **processnotificationactions** çağırın. Elde edilen **UserInfo** nesnesini geçirin ve *launchaction* bağımsız değişkeni için *true* kullanın. *Doğru* bir değer, uygulamanın uygulama başlatma işlemi sırasında işlenmekte olduğunu gösterir.

    ```swift
    if let userInfo = launchOptions?[.remoteNotification] as? [AnyHashable : Any] {
        processNotificationActions(userInfo: userInfo, launchAction: true)
    }
    ```
