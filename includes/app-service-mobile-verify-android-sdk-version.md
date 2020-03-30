---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: 46cfb27b8bde95990d13ec4bca4e96f25cfe9dc5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188886"
---
Devam eden geliştirme nedeniyle, Android Studio'da yüklü olan Android SDK sürümü koddaki sürümle eşleşmeyebilir. Bu öğretici başvurulan Android SDK sürüm 26, yazma sırasında en son. SDK'nın yeni sürümleri göründükçe sürüm numarası artabilir ve mevcut en son sürümü kullanmanızı öneririz.

Sürüm uyuşmazlığı iki belirtisi şunlardır:

- Projeyi oluşturduğunuzda veya yeniden oluşturduğunuzda, '' gibi `Gradle sync failed: Failed to find target with hash string 'android-XX'`Gradle hata iletileri alabilirsiniz.
- `import` Bildirimlere dayalı olarak çözülmesi gereken koddaki standart Android nesneleri hata iletileri oluşturuyor olabilir.

Bunlardan biri görünürse, Android Studio'da yüklü olan Android SDK sürümü indirilen projenin SDK hedefiyle eşleşmeyebilir. Sürümü doğrulamak için aşağıdaki değişiklikleri yapın:

1. Android Studio'da **Araçlar** > **Android** > **SDK Manager'ı**tıklatın. SDK Platformu'nun en son sürümünü yüklemediyseniz, yüklemek için tıklatın. Sürüm numarasını not edin.

2. Project **Explorer** sekmesinde, **Gradle Scripts**altında, dosya **build.gradle (Modül: uygulama)** açın. **CompileSdkVersion** ve **targetSdkVersion'un** en son SDK sürümüne ayarlandığından emin olun. Şuna `build.gradle` benzeyebilir:

    ```gradle
    android {
        compileSdkVersion 26
        defaultConfig {
            targetSdkVersion 26
        }
    }
    ```
