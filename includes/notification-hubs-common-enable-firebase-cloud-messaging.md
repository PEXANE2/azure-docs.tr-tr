---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 71e4b0b3b7bf84938123acf70ac18a93c3015a4d
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85081558"
---
### <a name="create-a-firebase-project-and-enable-firebase-cloud-messaging-for-android"></a>Firebase projesi oluşturma ve Android için Firebase bulut mesajlaşma özelliğini etkinleştirme

1. [Firebase konsolunda](https://firebase.google.com/console/) oturum açın. **Proje adı**olarak **pushdemo** girerek yeni bir Firebase projesi oluşturun.

    > [!NOTE]
    > Sizin için benzersiz bir ad oluşturulacaktır. Bu, varsayılan olarak, belirttiğiniz adın küçük harfli bir türevi ve bir kısa çizgiyle ayrılmış üretilmiş bir sayı ile oluşur. Yine de genel olarak benzersiz olmasını istiyorsanız bunu değiştirebilirsiniz.

1. Projenizi oluşturduktan sonra **Firebase’i Android uygulamanıza ekleyin**’i seçin.

    ![Firebase’i Android uygulamanıza ekleyin](./media/notification-hubs-common-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)

1. **Android uygulamanıza Firebase ekleme** sayfasında, aşağıdaki adımları uygulayın.
    1. **Android paket adı**için paketiniz için bir ad girin. Örneğin: `com.<organization_identifier>.<package_name>`.

        ![Paket adını belirtin](./media/notification-hubs-common-enable-firebase-cloud-messaging/specify-package-name-fcm-settings.png)
    1. **Uygulamayı kaydet**' i seçin.  
    1. **google-services.jskarşıdan yükleme '** yi seçin. Sonra dosyayı daha sonra kullanmak için yerel bir klasöre kaydedin ve **İleri**' yi seçin.  

        ![google-services.jsindir](./media/notification-hubs-common-enable-firebase-cloud-messaging/download-google-service-button.png)
    1. **İleri**’yi seçin.
    1. **Konsola devam et** ' i seçin

        > [!NOTE]
        > **Konsola devam et** düğmesi etkin değilse, *yüklemeyi doğrula* denetimi nedeniyle **Bu adımı atla**' yı seçin.

1. Firebase konsolunda projenizin dişli simgesini seçin. Sonra, **Proje Ayarları**’nı seçin.

    ![Proje Ayarları’nı seçin](./media/notification-hubs-common-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)

    > [!NOTE]
    > Dosyayı **google-services.js** indirmediyseniz bu sayfada indirebilirsiniz.

1. En üstteki **bulut mesajlaşma** sekmesine geçin. Daha sonra kullanmak üzere **sunucu anahtarını** kopyalayın ve kaydedin. Bu değeri, Bildirim Hub 'ınızı yapılandırmak için kullanırsınız.

    ![Sunucu anahtarını Kopyala](./media/notification-hubs-common-enable-firebase-cloud-messaging/server-key.png)
