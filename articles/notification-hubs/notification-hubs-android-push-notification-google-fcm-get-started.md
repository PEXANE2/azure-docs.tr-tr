---
title: Azure Bildirim Hub'larını ve Firebase'i kullanarak Android'e anında iletme bildirimleri gönderme | Microsoft Dokümanlar
description: Bu öğreticide, Android cihazlarına anında iletme bildirimleri göndermek için Azure Notification Hubs ve Google Firebase Cloud Messaging’in nasıl kullanılacağını öğrenirsiniz.
services: notification-hubs
documentationcenter: android
keywords: anında iletme bildirimleri,anında iletme bildirimi,android anında iletme bildirimi,fcm,firebase cloud messaging
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: 02298560-da61-4bbb-b07c-e79bd520e420
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: tutorial
ms.custom: mvc
ms.date: 09/11/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 09/11/2019
ms.openlocfilehash: e0682a33540cafd8ebab1bb248d462b30ee4dc2a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80127532"
---
# <a name="tutorial-send-push-notifications-to-android-devices-using-firebase"></a>Öğretici: Firebase kullanarak Android cihazlara anında iletme bildirimleri gönderme

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

Bu öğretici, bir Android uygulamasına bildirim leri iletmek için Azure Bildirim Hub'larını ve Firebase Bulut Mesajlaşmasını (FCM) nasıl kullanacağınızı gösterir. Bu öğreticide Firebase Cloud Messaging (FCM) kullanarak anında iletme bildirimleri alan bir Android uygulaması oluşturacaksınız.

Bu öğretici için tamamlanmış kod [GitHub](https://github.com/Azure/azure-notificationhubs-android/tree/master/samples/FCMTutorialApp)indirilebilir.

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Android Studio projesi oluşturma.
> * Firebase Cloud Messaging’i destekleyen bir Firebase projesi oluşturma.
> * Bir hub oluşturun.
> * Uygulamanızı hub'a bağlayın.
> * Uygulamayı test etme.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/free/). 

Ayrıca aşağıdaki öğelere de ihtiyacınız vardır: 

* [Android Studio](https://go.microsoft.com/fwlink/?LinkId=389797) en son sürümü
* Firebase Cloud Mesajlaşma için Android 2.3 veya üzeri
* Firebase Cloud Mesajlaşma için Google Depo revizyonu 27 veya daha yüksek
* Firebase Bulut Mesajlaşması için Google Play Hizmetleri 9.0.2 veya üzeri

Bu öğreticiyi tamamlamak, Android uygulamaları için diğer tüm Bildirim Hub'ları eğitimlerini yapmak için bir ön koşuldur.

## <a name="create-an-android-studio-project"></a>Android Studio Projesi oluşturma

1. Android Studio'yı başlatın.
2. **Dosya'yı**seçin , **Yeni'ye**işaret edin ve ardından **Yeni Proje'yi**seçin. 
2. Proje **sayfanızı seçin,** **Boş Etkinlik'i**seçin ve sonra **İleri'yi**seçin. 
3. Proje **sayfanızı Yapılandır'da** aşağıdaki adımları izleyin: 
    1. Uygulama için bir ad girin.
    2. Proje dosyalarını kaydetmek için bir konum belirtin. 
    3. **Bitiş'i**seçin. 

        ![Projenizi yapılandırın)](./media/notification-hubs-android-push-notification-google-fcm-get-started/configure-project.png)

## <a name="create-a-firebase-project-that-supports-fcm"></a>FCM’yi destekleyen bir Firebase projesi oluşturma

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-hub"></a>Hub'ı yapılandırma

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

### <a name="configure-firebase-cloud-messaging-settings-for-the-hub"></a>Hub için Firebase Cloud Messaging ayarlarını yapılandırma

