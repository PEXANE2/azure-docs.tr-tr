---
author: mikeparker104
ms.author: miparker
ms.date: 07/07/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 2f8cd5e554980225850780cc474106c3dc69288a
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86147850"
---
### <a name="create-the-flutter-solution"></a>Flutter çözümünü oluşturma

1. **Visual Studio Code**yeni bir örneğini açın.

1. **Komut paletini** açın (**SHIFT**  +  **komutu**  +  **P**).

1. **Flutter: New Project** komutunu seçin ve **ENTER**tuşuna basın.

1. **Proje adı** için *push_demo* girin ve ardından bir **Proje konumu**seçin.

1. İstendiğinde, **paketleri al**' ı seçin.

1. **Denetim**  +  **Click** **Kotlin** klasörüne ( **App**  >  **src**  >  **Main**altında) tıklayın ve ardından **Finder 'da açığa çıkar**' ı seçin. Sonra, alt klasörleri ( **Kotlin** klasörü altında) ```com``` , ```<your_organization>``` , ve sırasıyla yeniden adlandırın ```pushdemo``` .

    > [!NOTE]
    > **Visual Studio Code** şablonu kullanırken bu klasörler varsayılan olarak **com**, **örnek** **<Project_Name>**. **Kuruluş**için **mobcat** 'in kullanıldığı varsayıldığında, klasör yapısının şöyle görünmesi gerekir:
    >
    > - Kotlin
    >     - e
    >         - mobcat
    >             - pushdemo

1. **Visual Studio Code**geri döndüğünüzde, **Android** **applicationId**  >  **uygulama**  >  **Build. Gradle** ' deki ApplicationId değerini güncelleştirin `com.<your_organization>.pushdemo` .

    > [!NOTE]
    > *<your_organization>* yer tutucusu için kendi kuruluşunuzun adını kullanmanız gerekir. Örneğin, **mobcat** 'in kuruluş olarak kullanılması, *com. mobcat. pushdemo* **paket adı** değeriyle sonuçlanır.

1. **AndroidManifest.xml** dosyalarında **package** , sırasıyla **src**  >  **hata ayıklama**, **src**  >  **Main**ve **src**  >  **profili** altındaki Package özniteliğini güncelleştirin. Değerlerin, önceki adımda kullandığınız **ApplicationId** ile eşleştiğinden emin olun.

    ```xml
    <manifest
        xmlns:android="http://schemas.android.com/apk/res/android"
        package="com.<your_organization>.pushdemo>">
        ...
    </manifest>
    ```

1. ```android:label``` **AndroidManifest.xml** **Src**  >  **Main** to *pushdemo*altındakiAndroidManifest.xmldosyasındaki özniteliği güncelleştirin. Ardından, ```android:allowBackup``` doğrudan altında özniteliğini ekleyin ```android:label``` , değerini **false**olarak ayarlayarak.

    ```xml
    <application
        android:name="io.flutter.app.FlutterApplication"
        android:label="PushDemo"
        android:allowBackup="false"
        android:icon="@mipmap/ic_launcher">
        ...
    </application>
    ```

1. Uygulama düzeyi **Build. Gradle** dosyasını (**Android**  >  **App**  >  **Build. Gradle**) açın ve ardından *compilesdkversion* 'ı ( **Android** bölümünden) API **29**kullanacak şekilde güncelleştirin. Daha sonra, *minsdkversion* ve *targetsdkversion* değerlerini ( **defaultconfig** bölümünden) sırasıyla **26** ve **29** olarak güncelleştirin.

    > [!NOTE]
    > Bu öğreticinin amaçları doğrultusunda yalnızca **API düzeyi 26 ve üzeri** çalıştıran cihazlar desteklenir, ancak daha eski sürümleri çalıştıran cihazları desteklemek için genişletebilirsiniz.

1. **Denetim**  +  **İOS** klasörüne ve ardından **Xcode 'Da aç** **' ı** seçin.

1. **Xcode**'Da, **Çalıştırıcısı** ' na tıklayın (klasörü değil, en üstteki **XCODEPROJ** ). Ardından, **Çalıştırıcısı** hedefini seçin ve **genel** sekmesini seçin. **Tüm** derleme yapılandırması seçiliyken, **paket tanımlayıcısını** olarak güncelleştirin `com.<your_organization>.PushDemo` .

    > [!NOTE]
    > *<your_organization>* yer tutucusu için kendi kuruluşunuzun adını kullanmanız gerekir. Örneğin, **mobcat** 'in kuruluş olarak kullanılması, *com. mobcat. Pushdemo* **paket tanımlayıcı** değeri oluşmasına neden olur.

