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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75692581"
---
### <a name="create-a-storage-account-for-event-processor-host"></a>Olay İşleyicisi Ana Bilgisayarı için bir depolama hesabı oluşturma
Olay İşleyicisi Ana Bilgisayarı, olay hub’larına ait kalıcı denetim noktalarını ve paralel alımları yöneterek bu olay hub’larından olay almayı basitleştiren akıllı bir aracıdır. Olay İşleyicisi Ana Bilgisayarı, denetim noktası için bir depolama hesabına ihtiyaç duyar. Aşağıdaki örnekte depolama hesabı oluşturma ve erişim için anahtarını alma adımları gösterilmiştir:

1. Azure portal menüsünde **kaynak oluştur**' u seçin.

    ![Kaynak menü öğesi oluşturma, Microsoft Azure portal](./media/event-hubs-create-storage/create-resource.png)

2. **Depolama** > **depolama hesabı**' nı seçin.
   
    ![Depolama hesabı seçin Microsoft Azure portal](./media/event-hubs-create-storage/select-storage-account.png)

3. **Depolama hesabı oluştur** sayfasında aşağıdaki adımları gerçekleştirin: 

   1. **Depolama hesabı adını**girin.
   2. Olay Hub 'ını içeren bir Azure **aboneliği** seçin.
   3. Olay Hub 'ına sahip **kaynak grubunu** seçin veya oluşturun.
   4. Kaynağın oluşturulacağı bir **konum** seçin. 
   5. **İncele ve oluştur**’u seçin.
   
        ![Gözden geçir + oluştur, depolama hesabı oluştur, Microsoft Azure portal](./media/event-hubs-create-storage/review-create.png)

4. **Gözden geçir + oluştur** sayfasında değerleri gözden geçirin ve **Oluştur**'u seçin. 

    ![Depolama hesabı ayarlarını gözden geçirin ve Microsoft Azure portal oluşturun](./media/event-hubs-create-storage/create-storage-account.png)
5. Bildirimlerinde **dağıtımlar başarılı** iletisini gördüğünüzde, depolama hesabı sayfasını açmak Için **Kaynağa Git** ' i seçin. Alternatif olarak, **dağıtım ayrıntılarını** genişletebilir ve ardından kaynak listesinden yeni kaynağı seçebilirsiniz.  

    ![Kaynak, depolama hesabı dağıtımı ' na gidin Microsoft Azure portal](./media/event-hubs-create-storage/go-to-resource.png) 
6. **Kapsayıcıları**seçin.

    ![Bloblar kapsayıcı hizmetini, depolama hesaplarını Microsoft Azure portal seçin](./media/event-hubs-create-storage/select-blob-container-service.png)
7. Üstteki **+ kapsayıcı** ' yı seçin, kapsayıcı Için bir **ad** girin ve **Tamam**' ı seçin. 

    ![Yeni bir blob kapsayıcısı, depolama hesapları, Microsoft Azure portal oluşturun](./media/event-hubs-create-storage/create-new-blob-container.png)
8. **Depolama hesabı** sayfası menüsünden **erişim tuşları** ' nı seçin ve **KEY1**değerini kopyalayın.

    Aşağıdaki değerleri not defteri 'ne veya başka bir geçici konuma kaydedin.
    - Depolama hesabının adı
    - Depolama hesabı için erişim anahtarı
    - Kapsayıcının adı