1. Sol bölmede, **Ayarlar** altında **Google (GCM/FCM)** seçeneğini belirleyin. 
2. Daha önce kaydettiğiniz FCM projesinin **sunucu anahtarını** girin. 
3. Araç çubuğunda **Kaydet'i**seçin. 

    ![Azure Bildirim Merkezi - Google (FCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/fcm-server-key.png)
4. Azure portalı, hub'ın başarıyla güncelleştirildiğini belirten uyarılarda bir ileti görüntüler. **Kaydet** düğmesi devre dışı bırakılır. 

Hub'ınız artık Firebase Cloud Messaging ile çalışacak şekilde yapılandırılmıştır. Ayrıca, bir cihaza bildirim göndermek ve bildirimleri almak için bir uygulamayı kaydetmek için gerekli olan bağlantı dizelerine de sahipsiniz.

## <a name="connect-your-app-to-the-notification-hub"></a><a id="connecting-app"></a>Uygulamanızı bildirim merkezine bağlayın

### <a name="add-google-play-services-to-the-project"></a>Projeye Google Play hizmetlerini ekleme

1. Android Studio'da menüdeki **Araçlar'ı** seçin ve ardından **SDK Yöneticisi'ni**seçin. 
2. Projenizde kullanılan Android SDK'nın hedef sürümünü seçin. Ardından **Paket Ayrıntılarını Göster'i**seçin. 

    ![Android SDK Manager - hedef sürümü seçin](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. Zaten yüklü değilse, **Google API'leri**seçin.

    ![Android SDK Yöneticisi - Google API'ler seçildi](./media/notification-hubs-android-studio-add-google-play-services/googole-apis-selected.png)
4. **SDK Araçları** sekmesine geçin. Google Play Hizmetleri'ni henüz yüklemediyseniz, aşağıdaki resimde gösterildiği gibi **Google Play Hizmetleri'ni** seçin. Ardından yüklemek için **Uygula'yı** seçin. SDK yolunun sonraki bir adım için olduğunu unutmayın.

    ![Android SDK Yöneticisi - Google Play Hizmetleri seçildi](./media/notification-hubs-android-studio-add-google-play-services/google-play-services-selected.png)
3. **Değiştir'i onayla** iletişim kutusunu **görüyorsanız, Tamam'ı**seçin. Bileşen Yükleyici istenen bileşenleri yükler. Bileşenler yüklendikten sonra **Bitir'i** seçin.
4. **Yeni Projeler için Ayarlar** iletişim kutusunu kapatmak için **Tamam'ı** seçin.  
1. AndroidManifest.xml dosyasını açın ve *ardından uygulama* etiketine aşağıdaki etiketi ekleyin.

    ```xml
    <meta-data android:name="com.google.android.gms.version"
         android:value="@integer/google_play_services_version" />
    ```


### <a name="add-azure-notification-hubs-libraries"></a>Azure Bildirim Hub'ları kitaplıkları ekleme

1. Uygulama için Build.Gradle dosyasına bağımlılıklar bölümünde aşağıdaki satırları ekleyin.

    ```gradle
    implementation 'com.microsoft.azure:notification-hubs-android-sdk:0.6@aar'
    ```

2. Bağımlılıklar bölümünden sonra aşağıdaki depoyu ekleyin.

    ```gradle
    repositories {
        maven {
            url "https://dl.bintray.com/microsoftazuremobile/SDK"
        }
    }
    ```

### <a name="add-google-firebase-support"></a>Google Firebase desteği ekleme

1. Uygulama için Build.Gradle dosyasında, zaten yoksa **bağımlılıklar** bölümündeaşağıdaki satırları ekleyin. 

    ```gradle
    implementation 'com.google.firebase:firebase-core:16.0.8'
    implementation 'com.google.firebase:firebase-messaging:17.3.4'
    ```

2. Dosyanın sonuna, zaten orada değilse aşağıdaki eklentiyi ekleyin. 

    ```gradle
    apply plugin: 'com.google.gms.google-services'
    ```
3. Araç çubuğunda **Şimdi Eşitle'yi** seçin.

### <a name="update-the-androidmanifestxml-file"></a>AndroidManifest.xml dosyasını güncelleştirin

1. FCM kayıt belirtecinizi aldıktan sonra, [azure bildirim hub'larına kaydolmak için kullanırsınız.](notification-hubs-push-notification-registration-management.md) Bu kaydı arka planda `IntentService` bir adlandırılmış `RegistrationIntentService`kullanarak desteklersiniz. Bu hizmet aynı zamanda FCM kayıt belirtecinizi de yeniler. Ayrıca, bildirimleri `FirebaseService` almak `FirebaseMessagingService` ve işlemek için `onMessageReceived` yöntemi alt sınıf olarak adlandırılan bir sınıf oluşturur ve geçersiz kılarsınız. 

    Aşağıdaki hizmet tanımını AndroidManifest.xml dosyasında `<application>` etiketinin içine ekleyin.

    ```xml
    <service
        android:name=".RegistrationIntentService"
        android:exported="false">
    </service>
    <service
        android:name=".FirebaseService"
        android:exported="false">
        <intent-filter>
            <action android:name="com.google.firebase.MESSAGING_EVENT" />
        </intent-filter>
    </service>
    ```
3. `</application>` Etiketin altına FCM ile ilgili gerekli izinleri ekleyin.

    ```xml
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
    <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    ```

### <a name="add-code"></a>Kod ekleme

1. Proje Görünümünde, **app** > **src** > **ana** > **java**genişletin. **Java**altında paket klasörünüzü sağ tıklatın, **Yeni'yi**seçin ve ardından **Java Sınıfı'nı**seçin. Ad için **Bildirim Ayarları'nı** girin ve ardından **Tamam'ı**seçin.

    Aşağıdaki kodda `NotificationSettings` sınıfı için bu üç yer tutucuyu güncelleştirdiğinizden emin olun:

   * **HubListenConnectionString**: Hub'ınız için **DefaultListenAccessSignature** bağlantı dizesi. [Azure portalındaki]merkezinizdeki **Erişim İlkeleri'ni** tıklatarak bu bağlantı dizesini kopyalayabilirsiniz.
   * **HubName**: [Azure portalındaki]hub sayfasında görünen hub'ınızın adını kullanın.

     `NotificationSettings` kodu:

        ```java
        public class NotificationSettings {
            public static String HubName = "<Your HubName>";
            public static String HubListenConnectionString = "<Enter your DefaultListenSharedAccessSignature connection string>";
        }
        ```

     > [!IMPORTANT]
     > Daha fazla ilerlemeden önce hub'ınızın **adını** ve **DefaultListenSharedAccessSignature'ını** girin. 

2. `RegistrationIntentService` adlı projenize başka bir yeni sınıf ekleyin. Bu sınıf `IntentService` arabirimi uygular. Ayrıca [FCM belirteci yenileme](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) ve [bildirim hub'ı ile kayıt](notification-hubs-push-notification-registration-management.md)işler.

    Bu sınıf için aşağıdaki kod kullanın.

    ```java
    import android.app.IntentService;
    import android.content.Intent;
    import android.content.SharedPreferences;
    import android.preference.PreferenceManager;
    import android.util.Log;
    import com.google.android.gms.tasks.OnSuccessListener;
    import com.google.firebase.iid.FirebaseInstanceId;
    import com.google.firebase.iid.InstanceIdResult;
    import com.microsoft.windowsazure.messaging.NotificationHub;
    import java.util.concurrent.TimeUnit;

    public class RegistrationIntentService extends IntentService {

        private static final String TAG = "RegIntentService";
        String FCM_token = null;

        private NotificationHub hub;

        public RegistrationIntentService() {
            super(TAG);
        }

        @Override
        protected void onHandleIntent(Intent intent) {

            SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this);
            String resultString = null;
            String regID = null;
            String storedToken = null;

            try {
                FirebaseInstanceId.getInstance().getInstanceId().addOnSuccessListener(new OnSuccessListener<InstanceIdResult>() { 
                    @Override 
                    public void onSuccess(InstanceIdResult instanceIdResult) { 
                        FCM_token = instanceIdResult.getToken(); 
                        Log.d(TAG, "FCM Registration Token: " + FCM_token); 
                    } 
                }); 
                TimeUnit.SECONDS.sleep(1);

                // Storing the registration ID that indicates whether the generated token has been
                // sent to your server. If it is not stored, send the token to your server.
                // Otherwise, your server should have already received the token.
                if (((regID=sharedPreferences.getString("registrationID", null)) == null)){

                    NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                            NotificationSettings.HubListenConnectionString, this);
                    Log.d(TAG, "Attempting a new registration with NH using FCM token : " + FCM_token);
                    regID = hub.register(FCM_token).getRegistrationId();

                    // If you want to use tags...
                    // Refer to : https://azure.microsoft.com/documentation/articles/notification-hubs-routing-tag-expressions/
                    // regID = hub.register(token, "tag1,tag2").getRegistrationId();

                    resultString = "New NH Registration Successfully - RegId : " + regID;
                    Log.d(TAG, resultString);

                    sharedPreferences.edit().putString("registrationID", regID ).apply();
                    sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                }

                // Check to see if the token has been compromised and needs refreshing.
                else if ((storedToken=sharedPreferences.getString("FCMtoken", "")) != FCM_token) {

                    NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                            NotificationSettings.HubListenConnectionString, this);
                    Log.d(TAG, "NH Registration refreshing with token : " + FCM_token);
                    regID = hub.register(FCM_token).getRegistrationId();

                    // If you want to use tags...
                    // Refer to : https://azure.microsoft.com/documentation/articles/notification-hubs-routing-tag-expressions/
                    // regID = hub.register(token, "tag1,tag2").getRegistrationId();

                    resultString = "New NH Registration Successfully - RegId : " + regID;
                    Log.d(TAG, resultString);

                    sharedPreferences.edit().putString("registrationID", regID ).apply();
                    sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                }

                else {
                    resultString = "Previously Registered Successfully - RegId : " + regID;
                }
            } catch (Exception e) {
                Log.e(TAG, resultString="Failed to complete registration", e);
                // If an exception happens while fetching the new token or updating registration data
                // on a third-party server, this ensures that we'll attempt the update at a later time.
            }

            // Notify UI that registration has completed.
            if (MainActivity.isVisible) {
                MainActivity.mainActivity.ToastNotify(resultString);
            }
        }
    }
    ```

3. `MainActivity` Sınıfta, sınıf bildiriminin `import` üzerine aşağıdaki ifadeleri ekleyin.

    ```java
    import com.google.android.gms.common.ConnectionResult;
    import com.google.android.gms.common.GoogleApiAvailability;
    import android.content.Intent;
    import android.util.Log;
    import android.widget.TextView;
    import android.widget.Toast;
    ```

4. Aşağıdaki üyeleri sınıfın en üstüne ekleyin. [Google Play Hizmetleri'nin kullanılabilirliğini Google tarafından önerildiği şekilde denetlemek](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk) için bu alanları kullanırsınız.

    ```java
    public static MainActivity mainActivity;
    public static Boolean isVisible = false;
    private static final String TAG = "MainActivity";
    private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;
    ```

5. `MainActivity` Sınıfta, Google Play Hizmetleri'nin kullanılabilirliğini kontrol etmek için aşağıdaki yöntemi ekleyin.

    ```java
    /**
    * Check the device to make sure it has the Google Play Services APK. If
    * it doesn't, display a dialog box that enables  users to download the APK from
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

6. `MainActivity` Sınıfta, FCM kayıt jetonunuzu `IntentService` almak ve hub'ınıza kaydolmak için aramadan önce Google Play Hizmetleri'ni denetleyen aşağıdaki kodu ekleyin:

    ```java
    public void registerWithNotificationHubs()
    {
        if (checkPlayServices()) {
            // Start IntentService to register this application with FCM.
            Intent intent = new Intent(this, RegistrationIntentService.class);
            startService(intent);
        }
    }
    ```

7. Sınıfın `OnCreate` yönteminde, `MainActivity` etkinlik oluşturulduğunda kayıt işlemini başlatmak için aşağıdaki kodu ekleyin:

    ```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mainActivity = this;
        registerWithNotificationHubs();
        FirebaseService.createChannelAndHandleNotifications(getApplicationContext());
    }
    ```

8. Uygulamanızdaki uygulama durumunu ve rapor durumunu doğrulamak için `MainActivity`şu ek yöntemleri ekleyin:

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

9. `ToastNotify` yöntemi, uygulamada kalıcı olarak durumu ve bildirimleri raporlamak için *"Hello World"* `TextView` denetimini kullanır. **res** > **düzeniactivity_main.xml** **layout** > düzeninizde, bu denetim için aşağıdaki kimliği ekleyin.

    ```java
    android:id="@+id/text_hello"
    ```

    ![Azure Notification Hubs - Test Gönderimi](./media/notification-hubs-android-push-notification-google-fcm-get-started/activity-main-xml.png)

10. Daha sonra AndroidManifest.xml'de tanımladığınız alıcı için bir alt sınıf eklersiniz. `FirebaseService` adlı projenize başka bir yeni sınıf ekleyin.

11. `FirebaseService.java`'in üst kısmına şu içeri aktarma deyimlerini ekleyin:

    ```java
    import com.google.firebase.messaging.FirebaseMessagingService;
    import com.google.firebase.messaging.RemoteMessage;
    import android.util.Log;
    import android.app.NotificationChannel;
    import android.app.NotificationManager;
    import android.app.PendingIntent;
    import android.content.Context;
    import android.content.Intent;
    import android.media.RingtoneManager;
    import android.net.Uri;
    import android.os.Build;
    import android.os.Bundle;
    import androidx.core.app.NotificationCompat;
    ```

12. `FirebaseService` Sınıf için aşağıdaki kodu ekleyin, bu da `FirebaseMessagingService`onu 'nin bir alt sınıfı haline getirir

    Bu kod `onMessageReceived` yöntemi geçersiz kılar ve alınan bildirimleri raporlar. ayrıca `sendNotification()` yöntemi kullanarak Android bildirim yöneticisine anında iletme bildirimi gönderir. Uygulama `sendNotification()` çalışmadığında ve bir bildirim alındığında yöntemi arayın.

    ```java
    public class FirebaseService extends FirebaseMessagingService
    {
        private String TAG = "FirebaseService";
    
        public static final String NOTIFICATION_CHANNEL_ID = "nh-demo-channel-id";
        public static final String NOTIFICATION_CHANNEL_NAME = "Notification Hubs Demo Channel";
        public static final String NOTIFICATION_CHANNEL_DESCRIPTION = "Notification Hubs Demo Channel";
    
        public static final int NOTIFICATION_ID = 1;
        private NotificationManager mNotificationManager;
        NotificationCompat.Builder builder;
        static Context ctx;
    
        @Override
        public void onMessageReceived(RemoteMessage remoteMessage) {
            // ...
    
            // TODO(developer): Handle FCM messages here.
            // Not getting messages here? See why this may be: https://goo.gl/39bRNJ
            Log.d(TAG, "From: " + remoteMessage.getFrom());
    
            String nhMessage;
            // Check if message contains a notification payload.
            if (remoteMessage.getNotification() != null) {
                Log.d(TAG, "Message Notification Body: " + remoteMessage.getNotification().getBody());
    
                nhMessage = remoteMessage.getNotification().getBody();
            }
            else {
                nhMessage = remoteMessage.getData().values().iterator().next();
            }
    
            // Also if you intend on generating your own notifications as a result of a received FCM
            // message, here is where that should be initiated. See sendNotification method below.
            if (MainActivity.isVisible) {
                MainActivity.mainActivity.ToastNotify(nhMessage);
            }
            sendNotification(nhMessage);
        }
    
        private void sendNotification(String msg) {
    
            Intent intent = new Intent(ctx, MainActivity.class);
            intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
    
            mNotificationManager = (NotificationManager)
                    ctx.getSystemService(Context.NOTIFICATION_SERVICE);
    
            PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                    intent, PendingIntent.FLAG_ONE_SHOT);
    
            Uri defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
            NotificationCompat.Builder notificationBuilder = new NotificationCompat.Builder(
                    ctx,
                    NOTIFICATION_CHANNEL_ID)
                    .setContentText(msg)
                    .setPriority(NotificationCompat.PRIORITY_HIGH)
                    .setSmallIcon(android.R.drawable.ic_popup_reminder)
                    .setBadgeIconType(NotificationCompat.BADGE_ICON_SMALL);
    
            notificationBuilder.setContentIntent(contentIntent);
            mNotificationManager.notify(NOTIFICATION_ID, notificationBuilder.build());
        }
    
        public static void createChannelAndHandleNotifications(Context context) {
            ctx = context;
    
            if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
                NotificationChannel channel = new NotificationChannel(
                        NOTIFICATION_CHANNEL_ID,
                        NOTIFICATION_CHANNEL_NAME,
                        NotificationManager.IMPORTANCE_HIGH);
                channel.setDescription(NOTIFICATION_CHANNEL_DESCRIPTION);
                channel.setShowBadge(true);
    
                NotificationManager notificationManager = context.getSystemService(NotificationManager.class);
                notificationManager.createNotificationChannel(channel);
             }
        }
    }
    ```

13. Android Studio'da, menü çubuğunda, kodunuzda hata olmadığından emin olmak için > **Project'i Oluştur'u** seçin. **Build** `ic_launcher` Simge yle ilgili bir hata alırsanız, AndroidManifest.xml dosyasından aşağıdaki ifadeyi kaldırın: 

    ```
        android:icon="@mipmap/ic_launcher"
    ```
14. Uygulamayı çalıştırmak için sanal bir cihazınız olduğundan emin olun. Eğer yoksa, aşağıdaki gibi bir ekleyin:
    1. ![Aygıt yöneticisini aç](./media/notification-hubs-android-push-notification-google-fcm-get-started/open-device-manager.png)
    2. ![Sanal aygıt oluşturma](./media/notification-hubs-android-push-notification-google-fcm-get-started/your-virtual-devices.PNG)

15. Uygulamayı seçtiğiniz cihazda çalıştırın ve hub'a başarılı bir şekilde kaydolduğunu doğrulayın.

    > [!NOTE]
    > Örnek kimlik hizmeti `onTokenRefresh()` nin yöntemi çağrılana kadar kayıt ilk başlatma sırasında başarısız olabilir. Yenileme işlemi bildirim hub'ına başarılı bir kayıt başlatmalıdır.

    ![Cihaz kaydı başarılı](./media/notification-hubs-android-push-notification-google-fcm-get-started/device-registration.png)

## <a name="test-send-notification-from-the-notification-hub"></a>Bildirim hub’ından bildirim göndermeyi test edin

Aşağıdaki adımları atarak Azure [portalından] anında iletme bildirimleri gönderebilirsiniz:

1. Azure portalında, hub'ınız için Bildirim Merkezi sayfasında **Sorun Giderme** bölümünde **Test Gönder'i** seçin.
3. **Platformlar** için **Android**’i seçin.
4. **Gönder**’i seçin.  Mobil uygulamayı çalıştırmadığınız için Android cihazda henüz bir bildirim görmezsiniz. Mobil uygulamayı çalıştırdıktan sonra, bildirim iletisini görmek için yeniden **Gönder** düğmesini seçin.
5. En alttaki listede işlemin sonucunu görürsünüz.

    ![Azure Notification Hubs - Test Gönderimi](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-test-send.png)
6. Cihazınızda bildirim iletisini görürsünüz. 

    ![Cihazda bildirim iletisi](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-on-device.png)
    

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

### <a name="run-the-mobile-app-on-emulator"></a>Mobil uygulamayı emülatörde çalıştırın
Emülatör içindeki anında iletme bildirimlerini test etmeden önce, emülatör görüntünüzün uygulamanız için seçtiğiniz Google API düzeyini desteklediğinden emin olun. Resminiz yerel Google API'lerini desteklemiyorsa, **HİzMET\_YOK\_** özel durum alabilirsiniz.

Ayrıca Google hesabınızı **Ayarlar** > **Hesapları**altında çalışan emülatörünüzüze eklediğinizden emin olun. Aksi takdirde, FCM'ye kaydolmaya yönelik girişimleriniz **AUTHENTICATION\_FAILED** özel durumuyla sonuçlanabilir.

## <a name="next-steps"></a>Sonraki adımlar
Bu eğitimde, hizmete kayıtlı tüm Android cihazlara bildirim yayınlamak için Firebase Cloud Messaging'i kullandınız. Belirli cihazlara nasıl anında iletme bildirimleri gönderileceğini öğrenmek için aşağıdaki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
>[Öğretici: Belirli Android cihazlara anında bildirim](push-notifications-android-specific-devices-firebase-cloud-messaging.md)

<!-- Images. -->

<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: https://go.microsoft.com/fwlink/?LinkId=389800
[Notification Hubs Guidance]: notification-hubs-push-notification-overview.md
[Azure portal]: https://portal.azure.com
