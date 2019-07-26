---
title: Azure NetApp Files için kaynak sınırları | Microsoft Docs
description: NetApp hesapları, kapasite havuzları, birimler, anlık görüntüler ve Temsilcili alt ağ için sınırlar da dahil olmak üzere Azure NetApp Files kaynak sınırlarını açıklar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: b-juche
ms.openlocfilehash: fe8d46c897ef68563f2e0e5a1da106174ae504c5
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68424076"
---
# <a name="resource-limits-for-azure-netapp-files"></a>Azure NetApp Files için kaynak sınırları

Azure NetApp Files için kaynak sınırlarını anlamak, birimlerinizi yönetmenize yardımcı olur.

## <a name="resource-limits"></a>Kaynak sınırları

Aşağıdaki tabloda Azure NetApp Files için kaynak sınırları açıklanmaktadır:

|  Resource  |  Varsayılan limit  |  Destek isteği aracılığıyla ayarlanabilir  |
|----------------|---------------------|--------------------------------------|
|  Azure aboneliği başına NetApp hesabı sayısı   |  10    |  Evet   |
|  NetApp hesabı başına kapasite havuzlarının sayısı   |    25     |   Evet   |
|  Kapasite havuzu başına birim sayısı     |    500   |    Evet     |
|  Birim başına anlık görüntü sayısı       |    255     |    Hayır        |
|  Azure sanal ağı başına Azure NetApp Files (Microsoft. NetApp/birimler) için temsilci seçilen alt ağ sayısı    |   1\.   |    Hayır    |
|  Azure NetApp Files ile VNet 'te kullanılmakta olan en fazla IP sayısı (eşlenen VNET 'Ler dahil)    |    1000   |    Hayır   |
|  Tek bir kapasite havuzunun en küçük boyutu   |  4 TiB     |    Hayır  |
|  Tek bir kapasite havuzunun en büyük boyutu    |  500 TiB   |   Hayır   |
|  Tek bir birimin en küçük boyutu    |    100 GiB    |    Hayır    |
|  Tek bir birim için en fazla atanan kota *   |   92 TiB   |    Hayır   |
|  Tek bir birimin en büyük boyutu *     |    100 TiB    |    Hayır       |

\* Bir birim, el ile oluşturulabilir veya en düşük düzeyde 92 TiB olarak yeniden boyutlandırılabilir. Ancak, bir birim fazla kullanım senaryosunda 100 TiB 'ye kadar büyüyebilir. Kapasite fazla kullanımı hakkındaki ayrıntılar için bkz. [maliyet modeli Azure NetApp Files](azure-netapp-files-cost-model.md) . 

## <a name="request-limit-increase"></a>İstek sınırı artışı 

Yukarıdaki tablodan ayarlanabilir sınırları artırmak için bir Azure destek isteği oluşturabilirsiniz. 

Azure portal gezinti düzleminden: 

1. **Yardım ve destek**' e tıklayın.
2. **+ Yeni destek isteği**' ne tıklayın.
3. Temel bilgiler sekmesinde, aşağıdaki bilgileri sağlayın: 
    1. Sorun türü: **Hizmet ve abonelik sınırlarını (kotalar)** seçin.
    2. Abonelikler: Kotanın artması gereken kaynak için aboneliği seçin.
    3. Kota türü: Depolama **seçin: Azure NetApp Files sınırları**.
    4. İleri **' ye tıklayın: Çözümler**.
4. Ayrıntılar sekmesinde:
    1. Açıklama kutusunda, ilgili kaynak türü için aşağıdaki bilgileri sağlayın:

        |  Resource  |    Üst kaynaklar      |    İstenen yeni sınırlar     |    Kota artışı nedeni       |
        |----------------|------------------------------|---------------------------------|------------------------------------------|
        |  Hesap |  *Abonelik KIMLIĞI*   |  *İstenen yeni en büyük **Hesap** numarası*    |  *İstekte hangi senaryo veya kullanım örneği istendi?*  |
        |  Havuz    |  *Abonelik KIMLIĞI, hesap URI 'SI*  |  *İstenen yeni en büyük **Havuz** numarası*   |  *İstekte hangi senaryo veya kullanım örneği istendi?*  |
        |  Birim  |  *Abonelik KIMLIĞI, hesap URI 'si, havuz URI 'SI*   |  *İstenen yeni en yüksek **birim** numarası*     |  *İstekte hangi senaryo veya kullanım örneği istendi?*  |

    2. Uygun destek yöntemini belirtin ve sözleşme bilgilerinizi sağlayın.

    3. İleri **' ye tıklayın: İsteği oluşturmak için** + oluştur ' a bakın. 


## <a name="next-steps"></a>Sonraki adımlar  

- [Azure NetApp Files depolama hiyerarşisini anlayın](azure-netapp-files-understand-storage-hierarchy.md)
- [Azure NetApp Files için maliyet modeli](azure-netapp-files-cost-model.md)
