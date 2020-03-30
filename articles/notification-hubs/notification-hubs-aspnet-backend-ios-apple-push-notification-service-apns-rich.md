---
title: Azure Bildirim Hub'ları Zengin Push
description: Azure'dan bir iOS uygulamasına nasıl zengin anında iletme bildirimleri göndereceğinizi öğrenin. Amaç-C ve C# ile yazılmış kod örnekleri.
documentationcenter: ios
services: notification-hubs
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: 590304df-c0a4-46c5-8ef5-6a6486bb3340
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: 9da629929ca88f406dc503710477104be94c47e3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71212196"
---
# <a name="azure-notification-hubs-rich-push"></a>Azure Bildirim Hub'ları Zengin Push

## <a name="overview"></a>Genel Bakış

Kullanıcıların anlık zengin içerikleriyle etkileşime girebilmesi için, bir uygulama düz metnin ötesine itmek isteyebilir. Bu bildirimler kullanıcı etkileşimlerini teşvik eder ve urller, sesler, resimler/kuponlar ve daha fazlası gibi içerikler sunar. Bu öğretici, [Kullanıcıları Bildir](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) konusuna göre biroluşturur ve yükleri içeren anında iletme bildirimlerinin nasıl gönderilebildiğini (örneğin, resim) gösterir.

Bu öğretici iOS 7 & 8 ile uyumludur.

  ![][IOS1]

Yüksek düzeyde:

1. Uygulama arka ucu:
   * Zengin yükü (bu durumda, görüntü) arka uç veritabanında/yerel depolamada saklar
   * Bu zengin bildirimin kimliğini aygıta gönderir
2. Cihazdaki uygulama:
   * Aldığı kimlikle zengin yükü isteyen arka uçla iletişim kurun
   * Veri alma tamamlandığında kullanıcılara cihazda bildirimler gönderir ve kullanıcılar daha fazla bilgi edinmek için dokunduğunda yükü hemen gösterir

## <a name="webapi-project"></a>WebAPI Projesi

1. Visual Studio'da, [Kullanıcıları Bildir](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) öğreticisinde oluşturduğunuz **AppBackend** projesini açın.
2. Kullanıcılara bildirmek istediğiniz bir resim edinin ve proje dizininizdeki bir **img** klasörüne koyun.
3. Çözüm Gezgini'ndeki **Tüm Dosyaları Göster'i** tıklatın ve **Projeye Dahil**Etmek için klasörü sağ tıklatın.
4. Görüntü seçildiğinde, Özellikler'deki Yapı Eylemi penceresini **Gömülü Kaynak**olarak değiştirin.

    ![][IOS2]
5. In `Notifications.cs`, aşağıdaki ifadesini kullanarak ekleyin:

    ```csharp
    using System.Reflection;
    ```
