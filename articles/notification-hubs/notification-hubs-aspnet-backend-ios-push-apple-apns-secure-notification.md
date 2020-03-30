---
title: Azure Bildirim Hub'ları iOS için Güvenli Push
description: Azure'dan bir iOS uygulamasına güvenli anında iletme bildirimleri nasıl göndereceğinizi öğrenin. Amaç-C ve C# ile yazılmış kod örnekleri.
documentationcenter: ios
author: sethmanheim
manager: femila
editor: jwargo
services: notification-hubs
ms.assetid: 17d42b0a-2c80-4e35-a1ed-ed510d19f4b4
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: 96d1dd514f6fb9c11d7194714337583d6b4387cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75530757"
---
# <a name="azure-notification-hubs-secure-push"></a>Azure Bildirim Hub'ları Güvenli Push

> [!div class="op_single_selector"]
> * [Windows Evrensel](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)

## <a name="overview"></a>Genel Bakış

Microsoft Azure'da anında bildirim desteği, mobil cihazlar için hem tüketici hem de kurumsal uygulamalar için anında iletme bildirimlerinin uygulanmasını büyük ölçüde kolaylaştıran, kullanımı kolay, çok platformlu, ölçeklenmiş bir anında iletme altyapısına erişmenizi sağlar Platform.

Düzenleyici veya güvenlik kısıtlamaları nedeniyle, bazen bir uygulama bildirime standart anında iletme bildirimi altyapısı aracılığıyla aktarılamayan bir şey eklemek isteyebilir. Bu öğretici, istemci aygıtı ve uygulama arka ucu arasında güvenli, kimlik doğrulaması yla bağlantı yoluyla hassas bilgileri göndererek aynı deneyimi nasıl elde edilebiz açıklanır.

Yüksek bir düzeyde, akış aşağıdaki gibidir:

1. Uygulama arka uç:
   * Güvenli yükü arka uç veritabanında saklar.
   * Bu bildirimin kimliğini aygıta gönderir (güvenli bilgi gönderilmez).
2. Bildirim alırken cihazdaki uygulama:
   * Aygıt, güvenli yükü isteyen arka uçla bağlantı kurur.
   * Uygulama, yükü cihazda bir bildirim olarak gösterebilir.

Önceki akışta (ve bu öğreticide) aygıtın, kullanıcı oturum açtıktan sonra yerel depolama alanında bir kimlik doğrulama belirteci depoladığını varsaymak önemlidir. Bu, aygıt bu belirteci kullanarak bildirimin güvenli yükünü alabildiği için sorunsuz bir deneyim garanti eder. Uygulamanız cihazda kimlik doğrulama belirteçleri depolamazveya bu belirteçlerin süresi dolmuşsa, bildirimin alınmasından sonra cihaz uygulaması kullanıcıdan uygulamayı başlatmasını isteyen genel bir bildirim görüntülemelidir. Uygulama daha sonra kullanıcının kimliğini doğrular ve bildirim yükünü gösterir.

Bu Güvenli Push öğreticisi, anında iletme bildiriminin güvenli bir şekilde nasıl gönderilebildiğini gösterir. Öğretici, [Kullanıcıları Bildir](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) öğreticisine göre inşa eder, bu nedenle önce bu öğreticideki adımları tamamlamanız gerekir.

> [!NOTE]
> Bu öğretici, bildirim hub'ınızı [Bildirim Hub'larıyla (iOS) Başlat'ta](notification-hubs-ios-apple-push-notification-apns-get-started.md)açıklandığı şekilde oluşturduğunuzu ve yapılandırdığınızı varsayar.

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-ios-project"></a>iOS projesini değiştirme

Uygulamanızın arka ucunu yalnızca bir bildirimin *kimliğini* göndermek için değiştirdiğinize göre, bu bildirimi işlemek için iOS uygulamanızı değiştirmeniz ve görüntülenecek güvenli iletiyi almak için arka uçunuzu geri aramanız gerekir.

Bu amaca ulaşmak için, güvenli içeriği uygulamanın arka ucundan almak için mantığı yazmamız gerekir.

1. ' `AppDelegate.m`de, uygulamanın sessiz bildirimler için kaydolduğundan emin olun, böylece arka uçtan gönderilen bildirim kimliğini işler. didFinishLaunchingWithOptions seçeneğini `UIRemoteNotificationTypeNewsstandContentAvailability` ekleyin:

    ```objc
    [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
    ```
