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
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678551"
---
1. **Kaynak** > Oluştur'u Seçin**Tümünü gör.**

    > [!div class="mx-imgBorder"]
    > ![Arama Eşleme](../media/setup-seeall.png)

1. Arama kutusunda **Eşle'yi** arayın ve klavyenizde **Enter'u** seçin. Sonuçlardan bir **Peering** kaynağı seçin.

    > [!div class="mx-imgBorder"]
    > ![Fırlatma Eşleme](../media/setup-launch.png)

1. **Peering** başladıktan sonra, ayrıntıları anlamak için sayfayı gözden geçirin. Hazır olduğunuzda **Oluştur'u**seçin.

    > [!div class="mx-imgBorder"]
    > ![Eşleme Oluşturma](../media/setup-create.png)

1. **Eşleme Oluştur** sayfasında, Temel **Bilgiler** sekmesinde, burada gösterildiği gibi kutuları doldurun.

    > [!div class="mx-imgBorder"]
    > ![TemelLeri Eşleme sekmesi](../media/setup-basics-tab.png)

    * Azure **Aboneliğinizi**seçin.
    * **Kaynak grubu**için açılan listeden varolan bir kaynak grubu seçebilir veya yeni **oluştur**seçeneğini seçerek yeni bir grup oluşturabilirsiniz. Bu örnek için yeni bir kaynak grubu oluşturacağız.
    * **Ad** kaynak adına karşılık gelir ve seçtiğiniz herhangi bir şey olabilir.
    * Varolan bir kaynak grubu seçtiyseniz **bölge** otomatik olarak seçilir. Yeni bir kaynak grubu oluşturmayı seçtiyseniz, kaynağın ikamet etmesini istediğiniz Azure bölgesini de seçmeniz gerekir.

        > [!NOTE]
        > Kaynak grubunun bulunduğu bölge, Microsoft ile eşleme oluşturmak istediğiniz konumdan bağımsızdır. Ancak, en yakın Azure bölgelerinde bulunan kaynak grupları içinde bakan kaynaklarınızı düzenlemek en iyi yöntemdir. Örneğin, Ashburn'deki bakışlar için Doğu ABD veya Doğu US2'de bir kaynak grubu oluşturabilirsiniz.

    * **Eş ASN** kutusunda ASN'nizi seçin.

        > [!IMPORTANT]
        > * Yalnızca bir eşleme isteği göndermeden önce OnayLandığı gibi Onay Durumu olan bir ASN seçebilirsiniz. PeerAsn isteğinizi yeni gönderdiyseniz, ASN ilişkisinin onaylanması için 12 saat kadar bekleyin. Seçtiğiniz ASN onay bekliyorsa, bir hata iletisi görürsünüz. 
        > * Seçmeniz gereken ASN'yi görmüyorsanız, doğru aboneliği seçip seçmediğinizi kontrol edin. Bu nedenle, [Azure aboneliğine Eş Eş ASN'yi](../howto-subscription-association-portal.md)kullanarak PeerAsn'ı zaten oluşturmuş olup olmadığını kontrol edin.

        > [!div class="mx-imgBorder"]
        > ![Doldurulmuş Peering Temelleri](../media/setup-direct-basics-filled-tab.png)

    * **Sonrakini** Seçin : Yapılandırma >devam edin.
