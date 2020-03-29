---
title: Azure Bildirim Hub'ları ile Güvenli Anında Iletme Bildirimleri Gönderme
description: Azure'dan bir Android uygulamasına güvenli anında iletme bildirimleri gönderme yi öğrenin. Java ve C# ile yazılmış kod örnekleri.
documentationcenter: android
keywords: push notification,push bildirimleri,push iletileri,android push bildirimleri
author: sethmanheim
manager: femila
editor: jwargo
services: notification-hubs
ms.assetid: daf3de1c-f6a9-43c4-8165-a76bfaa70893
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: 419a9f9b5ce698c7516edd55856cbea9891ba029
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71212173"
---
# <a name="sending-secure-push-notifications-with-azure-notification-hubs"></a>Azure Bildirim Hub'ları ile Güvenli Anında Iletme Bildirimleri Gönderme

> [!div class="op_single_selector"]
> * [Windows Evrensel](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)

## <a name="overview"></a>Genel Bakış

> [!IMPORTANT]
> Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).

Microsoft Azure'da anında bildirim desteği, kullanımı kolay, çok platformlu, ölçeklenmiş anında iletme iletisi altyapısına erişmenizi sağlar ve bu da hem tüketici hem de kurumsal uygulamalar için anında iletme bildirimlerinin uygulanmasını büyük ölçüde kolaylaştırır mobil platformlar.

Düzenleyici veya güvenlik kısıtlamaları nedeniyle, bazen bir uygulama bildirime standart anında iletme bildirimi altyapısı aracılığıyla aktarılamayan bir şey eklemek isteyebilir. Bu öğretici, istemci Android aygıt ve uygulama arka uç arasında güvenli, kimlik doğrulaması bağlantı yoluyla hassas bilgileri göndererek aynı deneyimi elde etmek için nasıl açıklanır.

Yüksek bir düzeyde, akış aşağıdaki gibidir:

1. Uygulama arka uç:
   * Güvenli yükü arka uç veritabanında saklar.
   * Bu bildirimin kimliğini Android cihazına gönderir (güvenli bilgi gönderilmez).
2. Bildirim alırken cihazdaki uygulama:
   * Android cihazı, güvenli yükü talep etmek için arka uçla bağlantı kurur.
   * Uygulama, yükü cihazda bir bildirim olarak gösterebilir.

Önceki akışta (ve bu öğreticide) aygıtın, kullanıcı oturum açtıktan sonra yerel depolama alanında bir kimlik doğrulama belirteci depoladığının varsayılması önemlidir. Bu yaklaşım, aygıt bu belirteç kullanarak bildirimin güvenli yükünü alabildiği için sorunsuz bir deneyim garanti eder. Uygulamanız cihazda kimlik doğrulama belirteçleri depolamazveya bu belirteçlerin süresi dolmuşsa, anında iletme bildirimini aldıktan sonra kullanıcının uygulamayı başlatmasını isteyen genel bir bildirim görüntülemesi gerekir. Uygulama daha sonra kullanıcının kimliğini doğrular ve bildirim yükünü gösterir.

Bu öğretici, güvenli anında iletme bildirimleri nasıl gönderilen gösterir. [Kullanıcıları Bildir](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) öğreticisine göre inşa eder, bu nedenle önce bu öğreticideki adımları tamamlamanız gerekir.

