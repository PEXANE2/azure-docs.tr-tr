---
title: include dosyası
titleSuffix: Azure
description: include dosyası
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: include
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 00c24212706555667ad4680c086ece24f15ca59a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81678551"
---
1. **Kaynak** > oluştur ' u seçin**Tümünü gör**.

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
