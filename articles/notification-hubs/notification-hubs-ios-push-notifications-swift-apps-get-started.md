---
title: Azure Bildirim Hub'larını kullanan Swift iOS uygulamalarına anında iletme bildirimleri gönderme | Microsoft Dokümanlar
description: Azure Bildirim Hub'larını kullanan Swift iOS uygulamalarına nasıl bildirim tebliğ edileceğini öğrenin.
services: notification-hubs
documentationcenter: ios
author: mikeparker104
manager: femila
editor: jwargo
ms.assetid: 4e3772cf-20db-4b9f-bb74-886adfaaa65d
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 05/21/2019
ms.author: miparker
ms.reviewer: jowargo
ms.lastreviewed: 05/21/2019
ms.openlocfilehash: a721c519c7a836e20455c6f1887bcfa7b52951f3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80336631"
---
# <a name="tutorial-send-push-notifications-to-swift-ios-apps-using-notification-hubs-rest-api"></a>Öğretici: Bildirim Hub'ları REST API'yi kullanarak Swift iOS uygulamalarına anında iletme bildirimleri gönderme

> [!div class="op_single_selector"]
> * [Objective-C](notification-hubs-ios-apple-push-notification-apns-get-started.md)
> * [Swift](notification-hubs-ios-push-notifications-swift-apps-get-started.md)

