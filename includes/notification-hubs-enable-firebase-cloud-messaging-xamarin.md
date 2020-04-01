---
title: include dosyası
description: include dosyası
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 08/01/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 45bdd569741dc13181bcaf9e8587a02b3d02c621
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "68728842"
---
1. [Firebase konsolunda](https://firebase.google.com/console/) oturum açın. Henüz bir tane yoksa yeni bir Firebase projesi oluşturun.
2. Projenizi oluşturduktan sonra **Firebase’i Android uygulamanıza ekleyin**’i seçin. 

    ![Firebase’i Android uygulamanıza ekleyin](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)

3. Android **uygulama sayfanıza Firebase Ekle'de** aşağıdaki adımları izleyin: 
    1. Android **paket adı**için, paketiniz için bir ad girin. Örneğin: `tutorials.tutoria1.xamarinfcmapp`. 

        ![Paket adını belirtin](./media/notification-hubs-enable-firebase-cloud-messaging/specify-package-name-fcm-settings.png)
    2. **Kaydol uygulamasını**seçin.  
    1. **Google-services.json'u İndir'i**seçin. Ardından dosyayı projenizin klasörüne kaydedin ve **İleri'yi**seçin. Visual Studio projesini henüz oluşturmadıysanız, projeyi oluşturduktan sonra bu adımı yapabilirsiniz. 

        ![Google-services.json dosyasını indirin](./media/notification-hubs-enable-firebase-cloud-messaging/download-google-service-button.png)
    6. **Sonraki'ni**seçin. 
    7. **Bu adımı atla'yı**seçin. 

        ![Son adımı atla](./media/notification-hubs-enable-firebase-cloud-messaging/skip-this-step.png)
8. Firebase konsolunda projenizin dişli simgesini seçin. Sonra, **Proje Ayarları**’nı seçin.

    ![Proje Ayarları’nı seçin](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. **Google-services.json** dosyasını indirmediyseniz, bu sayfadan indirebilirsiniz. 

    ![Genel sekmesinden google-services.json'ı indirin](./media/notification-hubs-enable-firebase-cloud-messaging/download-google-services-json-general-page.png)
1. En üstteki **Bulut İletisi** sekmesine geçin. **Sunucu tuşunu** daha sonra kullanmak üzere kopyalayın ve kaydedin. Bildirim hub'ınızı yapılandırmak için bu değeri kullanırsınız.

    ![Sunucu anahtarını kopyalama](./media/notification-hubs-enable-firebase-cloud-messaging/server-key.png)
