---
title: Ağ bant genişliği düşük veya hiç olmayan büyük veri kümeleri için Azure veri aktarım seçenekleri| Microsoft Dokümanlar
description: Ortamınızda ağ bant genişliği olmadan sınırlı yken ve büyük veri kümeleri aktarmayı planlıyorsanız, veri aktarımı için bir Azure çözümünüzü nasıl seçeceğinizi öğrenin.
services: storage
author: alkohli
ms.service: storage
ms.subservice: blobs
ms.topic: article
ms.date: 04/01/2019
ms.author: alkohli
ms.openlocfilehash: 4c4ac9489b9613b2eeaf26a3df9f4cbc664a1026
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60730699"
---
# <a name="data-transfer-for-large-datasets-with-low-or-no-network-bandwidth"></a>Bant genişliği düşük olduğunda veya hiç olmadığında büyük veri kümeleri için veri aktarımı
 
Bu makalede, ortamınızda ağ bant genişliği ile sınırlı olduğunda ve büyük veri kümelerini aktarmayı planlıyorsanız, veri aktarım çözümlerine genel bir bakış sağlar. Makalede ayrıca önerilen veri aktarım seçenekleri ve bu senaryo için ilgili anahtar yetenek matrisi açıklanmaktadır.

Kullanılabilir tüm veri aktarım seçeneklerine genel bir bakış anlamak için [Azure veri aktarım çözümünün seçin'e](storage-choose-data-transfer-solution.md)gidin.

## <a name="offline-transfer-or-network-transfer"></a>Çevrimdışı aktarım veya ağ aktarımı

Büyük veri kümeleri, az miktarda Kisi ile birkaç PB veriniz olduğunu ima eder. Ağ bant genişliği yle sınırlı değil, ağınız yavaş veya güvenilmez. Ayrıca:

- Internet Servis Sağlayıcılarınızdan (ISS) ağ aktarım maliyetleri ile sınırlıdırsınız.
- Güvenlik veya kuruluş ilkeleri, hassas verilerle uğraşırken giden bağlantılara izin vermez.

Yukarıdaki tüm durumlarda, bir kerelik toplu veri aktarımı yapmak için fiziksel bir aygıt kullanın. Microsoft tarafından sağlanan Veri Kutusu Diski, Veri Kutusu, Veri Kutusu Ağır aygıtları veya kendi disklerinizi kullanarak Dışa Aktar/Dışa Aktar'dan seçim yapın.

Fiziksel aygıtın doğru seçenek olup olmadığını doğrulamak için aşağıdaki tabloyu kullanın. Çeşitli kullanılabilir bant genişlikleri için (%90 kullanım dan sonra) ağ veri aktarımı için öngörülen zamanı gösterir. Ağ aktarımın çok yavaş olması öngörülüyorsa, fiziksel bir aygıt kullanmanız gerekir.  

![Ağ aktarım veya çevrimdışı aktarım](media/storage-solution-large-dataset-low-network/storage-network-or-offline-transfer.png)

## <a name="recommended-options"></a>Önerilen seçenekler

Bu senaryoda kullanılabilen seçenekler, Azure Veri Kutusu çevrimdışı aktarım veya Azure İçe Aktarma/Verme aygıtlarıdır.

- **Çevrimdışı aktarımlar için Azure Veri Kutusu ailesi** – Zaman, ağ kullanılabilirliği veya maliyetlerle sınırlı olduğunuzda büyük miktarda veriyi Azure'a taşımak için Microsoft tarafından sağlanan Veri Kutusu aygıtlarından aygıtlar kullanın. Robocopy gibi araçları kullanarak şirket içi verileri kopyalayın. Aktarım için amaçlanan veri boyutuna bağlı olarak, Veri Kutusu Diski, Veri Kutusu veya Veri Kutusu Ağır'dan seçim yapabilirsiniz.
- **Azure İçe Aktar/Dışa Aktarma** – Azure Blob depolama ve Azure Dosyalarına büyük miktarda veri almak için kendi disk sürücülerinizi göndererek Azure İçe Alma/Dışa Aktarma hizmetini kullanın. Bu hizmet, Azure Blob depolama dan disk sürücülerine veri aktarmak ve şirket içi sitelerinize aktarmak için de kullanılabilir.

## <a name="comparison-of-key-capabilities"></a>Anahtar özelliklerin karşılaştırılması

Aşağıdaki tabloda anahtar yetenekleri farklılıkları özetlenebilir.

|                                     |    Data Box Disk      |    Data Box                                      |    Data Box Heavy              |    İçeri/Dışarı Aktarma                       |
|-------------------------------------|---------------------------------|--------------------------------------------------|------------------------------------------|----------------------------------------|
|    Veri boyutu                        |    35 TB'a kadar                 |    Cihaz başına 80 TB'a kadar                       |    Cihaz başına 800 TB'a kadar               |    Değişken                            |
|    Veri türü                        |    Azure Blobları                  |    Azure Blobları<br>Azure Dosyaları                    |    Azure Blobları<br>Azure Dosyaları            |    Azure Blobları<br>Azure Dosyaları          |
|    Form faktörü                      |    Sipariş başına 5 SSD             |    1 x 50-lbs. sipariş başına masaüstü boyutlu aygıt    |    1 X ~500-lbs. sipariş başına büyük cihaz    |    Sipariş başına en fazla 10 HDD/SSD        |
|    İlk kurulum süresi               |    Düşük <br>(15 dakika)            |    Düşük ve orta <br> (<30 dakika)               |    Orta<br>(1-2 saat)               |    Orta ve zor<br>(değişken) |
|    Azure'a veri gönderme               |    Evet                          |    Evet                                           |    Evet                                   |    Evet                                 |
|    Verileri Azure’dan dışarı aktarma           |    Hayır                           |    Hayır                                            |    Hayır                                    |    Evet                                 |
|    Şifreleme                       |    AES 128-bit                  |    AES 256 bit                                   |    AES 256 bit                           |    AES 128-bit                         |
|    Donanım                         |     Microsoft sağlanan          |    Microsoft sağlanan                            |    Microsoft sağlanan                    |    Müşteri sağlanan                   |
|    Ağ arabirimi                |    USB 3.1/SATA                 |    RJ 45, SFP+                                   |    RJ45, QSFP+                           |    SATA II/SATA III                    |
|    İş ortağı tümleştirmesi              |    Bazıları                         |    [Yüksek](https://azuremarketplace.microsoft.com/campaigns/databox/azure-data-box)                                          |    [Yüksek](https://azuremarketplace.microsoft.com/campaigns/databox/azure-data-box)                                  |    Bazıları                                |
|    Sevkiyat                         |    Microsoft yönetilen            |    Microsoft yönetilen                             |    Microsoft yönetilen                     |    Müşteri yönetimi                    |
| Veri taşınırken kullanma         |Bir ticaret sınırı içinde|Bir ticaret sınırı içinde|Bir ticaret sınırı içinde|Coğrafi sınırların ötesinde, örneğin ABD'den AB'ye|
|    Fiyatlandırma                          |    [Fiyatlandırma](https://azure.microsoft.com/pricing/details/databox/disk/)                    |   [Fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/databox/)                                      |  [Fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/databox/heavy/)                               |   [Fiyatlandırma](https://azure.microsoft.com/pricing/details/storage-import-export/)                            |


## <a name="next-steps"></a>Sonraki adımlar

- Nasıl yapılacağını anlama

    - [Veri Kutusu Diski ile veri aktarımı.](https://docs.microsoft.com/azure/databox/data-box-disk-quickstart-portal)
    - [Veri Kutusu ile veri aktarımı.](https://docs.microsoft.com/azure/databox/data-box-quickstart-portal)
    - [Veri Aktar/Dışa Aktar.](/azure/storage/common/storage-import-export-data-to-blobs)
