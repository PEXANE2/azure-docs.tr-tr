---
title: include dosyası
description: include dosyası
services: notification-hubs
author: jwargo
ms.service: notification-hubs
ms.topic: include
ms.date: 01/17/2019
ms.author: jowargo
ms.custom: include file
ms.openlocfilehash: 5afcc8e4524a0e8353766ba239d5ab9161b29d86
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67509134"
---
1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Sol menüdeki **Tüm hizmetleri** seçin ve ardından **Mobil** **bölümündeBildirim Hub'larını** seçin. Hizmeti sol menüdeki **SıK Kullanılanlar** bölümüne eklemek için hizmet adının yanındaki yıldız simgesini seçin. **Sık Kullanılanlar'a** **Bildirim Hub'ları** ekledikten sonra sol menüde seçin.

      ![Azure portalı - Bildirim Hub'larını seçin](./media/notification-hubs-portal-create-new-hub/all-services-select-notification-hubs.png)

1. Bildirim **Hub'ları** sayfasında araç çubuğuna **Ekle'yi** seçin.

      ![Bildirim Hub'ları - Araç çubuğu ekle düğmesi](./media/notification-hubs-portal-create-new-hub/add-toolbar-button.png)

1. Bildirim **Merkezi** sayfasında aşağıdaki adımları yapın:

    1. **Bildirim**Hub'ına bir ad girin.  

    1. Yeni bir **ad alanı oluştur'a**bir ad girin. Ad alanı bir veya daha fazla hub içerir.

    1. **Konum** açılır liste kutusundan bir değer seçin. Bu değer, hub'ı oluşturmak istediğiniz konumu belirtir.

    1. **Kaynak Grubu'nda**varolan bir kaynak grubu seçin veya yeni bir kaynak grubu için bir ad oluşturun.

    1. **Oluştur'u**seçin.

        ![Azure portalı - Bildirim hub'ı özelliklerini ayarlama](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)

1. **Bildirimler** 'i (çan simgesi) seçin ve ardından **kaynağa Git'i**seçin. Ayrıca **Bildirim Hub'ları** sayfasındaki listeyi yenileyebilir ve hub'ınızı seçebilirsiniz.

      ![Azure portalı - bildirimler -> Kaynağa git](./media/notification-hubs-portal-create-new-hub/go-to-notification-hub.png)

1. Listeden **Erişim İlkeleri**'ni seçin. İki bağlantı dizesinin sizin için kullanılabildiğini unutmayın. Anında iletme bildirimlerini işlemek için daha sonra bunlara ihtiyacınız olur.

      >[!IMPORTANT]
      >Uygulamanızda **VarsayılanFullSharedAccessSignature** ilkesini *kullanmayın.* Bu sadece arka uçta kullanılmak üzere dir.
      >

      ![Azure portalı - Bildirim hub'ı bağlantı dizeleri](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)
