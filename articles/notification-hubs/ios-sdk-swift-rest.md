---
title: Azure Notification Hubs ve REST API 'Leri kullanarak Swift iOS uygulamalarına anında iletme bildirimleri gönderme
description: Bu öğreticide, iOS cihazlarına anında iletme bildirimleri göndermek için Azure Notification Hubs ve REST API 'Lerini nasıl kullanacağınızı öğreneceksiniz.
author: sethmanheim
ms.author: sethm
ms.date: 06/19/2020
ms.topic: tutorial
ms.service: notification-hubs
ms.reviewer: thsomasu
ms.lastreviewed: 06/03/2020
ms.openlocfilehash: 861011874c63f4c5d18ce33b107bbe9a80359045
ms.sourcegitcommit: 398fecceba133d90aa8f6f1f2af58899f613d1e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2020
ms.locfileid: "85126532"
---
# <a name="tutorial-send-push-notifications-to-swift-ios-apps-using-notification-hubs-rest-apis"></a>Öğretici: Notification Hubs REST API 'Leri kullanarak Swift iOS uygulamalarına anında iletme bildirimleri gönderme

Bu öğreticide, REST API 'Lerini kullanarak bir iOS uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs 'nin nasıl kullanılacağı açıklanmaktadır.

Bu öğreticide aşağıdaki adımlar yer almaktadır:

- Örnek bir iOS uygulaması oluşturun.
- İOS uygulamanızı Azure Notification Hubs bağlayın.
- Test anında iletme bildirimleri gönderin.
- Uygulamanızın bildirimleri aldığını doğrulayın.

