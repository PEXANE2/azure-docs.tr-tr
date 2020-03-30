---
title: Web API kullanarak anında iletme bildirimleri için geçerli kullanıcıyı kaydedin | Microsoft Dokümanlar
description: Kayıt ASP.NET Web API tarafından yapıldığında Azure Bildirim Hub'larına sahip bir iOS uygulamasında anında bildirim kaydı nı nasıl isteyeceğinizi öğrenin.
services: notification-hubs
documentationcenter: ios
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: 4e3772cf-20db-4b9f-bb74-886adfaaa65d
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: 3fec04a1a45f8b154e27a1e5303e44111f4cb421
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71211879"
---
# <a name="register-the-current-user-for-push-notifications-by-using-aspnet"></a>ASP.NET kullanarak anında iletme bildirimleri için geçerli kullanıcıyı kaydedin

> [!div class="op_single_selector"]
> * [iOS](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)

## <a name="overview"></a>Genel Bakış

Bu konu, kayıt ASP.NET Web API tarafından yapıldığında Azure Bildirim Hub'ları ile anında iletme bildirimi kaydını nasıl isteyeceğinizi gösterir. Bu konu, bildirim [hub'ları ile kullanıcıları bildir]öğretici genişletir. Kimlik doğrulaması mobil hizmeti oluşturmak için bu öğreticide gerekli adımları tamamlamış olmalısınız. Bildirim kullanıcıları senaryosu hakkında daha fazla bilgi için [bkz.]

## <a name="update-your-app"></a>Uygulamanızı güncelleştirme

1. MainStoryboard_iPhone.storyboard'unuzda, nesne kitaplığından aşağıdaki bileşenleri ekleyin:

   * **Etiket**: "Bildirim Hub'ları ile Kullanıcıya Itme"
   * **Etiket**: "InstallationId"
   * **Etiket**: "Kullanıcı"
   * **Metin Alanı**: "Kullanıcı"
   * **Etiket**: "Şifre"
   * **Metin Alanı**: "Şifre"
   * **Düğme**: "Giriş"

     Bu noktada, film şeridiniz aşağıdaki gibi görünür:

     ![][0]

2. Yardımcı düzenleyicide, anahtarlanan tüm denetimler için çıkışlar oluşturun ve bunları arayın, metin alanlarını Görünüm Denetleyicisi'ne (temsilci) bağlayın ve **giriş** düğmesi için bir **Eylem** oluşturun.

    ![][1]

    BreakingNewsViewController.h dosyanız artık aşağıdaki kodu içermelidir:

    ```objc
    @property (weak, nonatomic) IBOutlet UILabel *installationId;
    @property (weak, nonatomic) IBOutlet UITextField *User;
    @property (weak, nonatomic) IBOutlet UITextField *Password;

    - (IBAction)login:(id)sender;
    ```
3. Adlandırılmış `DeviceInfo`bir sınıf oluşturun ve aşağıdaki kodu DeviceInfo.h dosyasının arabirim bölümüne kopyalayın:

    ```objc
    @property (readonly, nonatomic) NSString* installationId;
    @property (nonatomic) NSData* deviceToken;
    ```
4. DeviceInfo.m dosyasının uygulama bölümünde aşağıdaki kodu kopyalayın:

    ```objc
    @synthesize installationId = _installationId;

    - (id)init {
        if (!(self = [super init]))
            return nil;

        NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
        _installationId = [defaults stringForKey:@"PushToUserInstallationId"];
        if(!_installationId) {
            CFUUIDRef newUUID = CFUUIDCreate(kCFAllocatorDefault);
            _installationId = (__bridge_transfer NSString *)CFUUIDCreateString(kCFAllocatorDefault, newUUID);
            CFRelease(newUUID);

            //store the install ID so we don't generate a new one next time
            [defaults setObject:_installationId forKey:@"PushToUserInstallationId"];
            [defaults synchronize];
        }

        return self;
    }

    - (NSString*)getDeviceTokenInHex {
        const unsigned *tokenBytes = [[self deviceToken] bytes];
        NSString *hexToken = [NSString stringWithFormat:@"%08X%08X%08X%08X%08X%08X%08X%08X",
                                ntohl(tokenBytes[0]), ntohl(tokenBytes[1]), ntohl(tokenBytes[2]),
                                ntohl(tokenBytes[3]), ntohl(tokenBytes[4]), ntohl(tokenBytes[5]),
                                ntohl(tokenBytes[6]), ntohl(tokenBytes[7])];
        return hexToken;
    }
    ```

5. PushToUserAppDelegate.h olarak, aşağıdaki özellik singleton ekleyin:

    ```objc
    @property (strong, nonatomic) DeviceInfo* deviceInfo;
    ```
6. `didFinishLaunchingWithOptions` PushToUserAppDelegate.m'deki yönteme aşağıdaki kodu ekleyin:

    ```objc
    self.deviceInfo = [[DeviceInfo alloc] init];

    [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
    ```

    İlk satır singleton'u `DeviceInfo` baş harfe salar. İkinci satır, [Bildirim Hub'ları ile Başlat] eğitimini zaten tamamladıysanız zaten mevcut olan anında iletme bildirimleri için kaydı başlatır.
