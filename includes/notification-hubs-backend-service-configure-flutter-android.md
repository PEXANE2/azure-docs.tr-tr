---
author: mikeparker104
ms.author: miparker
ms.date: 07/07/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 11bd05231aa3b5b336400d2434e63e57807a7e69
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86147849"
---
### <a name="add-the-google-services-json-file"></a>Google Services JSON dosyasını ekleme

1. **Denetim**  +  **Android** klasörüne ve sonra **Android Studio aç** **' ı** seçin. Ardından **Proje** görünümüne geçin (henüz yoksa).

1. [Firebase konsolunda](https://console.firebase.google.com) **pushdemo** projesini ayarlarken daha önce indirdiğiniz dosyada *google-services.js* bulun. Ardından, **uygulamayı uygulama** modülü kök dizinine (**Android**  >  **Android**  >  **uygulaması**) sürükleyin.

### <a name="configure-build-settings-and-permissions"></a>Derleme ayarlarını ve izinleri yapılandırma

1. **Proje** görünümünü **Android**'e geçirin.

1. **AndroidManifest.xml**açın, ardından kapatma etiketinden önce **uygulama** öğesinden sonra **Internet** ve **READ_PHONE_STATE** izinleri ekleyin **</manifest>** .

    ```xml
    <manifest>
        <application>...</application>
        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    </manifest>
    ```

### <a name="add-the-firebase-sdks"></a>Firebase SDK 'larını ekleyin

1. **Android Studio**, proje düzeyi **Build. Gradle** dosyasını açın (**Gradle betikler**  >  **Build. Gradle (proje: Android)**). ve ``buildscript``  >  **Bağımlılıklar** düğümünde ' com. Google. GMS: Google-Services ' Sınıfyoluna sahip olduğunuzdan emin olun.

    ```json
    buildscript {

      repositories {
        // Check that you have the following line (if not, add it):
        google()  // Google's Maven repository
      }

      dependencies {
        // ...

        // Add the following line:
        classpath 'com.google.gms:google-services:4.3.3'  // Google Services plugin
      }
    }

    allprojects {
      // ...

      repositories {
        // Check that you have the following line (if not, add it):
        google()  // Google's Maven repository
        // ...
      }
    }
    ```

    > [!NOTE]
    > **Android projesini**oluştururken [Firebase konsolunda](https://console.firebase.google.com) sunulan yönergelere göre en son sürüme başvurtığınızdan emin olun.

1. Uygulama düzeyi **Build. Gradle** dosyasında (**Gradle Scripts**  >  **Build. Gradle (Module: App)**), **Google Services Gradle eklentisini**uygulayın. Eklentiyi doğrudan **Android** düğümünün üzerine uygulayın.

    ```json
    // ...

    // Add the following line:
    apply plugin: 'com.google.gms.google-services'  // Google Services plugin

    android {
      // ...
    }
    ```

1. Aynı dosyada, **Bağımlılıklar** düğümünde, **bulut mesajlaşma** Android kitaplığı için bağımlılığı ekleyin.

    ```json
    dependencies {
        // ...
        implementation 'com.google.firebase:firebase-messaging:20.2.0'
    }
    ```

    > [!NOTE]
    > [Bulut mesajlaşma Android istemci belgelerine](https://firebase.google.com/docs/cloud-messaging/android/client)göre en son sürüme başvurtığınızdan emin olun.

1. Değişiklikleri kaydedin ve **Şimdi Eşitle** düğmesine (araç çubuğu isteminden) veya **projeyi Gradle dosyalarıyla Eşitle**' ye tıklayın.

### <a name="handle-push-notifications-for-android"></a>Android için anında Iletme bildirimlerini işleme

1. **Android Studio**' de **,**  +  **com. <your_organization>. pushdemo** paket klasörü **' ne tıklayın** (**App**  >  **src**  >  **Main**  >  **Kotlin**), **Yeni** menüden **paket** ' i seçin. Adı olarak **Hizmetleri** girip **Return**tuşuna basın.

1. **Denetim**  +  **Hizmetler** klasörüne tıklayın, **Yeni** menüden **Kotlin dosyası/sınıfı** **' nı** seçin. Ad olarak **Deviceınstalservıce hizmetini** girip **Return**tuşuna basın.

1. Aşağıdaki kodu kullanarak **Deviceınstalservıce hizmetini** uygulayın.

    ```kotlin
    package com.<your_organization>.pushdemo.services

    import android.annotation.SuppressLint
    import android.content.Context
    import android.provider.Settings.Secure
    import com.google.android.gms.common.ConnectionResult
    import com.google.android.gms.common.GoogleApiAvailability
    import io.flutter.embedding.engine.FlutterEngine
    import io.flutter.plugin.common.MethodCall
    import io.flutter.plugin.common.MethodChannel

    @SuppressLint("HardwareIds")
    class DeviceInstallationService {

        companion object {
            const val DEVICE_INSTALLATION_CHANNEL = "com.<your_organization>.pushdemo/deviceinstallation"
            const val GET_DEVICE_ID = "getDeviceId"
            const val GET_DEVICE_TOKEN = "getDeviceToken"
            const val GET_DEVICE_PLATFORM = "getDevicePlatform"
        }

        private var context: Context
        private var deviceInstallationChannel : MethodChannel

        val playServicesAvailable
            get() = GoogleApiAvailability.getInstance().isGooglePlayServicesAvailable(context) == ConnectionResult.SUCCESS

        constructor(context: Context, flutterEngine: FlutterEngine) {
            this.context = context
            deviceInstallationChannel = MethodChannel(flutterEngine.dartExecutor.binaryMessenger, DEVICE_INSTALLATION_CHANNEL)
            deviceInstallationChannel.setMethodCallHandler { call, result -> handleDeviceInstallationCall(call, result) }
        }

        fun getDeviceId() : String
            = Secure.getString(context.applicationContext.contentResolver, Secure.ANDROID_ID)

        fun getDeviceToken() : String {
            if(!playServicesAvailable) {
                throw Exception(getPlayServicesError())
            }

            // TODO: Revisit once we have created the PushNotificationsFirebaseMessagingService
            val token = "Placeholder_Get_Value_From_FirebaseMessagingService_Implementation"

            if (token.isNullOrBlank()) {
                throw Exception("Unable to resolve token for FCM.")
            }

            return token
        }

        fun getDevicePlatform() : String = "fcm"

        private fun handleDeviceInstallationCall(call: MethodCall, result: MethodChannel.Result) {
            when (call.method) {
                GET_DEVICE_ID -> {
                    result.success(getDeviceId())
                }
                GET_DEVICE_TOKEN -> {
                    getDeviceToken(result)
                }
                GET_DEVICE_PLATFORM -> {
                    result.success(getDevicePlatform())
                }
                else -> {
                    result.notImplemented()
                }
            }
        }

        private fun getDeviceToken(result: MethodChannel.Result) {
            try {
                val token = getDeviceToken()
                result.success(token)
            }
            catch (e: Exception) {
                result.error("ERROR", e.message, e)
            }
        }

        private fun getPlayServicesError(): String {
            val resultCode = GoogleApiAvailability.getInstance().isGooglePlayServicesAvailable(context)

            if (resultCode != ConnectionResult.SUCCESS) {
                return if (GoogleApiAvailability.getInstance().isUserResolvableError(resultCode)){
                    GoogleApiAvailability.getInstance().getErrorString(resultCode)
                } else {
                    "This device is not supported"
                }
            }

            return "An error occurred preventing the use of push notifications"
        }
    }
    ```

    > [!NOTE]
    > Bu sınıf kanal için platforma özgü karşılığı uygular `com.<your_organization>.pushdemo/deviceinstallation` . Bu, uygulamanın **Deviceınstalyüklemehizmeti. Dart**içindeki flutter bölümünde tanımlanmıştır. Bu durumda, çağrılar ortak koddan yerel ana bilgisayara yapılır. **<your_organization>** bu kullanıldığı her yerde kendi kuruluşunuzla değiştirdiğinizden emin olun.
    >
    > Bu sınıf, Bildirim Hub 'ı kayıt yükünün bir parçası olarak benzersiz bir KIMLIK ( [Secure. AndroidId](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID)kullanarak) sağlar.

1. *Notificationregistrationservice*adlı **Hizmetler** klasörüne başka bir **Kotlin dosyası/sınıfı** ekleyin, ardından aşağıdaki kodu ekleyin.

    ```kotlin
    package com.<your_organization>.pushdemo.services

    import io.flutter.embedding.engine.FlutterEngine
    import io.flutter.plugin.common.MethodChannel

    class NotificationRegistrationService {

        companion object {
            const val NOTIFICATION_REGISTRATION_CHANNEL = "com.<your_organization>.pushdemo/notificationregistration"
            const val REFRESH_REGISTRATION = "refreshRegistration"
        }

        private var notificationRegistrationChannel : MethodChannel

        constructor(flutterEngine: FlutterEngine) {
            notificationRegistrationChannel = MethodChannel(flutterEngine.dartExecutor.binaryMessenger, NotificationRegistrationService.NOTIFICATION_REGISTRATION_CHANNEL)
        }

        fun refreshRegistration() {
            notificationRegistrationChannel.invokeMethod(REFRESH_REGISTRATION, null)
        }
    }
    ```

    > [!NOTE]
    > Bu sınıf kanal için platforma özgü karşılığı uygular `com.<your_organization>.pushdemo/notificationregistration` . Bu, uygulamanın **Notificationregistrationservice. Dart**içindeki flutter bölümünde tanımlanmıştır. Bu durumda, çağrılar yerel ana bilgisayardan ortak koda yapılır. Bu, kullanıldığı her yerde **<your_organization>** kendi kuruluşunuzla değiştirmek için dikkatli olun.

1. *Notificationactionservice*adlı **Hizmetler** klasörüne başka bir **Kotlin dosyası/sınıfı** ekleyin, ardından aşağıdaki kodu ekleyin.

    ```kotlin
    package com.<your_organization>.pushdemo.services

    import io.flutter.embedding.engine.FlutterEngine
    import io.flutter.plugin.common.MethodCall
    import io.flutter.plugin.common.MethodChannel

    class NotificationActionService {
        companion object {
            const val NOTIFICATION_ACTION_CHANNEL = "com.<your_organization>.pushdemo/notificationaction"
            const val TRIGGER_ACTION = "triggerAction"
            const val GET_LAUNCH_ACTION = "getLaunchAction"
        }

        private var notificationActionChannel : MethodChannel
        var launchAction : String? = null

        constructor(flutterEngine: FlutterEngine) {
            notificationActionChannel = MethodChannel(flutterEngine.dartExecutor.binaryMessenger, NotificationActionService.NOTIFICATION_ACTION_CHANNEL)
            notificationActionChannel.setMethodCallHandler { call, result -> handleNotificationActionCall(call, result) }
        }

        fun triggerAction(action: String) {
            notificationActionChannel.invokeMethod(NotificationActionService.TRIGGER_ACTION, action)
        }

        private fun handleNotificationActionCall(call: MethodCall, result: MethodChannel.Result) {
            when (call.method) {
                NotificationActionService.GET_LAUNCH_ACTION -> {
                    result.success(launchAction)
                }
                else -> {
                    result.notImplemented()
                }
            }
        }
    }
    ```

    > [!NOTE]
    > Bu sınıf kanal için platforma özgü karşılığı uygular `com.<your_organization>.pushdemo/notificationaction` . Bu, uygulamanın **Notificationactionservice. Dart**içindeki flutter bölümünde tanımlanmıştır. Çağrılar, bu durumda her iki yönde de yapılabilir. **<your_organization>** bu kullanıldığı her yerde kendi kuruluşunuzla değiştirdiğinizden emin olun.

1. **Com. <your_organization>. pushdemo** paketine *Pushnotificationsfirebasemessagingservice*adlı yeni bir **Kotlin dosyası/sınıfı** ekleyin ve ardından aşağıdaki kodu kullanarak uygulayın.

    ```kotlin
    package com.<your_organization>.pushdemo

    import android.os.Handler
    import android.os.Looper
    import com.google.firebase.messaging.FirebaseMessagingService
    import com.google.firebase.messaging.RemoteMessage
    import com.<your_organization>.pushdemo.services.NotificationActionService
    import com.<your_organization>.pushdemo.services.NotificationRegistrationService

    class PushNotificationsFirebaseMessagingService : FirebaseMessagingService() {

        companion object {
            var token : String? = null
            var notificationRegistrationService : NotificationRegistrationService? = null
            var notificationActionService : NotificationActionService? = null
        }

        override fun onNewToken(token: String) {
            PushNotificationsFirebaseMessagingService.token = token
            notificationRegistrationService?.refreshRegistration()
        }

        override fun onMessageReceived(message: RemoteMessage) {
            message.data.let {
                Handler(Looper.getMainLooper()).post {
                    notificationActionService?.triggerAction(it.getOrDefault("action", null))
                }
            }
        }
    }
    ```

    > [!NOTE]
    > Bu sınıf, uygulama ön planda çalışırken bildirimlerin işlenmesinden sorumludur. **OnMessageReceived**içinde alınan bildirim yüküne bir eylem varsa, **Notificationactionservice** üzerinde **TriggerAction** 'ı koşullu olarak çağırır. Bu Ayrıca, **Firebase** belirteci **onnewtoken** işlevini geçersiz kılarak yeniden üretildiğinde **Notificationregistrationservice** üzerinde **refreshregistration** öğesini çağırır.
    >
    > Bir kez daha **<your_organization>** , kullanıldığı her yerde kendi kuruluşunuzla değiştirin.

1. **AndroidManifest.xml** (**App**  >  **src**  >  **Main**) içinde, hedef filtresi ile birlikte **uygulama** öğesinin altına **pushnotificationsfirebasemessagingservice** ekleyin `com.google.firebase.MESSAGING_EVENT` .

    ```xml
    <manifest>
        <application>
            <!-- EXISTING MANIFEST CONTENT -->
             <service
                android:name="com.<your_organization>.pushdemo.PushNotificationsFirebaseMessagingService"
                android:exported="false">
                <intent-filter>
                    <action android:name="com.google.firebase.MESSAGING_EVENT" />
                </intent-filter>
            </service>
        </application>
    </manifest>
    ```

1. **Deviceınstalservıce hizmetine**geri döndüğünüzde, dosyanın en üstünde aşağıdaki içeri aktarmalar bulunduğundan emin olun.

    ```kotlin
    package com.<your_organization>.pushdemo
    import com.<your_organization>.pushdemo.services.PushNotificationsFirebaseMessagingService
    ```

    > [!NOTE]
    > **<your_organization>** kendi kuruluşunuzun değeri ile değiştirin.

1. **Pushnotificationfirebasemessagingservice**öğesinden belirteç değerini almak için *Placeholder_Get_Value_From_FirebaseMessagingService_Implementation* yer tutucu metnini güncelleştirin.

    ```kotlin
    fun getDeviceToken() : String {
        if(!playServicesAvailable) {
            throw Exception(getPlayServicesError())
        }

        // Get token from the PushNotificationsFirebaseMessagingService.token field.
        val token = PushNotificationsFirebaseMessagingService.token

        if (token.isNullOrBlank()) {
            throw Exception("Unable to resolve token for FCM.")
        }

        return token
    }
    ```

1. **MainActivity**'de, dosyanın en üstünde aşağıdaki içeri aktarmalar bulunduğundan emin olun.

    ```kotlin
    package com.<your_organization>.pushdemo

    import android.content.Intent
    import android.os.Bundle
    import com.google.android.gms.tasks.OnCompleteListener
    import com.google.firebase.iid.FirebaseInstanceId
    import com.<your_organization>.pushdemo.services.DeviceInstallationService
    import com.<your_organization>.pushdemo.services.NotificationActionService
    import com.<your_organization>.pushdemo.services.NotificationRegistrationService
    import io.flutter.embedding.android.FlutterActivity
    ```

    > [!NOTE]
    > **<your_organization>** kendi kuruluşunuzun değeri ile değiştirin.

1. **Deviceınstalınstalservıce hizmetine**bir başvuruyu depolamak için bir değişken ekleyin.

    ```kotlin
    private lateinit var deviceInstallationService: DeviceInstallationService
    ```

1. Bir **amacın** bir ek değer **eylemi**olup olmadığını denetlemek için **processnotificationactions** adlı bir işlev ekleyin. Bu eylemi koşullu olarak tetikleyin veya eylem uygulama başlatma sırasında işleniyorsa daha sonra kullanmak üzere saklayın.

    ```kotlin
     private fun processNotificationActions(intent: Intent, launchAction: Boolean = false) {
        if (intent.hasExtra("action")) {
            var action = intent.getStringExtra("action");

            if (action.isNotEmpty()) {
                if (launchAction) {
                    PushNotificationsFirebaseMessagingService.notificationActionService?.launchAction = action
                }
                else {
                    PushNotificationsFirebaseMessagingService.notificationActionService?.triggerAction(action)
                }
            }
        }
    }
    ```

1. **Processnotificationactions**çağırmak Için **Onnewınkatlanmış** işlevini geçersiz kılın.

    ```kotlin
    override fun onNewIntent(intent: Intent) {
        super.onNewIntent(intent)
        processNotificationActions(intent)
    }
    ```

    > [!NOTE]
    > **MainActivity** Için **Launchmode** değeri **singletop**olarak ayarlandığından, bir **Amaç** , **OnCreate** Işlevi yerine **onnewıntes** Işlevi aracılığıyla var olan **etkinlik** örneğine gönderilir ve bu nedenle hem **OnCreate** hem de **onnewinkatlanmış** işlevlerinde gelen bir **amacı** işlemeniz gerekir.

1. **OnCreate** işlevini geçersiz kılın, **Deviceınstalservıce hizmetini** **deviceınstalyüklemehizmeti**'nin yeni bir örneğine ayarlayın.

    ```kotlin
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        flutterEngine?.let {
            deviceInstallationService = DeviceInstallationService(context, it)
        }
    }
    ```

1. **Pushnotificationfirebasemessagingservices**üzerinde **notificationactionservice** ve **notificationregistrationservice** özelliklerini ayarlayın.

    ```kotlin
    flutterEngine?.let {
      deviceInstallationService = DeviceInstallationService(context, it)
      PushNotificationsFirebaseMessagingService.notificationActionService = NotificationActionService(it)
      PushNotificationsFirebaseMessagingService.notificationRegistrationService = NotificationRegistrationService(it)
    }
    ```

1. Aynı işlevde, **Firebaseınstanceıd. GetInstance (). InstanceId**koşullu çağrısını yapın. **Refreshregistration**çağrılmadan önce **Pushnotificationfirebasemessagingservice** üzerinde elde edilen **belirteç** değerini ayarlamak için **oncompletelistener** uygulamasını uygulayın.

    ```kotlin
    if(deviceInstallationService?.playServicesAvailable) {
        FirebaseInstanceId.getInstance().instanceId
            .addOnCompleteListener(OnCompleteListener { task ->
                if (!task.isSuccessful)
                    return@OnCompleteListener

                PushNotificationsFirebaseMessagingService.token = task.result?.token
                PushNotificationsFirebaseMessagingService.notificationRegistrationService?.refreshRegistration()
            })
    }
    ```

1. Hala **OnCreate**işlevinde, Işlevin sonundaki **processnotificationactions** çağırın. Uygulamanın başlatılması sırasında bu eylemin işlenmekte olduğunu göstermek için *launchaction* bağımsız değişkeni için *true* kullanın.

    ```kotlin
    processNotificationActions(this.intent, true)
    ```

> [!NOTE]
> Uygulamayı her çalıştırdığınız zaman yeniden kaydetmeniz ve anında iletme bildirimleri almaya devam etmek için bir hata ayıklama oturumundan durdurmanız gerekir.
