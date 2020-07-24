---
title: Azure Notification Hubs ve iOS SDK sürümü 3.0.0 Preview 1 kullanarak iOS 'a anında iletme bildirimleri gönderin
description: Bu öğreticide, iOS cihazlarına anında iletme bildirimleri göndermek için Azure Notification Hubs ve Apple Anında Iletilen bildirim hizmetini nasıl kullanacağınızı öğreneceksiniz.
author: sethmanheim
ms.author: sethm
ms.date: 06/19/2020
ms.topic: tutorial
ms.service: notification-hubs
ms.reviewer: thsomasu
ms.lastreviewed: 06/01/2020
ms.openlocfilehash: f43db986f70e64e55225465223b7e324450541a8
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87084346"
---
# <a name="tutorial-send-push-notifications-to-ios-apps-using-azure-notification-hubs-version-300-preview1"></a>Öğretici: Azure Notification Hubs kullanarak iOS uygulamalarına anında iletme bildirimleri gönderme (sürüm 3.0.0-preview1 'i)

Bu öğretici, Azure Notification Hubs SDK sürümü 2.0.4 kullanarak bir iOS uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs nasıl kullanacağınızı gösterir.

Bu öğreticide aşağıdaki adımlar yer almaktadır:

- Örnek bir iOS uygulaması oluşturun.
- İOS uygulamanızı Azure Notification Hubs bağlayın.
- Test anında iletme bildirimleri gönderin.
- Uygulamanızın bildirimleri aldığını doğrulayın.