7. PushToUserAppDelegate.m'de, AppDelegate'inizde yöntemi `didRegisterForRemoteNotificationsWithDeviceToken` uygulayın ve aşağıdaki kodu ekleyin:

    ```objc
    self.deviceInfo.deviceToken = deviceToken;
    ```

    Bu, istek için aygıt belirteci ayarlar.

   > [!NOTE]
   > Bu noktada, bu yöntemde başka bir kod olmamalıdır. [Bildirim Hub'ları ile Başlat](notification-hubs-ios-apple-push-notification-apns-get-started.md) eğitimini tamamladığınızda eklenen `registerNativeWithDeviceToken` yönteme zaten bir çağrınız varsa, bu çağrıyı açıklamanız veya kaldırmanız gerekir.

8. Dosyada `PushToUserAppDelegate.m` aşağıdaki işleyici yöntemini ekleyin:

    ```objc
    * (void) application:(UIApplication *) application didReceiveRemoteNotification:(NSDictionary *)userInfo {
       NSLog(@"%@", userInfo);
       UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                             [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:
                             @"OK" otherButtonTitles:nil, nil];
       [alert show];
    }
    ```

    Bu yöntem, uygulamanız çalışırken bildirim aldığında UI'de bir uyarı görüntüler.

9. Dosyayı `PushToUserViewController.m` açın ve aşağıdaki uygulamada klavyeyi döndürün:

    ```objc
    - (BOOL)textFieldShouldReturn:(UITextField *)theTextField {
        if (theTextField == self.User || theTextField == self.Password) {
            [theTextField resignFirstResponder];
        }
        return YES;
    }
    ```

10. Dosyadaki `viewDidLoad` yöntemde, `PushToUserViewController.m` `installationId` etiketi aşağıdaki gibi başlatma:

    ```objc
    DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
    Self.installationId.text = deviceInfo.installationId;
    ```

11. Arayüz aşağıdaki özellikleri `PushToUserViewController.m`ekleyin:

    ```objc
    @property (readonly) NSOperationQueue* downloadQueue;
    - (NSString*)base64forData:(NSData*)theData;
    ```

12. Ardından, aşağıdaki uygulamayı ekleyin:

    ```objc
    - (NSOperationQueue *)downloadQueue {
        if (!_downloadQueue) {
            _downloadQueue = [[NSOperationQueue alloc] init];
            _downloadQueue.name = @"Download Queue";
            _downloadQueue.maxConcurrentOperationCount = 1;
        }
        return _downloadQueue;
    }

    // base64 encoding
    - (NSString*)base64forData:(NSData*)theData
    {
        const uint8_t* input = (const uint8_t*)[theData bytes];
        NSInteger length = [theData length];

        static char table[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";

        NSMutableData* data = [NSMutableData dataWithLength:((length + 2) / 3) * 4];
        uint8_t* output = (uint8_t*)data.mutableBytes;

        NSInteger i;
        for (i=0; i < length; i += 3) {
            NSInteger value = 0;
            NSInteger j;
            for (j = i; j < (i + 3); j++) {
                value <<= 8;

                if (j < length) {
                    value |= (0xFF & input[j]);
                }
            }

            NSInteger theIndex = (i / 3) * 4;
            output[theIndex + 0] =                    table[(value >> 18) & 0x3F];
            output[theIndex + 1] =                    table[(value >> 12) & 0x3F];
            output[theIndex + 2] = (i + 1) < length ? table[(value >> 6)  & 0x3F] : '=';
            output[theIndex + 3] = (i + 2) < length ? table[(value >> 0)  & 0x3F] : '=';
        }

        return [[NSString alloc] initWithData:data encoding:NSASCIIStringEncoding];
    }
    ```

13. Aşağıdaki kodu XCode `login` tarafından oluşturulan işleyici yöntemine kopyalayın:

    ```objc
    DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];

    // build JSON
    NSString* json = [NSString stringWithFormat:@"{\"platform\":\"ios\", \"instId\":\"%@\", \"deviceToken\":\"%@\"}", deviceInfo.installationId, [deviceInfo getDeviceTokenInHex]];

    // build auth string
    NSString* authString = [NSString stringWithFormat:@"%@:%@", self.User.text, self.Password.text];

    NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"http://nhnotifyuser.azurewebsites.net/api/register"]];
    [request setHTTPMethod:@"POST"];
    [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
    [request addValue:[@([json lengthOfBytesUsingEncoding:NSUTF8StringEncoding]) description] forHTTPHeaderField:@"Content-Length"];
    [request addValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
    [request addValue:[NSString stringWithFormat:@"Basic %@",[self base64forData:[authString dataUsingEncoding:NSUTF8StringEncoding]]] forHTTPHeaderField:@"Authorization"];

    // connect with POST
    [NSURLConnection sendAsynchronousRequest:request queue:[self downloadQueue] completionHandler:^(NSURLResponse* response, NSData* data, NSError* error) {
        // add UIAlert depending on response.
        if (error != nil) {
            NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
            if ([httpResponse statusCode] == 200) {
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Back-end registration" message:@"Registration successful" delegate:nil cancelButtonTitle: @"OK" otherButtonTitles:nil, nil];
                [alert show];
            } else {
                NSLog(@"status: %ld", (long)[httpResponse statusCode]);
            }
        } else {
            NSLog(@"error: %@", error);
        }
    }];
    ```

    Bu yöntem, anında iletme bildirimleri için hem bir yükleme kimliği hem de kanal alır ve aygıt türüyle birlikte bildirim hub'larında bir kayıt oluşturan kimlik doğrulaması Web API yöntemine gönderir. Bu Web API' [si, Bildirim Hub'ları olan kullanıcıları bilgilendir' de tanımlanmıştır.]

İstemci uygulaması güncelleştirilmeye göre, [Bildirim Hub'ları ile kullanıcıları bilgilendirin] ve Bildirim Hub'larını kullanarak bildirim göndermek için mobil hizmeti güncelleyin.

<!-- Anchors. -->

<!-- Images. -->
[0]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios1.png
[1]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios2.png

<!-- URLs. -->
[Bildirim Hub'ları ile kullanıcıları bilgilendirme]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Bildirim Hub'larıyla Başlayın]: notification-hubs-ios-apple-push-notification-apns-get-started.md
