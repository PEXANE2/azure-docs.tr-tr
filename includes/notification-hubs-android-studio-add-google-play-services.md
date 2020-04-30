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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67509120"
---
1. Android Studio menüsünde **Araçlar** ' ı seçin ve ardından **SDK yöneticisi**' ni seçin. 
2. Projenizde kullanılan Android SDK hedef sürümünü seçin. Ardından **Paket ayrıntılarını göster**' i seçin. 

    ![Android SDK Yöneticisi-hedef sürümü seçin](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. Zaten yüklenmemişse **Google API 'leri**seçin.

    ![Android SDK Manager-Google API 'Leri seçildi](./media/notification-hubs-android-studio-add-google-play-services/googole-apis-selected.png)
4. **SDK Tools** sekmesine geçin. Google Play Hizmetleri zaten yüklemediyseniz, aşağıdaki görüntüde gösterildiği gibi **Google Play hizmetleri** seçin. Ardından, yüklemek için **Uygula** ' yı seçin. SDK yolunun sonraki bir adım için olduğunu unutmayın.

    ![Android SDK Manager-Google Play Hizmetleri seçildi](./media/notification-hubs-android-studio-add-google-play-services/google-play-services-selected.png)
3. **Değişikliği Onayla** iletişim kutusunu görürseniz **Tamam**' ı seçin. Bileşen yükleyicisi istenen bileşenleri yüklüyor. Bileşenler yüklendikten sonra **son** ' u seçin.
4. **Yeni projeler Için ayarlar** iletişim kutusunu kapatmak için **Tamam ' ı** seçin.  
5. **App** dizinindeki Build. Gradle dosyasını açın ve altına `dependencies`aşağıdaki satırı ekleyin. 

    ```gradle
    implementation 'com.google.android.gms:play-services-gcm:16.0.0'
    ```
5. Araç çubuğunda **Şimdi Eşitle** simgesini seçin.

    ![Gradle ile Eşitle](./media/notification-hubs-android-studio-add-google-play-services/gradle-sync.png)
1. AndroidManifest. xml dosyasını açın ve ardından aşağıdaki etiketi *uygulama* etiketine ekleyin.

    ```xml
    <meta-data android:name="com.google.android.gms.version"
         android:value="@integer/google_play_services_version" />
    ```
