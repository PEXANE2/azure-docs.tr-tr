---
title: include dosyası
description: include dosyası
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 09/11/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: ccb5e40738680181e7339b8652d029597c7d0bd4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "70935153"
---
1. [Firebase konsolunda](https://firebase.google.com/console/) oturum açın. Henüz bir tane yoksa yeni bir Firebase projesi oluşturun.
2. Projenizi oluşturduktan sonra **Firebase’i Android uygulamanıza ekleyin**’i seçin. 

    ![Firebase’i Android uygulamanıza ekleyin](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. Android **uygulama sayfanıza Firebase Ekle'de** aşağıdaki adımları izleyin: 
    1. **Android paket adı için,** uygulamanızın build.gradle dosyasındaki **uygulamanızın** değerini kopyalayın. Bu örnekte, bu `com.fabrikam.fcmtutorial1app`. 

        ![Paket adını belirtin](./media/notification-hubs-enable-firebase-cloud-messaging/specify-package-name-fcm-settings.png)
    2. **Kaydol uygulamasını**seçin. 
4. **google-services.json indir'i**seçin, dosyayı projenizin **uygulama** klasörüne kaydedin ve **sonra İleri'yi**seçin. 

    ![Google-services.json dosyasını indirin](./media/notification-hubs-enable-firebase-cloud-messaging/download-google-service-button.png)
5. Android Studio'da projenizde aşağıdaki **yapılandırma değişikliklerini** yapın. 
    1.  Proje düzeyinde build.gradle dosyanızda&lt;&gt;(project /build.gradle) **bağımlılıklar** bölümüne aşağıdaki ifadeyi ekleyin. 

        ```
        classpath 'com.google.gms:google-services:4.0.1'
        ```
    2. Uygulama düzeyindeki build.gradle dosyanızda&gt;/&lt;(proje&gt;&lt;uygulama modülü /build.gradle) **bağımlılıklar** bölümüne aşağıdaki ifadeleri ekleyin. 

        ```
        implementation 'com.google.firebase:firebase-core:16.0.8'
        implementation 'com.google.firebase:firebase-messaging:17.3.4'
        ```

    3. Bağımlılıklar bölümünden sonra uygulama düzeyi build.gradle dosyasının sonuna aşağıdaki satırı ekleyin. 

        ```
        apply plugin: 'com.google.gms.google-services'
        ```        
    4. Araç çubuğunda **Şimdi Eşitle'yi** seçin. 
 
        ![build.gradle yapılandırma değişiklikleri](./media/notification-hubs-enable-firebase-cloud-messaging/build-gradle-configurations.png)
6. **Sonraki'ni**seçin. 
7. **Bu adımı atla'yı**seçin. 

    ![Son adımı atla](./media/notification-hubs-enable-firebase-cloud-messaging/skip-this-step.png)
8. Firebase konsolunda projenizin dişli simgesini seçin. Sonra, **Proje Ayarları**’nı seçin.

    ![Proje Ayarları’nı seçin](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. Google-services.json dosyasını Android Studio projenizin **uygulama** klasörüne indirmediyseniz, bunu bu sayfada yapabilirsiniz. 
5. En üstteki **Bulut İletisi** sekmesine geçin. 
6. **Sunucu tuşunu** daha sonra kullanmak üzere kopyalayın ve kaydedin. Hub'ınızı yapılandırmak için bu değeri kullanırsınız.
