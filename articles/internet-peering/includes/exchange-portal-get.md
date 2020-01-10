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
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75774555"
---
**Eşleme** kaynağı başarıyla dağıtıldıktan sonra aşağıdaki adımları izleyerek görüntüleyebilirsiniz.

1. **Kaynak grupları** ' na gidin ve **eşleme** kaynağı oluştururken seçtiğiniz kaynak grubuna tıklayın. Çok fazla kaynak grubunuz varsa *filtre* alanını kullanabilirsiniz.

    > [!div class="mx-imgBorder"]
    > ![eşleme kaynak grubu](../media/setup-direct-get-resourcegroup.png)

1. Oluşturduğunuz **eşleme** kaynağına tıklayın.

    > [!div class="mx-imgBorder"]
    > ![eşleme kaynak görünümü](../media/setup-direct-get-open.png)

1. **Genel bakış** sayfası, üst düzey bilgileri gösterir. Aşağıda vurgulanan bilgileri gözlemleyin.

    > [!div class="mx-imgBorder"]
    > ![eşleme kaynak görünümü](../media/setup-exchange-get-overview.png)

1. Sol tarafta, **ASN bilgilerine** tıklayarak peerasn oluştururken gönderilen bilgileri görüntüleyin

    > [!div class="mx-imgBorder"]
    > ![eşleme kaynak görünümü](../media/setup-direct-get-asninfo.png)

1. Sol tarafta **Bağlantılar**' a tıklayın. Aynı metro 'daki farklı tesislerde ASN ve Microsoft arasındaki eşleme bağlantılarının bir özetini gözlemleyin. Yukarıda vurgulanan sırada orta bölmedeki **Bağlantılar** ' a tıklayarak da **genel bakış** sayfasından bağlantı Özeti sayfasına gelebiliriz.

    > [!div class="mx-imgBorder"]
    > ![eşleme kaynak görünümü](../media/setup-exchange-get-connectionssummary.png)

    * **Bağlantı durumu** , eşleme bağlantısı ayarlanan durumuna karşılık gelir. Bu alanda gösterilen durumlar, [Exchange eşlemesi](../walkthrough-exchange-all.md) Kılavuzu 'nda gösterilen durum diyagramını izler
    * **IPv4 oturum durumu** ve **IPv6 oturum durumu** SıRASıYLA IPv4 ve IPv6 BGP oturum durumlarına karşılık gelir.  
    * Üstteki bir satırı seçtiğinizde, alt taraftaki ***bağlantı*** bölümünde her bağlantı için ayrıntılar gösterilir. Alt bölüm ***yapılandırmasını***, ***IPv4 adresini*** ve ***IPv6 adresini*** genişletmek için ok işaretlerine tıklayabilirsiniz

    > [!div class="mx-imgBorder"]
    > ![eşleme kaynak görünümü](../media/setup-exchange-get-connectionsipv4.png)
