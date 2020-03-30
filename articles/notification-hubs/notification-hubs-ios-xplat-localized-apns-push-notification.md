---
title: Azure Bildirim Hub'larını kullanarak iOS'a yerelleştirilmiş anında iletme bildirimleri gönderme | Microsoft Dokümanlar
description: Azure Bildirim Hub'larını kullanarak iOS aygıtlarına yerelleştirilmiş bildirimleri nasıl kullanacağınızı öğrenin.
services: notification-hubs
documentationcenter: ios
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: 484914b5-e081-4a05-a84a-798bbd89d428
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: a8614156be5d516d16aff698b604cf0e661d7311
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72385654"
---
# <a name="tutorial-send-localized-push-notifications-to-ios-using-azure-notification-hubs"></a>Öğretici: Azure Bildirim Hub'larını kullanarak iOS'a yerelleştirilmiş anında iletme bildirimleri gönderme

> [!div class="op_single_selector"]
> * [Windows Mağazası C #](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)

Bu öğretici, dil ve aygıt tarafından yerelleştirilmiş son dakika haber bildirimlerini yayınlamak için Azure Bildirim Hub'larının [şablonlar](notification-hubs-templates-cross-platform-push-messages.md) özelliğini nasıl kullanacağınızı gösterir. Bu eğitimde, [son dakika haberleri göndermek için Bildirim Hub'larını kullan'da]oluşturulan iOS uygulamasıyla başlarsınız. Tamamlandığında, ilgilendiğiniz kategorilere kaydolabilir, bildirimleri alacağınız bir dil belirtebilir ve yalnızca o dilde seçili kategoriler için anında iletme bildirimleri alabilirsiniz.

Bu senaryonun iki bölümü vardır:

* iOS uygulaması istemci aygıtların bir dil belirtmesine ve farklı son dakika haber kategorilerine abone olmasını sağlar;
* Arka uç, Azure Bildirim Hub'larının **etiket** ve **şablon** özelliklerini kullanarak bildirimleri yayınlar.

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Uygulama kullanıcı arabirimini güncelleştirin
> * iOS uygulamasını oluşturun
> * .NET konsol uygulamasından yerelleştirilmiş şablon bildirimleri gönderme
> * Aygıttan yerelleştirilmiş şablon bildirimleri gönderme

## <a name="overview"></a>Genel Bakış

[Son dakika haberlerini göndermek için Bildirim Hub'larını]kullan'da, farklı haber kategorileri için bildirimlere abone olmak için **etiketleri** kullanan bir uygulama oluşturabilirsiniz. Ancak birçok uygulama birden çok pazarı hedefler ve yerelleştirme gerektirir. Bu, bildirimlerin içeriğinin yerelleştirilmesi ve doğru aygıt kümesine teslim edilmesi gerektiği anlamına gelir. Bu öğretici, yerelleştirilmiş son dakika haberleri bildirimlerini kolayca sunmak için Bildirim Hub'larının **şablon** özelliğini nasıl kullanacağınızı gösterir.

> [!NOTE]
> Yerelleştirilmiş bildirimler göndermenin bir yolu, her etiketin birden çok sürümü oluşturmaktır. Örneğin, İngilizce, Fransızca ve Mandarin'i desteklemek için dünya haberleri için üç farklı etikete ihtiyacınız vardır: "world_en", "world_fr" ve "world_ch". Daha sonra bu etiketlerin her birine dünya haberlerinin yerelleştirilmiş bir sürümünü göndermeniz gerekir. Bu konuda, etiketlerin çoğalmasını ve birden çok ileti gönderme gereksinimini önlemek için şablonlar kullanırsınız.

Şablonlar, belirli bir aygıtın bildirimi nasıl alması gerektiğini belirtmenin bir yoludur. Şablon, uygulama arka ucunuz tarafından gönderilen iletinin parçası olan özelliklere başvurarak tam yük biçimini belirtir. Sizin durumunuzda, desteklenen tüm dilleri içeren yerel bir ileti gönderirsiniz:

```json
{
    "News_English": "...",
    "News_French": "...",
    "News_Mandarin": "..."
}
```

Ardından, aygıtların doğru özelliği ifade eden bir şablonla kaydolduğundan emin olabilirsiniz. Örneğin, aşağıdaki sözdizimini kullanarak Fransızca haber kayıtlarına kaydolmak isteyen bir iOS uygulaması:

```json
{
    aps: {
        alert: "$(News_French)"
    }
}
```

Şablonlar hakkında daha fazla bilgi için [Şablonlar](notification-hubs-templates-cross-platform-push-messages.md) makalesine bakın.

## <a name="prerequisites"></a>Ön koşullar

* Belirli [iOS aygıtları öğreticisine anında iletme bildirimlerini](notification-hubs-ios-xplat-segmented-apns-push-notification.md) tamamlayın ve bu öğretici doğrudan bu kod üzerine inşa olduğundan kodu niçin kullanılabilir hale getirmek.
* Visual Studio 2019 isteğe bağlıdır.

## <a name="update-the-app-user-interface"></a>Uygulama kullanıcı arabirimini güncelleştirin

Bu bölümde, şablonları kullanarak yerelleştirilmiş son dakika haberleri [göndermek için son dakika haberleri göndermek için bildirim hub'larını kullan] konusunda oluşturduğunuz Son Dakika Haberleri uygulamasını değiştirirsiniz.

İngilizce, `MainStoryboard_iPhone.storyboard`Fransızca ve Mandarin: sizin , üç dilde segmentli kontrol ekleyin.

![iOS UI storyboard oluşturma][13]

Ardından ViewController.h'nizde aşağıdaki resimde gösterildiği gibi bir IBOutlet eklediğinizden emin olun:

![Anahtarlar için çıkışlar oluşturma][14]

## <a name="build-the-ios-app"></a>iOS uygulamasını oluşturun

1. Sizin `Notification.h`, `retrieveLocale` yöntemi ekleyin ve aşağıdaki kodda gösterildiği gibi mağaza ve abone yöntemlerini değiştirin:

    ```objc
    - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;

    - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;

    - (NSSet*) retrieveCategories;

    - (int) retrieveLocale;
    ```
    Sizin `Notification.m`, `storeCategoriesAndSubscribe` `locale` parametre ekleyerek ve kullanıcı varsayılanları depolayarak, yöntemi değiştirin:

    ```objc
    - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
        NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
        [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
        [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
        [defaults synchronize];

        [self subscribeWithLocale: locale categories:categories completion:completion];
    }
    ```

    Ardından, *yerel* bilgileri içerecek şekilde abone ol yöntemini değiştirin:

    ```objc
    - (void) subscribeWithLocale: (int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion{
        SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:@"<connection string>" notificationHubPath:@"<hub name>"];

        NSString* localeString;
        switch (locale) {
            case 0:
                localeString = @"English";
                break;
            case 1:
                localeString = @"French";
                break;
            case 2:
                localeString = @"Mandarin";
                break;
        }

        NSString* template = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"$(News_%@)\"},\"inAppMessage\":\"$(News_%@)\"}", localeString, localeString];

        [hub registerTemplateWithDeviceToken:self.deviceToken name:@"localizednewsTemplate" jsonBodyTemplate:template expiryTemplate:@"0" tags:categories completion:completion];
    }
    ```

    Yöntemi , `registerTemplateWithDeviceToken`yerine `registerNativeWithDeviceToken`. Bir şabloniçin kaydolduğunuzda, json şablonu ve şablon için bir ad sağlamanız gerekir (uygulama farklı şablonları kaydetmek isteyebilirsiniz). Bu haberlerin bildirimlerini aldığınızdan emin olmak istediğinizden, kategorilerinizi etiket olarak kaydettiğinizden emin olun.

    Kullanıcı varsayılan ayarlarından yerel durumu almak için bir yöntem ekleyin:

    ```objc
    - (int) retrieveLocale {
        NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

        int locale = [defaults integerForKey:@"BreakingNewsLocale"];

        return locale < 0?0:locale;
    }
    ```

2. Şimdi `Notifications` sınıf modifiye, bu yeni `ViewController` `UISegmentControl`kullanır emin olmak zorunda . Şu anda seçili olan yerel agünü gösterdiğinden emin olmak için `viewDidLoad` yönteme aşağıdaki satırı ekleyin:

    ```objc
    self.Locale.selectedSegmentIndex = [notifications retrieveLocale];
    ```

    Ardından, yönteminizde, `subscribe` aramanızı aşağıdaki `storeCategoriesAndSubscribe` koda değiştirin:

    ```objc
    [notifications storeCategoriesAndSubscribeWithLocale: self.Locale.selectedSegmentIndex categories:[NSSet setWithArray:categories] completion: ^(NSError* error) {
        if (!error) {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                    @"Subscribed!" delegate:nil cancelButtonTitle:
                                    @"OK" otherButtonTitles:nil, nil];
            [alert show];
        } else {
            NSLog(@"Error subscribing: %@", error);
        }
    }];
    ```

3. Son olarak, uygulamanız `didRegisterForRemoteNotificationsWithDeviceToken` başladığında kaydınızı doğru bir şekilde yenileyebilmeniz için AppDelegate.m'nizdeki yöntemi güncelleştirmeniz gerekir. Aramanızı aşağıdaki `subscribe` kodla bildirim yöntemiyle değiştirin:

    ```obj-c
    NSSet* categories = [self.notifications retrieveCategories];
    int locale = [self.notifications retrieveLocale];
    [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
        if (error != nil) {
            NSLog(@"Error registering for notifications: %@", error);
        }
    }];
    ```

## <a name="optional-send-localized-template-notifications-from-net-console-app"></a>(isteğe bağlı) .NET konsol uygulamasından yerelleştirilmiş şablon bildirimleri gönderme

[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

## <a name="optional-send-localized-template-notifications-from-the-device"></a>(isteğe bağlı) Aygıttan yerelleştirilmiş şablon bildirimleri gönderme

Visual Studio'ya erişiminiz yoksa veya yerelleştirilmiş şablon bildirimlerini doğrudan cihazdaki uygulamadan göndermeyi test etmek istiyorsanız. Yerelleştirilmiş şablon parametrelerini önceki `SendNotificationRESTAPI` öğreticide tanımladığınız yönteme ekleyebilirsiniz.

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
    json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\","
            \"News_English\":\"Breaking %@ News in English : %@\","
            \"News_French\":\"Breaking %@ News in French : %@\","
            \"News_Mandarin\":\"Breaking %@ News in Mandarin : %@\","
            categoryTag, self.notificationMessage.text,
            categoryTag, self.notificationMessage.text,  // insert English localized news here
            categoryTag, self.notificationMessage.text,  // insert French localized news here
            categoryTag, self.notificationMessage.text]; // insert Mandarin localized news here

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

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, iOS aygıtlarına yerelleştirilmiş bildirimler gönderdiniz. iOS uygulamalarının belirli kullanıcılarına bildirim nasıl iletilir öğrenmek için aşağıdaki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
>[Belirli kullanıcılara anında iletme bildirimleri gönderme](notification-hubs-aspnet-backend-ios-apple-apns-notification.md)

<!-- Images. -->
[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png

<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: https://msdn.microsoft.com/library/jj927168.aspx
[Son dakika haberleri göndermek için Notification Hubs’ı kullanma]: notification-hubs-ios-xplat-segmented-apns-push-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Notify users with Notification Hubs: Mobile Services]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Submit an app page]: https://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: https://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-users-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript and HTML]: ../get-started-with-push-js.md
[Windows Developer Preview registration steps for Mobile Services]: ../mobile-services-windows-developer-preview-registration.md
[wns object]: https://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: https://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: https://msdn.microsoft.com/library/jj927168.aspx
