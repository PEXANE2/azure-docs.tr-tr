---
title: Büyük veri kümeleri için Azure veri aktarımı seçenekleri, orta ila yüksek ağ bant genişliği | Microsoft Docs
description: Ortamınızda yüksek ağ bant genişliğine sahip olduğunuzda ve büyük veri kümelerini aktarmayı planladığınızda veri aktarımı için bir Azure çözümü seçme hakkında bilgi edinin.
services: storage
author: alkohli
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: alkohli
ms.openlocfilehash: cf0e423648db174433f0717f2e5971ac49697b42
ms.sourcegitcommit: 75041f1bce98b1d20cd93945a7b3bd875e6999d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98704632"
---
# <a name="data-transfer-for-large-datasets-with-moderate-to-high-network-bandwidth"></a>Bant genişliği orta veya yüksek olduğunda büyük veri kümeleri için veri aktarımı
 
Bu makalede, ortamınızda yüksek ağ bant genişliğine sahip olduğunuzda ve büyük veri kümelerini aktarmayı planlarken veri aktarımı çözümlerine genel bir bakış sağlanır. Makalede ayrıca önerilen veri aktarımı seçenekleri ve bu senaryonun ilgili anahtar özelliği matrisi de açıklanmaktadır.

Tüm kullanılabilir veri aktarımı seçeneklerine genel bir bakış için bkz. [Azure veri aktarımı çözümü seçme](storage-choose-data-transfer-solution.md).

## <a name="scenario-description"></a>Senaryo açıklaması

Büyük veri kümeleri, TBs 'nin PBs sırasındaki veri boyutlarına başvurur. Orta ila yüksek ağ bant genişliği 100 Mbps ila 10 Gbps anlamına gelir.

## <a name="recommended-options"></a>Önerilen seçenekler

Bu senaryoda ağ bant genişliğinizin orta veya yüksek olması durumuna göre farklı seçenekler önerilmiştir.

### <a name="moderate-network-bandwidth-100-mbps---1-gbps"></a>Orta ağ bant genişliği (100 Mb/sn-1 Gb/sn)

Orta düzeyde ağ bant genişliği sayesinde, ağ üzerinden veri aktarımı süresini proje yapmanız gerekir.

Saati tahmin etmek ve bunu temel alarak, çevrimdışı bir aktarım veya ağ aktarımı arasında seçim yapmak için aşağıdaki tabloyu kullanın. Tabloda, çeşitli kullanılabilir ağ bant genişlikleri (%90 kullanım varsayılıyor) için ağ veri aktarımı için öngörülen süre gösterilmektedir.  

![Ağ aktarımı veya çevrimdışı aktarım](media/storage-solution-large-dataset-low-network/storage-network-or-offline-transfer.png)

- Ağ aktarımının çok yavaş olması için yansıtıldıysanız fiziksel bir cihaz kullanmanız gerekir. Bu durumda önerilen seçenekler, Azure Data Box ailesi veya Azure Içeri/dışarı aktarma 'dan kendi disklerinizi kullanarak çevrimdışı aktarma cihazlarıdır.

    - **Azure Data Box ailesi çevrimdışı aktarımlar için** : Microsoft tarafından sağlanan Data Box cihazlarındaki cihazları, zaman, ağ kullanılabilirliği veya maliyetlerle sınırlı olduğunuzda Azure 'a büyük miktarlarda veri taşımak için kullanın. Robocopy gibi araçlarla şirket içindeki verileri kopyalayın. Aktarmak istediğiniz verilerin boyutuna göre Data Box Disk, Data Box veya Data Box Heavy çözümlerinden birini seçebilirsiniz.
    - **Azure içeri/dışarı aktarma** – büyük miktarlarda verileri Azure Blob depolama ve Azure dosyaları 'na güvenli bir şekilde aktarmak için kendi disk sürücülerinizi göndererek Azure Içeri/dışarı aktarma hizmetini kullanın. Bu hizmeti ayrıca Azure Blob depolama alanınızdaki verileri disk sürücülerine aktarıp şirket içi ortamınıza göndermek için de kullanabilirsiniz.

