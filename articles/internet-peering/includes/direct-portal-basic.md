---
title: dosya dahil etme
titleSuffix: Azure
description: dosya dahil etme
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: include
ms.date: 5/22/2020
ms.author: derekol
ms.openlocfilehash: e2804c8f9b1af89ac0ea86ec14136df66d900060
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83846199"
---
1. **Kaynak oluştur**' u seçin  >  **Tümünü gör**.

    > [!div class="mx-imgBorder"]
    > ![Eşleme ara](../media/setup-seeall.png)

1. Arama kutusunda **eşleme** araması yapın ve klavyenizde **ENTER** ' u seçin. Sonuçlardan bir **eşleme** kaynağı seçin.

    > [!div class="mx-imgBorder"]
    > ![Eşlemeyi Başlat](../media/setup-launch.png)

1. **Eşleme** başladıktan sonra, ayrıntıları anlamak için sayfayı gözden geçirin. Hazırsanız **Oluştur**' u seçin.

    > [!div class="mx-imgBorder"]
    > ![Eşleme Oluştur](../media/setup-create.png)

1. **Eşleme oluştur** sayfasında, **temel bilgiler** sekmesinde, kutulara aşağıda gösterildiği gibi kutuyu girin.

    > [!div class="mx-imgBorder"]
    > ![Eşleme temelleri sekmesi](../media/setup-basics-tab.png)

    * Azure **aboneliğinizi**seçin.
    * **Kaynak grubu**için, açılan listeden var olan bir kaynak grubunu seçebilir veya **Yeni oluştur**' u seçerek yeni bir grup oluşturabilirsiniz. Bu örnek için yeni bir kaynak grubu oluşturacağız.
    * **Ad** , kaynak adına karşılık gelir ve seçtiğiniz herhangi bir şey olabilir.
    * Mevcut bir kaynak grubunu seçtiyseniz **bölge** , oto seçilir. Yeni bir kaynak grubu oluşturmayı seçerseniz, kaynağın bulunmasını istediğiniz Azure bölgesini de seçmeniz gerekir.

        > [!NOTE]
        > Kaynak grubunun bulunduğu bölge, Microsoft ile eşleme oluşturmak istediğiniz konumdan bağımsızdır. Ancak, en yakın Azure bölgelerinde bulunan kaynak grupları içindeki eşleme kaynaklarınızı düzenlemek en iyi uygulamadır. Örneğin, Ashburn içindeki eşlemeler için Doğu ABD veya Doğu ABD2 içinde bir kaynak grubu oluşturabilirsiniz.

    * **Eş ASN** kutusunda ASN 'nizi seçin.

        > [!IMPORTANT]
        > * Bir eşleme isteği göndermeden önce, yalnızca onay durumu ile bir ASN seçebilirsiniz. PeerAsn isteğinizi gönderdikten sonra, ASN ilişkisinin onaylanabilmesi için 12 saat bekleyin. Seçtiğiniz ASN, doğrulama bekliyor ise, bir hata iletisi görürsünüz. 
        > * Seçmeniz gereken ASN 'yi görmüyorsanız doğru aboneliği seçtiğinizden emin olun. Bu durumda, [EŞDÜZEY ASN 'Yi Azure aboneliğine ilişkilendir](../howto-subscription-association-portal.md)' i kullanarak peerasn 'yi zaten oluşturmuş olup olmadığınızı kontrol edin.

        > [!div class="mx-imgBorder"]
        > ![Eşleme temelleri dolduruldu](../media/setup-direct-basics-filled-tab.png)

    * Ileri ' yi seçin: devam etmek için **yapılandırma >** .
