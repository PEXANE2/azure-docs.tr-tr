---
title: Düzenli veri aktarımı için bir Azure çözümü seçin | Microsoft Docs
description: Verileri düzenli aralıklarla aktarırken veri aktarımı için bir Azure çözümü seçme hakkında bilgi edinin.
services: storage
author: alkohli
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: alkohli
ms.openlocfilehash: 9ffa35e158d34a1fc6945ee2730dcf136d13edb5
ms.sourcegitcommit: 374e47efb65f0ae510ad6c24a82e8abb5b57029e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2020
ms.locfileid: "85504347"
---
# <a name="solutions-for-periodic-data-transfer"></a>Düzenli aralıklarla veri aktarımına yönelik çözümler
 
Bu makalede, verileri düzenli aralıklarla aktarırken veri aktarımı çözümlerine ilişkin bir genel bakış sunulmaktadır. Ağ üzerinden düzenli veri aktarımı, düzenli aralıklarla veya sürekli veri hareketine göre yinelenebilir olarak kategorilere ayrılır. Makalede ayrıca önerilen veri aktarımı seçenekleri ve bu senaryonun ilgili anahtar özelliği matrisi de açıklanmaktadır.

Tüm kullanılabilir veri aktarımı seçeneklerine genel bir bakış için bkz. [Azure veri aktarımı çözümü seçme](storage-choose-data-transfer-solution.md).

## <a name="recommended-options"></a>Önerilen Seçenekler

Düzenli veri aktarımı için önerilen seçenekler, aktarımın yinelenen veya sürekli olmasına bağlı olarak iki kategoriye ayrılır.

- **Komut dosyalı/programlı araçlar** – düzenli aralıklarla oluşan veri aktarımı Için AzCopy ve Azure Storage REST API 'leri gibi betikleştirilmiş ve programlı araçları kullanın. Bu araçlar BT uzmanlarına ve geliştiricilere yöneliktir.

    - **AzCopy** -bu komut satırı aracını, En Iyi performansla Azure Blob 'Larına, dosyalarına ve tablo depolamasına verileri kolayca kopyalamak için kullanın. AzCopy eşzamanlılık ve paralellik destekler ve kesintiye uğradığında kopyalama işlemlerini sürdürülemez.
    - **Azure depolama REST API 'leri/SDK 'lar** – bir uygulama oluştururken, uygulamayı Azure Storage REST API 'lerinde geliştirebilir ve birden çok dilde sunulan Azure SDK 'larını kullanabilirsiniz. REST API 'Leri, verilerin Azure 'a ve Azure 'a yüksek performanslı olarak kopyalanması için özellikle tasarlanan Azure Storage veri taşıma kitaplığından da yararlanabilir.

- **Sürekli veri alma araçları** : sürekli ve devam eden veri alımı için Data Box çevrimiçi aktarım cihazından birini veya Azure Data Factory seçebilirsiniz. Bu araçlar BT uzmanları tarafından ayarlanır ve veri aktarımını saydam bir şekilde otomatikleştirebilir.

    - **Azure Data Factory** – Data Factory bir aktarım işleminin ölçeğini genişletmek için ve düzenleme ve kurumsal sınıf izleme özelliklerine ihtiyaç duyuyorsanız kullanılmalıdır. Çeşitli Azure Hizmetleri, şirket içi veya ikisinin bir birleşimi arasında düzenli olarak dosya aktaran bir bulut işlem hattı ayarlamak için Azure Data Factory kullanın. Azure Data Factory, farklı veri mağazalarından veri alan ve veri taşıma ve veri dönüştürme işlemlerini otomatik hale getirmeye yönelik veri odaklı iş akışlarını düzenlemenizi sağlar.
    - **Çevrimiçi aktarımlar için Azure Data Box ailesi** -Data Box Edge ve Data Box Gateway, verileri Azure 'a ve dışına taşıyabileceği çevrimiçi ağ cihazlarıdır. Data Box Edge, karşıya yüklemeden önce verileri önceden işlemek için yapay zeka (AI) özellikli uç işlem kullanır. Data Box Gateway, aynı veri aktarımı özelliklerine sahip bir cihazın sanal sürümüdür.


## <a name="comparison-of-key-capabilities"></a>Anahtar özellikleri karşılaştırması

Aşağıdaki tabloda, önemli özelliklerde farklılıklar özetlenmektedir.

### <a name="scriptedprogrammatic-network-data-transfer"></a>Betikleştirilmiş/programlı ağ veri aktarımı

| Özellik                  | AzCopy                                 | Azure depolama REST API 'Leri       |
|-----------------------------|----------------------------------------|-------------------------------|
| Form faktörü                 | Microsoft 'tan komut satırı aracı       | Müşterilere depolamaya yönelik geliştirme <br> Azure istemci kitaplıklarını kullanarak REST API 'Leri |
| İlk bir kerelik kurulum     | En az                                | Orta, değişken geliştirme çabaları    |
| Veri biçimi                 | Azure Blobları, Azure dosyaları, Azure tabloları | Azure Blobları, Azure dosyaları, Azure tabloları   |
| Performans                 | Zaten iyileştirilmiş                      | Geliştirme sırasında iyileştirin                  |
| Fiyatlandırma                     | Ücretsiz, veri çıkış ücretleri geçerlidir      | Ücretsiz, veri çıkış ücretleri geçerlidir        |

### <a name="continuous-data-ingestion-over-network"></a>Ağ üzerinden sürekli veri alımı

| Özellik                                       | Data Box Gateway | Data Box Edge   | Azure Data Factory        |
|----------------------------------|-----------------------------------------|--------------------------|---------------------------|
| Form faktörü                                   | Sanal cihaz             | Fiziksel cihaz          | Azure portal hizmeti, şirket içi aracıda                                                            |
| Donanım                                      | Hiper yöneticinizi            | Microsoft tarafından sağlandı    | NA                                                            |
| İlk kurulum çabası                          | Düşük (<30 dakika)            | Orta (~ birkaç saat) | Büyük (~ gün)                                                 |
| Veri biçimi                                   | Azure Blobları, Azure dosyaları   | Azure Blobları, Azure dosyaları | [Veri depoları ve biçimleri için 70 ' ten fazla veri Bağlayıcısı destekler](https://docs.microsoft.com/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats)|
| Verileri önceden işleme                           | No                         | Evet, uç işlem ile    | Yes                                                           |
| Yerel önbellek<br>(Şirket içi verileri depolamak için)    | Yes                        | Yes                      | No                                                            |
| Diğer bulutlardan aktar                    | Hayır                         | Hayır                       | Evet                                                           |
| Fiyatlandırma                                       | [Fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/databox/gateway/)                    | [Fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/databox/edge/)                  | [Fiyatlandırma](https://azure.microsoft.com/pricing/details/data-factory/)                                                       |

## <a name="next-steps"></a>Sonraki adımlar

- [AzCopy ile veri aktarma](/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json).
- [Depolama REST API 'leri ile veri aktarımı hakkında daha fazla bilgi](https://docs.microsoft.com/dotnet/api/overview/azure/storage?view=azure-dotnet).
- Nasıl yapılacağını anlayın:
    - [Data Box Gateway verileri aktarın](https://docs.microsoft.com/azure/databox-online/data-box-gateway-deploy-add-shares).
    - [Azure 'a göndermeden önce Data Box Edge verileri dönüştürün](https://docs.microsoft.com/azure/databox-online/data-box-edge-deploy-configure-compute).
- [Azure Data Factory ile veri aktarmayı öğrenin](https://docs.microsoft.com/azure/data-factory/tutorial-bulk-copy-portal).