6. Tüm `Notifications` sınıfı aşağıdaki kodla güncelleştirin. Yer tutucuları bildirim merkezi kimlik bilgileriniz ve resim dosya adınızla değiştirdiğinizden emin olun.

    ```csharp
    public class Notification {
        public int Id { get; set; }
        // Initial notification message to display to users
        public string Message { get; set; }
        // Type of rich payload (developer-defined)
        public string RichType { get; set; }
        public string Payload { get; set; }
        public bool Read { get; set; }
    }

    public class Notifications {
        public static Notifications Instance = new Notifications();

        private List<Notification> notifications = new List<Notification>();

        public NotificationHubClient Hub { get; set; }

        private Notifications() {
            // Placeholders: replace with the connection string (with full access) for your notification hub and the hub name from the Azure Classics Portal
            Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",  "{hub name}");
        }

        public Notification CreateNotification(string message, string richType, string payload) {
            var notification = new Notification() {
                Id = notifications.Count,
                Message = message,
                RichType = richType,
                Payload = payload,
                Read = false
            };

            notifications.Add(notification);

            return notification;
        }

        public Stream ReadImage(int id) {
            var assembly = Assembly.GetExecutingAssembly();
            // Placeholder: image file name (for example, logo.png).
            return assembly.GetManifestResourceStream("AppBackend.img.{logo.png}");
        }
    }
    ```

   > [!NOTE]
   > (isteğe bağlı) Proje kaynaklarının nasıl ekleyip edinilir hakkında daha fazla bilgi için [Visual C# kullanarak kaynakları nasıl katıştırır ve erişirsiniz.](https://support.microsoft.com/kb/319292)

7. In `NotificationsController.cs`, aşağıdaki parçacıklar ile 'NotificationsController yeniden tanımlayın. Bu, aygıta ilk sessiz zengin bildirim kimliği gönderir ve istemci tarafından görüntü alınmasını sağlar:

    ```csharp
    // Return http response with image binary
    public HttpResponseMessage Get(int id) {
        var stream = Notifications.Instance.ReadImage(id);

        var result = new HttpResponseMessage(HttpStatusCode.OK);
        result.Content = new StreamContent(stream);
        // Switch in your image extension for "png"
        result.Content.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("image/{png}");

        return result;
    }

    // Create rich notification and send initial silent notification (containing id) to client
    public async Task<HttpResponseMessage> Post() {
        // Replace the placeholder with image file name
        var richNotificationInTheBackend = Notifications.Instance.CreateNotification("Check this image out!", "img",  "{logo.png}");

        var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;

        // Silent notification with content available
        var aboutUser = "{\"aps\": {\"content-available\": 1, \"sound\":\"\"}, \"richId\": \"" + richNotificationInTheBackend.Id.ToString() + "\",  \"richMessage\": \"" + richNotificationInTheBackend.Message + "\", \"richType\": \"" + richNotificationInTheBackend.RichType + "\"}";

        // Send notification to apns
        await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(aboutUser, usernameTag);

        return Request.CreateResponse(HttpStatusCode.OK);
    }
    ```
8. Şimdi bu uygulamayı tüm cihazlardan erişilebilir hale getirmek için bir Azure Web Sitesine yeniden dağıtacağız. **AppBackend** projesine sağ tıklayıp **Yayımla**’yı seçin.
9. Yayımlama hedefiniz olarak Azure Web Sitesini seçin. Azure hesabınızla oturum açın ve varolan veya yeni bir Web Sitesi seçin ve **Bağlantı** sekmesinde **hedef URL** özelliğine dikkat edin. Bu URL'yi daha sonra bu öğreticide *arka uç bitiş noktanız* olarak adlandıracağız. **Yayımla**’ta tıklayın.

## <a name="modify-the-ios-project"></a>iOS projesini değiştirme

Artık yalnızca bir *bildirimin kimliğini* göndermek için uygulama arka uçunuzu değiştirdiğinize göre, iOS uygulamanızı değiştirip bu kimliği işleyecek ve zengin iletiyi arka uçunuzdan alırsınız.

1. iOS projenizi açın ve **Hedefler** bölümündeki ana uygulama hedefinize giderek uzaktan bildirimleri etkinleştirin.
2. **Özellikler'e**tıklayın, **Arka Plan Modlarını**açın ve **Uzak Bildirimler** onay kutusunu kontrol edin.

    ![][IOS3]
3. `Main.storyboard` [Kullanıcıyı Bildir](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) öğreticisinden bir Görünüm Denetleyicisi (bu eğitimde Home View Controller olarak anılacaktır) olduğundan emin olun.
4. Film şeridinize bir **Gezinti Denetleyicisi** ekleyin ve gezintinin **kök görünümü** yapmak için Ana Görünüm Denetleyicisine denetim sürükleyin. Yalnızca Gezinti Denetleyicisi için Özniteliklerdeki İlk Görünüm Denetleyicisi'nin seçildiğinden emin olun. **Is Initial View Controller**
5. Film şeridine **Görünüm Denetleyicisi** ekleyin ve **Görüntü Görünümü**ekleyin. Bu, kullanıcıların bildirime tıklayarak daha fazla bilgi edinmeyi seçtiklerinde görecekleri sayfadır. Hikaye şeridiniz aşağıdaki gibi görünmelidir:

    ![][IOS4]
6. Storyboard'daki **Home View Controller'a** tıklayın ve Kimlik denetçisi altında **Özel Sınıf** ve **Storyboard Kimliği** olarak **homeViewController** olduğundan emin olun.
7. **ImageViewController**olarak Görüntü Görünümü Denetleyicisi için aynı şeyi yapın.
8. Ardından, az önce oluşturduğunuz UI'yi işlemek için **imageViewController** başlıklı yeni bir Görünüm Denetleyicisi sınıfı oluşturun.
9. **imageViewController.h'de,** denetleyicinin arabirim bildirimlerine aşağıdakileri ekleyin. İkisini bağlamak için film şeridi görüntü görünümünden bu özelliklere denetim-sürükleme yaptığınızdan emin olun:

    ```objc
    @property (weak, nonatomic) IBOutlet UIImageView *myImage;
    @property (strong) UIImage* imagePayload;
    ```
10. In `imageViewController.m`, sonuna aşağıdaki `viewDidload`ekleyin:

    ```objc
    // Display the UI Image in UI Image View
    [self.myImage setImage:self.imagePayload];
    ```
11. Içinde `AppDelegate.m`, oluşturduğunuz görüntü denetleyicisini içe aktarın:

    ```objc
    #import "imageViewController.h"
    ```
12. Aşağıdaki bildirimi içeren bir arabirim bölümü ekleyin:

    ```objc
    @interface AppDelegate ()

    @property UIImage* imagePayload;
    @property NSDictionary* userInfo;
    @property BOOL iOS8;

    // Obtain content from backend with notification id
    - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;

    // Redirect to Image View Controller after notification interaction
    - (void)redirectToImageViewWithImage: (UIImage *)img;

    @end
    ```
13. , `AppDelegate`uygulamanızın sessiz bildirimler için kaydolduğundan `application: didFinishLaunchingWithOptions`emin olun:

    ```objc
    // Software version
    self.iOS8 = [[UIApplication sharedApplication] respondsToSelector:@selector(registerUserNotificationSettings:)] && [[UIApplication sharedApplication] respondsToSelector:@selector(registerForRemoteNotifications)];

    // Register for remote notifications for iOS8 and previous versions
    if (self.iOS8) {
        NSLog(@"This device is running with iOS8.");

        // Action
        UIMutableUserNotificationAction *richPushAction = [[UIMutableUserNotificationAction alloc] init];
        richPushAction.identifier = @"richPushMore";
        richPushAction.activationMode = UIUserNotificationActivationModeForeground;
        richPushAction.authenticationRequired = NO;
        richPushAction.title = @"More";

        // Notification category
        UIMutableUserNotificationCategory* richPushCategory = [[UIMutableUserNotificationCategory alloc] init];
        richPushCategory.identifier = @"richPush";
        [richPushCategory setActions:@[richPushAction] forContext:UIUserNotificationActionContextDefault];

        // Notification categories
        NSSet* richPushCategories = [NSSet setWithObjects:richPushCategory, nil];

        UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                UIUserNotificationTypeAlert |
                                                UIUserNotificationTypeBadge
                                                                                    categories:richPushCategories];

        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];

    }
    else {
        // Previous iOS versions
        NSLog(@"This device is running with iOS7 or earlier versions.");

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
    }

    return YES;
    ```

14. Storyboard Kullanıcı Arabirimi değişikliklerini dikkate almak için `application:didRegisterForRemoteNotificationsWithDeviceToken` aşağıdaki uygulamanın yerine geç:

    ```objc
    // Access navigation controller which is at the root of window
    UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
    // Get home view controller from stack on navigation controller
    homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
    hvc.deviceToken = deviceToken;
    ```
15. Ardından, görüntüyü bitiş `AppDelegate.m` noktanızdan almak ve alma tamamlandığında yerel bir bildirim göndermek için aşağıdaki yöntemleri ekleyin. Yer tutucuyu `{backend endpoint}` arka uç bitiş noktanızla değiştirin:

    ```objc
    NSString *const GetNotificationEndpoint = @"{backend endpoint}/api/notifications";

    // Helper: retrieve notification content from backend with rich notification id
    - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion {
        UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
        homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
        NSString* authenticationHeader = hvc.registerClient.authenticationHeader;
        // Check if authenticated
        if (!authenticationHeader) return;

        NSURLSession* session = [NSURLSession
                                sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                delegate:nil
                                delegateQueue:nil];

        NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, richId]];
        NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
        [request setHTTPMethod:@"GET"];
        NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
        [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

        NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {

            NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
            if (!error && httpResponse.statusCode == 200) {
                // From NSData to UIImage
                self.imagePayload = [UIImage imageWithData:data];

                completion(nil);
            }
            else {
                NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                if (error)
                    completion(error);
                else {
                    completion([NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                }
            }
        }];
        [dataTask resume];
    }

    // Handle silent push notifications when id is sent from backend
    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler {
        self.userInfo = userInfo;
        int richId = [[self.userInfo objectForKey:@"richId"] intValue];
        NSString* richType = [self.userInfo objectForKey:@"richType"];

        // Retrieve image data
        if ([richType isEqualToString:@"img"]) {  
            [self retrieveRichImageWithId:richId completion:^(NSError* error) {
                if (!error){
                    // Send local notification
                    UILocalNotification* localNotification = [[UILocalNotification alloc] init];

                    // "5" is arbitrary here to give you enough time to quit out of the app and receive push notifications
                    localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:5];
                    localNotification.userInfo = self.userInfo;
                    localNotification.alertBody = [self.userInfo objectForKey:@"richMessage"];
                    localNotification.timeZone = [NSTimeZone defaultTimeZone];

                    // iOS8 categories
                    if (self.iOS8) {
                        localNotification.category = @"richPush";
                    }

                    [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                    handler(UIBackgroundFetchResultNewData);
                }
                else{
                    handler(UIBackgroundFetchResultFailed);
                }
            }];
        }
        // Add "else if" here to handle more types of rich content such as url, sound files, etc.
    }
    ```
16. Aşağıdaki `AppDelegate.m` yöntemlerle görüntü görünümü denetleyicisini açarak yukarıdaki yerel bildirimi işleyin:

    ```objc
    // Helper: redirect users to image view controller
    - (void)redirectToImageViewWithImage: (UIImage *)img {
        UINavigationController *navigationController = (UINavigationController*) self.window.rootViewController;
        UIStoryboard *mainStoryboard = [UIStoryboard storyboardWithName:@"Main" bundle: nil];
        imageViewController *imgViewController = [mainStoryboard instantiateViewControllerWithIdentifier: @"imageViewController"];
        // Pass data/image to image view controller
        imgViewController.imagePayload = img;

        // Redirect
        [navigationController pushViewController:imgViewController animated:YES];
    }

    // Handle local notification sent above in didReceiveRemoteNotification
    - (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification {
        if (application.applicationState == UIApplicationStateActive) {
            // Show in-app alert with an extra "more" button
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:notification.alertBody delegate:self cancelButtonTitle:@"OK" otherButtonTitles:@"More", nil];
            [alert show];
        }
        // App becomes active from user's tap on notification
        else {
            [self redirectToImageViewWithImage:self.imagePayload];
        }
    }

    // Handle buttons in in-app alerts and redirect with data/image
    - (void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex {
        // Handle "more" button
        if (buttonIndex == 1)
        {
            [self redirectToImageViewWithImage:self.imagePayload];
        }
        // Add "else if" here to handle more buttons
    }

    // Handle notification setting actions in iOS8
    - (void)application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier forLocalNotification:(UILocalNotification *)notification completionHandler:(void (^)())completionHandler {
        // Handle richPush related buttons
        if ([identifier isEqualToString:@"richPushMore"]) {
            [self redirectToImageViewWithImage:self.imagePayload];
        }
        completionHandler();
    }
    ```

## <a name="run-the-application"></a>Uygulamayı Çalıştır

1. XCode'da uygulamayı fiziksel bir iOS aygıtında çalıştırın (anında iletme bildirimleri simülatörde çalışmaz).
2. iOS uygulaması Kullanıcı Arabirimi'nde, kimlik doğrulama için aynı değerde bir kullanıcı adı ve parola girin ve **Giriş Yap'ı**tıklatın.
3. **Gönder tuşuna** bastığınızda uygulama içi bir uyarı görmeniz gerekir. **Daha Fazla'ya**tıklarsanız, uygulama arka uçunuza eklemeyi seçtiğiniz resme getirilirsiniz.
4. Ayrıca Gönder **tuşuna** tıklayıp hemen cihazınızın ana düğmesine basabilirsiniz. Birkaç dakika içinde, bir anında iletme bildirimi alacaksınız. Üzerine dokunur sanız veya Daha Fazlasını tıklarsanız, uygulamanıza ve zengin resim içeriğine getirilirsiniz.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png