Bu öğreticinin tüm kodunu [GitHub 'dan](https://github.com/Azure/azure-notificationhubs-ios/tree/v3-preview2/Samples)indirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlayabilmeniz için aşağıdaki önkoşullara sahip olmanız gerekir:

- Anahtarınıza yüklenmiş geçerli bir geliştirici sertifikasıyla birlikte [Xcode](https://go.microsoft.com/fwLink/p/?LinkID=266532)çalıştıran bir Mac.
- İOS sürüm 10 veya üstünü çalıştıran bir iPhone veya iPad.
- Fiziksel cihazınız [Apple portalında](https://developer.apple.com/)kayıtlı ve sertifikanız ile ilişkili.

Devam etmeden önce, Bildirim Hub 'ınızdaki anında iletme kimlik bilgilerini ayarlamak ve yapılandırmak için [iOS uygulamaları Için Azure Notification Hubs](ios-sdk-get-started.md)kullanmaya başlama hakkında önceki öğreticiye gitdiğinizden emin olun. İOS geliştirmeyle ilgili önceki bir deneyim olmasa bile, bu adımları takip edebilmelisiniz.

> [!NOTE]
> Anında iletme bildirimlerinin yapılandırma gereksinimleri nedeniyle, iOS öykünücüsü yerine bir fiziksel iOS cihazında (iPhone veya iPad) anında iletme bildirimleri dağıtmanız ve test etmeniz gerekir.

## <a name="connect-your-ios-app-to-notification-hubs"></a>iOS uygulamanızı Notification Hubs'a bağlama

1. Xcode 'da yeni bir iOS projesi oluşturun ve **tek görünüm uygulama**   şablonunu seçin.

   :::image type="content" source="media/ios-sdk/image1.png" alt-text="Şablon seç":::

2. Yeni projeniz için seçenekleri ayarlarken, **Product Name**   Apple Developer Portal 'da paket tanımlayıcısını ayarlarken kullandığınız ürün adını ve **kuruluş tanımlayıcısını**kullandığınızdan emin olun   .

3. Proje Gezgini altında, **hedefler**altında proje adınızı seçin, sonra **imzalama & özellikleri**   sekmesini seçin. Apple geliştirici hesabınız için uygun **ekibi**seçtiğinizden emin olun   . XCode, paket tanımlayıcınızı temel alarak önceden oluşturduğunuz Sağlama Profilini otomatik olarak aşağı çekmelidir.

   Xcode'da oluşturduğunuz yeni hazırlama profilini göremiyorsanız imzalama kimliğiniz için profilleri yenilemeyi deneyin. Menü çubuğunda **Xcode**' a tıklayın,    **Tercihler**' e tıklayın, **Hesap**   sekmesine tıklayın, **Ayrıntıları görüntüle**   düğmesine tıklayın, imzalama kimliğinize tıklayın ve sonra sağ alt köşedeki Yenile düğmesine tıklayın.

   :::image type="content" source="media/ios-sdk/image2.png" alt-text="Ayrıntıları görüntüle":::

4.  **İmzalama & özellikleri**   sekmesinde **+ yetenek**' ı seçin.  **Anında Iletme bildirimleri**' ne çift tıklayarak   etkinleştirin.

   :::image type="content" source="media/ios-sdk/image3.png" alt-text="Özellik":::

5. Azure Notification Hubs SDK modüllerini ekleyin.

    [Cocoapods](https://cocoapods.org/)kullanarak   veya ikili dosyaları projenize el ile ekleyerek Azure Notification Hubs SDK 'sını uygulamanız ile tümleştirebilirsiniz.

   - Cocoapods aracılığıyla tümleştirme: uygulamanıza Azure Notification Hubs SDK 'yı dahil etmek için aşağıdaki bağımlılıkları Pod dosyanıza ekleyin:

      ```ruby
      pod 'AzureNotificationHubs-iOS'
      ```

      - Yeni tanımlanan Pod 'nizi yüklemek ve. xcworkspace Dosyanızı açmak için pod Install 'ı çalıştırın.

         Pod yüklemesi çalıştırılırken **azurenocertificate 'lar-iOS için bir belirtim bulamıyor** gibi bir hata görürseniz, `pod repo update` Cocoapods deposundan en son Pod 'yi almak için komutunu çalıştırın ve pod Install komutunu çalıştırın.

   - İkililerini projenize kopyalayarak tümleştirme:

      İkililerini projenize kopyalayarak aşağıdaki gibi tümleştirebilirsiniz:

        - Zip dosyası olarak sunulan [Azure NOTIFICATION HUBS SDK](https://github.com/Azure/azure-notificationhubs-iOS/releases/)   çerçevesini indirin ve sıkıştırmayı açın.

        - Xcode 'da projenize sağ tıklayın ve **Add Files to**    **Windowsazuremessaging. Framework**   klasörünü Xcode projenize eklemek için dosya Ekle seçeneğine tıklayın.  **Seçenekler**   ' i seçin ve **gerekirse öğeleri Kopyala**   ' nın seçili olduğundan emin olun ve ardından **Ekle**' ye tıklayın.

          :::image type="content" source="media/ios-sdk/image4.png" alt-text="Çerçeve Ekle":::

6. Projenize **sabitler. h**adlı yeni bir üst bilgi dosyası ekleyin. Bunu yapmak için proje adına sağ tıklayın ve **yeni dosya...** öğesini seçin. Ardından **üstbilgi dosyası**' nı seçin. Bu dosya, bildirim hub’ınız için sabitleri tutar. Ardından **İleri**' yi seçin. Dosya **sabitlerini adlandırın. h**.

7. Aşağıdaki kodu sabitler. h dosyasına ekleyin:

   ```objc
   #ifndef Constants_h
   #define Constants_h
   extern NSString* const NHInfoConnectionString;
   extern NSString* const NHInfoHubName;
   extern NSString* const NHUserDefaultTags;
   #endif /* Constants_h */
   ```

8. Sabitler. h için uygulama dosyası ekleyin. Bunu yapmak için proje adına sağ tıklayın ve **yeni dosya...** öğesini seçin.  **Amaç-C dosyası**' nı seçin ve ardından **İleri**' yi seçin. Dosya **sabitlerini adlandırın. d**.

   :::image type="content" source="media/ios-sdk/image5.png" alt-text="Uygulama dosyası Ekle":::

9.  **Sabitleri. d**   dosyasını açın ve içeriğini aşağıdaki kodla değiştirin.  `NotificationHubConnectionString`    `NotificationHubConnectionString`   Daha önce portaldan elde ettiğiniz gibi, dize değişmez yer tutucuları ve hub adı ve **Defaultlistensharedaccesssignature**ile değiştirin:

   ```objc
   #import <Foundation/Foundation.h>
   #import "Constants.h"

   NSString* const NHInfoConnectionString = @"NotificationHubConnectionString";
   NSString* const NHInfoHubName = @"NotificationHubName";NSString* const NHUserDefaultTags = @"notification_tags";
   ```

10. Project **Appdelegate. h**   dosyasında aşağıdaki `import` ifadeyi ekleyin:

    ```objc
    #import "Constants.h"
    ```

11. Aynı **Appdelegate. d**   dosyasında aşağıdaki kodla sonraki tüm kodu değiştirin  `didFinishLaunchingWithOptions`   :

    ```objc
    // Tells the delegate that the app successfully registered with Apple Push Notification service (APNs).


    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {

    NSString *hubName = [[NSBundle mainBundle] objectForInfoDictionaryKey:NHInfoHubName];
    NSString *connectionString = [[NSBundle mainBundle] objectForInfoDictionaryKey:NHInfoConnectionString];
    [MSNotificationHub initWithConnectionString:connectionString withHubName:hubName];


    NSMutableSet *tags = [[NSMutableSet alloc] init];

    // Load and parse stored tags
    NSString *unparsedTags = [[NSUserDefaults standardUserDefaults] valueForKey:NHUserDefaultTags];
    if (unparsedTags.length > 0) {
        NSArray *tagsArray = [unparsedTags componentsSeparatedByString: @","];

        [MSNotificationHub addTags:tagsArray];
    }

    }
    - (void)showAlert:(NSString *)message withTitle:(NSString *)title {
    if (title == nil) {
        title = @"Alert";
    }
    UIAlertController *alert = [UIAlertController alertControllerWithTitle:title message:message preferredStyle:UIAlertControllerStyleAlert];
    [alert addAction:[UIAlertAction actionWithTitle:@"OK" style:UIAlertActionStyleDefault handler:nil]];
    [[[[UIApplication sharedApplication] keyWindow] rootViewController] presentViewController:alert animated:YES completion:nil];
    }

    - (void)showNotification:(NSDictionary *)userInfo {
    [self logNotificationDetails:userInfo];

    NotificationDetailViewController *notificationDetail = [[NotificationDetailViewController alloc] initWithUserInfo:userInfo];
    [[[[UIApplication sharedApplication] keyWindow] rootViewController] presentViewController:notificationDetail animated:YES completion:nil];
    }

    @end
    ```

    Bu kod, **sabitler. h**içinde belirttiğiniz bağlantı bilgilerini kullanarak Bildirim Hub 'ına bağlanır. Daha sonra, hub 'ın bildirim gönderebilmesi için cihaz belirtecini Bildirim Hub 'ına verir.

### <a name="create-notificationdetailviewcontroller-header-file"></a>NotificationDetailViewController üst bilgi dosyası oluştur

1. Önceki yönergelere benzer şekilde, **Notificationdetailviewcontroller. h**adlı başka bir üst bilgi dosyası ekleyin. Yeni üstbilgi dosyasının içeriğini aşağıdaki kodla değiştirin:

   ```objc
   #import <UIKit/UIKit.h>

   NS_ASSUME_NONNULL_BEGIN

   @interface NotificationDetailViewController : UIViewController

   @property (strong, nonatomic) IBOutlet UILabel *titleLabel;
   @property (strong, nonatomic) IBOutlet UILabel *bodyLabel;
   @property (strong, nonatomic) IBOutlet UIButton *dismissButton;

   @property (strong, nonatomic) NSDictionary *userInfo;

   - (id)initWithUserInfo:(NSDictionary *)userInfo;

   @end

   NS_ASSUME_NONNULL_END
   ```

2.  **Notificationdetailviewcontroller. d**uygulama dosyasını ekleyin. Dosya içeriğini, UIViewController yöntemlerini uygulayan aşağıdaki kodla değiştirin:

   ```objc
   #import "NotificationDetailViewController.h"

   @interface NotificationDetailViewController ()

   @end

   @implementation NotificationDetailViewController

   - (id)initWithUserInfo:(NSDictionary *)userInfo {
    self = [super initWithNibName:@"NotificationDetail" bundle:nil];
    if (self) {
        _userInfo = userInfo;
    }
    return self;
   }

   - (void)viewDidLayoutSubviews {
    [self.titleLabel sizeToFit];
    [self.bodyLabel sizeToFit];
   }

   - (void)viewDidLoad {
    [super viewDidLoad];

    NSString *title = nil;
    NSString *body = nil;

    NSDictionary *aps = [_userInfo valueForKey:@"aps"];
    NSObject *alertObject = [aps valueForKey:@"alert"];
    if (alertObject != nil) {
        if ([alertObject isKindOfClass:[NSDictionary class]]) {
            NSDictionary *alertDict = (NSDictionary *)alertObject;
            title = [alertDict valueForKey:@"title"];
            body = [alertObject valueForKey:@"body"];
        } else if ([alertObject isKindOfClass:[NSString class]]) {
            body = (NSString *)alertObject;
        } else {
            NSLog(@"Unable to parse notification content. Unexpected format: %@", alertObject);
        }
    }

    if (title == nil) {
        title = @"<unset>";
    }

    if (body == nil) {
        body = @"<unset>";
    }

    self.titleLabel.text = title;
    self.bodyLabel.text = body;
   }

   - (IBAction)handleDismiss:(id)sender {
    [self dismissViewControllerAnimated:YES completion:nil];
   }

   @end
   ```

### <a name="viewcontroller"></a>ViewController

1. Project **ViewController. h**   dosyasında aşağıdaki  `import`   deyimleri ekleyin:

   ```objc
   #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
   #import <UserNotifications/UserNotifications.h>
   ```

2. Ayrıca, **ViewController. h**' de, bildirimden sonra aşağıdaki özellik bildirimini ekleyin  `@interface`   :

   ```objc
   @property (strong, nonatomic) IBOutlet UITextField *tagsTextField;
   ```

3. Projenin **ViewController. d**   uygulama dosyasında, dosyanın içeriğini aşağıdaki kodla değiştirin:

   ```objc
   #import "ViewController.h"
   #import "Constants.h"
   #import "AppDelegate.h"

   @interface ViewController ()

   @end

   @implementation ViewController

   // UIViewController methods

   - (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event {
    // Simple method to dismiss keyboard when user taps outside of the UITextField.
    [self.view endEditing:YES];
   }

   - (void)viewDidLoad {
    [super viewDidLoad];

    // Load raw tags text from storage and initialize the text field
    self.tagsTextField.text = [[NSUserDefaults standardUserDefaults] valueForKey:NHUserDefaultTags];
   }

   - (IBAction)handleRegister:(id)sender {
    // Save raw tags text in storage
    [[NSUserDefaults standardUserDefaults] setValue:self.tagsTextField.text forKey:NHUserDefaultTags];

   // Delegate processing the register action to the app delegate.
   [[[UIApplication sharedApplication] delegate] performSelector:@selector(handleRegister)];
   }

   - (IBAction)handleUnregister:(id)sender {
    [[[UIApplication sharedApplication] delegate] performSelector:@selector(handleUnregister)];
   }

   @end
   ```

4. Herhangi bir hata olmadığını doğrulamak için cihazınızda uygulamayı derleyin ve çalıştırın.

## <a name="send-test-push-notifications"></a>Test amaçlı anında iletme bildirimleri gönderme

Uygulamanızdaki bildirimleri **Test gönder**    [Azure Portal](https://portal.azure.com/)seçeneğiyle test edebilirsiniz. Bu, cihazınıza test amaçlı anında iletme bildirimi gönderir.

:::image type="content" source="media/ios-sdk/image6.png" alt-text="Testi gönder":::

Anında iletme bildirimleri normal olarak, uyumlu bir kitaplık kullanılarak Mobile Apps veya ASP.NET gibi bir arka uç hizmetine gönderilir. Arka uçta bir kitaplık yoksa, bildirim iletilerini göndermek için doğrudan REST API de kullanabilirsiniz.

Bildirim göndermek için gözden geçirmek isteyebileceğiniz diğer bazı öğreticilerin listesi aşağıda verilmiştir:

- Azure Mobile Apps: Notification Hubs ile tümleştirilmiş Mobile Apps arka uçta bildirimlerin nasıl gönderileceği hakkında bir örnek Için bkz. [IOS uygulamanıza anında Iletme bildirimleri ekleme](/azure/app-service-mobile/app-service-mobile-ios-get-started-push).
- ASP.NET: [kullanıcılara anında iletme bildirimleri göndermek için Notification Hubs kullanın](notification-hubs-aspnet-backend-ios-apple-apns-notification.md).
- Azure Notification Hubs Java SDK 'Sı: Java 'dan bildirim göndermek için [Java 'dan Notification Hubs nasıl kullanacağınızı](notification-hubs-java-push-notification-tutorial.md)öğrenin   . Android Geliştirmesi için Eclipse'te test edilmiştir.
- PHP: [php 'den Notification Hubs nasıl kullanılır](notification-hubs-php-push-notification-tutorial.md).

## <a name="verify-that-your-app-receives-push-notifications"></a>Uygulamanızın anında iletme bildirimleri aldığını doğrulama

iOS'ta anında iletme bildirimlerini test etmek için, uygulamayı fiziksel bir iOS cihazına dağıtmanız gerekir. İOS simülatörü kullanarak Apple anında iletme bildirimleri gönderemezsiniz.

1. Uygulamayı çalıştırın ve kaydın başarılı olduğunu doğrulayıp **Tamam**' a basın.

   :::image type="content" source="media/ios-sdk/image7.png" alt-text="Kaydet":::

2. Ardından, önceki bölümde açıklandığı gibi [Azure Portal](https://portal.azure.com/)bir test anında iletme bildirimi gönderin.

3. Anında iletme bildirimi, verilen Bildirim Hub 'ından bildirimleri almak için kayıtlı tüm cihazlara gönderilir.

   :::image type="content" source="media/ios-sdk/image8.png" alt-text="Test gönder":::

## <a name="next-steps"></a>Sonraki adımlar

Bu basit örnekte, tüm kayıtlı iOS cihazlarınıza anında iletme bildirimleri yayınlayacaktır. Belirli iOS cihazlarına anında iletme bildirimleri göndermeyi öğrenmek için aşağıdaki öğreticiye ilerleyin:

[Öğretici: belirli cihazlara anında Iletme bildirimleri gönderme](notification-hubs-ios-xplat-segmented-apns-push-notification.md)

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
