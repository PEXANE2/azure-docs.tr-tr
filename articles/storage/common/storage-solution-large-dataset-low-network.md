---
title: Düşük veya ağ bant genişliğine sahip büyük veri kümeleri için Azure veri aktarımı seçenekleri | Microsoft Docs
description: Ortamınızda ağ bant genişliği olmadan sınırlı olduğunuzda ve büyük veri kümelerini aktarmayı planlarken, veri aktarımı için bir Azure çözümü seçme hakkında bilgi edinin.
services: storage
author: alkohli
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: alkohli
ms.openlocfilehash: 9b83ba106b35a0a3abd035e85f60c4c39bbadd3b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98704649"
---
# <a name="data-transfer-for-large-datasets-with-low-or-no-network-bandwidth"></a>Bant genişliği düşük olduğunda veya hiç olmadığında büyük veri kümeleri için veri aktarımı
 
Bu makalede, ortamınızda ağ bant genişliği olmadan sınırlı olduğunuzda ve büyük veri kümelerini aktarmayı planladığınızda veri aktarımı çözümlerine genel bir bakış sağlanır. Makalede ayrıca önerilen veri aktarımı seçenekleri ve bu senaryonun ilgili anahtar özelliği matrisi de açıklanmaktadır.

Tüm kullanılabilir veri aktarımı seçeneklerine genel bir bakış için bkz. [Azure veri aktarımı çözümü seçme](storage-choose-data-transfer-solution.md).

## <a name="offline-transfer-or-network-transfer"></a>Çevrimdışı aktarım veya ağ aktarımı

Büyük veri kümeleri, birkaç veri PBs için birkaç TBs olduğunu kapsıyor. Ağ bant genişliği olmadan sınırlı, ağınız yavaş veya güvenilir değil. Ayrıca:

- Internet servis sağlayıcılarınızdan (ISS) ağ aktarımı maliyetleriyle sınırlı olursunuz.
- Güvenlik veya kuruluş ilkeleri, hassas verilerle ilgilenirken giden bağlantılara izin vermez.

Yukarıdaki tüm örneklerde, bir kerelik toplu veri aktarımı yapmak için fiziksel bir cihaz kullanın. Microsoft tarafından sağlanan Data Box Disk, Data Box, Data Box Heavy cihazları seçin veya kendi disklerinizi kullanarak Içeri/dışarı aktarın.

Fiziksel bir cihazın doğru seçenek olup olmadığını doğrulamak için aşağıdaki tabloyu kullanın. Çeşitli kullanılabilir bant genişlikleri (%90 kullanım varsayılıyor) için ağ veri aktarımı için öngörülen zamanı gösterir. Ağ aktarımının çok yavaş olması için yansıtıldıysanız fiziksel bir cihaz kullanmanız gerekir.  

![Ağ aktarımı veya çevrimdışı aktarım](media/storage-solution-large-dataset-low-network/storage-network-or-offline-transfer.png)

## <a name="recommended-options"></a>Önerilen seçenekler

Bu senaryoda kullanılabilen seçenekler, Azure Data Box çevrimdışı aktarma veya Azure Içeri/dışarı aktarma için cihazlardır.

- **Azure Data Box ailesi çevrimdışı aktarımlar için** : Microsoft tarafından sağlanan Data Box cihazlarındaki cihazları, zaman, ağ kullanılabilirliği veya maliyetlerle sınırlı olduğunuzda Azure 'a büyük miktarlarda veri taşımak için kullanın. Robocopy gibi araçlarla şirket içindeki verileri kopyalayın. Aktarmak istediğiniz verilerin boyutuna göre Data Box Disk, Data Box veya Data Box Heavy çözümlerinden birini seçebilirsiniz.
- **Azure içeri/dışarı aktarma** – büyük miktarlarda verileri Azure Blob depolama ve Azure dosyaları 'na güvenli bir şekilde aktarmak için kendi disk sürücülerinizi göndererek Azure Içeri/dışarı aktarma hizmetini kullanın. Bu hizmeti ayrıca Azure Blob depolama alanınızdaki verileri disk sürücülerine aktarıp şirket içi ortamınıza göndermek için de kullanabilirsiniz.

