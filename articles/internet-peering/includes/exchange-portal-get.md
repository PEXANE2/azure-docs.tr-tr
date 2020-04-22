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
ms.openlocfilehash: 000971878e24c46892aaef1fa0c65237a4219883
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678585"
---
**Eşleme** kaynağı başarıyla dağıtıldıktan sonra, aşağıdaki adımları izleyerek bu kaynağı görüntüleyebilirsiniz.

1. Kaynak **gruplarına**gidin ve **Eşleme** kaynağını oluşturduğunuzda seçtiğiniz kaynak grubunu seçin. Çok fazla kaynak grubunuz varsa **Filtre** kutusunu kullanın.

    > [!div class="mx-imgBorder"]
    > ![Kaynak grupları](../media/setup-direct-get-resourcegroup.png)

1. Oluşturduğunuz **Eşleme** kaynağını seçin.

    > [!div class="mx-imgBorder"]
    > ![Kaynak görünümünü görüntüleme](../media/setup-direct-get-open.png)

1. **Genel Bakış** sayfası, burada gösterildiği gibi üst düzey bilgileri gösterir.

    > [!div class="mx-imgBorder"]
    > ![Peering resource Genel Bakış bölmesi](../media/setup-exchange-get-overview.png)

1. Solda, PeerAsn'ı oluşturduğunuzda gönderilen bilgileri görüntülemek için **ASN bilgilerini** seçin.

    > [!div class="mx-imgBorder"]
    > ![Eşleme kaynağı ASN bilgileri](../media/setup-direct-get-asninfo.png)

1. Solda, **Bağlantılar'ı**seçin. Ekranın üst kısmında, METRO'daki farklı tesislerde ASN ile Microsoft arasındaki bakan bağlantıların bir özetini görürsünüz. Ayrıca, gösterildiği gibi orta bölmedeki **Bağlantıları** seçerek **Genel Bakış** sayfasından bağlantı özetine de erişebilirsiniz.

    > [!div class="mx-imgBorder"]
    > ![Eşleyen kaynak Bağlantıları](../media/setup-exchange-get-connectionssummary.png)

    * **Bağlantı Durumu,** bakan bağlantı kurulumunun durumuna karşılık gelir. Bu alanda görüntülenen durumlar [Exchange bakan izlenecek yol](../walkthrough-exchange-all.md)gösterilen durum diyagramını izleyin.
    * **IPv4 Session State** ve **IPv6 Session State** sırasıyla IPv4 ve IPv6 BGP oturum durumlarına karşılık gelir.  
    * Ekranın üst kısmında bir satır seçtiğinizde, alttaki **Bağlantı** bölümü her bağlantının ayrıntılarını gösterir. **Yapılandırma,** **IPv4 adresi**ve **IPv6 adresini**genişletmek için okları seçin.

    > [!div class="mx-imgBorder"]
    > ![Kaynak görünümünü görüntüleme](../media/setup-exchange-get-connectionsipv4.png)
