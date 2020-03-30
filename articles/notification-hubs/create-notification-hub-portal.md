---
title: Azure portalını kullanarak bir Azure bildirim merkezi oluşturma | Microsoft Dokümanlar
description: Bu eğitimde, Azure portalını kullanarak bir Azure bildirim merkezi oluşturmayı öğrenirsiniz.
services: notification-hubs
author: sethmanheim
manager: femila
editor: jwargo
ms.service: notification-hubs
ms.workload: mobile
ms.topic: quickstart
ms.date: 02/14/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 02/14/2019
ms.openlocfilehash: 3aeeb989d15dc74849c85fa58cbefa891809f3c5
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80347083"
---
# <a name="quickstart-create-an-azure-notification-hub-in-the-azure-portal"></a>Hızlı başlatma: Azure portalında bir Azure bildirim merkezi oluşturma 
Azure Notification Hubs, herhangi bir arka uçtan (bulut ya da şirket içi) herhangi bir platforma (iOS, Android, Windows, Kindle, Baidu vb.) bildirim göndermenize olanak tanıyan, kullanımı kolay ve ölçeği artırılmış bir gönderme altyapısı sağlar. Hizmet hakkında daha fazla bilgi için Azure [Bildirim Hub'ları nedir?](notification-hubs-push-notification-overview.md)

Bu hızlı başlangıçta, Azure portalında bir bildirim merkezi oluşturursunuz. İlk bölümde, bildirim hub'ları ad alanı ve bu ad alanında bir hub oluşturmak için adımlar verir. İkinci bölüm, varolan bir Bildirim Hub'ları ad alanında bir bildirim hub'ı oluşturmak için adımlar verir. 

## <a name="create-a-namespace-and-a-notification-hub"></a>Ad alanı ve bildirim merkezi oluşturma
Bu bölümde, ad alanında bir ad alanı ve hub oluşturursunuz. 

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

## <a name="create-a-notification-hub-in-an-existing-namespace"></a>Varolan bir ad alanında bildirim merkezi oluşturma
Bu bölümde, varolan bir ad alanında bir bildirim hub'ı oluşturursunuz. 

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Sol menüdeki **Tüm hizmetleri** seçin, Bildirim Hub'ını arayın , **Bildirim Hub'ı Ad Alanları'nın** yanındaki **yıldız** (`*`) seçin ve sol menüdeki **SıK Kullanılanlar** bölümüne ekleyin. **Notification Hub** **Bildirim Merkezi Ad Alanlarını**seçin. 

      ![Azure portalı - Bildirim Merkezi Ad Alanları'nı seçin](./media/create-notification-hub-portal/select-notification-hub-namespaces-all-services.png)
3. Bildirim **Merkezi Ad Alanları** sayfasında, listeden ad alanınızı seçin. 

      ![Listeden ad alanınızı seçin](./media/create-notification-hub-portal/select-namespace.png)
1. Bildirim **Hub'ı Ad Alanı** sayfasında araç çubuğunda **Hub Ekle'yi** seçin. 

      ![Bildirim Hub Ad Alanları - Hub ekle düğmesi](./media/create-notification-hub-portal/add-hub-button.png)
4. Yeni **Bildirim Hub** sayfasında, bildirim hub'ı için bir ad girin ve **Tamam'ı**seçin.

      ![Yeni Bildirim Hub sayfası -hub'ınız için bir ad >](./media/create-notification-hub-portal/new-notification-hub-page.png)
4. Yeni hub'ın dağıtım durumunu görmek için en üstteki **Bildirimler** 'i (Çan simgesi) seçin. Bildirim penceresini kapatmak için sağ köşede **X'i** seçin. 

      ![Dağıtım bildirimi](./media/create-notification-hub-portal/deployment-notification.png)
5. Listede yeni hub'ınızı görmek için **Bildirim Merkezi Ad Alanları** web sayfasını yenileyin. 

      ![Azure portalı - bildirimler -> Kaynağa git](./media/create-notification-hub-portal/new-hub-in-list.png)
6. Bildirim merkezinizin ana sayfasını görmek için **bildirim merkezinizi** seçin. 

      ![Azure portalı - bildirimler -> Kaynağa git](./media/create-notification-hub-portal/hub-home-page.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, bir bildirim hub'ı oluşturdunuz. Hub'ı platform bildirim sistemi (PNS) ayarlarıyla nasıl yapılandırıştırılamayı öğrenmek için [bkz.](configure-notification-hub-portal-pns-settings.md) 