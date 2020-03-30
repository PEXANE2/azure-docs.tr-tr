---
title: Büyük veri kümeleri için azure veri aktarım seçenekleri, orta ve yüksek ağ bant genişliği| Microsoft Dokümanlar
description: Ortamınızda orta ve yüksek ağ bant genişliğine sahipseniz ve büyük veri kümeleri aktarmayı planlıyorsanız, veri aktarımı için nasıl bir Azure çözümü seçeceğinizi öğrenin.
services: storage
author: alkohli
ms.service: storage
ms.subservice: blobs
ms.topic: article
ms.date: 04/01/2019
ms.author: alkohli
ms.openlocfilehash: f7177a95bdd585ff2822c9ac8c94a85d12f9259b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69900358"
---
# <a name="data-transfer-for-large-datasets-with-moderate-to-high-network-bandwidth"></a>Bant genişliği orta veya yüksek olduğunda büyük veri kümeleri için veri aktarımı
 
Bu makalede, ortamınızda orta ve yüksek ağ bant genişliği varsa ve büyük veri kümeleri aktarmayı planlıyorsanız veri aktarım çözümlerine genel bir bakış sağlar. Makalede ayrıca önerilen veri aktarım seçenekleri ve bu senaryo için ilgili anahtar yetenek matrisi açıklanmaktadır.

