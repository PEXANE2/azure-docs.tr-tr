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
ms.openlocfilehash: b967c844ab145074490e931122cbe092d67de0c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75774555"
---
**Peering** kaynağı başarıyla dağıtıldıktan sonra, aşağıdaki adımları izleyerek kaynağı görüntüleyebilirsiniz.

1. Kaynak **gruplarına** gidin ve **Peering** kaynağını oluştururken seçtiğiniz kaynak grubunu tıklatın. Çok fazla kaynak grubunuz varsa *Filtre* alanını kullanabilirsiniz.

    > [!div class="mx-imgBorder"]
    > ![Eşleme kaynak grubu](../media/setup-direct-get-resourcegroup.png)

1. Oluşturduğunuz **Eşleme** kaynağına tıklayın.

    > [!div class="mx-imgBorder"]
    > ![Kaynak görünümünü görüntüleme](../media/setup-direct-get-open.png)

1. **Genel Bakış** sayfası üst düzey bilgileri gösterir. Aşağıda vurgulanan bilgileri gözlemleyin.

    > [!div class="mx-imgBorder"]
    > ![Kaynak görünümünü görüntüleme](../media/setup-exchange-get-overview.png)

1. Solda, PeerAsn oluştururken gönderilen bilgileri görüntülemek için **ASN bilgilerine** tıklayın

    > [!div class="mx-imgBorder"]
    > ![Kaynak görünümünü görüntüleme](../media/setup-direct-get-asninfo.png)

1. Solda, **Bağlantılar'a**tıklayın. Üstte, METRO'daki farklı tesisler de ASN ile Microsoft arasındaki bakan bağlantıların bir özetini gözlemleyin. Ayrıca, yukarıda vurgulandığı gibi orta bölmedeki **Bağlantılar'a** tıklayarak **Genel Bakış** sayfasından bağlantı özetine de ulaşabilirsiniz.

    > [!div class="mx-imgBorder"]
    > ![Kaynak görünümünü görüntüleme](../media/setup-exchange-get-connectionssummary.png)

    * **Bağlantı Durumu,** ayarlanan eşleme bağlantısının durumuna karşılık gelir. Bu alanda görüntülenen [durumlar, Exchange bakan gözden geçirme](../walkthrough-exchange-all.md) de gösterilen durum diyagramını izleyin
    * **IPv4 Session State** ve **IPv6 Session State** sırasıyla IPv4 ve IPv6 BGP oturum durumlarına karşılık gelir.  
    * Üstte bir satır seçtiğinizde, alttaki ***Bağlantı*** bölümü her bağlantının ayrıntılarını gösterir. Alt bölümler ***yapılandırma,*** ***IPv4 adresi*** ve ***IPv6 adresini*** genişletmek için ok işaretlerine tıklayabilirsiniz

    > [!div class="mx-imgBorder"]
    > ![Kaynak görünümünü görüntüleme](../media/setup-exchange-get-connectionsipv4.png)