## <a name="comparison-of-key-capabilities"></a>Anahtar özellikleri karşılaştırması

Aşağıdaki tabloda, temel özellikler arasındaki farklar özetlenmiştir.

|                                     |    Data Box Disk      |    Data Box                                      |    Data Box Heavy              |    İçeri/Dışarı Aktarma                       |
|-------------------------------------|---------------------------------|--------------------------------------------------|------------------------------------------|----------------------------------------|
|    **Veri boyutu**                    |    35 TBs 'ye kadar                 |    Cihaz başına 80 TBs 'ye kadar                       |    Cihaz başına 800 TB 'a kadar               |    Değişken                            |
|    **Veri türü**                    |    Azure Blobları                  |    Azure Blobları<br>Azure Dosyaları                    |    Azure Blobları<br>Azure Dosyaları            |    Azure Blobları<br>Azure Dosyaları          |
|    **Form faktörü**                  |    sipariş başına 5 SSD             |    1 X 50-lbs. sipariş başına masaüstü boyutunda cihaz    |    1 X ~ 500-lbs. sipariş başına büyük cihaz    |    Sipariş başına en fazla 10 HDD/SSD        |
|    **İlk kurulum saati**           |    Düşük <br>(15 dakika)            |    Düşük-orta <br> (<30 dakika)               |    Orta<br>(1-2 saat)               |    Orta-zor<br>değişken |
|    **Verileri Azure 'a gönderme**           |    Yes                          |    Yes                                           |    Yes                                   |    Yes                                 |
|    **Verileri Azure'dan dışarı aktarma**       |    Hayır                           |    Hayır                                            |    Hayır                                    |    Yes                                 |
|    **Şifreleme**                   |    AES 128 bit                  |    AES 256 bit                                   |    AES 256 bit                           |    AES 128 bit                         |
|    **Donanım**                     |     Microsoft tarafından sağlanan          |    Microsoft tarafından sağlanan                            |    Microsoft tarafından sağlanan                    |    Müşteri sağlandı                   |
|    **Ağ arabirimi**            |    USB 3.1/SATA                 |    RJ 45, SFP +                                   |    RJ45, QSFP +                           |    SATA ıı/SATA ııı                    |
|    **İş ortağı tümleştirmesi**          |    Bazen                         |    [Yüksek](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureExpressPod)                                          |    [Yüksek](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureExpressPod)                                  |    Bazen                                |
|    **Gönderim**                     |    Microsoft tarafından yönetilen            |    Microsoft tarafından yönetilen                             |    Microsoft tarafından yönetilen                     |    Müşteri tarafından yönetilen                    |
| **Veri taşırken kullanın**     |Bir ticaret sınırı içinde|Bir ticaret sınırı içinde|Bir ticaret sınırı içinde|Coğrafi sınırların tamamında, örneğin ABD-AB|
|    **Fiyatlandırma**                      |    [Fiyatlandırma](https://azure.microsoft.com/pricing/details/databox/disk/)                    |   [Fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/databox/)                                      |  [Fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/databox/heavy/)                               |   [Fiyatlandırma](https://azure.microsoft.com/pricing/details/storage-import-export/)                            |


## <a name="next-steps"></a>Sonraki adımlar

- Nasıl yapılacağını öğrenin

    - [Data Box disk verileri aktarın](../../databox/data-box-disk-quickstart-portal.md).
    - [Data Box verileri aktarın](../../databox/data-box-quickstart-portal.md).
    - [İçeri/dışarı aktarma ile veri aktarma](../../import-export/storage-import-export-data-to-blobs.md).