> [!NOTE]
> Bu öğretici, [Bildirim Hub'ları (Android) ile Başlarken](notification-hubs-android-push-notification-google-gcm-get-started.md)açıklandığı gibi bildirim hub'ınızı oluşturduğunuzu ve yapılandırdığınızı varsayar.

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-android-project"></a>Android projesini değiştirme

Uygulamanızın arka ucunu yalnızca bir anında iletme bildiriminin *kimliğini* göndermek için değiştirdiğinize göre, bu bildirimi işlemek için Android uygulamanızı değiştirmeniz ve görüntülenecek güvenli iletiyi almak için arka uçunuzu geri aramanız gerekir.
Bu amaca ulaşmak için, Android uygulamanızın anında iletme bildirimlerini aldığında arka uçla kendini nasıl doğrulayabildiğini bildiğinden emin olmalısınız.

Şimdi, uygulamanızın paylaşılan tercihlerinde kimlik doğrulama üstbilgi değerini kaydetmek için *oturum açma* akışını değiştirin. Benzer mekanizmalar, uygulamanın kullanıcı kimlik bilgilerine gerek kalmadan kullanması gereken kimlik doğrulama belirteci (örneğin, OAuth belirteçleri) depolamak için kullanılabilir.

1. Android uygulama projenizde, `MainActivity` sınıfın en üstündeki sabitleri ekleyin:

    ```java
    public static final String NOTIFY_USERS_PROPERTIES = "NotifyUsersProperties";
    public static final String AUTHORIZATION_HEADER_PROPERTY = "AuthorizationHeader";
    ```
2. Hala `MainActivity` sınıfta, aşağıdaki `getAuthorizationHeader()` kodu içerecek şekilde yöntemi güncelleştirmek:

    ```java
    private String getAuthorizationHeader() throws UnsupportedEncodingException {
        EditText username = (EditText) findViewById(R.id.usernameText);
        EditText password = (EditText) findViewById(R.id.passwordText);
        String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
        basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);

        SharedPreferences sp = getSharedPreferences(NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
        sp.edit().putString(AUTHORIZATION_HEADER_PROPERTY, basicAuthHeader).commit();

        return basicAuthHeader;
    }
    ```
3. Dosyanın `import` üst kısmında aşağıdaki ifadeleri ekleyin: `MainActivity`

    ```java
    import android.content.SharedPreferences;
    ```

Şimdi, bildirim alındığı zaman çağrılan işleyiciyi değiştirin.

1. `MyHandler` Sınıfta aşağıdakileri `OnReceive()` içerecek yöntemi değiştirin:

    ```java
    public void onReceive(Context context, Bundle bundle) {
        ctx = context;
        String secureMessageId = bundle.getString("secureId");
        retrieveNotification(secureMessageId);
    }
    ```
2. Ardından, `retrieveNotification()` arka uçunuzu dağıtırken `{back-end endpoint}` yer tutucuyu elde edilen arka uç noktayla değiştirerek yöntemi ekleyin:

    ```java
    private void retrieveNotification(final String secureMessageId) {
        SharedPreferences sp = ctx.getSharedPreferences(MainActivity.NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
        final String authorizationHeader = sp.getString(MainActivity.AUTHORIZATION_HEADER_PROPERTY, null);

        new AsyncTask<Object, Object, Object>() {
            @Override
            protected Object doInBackground(Object... params) {
                try {
                    HttpUriRequest request = new HttpGet("{back-end endpoint}/api/notifications/"+secureMessageId);
                    request.addHeader("Authorization", "Basic "+authorizationHeader);
                    HttpResponse response = new DefaultHttpClient().execute(request);
                    if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                        Log.e("MainActivity", "Error retrieving secure notification" + response.getStatusLine().getStatusCode());
                        throw new RuntimeException("Error retrieving secure notification");
                    }
                    String secureNotificationJSON = EntityUtils.toString(response.getEntity());
                    JSONObject secureNotification = new JSONObject(secureNotificationJSON);
                    sendNotification(secureNotification.getString("Payload"));
                } catch (Exception e) {
                    Log.e("MainActivity", "Failed to retrieve secure notification - " + e.getMessage());
                    return e;
                }
                return null;
            }
        }.execute(null, null, null);
    }
    ```

Bu yöntem, paylaşılan tercihlerde depolanan kimlik bilgilerini kullanarak bildirim içeriğini almak için uygulamanızı arka uç olarak çağırır ve normal bir bildirim olarak görüntüler. Bildirim, uygulama kullanıcısına diğer anında iletme bildirimleri gibi görünür.

Eksik kimlik doğrulama üstbilgi özelliği veya geri uç tarafından reddedilme durumlarını işlemek tercih edilir. Bu servis taleplerini belirli şekilde işleme, çoğunlukla hedef kullanıcı deneyiminize bağlıdır. Seçeneklerden biri, kullanıcının gerçek bildirimi almak için kimlik doğrulaması için genel bir komut istemiyle bir bildirim görüntülemektir.

## <a name="run-the-application"></a>Uygulamayı Çalıştır

Uygulamayı çalıştırmak için aşağıdaki eylemleri gerçekleştirin:

1. **AppBackend'in** Azure'da dağıtıldığından emin olun. Visual Studio kullanıyorsanız, **AppBackend** Web API uygulamasını çalıştırın. ASP.NET bir web sayfası görüntülenir.
2. Eclipse'de, uygulamayı fiziksel bir Android cihazda veya emülatörde çalıştırın.
3. Android uygulaması Kullanıcı Arabirimi'ne bir kullanıcı adı ve parola girin. Bunlar herhangi bir dize olabilir, ancak aynı değerolmalıdır.
4. Android uygulaması UI'de **Giriş Yap'ı**tıklatın. Ardından **Gönder tuşuna**bas.
