---
title: include dosyası
description: include dosyası
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 02/05/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: cd202c98ed605209f5600965ecdb6c0b4c03c17e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67509120"
---
1. Android Studio'da menüdeki **Araçlar'ı** seçin ve ardından **SDK Yöneticisi'ni**seçin. 
2. Projenizde kullanılan Android SDK'nın hedef sürümünü seçin. Ardından **Paket Ayrıntılarını Göster'i**seçin. 

    ![Android SDK Manager - hedef sürümü seçin](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. Zaten yüklü değilse, **Google API'leri**seçin.

    ![Android SDK Yöneticisi - Google API'ler seçildi](./media/notification-hubs-android-studio-add-google-play-services/googole-apis-selected.png)
4. **SDK Araçları** sekmesine geçin. Google Play Hizmetleri'ni henüz yüklemediyseniz, aşağıdaki resimde gösterildiği gibi **Google Play Hizmetleri'ni** seçin. Ardından yüklemek için **Uygula'yı** seçin. SDK yolunun sonraki bir adım için olduğunu unutmayın.

    ![Android SDK Yöneticisi - Google Play Hizmetleri seçildi](./media/notification-hubs-android-studio-add-google-play-services/google-play-services-selected.png)
3. **Değiştir'i onayla** iletişim kutusunu **görüyorsanız, Tamam'ı**seçin. Bileşen Yükleyici istenen bileşenleri yükler. Bileşenler yüklendikten sonra **Bitir'i** seçin.
4. **Yeni Projeler için Ayarlar** iletişim kutusunu kapatmak için **Tamam'ı** seçin.  
5. **uygulama** dizinindeki build.gradle dosyasını açın ve aşağıdaki satırı `dependencies`ekleyin. 

    ```gradle
    implementation 'com.google.android.gms:play-services-gcm:16.0.0'
    ```
5. Araç çubuğunda **Şimdi Eşitle** simgesini seçin.

    ![Gradle ile senkronize](./media/notification-hubs-android-studio-add-google-play-services/gradle-sync.png)
1. AndroidManifest.xml dosyasını açın ve *ardından uygulama* etiketine aşağıdaki etiketi ekleyin.

    ```xml
    <meta-data android:name="com.google.android.gms.version"
         android:value="@integer/google_play_services_version" />
    ```
