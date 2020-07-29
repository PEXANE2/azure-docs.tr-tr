---
title: Veri & depolama önerileri-Azure Güvenlik Merkezi
description: Bu belge, Azure Güvenlik Merkezi 'ndeki, verilerinizi ve Azure SQL hizmetinizi korumanıza ve güvenlik ilkeleriyle uyumlu kalmanıza yardımcı olan önerilere yöneliktir.
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75552874"
---
# <a name="protect-azure-data-and-storage-services"></a>Azure veri ve depolama hizmetlerini koruma
Azure Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde, kaynaklarınızın güvenliğini sağlamak ve korumak için gerekli denetimleri yapılandırma sürecinde size kılavuzluk eden öneriler oluşturur.

Bu makalede, güvenlik merkezi 'nin kaynak güvenliği bölümünün **veri güvenliği sayfası** açıklanmaktadır.

Bu sayfada görebileceğiniz önerilerin tam listesi için bkz. [veri ve depolama önerileri](recommendations-reference.md#recs-datastorage).


## <a name="view-your-data-security-information"></a>Veri güvenliği bilgilerinizi görüntüleyin

1. **Kaynak güvenliği durumu** bölümünde, **veri ve depolama kaynakları**' na tıklayın.

    ![Veri & depolama kaynakları](./media/security-center-monitoring/click-data.png)

    Veri **güvenliği** sayfası, veri kaynaklarına yönelik önerilerle açılır.

    [![Veri kaynakları](./media/security-center-monitoring/sql-overview.png)](./media/security-center-monitoring/sql-overview.png#lightbox)

    Bu sayfada şunları yapabilirsiniz:

    * **Genel bakış** sekmesine tıkladığınızda, düzeltilen tüm veri kaynakları önerileri listelenir. 
    * Her sekmeye tıklayın ve önerileri kaynak türüne göre görüntüleyin.

    > [!NOTE]
    > Depolama şifrelemesi hakkında daha fazla bilgi için bkz. [bekleyen veriler Için Azure depolama şifrelemesi](../storage/common/storage-service-encryption.md).


## <a name="remediate-a-recommendation-on-a-data-resource"></a>Veri kaynağı üzerindeki öneriyi Düzeltme

1. Kaynak sekmeleriyle bir kaynağa tıklayın. Bilgi sayfası, düzeltilen önerilerin listelenmesi halinde açılır.

    ![Kaynak bilgileri](./media/security-center-monitoring/sql-recommendations.png)

2. Bir öneriye tıklayın. Öneri sayfası açılır ve öneriyi uygulamak için **düzeltme adımlarını** görüntüler.

   ![Düzeltme adımları](./media/security-center-monitoring/remediate1.png)

3. **Eylem al**' a tıklayın. Kaynak ayarları sayfası görüntülenir.

    ![Öneriyi etkinleştir](./media/security-center-monitoring/remediate2.png)

4. **Düzeltme adımlarını** Izleyin ve **Kaydet**' e tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

Diğer Azure kaynak türlerine uygulanan öneriler hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Azure Güvenlik Merkezi 'nin güvenlik önerilerinin tam başvuru listesi](recommendations-reference.md)
* [Azure Güvenlik Merkezi'nde makinelerinizi ve uygulamalarınızı koruma](security-center-virtual-machine-protection.md)
* [Azure Güvenlik Merkezi'nde ağınızı koruma](security-center-network-recommendations.md)