Bu öğreticinin tüm kodu [GitHub](https://github.com/Azure/azure-notificationhubs-ios/tree/v3-preview2/Samples)' dan indirilebilir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

- Anahtarınıza yüklenmiş geçerli bir geliştirici sertifikasıyla birlikte [Xcode](https://go.microsoft.com/fwLink/p/?LinkID=266532)çalıştıran bir Mac.

- İOS sürüm 10 veya üstünü çalıştıran bir iPhone veya iPad.

- Fiziksel cihazınız [Apple portalında](https://developer.apple.com/)kayıtlı   ve sertifikanız ile ilişkilendirilmiştir.

Devam etmeden önce, Bildirim Hub 'ınızdaki anında iletme kimlik bilgilerini ayarlamak ve yapılandırmak üzere [iOS uygulamaları Için Azure Notification Hubs](https://go.microsoft.com/fwlink/?linkid=2129801) kullanmaya başlama hakkında bir önceki öğreticiye gitdiğinizden emin olun. İOS geliştirmeyle ilgili önceki bir deneyim olmasa bile, bu adımları takip edebilmelisiniz.

> [!NOTE]
> Anında iletme bildirimlerinin yapılandırma gereksinimleri nedeniyle, iOS öykünücüsü yerine bir fiziksel iOS cihazında (iPhone veya iPad) anında iletme bildirimleri dağıtmanız ve test etmeniz gerekir.

Aşağıdaki bölümlerde, Bildirim Hub 'ına bağlanan bir iOS uygulaması oluşturacaksınız.

## <a name="create-an-ios-project"></a>İOS projesi oluşturma

1. Xcode 'da yeni bir iOS projesi oluşturun ve **tek görünüm uygulama**   şablonunu seçin.

2. Yeni proje için seçenekleri ayarlarken:
   -  **Product Name**    **Organization Identifier**   `com.<organization>` Apple Developer Portal 'da **paket tanımlayıcısını**ayarladığınızda kullandığınız ürün adını (pushdemo) ve kuruluş tanımlayıcısını () belirtin   .
   -  **Team**    **Uygulama kimliğinin**   ayarlandığı ekibi seçin.
   -  **Dili**    **Swift**olarak ayarlayın.
   -  **İleri**'yi seçin.

3.  **Supportingfiles**adlı yeni bir klasör oluşturun.

4. Supportingfiles klasöründe **devsettings. plist**adlı yeni bir p-List dosyası oluşturun    **SupportingFiles**   . Bu klasörü **gitignore**dosyanıza eklediğinizden emin olun   , böylece bir git deposu ile çalışırken uygulanmıyor. Bir üretim uygulamasında, otomatik derleme sürecinin bir parçası olarak bu gizli dizileri koşullu olarak ayarlayabilirsiniz. Bu ayarlar, bu öğreticide ele alınmıştır.

5.  **Devsettings. plist**   öğesini, sağladığınız Bildirim Hub 'ından kendi değerlerinizi kullanarak aşağıdaki yapılandırma girdilerini içerecek şekilde güncelleştirin:

   | **Anahtar**                  | **Tür** | **Değer**        |
   | ------------------------ | -------- | ---------------- |
   | notificationHubKey       | Dize   | `<hubKey>`       |
   | notificationHubKeyName   | Dize   | `<hubKeyName>`   |
   | notificationHubName      | Dize   | `<hubName>`      |
   | notificationHubNamespace | Dize   | `<hubNamespace>` |

   Gerekli değerleri, Azure portal Notification Hub kaynağına giderek bulabilirsiniz. Özellikle, **Notificationhubname**   ve **Notificationhubnamespace**   değerleri **Essentials**    **genel bakış**sayfasındaki temel bileşenler özetinin sağ üst köşesinde bulunur   .

   :::image type="content" source="media/ios-sdk-swift-rest/image1.png" alt-text="Gerekli değerler":::

   Ayrıca, **notificationHubKeyName**   erişim Ilkelerine giderek ve **notificationHubKey**    **Access Policies**    **Defaultfullsharedaccesssignature**gibi ilgili **erişim ilkesini**seçerek notificationhubkeyname ve notificationhubkey değerlerini de bulabilirsiniz. Bundan sonra, **Primary Connection String**   notificationhubkeyname için önekli değeri olan birincil bağlantı dizesinden  `SharedAccessKeyName=`   ve **notificationHubKeyName**  `SharedAccessKey=`    **notificationhubkey**için önekli değere kopyalayın. Bağlantı dizesi aşağıdaki biçimde olmalıdır:

   ```xml
   Endpoint=sb://<namespace>.servicebus.windows.net/;SharedAccessKeyName=<notificationHubKeyName>;SharedAccessKey=<notificationHubKey>
   ```

   Basitlik için, bildirim göndermek üzere belirteci kullanabilmeniz için, **Defaultfullsharedaccesssignature**' ı belirtin. Uygulamada, **Defaultlistensharedaccesssignature**   yalnızca bildirim almak istediğiniz durumlara yönelik daha iyi bir seçimdir.

6.  **Proje Gezgini**altında **proje adını**seçin   ve ardından **genel**   sekmesini seçin.

7.  **Kimlik**bulun   ve ardından **paket tanımlayıcı**   DEĞERINI  `com.<organization>.PushDemo` , bir önceki adımdan **uygulama kimliği**için kullanılan değer olan, eşleşecek şekilde ayarlayın   .

8.  **İmzalama & yeteneklerini**bulun ve ardından **Team**    **Apple geliştirici hesabınız**için uygun ekibi seçin.  **Takım**   değeri, sertifikalarınızı ve profillerinizi oluşturduğunuz ile eşleşmelidir.

9. Xcode, **Provisioning Profile**    **paket tanımlayıcısına**göre uygun sağlama profili değerini otomatik olarak indirmelidir. Yeni **sağlama profili**   değerini görmüyorsanız, **oturum açma kimliği Için**    **Xcode**, **Tercihler**, **Hesap**öğesini seçip profilleri yenilemek için profilleri yeniden **Yükle**   düğmesini seçin.

10. Hala **imzalama & özellikleri**   sekmesinde **+ yetenek**   düğmesine tıklayın ve anında iletme **Push Notifications**    **bildirimlerinin**etkinleştirildiğinden emin olmak için listeden anında iletme bildirimleri ' ne çift dokunun   .

11.  **AppDelegate.swift**    **Unusernotificationcenterdelegate**protokolünü uygulamak için appdelegate. Swift dosyanızı açın   ve sınıfının en üstüne aşağıdaki kodu ekleyin:

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

    Bu üyeleri daha sonra kullanacaksınız. Özellikle, **tags**    **özel bir şablon**kullanarak, kaydın bir parçası olarak Etiketler üyesini kullanacaksınız. Etiketler hakkında daha fazla bilgi için bkz [Tags for registrations](notification-hubs-tags-segment-push-message.md)   . kayıtlar ve [şablon kayıtları](notification-hubs-templates-cross-platform-push-messages.md)için Etiketler.

12. Aynı dosyada, **Didsonlandırhlaunchingwithoptions** işlevine aşağıdaki kodu ekleyin:

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

    Bu kod, **devsettings. plist**dosyasından ayarları alır, **Appdelegate**   sınıfını **unusernotificationcenter**   temsilcisi olarak ayarlar, anında iletme bildirimleri için yetkilendirme ister ve ardından **registerForRemoteNotifications**çağırır.

    Kolaylık olması için, kod yalnızca iOS 10 ve üstünü destekler. Normalde yaptığınız gibi, ilgili API 'Leri ve yaklaşımları koşullu olarak kullanarak önceki iOS sürümleri için destek ekleyebilirsiniz.

13. Aynı dosyaya aşağıdaki kodu ekleyin:

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

    Bu kod, Bildirim Hub 'ına kaydolmak için **ınstald ID**   ve **pushchannel**   değerlerini kullanır. Bu durumda, **UIDevice.current.identifierForVendor**   cihazı tanımlayacak ve sonra **deviceToken**   Istenen **pushchannel**değerini sağlamak üzere Devicetoken 'ı biçimlendirirken benzersiz bir değer sağlamak Için UIDevice. Current. ıdentifierforvendor kullanıyorsunuz   .  **Showalert**   işlevi, Gösterim amacıyla bazı ileti metinlerini göstermek için yeterlidir.

14.  **Appdelegate. Swift**   dosyasında hala **willPresent**    **didReceive**    **Unusernotificationcenterdelegate**öğesine willvar ve didreceive işlevlerini ekleyin. Bu işlevler, bir uygulamanın ön planda veya arka planda çalıştığı bildirildiklerinde bir uyarı görüntüler.

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

15. `print` **Instaldregisterforremotenocertificate Ationswithdevicetoken**işlevinin alt kısmına deyimler ekleyerek    **yüklemekimliği**   ve **pushchannel**   değerlerinin atandığını doğrulayın:

    ```swift
    print(installationId)
    print(pushChannel)
    ```

16. Projeye daha sonra ekleyeceğiniz temel bileşenler için **modeller**, **Hizmetler**ve **yardımcı programlar**   klasörü oluşturun.

17. Projenin fiziksel bir cihazda oluşturulup çalıştırmalarından emin olun. Anında iletme bildirimleri simülatörü kullanılarak test edilemez.

## <a name="create-models"></a>Model oluşturma

Bu adımda, [Notification Hubs REST API](/rest/api/notificationhubs/)   yüklerini temsil etmek ve gerekli paylaşılan ERIŞIM imzası (SAS) belirteç verilerini depolamak için bir modeller kümesi oluşturursunuz.

1. Modeller klasörüne **Pushtemplate. Swift**adlı yeni bir Swift dosyası ekleyin    **Models**   . Bu model, **BODY**    **deviceınstallation**yükünün parçası olarak tek bir şablonun gövdesini temsil eden bir yapıyı tanımlar   .

    ```swift
    import Foundation

    struct PushTemplate : Codable {
        let body : String

        init(withBody body : String) {
            self.body = body
        }
    }
    ```

2. Modeller klasörüne **Deviceınstallation. Swift**adlı yeni bir Swift dosyası ekleyin    **Models**   . Bu dosya, bir **cihaz yüklemesi**oluşturma veya güncelleştirme yükünü temsil eden bir yapıyı tanımlar. Dosyaya aşağıdaki kodu ekleyin:

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

3. Modeller klasörüne **Tokendata. Swift**adlı yeni bir Swift dosyası ekleyin    **Models**   . Bu model, bir SAS belirtecini süre sonu ile birlikte depolamak için kullanılır. Dosyaya aşağıdaki kodu ekleyin:

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

## <a name="generate-a-sas-token"></a>SAS belirteci oluşturma

Notification Hubs, Azure Service Bus aynı güvenlik altyapısını kullanır. REST API çağırmak için, [Program aracılığıyla](/rest/api/eventhub/generate-sas-token)   isteğin **YETKILENDIRME**üstbilgisinde kullanılabilecek bir SAS belirteci oluşturun   .

Elde edilen belirteç aşağıdaki biçimde olacaktır:

```xml
SharedAccessSignature sig=\<UrlEncodedSignature\>\&se=\<ExpiryEpoch\>\&skn=\<KeyName\>\&sr=\<UrlEncodedResourceUri\> |
```

İşlemin kendisi aynı altı adımdan oluşur:

1. Süre sonunu [UNIX dönem saat](https://en.wikipedia.org/wiki/Unix_time)   biçiminde hesaplayın, bu da gece yarısı Evrensel Eşgüdümlü saat, 1 Ocak 1970 ' den beri geçen saniye sayısı anlamına gelir.

2. Erişmeye çalıştığınız kaynağı temsil eden **Resourceurl 'yi**biçimlendirin,   Bu nedenle yüzde kodlamalı ve küçük harfle yazılmalıdır.  **Resourceurl**'nin   biçimi vardır `https://<namespace>.servicebus.windows.net/<hubName>` .

3. Olarak biçimlendirilen **Stringtosign**öğesini hazırlayın `<UrlEncodedResourceUrl>\n<ExpiryEpoch>` .

4.  **Signature**    **STRINGTOSIGN**değerinin HMAC-SHA256 karmasını kullanarak işlem ve Base64 kodlaması   . Karma değeri, **Key**   ilgili yetkilendirme kuralı Için **bağlantı dizesinin**anahtar bölümüyle birlikte kullanılır   . **Authorization Rule**

5. Base64 kodlamalı **imzayı**   yüzde kodlamalı olacak şekilde biçimlendirin.

6.  **Urlencodedsignature**, **expiryepoch**, **KeyName**ve **urlencodedresourceurl**değerlerini kullanarak belirteci beklenen biçimde oluşturun   .

 [Azure Service Bus documentation](../service-bus-messaging/service-bus-sas.md)   Paylaşılan erişim imzaları hakkında daha ayrıntılı bir genel bakış için Azure Service Bus belgelerine ve Azure Service Bus ve Notification Hubs nasıl kullandığını görün.

Bu Swift örneği amaçları doğrultusunda, imza karmasından yararlanmak için Apple açık kaynaklı **Commonşifre**Kitaplığı ' nı kullanırsınız   . C Kitaplığı olduğundan, bu, kutudaki Swift 'ta erişilebilir değildir. Bir köprü oluşturma üst bilgisi kullanarak kitaplığı kullanılabilir hale getirebilirsiniz.

Köprü oluşturma üst bilgisini eklemek ve yapılandırmak için:

1. Xcode 'da **Dosya**, **Yeni**ve ardından **Dosya**' yı seçin ve ardından **üstbilgi dosyası**' nı seçin. Üst bilgi dosyasını **Bridgingheader. h**olarak adlandırın.

2.  **Commonhmac. h**dosyasını içeri aktarmak için dosyayı düzenleyin:

   ```swift
   #import <CommonCrypto/CommonHMAC.h>

   #ifndef BridgingHeader_h
   #define BridgingHeader_h

   #endif /* BridgingHeader_h */
   ```

3. Hedef **derleme ayarlarını köprü oluşturma**   başlığına başvuracak şekilde güncelleştirin:

   1.  **Pushdemo**   projesini seçin ve **Swift derleyicisi**   bölümüne kaydırın.

   2.  **Install hedefi-C uyumluluk üstbilgisi**   seçeneğinin **Yes**olarak ayarlandığından emin olun.

   3.  `<ProjectName>/BridgingHeader.h`    **Hedef-C köprü üstbilgi**seçeneğinde dosya yolunu girin   . Bu, köprü oluşturma üst bilgisinin dosya yoludur.

   Bu seçenekleri bulamıyorsanız, **All**    **temel**   veya **özelleştirilmiş**olmak yerine tüm görünüm ' ün seçili olduğundan emin olun.

   Birçok üçüncü taraf açık kaynaklı sarmalayıcı kitaplığı vardır ve bu, çok sayıda **Commonşifre**kullanarak   biraz daha kolay olabilir. Ancak, bu kitaplıkların tartışılması Bu makalenin kapsamı dışındadır.

4. Utilities klasörü içinde **Tokenutility. Swift**adlı yeni bir Swift dosyası ekleyin    **Utilities**   ve aşağıdaki kodu ekleyin:

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

   Bu yardımcı program SAS belirtecini oluşturmaktan sorumlu mantığı kapsüller.

   Daha önce açıklandığı gibi, **Getsastoken**   işlevi belirteci hazırlamak için gereken üst düzey adımları düzenler. İşlev, Bu öğreticinin ilerleyen kısımlarında yükleme hizmeti tarafından çağrılır.

   Diğer iki işlev **Getsastoken**   işlevi tarafından çağrılır: **sha256HMac**   IMZAYı hesaplama için sha256HMac ve Ilişkili URL dizesini kodlamak için **urlencodedstring**   .  **Urlencodedstring**   işlevi gereklidir, çünkü yerleşik **addingyüztenkodlama**işlevini kullanarak gerekli çıktıyı elde etmek mümkün değildir   .

    [Azure Storage IOS SDK 'sı](https://github.com/Azure/azure-storage-ios/blob/master/Lib/Azure%20Storage%20Client%20Library/Azure%20Storage%20Client%20Library/AZSUtil.m),   Bu işlemlere, amaç-C ' d e nasıl yaklaşıma yönelik harika bir örnektir. Azure Service Bus SAS belirteçleri hakkında daha fazla bilgi [Azure Service Bus belgelerinde](../service-bus-messaging/service-bus-sas.md)bulunabilir.

5.  **Appdelegate. Swift**Içinde,  `didRegisterForRemoteNotificationsWithDeviceToken`    **Tokenutility. getsastoken**'ın   geçerli bir belirteç oluşturduğunu doğrulamak için işlevine aşağıdaki kodu ekleyin

   ```swift
   let baseAddress = "https://<notificaitonHubNamespace>.servicebus.windows.net/<notifiationHubName>"

   let tokenData = TokenUtility.getSasToken(forResourceUrl: baseAddress,
                                            withKeyName: self.notificationHubKeyName!,
                                            andKey: self.notificationHubKey!)

   print(tokenData.token)
   ```

    **BaseAddress**   dizesindeki yer tutucuları kendi değerlerinizle değiştirdiğinizden emin olun.

## <a name="verify-the-sas-token"></a>SAS belirtecini doğrulama

Yükleme hizmetini istemcisine uygulamadan önce, uygulamanızın bir HTTP yardımcı programını kullanarak SAS belirtecini doğru bir şekilde ürettikten emin olun. Bu öğreticinin amaçları doğrultusunda, tercih ettiğiniz aracımız **Postman**' dır.

Uygulama tarafından oluşturulan **ınstald kimliğini**   ve **belirteç**değerlerini bir yere unutmayın   .

 **Yükleme**API 'sini çağırmak için aşağıdaki adımları izleyin   :

1.  **Postman**'da yeni bir sekme açın.
2. Get için isteği ayarlayın **GET**   ve şu adresi belirtin:

   ```xml
   https://<namespace>.servicebus.windows.net/<hubName>/installations/<installationId>?api-version=2015-01
   ```

3. İstek üstbilgilerini şu şekilde yapılandırın:

   | **Anahtar**       | **Değer**        |
   | ------------- | ---------------- |
   | İçerik Türü  | uygulama/json |
   | Yetkilendirme | \<sasToken\>     |
   | x-MS-sürümü  | 2015-01          |

4.  **Code**    **Kaydet**düğmesinin altındaki sağ üst köşede görüntülenen kod düğmesini seçin   . İstek aşağıdaki örneğe benzer şekilde görünmelidir:

   ```xml
   GET /<hubName>/installations/<installationId>?api-version=2015-01 HTTP/1.1
   Host: <namespace>.servicebus.windows.net
   Content-Type: application/json
   Authorization: <sasToken>
   x-ms-version: 2015-01
   Cache-Control: no-cache
   Postman-Token: <postmanToken>
   ```

5.  **Gönder**   düğmesini seçin.

Bu noktada belirtilen **Yüklemekimliği**için bir kayıt yok   . Doğrulamanın "401 Yetkisiz" yanıtı yerine "404 bulunamadı" yanıtı ile sonuçlanmalıdır. Bu sonuç SAS belirtecinin kabul edildiğini onaylamalıdır.

## <a name="implement-the-installation-service-class"></a>Yükleme hizmeti sınıfını uygulama

Sonra, [yüklemeler REST API](/rest/api/notificationhubs/create-overwrite-installation)etrafında temel bir sarmalayıcı uygulayın.

Hizmetler klasörü altına **Notificationregistrationservice. Swift**adlı yeni bir Swift dosyası ekleyin    **Services**   ve bu dosyaya aşağıdaki kodu ekleyin:

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

Önkoşul ayrıntıları, başlatmanın bir parçası olarak sağlanır. Etiketler ve şablonlar, isteğe bağlı olarak, **register**    **cihaz yüklemesi**JSON yükünün bir parçasını oluşturmak için Register işlevine geçirilir   .

 **Register**   işlevi, isteği hazırlamak için diğer özel işlevleri çağırır. Yanıt alındıktan sonra, tamamlama çağrılır ve kaydın başarılı olup olmadığını gösterir.

İstek uç noktası **GetBaseAddress**   işlevi tarafından oluşturulur. Yapım, *namespace*   başlatma sırasında sağlanmış olan Notification Hub parametreleri ad alanını ve *adını*kullanır   .

 **Getsastoken**   işlevi, şu anda saklı belirtecin geçerli olup olmadığını denetler. Belirteç geçerli değilse, işlev, **TokenUtility**   Yeni bir belirteç oluşturmak için tokenutility yöntemini çağırır ve sonra bir değer döndürmeden önce onu depolar.

Son olarak, **Encodetojson**,   istek gövdesinin bir parçası olarak kullanılmak üzere ılgılı model nesnelerini JSON 'a dönüştürür.

## <a name="invoke-the-notification-hubs-rest-api"></a>Notification Hubs REST API çağır

Son adım, **AppDelegate**   notificationhub 'A kaydolmak için Appdelegate 'ı **notificationregistrationservice**kullanacak şekilde güncelleştiriyor   . **NotificationHub**

1.  **AppDelegate.swift**    **Noficiationregistrationservice**   ve genel **pushtemplate**başvurusunu depolamak için appdelegate. Swift ' i açın ve sınıf düzeyi değişkenler ekleyin:

   ```swift
   var registrationService : NotificationRegistrationService?
   let genericTemplate = PushTemplate(withBody: "{\"aps\":{\"alert\":\"$(message)\"}}")
   ```

2. Aynı dosyada, **Ddregisterforremotenocertificate Ationswithdevicetoken**   Işlevini kullanarak **notificationregistrationservice**   öğesini önkoşul parametreleriyle başlatın ve ardından **register**   işlevini çağırın.

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

   Kolaylık olması için, kod `print` deyimleri kullanarak çıkış penceresini **kayıt**   işleminin sonucuyla güncelleştirir.

3. Uygulamayı bir fiziksel cihazda derleyin ve çalıştırın. Çıkış penceresinde **kayıtlı** ' i görmeniz gerekir.

## <a name="test-the-solution"></a>Çözümü test etme

Bu noktada, uygulama **Notificationhub**'a kaydedilir   ve anında iletme bildirimleri alabilir. Xcode 'da, hata ayıklayıcıyı durdurun ve şu anda çalışıyorsa uygulamayı kapatın. Ardından, **Cihaz yükleme**   ayrıntılarının beklenen şekilde göründüğünden ve uygulamanın artık anında iletme bildirimleri alabilmesine bakın.

### <a name="verify-the-device-installation"></a>Cihaz yüklemesini doğrulama

Artık, daha önce yaptığınız gibi, **Postman**    [SAS belirtecini doğrulamak](notification-hubs-ios-push-notifications-swift-apps-get-started.md#verify-the-sas-token)için Postman kullanarak aynı isteği yapabilirsiniz. SAS belirtecinin süresinin dolmadığı varsayıldığında, yanıt artık, sizin belirttiğiniz şablonlar ve Etiketler gibi yükleme ayrıntılarını içermelidir.

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

Önceki SAS belirtecinizin süresi dolmuşsa, **TokenUtility**   Yeni bir SAS belirteci almak ve **Yetkilendirme**   üstbilgisini bu yeni değerle güncelleştirmek için tokenutility sınıfının 24. satırına bir kesme noktası ekleyebilirsiniz.

### <a name="send-a-test-notification-azure-portal"></a>Test bildirimi (Azure portal) gönder

Artık bildirimleri alabilmeniz için en hızlı yol, Azure portal Bildirim Hub 'ına gözatabiliyor.

1. Azure portal, Bildirim Hub 'ınızdaki **genel bakış**   sekmesine göz atın.

2. Portal penceresinin sol üst kısmındaki **temel**bileşenler özetinin üzerinde olan **Test gönder**' i seçin   :

   :::image type="content" source="media/ios-sdk-swift-rest/image2.png" alt-text="Notification Hubs Essentials Özet testini gönder":::

3. Platformlar listesinden **özel şablon**' ı seçin    **Platforms**   .

4.  **12345**    **Send to Tag ifadesi**için 12345 girin. Bu etiketi daha önce yüklemenizde belirtmiştiniz.

5. İsteğe bağlı olarak, **message**   JSON yükünde iletiyi düzenleyin:

   :::image type="content" source="media/ios-sdk-swift-rest/image3.png" alt-text="Notification Hubs testi gönder":::

6.  **Gönder**' i seçin. Portalın, bildirimin cihaza başarıyla gönderilip gönderilmediğini belirtmesi gerekir:

   :::image type="content" source="media/ios-sdk-swift-rest/image4.png" alt-text="Test gönderme sonucu":::

   Uygulamanın ön planda çalışmadığı varsayıldığında, cihazınızdaki **bildirim merkezinde**bir bildirim de görmeniz gerekir   . Bildirime dokunduğunuzda uygulamayı açmanız ve uyarının gösterilmesi gerekir.

   :::image type="content" source="media/ios-sdk-swift-rest/image5.png" alt-text="Test bildirimi":::

### <a name="send-a-test-notification-mail-carrier"></a>Test bildirimi gönderme (posta taşıyıcısı)

 [REST API](/rest/api/notificationhubs/)   Sınama için daha kolay bir yol olabilecek **Postman**kullanarak REST API bildirim gönderebilirsiniz.

1.  **Postman**'da yeni bir sekme açın.

2.  **Gönderi**isteği olarak ayarlayın ve şu adresi girin:

   ```xml
   https://<namespace>.servicebus.windows.net/<hubName>/messages/?api-version=2015-01
   ```

3. İstek üstbilgilerini şu şekilde yapılandırın:

   | Anahtar                           | Değer                          |
   | ----------------------------- | ------------------------------ |
   | İçerik Türü                  | Uygulama/JSON; charset = UTF-8 |
   | Yetkilendirme                 | \<sasToken\>                   |
   | ServiceBusNotification-biçim | şablon                       |
   | Etiketler                          | "12345"                        |

4.  **BODY**   Aşağıdaki JSON yüküyle **Ham JSON (application.json)** kullanmak için istek gövdesini yapılandırın   :

   ```json
   {
   "message" : "Hello from Postman!"
   }
   ```

5.  **Code**   Pencerenin sağ üst kısmındaki **Kaydet**düğmesi altında bulunan kod düğmesini seçin   . İstek aşağıdaki örneğe benzer şekilde görünmelidir:

   ```xml
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

6.  **Gönder**   düğmesini seçin.

 **201 oluşturma**   başarılı bir durum kodu almalısınız ve istemci cihazında bildirimi alacaksınız.

## <a name="next-steps"></a>Sonraki adımlar

Artık [Notification Hubs REST API](/rest/api/notificationhubs/)aracılığıyla bir Bildirim Hub 'ına bağlı temel bir iOS Swift uygulamasına sahipsiniz ve bildirim gönderebilir ve alabilir. Belirli cihazlara bildirim gönderme hakkında daha fazla bilgi edinmek için aşağıdaki öğreticiye ilerleyin:

- [Öğretici: belirli cihazlara anında Iletme bildirimleri gönderme](notification-hubs-ios-xplat-segmented-apns-push-notification.md)

Daha fazla bilgi için aşağıdaki makaleleri inceleyin:

- [Azure Notification Hubs genel bakış](notification-hubs-push-notification-overview.md)
- [REST API 'Leri Notification Hubs](/rest/api/notificationhubs/)
- [Arka uç işlemleri için SDK Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)
- [GitHub 'da SDK 'Yı Notification Hubs](https://github.com/Azure/azure-notificationhubs)
- [Uygulama arka ucuna kaydol](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
- [Kayıt yönetimi](notification-hubs-push-notification-registration-management.md)
- [Etiketlerle çalışma](notification-hubs-tags-segment-push-message.md)
- [Özel şablonlarla çalışma](notification-hubs-templates-cross-platform-push-messages.md)
- [Paylaşılan erişim imzaları ile erişim denetimi Service Bus](../service-bus-messaging/service-bus-sas.md)
- [Program aracılığıyla SAS belirteçleri oluşturma](/rest/api/eventhub/generate-sas-token)
- [Apple güvenliği: ortak şifre](https://developer.apple.com/security/)
- [UNIX dönem saati](https://en.wikipedia.org/wiki/Unix_time)
- [HMAC](https://en.wikipedia.org/wiki/HMAC)
