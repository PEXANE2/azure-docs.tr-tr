---
title: Java ile Azure Bildirim Hub'ları nasıl kullanılır?
description: Azure Bildirim Hub'larını Java arka ucundan nasıl kullanacağınızı öğrenin.
services: notification-hubs
documentationcenter: ''
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: 4c3f966d-0158-4a48-b949-9fa3666cb7e4
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: java
ms.devlang: java
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: d48973cc7c5ed1fc7ae3f96128d488f3f1df3a05
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76263872"
---
# <a name="how-to-use-notification-hubs-from-java"></a>Java'dan Bildirim Hub'ları nasıl kullanılır?

[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Bu konu, tam olarak desteklenen yeni resmi Azure Bildirim Hub Java SDK'nın temel özelliklerini açıklar.
Bu proje açık kaynak kodlu bir projedir ve [Java SDK'da]tüm SDK kodunu görüntüleyebilirsiniz.

Genel olarak, MSDN konu [Bildirim Hub'larında](https://msdn.microsoft.com/library/dn223264.aspx)açıklandığı gibi Bildirim Hub REST arabirimini kullanarak Java/PHP/Python/Ruby arka ucundan tüm Bildirim Hub'ları özelliklerine erişebilirsiniz. Bu Java SDK Java bu REST arayüzleri üzerinde ince bir sarıcı sağlar.

SDK şu anda şunları desteklemektedir:

* Bildirim Hub'larında CRUD
* Kayıtlarda CRUD
* Kurulum Yönetimi
* İthalat/İhracat Kayıtları
* Düzenli Göndermeler
* Zamanlanmış Gönder
* Java NIO ile async işlemleri
* Desteklenen platformlar: APNS (iOS), FCM (Android), WNS (Windows Mağazası uygulamaları), MPNS(Windows Phone), ADM (Amazon Kindle Fire), Baidu (Google hizmetleri olmadan Android)

## <a name="sdk-usage"></a>SDK Kullanımı

### <a name="compile-and-build"></a>Derleme ve oluşturma

[Maven] kullanın

Oluşturmak için:

    mvn package

## <a name="code"></a>Kod

### <a name="notification-hub-cruds"></a>Bildirim Hub CRUDs

**Bir NamespaceManager oluşturun:**

    ```java
    NamespaceManager namespaceManager = new NamespaceManager("connection string")
    ```

**Bildirim Merkezi Oluştur:**

    ```java
    NotificationHubDescription hub = new NotificationHubDescription("hubname");
    hub.setWindowsCredential(new WindowsCredential("sid","key"));
    hub = namespaceManager.createNotificationHub(hub);
    ```

 OR

    ```java
    hub = new NotificationHub("connection string", "hubname");
    ```

**Bildirim Hub'ı Alın:**

    ```java
    hub = namespaceManager.getNotificationHub("hubname");
    ```

**Bildirim Merkezini Güncelleştir:**

    ```java
    hub.setMpnsCredential(new MpnsCredential("mpnscert", "mpnskey"));
    hub = namespaceManager.updateNotificationHub(hub);
    ```

**Bildirim Hub'ı Sil:**

    ```java
    namespaceManager.deleteNotificationHub("hubname");
    ```

### <a name="registration-cruds"></a>Kayıt CRUDs

**Bildirim Merkezi istemcisi oluşturma:**

    ```java
    hub = new NotificationHub("connection string", "hubname");
    ```

**Windows kaydı oluşturma:**

    ```java
    WindowsRegistration reg = new WindowsRegistration(new URI(CHANNELURI));
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");
    hub.createRegistration(reg);
    ```

**iOS kaydı oluşturma:**

    ```java
    AppleRegistration reg = new AppleRegistration(DEVICETOKEN);
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");
    hub.createRegistration(reg);
    ```

Benzer şekilde Android (FCM), Windows Phone (MPNS) ve Kindle Fire (ADM) için de kayıtlar oluşturabilirsiniz.

**Şablon kayıtları oluşturun:**

    ```java
    WindowsTemplateRegistration reg = new WindowsTemplateRegistration(new URI(CHANNELURI), WNSBODYTEMPLATE);
    reg.getHeaders().put("X-WNS-Type", "wns/toast");
    hub.createRegistration(reg);
    ```

**Kayıt kimliği oluşturma + yukarı desen kullanarak kayıt oluşturma:**

Kayıt adlarını aygıtta depolarken kaybolan yanıtlar nedeniyle yinelenenleri kaldırır:

    ```java
    String id = hub.createRegistrationId();
    WindowsRegistration reg = new WindowsRegistration(id, new URI(CHANNELURI));
    hub.upsertRegistration(reg);
    ```

**Kayıtları güncelleştirin:**

    ```java
    hub.updateRegistration(reg);
    ```

**Kayıtları silme:**

    ```java
    hub.deleteRegistration(regid);
    ```

**Sorgu kayıtları:**

* **Tek kayıt alın:**

    ```java
    hub.getRegistration(regid);
    ```

* **Tüm kayıtları hub'dan alın:**

    ```java
    hub.getRegistrations();
    ```

* **Etiketli kayıtları alın:**

    ```java
    hub.getRegistrationsByTag("myTag");
    ```

* **Kanala göre kayıt alın:**

    ```java
    hub.getRegistrationsByChannel("devicetoken");
    ```

Tüm koleksiyon sorguları $top ve devam belirteçlerini destekler.

### <a name="installation-api-usage"></a>Kurulum API kullanımı

Yükleme API kayıt yönetimi için alternatif bir mekanizmadır. Önemsiz olmayan ve kolayca yanlış veya verimsiz olarak yapılabilen birden çok kaydı korumak yerine, artık tek bir Yükleme nesnesi kullanmak mümkündür.

Yükleme ihtiyacınız olan her şeyi içerir: push channel (aygıt belirteci), etiketler, şablonlar, ikincil kutucuklar (WNS ve APNS için). Artık kimlik almak için hizmeti aramanız gerekmez - sadece GUID veya başka bir tanımlayıcı oluşturun, aygıtın üzerinde tutun ve itme kanalıyla (aygıt belirteci) arka uca gönderin.

Arka uçta, yalnızca tek bir arama `CreateOrUpdateInstallation`yapmalısınız; tamamen idempotent, bu yüzden gerekirse yeniden denemek için çekinmeyin.

Amazon Kindle Yangın için örnek olarak:

    ```java
    Installation installation = new Installation("installation-id", NotificationPlatform.Adm, "adm-push-channel");
    hub.createOrUpdateInstallation(installation);
    ```

Güncellemek isterseniz:

    ```java
    installation.addTag("foo");
    installation.addTemplate("template1", new InstallationTemplate("{\"data\":{\"key1\":\"$(value1)\"}}","tag-for-template1"));
    installation.addTemplate("template2", new InstallationTemplate("{\"data\":{\"key2\":\"$(value2)\"}}","tag-for-template2"));
    hub.createOrUpdateInstallation(installation);
    ```

Gelişmiş senaryolar için, yükleme nesnesinin yalnızca belirli özelliklerini değiştirmeye olanak tanıyan kısmi güncelleştirme özelliğini kullanın. Kısmi güncelleştirme, Yükleme nesnesi karşı çalıştırabileceğiniz JSON Yama işlemlerinin alt kümesidir.

    ```java
    PartialUpdateOperation addChannel = new PartialUpdateOperation(UpdateOperationType.Add, "/pushChannel", "adm-push-channel2");
    PartialUpdateOperation addTag = new PartialUpdateOperation(UpdateOperationType.Add, "/tags", "bar");
    PartialUpdateOperation replaceTemplate = new PartialUpdateOperation(UpdateOperationType.Replace, "/templates/template1", new InstallationTemplate("{\"data\":{\"key3\":\"$(value3)\"}}","tag-for-template1")).toJson());
    hub.patchInstallation("installation-id", addChannel, addTag, replaceTemplate);
    ```

Yüklemeyi Sil:

    ```java
    hub.deleteInstallation(installation.getInstallationId());
    ```

`CreateOrUpdate`, `Patch`, `Delete` ve sonunda `Get`tutarlı . İstediğiniz işlem, arama sırasında sistem kuyruğuna gider ve arka planda yürütülür. Get ana çalışma zamanı senaryosu için değil, yalnızca hata ayıklama ve sorun giderme amacıyla tasarlanmıştır, hizmet tarafından sıkı bir şekilde daraltılır.

Yüklemeler için gönderme akışı Kayıtlar için aynıdır. Belirli Yükleme'ye bildirim hedeflemek için "InstallationId:{desired-id} etiketini kullanmanız gerekir. Bu durumda, kod:

    ```java
    Notification n = Notification.createWindowsNotification("WNS body");
    hub.sendNotification(n, "InstallationId:{installation-id}");
    ```

Birkaç şablondan biri için:

    ```java
    Map<String, String> prop =  new HashMap<String, String>();
    prop.put("value3", "some value");
    Notification n = Notification.createTemplateNotification(prop);
    hub.sendNotification(n, "InstallationId:{installation-id} && tag-for-template1");
    ```

### <a name="schedule-notifications-available-for-standard-tier"></a>Zamanlama Bildirimleri (STANDART Katman için kullanılabilir)

Düzenli göndermek gibi ama bir ek parametre ile aynı - scheduledTime, hangi bildirim teslim edilmelidir diyor. Hizmet şimdi + 5 dakika ve şimdi + 7 gün arasında zaman herhangi bir nokta kabul eder.

**Bir Windows yerel bildirimi zamanlama:**

    ```java
    Calendar c = Calendar.getInstance();
    c.add(Calendar.DATE, 1);
    Notification n = Notification.createWindowsNotification("WNS body");
    hub.scheduleNotification(n, c.getTime());
    ```

### <a name="importexport-available-for-standard-tier"></a>İthalat/İhracat (STANDART Katman için kullanılabilir)

Kayıtlara karşı toplu işlem yapmanız gerekebilir. Genellikle başka bir sistem veya etiketleri güncellemek için büyük bir düzeltme ile entegrasyon içindir. Binlerce kayıt söz konusuysa, Get/Update akışını kullanmanızı önermiyoruz. Sistemin Alma/Dışa Aktarma özelliği senaryoyu kapsayacak şekilde tasarlanmıştır. Gelen veri kaynağı ve çıktı için konum kaynağı olarak depolama hesabınızın altındaki bir blob kapsayıcısına erişim sağlarsınız.

**Bir dışa aktarma işi gönderin:**

    ```java
    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ExportRegistrations);
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);
    ```

**Bir alma işi gönderme:**

    ```java
    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ImportCreateRegistrations);
    job.setImportFileUri("input file uri with SAS signature");
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);
    ```

**Bir iş bitene kadar bekleyin:**

    ```java
    while(true){
        Thread.sleep(1000);
        job = hub.getNotificationHubJob(job.getJobId());
        if(job.getJobStatus() == NotificationHubJobStatus.Completed)
            break;
    }
    ```

**Tüm işleri alın:**

    ```java
    List<NotificationHubJob> jobs = hub.getAllNotificationHubJobs();
    ```

**SAS imzalı URI:**

 Bu URL, bir blob dosyası veya blob kapsayıcısının URL'si ve izinler ve son kullanma süresi gibi bir dizi parametrenin yanı sıra hesabın SAS tuşu kullanılarak yapılan tüm bu şeylerin imzasıdır. Azure Depolama Java SDK, bu URL'lerin oluşturulması da dahil olmak üzere zengin özelliklere sahiptir. Basit bir alternatif olarak, `ImportExportE2E` imzalama algoritmasının temel ve kompakt uygulamasına sahip test sınıfına (GitHub konumundan) bir göz atın.

### <a name="send-notifications"></a>Bildirim Gönder

Bildirim nesnesi yalnızca üstbilgiiçeren bir gövdedir, bazı yardımcı program yöntemleri yerel ve şablon bildirimleri nesnelerinin oluşturulmasına yardımcı olur.

* **Windows Mağazası ve Windows Phone 8.1 (Silverlight olmayan)**

    ```java
    String toast = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello from Java!</text></binding></visual></toast>";
    Notification n = Notification.createWindowsNotification(toast);
    hub.sendNotification(n);
    ```

* **iOS**

    ```java
    String alert = "{\"aps\":{\"alert\":\"Hello from Java!\"}}";
    Notification n = Notification.createAppleNotification(alert);
    hub.sendNotification(n);
    ```

* **Android**

    ```java
    String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
    Notification n = Notification.createFcmNotification(message);
    hub.sendNotification(n);
    ```

* **Windows Phone 8.0 ve 8.1 Silverlight**

    ```java
    String toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                "<wp:Notification xmlns:wp=\"WPNotification\">" +
                    "<wp:Toast>" +
                        "<wp:Text1>Hello from Java!</wp:Text1>" +
                    "</wp:Toast> " +
                "</wp:Notification>";
    Notification n = Notification.createMpnsNotification(toast);
    hub.sendNotification(n);
    ```

* **Kindle Yangın**

    ```java
    String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
    Notification n = Notification.createAdmNotification(message);
    hub.sendNotification(n);
    ```

* **Etiketlere Gönder**
  
    ```java
    Set<String> tags = new HashSet<String>();
    tags.add("boo");
    tags.add("foo");
    hub.sendNotification(n, tags);
    ```

* **Etiket ifadesini gönder**

    ```java
    hub.sendNotification(n, "foo && ! bar");
    ```

* **Şablon bildirimi gönder**

    ```java
    Map<String, String> prop =  new HashMap<String, String>();
    prop.put("prop1", "v1");
    prop.put("prop2", "v2");
    Notification n = Notification.createTemplateNotification(prop);
    hub.sendNotification(n);
    ```

Java kodunuzu çalıştırmak artık hedef aygıtınızda görünen bir bildirim oluşturmalıdır.

## <a name="next-steps"></a><a name="next-steps"></a>Sonraki Adımlar

Bu konu, Bildirim Hub'ları için basit bir Java REST istemcisi oluşturmanızı gösterdi. Burada yapabilecekleriniz:

* Tüm SDK kodunu içeren tam [Java SDK'yı]indirin.
* Örneklerle oynayın:
  * [Bildirim Hub'larıyla Başlayın]
  * [Son dakika haberleri gönder]
  * [Yerelleştirilmiş son dakika haberleri gönder]
  * [Kimlik doğrulaması yapılan kullanıcılara bildirim gönderme]
  * [Kimlik doğrulaması yapılan kullanıcılara platform ötesi bildirimler gönderme]

[Java SDK]: https://github.com/Azure/azure-notificationhubs-java-backend
[Get started tutorial]: notification-hubs-ios-apple-push-notification-apns-get-started.md
[Bildirim Hub'larıyla Başlayın]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Son dakika haberleri gönder]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Yerelleştirilmiş son dakika haberleri gönder]: notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md
[Kimlik doğrulaması yapılan kullanıcılara bildirim gönderme]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Kimlik doğrulaması yapılan kullanıcılara platform ötesi bildirimler gönderme]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Maven]: https://maven.apache.org/