Kullanılabilir tüm veri aktarım seçeneklerine genel bir bakış anlamak için [Azure veri aktarım çözümünün seçin'e](storage-choose-data-transfer-solution.md)gidin.

## <a name="scenario-description"></a>Senaryo açıklaması

Büyük veri kümeleri, TB'ler sırasına göre pb'lere veri boyutlarına başvurur. Orta ve yüksek ağ bant genişliği 100 Mbps ile 10 Gbps arasında dır.

## <a name="recommended-options"></a>Önerilen seçenekler

Bu senaryoda önerilen seçenekler, orta ağ bant genişliğine veya yüksek ağ bant genişliğine sahip olup olmadığınıza bağlıdır.

### <a name="moderate-network-bandwidth-100-mbps---1-gbps"></a>Orta ağ bant genişliği (100 Mbps - 1 Gbps)

Orta ağ bant genişliği yle, ağ üzerinden veri aktarım süresini yansıtmanız gerekir.

Saati tahmin etmek için aşağıdaki tabloyu kullanın ve buna göre, çevrimdışı aktarım arasında veya ağ aktarımı üzerinden seçim yapın. Tablo, çeşitli kullanılabilir ağ bant genişlikleri için (%90 kullanım varsayarak) ağ veri aktarımı için öngörülen zamanı gösterir.  

![Ağ aktarım veya çevrimdışı aktarım](media/storage-solution-large-dataset-low-network/storage-network-or-offline-transfer.png)

- Ağ aktarımın çok yavaş olması öngörülüyorsa, fiziksel bir aygıt kullanmanız gerekir. Bu durumda önerilen seçenekler, Azure Veri Kutusu ailesinden çevrimdışı aktarım aygıtları veya kendi disklerinizi kullanarak Azure İçe Aktarma/Dışa Aktarma'dır.

    - **Çevrimdışı aktarımlar için Azure Veri Kutusu ailesi** – Zaman, ağ kullanılabilirliği veya maliyetlerle sınırlı olduğunuzda büyük miktarda veriyi Azure'a taşımak için Microsoft tarafından sağlanan Veri Kutusu aygıtlarından aygıtlar kullanın. Robocopy gibi araçları kullanarak şirket içi verileri kopyalayın. Aktarım için amaçlanan veri boyutuna bağlı olarak, Veri Kutusu Diski, Veri Kutusu veya Veri Kutusu Ağır'dan seçim yapabilirsiniz.
    - **Azure İçe Aktar/Dışa Aktarma** – Azure Blob depolama ve Azure Dosyalarına büyük miktarda veri almak için kendi disk sürücülerinizi göndererek Azure İçe Alma/Dışa Aktarma hizmetini kullanın. Bu hizmet, Azure Blob depolama dan disk sürücülerine veri aktarmak ve şirket içi sitelerinize aktarmak için de kullanılabilir.

- Ağ aktarımının makul olması öngörülüyorsa, [Yüksek ağ bant genişliğinde](#high-network-bandwidth)ayrıntılı olarak belirtilen araçlardan herhangi birini kullanabilirsiniz.


### <a name="high-network-bandwidth-1-gbps---100-gbps"></a>Yüksek ağ bant genişliği (1 Gbps - 100 Gbps)

Kullanılabilir ağ bant genişliği yüksekse, aşağıdaki araçlardan birini kullanın.

- **AzCopy** - Azure Blobs, Files ve Table depolama alanına en iyi performansla verileri kolayca kopyalamak için bu komut satırı aracını kullanın. AzCopy eşzamanlılık ve paralellik ve kesildiğinde kopyalama işlemlerine devam etme yeteneğini destekler.
- **Azure Depolama REST API'leri/SDK'ları** – Bir uygulama geliştirirken, uygulamayı Azure Depolama REST API'lerine karşı geliştirebilir ve birden çok dilde sunulan Azure SDK'larını kullanabilirsiniz.
- **Çevrimiçi aktarımlar için Azure Veri Kutusu ailesi** – Veri Kutusu Kenarı ve Veri Kutusu Ağ Geçidi, verileri Azure'a girip çıkarabilen çevrimiçi ağ aygıtlarıdır. Veri Kutusu Kenarı fiziksel aygıtını, yüklemeden önce verilerin sürekli olarak alınması ve ön işlenmesi için aynı anda ihtiyaç duyulduğunda kullanın. Veri Kutusu Ağ Geçidi, aynı veri aktarım özelliklerine sahip aygıtın sanal bir sürümüdür. Her durumda, veri aktarımı aygıt tarafından yönetilir.
- **Azure Veri Fabrikası** – Veri Fabrikası, bir aktarım işlemini ölçeklendirmek ve düzenleme ve kurumsal sınıf izleme özelliklerine ihtiyaç duyulduğunda kullanılmalıdır. Dosyaları birkaç Azure hizmeti, şirket içi veya ikisinin birleşimi arasında düzenli olarak aktarmak için Veri Fabrikası'nı kullanın. Veri Fabrikası ile, birbirinden farklı veri depolarından veri yutup veri hareketi ve veri dönüşümasyonuna otomatikleştiren veri tabanlı iş akışları (ardışık işler olarak adlandırılır) oluşturabilir ve zamanlayabilirsiniz.

## <a name="comparison-of-key-capabilities"></a>Anahtar özelliklerin karşılaştırılması

Aşağıdaki tablolar, önerilen seçenekler için anahtar yetenekleri farklılıklarını özetler.

### <a name="moderate-network-bandwidth"></a>Orta ağ bant genişliği

Çevrimdışı veri aktarımını kullanıyorsanız, temel özelliklerdeki farklılıkları anlamak için aşağıdaki tabloyu kullanın.

|                                     |    Data Box Disk      |    Data Box                                      |    Data Box Heavy            |    İçeri/Dışarı Aktarma                       |
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


Çevrimiçi veri aktarımını kullanıyorsanız, yüksek ağ bant genişliği için aşağıdaki bölümdeki tabloyu kullanın.

### <a name="high-network-bandwidth"></a>Yüksek ağ bant genişliği

|                                     |    Araçlar azmin, <br>Azure PowerShell, <br>Azure CLI             |    Azure Depolama REST API'leri, SDK'lar                   |    Veri Kutusu Ağ Geçidi veya Veri Kutusu Kenarı          |    Azure Data Factory                                            |
|-------------------------------------|------------------------------------|----------------------------------------------|----------------------------------|-----------------------------------------------------------------------|
|    Veri türü                  |    Azure Blobs, Azure Dosyaları, Azure Tablolar    |    Azure Blobs, Azure Dosyaları, Azure Tablolar    |    Azure Blobs, Azure Dosyaları                           |   Veri depoları ve biçimleri için 70'den fazla veri bağlayıcısı destekler    |
|    Form faktörü                |    Komut satırı araçları                        |    Programlı arayüz                    |    Microsoft sanal bir <br>veya fiziksel cihaz     |    Azure portalında hizmet                                            |
|    İlk tek seferlik kurulum     |    Kolay               |    Orta                       |    Kolay (<30 dakika) orta (1-2 saat)            |    Geniş                                                          |
|    Veri ön işleme              |    Hayır                                        |    Hayır                                        |    Evet (Kenar hesaplamaile)                               |    Evet                                                                |
|    Diğer bulutlardan aktarma       |    Hayır                                        |    Hayır                                        |    Hayır                                                    |    Evet                                                                |
|    Kullanıcı türü                        |    BT Pro veya dev                                       |    Geliştirme                                       |    BT Profesyoneli                                                |    BT Profesyoneli                                                             |
|    Fiyatlandırma                          |    Ücretsiz, veri çıkış ücretleri geçerlidir         |    Ücretsiz, veri çıkış ücretleri geçerlidir         |    [Fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/databox/edge/)                                               |    [Fiyatlandırma](https://azure.microsoft.com/pricing/details/data-factory/)                                                            |

## <a name="next-steps"></a>Sonraki adımlar

- [Alma/Dışa Aktarma ile veri aktarımı yapmayı öğrenin.](/azure/storage/common/storage-import-export-data-to-blobs)
- Nasıl yapılacağını anlama

    - [Veri Kutusu Diski ile veri aktarımı.](https://docs.microsoft.com/azure/databox/data-box-disk-quickstart-portal)
    - [Veri Kutusu ile veri aktarımı.](https://docs.microsoft.com/azure/databox/data-box-quickstart-portal)
- [AzCopy ile veri aktarımı.](/azure/storage/common/storage-use-azcopy-v10)
- Nasıl yapılacağını anlayın:
    - [Veri Kutusu Ağ Geçidi ile veri aktarımı.](https://docs.microsoft.com/azure/databox-online/data-box-gateway-deploy-add-shares)
    - [Azure'a göndermeden önce Verileri Veri Kutusu Kenarı ile dönüştürün.](https://docs.microsoft.com/azure/databox-online/data-box-edge-deploy-configure-compute)
- [Azure Veri Fabrikası ile nasıl veri aktarılamayı öğrenin.](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal)
- Veri aktarmak için REST API'lerini kullanın

    - [In .NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage)
    - [Java üzerinde](https://docs.microsoft.com/java/api/overview/azure/storage)
