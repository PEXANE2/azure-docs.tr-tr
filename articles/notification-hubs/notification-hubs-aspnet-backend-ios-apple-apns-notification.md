---
title: Azure Bildirim Hub'larını kullanarak belirli kullanıcılara anında iletme bildirimleri gönderme | Microsoft Dokümanlar
description: Azure Notification Hubs kullanarak belirli kullanıcılara anında iletme bildirimleri göndermeyi öğrenin.
documentationcenter: ios
author: sethm
manager: femila
editor: jwargo
services: notification-hubs
ms.assetid: 1f7d1410-ef93-4c4b-813b-f075eed20082
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: 48135ea614bbab4ca6649a83895ae5f632918c61
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72387467"
---
# <a name="tutorial-send-push-notifications-to-specific-users-using-azure-notification-hubs"></a>Öğretici: Azure Bildirim Hub'larını kullanarak belirli kullanıcılara anında iletme bildirimleri gönderme

[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

Bu öğreticide, belirli bir cihazdaki belirli bir uygulama kullanıcısına anında iletme bildirimleri göndermek için Azure Bildirim Hubs’ı nasıl kullanacağınız gösterilmektedir. Bir ASP.NET WebAPI arka uç istemcileri doğrulamak ve bildirimler oluşturmak için kullanılır, uygulama [arka uçtan kayıt](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend)rehberlik konusu gösterildiği gibi.

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * WebAPI projesi oluşturma
> * WebAPI arka ucunda istemcilerin kimliğini doğrulama
> * WebAPI arka ucunu kullanarak bildirimlere kaydolma
> * WebAPI arka ucundan bildirim gönderme
> * Yeni WebAPI arka ucunu yayımlama
> * iOS uygulamanızı değiştirme
> * Uygulamayı test etme

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici, bildirim hub'ınızı [Bildirim Hub'larıyla (iOS) Başlat'ta](notification-hubs-ios-apple-push-notification-apns-get-started.md)açıklandığı şekilde oluşturduğunuzu ve yapılandırdığınızı varsayar. Bu öğretici aynı zamanda [Secure Push (iOS)](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md) öğreticisinin ön koşuludur.
Mobil Uygulamaları arka uç hizmetiniz olarak kullanmak istiyorsanız, [Mobil Uygulamalar Push ile Başlayın'a](../app-service-mobile/app-service-mobile-ios-get-started-push.md)bakın.

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="modify-your-ios-app"></a>iOS uygulamanızı değiştirme

1. [Bildirim Hub'ları (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) ile Başlarken öğreticisinde oluşturduğunuz Tek Sayfa görünümü uygulamasını açın.

   > [!NOTE]
   > Bu bölüm, projenizin boş bir kuruluş adı ile yapılandırıldığı varsayar. Değilse, kuruluş adınızı tüm sınıf adlarına hazırlamanız gerekir.

2. Dosyada, `Main.storyboard` nesne kitaplığından ekran görüntüsünde gösterilen bileşenleri ekleyin.

    ![Xcode arayüz oluşturucusunda hikaye şeridini edin][1]

   * **Kullanıcı Adı**: Yer tutucu metni içeren bir UITextField, *Kullanıcı Adı Girin,* gönder sonuçları etiketinin hemen altında ve sol ve sağ kenar boşluklarına ve send sonuçları etiketinin altına sınırlandırılmıştır.
   * **Şifre**: Yer tutucu metiniçeren bir UITextField, Kullanıcı adı metin alanının hemen altında ve sol ve sağ kenar boşluklarıve kullanıcı adı metin alanının altında şifre *girin.* *İade Anahtarı*altında, Atfuz Denetçisi'ndeki **Güvenli Metin Girişi** seçeneğini işaretleyin.
   * **Giriş Yapın**: Parola metin alanının hemen altında etiketlenmiş ve *Denetim-İçerik* altında Öznitelikler Denetçisi'ndeki **Etkin** seçeneğin işaretlerini kaldırın
   * **WNS**: Hub'da ayarlanmışsa bildirim Windows Bildirim Hizmeti'nin gönderilmesini etkinleştirmek için etiketleyin ve değiştirin. Windows [Başlarken](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) öğreticibakın.
   * **GCM**: Hub'da ayarlanmışsa, bildirimin Google Cloud Messaging'e gönderilmesini etkinleştirmek için etiketleyin ve değiştirin. Bkz. [Android Başlangıç](notification-hubs-android-push-notification-google-gcm-get-started.md) eğitimi.
   * **APNS**: Bildirimin Apple Platformu Bildirim Hizmeti'ne gönderilmesini etkinleştirmek için etiketleyin ve değiştirin.
   * **Alıcı Kullanıcı Adı:Yer** tutucu metinli bir UITextField, *Alıcı kullanıcı adı etiketi,* GCM etiketinin hemen altında ve sol ve sağ kenar boşluklarında ve GCM etiketinin altında sınırlandırılmıştır.

     [Bildirim Hub'ları (iOS) öğreticisinde](notification-hubs-ios-apple-push-notification-apns-get-started.md) bazı bileşenler eklendi.

3. **Ctrl** görünümdeki bileşenlerden sürükleyin `ViewController.h` ve bu yeni çıkışları ekleyin.

    ```objc
    @property (weak, nonatomic) IBOutlet UITextField *UsernameField;
    @property (weak, nonatomic) IBOutlet UITextField *PasswordField;
    @property (weak, nonatomic) IBOutlet UITextField *RecipientField;
    @property (weak, nonatomic) IBOutlet UITextField *NotificationField;

    // Used to enable the buttons on the UI
    @property (weak, nonatomic) IBOutlet UIButton *LogInButton;
    @property (weak, nonatomic) IBOutlet UIButton *SendNotificationButton;

    // Used to enabled sending notifications across platforms
    @property (weak, nonatomic) IBOutlet UISwitch *WNSSwitch;
    @property (weak, nonatomic) IBOutlet UISwitch *GCMSwitch;
    @property (weak, nonatomic) IBOutlet UISwitch *APNSSwitch;

    - (IBAction)LogInAction:(id)sender;
    ```

4. In `ViewController.h`, alma `#define` ekstrelerinizden sonra aşağıdakileri ekleyin. Yer `<Enter Your Backend Endpoint>` tutucuyu, uygulama arka uçunuzu önceki bölümde dağıtmak için kullandığınız Hedef URL ile değiştirin. Örneğin, `http://your_backend.azurewebsites.net`.

    ```objc
    #define BACKEND_ENDPOINT @"<Enter Your Backend Endpoint>"
    ```

5. Projenizde, oluşturduğunuz ASP.NET arka uçla arabirim için yeni `RegisterClient` bir Cocoa Touch sınıfı oluşturun. 'den `NSObject`devralınan sınıf oluşturun. Sonra aşağıdaki kodu `RegisterClient.h`ekleyin.

    ```objc
    @interface RegisterClient : NSObject

    @property (strong, nonatomic) NSString* authenticationHeader;

    -(void) registerWithDeviceToken:(NSData*)token tags:(NSSet*)tags
        andCompletion:(void(^)(NSError*))completion;

    -(instancetype) initWithEndpoint:(NSString*)Endpoint;

    @end
    ```

6. `RegisterClient.m`Bölümünde, güncelleme: `@interface`

    ```objc
    @interface RegisterClient ()

    @property (strong, nonatomic) NSURLSession* session;
    @property (strong, nonatomic) NSURLSession* endpoint;

    -(void) tryToRegisterWithDeviceToken:(NSData*)token tags:(NSSet*)tags retry:(BOOL)retry
                andCompletion:(void(^)(NSError*))completion;
    -(void) retrieveOrRequestRegistrationIdWithDeviceToken:(NSString*)token
                completion:(void(^)(NSString*, NSError*))completion;
    -(void) upsertRegistrationWithRegistrationId:(NSString*)registrationId deviceToken:(NSString*)token
                tags:(NSSet*)tags andCompletion:(void(^)(NSURLResponse*, NSError*))completion;

    @end
    ```

7. RegisterClient.m'deki `@implementation` bölümü aşağıdaki kodla değiştirin:

    ```objc
    @implementation RegisterClient

    // Globals used by RegisterClient
    NSString *const RegistrationIdLocalStorageKey = @"RegistrationId";

    -(instancetype) initWithEndpoint:(NSString*)Endpoint
    {
        self = [super init];
        if (self) {
            NSURLSessionConfiguration* config = [NSURLSessionConfiguration defaultSessionConfiguration];
            _session = [NSURLSession sessionWithConfiguration:config delegate:nil delegateQueue:nil];
            _endpoint = Endpoint;
        }
        return self;
    }

    -(void) registerWithDeviceToken:(NSData*)token tags:(NSSet*)tags
                andCompletion:(void(^)(NSError*))completion
    {
        [self tryToRegisterWithDeviceToken:token tags:tags retry:YES andCompletion:completion];
    }

    -(void) tryToRegisterWithDeviceToken:(NSData*)token tags:(NSSet*)tags retry:(BOOL)retry
                andCompletion:(void(^)(NSError*))completion
    {
        NSSet* tagsSet = tags?tags:[[NSSet alloc] init];

        NSString *deviceTokenString = [[token description]
            stringByTrimmingCharactersInSet:[NSCharacterSet characterSetWithCharactersInString:@"<>"]];
        deviceTokenString = [[deviceTokenString stringByReplacingOccurrencesOfString:@" " withString:@""]
                                uppercaseString];

        [self retrieveOrRequestRegistrationIdWithDeviceToken: deviceTokenString
            completion:^(NSString* registrationId, NSError *error) {
            NSLog(@"regId: %@", registrationId);
            if (error) {
                completion(error);
                return;
            }

            [self upsertRegistrationWithRegistrationId:registrationId deviceToken:deviceTokenString
                tags:tagsSet andCompletion:^(NSURLResponse * response, NSError *error) {
                if (error) {
                    completion(error);
                    return;
                }

                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
                if (httpResponse.statusCode == 200) {
                    completion(nil);
                } else if (httpResponse.statusCode == 410 && retry) {
                    [self tryToRegisterWithDeviceToken:token tags:tags retry:NO andCompletion:completion];
                } else {
                    NSLog(@"Registration error with response status: %ld", (long)httpResponse.statusCode);

                    completion([NSError errorWithDomain:@"Registration" code:httpResponse.statusCode
                                userInfo:nil]);
                }

            }];
        }];
    }

    -(void) upsertRegistrationWithRegistrationId:(NSString*)registrationId deviceToken:(NSData*)token
                tags:(NSSet*)tags andCompletion:(void(^)(NSURLResponse*, NSError*))completion
    {
        NSDictionary* deviceRegistration = @{@"Platform" : @"apns", @"Handle": token,
                                                @"Tags": [tags allObjects]};
        NSData* jsonData = [NSJSONSerialization dataWithJSONObject:deviceRegistration
                            options:NSJSONWritingPrettyPrinted error:nil];

        NSLog(@"JSON registration: %@", [[NSString alloc] initWithData:jsonData
                                            encoding:NSUTF8StringEncoding]);

        NSString* endpoint = [NSString stringWithFormat:@"%@/api/register/%@", _endpoint,
                                registrationId];
        NSURL* requestURL = [NSURL URLWithString:endpoint];
        NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
        [request setHTTPMethod:@"PUT"];
        [request setHTTPBody:jsonData];
        NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",
                                                self.authenticationHeader];
        [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];
        [request setValue:@"application/json" forHTTPHeaderField:@"Content-Type"];

        NSURLSessionDataTask* dataTask = [self.session dataTaskWithRequest:request
            completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
        {
            if (!error)
            {
                completion(response, error);
            }
            else
            {
                NSLog(@"Error request: %@", error);
                completion(nil, error);
            }
        }];
        [dataTask resume];
    }

    -(void) retrieveOrRequestRegistrationIdWithDeviceToken:(NSString*)token
                completion:(void(^)(NSString*, NSError*))completion
    {
        NSString* registrationId = [[NSUserDefaults standardUserDefaults]
                                    objectForKey:RegistrationIdLocalStorageKey];

        if (registrationId)
        {
            completion(registrationId, nil);
            return;
        }

        // request new one & save
        NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/api/register?handle=%@",
                                _endpoint, token]];
        NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
        [request setHTTPMethod:@"POST"];
        NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",
                                                self.authenticationHeader];
        [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

        NSURLSessionDataTask* dataTask = [self.session dataTaskWithRequest:request
            completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
        {
            NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
            if (!error && httpResponse.statusCode == 200)
            {
                NSString* registrationId = [[NSString alloc] initWithData:data
                    encoding:NSUTF8StringEncoding];

                // remove quotes
                registrationId = [registrationId substringWithRange:NSMakeRange(1,
                                    [registrationId length]-2)];

                [[NSUserDefaults standardUserDefaults] setObject:registrationId
                    forKey:RegistrationIdLocalStorageKey];
                [[NSUserDefaults standardUserDefaults] synchronize];

                completion(registrationId, nil);
            }
            else
            {
                NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                if (error)
                    completion(nil, error);
                else {
                    completion(nil, [NSError errorWithDomain:@"Registration" code:httpResponse.statusCode
                                userInfo:nil]);
                }
            }
        }];
        [dataTask resume];
    }

    @end
    ```

    Bu kod, uygulama arka ucunuza REST çağrıları gerçekleştirmek için NSURLSession'ı kullanarak [uygulama arka uçunuzdan kaydolan](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend) kılavuz makalesinde açıklanan mantığı uygular ve NSUserDefaults bildirim merkezi tarafından döndürülen registrationId'i yerel olarak depolamak için kullanır.

    Bu sınıf düzgün `authorizationHeader` çalışması için özelliğinin ayarlanmasını gerektirir. Bu özellik, oturum `ViewController` açmadan sonra sınıf tarafından ayarlanır.

8. In `ViewController.h`, `#import` için `RegisterClient.h`bir deyim ekleyin. Ardından aygıt belirteci için bir bildirim `RegisterClient` ve `@interface` bölümdeki bir örne başvuru ekleyin:

    ```objc
    #import "RegisterClient.h"

    @property (strong, nonatomic) NSData* deviceToken;
    @property (strong, nonatomic) RegisterClient* registerClient;
    ```

9. ViewController.m'de, `@interface` bölüme özel bir yöntem bildirimi ekleyin:

    ```objc
    @interface ViewController () <UITextFieldDelegate, NSURLConnectionDataDelegate, NSXMLParserDelegate>

    // create the Authorization header to perform Basic authentication with your app back-end
    -(void) createAndSetAuthenticationHeaderWithUsername:(NSString*)username
                    AndPassword:(NSString*)password;

    @end
    ```

    > [!NOTE]
    > Aşağıdaki parçacık güvenli bir kimlik doğrulama düzeni değildir, uygulamayerine, `createAndSetAuthenticationHeaderWithUsername:AndPassword:` kayıt istemcisınıfı (Örneğin OAuth, Active Directory) tarafından tüketilecek bir kimlik doğrulama belirteci oluşturan özel kimlik doğrulama mekanizmasının uygulanmasını değiştirmelisiniz.

10. Daha sonra `@implementation` `ViewController.m`, aygıt belirteci ve kimlik doğrulama üstbilgisini ayarlamak için uygulama ekler aşağıdaki kodu ekleyin bölümünde.

    ```objc
    -(void) setDeviceToken: (NSData*) deviceToken
    {
        _deviceToken = deviceToken;
        self.LogInButton.enabled = YES;
    }

    -(void) createAndSetAuthenticationHeaderWithUsername:(NSString*)username
                    AndPassword:(NSString*)password;
    {
        NSString* headerValue = [NSString stringWithFormat:@"%@:%@", username, password];

        NSData* encodedData = [[headerValue dataUsingEncoding:NSUTF8StringEncoding] base64EncodedDataWithOptions:NSDataBase64EncodingEndLineWithCarriageReturn];

        self.registerClient.authenticationHeader = [[NSString alloc] initWithData:encodedData
                                                    encoding:NSUTF8StringEncoding];
    }

    -(BOOL)textFieldShouldReturn:(UITextField *)textField
    {
        [textField resignFirstResponder];
        return YES;
    }
    ```

    Aygıt belirteci ayarlamasının oturum açma düğmesini nasıl etkinleştirimelerine dikkat edin. Çünkü giriş eyleminin bir parçası olarak, görünüm denetleyicisi uygulama arka ucuyla anında iletme bildirimleri için kaydolur. Bu nedenle, giriş eyleminin aygıt belirteci düzgün bir şekilde ayarlanana kadar erişilebilir olmasını istemeyin. Giriş, ikincisinden önce gerçekleştiği sürece giriş kaydını itme kaydından ayırabilirsiniz.

11. ViewController.m'de, **Giriş Yap** düğmeniz için eylem yöntemini ve ASP.NET arka ucunu kullanarak bildirim iletisini göndermek için bir yöntem uygulamak için aşağıdaki parçacıkları kullanın.

    ```objc
    - (IBAction)LogInAction:(id)sender {
        // create authentication header and set it in register client
        NSString* username = self.UsernameField.text;
        NSString* password = self.PasswordField.text;

        [self createAndSetAuthenticationHeaderWithUsername:username AndPassword:password];

        __weak ViewController* selfie = self;
        [self.registerClient registerWithDeviceToken:self.deviceToken tags:nil
            andCompletion:^(NSError* error) {
            if (!error) {
                dispatch_async(dispatch_get_main_queue(),
                ^{
                    selfie.SendNotificationButton.enabled = YES;
                    [self MessageBox:@"Success" message:@"Registered successfully!"];
                });
            }
        }];
    }

    - (void)SendNotificationASPNETBackend:(NSString*)pns UsernameTag:(NSString*)usernameTag
                Message:(NSString*)message
    {
        NSURLSession* session = [NSURLSession
            sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration] delegate:nil
            delegateQueue:nil];

        // Pass the pns and username tag as parameters with the REST URL to the ASP.NET backend
        NSURL* requestURL = [NSURL URLWithString:[NSString
            stringWithFormat:@"%@/api/notifications?pns=%@&to_tag=%@", BACKEND_ENDPOINT, pns,
            usernameTag]];

        NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
        [request setHTTPMethod:@"POST"];

        // Get the mock authenticationheader from the register client
        NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",
            self.registerClient.authenticationHeader];
        [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

        //Add the notification message body
        [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];
        [request setHTTPBody:[message dataUsingEncoding:NSUTF8StringEncoding]];

        // Execute the send notification REST API on the ASP.NET Backend
        NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
            completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
        {
            NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
            if (error || httpResponse.statusCode != 200)
            {
                NSString* status = [NSString stringWithFormat:@"Error Status for %@: %d\nError: %@\n",
                                    pns, httpResponse.statusCode, error];
                dispatch_async(dispatch_get_main_queue(),
                ^{
                    // Append text because all 3 PNS calls may also have information to view
                    [self.sendResults setText:[self.sendResults.text stringByAppendingString:status]];
                });
                NSLog(status);
            }

            if (data != NULL)
            {
                xmlParser = [[NSXMLParser alloc] initWithData:data];
                [xmlParser setDelegate:self];
                [xmlParser parse];
            }
        }];
        [dataTask resume];
    }
    ```

12. ASP.NET arka ucunu kullanmak ve bir anahtar tarafından etkinleştirilen herhangi bir PNS'ye göndermek için **Bildirim Gönder** düğmesinin eylemini güncelleştirin.

    ```objc
    - (IBAction)SendNotificationMessage:(id)sender
    {
        //[self SendNotificationRESTAPI];
        [self SendToEnabledPlatforms];
    }

    -(void)SendToEnabledPlatforms
    {
        NSString* json = [NSString stringWithFormat:@"\"%@\"",self.notificationMessage.text];

        [self.sendResults setText:@""];

        if ([self.WNSSwitch isOn])
            [self SendNotificationASPNETBackend:@"wns" UsernameTag:self.RecipientField.text Message:json];

        if ([self.GCMSwitch isOn])
            [self SendNotificationASPNETBackend:@"gcm" UsernameTag:self.RecipientField.text Message:json];

        if ([self.APNSSwitch isOn])
            [self SendNotificationASPNETBackend:@"apns" UsernameTag:self.RecipientField.text Message:json];
    }
    ```

13. İşlevde, `ViewDidLoad` `RegisterClient` örneği anında anlatmak ve metin alanlarınız için temsilciyi ayarlamak için aşağıdakileri ekleyin.

    ```objc
    self.UsernameField.delegate = self;
    self.PasswordField.delegate = self;
    self.RecipientField.delegate = self;
    self.registerClient = [[RegisterClient alloc] initWithEndpoint:BACKEND_ENDPOINT];
    ```

14. Şimdi, `AppDelegate.m`yöntemin `application:didRegisterForPushNotificationWithDeviceToken:` tüm içeriğini kaldırın ve aşağıdakilerle değiştirin (görünüm denetleyicisinin APN'lerden alınan en son aygıt belirteci içerdiğinden emin olmak için):

    ```objc
    // Add import to the top of the file
    #import "ViewController.h"

    - (void)application:(UIApplication *)application
                didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
    {
        ViewController* rvc = (ViewController*) self.window.rootViewController;
        rvc.deviceToken = deviceToken;
    }
    ```

15. Son `AppDelegate.m`olarak, aşağıdaki yönteme sahip olduğundan emin olun:

    ```objc
    - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
        NSLog(@"%@", userInfo);
        [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
    }
    ```

## <a name="test-the-application"></a>Uygulamayı test etme

1. XCode'da uygulamayı fiziksel bir iOS aygıtında çalıştırın (anında iletme bildirimleri simülatörde çalışmaz).
2. iOS uygulaması Kullanıcı Arabirimi'nde, hem kullanıcı adı hem de parola için aynı değeri girin. Ardından **Giriş Yap'ı**tıklatın.

    ![iOS test uygulaması][2]

3. Kayıt başarısını bildiren bir açılır pencere görmelisiniz. **Tamam**'a tıklayın.

    ![iOS test bildirimi görüntülendi][3]

4. **Alıcı kullanıcı adı etiketi* metin alanına, başka bir cihazdan kayıt ile kullanılan kullanıcı adı etiketini girin.
5. Bir bildirim iletisi girin ve **Bildirim Gönder'i**tıklatın. Bildirim iletisini yalnızca alıcı kullanıcı adı etiketine sahip aygıtlar alır. Yalnızca bu kullanıcılara gönderilir.

    ![iOS testi etiketli bildirim][4]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, kayıtlarıyla ilişkili etiketleri olan belirli kullanıcılara anında iletme bildirimleri göndermeyi öğrendiniz. Konum tabanlı anında iletme bildirimleri göndermeyi öğrenmek için aşağıdaki öğreticiye ilerleyin: 

> [!div class="nextstepaction"]
>[Konum temelli anında iletme bildirimleri gönderme](notification-hubs-push-bing-spatial-data-geofencing-notification.md)

[1]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-interface.png
[2]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-user-pwd.png
[3]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-registered.png
[4]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-msg.png