Bu öğreticide, [REST API'sini](/rest/api/notificationhubs/)kullanarak Bildirimleri Swift tabanlı bir iOS uygulamasına iletmek için Azure Bildirim Hub'larını kullanıyorsunuz. Ayrıca, [Apple Anında Bildirim hizmetini (APN)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1)kullanarak anında iletme bildirimleri alan boş bir iOS uygulaması da oluşturursunuz.

Bu öğretici aşağıdaki adımları ile alır:

> [!div class="checklist"]
> * Sertifika imzalama isteği dosyasını oluşturun.
> * Anında iletme bildirimleri için uygulamanızı isteyin.
> * Uygulama için bir sağlama profili oluşturun.
> * Bildirim hub’ı oluşturma.
> * Bildirim merkezini APN bilgileriyle yapılandırın.
> * iOS uygulamanızı bir bildirim merkezine bağlayın.
> * Çözümü test edin.

Bu öğretici için tam kod [GitHub](https://github.com/xamcat/mobcat-samples/tree/master/notification_hub_rest)bulunabilir.

## <a name="prerequisites"></a>Ön koşullar

Takip etmek için şunları yapmanız gerekir:

- Hizmeti bilmiyorsanız [Azure Bildirim Hub'larına genel bakış](notification-hubs-push-notification-overview.md) ı gözden geçirmek için.
- [Kayıtlar ve kurulum](notification-hubs-push-notification-registration-management.md)hakkında bilgi almak için.
- Etkin bir [Apple Geliştirici Hesabı.](https://developer.apple.com)
- Anahtar zincirinize yüklenen geçerli bir geliştirici sertifikasıyla birlikte Xcode çalıştıran bir Mac.
- Anında iletme bildirimlerini simülatörle test edebildiğiniz fiziksel bir iPhone aygıtı yla çalıştırıp hata ayıklayabilirsiniz.
- Apple [Portalı'na](https://developer.apple.com) kayıtlı ve sertifikanızla ilişkili fiziksel iPhone aygıtınız.
- Kaynakları oluşturabileceğiniz ve yönetebileceğiniz bir [Azure aboneliği.](https://portal.azure.com)

iOS geliştirme ile ilgili önceden deneyiminiz olmasa bile, bu ilk ilkeler örneğini oluşturmak için aşağıdaki adımları izleyebilmelisiniz. Ancak, aşağıdaki kavramlara aşinalıktan yararlanacaksınız:

- Xcode ve Swift ile iOS uygulamaları oluşturma.
- iOS için azure [bildirim hub'ı](notification-hubs-ios-apple-push-notification-apns-get-started.md) yapılandırma.
- [Apple Geliştirici Portalı](https://developer.apple.com) ve Azure [portalı.](https://portal.azure.com)

> [!NOTE]
> Bildirim hub'ı yalnızca **Sandbox** kimlik doğrulama modunu kullanacak şekilde yapılandırılır. Üretim iş yükleri için bu kimlik doğrulama modunu kullanmamalısınız.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="connect-your-ios-app-to-a-notification-hub"></a>iOS uygulamanızı bir bildirim merkezine bağlama

Bu bölümde, bildirim merkezine bağlanacak iOS uygulamasını oluşturursunuz.  

### <a name="create-an-ios-project"></a>iOS projesi oluşturma

1. Xcode'da yeni bir iOS projesi oluşturun ve **Single View Application** (Tek Görünüm Uygulaması) şablonunu seçin.

1. Yeni proje için seçenekleri ayarlarken:

   1. **Apple** Geliştirici Portalı'nda Paket Tanımlayıcısını ayarlarken`com.<organization>`kullandığınız Ürün **Adı** (PushDemo) ve Organizasyon **Tanımlayıcısını** () belirtin.

   1. Uygulama **Team** **Kimliği'nin** ayarlandığını seçin.

   1. **Dili** **Swift'e**ayarlayın.

   1. **Sonraki'ni**seçin.

1. **Destekleyen Dosyalar**adlı yeni bir klasör oluşturun.

1. **SupportingFiles** klasöründe **devsettings.plist** adlı yeni bir p-list dosyası oluşturun. Git repo ile çalışırken işlenmeyecek şekilde bu klasörü **gitignore** dosyanıza eklediğinizden emin olun. Bir üretim uygulamasında, bu sırları otomatik bir yapı işleminin parçası olarak koşullu olarak ayarlıyor olabilirsiniz. Bu tür ayarlar bu gözden geçirme alanında yer almıyor.

1. **Devsettings.plist'i,** sizin sağladığınız bildirim merkezinden kendi değerlerinizi kullanarak aşağıdaki yapılandırma girişlerini içerecek şekilde güncelleştirin:

   | Anahtar                            | Tür                     | Değer                     |
   |--------------------------------| -------------------------| --------------------------|
   | bildirimHubKey             | Dize                   | \<hubKey>                  |
   | bildirimHubKeyName         | Dize                   | \<hubKeyName>              |
   | bildirimHubName            | Dize                   | \<hubName>                 |
   | bildirimHubNamespace       | Dize                   | \<hubNamespace>            |

   Azure portalındaki bildirim merkezi kaynağına giderek gerekli değerleri bulabilirsiniz. Özellikle, **bildirimHubName** ve **notificationHubNamespace** değerleri **Genel Bakış** sayfasında **Temel ler** özetinin sağ üst köşesindedir.

   ![Bildirim Hub'ları Essentials özeti](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-essentials.png)

   Ayrıca **Access İlkeleri'ne** göz gezdirerek ve ilgili **Erişim İlkesini**seçerek **bildirimHubKeyName** ve **notificationHubKey** değerlerini `DefaultFullSharedAccessSignature`bulabilirsiniz. Bundan sonra, **Birincil Bağlantı Dizesi'nden** `notificationHubKeyName` kopyala için önceden belirlenmiş değer ve '' `SharedAccessKey=` `notificationHubKey`için önceden belirlenmiş `SharedAccessKeyName=` değer.

   Bağlantı dizesi aşağıdaki biçimde olmalıdır:

   ```xml
   Endpoint=sb://<namespace>.servicebus.windows.net/;SharedAccessKeyName=<notificationHubKeyName>;SharedAccessKey=<notificationHubKey>
   ```

   Basit tutmak için, `DefaultFullSharedAccessSignature` bildirim göndermek için belirteci kullanabilirsiniz şekilde belirtin. Uygulamada, `DefaultListenSharedAccessSignature` yalnızca bildirim almak istediğiniz durumlar için daha iyi bir seçim olacaktır.

1. **Project Navigator** **altında, Proje Adı'nı** seçin ve ardından **Genel** sekmesini seçin.

1. **Kimliği** bulun ve daha sonra Önceki adımdaki Uygulama `com.<organization>.PushDemo` **Kimliği** için kullanılan değer olan Paket **Tanımlayıcı** değerini eşleşebilecek şekilde ayarlayın.

1. **İmzalama & Özelliklerini**bulun ve ardından **Apple Geliştirici Hesabınız**için uygun **Ekibi** seçin. **Takım** değeri, sertifikalarınızı ve profillerinizi oluşturduğunuz değerle eşleşmelidir.

1. Xcode, **Paket Tanımlayıcısını**temel alarak uygun **Tedarik Profili** değerini otomatik olarak aşağı çekmelidir. Yeni **Sağlama Profili** değerini görmüyorsanız, **Xcode** > **Tercihleri** > **Hesabı'nı** seçerek **İmzalama Kimliği** profillerini yenilemeyi deneyin ve ardından profilleri indirmek için Manuel **Profilleri İndir** düğmesini seçin.

1. **İmzalama & Özellikleri** sekmesinde , + **Özellik** düğmesini tıklatın ve **Push Bildirimleri'nin** etkin olduğundan emin olmak için listeden **Bildirimlere Dokun'a** çift dokunun.

1. **UNUserNotificationCenterDelegate** protokolünü uygulamak için **AppDelegate.swift** dosyanızı açın ve sınıfın en üstüne aşağıdaki kodu ekleyin:

    ```swift
    @UIApplicationMain
    class AppDelegate: UIResponder, UIApplicationDelegate, UNUserNotificationCenterDelegate {
        
        ...

        var configValues: NSDictionary?
        var notificationHubNamespace : String?
        var notificationHubName : String?
        var notificationHubKeyName : String?
        var notificationHubKey : String?
        let tags = ["12345"]
        
        ...
    }
    ```

    Bu üyeleri daha sonra kullanacaksın. Özellikle, özel bir **şablon**kullanarak kaydın bir parçası olarak **etiketleri** üye kullanırsınız. Etiketler hakkında daha fazla bilgi için, kayıtlar ve [Şablon kayıtları](notification-hubs-templates-cross-platform-push-messages.md) [için Etiketler'e](notification-hubs-tags-segment-push-message.md) bakın.

1. Aynı dosyada, **didFinishLaunchingWithOptions** işlevine aşağıdaki kodu ekleyin:

    ```swift
    if let path = Bundle.main.path(forResource: "devsettings", ofType: "plist") {
        if let configValues = NSDictionary(contentsOfFile: path) {
            self.notificationHubNamespace = configValues["notificationHubNamespace"] as? String
            self.notificationHubName = configValues["notificationHubName"] as? String
            self.notificationHubKeyName = configValues["notificationHubKeyName"] as? String
            self.notificationHubKey = configValues["notificationHubKey"] as? String
        }
    }

    if #available(iOS 10.0, *){
        UNUserNotificationCenter.current().delegate = self
        UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .sound, .badge]) {
            (granted, error) in

            if (granted)
            {
                DispatchQueue.main.async {
                    application.registerForRemoteNotifications()
                }
            }
        }
    }

    return true
    ```

    Bu **kod, devsettings.plist**ayar değerlerini alır, **UNUserNotificationCenter** temsilcisi olarak **AppDelegate** sınıfAyarlar, anında iletme bildirimleri için yetkilendirme talepleri ve sonra **registerForRemoteNotifications**çağırır.

    Basit tutmak için, kod *iOS 10 ve daha sonra yalnızca*destekler. Normalde yaptığınız gibi, ilgili API'leri ve yaklaşımları koşullu olarak kullanarak önceki işletim sistemi sürümleri için destek ekleyebilirsiniz.

1. Aynı dosyada, aşağıdaki işlevleri ekleyin:

    ```swift
    func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
        let installationId = (UIDevice.current.identifierForVendor?.description)!
        let pushChannel = deviceToken.reduce("", {$0 + String(format: "%02X", $1)})
    }

    func showAlert(withText text : String) {
        let alertController = UIAlertController(title: "PushDemo", message: text, preferredStyle: UIAlertControllerStyle.alert)
        alertController.addAction(UIAlertAction(title: "OK", style: UIAlertActionStyle.default,handler: nil))
        let keyWindow = UIApplication.shared.windows.filter {$0.isKeyWindow}.first
        keyWindow?.rootViewController?.present(alertController, animated: true, completion: nil)
    }
    ```

    Kod, bildirim merkezine kaydolmak için **installationId** ve **pushChannel** değerlerini kullanır. Bu durumda, cihazı tanımlamak için benzersiz bir değer sağlamak için **UIDevice.current.identifierForVendor** kullanıyorsanız ve sonra istenen **pushChannel** değerini sağlamak için **aygıtToken** biçimlendirme. **ShowAlert** işlevi yalnızca gösteri amacıyla bazı ileti metnini görüntülemek için vardır.

1. Hala **AppDelegate.swift** dosyasında, **unUserNotificationCenterDelegate** **için willPresent** ve **didReceive** işlevleri ekleyin. Bu işlevler, bir uygulamanın sırasıyla ön planda veya arka planda çalıştığı bildirilirken bir uyarı görüntüler.

    ```swift
    @available(iOS 10.0, *)
    func userNotificationCenter(_ center: UNUserNotificationCenter, 
        willPresent notification: UNNotification, 
        withCompletionHandler completionHandler: @escaping (UNNotificationPresentationOptions) -> Void) {
        showAlert(withText: notification.request.content.body)
    }

    @available(iOS 10.0, *)
    func userNotificationCenter(_ center: UNUserNotificationCenter, 
        didReceive response: UNNotificationResponse, 
        withCompletionHandler completionHandler: @escaping () -> Void) {
        showAlert(withText: response.notification.request.content.body)
    }
    ```

1. **InstallationId** ve **pushChannel'a** değer atandığını doğrulamak için **didRegisterForRemoteNotificationsWithDeviceToken** işlevinin altına yazdırma ekstreleri ekleyin.

    ```swift
    print(installationId)
    print(pushChannel)
    ```

1. Daha sonra projeye ekleyeceğiniz temel bileşenler için **Modeller,** **Hizmetler**ve **Yardımcı Programlar** klasörleri oluşturun.

1. Projenin fiziksel bir aygıtta oluşturup çalışmadığını denetleyin. Anında iletme bildirimleri simülatörü kullanılarak test edilemez.

### <a name="create-models"></a>Modeller oluşturma

Bu adımda, [Bildirim Hub'ları REST API](/rest/api/notificationhubs/) yüklerini temsil eden ve gerekli paylaşılan erişim imzası (SAS) belirteç verilerini depolamak için bir dizi model oluşturursunuz.

1. **Modeller** klasörüne **PushTemplate.swift** adlı yeni bir Swift dosyası ekleyin. Bu model, **DeviceInstallation** yükünün bir parçası olarak tek bir şablonun **GÖVDESIni** temsil eden bir yapı sağlar.

    ```swift
    import Foundation

    struct PushTemplate : Codable {
        let body : String

        init(withBody body : String) {
            self.body = body
        }
    }
    ```

1. **Models** klasörüne **DeviceInstallation.swift** adlı yeni bir Swift dosyası ekleyin. Bu dosya, **aygıt yüklemesi**oluşturmak veya güncelleştirmek için yükü temsil eden bir yapı tanımlar. Dosyaya aşağıdaki kodu ekleyin:

    ```swift
    import Foundation

    struct DeviceInstallation : Codable {
        let installationId : String
        let pushChannel : String
        let platform : String = "apns"
        var tags : [String]
        var templates : Dictionary<String, PushTemplate>

        init(withInstallationId installationId : String, andPushChannel pushChannel : String) {
            self.installationId = installationId
            self.pushChannel = pushChannel
            self.tags = [String]()
            self.templates = Dictionary<String, PushTemplate>()
        }
    }
    ```

1. **Modeller** klasörüne **TokenData.swift** adında yeni bir Swift dosyası ekleyin. Bu model, son kullanma tarihiyle birlikte bir SAS belirteci depolamak için kullanılacaktır.

    ```swift
    import Foundation

    struct TokenData {

        let token : String
        let expiration : Int

        init(withToken token : String, andTokenExpiration expiration : Int) {
            self.token = token
            self.expiration = expiration
        }
    }
    ```

### <a name="generate-a-sas-token"></a>SAS belirteci oluşturma

Bildirim Hub'ları, Azure Hizmet Veri Servisi ile aynı güvenlik altyapısını kullanır. REST API'yi aramak için, isteğin **Yetkilendirme** üstbilgisinde kullanılabilecek [bir SAS belirteci programlamak için oluşturmanız](/rest/api/eventhub/generate-sas-token) gerekir.  

Ortaya çıkan belirteç aşağıdaki biçimde olacaktır:

```xml
SharedAccessSignature sig=<UrlEncodedSignature>&se=<ExpiryEpoch>&skn=<KeyName>&sr=<UrlEncodedResourceUri>
```

Sürecin kendisi aynı altı temel adımı içerir:  

1. [UNIX Epoch zaman](https://en.wikipedia.org/wiki/Unix_time) biçiminde son kullanma işlemini hesaplamak, yani 1 Ocak 1970'te gece yarısından bu yana geçen saniye sayısı Eşgüdümlü Evrensel Saat.
1. Erişmeye çalıştığınız kaynağı temsil eden **Kaynak Url'sini** yüzde kodlanmış ve küçük olacak şekilde biçimlendirme. Kaynak Url'si `'https://<namespace>.servicebus.windows.net/<hubName>'`formu vardır. **ResourceUrl**
1. **StringToSign**hazırlanması , olarak `'<UrlEncodedResourceUrl>\n<ExpiryEpoch>'`biçimlendirilmiştir .
1. **StringToSign** değerinin HMAC-SHA256 karmasını kullanarak **İmza'yı** hesaplama ve Base64 kodlama. Karma değer, ilgili **Yetkilendirme Kuralı**için **Bağlantı Dizesi'nin** **Anahtar** bölümüyle birlikte kullanılır.
1. Base64 kodlanmış **İmza'yı** yüzde kodlanmış olacak şekilde biçimlendirme.
1. **UrlEncodedSignature**, **ExpiryEpoch**, **KeyName**ve **UrlEncodedResourceUrl** değerlerini kullanarak beklenen biçimde belirteç oluşturma.

Paylaşılan erişim imzasını ve Azure Hizmet Veri Ve Servis Veri Merkezi ve Bildirim Hub'larının bu imzayı nasıl kullandığına daha ayrıntılı bir genel bakış için [Azure Hizmet Veri Servisi belgelerine](../service-bus-messaging/service-bus-sas.md) bakın.

Bu Swift örneğinin amaçları için, imza karma yardımcı olmak için Apple'ın açık kaynak **CommonCrypto** kütüphane kullanmak için gidiyoruz. C kütüphanesi olduğu için Swift'e kutudan erişilemez. Köprü üstbilgisini kullanarak kitaplığı kullanılabilir hale getirebilirsiniz.

Köprü üstbilgisini eklemek ve yapılandırmak için:

1. Xcode'da **Dosya** > **Yeni** > **Dosya** > **Üstbilgi Dosyası'nı**seçin. Üstbilgi dosyasını adlandırın **KöprüHeader.h**.

1. **CommonHMAC.h'yi**almak için dosyayı edin:

    ```swift
    #import <CommonCrypto/CommonHMAC.h>

    #ifndef BridgingHeader_h
    #define BridgingHeader_h


    #endif /* BridgingHeader_h */
    ```

1. Köprü üstbilgisine başvurmak için Hedefin **Yapı Ayarlarını** güncelleştirin:

   1. **PushDemo** projesine dokunun ve **Swift Derleyici** bölümüne gidin.

   1. **Install Objective-C Uyumluluk Üstbilgisi** seçeneğinin **Evet**olarak ayarlandığından emin olun.

   1. Dosya yolunu `'<ProjectName>/BridgingHeader.h'` **Objective-C köprü üstbilgi** seçeneğine girin. Bu, köprüleme üstbilgimize giden dosya yoludur.

   Bu seçenekleri bulamıyorsanız, **Temel** veya Özelleştirilmiş yerine **Tüm** görünümün seçildiğinden emin **olun.**

   **CommonCrypto** kullanarak biraz daha kolay hale getirebilir birçok üçüncü taraf açık kaynak sarma kitaplıkları mevcuttur. Ancak, bu tür kitaplıkların tartışılması bu makalenin kapsamı dışındadır.

1. **Yardımcı Programlar** klasörüne **TokenUtility.swift** adında yeni bir Swift dosyası ekleyin ve aşağıdaki kodu ekleyin:

   ```swift
   import Foundation

   struct TokenUtility {    
        typealias Context = UnsafeMutablePointer<CCHmacContext>

        static func getSasToken(forResourceUrl resourceUrl : String, withKeyName keyName : String, andKey key : String, andExpiryInSeconds expiryInSeconds : Int = 3600) -> TokenData {
            let expiry = (Int(NSDate().timeIntervalSince1970) + expiryInSeconds).description
            let encodedUrl = urlEncodedString(withString: resourceUrl)
            let stringToSign = "\(encodedUrl)\n\(expiry)"
            let hashValue = sha256HMac(withData: stringToSign.data(using: .utf8)!, andKey: key.data(using: .utf8)!)
            let signature = hashValue.base64EncodedString(options: .init(rawValue: 0))
            let encodedSignature = urlEncodedString(withString: signature)
            let sasToken = "SharedAccessSignature sr=\(encodedUrl)&sig=\(encodedSignature)&se=\(expiry)&skn=\(keyName)"
            let tokenData = TokenData(withToken: sasToken, andTokenExpiration: expiryInSeconds)

            return tokenData
        }

        private static func sha256HMac(withData data : Data, andKey key : Data) -> Data {
            let context = Context.allocate(capacity: 1)
            CCHmacInit(context, CCHmacAlgorithm(kCCHmacAlgSHA256), (key as NSData).bytes, size_t((key as NSData).length))
            CCHmacUpdate(context, (data as NSData).bytes, (data as NSData).length)
            var hmac = Array<UInt8>(repeating: 0, count: Int(CC_SHA256_DIGEST_LENGTH))
            CCHmacFinal(context, &hmac)

            let result = NSData(bytes: hmac, length: hmac.count)
            context.deallocate()

            return result as Data
        }

        private static func urlEncodedString(withString stringToConvert : String) -> String {
            var encodedString = ""
            let sourceUtf8 = (stringToConvert as NSString).utf8String
            let length = strlen(sourceUtf8)

            let charArray: [Character] = [ ".", "-", "_", "~", "a", "z", "A", "Z", "0", "9"]
            let asUInt8Array = String(charArray).utf8.map{ Int8($0) }

            for i in 0..<length {
                let currentChar = sourceUtf8![i]

                if (currentChar == asUInt8Array[0] || currentChar == asUInt8Array[1] || currentChar == asUInt8Array[2] || currentChar == asUInt8Array[3] ||
                    (currentChar >= asUInt8Array[4] && currentChar <= asUInt8Array[5]) ||
                    (currentChar >= asUInt8Array[6] && currentChar <= asUInt8Array[7]) ||
                    (currentChar >= asUInt8Array[8] && currentChar <= asUInt8Array[9])) {
                    encodedString += String(format:"%c", currentChar)
                }
                else {
                    encodedString += String(format:"%%%02x", currentChar)
                }
            }

            return encodedString
        }
    }
   ```

   Bu yardımcı program, SAS belirteci oluşturmakiçin sorumlu mantığı kapsüller.

   Daha önce belirtildiği gibi, **getSasToken** işlevi belirteci hazırlamak için gerekli üst düzey adımları yönetir. İşlev daha sonra bu öğreticide yükleme hizmeti tarafından çağrılacaktır.

   Diğer iki fonksiyon **getSasToken** işlevi tarafından çağrılır: ilgili URL dizesini kodlamak için imza ve **urlEncodedString** hesaplamak için **sha256HMac.** **urlEncodedString** işlevi yerleşik **addingPercentEncoding** işlevini kullanarak gerekli çıktıyı elde etmek mümkün olmadığından gereklidir.

   [Azure Depolama iOS SDK,](https://github.com/Azure/azure-storage-ios/blob/master/Lib/Azure%20Storage%20Client%20Library/Azure%20Storage%20Client%20Library/AZSUtil.m) bu işlemlere Objective-C'de nasıl yaklaşılabildiğini mükemmel bir örnektir. Azure Hizmet Veri Yolunda's SAS belirteçleri hakkında daha fazla bilgiyi [Azure Hizmet Veri Yolundan belgelerde](../service-bus-messaging/service-bus-sas.md)bulabilirsiniz.

1. **AppDelegate.swift'** de, **TokenUtility.getSasToken'in** geçerli bir belirteç ürettiğini doğrulamak için *didRegisterForRemoteNotificationsWithDeviceToken* işlevine aşağıdaki kodu ekleyin
    
    ```swift
    let baseAddress = "https://<notificaitonHubNamespace>.servicebus.windows.net/<notifiationHubName>"

    let tokenData = TokenUtility.getSasToken(forResourceUrl: baseAddress,
                                                withKeyName: self.notificationHubKeyName!,
                                                andKey: self.notificationHubKey!)
    
    print(tokenData.token)
    ```

    **BaseAddress** dizesindeki yer tutucu değerlerini kendi

### <a name="verify-the-sas-token"></a>SAS belirteci doğrula

Yükleme hizmetini istemcide uygulamadan önce, seçtiğiniz HTTP yardımcı programını kullanarak uygulamamızın SAS belirteci doğru şekilde oluşturduğundan kontrol edin. Bu öğretici amaçları için, bizim seçim aracı **Postman**olacaktır.

Uygulama tarafından oluşturulan **installationId** ve **belirteç** değerlerini not edin.

**Yüklemeleri** API aramak için aşağıdaki adımları izleyin:

1. **Postacı'da**yeni bir sekme açın.

1. **İstek** get için ayarlayın ve aşağıdaki adresi belirtin:

    ```xml
    https://<namespace>.servicebus.windows.net/<hubName>/installations/<installationId>?api-version=2015-01
    ```

1. İstek üstbilgilerini aşağıdaki gibi yapılandırın:

   | Anahtar           | Değer            |
   | ------------- | ---------------- |
   | İçerik Türü  | uygulama/json |
   | Yetkilendirme | \<sasToken>       |
   | x-ms sürümü  | 2015-01          |

1. **Kaydet** düğmesinin altında sağ üstte görünen **Kod** düğmesini seçin. İstek aşağıdaki örneğe benzer olmalıdır:

    ```html
    GET /<hubName>/installations/<installationId>?api-version=2015-01 HTTP/1.1
    Host: <namespace>.servicebus.windows.net
    Content-Type: application/json
    Authorization: <sasToken>
    x-ms-version: 2015-01
    Cache-Control: no-cache
    Postman-Token: <postmanToken>
    ```

1. **Gönder** düğmesini seçin.

Bu noktada belirtilen **installationId** için kayıt bulunmamaktadır. Doğrulama, "401 Yetkisiz" yanıtı yerine "404 Bulunamadı" yanıtıyla sonuçlanmalıdır. Bu sonuç, SAS belirteci kabul edildiğini doğrulamalıdır.

### <a name="implement-the-installation-service-class"></a>Yükleme hizmeti sınıfını uygulama

Daha sonra [Kurulumları REST API](/rest/api/notificationhubs/create-overwrite-installation)etrafında temel sarıcı uygulayacağız.  

**Hizmetler** klasörü altına **NotificationRegistrationService.swift** adlı yeni bir Swift dosyası ekleyin ve ardından bu dosyaya aşağıdaki kodu ekleyin:

```swift
import Foundation

class NotificationRegistrationService {
    private let tokenizedBaseAddress: String = "https://%@.servicebus.windows.net/%@"
    private let tokenizedCreateOrUpdateInstallationRequest = "/installations/%@?api-version=%@"
    private let session = URLSession(configuration: URLSessionConfiguration.default)
    private let apiVersion = "2015-01"
    private let jsonEncoder = JSONEncoder()
    private let defaultHeaders: [String : String]
    private let installationId : String
    private let pushChannel : String
    private let hubNamespace : String
    private let hubName : String
    private let keyName : String
    private let key : String
    private var tokenData : TokenData? = nil
    private var tokenExpiryDate : Date? = nil
    
    init(withInstallationId installationId : String,
            andPushChannel pushChannel : String,
            andHubNamespace hubNamespace : String,
            andHubName hubName : String,
            andKeyName keyName : String,
            andKey key: String) {
        self.installationId = installationId
        self.pushChannel = pushChannel
        self.hubNamespace = hubNamespace
        self.hubName = hubName
        self.keyName = keyName
        self.key = key
        self.defaultHeaders = ["Content-Type": "application/json", "x-ms-version": apiVersion]
    }
    
    func register(
        withTags tags : [String]? = nil,
        andTemplates templates : Dictionary<String, PushTemplate>? = nil,
        completeWith completion: ((_ result: Bool) -> ())? = nil) {
        
        var deviceInstallation = DeviceInstallation(withInstallationId: installationId, andPushChannel: pushChannel)
        
        if let tags = tags {
            deviceInstallation.tags = tags
        }
        
        if let templates = templates {
            deviceInstallation.templates = templates
        }
        
        if let deviceInstallationJson = encodeToJson(deviceInstallation) {
            let sasToken = getSasToken()
            let requestUrl = String.init(format: tokenizedCreateOrUpdateInstallationRequest, installationId, apiVersion)
            let apiEndpoint = "\(getBaseAddress())\(requestUrl)"
            
            var request = URLRequest(url: URL(string: apiEndpoint)!)
            request.httpMethod = "PUT"
            
            for (key,value) in self.defaultHeaders {
                request.addValue(value, forHTTPHeaderField: key)
            }
            
            request.addValue(sasToken, forHTTPHeaderField: "Authorization")
            request.httpBody = Data(deviceInstallationJson.utf8)
            
            (self.session.dataTask(with: request) { dat, res, err in
                if let completion = completion {
                        completion(err == nil && 
                        (res as! HTTPURLResponse).statusCode == 200)
                }
            }).resume()
        }
    }
    
    private func getBaseAddress() -> String {
        return String.init(format: tokenizedBaseAddress, hubNamespace, hubName)
    }
    
    private func getSasToken() -> String {
        if (tokenData == nil ||
            tokenExpiryDate == nil ||
            Date() >= tokenExpiryDate!) {
            
            self.tokenData = TokenUtility.getSasToken(
                forResourceUrl: getBaseAddress(),
                withKeyName: self.keyName,
                andKey: self.key)
            
            self.tokenExpiryDate = Date(timeIntervalSinceNow: -(5 * 60))
                .addingTimeInterval(TimeInterval(tokenData!.expiration))
        }

        return (tokenData?.token)!
    }
    
    private func encodeToJson<T : Encodable>(_ object: T) -> String? {
        do {
            let jsonData = try jsonEncoder.encode(object)
            if let jsonString = String(data: jsonData, encoding: .utf8) {
                return jsonString
            } else {
                return nil
            }
        }
        catch {
            return nil
        }
    }
}
```

Gerekli ayrıntılar başlatmanın bir parçası olarak sağlanır. Etiketler ve şablonlar isteğe bağlı olarak Aygıt **Yükleme** JSON yükünün bir parçasını oluşturmak için **kayıt** işlevine aktarılır.  

**Kayıt** işlevi isteği hazırlamak için diğer özel işlevleri çağırır. Yanıt alındıktan sonra, tamamlanma çağrılır ve kaydın başarılı olup olmadığını gösterir.  

İstek bitiş noktası **getBaseAddress** işlevi tarafından oluşturulur. Yapı, başlatma sırasında sağlanan bildirim merkezi *parametreleri ad alanını* ve *adını* kullanır.  

**getSasToken** işlevi, şu anda depolanan belirteç geçerli olup olmadığını denetler. Belirteç geçerli değilse, işlev **tokenUtility'i** yeni bir belirteç oluşturması için çağırır ve bir değer döndürmeden önce depolar.

Son olarak, **encodeToJson** istek gövdesinin bir parçası olarak kullanılmak üzere json içine ilgili model nesneleri dönüştürür.

### <a name="invoke-the-notification-hubs-rest-api"></a>Bildirim Hub'larını çağırıN REST API

Son adım, **NotificationHub'ımıza**kaydolmak için **NotificationRegistrationService'i** kullanmak için **AppDelegate'i** güncellemektir.

1. **AppDelegate.swift'i** açın ve **NoficiationRegistrationService** ve genel **PushTemplate'e**bir başvuru depolamak için sınıf düzeyinde değişkenler ekleyin:

    ```swift
    var registrationService : NotificationRegistrationService?
    let genericTemplate = PushTemplate(withBody: "{\"aps\":{\"alert\":\"$(message)\"}}")
    ```

1. Aynı dosyada, gerekli parametrelerle **NotificationRegistrationService'i** başlatmak için **didRegisterForRemoteNotificationsWithDeviceToken** işlevini güncelleyin ve ardından **kayıt** işlevini arayın.

    ```swift
    func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
        let installationId = (UIDevice.current.identifierForVendor?.description)!
        let pushChannel = deviceToken.reduce("", {$0 + String(format: "%02X", $1)})

        // Initialize the Notification Registration Service
        self.registrationService = NotificationRegistrationService(
            withInstallationId: installationId,
            andPushChannel: pushChannel,
            andHubNamespace: notificationHubNamespace!,
            andHubName: notificationHubName!,
            andKeyName: notificationHubKeyName!,
            andKey: notificationHubKey!)

        // Call register, passing in the tags and template parameters
        self.registrationService!.register(withTags: tags, andTemplates: ["genericTemplate" : self.genericTemplate]) { (result) in
            if !result {
                print("Registration issue")
            } else {
                print("Registered")
            }
        }
    }
    ```

    Basit tutmak için, çıkış penceresini **kayıt** işleminin sonucuyla güncelleştirmek için birkaç yazdırma deyimi kullanırsınız.

1. Şimdi uygulamayı fiziksel bir cihazda oluşturun ve çalıştırın. Çıkış penceresinde "Kayıtlı" bakın.

## <a name="test-the-solution"></a>Çözümü test edin

Bu aşamadaki uygulamamız **NotificationHub'a** kayıtlıdır ve anında iletme bildirimleri alabilir. Xcode'da hata ayıklayıcıyı durdurun ve şu anda çalışıyorsa uygulamayı kapatın. Ardından, Aygıt **Yükleme** ayrıntılarının beklendiği gibi olup olmadığını ve uygulamamızın artık anında iletme bildirimleri alabilecek olduğundan kontrol edin.  

### <a name="verify-the-device-installation"></a>Aygıt yüklemesini doğrulama

Artık [SAS belirteci doğrulamak](#verify-the-sas-token)için **Postacı** kullanarak daha önce yaptığı gibi aynı isteği yapabilirsiniz. SAS belirteci süresinin dolmadığını varsayarsak, yanıt artık şablonlar ve etiketler gibi sağladığınız yükleme ayrıntılarını içermelidir.

```json
{
    "installationId": "<installationId>",
    "pushChannel": "<pushChannel>",
    "pushChannelExpired": false,
    "platform": "apns",
    "expirationTime": "9999-12-31T23:59:59.9999999Z",
    "tags": [
        "12345"
    ],
    "templates": {
        "genericTemplate": {
            "body": "{\"aps\":{\"alert\":\"$(message)\"}}",
            "tags": [
                "genericTemplate"
            ]
        }
    }
}
```

Önceki **SAS belirteçlerinizin** süresi dolduysa, yeni bir **SAS belirteci** almak ve **Yetkilendirme** üstbilgisini bu yeni değerle güncelleştirmek için **TokenUtility** sınıfının **24** satırına bir **kesme noktası** ekleyebilirsiniz.

### <a name="send-a-test-notification-azure-portal"></a>Test bildirimi gönderme (Azure portalı)

Bildirimleri artık alabileceğinizi test etmenin en hızlı yolu, Azure portalındaki bildirim merkezine göz atmaktır:

1. Azure portalında, bildirim merkezinizdeki **Genel Bakış** sekmesine göz atın.

1. Portal penceresinin sol üst kısmındaki **Temel Ler** özetinin üzerinde yer alan **Test Gönder'i**seçin:

    ![Bildirim Hub'ları Temel Bilgiler Özet Test Gönder Düğmesi](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-essentials-test-send.png)

1. **Platformlar** listesinden **Özel Şablon'u** seçin.

1. **Tag Expression'ya Gönder**için **12345'i** girin. Yüklememizde bu etiketi daha önce belirtmiştiniz.

1. İsteğe bağlı olarak JSON yükündeki **iletiyi** edin:

    ![Bildirim Hub'ları Test Gönder](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-test-send.png)

1. **Gönder**’i seçin. Portal, bildirimin aygıta başarıyla gönderilip gönderilmediğini belirtmelidir:

    ![Bildirim Hub'ları Test Gönderme Sonuçları](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-test-send-result.png)

    Uygulamanın ön planda çalışmadığını varsayarsak, cihazınızdaki **Bildirim Merkezi'nde** de bir bildirim görmeniz gerekir. Bildirime dokunmak uygulamayı açmalı ve uyarıyı göstermelidir.

    ![Bildirim Alınan Örnek](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/test-send-notification-received.png)

### <a name="send-a-test-notification-mail-carrier"></a>Test bildirimi gönderme (Posta taşıyıcısı)

Test etmek için daha uygun bir yol olabilir **Postman**kullanarak [REST API](/rest/api/notificationhubs/) üzerinden bildirimler gönderebilirsiniz.

1. **Postacı'da**yeni bir sekme açın.

1. İsteği **POST'a**ayarlayın ve aşağıdaki adresi girin:

    ```xml
    https://<namespace>.servicebus.windows.net/<hubName>/messages/?api-version=2015-01
    ```

1. İstek üstbilgilerini aşağıdaki gibi yapılandırın:

   | Anahtar                            | Değer                          |
   | ------------------------------ | ------------------------------ |
   | İçerik Türü                   | uygulama/json;charset=utf-8 |
   | Yetkilendirme                  | \<sasToken>                     |
   | ServiceBusBildirim-Format  | şablon                       |
   | Etiketler                           | "12345"                        |

1. İstek **GÖVDESIni** RAW **- JSON (application.json)** aşağıdaki JSON yüküyle kullanacak şekilde yapılandırın:

    ```json
    {
       "message" : "Hello from Postman!"
    }
    ```

1. Pencerenin sağ üst kısmındaki **Kaydet** düğmesinin altında yer alan **Kod** düğmesini seçin. İstek aşağıdaki örneğe benzer olmalıdır:

    ```html
    POST /<hubName>/messages/?api-version=2015-01 HTTP/1.1
    Host: <namespace>.servicebus.windows.net
    Content-Type: application/json;charset=utf-8.
    ServiceBusNotification-Format: template
    Tags: "12345"
    Authorization: <sasToken>
    Cache-Control: no-cache
    Postman-Token: <postmanToken>

    {
        "message" : "Hello from Postman!"
    }
    ```

1. **Gönder** düğmesini seçin.

**201 Oluşturulan** başarı durum kodunu almalı ve istemci aygıtındaki bildirimi almalısınız...

## <a name="next-steps"></a>Sonraki adımlar
Artık [REST API](/rest/api/notificationhubs/) üzerinden bir bildirim merkezine bağlı temel bir iOS Swift uygulamanız var ve bildirim gönderip alabilirsiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Bildirim Hub'larına genel bakış](notification-hubs-push-notification-overview.md)
- [Bildirim Hub'ları REST API'leri](/rest/api/notificationhubs/)
- [Arka uç işlemleri için Bildirim Hub'ları SDK](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)
- [GitHub'da Bildirim Hub'ları SDK](https://github.com/Azure/azure-notificationhubs)
- [Uygulama arka ucuyla kaydolun](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
- [Kayıt yönetimi](notification-hubs-push-notification-registration-management.md)
- [Etiketlerle çalışma](notification-hubs-tags-segment-push-message.md) 
- [Özel şablonlarla çalışma](notification-hubs-templates-cross-platform-push-messages.md)
- [Paylaşılan erişim imzaları ile Servis Veri Aracı erişim denetimi](../service-bus-messaging/service-bus-sas.md)
- [Programlı olarak SAS belirteçleri oluşturmak](/rest/api/eventhub/generate-sas-token)
- [Apple güvenlik: ortak kripto](https://developer.apple.com/security/)
- [UNIX Dönemi zamanlı](https://en.wikipedia.org/wiki/Unix_time)
- [Hmac](https://en.wikipedia.org/wiki/HMAC)
