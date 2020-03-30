---
title: Veri & depolama önerileri - Azure Güvenlik Merkezi
description: Bu belge, Azure Güvenlik Merkezi'nde verilerinizi ve Azure SQL hizmetinizi korumanıza ve güvenlik ilkelerine uymanıza yardımcı olan önerileri ele almaktadır.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/19/2019
ms.author: memildin
ms.openlocfilehash: 74ed55e1d460495bfa8d3d4c00bd37bb7f05260e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75552874"
---
# <a name="protect-azure-data-and-storage-services"></a>Azure verilerini ve depolama hizmetlerini koruma
Azure Güvenlik Merkezi olası güvenlik açıklarını tanımladığında, kaynaklarınızı sertleştirmek ve korumak için gerekli denetimleri yapılandırma sürecinde size rehberlik eden öneriler oluşturur.

Bu makalede, Güvenlik Merkezi'nin kaynak güvenliği bölümünün **Veri Güvenliği sayfası** açıklanmaktadır.

Bu sayfada görebileceğiniz önerilerin tam listesi için [Bkz. Veri ve Depolama önerileri.](recommendations-reference.md#recs-datastorage)


## <a name="view-your-data-security-information"></a>Veri güvenliği bilgilerinizi görüntüleme

1. Kaynak **güvenliği hijyeni** bölümünde, **Veri ve depolama kaynaklarını**tıklatın.

    ![Veri & depolama kaynakları](./media/security-center-monitoring/click-data.png)

    **Veri güvenliği** sayfası, veri kaynakları için önerilerle açılır.

    [![Veri Kaynakları](./media/security-center-monitoring/sql-overview.png)](./media/security-center-monitoring/sql-overview.png#lightbox)

    Bu sayfada şunları yapabilirsiniz:

    * Genel **Bakış** sekmesini tıklatın, düzeltilecek tüm veri kaynakları önerilerini listeler. 
    * Her sekmeyi tıklatın ve kaynak türüne göre önerileri görüntüleyin.

    > [!NOTE]
    > Depolama şifrelemesi hakkında daha fazla bilgi için, [veriler için Azure Depolama şifrelemesi'ne](../storage/common/storage-service-encryption.md)bakın.


## <a name="remediate-a-recommendation-on-a-data-resource"></a>Veri kaynağıyla ilgili bir öneriyi düzeltin

1. Kaynak sekmelerinden herhangi birinden bir kaynağı tıklatın. Bilgi sayfası, düzeltilecek önerileri listeler.

    ![Kaynak bilgileri](./media/security-center-monitoring/sql-recommendations.png)

2. Bir tavsiyeyi tıklatın. Öneri sayfası açılır ve öneriyi uygulamak için **Düzeltme adımlarını** görüntüler.

   ![Düzeltme adımları](./media/security-center-monitoring/remediate1.png)

3. **Eylem Eve'yi**tıklatın. Kaynak ayarları sayfası görüntülenir.

    ![Öneriyi etkinleştirme](./media/security-center-monitoring/remediate2.png)

4. Düzeltme **adımlarını** izleyin ve **Kaydet'i**tıklatın.


## <a name="next-steps"></a>Sonraki adımlar

Diğer Azure kaynak türleri için geçerli olan öneriler hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Azure Güvenlik Merkezi'nin güvenlik önerilerinin tam başvuru listesi](recommendations-reference.md)
* [Azure Güvenlik Merkezi'nde makinelerinizi ve uygulamalarınızı koruma](security-center-virtual-machine-protection.md)
* [Azure Güvenlik Merkezi'nde ağınızı koruma](security-center-network-recommendations.md)