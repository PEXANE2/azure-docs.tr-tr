---
title: Periyodik veri aktarımı için bir Azure çözümü seçin| Microsoft Dokümanlar
description: Verileri düzenli aralıklarla aktarırken veri aktarımı için bir Azure çözümlerini nasıl seçeceğinizi öğrenin.
services: storage
author: alkohli
ms.service: storage
ms.subservice: blobs
ms.topic: article
ms.date: 06/24/2019
ms.author: alkohli
ms.openlocfilehash: fb49802adf6242f445b700d06622d7e6aa336b4d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67357030"
---
# <a name="solutions-for-periodic-data-transfer"></a>Düzenli aralıklarla veri aktarımına yönelik çözümler
 
Bu makalede, verileri periyodik olarak aktarırken veri aktarım çözümlerine genel bir bakış sağlanmaktadır. Ağ üzerinden periyodik veri aktarımı düzenli aralıklarla veya sürekli veri hareketi yinelenen olarak kategorize edilebilir. Makalede ayrıca önerilen veri aktarım seçenekleri ve bu senaryo için ilgili anahtar yetenek matrisi açıklanmaktadır.

Kullanılabilir tüm veri aktarım seçeneklerine genel bir bakış anlamak için [Azure veri aktarım çözümünün seçin'e](storage-choose-data-transfer-solution.md)gidin.

## <a name="recommended-options"></a>Önerilen seçenekler

Periyodik veri aktarımı için önerilen seçenekler, aktarımın yinelenen veya sürekli olup olmadığına bağlı olarak iki kategoriye ayrılır.

- **Komut dosyası/programlı araçlar** – Düzenli aralıklarla gerçekleşen veri aktarımı için AzCopy ve Azure Depolama REST API'leri gibi komut dosyası ve programlı araçları kullanın. Bu araçlar BT uzmanlarına ve geliştiricilere yöneliktir.

    - **AzCopy** - Azure Blobs, Files ve Table depolama alanına en iyi performansla verileri kolayca kopyalamak için bu komut satırı aracını kullanın. AzCopy eşzamanlılık ve paralellik ve kesildiğinde kopyalama işlemlerine devam etme yeteneğini destekler.
    - **Azure Depolama REST API'leri/SDK'ları** – Bir uygulama geliştirirken, uygulamayı Azure Depolama REST API'lerine karşı geliştirebilir ve birden çok dilde sunulan Azure SDK'larını kullanabilirsiniz. REST API'leri, özellikle Azure'a ve Azure'dan gelen verilerin yüksek performanslı kopyalanması için tasarlanmış Azure Depolama Veri Hareketi Kitaplığı'ndan da yararlanabilir.

- **Sürekli veri alma araçları** – Sürekli ve sürekli veri alımı için Veri Kutusu çevrimiçi aktarım aygıtından birini veya Azure Veri Fabrikası'nı seçebilirsiniz. Bu araçlar BT uzmanları tarafından ayarlanır ve veri aktarımında saydam olarak otomatikleştirebilir.

    - **Azure Veri Fabrikası** – Veri Fabrikası, bir aktarım işlemini ölçeklendirmek ve düzenleme ve kurumsal sınıf izleme özelliklerine ihtiyaç duyulduğunda kullanılmalıdır. Dosyaları birkaç Azure hizmeti, şirket içi veya ikisinin birleşimi arasında düzenli olarak aktaran bir bulut ardışık hattı oluşturmak için Azure Veri Fabrikası'nı kullanın. Azure Veri Fabrikası, birbirinden farklı veri depolarından veri alan ve veri hareketi ve veri dönüşümlerini otomatikleştiren veri tabanlı iş akışlarını düzenlemenize olanak tanır.
    - **Çevrimiçi aktarımlar için Azure Veri Kutusu ailesi** - Veri Kutusu Kenarı ve Veri Kutusu Ağ Geçidi, verileri Azure'a girip çıkarabilen çevrimiçi ağ aygıtlarıdır. Data Box Edge, yüklemeden önce verileri önceden işlemek için yapay zeka (AI) etkin Edge bilgi işlemini kullanır. Veri Kutusu Ağ Geçidi, aynı veri aktarım özelliklerine sahip aygıtın sanal bir sürümüdür.


## <a name="comparison-of-key-capabilities"></a>Anahtar özelliklerin karşılaştırılması

Aşağıdaki tabloda anahtar yetenekleri farklılıkları özetlenebilir.

### <a name="scriptedprogrammatic-network-data-transfer"></a>Komut dosyası/Programlı ağ veri aktarımı

| Özellik                  | AzCopy                                 | Azure Depolama REST API'leri       |
|-----------------------------|----------------------------------------|-------------------------------|
| Form faktörü                 | Microsoft'tan komut satırı aracı       | Müşteriler Depolama'ya karşı gelişir <br> Azure istemci kitaplıklarını kullanarak REST API'leri |
| İlk tek seferlik kurulum     | En az                                | Orta, değişken geliştirme çabası    |
| Veri Biçimi                 | Azure Blobs, Azure Dosyaları, Azure Tablolar | Azure Blobs, Azure Dosyaları, Azure Tablolar   |
| Performans                 | Zaten optimize edilmiş                      | Geliştikçe optimize edin                  |
| Fiyatlandırma                     | Ücretsiz, veri çıkış ücretleri geçerlidir      | Ücretsiz, veri çıkış ücretleri geçerlidir        |

### <a name="continuous-data-ingestion-over-network"></a>Ağ üzerinden sürekli veri alımı

| Özellik                                       | Data Box Gateway | Data Box Edge   | Azure Data Factory        |
|----------------------------------|-----------------------------------------|--------------------------|---------------------------|
| Form faktörü                                   | Sanal cihaz             | Fiziksel cihaz          | Azure portalında hizmet, şirket içi temsilci                                                            |
| Donanım                                      | Hipervizörünüz            | Microsoft tarafından sağlanan    | NA                                                            |
| İlk kurulum çabası                          | Düşük (<30 dakika.)            | Orta (~birkaç saat) | Büyük (~gün)                                                 |
| Veri Biçimi                                   | Azure Blobs, Azure Dosyaları   | Azure Blobs, Azure Dosyaları | [Veri depoları ve biçimleri için 70'den fazla veri bağlayıcısı destekler](https://docs.microsoft.com/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats)|
| Veri ön işleme                           | Hayır                         | Evet, Edge compute üzerinden    | Evet                                                           |
| Yerel önbellek<br>(şirket içi verileri depolamak için)    | Evet                        | Evet                      | Hayır                                                            |
| Diğer bulutlardan aktarma                    | Hayır                         | Hayır                       | Evet                                                           |
| Fiyatlandırma                                       | [Fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/databox/gateway/)                    | [Fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/databox/edge/)                  | [Fiyatlandırma](https://azure.microsoft.com/pricing/details/data-factory/)                                                       |

## <a name="next-steps"></a>Sonraki adımlar

- [AzCopy ile veri aktarımı.](/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json)
- [Depolama REST API'leri ile veri aktarımı hakkında daha fazla bilgi.](https://docs.microsoft.com/dotnet/api/overview/azure/storage?view=azure-dotnet)
- Nasıl yapılacağını anlayın:
    - [Veri Kutusu Ağ Geçidi ile veri aktarımı.](https://docs.microsoft.com/azure/databox-online/data-box-gateway-deploy-add-shares)
    - [Azure'a göndermeden önce Verileri Veri Kutusu Kenarı ile dönüştürün.](https://docs.microsoft.com/azure/databox-online/data-box-edge-deploy-configure-compute)
- [Azure Veri Fabrikası ile nasıl veri aktarılamayı öğrenin.](https://docs.microsoft.com/azure/data-factory/tutorial-bulk-copy-portal)
