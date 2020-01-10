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
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75774646"
---
1. **Kaynak oluştur** > **Tümünü gör**' e tıklayın.

    > [!div class="mx-imgBorder"]
    > ![arama eşlemesi](../media/setup-seeall.png)

1. Arama kutusunda *eşleme* araması yapın ve klavyenizde *ENTER* tuşuna basın. Sonuçlardan **eşleme** kaynağı ' na tıklayın.

    > [!div class="mx-imgBorder"]
    > ![başlatma eşlemesi](../media/setup-launch.png)

1. **Eşleme** başlatıldıktan sonra, ayrıntıları anlamak için sayfayı gözden geçirin. Hazırlanıyor, **Oluştur**' a tıklayın.

    > [!div class="mx-imgBorder"]
    > ![eşleme oluşturma](../media/setup-create.png)

1. **Eşleme oluştur** sayfasında, **temel bilgiler** sekmesinde, alanları aşağıda gösterildiği gibi doldurun.

    > [!div class="mx-imgBorder"]
    > ![eşleme temelleri](../media/setup-basics-tab.png)

    * Azure **aboneliğinizi**seçin.
    * **Kaynak grubu**için, açılan listeden var olan bir kaynak grubunu seçebilir veya **Yeni oluştur**' a tıklayarak yeni bir grup oluşturabilirsiniz. Bu örnek için yeni bir kaynak grubu oluşturacağız.
    * **Ad** , kaynak adına karşılık gelir ve seçtiğiniz herhangi bir şey olabilir.
    * Yukarıdaki adımda var olan bir kaynak grubunu seçtiyseniz **bölge** otomatik olarak seçilir. Yeni bir kaynak grubu oluşturmayı seçerseniz, kaynağın bulunmasını istediğiniz Azure bölgesini de seçmeniz gerekir. Doğu ABD

        > [!NOTE]
        > Kaynak grubunun bulunduğu bölge, Microsoft ile eşleme oluşturmak istediğiniz konumdan bağımsızdır. Ancak, en yakın Azure bölgelerinde bulunan kaynak grupları içindeki eşleme kaynaklarınızı düzenlemek en iyi uygulamadır. Örneğin: Ashburn içindeki eşlemeler için *Doğu ABD* veya *Doğu ABD2* içinde bir kaynak grubu oluşturabilirsiniz

    * **EŞDÜZEY ASN** alanında ASN 'nizi seçin.

        > [!IMPORTANT]
        > * Bir eşleme isteği göndermeden önce yalnızca "Onaylandı" olarak bir ASN 'yi seçebilirsiniz. PeerAsn isteğinizi gönderdikten sonra, ASN ilişkisinin "onaylanmış" olması için 12 saat bekleyin. Seçtiğiniz ASN, doğrulama bekliyor ise, bir hata iletisi görürsünüz. 
        > * Seçmeniz gereken ASN 'yi görmüyorsanız, doğru aboneliği seçtiğinizden emin olun. Bu durumda, [EŞDÜZEY ASN 'Yi Azure aboneliğine ilişkilendir](../howto-subscription-association-portal.md)' i kullanarak peerasn oluşturmuş olup olmadığınızı kontrol edin.

        > [!div class="mx-imgBorder"]
        > ![eşleme temelleri doldurulmuş](../media/setup-direct-basics-filled-tab.png)

    * Ileri ' ye tıklayın: devam etmek için **yapılandırma >** .