1. **Info. plist** ' e tıklayın ve ardından **paket adı** değerini **pushdemo** olarak güncelleştirin

1. **Xcode** 'u kapatın ve **Visual Studio Code**geri dönün.

1. **Visual Studio Code**geri dönüp **pubspec. YAML**dosyasını açın, [http](https://pub.dev/packages/http) ve [flutter_secure_storage](https://pub.dev/packages/flutter_secure_storage) **Dart paketlerini** bağımlılıklar olarak ekleyin. Ardından, dosyayı kaydedin ve istendiğinde **paketleri al** ' a tıklayın.

    ```yaml
    dependencies:
      flutter:
        sdk: flutter

      http: ^0.12.1
      flutter_secure_storage: ^3.3.3
    ```

1. **Terminal**' de, dizini **IOS** klasörüne (flutter projeniz için) değiştirin. Ardından, yeni pod yüklemek için [**Pod install**](https://guides.cocoapods.org/using/getting-started.html#installation) komutunu yürütün ( [flutter_secure_storage](https://pub.dev/packages/flutter_secure_storage) paketi için gereklidir).

1. **Denetim**  +  **LIB** klasörüne tıklayın ve ardından dosya adı olarak *main_page. Dart* kullanarak menüden **yeni dosya** **' yı** seçin. Ardından aşağıdaki kodu ekleyin.

    ```kotlin
    import 'package:flutter/material.dart';

    class MainPage extends StatefulWidget {
      @override
      _MainPageState createState() => _MainPageState();
    }

    class _MainPageState extends State<MainPage> {
      @override
      Widget build(BuildContext context) {
        return Scaffold(
          body: SafeArea(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.stretch,
              children: <Widget>[],
            )
          )
        );
      }
    }
    ```

1. **Main. Dart**içinde şablonlu kodu aşağıdaki kodla değiştirin.

    ```dart
    import 'package:flutter/material.dart';
    import 'package:push_demo/main_page.dart';

    final navigatorKey = GlobalKey<NavigatorState>();

    void main() => runApp(MaterialApp(home: MainPage(), navigatorKey: navigatorKey));
    ```

1. **Terminalde**, kuruluşunuzda şablonlu uygulamayı test etmek için her bir hedef platformda uygulamayı derleyin ve çalıştırın. Desteklenen cihazların bağlı olduğundan emin olun.

    ```bash
    flutter run
    ```

### <a name="implement-the-cross-platform-components"></a>Platformlar arası bileşenleri uygulama

1. **Denetim**  +  **LIB** klasörüne tıklayın ve ardından **klasör adı**olarak *modeller* ' ı kullanarak menüden **Yeni klasör** **' ü** seçin.

1. **Denetim**  +  **Modeller** klasörüne tıklayın ve ardından dosya adı olarak *device_installation. Dart* kullanarak menüden **yeni dosya** **' yı** seçin. Ardından aşağıdaki kodu ekleyin.

    ```dart
    class DeviceInstallation {
        final String deviceId;
        final String platform;
        final String token;
        final List<String> tags;

        DeviceInstallation(this.deviceId, this.platform, this.token, this.tags);

        DeviceInstallation.fromJson(Map<String, dynamic> json)
          : deviceId = json['installationId'],
            platform = json['platform'],
            token = json['pushChannel'],
            tags = json['tags'];

        Map<String, dynamic> toJson() =>
        {
          'installationId': deviceId,
          'platform': platform,
          'pushChannel': token,
          'tags': tags,
        };
    }
    ```

1. Bu örnekte desteklenmekte olan eylemlerin numaralandırılmasını tanımlayan *push_demo_action. Dart* adlı **modeller** klasörüne yeni bir dosya ekleyin.

    ```dart
    enum PushDemoAction {
      actionA,
      actionB,
    }
    ```

1. Yeni bir klasörü **Hizmetler** adlı projeye ekleyin ve ardından *device_installation_service. Dart* adlı klasöre aşağıdaki uygulamayla yeni bir dosya ekleyin.

    ```dart
    import 'package:flutter/services.dart';

    class DeviceInstallationService {
      static const deviceInstallation = const MethodChannel('com.<your_organization>.pushdemo/deviceinstallation');
      static const String getDeviceIdChannelMethod = "getDeviceId";
      static const String getDeviceTokenChannelMethod = "getDeviceToken";
      static const String getDevicePlatformChannelMethod = "getDevicePlatform";

      Future<String> getDeviceId() {
        return deviceInstallation.invokeMethod(getDeviceIdChannelMethod);
      }

      Future<String> getDeviceToken() {
        return deviceInstallation.invokeMethod(getDeviceTokenChannelMethod);
      }

      Future<String> getDevicePlatform() {
        return deviceInstallation.invokeMethod(getDevicePlatformChannelMethod);
      }
    }
    ```

    > [!NOTE]
    > *<your_organization>* yer tutucusu için kendi kuruluşunuzun adını kullanmanız gerekir. Örneğin, **mobcat** 'in kuruluş olarak kullanılması, *com. mobcat. pushdemo/Deviceınstallation* [methodchannel](https://api.flutter.dev/flutter/services/MethodChannel-class.html) adına neden olur.
    >
    > Bu sınıf, önkoşul cihaz yükleme ayrıntılarını almak için temel alınan yerel platformla çalışmayı kapsüller. [Methodchannel](https://api.flutter.dev/flutter/services/MethodChannel-class.html) , temeldeki yerel platformlarla çift yönlü zaman uyumsuz iletişimi kolaylaştırır. Bu kanal için platforma özgü karşılık gelen, sonraki adımlarda de oluşturulacaktır.

1. Aşağıdaki uygulamayla *notification_action_service. Dart* adlı klasöre başka bir dosya ekleyin.

    ```dart
    import 'package:flutter/services.dart';
    import 'dart:async';
    import 'package:push_demo/models/push_demo_action.dart';

    class NotificationActionService {
      static const notificationAction =
          const MethodChannel('com.<your_organization>.pushdemo/notificationaction');
      static const String triggerActionChannelMethod = "triggerAction";
      static const String getLaunchActionChannelMethod = "getLaunchAction";

      final actionMappings = {
        'action_a' : PushDemoAction.actionA,
        'action_b' : PushDemoAction.actionB
      };

      final actionTriggeredController = StreamController.broadcast();

      NotificationActionService() {
        notificationAction
            .setMethodCallHandler(handleNotificationActionCall);
      }

      Stream get actionTriggered => actionTriggeredController.stream;

      Future<void> triggerAction({action: String}) async {

        if (!actionMappings.containsKey(action)) {
          return;
        }

        actionTriggeredController.add(actionMappings[action]);
      }

      Future<void> checkLaunchAction() async {
        final launchAction = await notificationAction.invokeMethod(getLaunchActionChannelMethod) as String;

        if (launchAction != null) {
          triggerAction(action: launchAction);
        }
      }

      Future<void> handleNotificationActionCall(MethodCall call) async {
        switch (call.method) {
          case triggerActionChannelMethod:
            return triggerAction(action: call.arguments as String);
          default:
            throw MissingPluginException();
            break;
        }
      }
    }
    ```

    > [!NOTE]
    > Bu, kesin belirlenmiş bir numaralandırma kullanılarak platformlar arası bir şekilde işlenebilmeleri için bildirim eylemlerinin işlenmesini merkezileştirmek üzere basit bir mekanizma olarak kullanılır. Hizmet, bildirim yükünde belirtildiğinde, temel alınan yerel platformun bir eylemi tetiklemesine olanak sağlar. Ayrıca, ortak kodun, uygulama başlatma sırasında bir eylemin, flutter işlemeye hazırladıktan sonra belirtilip belirlenmediğini geriye dönük olarak denetlemesini sağlar. Örneğin, uygulama, bildirim merkezinden gelen bir Bildirime dokunarak başlatıldığında başlatılır.

1. Aşağıdaki uygulamayla *notification_registration_service. Dart* adlı **Hizmetler** klasörüne yeni bir dosya ekleyin.

    ```dart
    import 'dart:convert';
    import 'package:flutter/services.dart';
    import 'package:http/http.dart' as http;
    import 'package:push_demo/services/device_installation_service.dart';
    import 'package:push_demo/models/device_installation.dart';
    import 'package:flutter_secure_storage/flutter_secure_storage.dart';

    class NotificationRegistrationService {
      static const notificationRegistration =
          const MethodChannel('com.<your_organization>.pushdemo/notificationregistration');

      static const String refreshRegistrationChannelMethod = "refreshRegistration";
      static const String installationsEndpoint = "api/notifications/installations";
      static const String cachedDeviceTokenKey = "cached_device_token";
      static const String cachedTagsKey = "cached_tags";

      final deviceInstallationService = DeviceInstallationService();
      final secureStorage = FlutterSecureStorage();

      String baseApiUrl;
      String apikey;

      NotificationRegistrationService(this.baseApiUrl, this.apikey) {
        notificationRegistration
            .setMethodCallHandler(handleNotificationRegistrationCall);
      }

      String get installationsUrl => "$baseApiUrl$installationsEndpoint";

      Future<void> deregisterDevice() async {
        final cachedToken = await secureStorage.read(key: cachedDeviceTokenKey);
        final serializedTags = await secureStorage.read(key: cachedTagsKey);

        if (cachedToken == null || serializedTags == null) {
          return;
        }

        var deviceId = await deviceInstallationService.getDeviceId();

        if (deviceId.isEmpty) {
          throw "Unable to resolve an ID for the device.";
        }

        var response = await http
            .delete("$installationsUrl/$deviceId", headers: {"apikey": apikey});

        if (response.statusCode != 200) {
          throw "Deregister request failed: ${response.reasonPhrase}";
        }

        await secureStorage.delete(key: cachedDeviceTokenKey);
        await secureStorage.delete(key: cachedTagsKey);
      }

      Future<void> registerDevice(List<String> tags) async {
        try {
          final deviceId = await deviceInstallationService.getDeviceId();
          final platform = await deviceInstallationService.getDevicePlatform();
          final token = await deviceInstallationService.getDeviceToken();

          final deviceInstallation =
              DeviceInstallation(deviceId, platform, token, tags);

          final response = await http.put(installationsUrl,
              body: jsonEncode(deviceInstallation),
              headers: {"apikey": apikey, "Content-Type": "application/json"});

          if (response.statusCode != 200) {
            throw "Register request failed: ${response.reasonPhrase}";
          }

          final serializedTags = jsonEncode(tags);

          await secureStorage.write(key: cachedDeviceTokenKey, value: token);
          await secureStorage.write(key: cachedTagsKey, value: serializedTags);
        } on PlatformException catch (e) {
          throw e.message;
        } catch (e) {
          throw "Unable to register device: $e";
        }
      }

      Future<void> refreshRegistration() async {
        final currentToken = await deviceInstallationService.getDeviceToken();
        final cachedToken = await secureStorage.read(key: cachedDeviceTokenKey);
        final serializedTags = await secureStorage.read(key: cachedTagsKey);

        if (currentToken == null ||
            cachedToken == null ||
            serializedTags == null ||
            currentToken == cachedToken) {
          return;
        }

        final tags = jsonDecode(serializedTags);

        return registerDevice(tags);
      }

      Future<void> handleNotificationRegistrationCall(MethodCall call) async {
        switch (call.method) {
          case refreshRegistrationChannelMethod:
            return refreshRegistration();
          default:
            throw MissingPluginException();
            break;
        }
      }
    }
    ```

    > [!NOTE]
    > Bu sınıf, gerekli kayıt, kaydı kaldırma ve yenileme kayıt eylemlerini gerçekleştirmek için **Deviceınstalservıce hizmetinin** ve arka uç hizmetine yapılan isteklerin kullanımını kapsüller. **Apikey** bağımsız DEĞIŞKENI yalnızca [API anahtarı kullanarak kimlik doğrulama istemcilerinin](#authenticate-clients-using-an-api-key-optional) tamamlanmasını tercih ediyorsanız gereklidir.

1. *Config. Dart* adlı **LIB** klasörüne aşağıdaki uygulamayla yeni bir dosya ekleyin.

    ```dart
    class Config {
      static String apiKey = "API_KEY";
      static String backendServiceEndpoint = "BACKEND_SERVICE_ENDPOINT";
    }
    ```

    > [!NOTE]
    > Bu, uygulama gizli dizileri tanımlamanın basit bir yolu olarak kullanılır. Yer tutucu değerlerini kendi değerlerinizle değiştirin. Arka uç hizmetini oluştururken bunları bir yere döndürmelisiniz. **API uygulaması URL 'si** olmalıdır `https://<api_app_name>.azurewebsites.net/` . **Apikey** ÜYESI yalnızca [API anahtarı kullanarak kimlik doğrulama istemcilerinin](#authenticate-clients-using-an-api-key-optional) tamamlanmasını tercih ediyorsanız gereklidir.
    >
    > Bu gizli dizileri kaynak denetimine kaydetmeden kaçınmak için bunu gitignore dosyanıza eklediğinizden emin olun.

### <a name="implement-the-cross-platform-ui"></a>Platformlar arası kullanıcı arabirimini uygulama

1. **Main_page. Dart**içinde, **Yapı** işlevini aşağıdaki ile değiştirin.

    ```dart
    @override
    Widget build(BuildContext context) {
    return Scaffold(
        body: Padding(
          padding: const EdgeInsets.symmetric(horizontal: 20.0, vertical: 40.0),
          child: Column(
            mainAxisAlignment: MainAxisAlignment.end,
            crossAxisAlignment: CrossAxisAlignment.stretch,
            children: <Widget>[
              FlatButton(
                child: Text("Register"),
                onPressed: registerButtonClicked,
              ),
              FlatButton(
                child: Text("Deregister"),
                onPressed: deregisterButtonClicked,
              ),
            ],
          ),
        ),
      );
    }
    ```

1. Önkoşul içeri aktarmaları **main_page. Dart** dosyasının en üstüne ekleyin.

    ```dart
    import 'package:push_demo/services/notification_registration_service.dart';
    import 'config.dart';
    ```

1. **Notificationregistrationservice**'e bir başvuru depolamak için **_MainPageState** sınıfına bir alan ekleyin.

    ```dart
    final notificationRegistrationService = NotificationRegistrationService(Config.backendServiceEndpoint, Config.apiKey);
    ```

1. **_MainPageState** sınıfında, **Kaydet ve kayıt** kaldırma düğmelerinin **basıldığı** olaylar için olay **Deregister** işleyicilerini uygulayın. Karşılık gelen **kayıt** / **kaydını** kaldırma yöntemlerini çağırın ve sonucu göstermek için bir uyarı gösterir.

    ```dart
    void registerButtonClicked() async {
        try {
          await notificationRegistrationService.registerDevice(List<String>());
          await showAlert(message: "Device registered");
        }
        catch (e) {
          await showAlert(message: e);
        }
      }

      void deregisterButtonClicked() async {
        try {
          await notificationRegistrationService.deregisterDevice();
          await showAlert(message: "Device deregistered");
        }
        catch (e) {
          await showAlert(message: e);
        }
      }

      Future<void> showAlert({ message: String }) async {
        return showDialog<void>(
          context: context,
          barrierDismissible: false,
          builder: (BuildContext context) {
            return AlertDialog(
              title: Text('PushDemo'),
              content: SingleChildScrollView(
                child: ListBody(
                  children: <Widget>[
                    Text(message),
                  ],
                ),
              ),
              actions: <Widget>[
                FlatButton(
                  child: Text('OK'),
                  onPressed: () {
                    Navigator.of(context).pop();
                  },
                ),
              ],
            );
          },
        );
      }
    ```

1. Şimdi **Main. Dart**içinde, dosyanın en üstünde aşağıdaki içeri aktarmalar bulunduğundan emin olun.

    ```dart
    import 'package:flutter/material.dart';
    import 'package:push_demo/models/push_demo_action.dart';
    import 'package:push_demo/services/notification_action_service.dart';
    import 'package:push_demo/main_page.dart';
    ```

1. Bir **Notificationactionservice** örneğine başvuru depolamak ve onu başlatmak için bir değişken bildirin.

    ```dart
    final notificationActionService = NotificationActionService();
    ```

1. Bir eylem tetiklendiğinde uyarının görüntülenmesini işleyecek işlevler ekleyin.

    ```dart
    void notificationActionTriggered(PushDemoAction action) {
      showActionAlert(message: "${action.toString().split(".")[1]} action received");
    }

    Future<void> showActionAlert({ message: String }) async {
      return showDialog<void>(
        context: navigatorKey.currentState.overlay.context,
        barrierDismissible: false,
        builder: (BuildContext context) {
          return AlertDialog(
            title: Text('PushDemo'),
            content: SingleChildScrollView(
              child: ListBody(
                children: <Widget>[
                  Text(message),
                ],
              ),
            ),
            actions: <Widget>[
              FlatButton(
                child: Text('OK'),
                onPressed: () {
                  Navigator.of(context).pop();
                },
              ),
            ],
          );
        },
      );
    }
    ```

1. **Notificationactionservice** **actiontetiklenmiş** akışını gözlemleyecek **ana** işlevi güncelleştirin ve uygulama başlatma sırasında yakalanan tüm eylemleri denetleyin.

    ```dart
    void main() async {
      runApp(MaterialApp(home: MainPage(), navigatorKey: navigatorKey,));
      notificationActionService.actionTriggered.listen((event) { notificationActionTriggered(event as PushDemoAction); });
      await notificationActionService.checkLaunchAction();
    }
    ```

    > [!NOTE]
    > Bu, anında iletme bildirimi eylemlerinin alındığını ve yayılmasını göstermek için yeterlidir. Genellikle, bu durum, belirli bir görünüme gidilirken veya bir uyarı görüntülemek yerine bazı verileri yenileirken sessizce işlenir.
