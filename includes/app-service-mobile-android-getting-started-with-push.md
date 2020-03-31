---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: e66e7d4646b650d2d811d3807db04605dfddeded
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188885"
---
1. **Uygulama** projenizde dosyayı `AndroidManifest.xml`açın. Açılış etiketinden `application` sonra aşağıdaki kodu ekleyin:

    ```xml
    <service android:name=".ToDoMessagingService">
        <intent-filter>
            <action android:name="com.google.firebase.MESSAGING_EVENT"/>
        </intent-filter>
    </service>
    <service android:name=".ToDoInstanceIdService">
        <intent-filter>
            <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
        </intent-filter>
    </service>
    ```

2. Dosyayı `ToDoActivity.java`açın ve aşağıdaki değişiklikleri yapın:

    - Alma deyimini ekleyin:

        ```java
        import com.google.firebase.iid.FirebaseInstanceId;
        ```

    - `MobileServiceClient` **Özel** statik ten özel **tanımını**değiştirin, böylece şimdi şuna benzer:

        ```java
        private static MobileServiceClient mClient;
        ```

    - Yöntem `registerPush` ekle:

        ```java
        public static void registerPush() {
            final String token = FirebaseInstanceId.getInstance().getToken();
            if (token != null) {
                new AsyncTask<Void, Void, Void>() {
                    protected Void doInBackground(Void... params) {
                        mClient.getPush().register(token);
                        return null;
                    }
                }.execute();
            }
        }
        ```

    - Sınıfın **onCreate** yöntemini `ToDoActivity` güncelleştirin. Anlık hale alındıktan sonra `MobileServiceClient` bu kodu eklediğinizden emin olun.

        ```java
        registerPush();
        ```

3. Bildirimleri işlemek için yeni bir sınıf ekleyin. Project Explorer'da, **uygulama** > **java** > **your-project-namespace** düğümlerini açın ve paket adı düğümlerini sağ tıklatın. **Yeni'yi**tıklatın ve ardından **Java Sınıfı'nı**tıklatın. Ad'da, `ToDoMessagingService`'yazın ve sonra Tamam'ı tıklatın. Ardından, sınıf bildirimini aşağıdakilerle değiştirin:

    ```java
    import android.app.Notification;
    import android.app.NotificationManager;
    import android.app.PendingIntent;
    import android.content.Context;
    import android.content.Intent;

    import com.google.firebase.messaging.FirebaseMessagingService;
    import com.google.firebase.messaging.RemoteMessage;

    public class ToDoMessagingService extends FirebaseMessagingService {

        private static final int NOTIFICATION_ID = 1;

        @Override
        public void onMessageReceived(RemoteMessage remoteMessage) {
            String message = remoteMessage.getData().get("message");
            if (message != null) {
                sendNotification("Notification Hub Demo", message);
            }
        }

        private void sendNotification(String title, String messageBody) {
            PendingIntent contentIntent = PendingIntent.getActivity(this, 0, new Intent(this, ToDoActivity.class), 0);
            Notification.Builder notificationBuilder = new Notification.Builder(this)
                    .setSmallIcon(R.drawable.ic_launcher)
                    .setContentTitle(title)
                    .setContentText(messageBody)
                    .setContentIntent(contentIntent);
            NotificationManager notificationManager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
            if (notificationManager != null) {
                notificationManager.notify(NOTIFICATION_ID, notificationBuilder.build());
            }
        }
    }
    ```

4. Belirteç güncelleştirmelerini işlemek için başka bir sınıf ekleyin. Java `ToDoInstanceIdService` sınıfı oluşturun ve sınıf bildirimini aşağıdakilerle değiştirin:

    ```java
    import com.google.firebase.iid.FirebaseInstanceIdService;

    public class ToDoInstanceIdService extends FirebaseInstanceIdService {

        @Override
        public void onTokenRefresh() {
            ToDoActivity.registerPush();
        }
    }
    ```

Uygulamanız artık anında iletme bildirimlerini destekleyecek şekilde güncelleştirildi.