- Ağ aktarımının makul olması için tasarlanan her türlü, [yüksek ağ bant genişliğine](#high-network-bandwidth)göre ayrıntılı olan aşağıdaki araçlardan herhangi birini kullanabilirsiniz.


### <a name="high-network-bandwidth-1-gbps---100-gbps"></a>Yüksek ağ bant genişliği (1 Gb/sn-100 Gb/sn)

Kullanılabilir ağ bant genişliği yüksekse, aşağıdaki araçlardan birini kullanın.

- **AzCopy** -bu komut satırı aracını, En Iyi performansla Azure Blob 'Larına, dosyalarına ve tablo depolamasına verileri kolayca kopyalamak için kullanın. AzCopy eşzamanlılık ve paralellik desteğine ek olarak kesintiye uğrayan kopyalama işlemlerini sürdürme olanağı sunar.
- **Azure depolama REST API 'leri/SDK 'lar** – bir uygulama oluştururken, uygulamayı Azure Storage REST API 'lerinde geliştirebilir ve birden çok dilde sunulan Azure SDK 'larını kullanabilirsiniz.
- **Çevrimiçi aktarımlar için Azure Data Box ailesi** – Data Box Edge ve Data Box Gateway, verileri Azure 'a ve dışına taşıyabileceği çevrimiçi ağ cihazlarıdır. Sürekli alım ve verilerin karşıya yüklenmeden önce işlenmesi gereksinimlerinin aynı anda mevcut olması halinde Data Box Edge fiziksel cihazını kullanın. Data Box Gateway, cihazın sanal bir sürümüdür ancak aynı veri aktarımı yeteneklerine sahiptir. İki durumda da veri aktarımı cihaz tarafından yönetilir.
- **Azure Data Factory** – Data Factory bir aktarım işleminin ölçeğini genişletmek için ve düzenleme ve kurumsal sınıf izleme özelliklerine ihtiyaç duyuyorsanız kullanılmalıdır. Data Factory'yi kullanarak birden çok Azure hizmeti, şirket içi ortam veya ikisinin birleşiminin bulunduğu bir ortamda düzenli dosya aktarımı gerçekleştirebilirsiniz. Data Factory ile farklı veri depolarından veri alan veri odaklı iş akışları (işlem hattı olarak adlandırılır) oluşturabilir ve zamanlayabilir, ayrıca veri taşıma ile veri dönüştürme süreçlerini otomatikleştirebilirsiniz.

## <a name="comparison-of-key-capabilities"></a>Anahtar özellikleri karşılaştırması

Aşağıdaki tablolarda önerilen seçenekler için önemli olanaklarla ilgili farklılıklar özetlenmektedir.

### <a name="moderate-network-bandwidth"></a>Orta düzeyde ağ bant genişliği

Çevrimdışı veri aktarımı kullanıyorsanız, önemli özelliklerde farkları anlamak için aşağıdaki tabloyu kullanın.

|                                     |    Data Box Disk      |    Data Box                                      |    Data Box Heavy            |    İçeri/Dışarı Aktarma                       |
|-------------------------------------|---------------------------------|--------------------------------------------------|------------------------------------------|----------------------------------------|
|    **Veri boyutu**                    |    35 TBs 'ye kadar                 |    Cihaz başına 80 TBs 'ye kadar                       |    Cihaz başına 800 TB 'a kadar               |    Değişken                            |
|    **Veri türü**                    |    Azure Blobları                  |    Azure Blobları<br>Azure Dosyaları                    |    Azure Blobları<br>Azure Dosyaları            |    Azure Blobları<br>Azure Dosyaları          |
|    **Form faktörü**                  |    sipariş başına 5 SSD             |    1 X 50-lbs. sipariş başına masaüstü boyutunda cihaz    |    1 X ~ 500-lbs. sipariş başına büyük cihaz    |    Sipariş başına en fazla 10 HDD/SSD        |
|    **İlk kurulum saati**               |    Düşük <br>(15 dakika)            |    Düşük-orta <br> (<30 dakika)               |    Orta<br>(1-2 saat)               |    Orta-zor<br>değişken |
|    **Verileri Azure 'a gönderme**           |    Yes                          |    Yes                                           |    Yes                                   |    Yes                                 |
|    **Verileri Azure'dan dışarı aktarma**           |    Hayır                           |    Hayır                                            |    Hayır                                    |    Yes                                 |
|    **Şifreleme**                   |    AES 128 bit                  |    AES 256 bit                                   |    AES 256 bit                           |    AES 128 bit                         |
|    **Donanım**                     |     Microsoft tarafından sağlanan          |    Microsoft tarafından sağlanan                            |    Microsoft tarafından sağlanan                    |    Müşteri sağlandı                   |
|    **Ağ arabirimi**            |    USB 3.1/SATA                 |    RJ 45, SFP +                                   |    RJ45, QSFP +                           |    SATA ıı/SATA ııı                    |
|    **İş ortağı tümleştirmesi**          |    Bazen                         |    [Yüksek](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureExpressPod)                                          |    [Yüksek](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureExpressPod)                                  |    Bazen                                |
|    **Gönderim**                     |    Microsoft tarafından yönetilen            |    Microsoft tarafından yönetilen                             |    Microsoft tarafından yönetilen                     |    Müşteri tarafından yönetilen                    |
| **Veri taşırken kullanın**     |Bir ticaret sınırı içinde|Bir ticaret sınırı içinde|Bir ticaret sınırı içinde|Coğrafi sınırların tamamında, örneğin ABD-AB|
|    **Fiyatlandırma**                          |    [Fiyatlandırma](https://azure.microsoft.com/pricing/details/databox/disk/)                    |   [Fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/databox/)                                      |  [Fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/databox/heavy/)                               |   [Fiyatlandırma](https://azure.microsoft.com/pricing/details/storage-import-export/)                            |


Çevrimiçi veri aktarımı kullanıyorsanız, yüksek ağ bant genişliği için aşağıdaki bölümdeki tabloyu kullanın.

### <a name="high-network-bandwidth"></a>Yüksek ağ bant genişliği

|                                     |    Araçlar AzCopy, <br>Azure PowerShell <br>Azure CLI             |    Azure depolama REST API 'Leri, SDK 'Lar                   |    Data Box Gateway veya Data Box Edge          |    Azure Data Factory                                            |
|-------------------------------------|------------------------------------|----------------------------------------------|----------------------------------|-----------------------------------------------------------------------|
|    **Veri türü**              |    Azure Blobları, Azure dosyaları, Azure tabloları    |    Azure Blobları, Azure dosyaları, Azure tabloları    |    Azure Blobları, Azure dosyaları                           |   Veri depoları ve biçimleri için 70 ' ten fazla veri Bağlayıcısı destekler    |
|    **Form faktörü**            |    Komut satırı araçları                        |    Programlı arabirim                    |    Microsoft bir sanal <br>veya fiziksel cihaz     |    Azure portal hizmet                                            |
|    **İlk bir kerelik kurulum** |    Kolaylık               |    Orta                       |    Kolay (<30 dakika)-Orta (1-2 saat)            |    Yaygın                                                          |
|    **Verileri önceden işleme**          |    Hayır                                        |    Hayır                                        |    Evet (Edge işlem Ile)                               |    Yes                                                                |
|    **Diğer bulutlardan aktar**   |    Hayır                                        |    Hayır                                        |    Hayır                                                    |    Yes                                                                |
|    **Kullanıcı türü**                    |    BT uzmanı veya geliştirme                                       |    Geliştirme                                       |    BT Profesyoneli                                                |    BT Profesyoneli                                                             |
|    **Fiyatlandırma**                      |    Ücretsiz, veri çıkış ücretleri geçerlidir         |    Ücretsiz, veri çıkış ücretleri geçerlidir         |    [Fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/databox/edge/)                                               |    [Fiyatlandırma](https://azure.microsoft.com/pricing/details/data-factory/)                                                            |

## <a name="next-steps"></a>Sonraki adımlar

- [İçeri/dışarı aktarma ile veri aktarmayı öğrenin](../../import-export/storage-import-export-data-to-blobs.md).
- Nasıl yapılacağını öğrenin

    - [Data Box disk verileri aktarın](../../databox/data-box-disk-quickstart-portal.md).
    - [Data Box verileri aktarın](../../databox/data-box-quickstart-portal.md).
- [AzCopy ile veri aktarma](./storage-use-azcopy-v10.md).
- Nasıl yapılacağını anlayın:
    - [Data Box Gateway verileri aktarın](../../databox-gateway/data-box-gateway-deploy-add-shares.md).
    - [Azure 'a göndermeden önce Data Box Edge verileri dönüştürün](../../databox-online/azure-stack-edge-deploy-configure-compute.md).
- [Azure Data Factory ile veri aktarmayı öğrenin](../../data-factory/quickstart-create-data-factory-portal.md).
- Veri aktarmak için REST API 'Lerini kullanma

    - [.NET 'te](/dotnet/api/overview/azure/storage)
    - [Java üzerinde](/java/api/overview/azure/storage)