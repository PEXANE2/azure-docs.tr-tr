---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 69dc0e1c14bc88cdbf0aa48700f95058ba759cc0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188877"
---
1. Çözüm görünümünde (veya Visual Studio'daki **Solution Explorer),** **Bileşenler** klasörüne sağ tıklayın, Daha Fazla **Bileşen Al...,** **Google Bulut İleti Istemcisi** bileşenini arayın ve projeye ekleyin.
2. ToDoActivity.cs proje dosyasını açın ve sınıfa aşağıdaki ifadesini ekleyin:

    ```csharp
    using Gcm.Client;
    ```

3. **ToDoActivity** sınıfına aşağıdaki yeni kodu ekleyin: 

    ```csharp
    // Create a new instance field for this activity.
    static ToDoActivity instance = new ToDoActivity();

    // Return the current activity instance.
    public static ToDoActivity CurrentActivity
    {
        get
        {
            return instance;
        }
    }
    // Return the Mobile Services client.
    public MobileServiceClient CurrentClient
    {
        get
        {
            return client;
        }
    }
    ```

    Bu, anında işleyici hizmet sürecinden mobil istemci örneğine erişmenizi sağlar.
4. **MobileServiceClient** oluşturulduktan sonra **OnCreate** yöntemine aşağıdaki kodu ekleyin:

    ```csharp
    // Set the current instance of TodoActivity.
    instance = this;

    // Make sure the GCM client is set up correctly.
    GcmClient.CheckDevice(this);
    GcmClient.CheckManifest(this);

    // Register the app for push notifications.
    GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);
    ```

**ToDoActivity'unuz** artık anında iletme bildirimleri eklemek için hazırlanmıştır.
