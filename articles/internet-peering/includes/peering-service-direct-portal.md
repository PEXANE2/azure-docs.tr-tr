---
title: include dosyası
titleSuffix: Azure
description: include dosyası
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: include
ms.date: 3/18/2020
ms.author: derekol
ms.openlocfilehash: e5804aa1b005e670d8b430b1c0a3bd62efd0bb06
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687035"
---
1. Azure Bakma Hizmeti için etkinleştirmek istediğiniz bir karşılama bağlantısı seçin. Sonra seçin **...**  >  **Bağlantıyı edin.**
    > [!div class="mx-imgBorder"]
    > ![Peering bağlantısı Edit bağlantısı](../media/setup-direct-modify-editconnection.png)
1. **Peering Service için Kullanım**altında, **Etkin'i** seçin ve ardından **Kaydet'i**seçin.
    > [!div class="mx-imgBorder"]
    > ![Peering bağlantısı Arayı Etkinleştir eki](../media/setup-direct-modify-editconnectionsettings-peering-service.png)
1. Genel **Bakış** ekranında dağıtım ayrıntılarını görürsünüz. Dağıtımınız bittikten sonra **kaynağa git'i**seçin.
    > [!div class="mx-imgBorder"]
    > ![Dağıtımınız tamamlandı](../media/setup-direct-modify-overview-deployment-complete.png)

1. Kayıtlı **Önekleri** bölmesine kayıtlı **önek ekle'yi**seçin.
    > [!div class="mx-imgBorder"]
    > ![Kayıtlı önek ekleme](../media/setup-direct-modify-add-registered-prefix.png)
1. **Ad** ve **Önek** seçip Kaydet'i seçerek bir önek **kaydedin.**
    > [!div class="mx-imgBorder"]
    >  ![Önek kaydetme](../media/setup-direct-modify-register-a-prefix.png) 

1. Bir önek oluşturulduktan sonra, **kayıtlı öneekler**listesinde görürsünüz. Daha **Name** fazla ayrıntı görmek için önek adı'nı seçin.
    > [!div class="mx-imgBorder"]
    > ![Kayıtlı önekler ve bağlantılar](../media/setup-direct-modify-registered-prefixes.png)
1. Kayıtlı önek sayfasında, her önek için **Önek anahtarını** içeren tüm ayrıntıları görürsünüz. Bu anahtar, sağlayıcı ISS'den bu önek ayrılan müşteriye sağlanmalıdır. Müşteri daha sonra bu anahtarı kullanarak kendi aboneliği içinde kendi önek kaydedebilirsiniz.
    > [!div class="mx-imgBorder"]
    > ![Önek tuşu ile önek](../media/setup-direct-modify-registered-prefix-detail.png)