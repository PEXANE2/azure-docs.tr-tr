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
ms.openlocfilehash: 79cf4b2edd1a25df427cf15f9ee7f2f331ebd2df
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75774646"
---
1. **Kaynak** > Oluştur'u tıklatın**Tümlerini Görün**.

    > [!div class="mx-imgBorder"]
    > ![Arama Eşleme](../media/setup-seeall.png)

1. Arama kutusunda *Akran* araması yapın ve klavyenizde *Enter* tuşuna basın. Sonuçlardan, **Peering** kaynağını tıklatın.

    > [!div class="mx-imgBorder"]
    > ![Fırlatma Eşleme](../media/setup-launch.png)

1. **Peering** başlatıldıktan sonra, ayrıntıları anlamak için sayfayı gözden geçirin. Hazır olduğunuzda **Oluştur'u**tıklatın.

    > [!div class="mx-imgBorder"]
    > ![Eşleme Oluşturma](../media/setup-create.png)

1. **Temel Bilgiler** sekmesinin altında, **Bir Eşleme Oluştur** sayfasında, aşağıda gösterildiği gibi alanları doldurun.

    > [!div class="mx-imgBorder"]
    > ![Peering Temelleri](../media/setup-basics-tab.png)

    * Azure **Aboneliğinizi**seçin.
    * **Kaynak grubu**için, açılan kaynak grubundan varolan bir kaynak grubu seçebilir veya **yeni oluştur'u**tıklatarak yeni bir grup oluşturabilirsiniz. Bu örnek için yeni bir kaynak grubu oluşturacağız.
    * **Ad** kaynak adına karşılık gelir ve seçtiğiniz herhangi bir şey olabilir.
    * Yukarıdaki adımda varolan bir kaynak grubu seçtiyseniz **bölge** otomatik olarak seçilir. Yeni bir kaynak grubu oluşturmayı seçtiyseniz, kaynağın ikamet etmesini istediğiniz Azure bölgesini de seçmeniz gerekir. Doğu ABD

        > [!NOTE]
        > Kaynak grubunun bulunduğu bölge, Microsoft ile eşleme oluşturmak istediğiniz konumdan bağımsızdır. Ancak, en yakın Azure bölgelerinde bulunan kaynak grupları içinde bakan kaynaklarınızı düzenlemek en iyi yöntemdir. Örneğin: Ashburn'deki bakışlar için *Doğu ABD* veya Doğu *US2'de* bir kaynak grubu oluşturabilirsiniz

    * **Eş ASN** alanında ASN'nizi seçin.

        > [!IMPORTANT]
        > * Bir eşleme isteği göndermeden önce yalnızca "Onaylı" olarak Doğrulama Durumu olan bir ASN seçebilirsiniz. PeerAsn isteğinizi gönderdiyseniz, ASN ilişkisinin "Onaylandı" olması için 12 saat kadar bekleyin. Seçtiğiniz ASN onay bekliyorsa bir hata iletisi görürsünüz. 
        > * Seçmeniz gereken ASN'yi görmüyorsanız, doğru aboneliği seçip seçmediğinizi kontrol edin. Bu nedenle, Azure Aboneliği'ne Eş [Eş ASN'yi](../howto-subscription-association-portal.md)kullanarak PeerAsn'ı zaten oluşturmuş olup olmadığını kontrol edin.

        > [!div class="mx-imgBorder"]
        > ![Peering Temelleri dolu](../media/setup-direct-basics-filled-tab.png)

    * İleri ' ye tıklayın **: Yapılandırma >** devam etmek.
