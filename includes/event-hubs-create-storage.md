---
title: include dosyası
description: include dosyası
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 10/16/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: bf107da82fb3f772a341e70ce472f08ea674a450
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75692581"
---
### <a name="create-a-storage-account-for-event-processor-host"></a>Olay İşleyicisi Ana Bilgisayarı için bir depolama hesabı oluşturma
Olay İşleyicisi Ana Bilgisayarı, olay hub’larına ait kalıcı denetim noktalarını ve paralel alımları yöneterek bu olay hub’larından olay almayı basitleştiren akıllı bir aracıdır. Olay İşleyicisi Ana Bilgisayarı, denetim noktası için bir depolama hesabına ihtiyaç duyar. Aşağıdaki örnekte depolama hesabı oluşturma ve erişim için anahtarını alma adımları gösterilmiştir:

1. Azure portalı menüsünden **kaynak oluştur'u**seçin.

    ![Kaynak menüsü öğesi oluşturma, Microsoft Azure portalı](./media/event-hubs-create-storage/create-resource.png)

2. **Depolama** > **Deposu hesabını**seçin.
   
    ![Depolama hesabı, Microsoft Azure portalı'nı seçin](./media/event-hubs-create-storage/select-storage-account.png)

3. **Depolama hesabı oluştur** sayfasında aşağıdaki adımları gerçekleştirin: 

   1. Depolama **hesabı adını**girin.
   2. Etkinlik merkezini içeren bir Azure **Aboneliği** seçin.
   3. Olay göbeğine sahip **Kaynak grubunu** seçin veya oluşturun.
   4. Kaynağı oluşturmak için bir **Konum** seçin. 
   5. **İncele ve oluştur**’u seçin.
   
        ![İnceleme + oluşturma, depolama hesabı oluşturma, Microsoft Azure portalı](./media/event-hubs-create-storage/review-create.png)

4. **Gözden geçir + oluştur** sayfasında değerleri gözden geçirin ve **Oluştur**'u seçin. 

    ![Depolama hesabı ayarlarını gözden geçirin ve Microsoft Azure portalı oluşturun](./media/event-hubs-create-storage/create-storage-account.png)
5. Bildirimlerinizde **Dağıtımlar Başarılı** iletisini gördünkten sonra, Depolama Hesabı sayfasını açmak **için kaynağa Git'i** seçin. Alternatif olarak, **Dağıtım ayrıntılarını** genişletebilir ve kaynak listesinden yeni kaynağınızı seçebilirsiniz.  

    ![Kaynağa git, depolama hesabı dağıtımı, Microsoft Azure portalı](./media/event-hubs-create-storage/go-to-resource.png) 
6. **Kapsayıcılar'ı**seçin.

    ![Blobs kapsayıcı hizmetini, depolama hesaplarını, Microsoft Azure portalını seçin](./media/event-hubs-create-storage/select-blob-container-service.png)
7. Üstte **+ Kapsayıcı'yı** seçin, kapsayıcı için bir **Ad** girin ve **Tamam'ı**seçin. 

    ![Yeni bir blob kapsayıcısı, depolama hesapları, Microsoft Azure portalı oluşturma](./media/event-hubs-create-storage/create-new-blob-container.png)
8. **Depolama hesabı** sayfası menüsünden **Erişim anahtarlarını** seçin ve anahtarın değerini kopyalayın1. **key1**

    Aşağıdaki değerleri Notepad'e veya başka bir geçici konuma kaydedin.
    - Depolama hesabının adı
    - Depolama hesabı için erişim anahtarı
    - Konteynerin adı
