---
title: Azure Bildirim Hub'larını kullanarak belirli iOS aygıtlarına anında iletme bildirimleri gönderme | Microsoft Dokümanlar
description: Bu eğitimde, belirli iOS aygıtlarına anında iletme bildirimleri göndermek için Azure Bildirim Hub'larını nasıl kullanacağınızı öğrenirsiniz.
services: notification-hubs
documentationcenter: ios
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: 6ead4169-deff-4947-858c-8c6cf03cc3b2
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 11/07/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 11/07/2019
ms.openlocfilehash: a775963f1b0fa19cd687c839f527f4a078c76864
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80126988"
---
# <a name="tutorial-send-push-notifications-to-specific-ios-devices-using-azure-notification-hubs"></a>Öğretici: Azure Bildirim Hub'larını kullanarak belirli iOS aygıtlarına anında iletme bildirimleri gönderme

[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Genel Bakış

Bu öğretici, bir iOS uygulamasına son dakika haber bildirimlerini yayınlamak için Azure Bildirim Hub'larını nasıl kullanacağınızı gösterir. Tamamlandığında, ilgilendiğiniz son dakika haber kategorileri için kaydolabilir ve yalnızca bu kategoriler için anında iletme bildirimleri alabilirsiniz. Bu senaryo, daha önce ilgisini belirtmiş kullanıcı gruplarına bildirim gönderilmesi gereken RSS okuyucu, müzik hayranlarına yönelik uygulamalar vb. birçok uygulama için ortak düzendir.

Yayın senaryoları, bildirim hub’ında bir kayıt oluştururken bir veya daha fazla *etiket* dahil edilerek etkinleştirilir. Bildirimler bir etikete gönderildiğinde, etikete kaydolmuş aygıtlar bildirimi alır. Etiketler basitçe birer dize olduğundan, önceden hazırlanması gerekmez. Etiketler hakkında daha fazla bilgi için bkz. [Notification Hubs Yönlendirme ve Etiket İfadeleri](notification-hubs-tags-segment-push-message.md).

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Uygulamaya kategori seçimi ekleme
> * Etiketli bildirimler gönderme
> * Cihazdan bildirim gönderme
> * Uygulamayı çalıştırma ve bildirimler oluşturma

## <a name="prerequisites"></a>Ön koşullar

Bu konu, [Öğretici: Azure Bildirim Hub'larını kullanarak iOS uygulamalarına bildirimler ilerletme][get-started]uygulamasında oluşturduğunuz uygulamaya dayanmaktadır. Bu öğreticiyi başlatmadan [önce, Öğretici: Azure Bildirim Hub'larını kullanarak iOS uygulamalarına bildirimleri iletme][get-started]eğitimini tamamlamış olmalısınız.

## <a name="add-category-selection-to-the-app"></a>Uygulamaya kategori seçimi ekleme

İlk adım, kullanıcının kaydolmak için kategorileri seçmesini sağlayan varolan film şeridinize Kullanıcı Arabirimi öğelerini eklemektir. Bir kullanıcı tarafından seçilen kategoriler cihazda depolanır. Uygulama başlatıldığında, etiketler olarak seçilen kategorilerle bildirim hub’ınızda bir cihaz kaydı oluşturulur.

1. **MainStoryboard_iPhone.storyboard'unuzda** nesne kitaplığından aşağıdaki bileşenleri ekleyin:

   * "Son Dakika Haberleri" yazan bir etiket.
   * Kategori metinleri "Dünya", "Siyaset", "İş", "Teknoloji", "Bilim", "Spor" ile etiketler,
   * Kategori başına bir olmak üzere altı anahtar, her anahtar **durumunu** varsayılan olarak **Kapalı** olarak ayarlayın.
   * "Abone Ol" etiketli tek düğme

     Hikaye şeridiniz aşağıdaki gibi görünmelidir:

     ![Xcode arayüz oluşturucu][3]

2. Yardımcı editörolarak, tüm anahtarlar için çıkışlar oluşturun ve onlara "WorldSwitch", "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch" deyin.

3. Adını `subscribe`verdi niz için bir Eylem oluşturun; aşağıdaki `ViewController.h` kodu içermelidir:

    ```objc
    @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
    @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
    @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
    @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
    @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
    @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;

    - (IBAction)subscribe:(id)sender;
    ```

4. Yeni bir **Cocoa Touch Class** adlı `Notifications`oluşturun. Bildirims.h dosyasının arayüz bölümünde aşağıdaki kodu kopyalayın:

    ```objc
    @property NSData* deviceToken;

    - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;

    - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                completion:(void (^)(NSError* error))completion;

    - (NSSet*)retrieveCategories;

    - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;
    ```

5. Bildirimler.m'ye aşağıdaki alma yönergesini ekleyin:

    ```objc
    #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
    ```

6. Bildirimler dosyasının uygulama bölümünde aşağıdaki kodu kopyalayın.

    ```objc
    SBNotificationHub* hub;

    - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName{

        hub = [[SBNotificationHub alloc] initWithConnectionString:listenConnectionString
                                    notificationHubPath:hubName];

        return self;
    }

    - (void)storeCategoriesAndSubscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion {
        NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
        [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];

        [self subscribeWithCategories:categories completion:completion];
    }

    - (NSSet*)retrieveCategories {
        NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

        NSArray* categories = [defaults stringArrayForKey:@"BreakingNewsCategories"];

        if (!categories) return [[NSSet alloc] init];
        return [[NSSet alloc] initWithArray:categories];
    }

    - (void)subscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion
    {
        //[hub registerNativeWithDeviceToken:self.deviceToken tags:categories completion: completion];

        NSString* templateBodyAPNS = @"{\"aps\":{\"alert\":\"$(messageParam)\"}}";

        [hub registerTemplateWithDeviceToken:self.deviceToken name:@"simpleAPNSTemplate" 
            jsonBodyTemplate:templateBodyAPNS expiryTemplate:@"0" tags:categories completion:completion];
    }
    ```

    Bu sınıf, bu aygıtın aldığı haber kategorilerini depolamak ve almak için yerel depolama yı kullanır. Ayrıca, [şablon](notification-hubs-templates-cross-platform-push-messages.md) kaydı kullanarak bu kategorilere kaydolmak için bir yöntem içerir.

7. `AppDelegate.h` Dosyada, `Notifications` sınıfın bir örneği için `Notifications.h` bir alma deyimi ekleyin ve bir özellik ekleyin:

    ```objc
    #import "Notifications.h"

    @property (nonatomic) Notifications* notifications;
    ```

8. `didFinishLaunchingWithOptions` Yöntemde, `AppDelegate.m`yöntemin başında bildirimler örneğini başlatmakodu ekleyin.  
    `HUBNAME`ve `HUBLISTENACCESS` `hubinfo.h`(tanımlı) zaten `<hub name>` bildirim `<connection string with listen access>` hub adı ve daha önce elde ettiğiniz *DefaultListenSharedAccessSignature* için bağlantı dizesi ile ve yer tutucuları değiştirmelidir

    ```objc
    self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];
    ```

    > [!NOTE]
    > Bir istemci uygulaması ile dağıtılmış kimlik bilgileri genellikle güvenli olmadığından yalnızca istemci uygulamanızla dinleme erişimi için anahtarı dağıtmanız gerekir. Dinleme erişimi, uygulamanızın bildirimlere kaydolmasını sağlar, ancak mevcut kayıtlar değiştirilemez ve bildirimler gönderilemez. Tam erişim anahtarı, güvenli bir arka uç hizmetinde bildirimler göndermek ve mevcut kayıtları değiştirmek için kullanılır.

9. `didRegisterForRemoteNotificationsWithDeviceToken` Yöntemde, aygıt belirteci sınıfa geçmek için `notifications` yöntemdeki kodu aşağıdaki kodla değiştirin. `AppDelegate.m` Sınıf `notifications` kategorileri ile bildirimler için kayıt gerçekleştirir. Kullanıcı kategori seçimlerini değiştirirse, `subscribeWithCategories` bunları güncellemek için **abone düğmesine** yanıt olarak yöntemi arayın.

    > [!NOTE]
    > Apple Push Bildirim Hizmeti (APNS) tarafından atanan aygıt belirteci herhangi bir zamanda değişebildiği için, bildirim hatalarını önlemek için bildirimlere sık sık kaydolmanız gerekir. Bu örnek, uygulama her başlatıldığında bildirimlere kaydolur. Sık sık çalıştırılan uygulamalar için, önceki kayıttan bu yana bir günden az zaman geçtiyse bant genişliğini korumak için günde birkaç kere kaydı atlayabilirsiniz.

    ```objc
    self.notifications.deviceToken = deviceToken;

    // Retrieves the categories from local storage and requests a registration for these categories
    // each time the app starts and performs a registration.

    NSSet* categories = [self.notifications retrieveCategories];
    [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
        if (error != nil) {
            NSLog(@"Error registering for notifications: %@", error);
        }
    }];
    ```

    Bu noktada, `didRegisterForRemoteNotificationsWithDeviceToken` yöntemde başka bir kod olmamalıdır.

10. Bildirim `AppDelegate.m` [Hub'ları öğreticisiyle başlatılan Get'i][get-started] tamamladıktan sonra aşağıdaki yöntemler zaten mevcut olmalıdır. Değilse, ekleyin.

    ```objc
    - (void)MessageBox:(NSString *)title message:(NSString *)messageText
    {

        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
            cancelButtonTitle:@"OK" otherButtonTitles: nil];
        [alert show];
    }

    - (void)application:(UIApplication *)application didReceiveRemoteNotification:
       (NSDictionary *)userInfo {
       NSLog(@"%@", userInfo);
       [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
     }
    ```

    Bu yöntem, uygulama çalışırken alınan bildirimleri basit bir **UIAlert**görüntüleyerek işler.

11. In `ViewController.m`, `import` için `AppDelegate.h` bir deyim ekleyin ve XCode `subscribe` oluşturulan yönteme aşağıdaki kodu kopyalayın. Bu kod, kullanıcının kullanıcı arabiriminde seçtiği yeni kategori etiketlerini kullanmak için bildirim kaydını güncelleştirir.

    ```objc
    #import "Notifications.h"

    NSMutableArray* categories = [[NSMutableArray alloc] init];

    if (self.WorldSwitch.isOn) [categories addObject:@"World"];
    if (self.PoliticsSwitch.isOn) [categories addObject:@"Politics"];
    if (self.BusinessSwitch.isOn) [categories addObject:@"Business"];
    if (self.TechnologySwitch.isOn) [categories addObject:@"Technology"];
    if (self.ScienceSwitch.isOn) [categories addObject:@"Science"];
    if (self.SportsSwitch.isOn) [categories addObject:@"Sports"];

    Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

    [notifications storeCategoriesAndSubscribeWithCategories:categories completion: ^(NSError* error) {
        if (!error) {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:"Notification" message:"Subscribed" delegate:self
            cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        } else {
            NSLog(@"Error subscribing: %@", error);
        }
    }];
    ```

    Bu yöntem bir `NSMutableArray` kategori oluşturur `Notifications` ve listeyi yerel depolama alanında depolamak için sınıfı kullanır ve ilgili etiketleri bildirim merkezinize kaydeder. Kategoriler değiştirildiğinde kayıt yeni kategorilerle yeniden oluşturulur.

12. Kullanıcı `ViewController.m`arabirimini `viewDidLoad` daha önce kaydedilmiş kategorilere göre ayarlamak için yönteme aşağıdaki kodu ekleyin.

    ```objc
    // This updates the UI on startup based on the status of previously saved categories.

    Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

    NSSet* categories = [notifications retrieveCategories];

    if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
    if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
    if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
    if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
    if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
    if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;
    ```

Uygulama artık, uygulama her başlatıldığında bildirim merkezine kaydolmak için kullanılan aygıt yerel depolama alanında bir dizi kategori depolayabilir. Kullanıcı çalışma zamanında kategorilerin seçimini değiştirebilir ve `subscribe` cihazın kaydını güncelleştirmek için yöntemi tıklatabilir. Ardından, son dakika haberlerini doğrudan uygulamanın kendisine göndermek için uygulamayı güncellersiniz.

## <a name="optional-send-tagged-notifications"></a>(isteğe bağlı) Etiketli bildirimleri gönderme

Visual Studio'ya erişiminiz yoksa, bir sonraki bölüme atlayabilir ve uygulamanın kendisinden bildirimler gönderebilirsiniz. Ayrıca, bildirim hub'ınız için hata ayıklama sekmesini kullanarak [Azure portalından] uygun şablon bildirimini de gönderebilirsiniz.

[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="optional-send-notifications-from-the-device"></a>(isteğe bağlı) Cihazdan bildirim gönderme

Normalde bildirimler bir arka uç hizmeti tarafından gönderilir, ancak son dakika haberlerini doğrudan uygulamadan gönderebilirsiniz. Bunu yapmak için, `SendNotificationRESTAPI` [Bildirim Hub'ları öğreticisiyle başlatılan yöntemde][get-started] tanımladığınız yöntemi güncelleştirmiş olursunuz.

1. Kategori `ViewController.m`etiketi `SendNotificationRESTAPI` için bir parametre kabul eder ve uygun [şablon](notification-hubs-templates-cross-platform-push-messages.md) bildirimi gönderir, böylece yöntemi aşağıdaki gibi güncelleştirin.

    ```objc
    - (void)SendNotificationRESTAPI:(NSString*)categoryTag
    {
        NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                    defaultSessionConfiguration] delegate:nil delegateQueue:nil];

        NSString *json;

        // Construct the messages REST endpoint
        NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                            HUBNAME, API_VERSION]];

        // Generated the token to be used in the authorization header.
        NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

        //Create the request to add the template notification message to the hub
        NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
        [request setHTTPMethod:@"POST"];

        // Add the category as a tag
        [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];

        // Template notification
        json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\"}",
                categoryTag, self.notificationMessage.text];

        // Signify template notification format
        [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];

        // JSON Content-Type
        [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

        //Authenticate the notification message POST request with the SaS token
        [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

        //Add the notification message body
        [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

        // Send the REST request
        NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                    completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
            NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (error || httpResponse.statusCode != 200)
                {
                    NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                }
                if (data != NULL)
                {
                    //xmlParser = [[NSXMLParser alloc] initWithData:data];
                    //[xmlParser setDelegate:self];
                    //[xmlParser parse];
                }
            }];

        [dataTask resume];
    }
    ```

2. In `ViewController.m`, `Send Notification` aşağıdaki kodda gösterildiği gibi eylemi güncelleştirin. Böylece bildirimleri her etiketi tek tek kullanarak gönderir ve birden çok platforma gönderir.

    ```objc
    - (IBAction)SendNotificationMessage:(id)sender
    {
        self.sendResults.text = @"";

        NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                @"Technology", @"Science", @"Sports", nil];

        // Lets send the message as breaking news for each category to WNS, FCM, and APNS
        // using a template.
        for(NSString* category in categories)
        {
            [self SendNotificationRESTAPI:category];
        }
    }
    ```

3. Projenizi yeniden oluşturun ve oluşturma hatanız olmadığından emin olun.

## <a name="run-the-app-and-generate-notifications"></a>Uygulamayı çalıştırma ve bildirimler oluşturma

1. Projeyi oluşturmak ve uygulamayı başlatmak için Çalıştır düğmesine basın. Abone olmak için bazı son dakika haberleri seçeneklerini seçin ve ardından **Abone Ol** düğmesine basın. Bildirimlerin abone olduğunu belirten bir iletişim kutusu görmeniz gerekir.

    ![iOS'ta örnek bildirim][1]

    **Abone Ol'u**seçtiğinizde, uygulama seçili kategorileri etiketlere dönüştürür ve bildirim merkezinden seçili etiketler için yeni bir cihaz kaydı ister.

2. Son dakika haberi olarak gönderilecek bir ileti girin ve **ardından Bildirim Gönder** düğmesine basın. Alternatif olarak, bildirim oluşturmak için .NET konsol uygulamasını çalıştırın.

    ![iOS'ta bildirim tercihlerini değiştirme][2]

3. Son dakika haberlerine abone olan her cihaz, gönderdiğiniz son dakika haberlerini alır.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, kategorileriçin kaydolmuş belirli iOS aygıtlarına yayın bildirimleri gönderdiniz. Yerelleştirilmiş bildirimleri nasıl iteriz öğrenmek için aşağıdaki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
>[Yerelleştirilmiş anında iletme bildirimleri gönderme](notification-hubs-ios-xplat-localized-apns-push-notification.md)

<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png

<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: https://msdn.microsoft.com/library/jj927168.aspx
[Use Notification Hubs to broadcast localized breaking news]: notification-hubs-ios-xplat-localized-apns-push-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: https://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-To for iOS]: https://msdn.microsoft.com/library/jj927168.aspx
[get-started]: notification-hubs-ios-apple-push-notification-apns-get-started.md
[Azure portalında]: https://portal.azure.com
