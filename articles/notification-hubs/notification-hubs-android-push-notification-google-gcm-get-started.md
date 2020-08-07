---
title: Azure Notification Hubs ve Google Cloud Messaging kullanarak Android 'e anında iletme bildirimleri gönderin | Microsoft Docs
description: Bu öğreticide, Android cihazlarına anında iletme bildirimleri göndermek için Azure Notification Hubs ve Google Firebase Cloud Messaging’in nasıl kullanılacağını öğrenirsiniz.
services: notification-hubs
documentationcenter: android
keywords: anında iletme bildirimleri,anında iletme bildirimi,android anında iletme bildirimi
author: sethmanheim
manager: femila
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: tutorial
ms.custom: mvc, devx-track-java
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: 8c97710202a448c613ab685932cb335bbaed4953
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87832663"
---
# <a name="tutorial-send-push-notifications-to-android-devices-by-using-azure-notification-hubs-and-google-cloud-messaging-deprecated"></a>Öğretici: Azure Notification Hubs ve Google Cloud Messaging (kullanım dışı) kullanarak Android cihazlara anında iletme bildirimleri gönderme

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

> [!WARNING]
> 10 Nisan 2018 itibariyle, Google Google Cloud Messaging (GCM) kullanım dışıdır. GCM sunucusu ve istemci API 'Leri kullanım dışıdır ve 29 Mayıs 2019 ' den hemen sonra kaldırılacaktır. Daha fazla bilgi için bkz. [GCM ve FCM hakkında sık sorulan sorular](https://developers.google.com/cloud-messaging/faq).

## <a name="overview"></a>Genel Bakış

Bu öğreticide, bir Android uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağı gösterilir.
Google Cloud Messaging (GCM) kullanarak anında iletme bildirimleri alan bir Android uygulaması oluşturursunuz.

> [!IMPORTANT]
> Google Cloud Messaging (GCM) kullanım dışıdır ve [yakında](https://developers.google.com/cloud-messaging/faq)kaldırılacak.

> [!IMPORTANT]
> Bu konuda Google Cloud Messaging (GCM) ile anında iletme bildirimleri gösterilmektedir. Google Firebase Cloud Messaging (FCM) kullanıyorsanız bkz. [Azure Notification Hubs ve FCM ile Android’e anında iletme bildirimleri gönderme](notification-hubs-android-push-notification-google-fcm-get-started.md).

Bu öğreticinin tamamlanan kodu GitHub'da [buradan](https://github.com/Azure/azure-notificationhubs-android/tree/master/FCMTutorialApp) indirilebilir.

Bu öğreticide, aşağıdaki eylemleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Google Cloud Messaging'i destekleyen bir proje oluşturma.
> * Bildirim hub’ı oluşturma
> * Uygulamanızı bildirim hub'ına bağlama
> * Uygulamayı test etme

## <a name="prerequisites"></a>Önkoşullar

* **Azure aboneliği**. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı oluşturun](https://azure.microsoft.com/free/) .
* [Android Studio](https://go.microsoft.com/fwlink/?LinkId=389797).

## <a name="creating-a-project-that-supports-google-cloud-messaging"></a>Google Cloud Messaging'i destekleyen bir proje oluşturma

[!INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

## <a name="create-a-notification-hub"></a>Bildirim hub’ı oluşturma

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

### <a name="configure-gcm-setting-for-the-notification-hub"></a>Bildirim hub’ı için GCM ayarını yapılandırma

1. **BILDIRIM ayarlarında** **Google (GCM)** seçeneğini belirleyin.
2. Google Cloud Console’dan aldığınız **API Anahtarını** girin.
3. Araç çubuğunda **Kaydet**’i seçin.

    ![Azure Notification Hubs - Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

Bildirim hub'ınız şimdi GCM ile birlikte çalışmak üzere yapılandırıldı. Ayrıca, uygulamanızı anında iletme bildirimleri alması ve göndermesi amacıyla kaydetmek için bağlantı dizelerine sahipsiniz.

## <a name="connect-your-app-to-the-notification-hub"></a><a id="connecting-app"></a>Uygulamanızı Bildirim Hub 'ına bağlama

### <a name="create-a-new-android-project"></a>Yeni bir Android projesi oluşturma

1. Android Studio'da yeni bir Android Studio projesi başlatın.

   ![Android Studio - yeni proje][13]
2. **Telefon ve Tablet** form faktörünü ve desteklemek istediğiniz **Minimum SDK**'yı seçin. Ardından **İleri**'ye tıklayın.

   ![Android Studio - proje oluşturma iş akışı][14]
3. Ana etkinlik için **Boş Etkinlik**'i seçin, **İleri**'ye tıklayın ve ardından **Son**'a tıklayın.

### <a name="add-google-play-services-to-the-project"></a>Projeye Google Play hizmetlerini ekleme

[!INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

### <a name="adding-azure-notification-hubs-libraries"></a>Azure Notification Hubs kitaplıkları ekleme

1. `Build.Gradle` **Uygulamanın**dosyasında, **Bağımlılıklar** bölümüne aşağıdaki satırları ekleyin.

    ```gradle
    implementation 'com.microsoft.azure:notification-hubs-android-sdk:0.6@aar'
    implementation 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
    ```

2. **Bağımlılıklar** bölümünden sonra aşağıdaki depoyu ekleyin.

    ```gradle
    repositories {
        maven {
            url "https://dl.bintray.com/microsoftazuremobile/SDK"
        }
    }
    ```

### <a name="updating-the-projects-androidmanifestxml"></a>Projenin AndroidManifest.xml güncelleştiriliyor

1. GCM’yi desteklemek için, kodda [Google’ın Örnek Kimliği API’sini](https://developers.google.com/instance-id/) kullanarak [kayıt belirteçleri elde etmek](https://developers.google.com/cloud-messaging/) için kullanılan bir Örnek Kimliği dinleyici hizmeti uygulayın. Bu öğreticide sınıfın adı `MyInstanceIDService` şeklindedir.

    Aşağıdaki hizmet tanımını AndroidManifest.xml dosyasında `<application>` etiketinin içine ekleyin. `<your package>` yer tutucusunu, `AndroidManifest.xml` dosyasının üst kısmında gösterilen asıl paket adınızla değiştirin.
  
    ```xml
    <service android:name="<your package>.MyInstanceIDService" android:exported="false">
        <intent-filter>
            <action android:name="com.google.android.gms.iid.InstanceID"/>
        </intent-filter>
    </service>
    ```

2. Uygulama, Örnek Kimliği API’sinden GCM kayıt belirtecini aldıktan sonra, [Azure Notification Hub'a kaydolmak](notification-hubs-push-notification-registration-management.md) için bu belirteci kullanır. `RegistrationIntentService` adlı bir `IntentService` kullanılarak arka planda kayıt gerçekleştirilir. Bu hizmet, [GCM kayıt belirtecini yenilemekten](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) sorumludur.

    Aşağıdaki hizmet tanımını AndroidManifest.xml dosyasında `<application>` etiketinin içine ekleyin. `<your package>` yer tutucusunu, `AndroidManifest.xml` dosyasının üst kısmında gösterilen asıl paket adınızla değiştirin.

    ```xml
    <service
        android:name="<your package>.RegistrationIntentService"
        android:exported="false">
    </service>
    ```

3. Bildirimleri almak için bir alıcı tanımlayın. Aşağıdaki alıcı tanımını AndroidManifest.xml dosyasına `<application>` etiketinin içine ekleyin. `<your package>` yer tutucusunu, `AndroidManifest.xml` dosyasının üst kısmında gösterilen asıl paket adınızla değiştirin.

    ```xml
    <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
        android:permission="com.google.android.c2dm.permission.SEND">
        <intent-filter>
            <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            <category android:name="<your package name>" />
        </intent-filter>
    </receiver>
    ```

4. Aşağıdaki gerekli GCM izinlerini `<application>` etiketinin altına ekleyin. `<your package>` öğesini, `AndroidManifest.xml` dosyasının üst kısmında gösterilen paket adıyla değiştirin.

    Bu izinler hakkında daha fazla bilgi için bkz. [Android IÇIN GCM istemci uygulaması ayarlama](https://developers.google.com/cloud-messaging/).

    ```xml
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
    <uses-permission android:name="android.permission.WAKE_LOCK"/>
    <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />

    <permission android:name="<your package>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
    <uses-permission android:name="<your package>.permission.C2D_MESSAGE"/>
    ```

### <a name="adding-code"></a>Kod ekleme

1. Proje görünümünde, **uygulama**  >  **src**  >  **Main**  >  **Java**' yı genişletin. **Java**'nın altındaki paket klasörünüze sağ tıklayın, **Yeni**'ye tıklayın ve ardından **Java Sınıfı**'na tıklayın. `NotificationSettings` adlı yeni bir sınıf ekleyin.

    ![Android Studio - yeni Java sınıfı][6]

    Aşağıdaki kodda `NotificationSettings` sınıfı için üç yer tutucuyu güncelleştirin:

   * `SenderId`: [Google Cloud konsolunda](https://cloud.google.com/console)daha önce edindiğiniz proje numarası.
   * `HubListenConnectionString`: Hub 'ınız için **Defaultlistenaccesssignature** bağlantı dizesi. [Azure portalında] hub’ınızın **Ayarlar** sayfasında bulunan **Erişim İlkeleri**’ne tıklayarak bağlantı dizesini kopyalayabilirsiniz.
   * `HubName`: [Azure Portal]hub sayfasında görünen Bildirim Hub 'ınızın adını kullanın.

     `NotificationSettings` kodu:

     ```java
     public class NotificationSettings {
        public static String SenderId = "<Your project number>";
        public static String HubName = "<Your HubName>";
        public static String HubListenConnectionString = "<Your default listen connection string>";
     }
     ```

2. `MyInstanceIDService` adlı başka bir yeni sınıf ekleyin. Bu sınıf, Örnek Kimliği dinleyici hizmeti uygulamasıdır.

    Bu sınıfın kodu, arka planda [GCM belirtecini yenilemek](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) için `IntentService` çağırır.

    ```java
    import android.content.Intent;
    import android.util.Log;
    import com.google.android.gms.iid.InstanceIDListenerService;

    public class MyInstanceIDService extends InstanceIDListenerService {

        private static final String TAG = "MyInstanceIDService";

        @Override
        public void onTokenRefresh() {

            Log.i(TAG, "Refreshing GCM Registration Token");

            Intent intent = new Intent(this, RegistrationIntentService.class);
            startService(intent);
        }
    };
    ```

3. Projenize adlı başka bir yeni sınıf ekleyin `RegistrationIntentService` . Bu sınıf, [GCM belirtecini yenileme](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) ve [bildirim hub’ına kaydolma](notification-hubs-push-notification-registration-management.md) işlemlerini gerçekleştiren `IntentService` arabirimini uygular.

    Bu sınıf için aşağıdaki kod kullanın.

    ```java
    import android.app.IntentService;
    import android.content.Intent;
    import android.content.SharedPreferences;
    import android.preference.PreferenceManager;
    import android.util.Log;

    import com.google.android.gms.gcm.GoogleCloudMessaging;
    import com.google.android.gms.iid.InstanceID;
    import com.microsoft.windowsazure.messaging.NotificationHub;

    public class RegistrationIntentService extends IntentService {

        private static final String TAG = "RegIntentService";

        private NotificationHub hub;

        public RegistrationIntentService() {
            super(TAG);
        }

        @Override
        protected void onHandleIntent(Intent intent) {
            SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this);
            String resultString = null;
            String regID = null;

            try {
                InstanceID instanceID = InstanceID.getInstance(this);
                String token = instanceID.getToken(NotificationSettings.SenderId,
                        GoogleCloudMessaging.INSTANCE_ID_SCOPE);
                Log.i(TAG, "Got GCM Registration Token: " + token);

                // Storing the registration id that indicates whether the generated token has been
                // sent to your server. If it is not stored, send the token to your server,
                // otherwise your server should have already received the token.
                if ((regID=sharedPreferences.getString("registrationID", null)) == null) {
                    NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                            NotificationSettings.HubListenConnectionString, this);
                    Log.i(TAG, "Attempting to register with NH using token : " + token);

                    regID = hub.register(token).getRegistrationId();

                    // If you want to use tags...
                    // Refer to : https://azure.microsoft.com/documentation/articles/notification-hubs-routing-tag-expressions/
                    // regID = hub.register(token, "tag1", "tag2").getRegistrationId();

                    resultString = "Registered Successfully - RegId : " + regID;
                    Log.i(TAG, resultString);
                    sharedPreferences.edit().putString("registrationID", regID ).apply();
                } else {
                    resultString = "Previously Registered Successfully - RegId : " + regID;
                }
            } catch (Exception e) {
                Log.e(TAG, resultString="Failed to complete token refresh", e);
                // If an exception happens while fetching the new token or updating the registration data
                // on a third-party server, this ensures that we'll attempt the update at a later time.
            }

            // Notify UI that registration has completed.
            if (MainActivity.isVisible) {
                MainActivity.mainActivity.ToastNotify(resultString);
            }
        }
    }
    ```

4. `MainActivity` sınıfınızda, aşağıdaki `import` deyimlerini sınıfın başına ekleyin.

    ```java
    import com.google.android.gms.common.ConnectionResult;
    import com.google.android.gms.common.GoogleApiAvailability;
    import com.google.android.gms.gcm.*;
    import com.microsoft.windowsazure.notifications.NotificationsManager;
    import android.util.Log;
    import android.widget.TextView;
    import android.widget.Toast;
    import android.content.Intent;
    ```

5. Aşağıdaki özel üyeleri sınıfın en üst kısmına ekleyin. Bu kod, [Google Play Hizmetleri’nin kullanılabilirliğini Google tarafından önerildiği şekilde denetler](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).

    ```java
    public static MainActivity mainActivity;
    public static Boolean isVisible = false;
    private GoogleCloudMessaging gcm;
    private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;
    private static final String TAG = "MainActivity";
    ```

6. `MainActivity` sınıfınızda Google Play Hizmetleri'nin kullanılabilirliğine aşağıdaki yöntemi ekleyin.

    ```java
    /**
        * Check the device to make sure it has the Google Play Services APK. If
        * it doesn't, display a dialog that allows users to download the APK from
        * the Google Play Store or enable it in the device's system settings.
        */
    private boolean checkPlayServices() {
        GoogleApiAvailability apiAvailability = GoogleApiAvailability.getInstance();
        int resultCode = apiAvailability.isGooglePlayServicesAvailable(this);
        if (resultCode != ConnectionResult.SUCCESS) {
            if (apiAvailability.isUserResolvableError(resultCode)) {
                apiAvailability.getErrorDialog(this, resultCode, PLAY_SERVICES_RESOLUTION_REQUEST)
                        .show();
            } else {
                Log.i(TAG, "This device is not supported by Google Play Services.");
                ToastNotify("This device is not supported by Google Play Services.");
                finish();
            }
            return false;
        }
        return true;
    }
    ```

7. `MainActivity` sınıfınızda, GCM kayıt belirtecinizi almak ve bildirim hub’ınıza kaydolmak için `IntentService` hizmetinizi çağırmadan önce Google Play Hizmetleri’ni denetleyen aşağıdaki kodu ekleyin.

    ```java
    public void registerWithNotificationHubs()
    {
        Log.i(TAG, " Registering with Notification Hubs");

        if (checkPlayServices()) {
            // Start IntentService to register this application with GCM.
            Intent intent = new Intent(this, RegistrationIntentService.class);
            startService(intent);
        }
    }
    ```

8. `MainActivity` sınıfının `OnCreate` yönteminde, etkinlik oluşturulduğunda kayıt işlemini başlatmak için aşağıdaki kodu ekleyin.

    ```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mainActivity = this;
        NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
        registerWithNotificationHubs();
    }
    ```

9. Uygulama durumunu doğrulamak ve uygulamanızda durumu raporlamak için bu ek yöntemleri `MainActivity` öğesine ekleyin.

    ```java
    @Override
    protected void onStart() {
        super.onStart();
        isVisible = true;
    }

    @Override
    protected void onPause() {
        super.onPause();
        isVisible = false;
    }

    @Override
    protected void onResume() {
        super.onResume();
        isVisible = true;
    }

    @Override
    protected void onStop() {
        super.onStop();
        isVisible = false;
    }

    public void ToastNotify(final String notificationMessage) {
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                Toast.makeText(MainActivity.this, notificationMessage, Toast.LENGTH_LONG).show();
                TextView helloText = (TextView) findViewById(R.id.text_hello);
                helloText.setText(notificationMessage);
            }
        });
    }
    ```

10. `ToastNotify` yöntemi, uygulamada kalıcı olarak durumu ve bildirimleri raporlamak için *"Hello World"* `TextView` denetimini kullanır. activity_main.xml düzeninizde bu denetim için aşağıdaki kimliği ekleyin.

    ```xml
    android:id="@+id/text_hello"
    ```

11. AndroidManifest.xml’de tanımlanan alıcı için bir alt sınıf ekleyin. `MyHandler` adlı projenize başka bir yeni sınıf ekleyin.

12. `MyHandler.java`'in üst kısmına şu içeri aktarma deyimlerini ekleyin:

    ```java
    import android.app.NotificationManager;
    import android.app.PendingIntent;
    import android.content.Context;
    import android.content.Intent;
    import android.os.Bundle;
    import android.support.v4.app.NotificationCompat;
    import com.microsoft.windowsazure.notifications.NotificationsHandler;
    import android.net.Uri;
    import android.media.RingtoneManager;
    ```

13. Sınıfı için aşağıdaki kodu ekleyin `MyHandler` , bunun bir alt sınıfı haline gelir `com.microsoft.windowsazure.notifications.NotificationsHandler` .

    Bu kod, `OnReceive` yöntemini geçersiz kılar; böylece işleyici alınan bildirimleri raporlar. İşleyici, `sendNotification()` yöntemini kullanarak Android bildirim yöneticisine anında iletme bildirimi gönderir. `sendNotification()` yöntemi, uygulama çalışmıyorken ve bir bildirim alındığında yürütülmelidir.

    ```java
    public class MyHandler extends NotificationsHandler {
        public static final int NOTIFICATION_ID = 1;
        private NotificationManager mNotificationManager;
        NotificationCompat.Builder builder;
        Context ctx;

        @Override
        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String nhMessage = bundle.getString("message");
            sendNotification(nhMessage);
            if (MainActivity.isVisible) {
                MainActivity.mainActivity.ToastNotify(nhMessage);
            }
        }

        private void sendNotification(String msg) {

            Intent intent = new Intent(ctx, MainActivity.class);
            intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);

            mNotificationManager = (NotificationManager)
                    ctx.getSystemService(Context.NOTIFICATION_SERVICE);

            PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                    intent, PendingIntent.FLAG_ONE_SHOT);

            Uri defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
            NotificationCompat.Builder mBuilder =
                    new NotificationCompat.Builder(ctx)
                            .setSmallIcon(R.mipmap.ic_launcher)
                            .setContentTitle("Notification Hub Demo")
                            .setStyle(new NotificationCompat.BigTextStyle()
                                    .bigText(msg))
                            .setSound(defaultSoundUri)
                            .setContentText(msg);

            mBuilder.setContentIntent(contentIntent);
            mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
        }
    }
    ```

14. **Build**  >  Kodunuzda bir hata olmadığından emin olmak için, menü çubuğunda Android Studio oluştur**projeyi** derle ' ye tıklayın.

## <a name="testing-your-app"></a>Uygulamanızı test etme

### <a name="run-the-mobile-application"></a>Mobil uygulamayı çalıştırma

1. Uygulamayı çalıştırın ve kayıt kimliğinin başarılı bir kaydı bildirdiğine dikkat edin.

      ![Android'de test etme - Kanal kaydı][18]
2. Hub'da kayıtlı tüm Android cihazlara gönderilecek bir bildirim iletisi girin.

      ![Andorid'de test etme - bir ileti gönderme][19]

3. **Bildirim Gönder**'e basın. Uygulamayı çalıştıran tüm cihazlarda, anında iletme bildirimi iletisiyle birlikte bir `AlertDialog` örneği görünür. Uygulamayı çalıştırmayan ancak anında iletme bildirimleri için daha önce kaydolan cihazlar, Android Bildirim Yöneticisi'nde bir bildirim alır. Bildirim iletileri, sol üst köşeden aşağı çekilerek görüntülenebilir.

      ![Android'de test etme - bildirimler][21]

### <a name="test-send-push-notifications-from-the-azure-portal"></a>Azure portalından anında iletme bildirimleri göndermeyi test etme

[Azure portalında] anında iletme bildirimleri göndererek uygulamanızda anında iletme bildirimi alma testi gerçekleştirebilirsiniz.

1. **Sorun Giderme** bölümünde **Test Gönderimi**’ni seçin.
2. **Platformlar** için **Android**’i seçin.
3. **Gönder**’i seçerek test bildirimini gönderin.
4. Android cihazında bildirim iletisini gördüğünüzü onaylayın.

    ![Azure Notification Hubs - Test Gönderimi](./media/notification-hubs-android-get-started/notification-hubs-test-send.png)

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

#### <a name="push-notifications-in-the-emulator"></a>Öykünücüde anında iletme bildirimleri

Anında iletme bildirimlerini bir öykünücüde test etmek isterseniz öykünücü görüntünüzün, uygulamanız için seçtiğiniz Google API düzeyini desteklediğinden emin olun. Görüntünüz yerel Google API'lerini desteklemiyorsa **HİZMET\_\_KULLANILAMIYOR** özel durumuyla karşılaşırsınız.

Ayrıca, Google hesabınızı, **Ayarlar**hesaplar bölümünde çalışan öykünücüsünüzün eklendiğinden emin olun  >  **Accounts**. Aksi halde, GCM ile kayıt girişimleriniz **KİMLİK DOĞRULAMASI\_BAŞARISIZ** özel durumuyla sonuçlanabilir.

## <a name="optional-send-push-notifications-directly-from-the-app"></a>(İsteğe bağlı) Doğrudan uygulamadan anında iletme bildirimleri gönderme

Normalde bildirimleri bir arka uç sunucusu kullanarak gönderirsiniz. Bazı durumlarda, anında iletme bildirimlerini doğrudan istemci uygulamasından gönderebilmek isteyebilirsiniz. Bu bölüm, [Azure Notification Hubs REST API'si](/previous-versions/azure/reference/dn223264(v=azure.100)) kullanarak istemciden nasıl bildirim gönderildiğini açıklamaktadır.

1. Android Studio projesi görünümünde, **uygulama**  >  **src**  >  **ana**  >  **res**  >  **düzeni**' ni genişletin. `activity_main.xml` düzen dosyasını açın ve dosyanın metin içeriğini güncelleştirmek için **Metin** sekmesine tıklayın. Bildirim hub'ına anında iletme bildirimi iletileri göndermek için yeni `Button` ve `EditText` denetimlerini ekleyen aşağıdaki kod ile güncelleştirin. Bu kodu en alta, `</RelativeLayout>` öğesinin hemen önüne ekleyin.

    ```xml
    <Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/send_button"
    android:id="@+id/sendbutton"
    android:layout_centerVertical="true"
    android:layout_centerHorizontal="true"
    android:onClick="sendNotificationButtonOnClick" />

    <EditText
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:id="@+id/editTextNotificationMessage"
    android:layout_above="@+id/sendbutton"
    android:layout_centerHorizontal="true"
    android:layout_marginBottom="42dp"
    android:hint="@string/notification_message_hint" />
    ```

2. Android Studio projesi görünümünde, **uygulama**  >  **src**  >  **ana**  >  **res**  >  **değerleri**' ni genişletin. `strings.xml` dosyasını açın ve yeni `Button` ve `EditText` denetimleri tarafından başvurulan dize değerlerini ekleyin. Aşağıdaki satırları dosyanın en altına, `</resources>` öğesinin hemen önüne ekleyin.

    ```xml
    <string name="send_button">Send Notification</string>
    <string name="notification_message_hint">Enter notification message text</string>
    ```

3. `NotificationSetting.java` dosyanızda `NotificationSettings` sınıfına aşağıdaki ayarı ekleyin.

    `HubFullAccess` öğesini, **DefaultFullSharedAccessSignature** bağlantı dizesiyle hub'ınız için güncelleştirin. Bu bağlantı dizesi, bildirim hub’ınızın **Ayarlar** sayfasında **Erişim İlkeleri**’ne tıklanarak [Azure portalında] kopyalanabilir.

    ```java
    public static String HubFullAccess = "<Enter Your DefaultFullSharedAccess Connection string>";
    ```

4. `MainActivity.java` dosyanızda, aşağıdaki `import` deyimlerini dosyanın başına ekleyin.

    ```java
    import java.io.BufferedOutputStream;
    import java.io.BufferedReader;
    import java.io.InputStreamReader;
    import java.io.OutputStream;
    import java.net.HttpURLConnection;
    import java.net.URL;
    import java.net.URLEncoder;
    import javax.crypto.Mac;
    import javax.crypto.spec.SecretKeySpec;
    import android.util.Base64;
    import android.view.View;
    import android.widget.EditText;
    ```

5. `MainActivity.java` dosyanızda aşağıdaki üyeleri `MainActivity` sınıfının üstüne ekleyin.

    ```java
    private String HubEndpoint = null;
    private String HubSasKeyName = null;
    private String HubSasKeyValue = null;
    ```

6. Bildirim Hub 'ınıza ileti göndermek için bir POST isteğinin kimliğini doğrulamak üzere bir paylaşılan erişim Imzası (SaS) belirteci oluşturun. Anahtar verilerini bağlantı dizesinden ayrıştırıp ardından [Ortak Kavramlar](/previous-versions/azure/reference/dn495627(v=azure.100)) REST API’si başvurusunda belirtildiği gibi SaS belirtecini oluşturun. Aşağıdaki kod örnek bir uygulamadır.

    Bağlantı dizenizi ayrıştırmak için `MainActivity.java` öğesinde aşağıdaki yöntemi `MainActivity` sınıfına ekleyin.

    ```java
    /**
        * Example code from https://msdn.microsoft.com/library/azure/dn495627.aspx
        * to parse the connection string so a SaS authentication token can be
        * constructed.
        *
        * @param connectionString This must be the DefaultFullSharedAccess connection
        *                         string for this example.
        */
    private void ParseConnectionString(String connectionString)
    {
        String[] parts = connectionString.split(";");
        if (parts.length != 3)
            throw new RuntimeException("Error parsing connection string: "
                    + connectionString);

        for (int i = 0; i < parts.length; i++) {
            if (parts[i].startsWith("Endpoint")) {
                this.HubEndpoint = "https" + parts[i].substring(11);
            } else if (parts[i].startsWith("SharedAccessKeyName")) {
                this.HubSasKeyName = parts[i].substring(20);
            } else if (parts[i].startsWith("SharedAccessKey")) {
                this.HubSasKeyValue = parts[i].substring(16);
            }
        }
    }
    ```

7. Bir SaS kimlik doğrulaması belirteci oluşturmak için `MainActivity.java` öğesinde aşağıdaki yöntemi `MainActivity` sınıfına ekleyin.

    ```java
    /**
        * Example code from https://msdn.microsoft.com/library/azure/dn495627.aspx to
        * construct a SaS token from the access key to authenticate a request.
        *
        * @param uri The unencoded resource URI string for this operation. The resource
        *            URI is the full URI of the Service Bus resource to which access is
        *            claimed. For example,
        *            "http://<namespace>.servicebus.windows.net/<hubName>"
        */
    private String generateSasToken(String uri) {

        String targetUri;
        String token = null;
        try {
            targetUri = URLEncoder
                    .encode(uri.toString().toLowerCase(), "UTF-8")
                    .toLowerCase();

            long expiresOnDate = System.currentTimeMillis();
            int expiresInMins = 60; // 1 hour
            expiresOnDate += expiresInMins * 60 * 1000;
            long expires = expiresOnDate / 1000;
            String toSign = targetUri + "\n" + expires;

            // Get an hmac_sha1 key from the raw key bytes
            byte[] keyBytes = HubSasKeyValue.getBytes("UTF-8");
            SecretKeySpec signingKey = new SecretKeySpec(keyBytes, "HmacSHA256");

            // Get an hmac_sha1 Mac instance and initialize with the signing key
            Mac mac = Mac.getInstance("HmacSHA256");
            mac.init(signingKey);

            // Compute the hmac on input data bytes
            byte[] rawHmac = mac.doFinal(toSign.getBytes("UTF-8"));

            // Using android.util.Base64 for Android Studio instead of
            // Apache commons codec
            String signature = URLEncoder.encode(
                    Base64.encodeToString(rawHmac, Base64.NO_WRAP).toString(), "UTF-8");

            // Construct authorization string
            token = "SharedAccessSignature sr=" + targetUri + "&sig="
                    + signature + "&se=" + expires + "&skn=" + HubSasKeyName;
        } catch (Exception e) {
            if (isVisible) {
                ToastNotify("Exception Generating SaS : " + e.getMessage().toString());
            }
        }

        return token;
    }
    ```

8. **Bildirim Gönder** düğmesine tıklanmasını ele almak ve yerleşik REST API'sini kullanarak hub'a anında iletme bildirimi iletisi göndermek için `MainActivity.java` öğesinde `MainActivity` sınıfına aşağıdaki yöntemi ekleyin.

    ```java
    /**
        * Send Notification button click handler. This method parses the
        * DefaultFullSharedAccess connection string and generates a SaS token. The
        * token is added to the Authorization header on the POST request to the
        * notification hub. The text in the editTextNotificationMessage control
        * is added as the JSON body for the request to add a GCM message to the hub.
        *
        * @param v
        */
    public void sendNotificationButtonOnClick(View v) {
        EditText notificationText = (EditText) findViewById(R.id.editTextNotificationMessage);
        final String json = "{\"data\":{\"message\":\"" + notificationText.getText().toString() + "\"}}";

        new Thread()
        {
            public void run()
            {
                try
                {
                    // Based on reference documentation...
                    // https://msdn.microsoft.com/library/azure/dn223273.aspx
                    ParseConnectionString(NotificationSettings.HubFullAccess);
                    URL url = new URL(HubEndpoint + NotificationSettings.HubName +
                            "/messages/?api-version=2015-01");

                    HttpURLConnection urlConnection = (HttpURLConnection)url.openConnection();

                    try {
                        // POST request
                        urlConnection.setDoOutput(true);

                        // Authenticate the POST request with the SaS token
                        urlConnection.setRequestProperty("Authorization",
                            generateSasToken(url.toString()));

                        // Notification format should be GCM
                        urlConnection.setRequestProperty("ServiceBusNotification-Format", "gcm");

                        // Include any tags
                        // Example below targets 3 specific tags
                        // Refer to : https://azure.microsoft.com/documentation/articles/notification-hubs-routing-tag-expressions/
                        // urlConnection.setRequestProperty("ServiceBusNotification-Tags",
                        //        "tag1 || tag2 || tag3");

                        // Send notification message
                        urlConnection.setFixedLengthStreamingMode(json.length());
                        OutputStream bodyStream = new BufferedOutputStream(urlConnection.getOutputStream());
                        bodyStream.write(json.getBytes());
                        bodyStream.close();

                        // Get response
                        urlConnection.connect();
                        int responseCode = urlConnection.getResponseCode();
                        if ((responseCode != 200) && (responseCode != 201)) {
                            BufferedReader br = new BufferedReader(new InputStreamReader((urlConnection.getErrorStream())));
                            String line;
                            StringBuilder builder = new StringBuilder("Send Notification returned " +
                                    responseCode + " : ")  ;
                            while ((line = br.readLine()) != null) {
                                builder.append(line);
                            }

                            ToastNotify(builder.toString());
                        }
                    } finally {
                        urlConnection.disconnect();
                    }
                }
                catch(Exception e)
                {
                    if (isVisible) {
                        ToastNotify("Exception Sending Notification : " + e.getMessage().toString());
                    }
                }
            }
        }.start();
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, arka uca kayıtlı olan tüm Android cihazlarınıza yayın bildirimleri gönderdiniz. Belirli Android cihazlara nasıl anında iletme bildirimleri gönderileceğini öğrenmek için aşağıdaki öğreticiye ilerleyin:  

 > [!div class="nextstepaction"]
 > [Belirli cihazlara anında iletme bildirimleri gönderme](notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md)

<!-- Images. -->
[6]: ./media/notification-hubs-android-get-started/notification-hub-android-new-class.png
[12]: ./media/notification-hubs-android-get-started/notification-hub-connection-strings.png
[13]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-new-project.png
[14]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-choose-form-factor.png
[15]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app4.png
[16]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app5.png
[17]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app6.png
[18]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-registered.png
[19]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-set-message.png
[20]: ./media/notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-received-message.png
[22]: ./media/notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/notification-hubs-android-get-started/notification-hub-scheduler2.png
[29]: ./media/mobile-services-android-get-started-push/mobile-eclipse-import-Play-library.png
[30]: ./media/notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png
[31]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-add-ui.png

<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: https://go.microsoft.com/fwlink/?LinkId=389800
[Notification Hubs Guidance]: /previous-versions/azure/azure-services/jj927170(v=azure.100)
[Use Notification Hubs to push notifications to users]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[Use Notification Hubs to send breaking news]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[Azure Portal]: https://portal.azure.com