2. En `AppDelegate.m` üstteki uygulama bölümünüze aşağıdaki bildirimle eklemenizde:

    ```objc
    @interface AppDelegate ()
    - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
    @end
    ```
3. Daha sonra uygulama bölümüne aşağıdaki kodu ekleyin ve `{back-end endpoint}` yer tutucuyu daha önce elde edilen arka uç için bitiş noktasıyla ikame edin:

    ```objc
    NSString *const GetNotificationEndpoint = @"{back-end endpoint}/api/notifications";

    - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
    {
        // check if authenticated
        ANHViewController* rvc = (ANHViewController*) self.window.rootViewController;
        NSString* authenticationHeader = rvc.registerClient.authenticationHeader;
        if (!authenticationHeader) return;

        NSURLSession* session = [NSURLSession
                                    sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                    delegate:nil
                                    delegateQueue:nil];

        NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, payloadId]];
        NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
        [request setHTTPMethod:@"GET"];
        NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
        [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

        NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
            NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
            if (!error && httpResponse.statusCode == 200)
            {
                NSLog(@"Received secure payload: %@", [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding]);

                NSMutableDictionary *json = [NSJSONSerialization JSONObjectWithData:data options: NSJSONReadingMutableContainers error: &error];

                completion([json objectForKey:@"Payload"], nil);
            }
            else
            {
                NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                if (error)
                    completion(nil, error);
                else {
                    completion(nil, [NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                }
            }
        }];
        [dataTask resume];
    }
    ```

    Bu yöntem, paylaşılan tercihlerde depolanan kimlik bilgilerini kullanarak bildirim içeriğini almak için uygulamanızı geri uç olarak çağırır.

4. Şimdi gelen bildirimi işlemek ve görüntülemek için içerik almak için yukarıdaki yöntemi kullanmak zorunda. İlk olarak, bir anında iletme bildirimi alırken iOS uygulamanızın arka planda çalışmasını sağlamamız gerekir. **XCode'da,** sol panelde uygulama projenizi seçin ve ardından merkezi bölmeden **Hedefler** bölümündeki ana uygulama hedefinizi tıklatın.
5. Ardından merkezi bölmenizin üst kısmındaki **Yetenekler** sekmesini tıklatın ve **Uzak Bildirimler** onay kutusunu işaretleyin.

    ![][IOS1]

6. Anında `AppDelegate.m` iletme bildirimlerini işlemek için aşağıdaki yöntemi ekleyin:

    ```objc
    -(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
    {
        NSLog(@"%@", userInfo);

        [self retrieveSecurePayloadWithId:[[userInfo objectForKey:@"secureId"] intValue] completion:^(NSString * payload, NSError *error) {
            if (!error) {
                // show local notification
                UILocalNotification* localNotification = [[UILocalNotification alloc] init];
                localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:0];
                localNotification.alertBody = payload;
                localNotification.timeZone = [NSTimeZone defaultTimeZone];
                [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                completionHandler(UIBackgroundFetchResultNewData);
            } else {
                completionHandler(UIBackgroundFetchResultFailed);
            }
        }];

    }
    ```

    Eksik kimlik doğrulama üstbilgi özelliği veya geri uç tarafından reddedilme durumlarını işlemek için tercih edilir olduğunu unutmayın. Bu servis taleplerinin özel olarak işlenmesi büyük oranda hedef kullanıcı deneyiminize bağlıdır. Seçeneklerden biri, kullanıcının gerçek bildirimi almak için kimlik doğrulaması için genel bir komut istemiyle bir bildirim görüntülemektir.

## <a name="run-the-application"></a>Uygulamayı Çalıştır

Uygulamayı çalıştırmak için aşağıdakileri yapın:

1. XCode'da uygulamayı fiziksel bir iOS aygıtında çalıştırın (anında iletme bildirimleri simülatörde çalışmaz).
2. iOS uygulaması Kullanıcı Arabirimi'ne bir kullanıcı adı ve parola girin. Bunlar herhangi bir dize olabilir, ancak aynı değerolmalıdır.
3. iOS uygulaması UI'de **Oturum Aç'ı**tıklatın. Ardından **Gönder tuşuna**bas. Bildirim merkezinizde görüntülenen güvenli bildirimi görmeniz gerekir.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-secure-push/secure-push-ios-1.png
