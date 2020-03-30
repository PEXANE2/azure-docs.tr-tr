---
title: Azure Veri Gölü Depolamasını Gen1'den Gen2'ye Geçirin
description: Azure Veri Gölü Depolamasını Gen1'den Gen2'ye geçirin.
author: normesta
ms.topic: conceptual
ms.author: normesta
ms.date: 03/11/2020
ms.service: storage
ms.reviewer: rukmani-msft
ms.subservice: data-lake-storage-gen2
ms.openlocfilehash: fb982324b66c5ac0d2db00eb906ed850827bc72e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79533292"
---
# <a name="migrate-azure-data-lake-storage-from-gen1-to-gen2"></a>Azure Veri Gölü Depolamasını Gen1'den Gen2'ye Geçirin

Verilerinizi, iş yüklerinizi ve uygulamalarınızı Veri Gölü Depolama Gen1'den Veri Gölü Depolama Gen2'ye geçirebilirsiniz.

Azure Veri Gölü Depolama Gen2, [Azure Blob depolama](storage-blobs-introduction.md) alanı üzerine kuruludur ve büyük veri analitiğine özel bir dizi özellik sağlar. [Data Lake Storage Gen2,](https://azure.microsoft.com/services/storage/data-lake-storage/) Dosya sistemi semantikleri, dizin ve dosya düzeyi güvenliği gibi [Azure Veri Gölü Depolama Gen1'deki](https://docs.microsoft.com/azure/data-lake-store/index)özellikleri düşük maliyetli, katmanlı depolama, yüksek kullanılabilirlik/olağanüstü durum kurtarma özellikleriyle [Birleştirir.](storage-blobs-introduction.md)

> [!NOTE]
> Daha kolay okumak için bu makalede, Azure Veri Gölü Depolama Gen1'i için *Gen1* terimi ve Azure Veri Gölü Depolama Gen2'ye başvurmak için *Gen2* terimini kullanılır.

## <a name="recommended-approach"></a>Önerilen yaklaşım

Gen2'ye göç etmek için aşağıdaki yaklaşımı öneririz.

:heavy_check_mark: Adım 1: Hazırlığı değerlendirin

:heavy_check_mark: Adım 2: Göç etmeye hazırlanın

:heavy_check_mark: Adım 3: Verileri ve uygulama iş yüklerini geçirin

:heavy_check_mark: Adım 4: Cutover Gen1'den Gen2'ye

> [!NOTE]
> Gen1 ve Gen2 farklı hizmetlerdir, yerinde yükseltme deneyimi yoktur, kasıtlı geçiş çabası gereklidir. 

### <a name="step-1-assess-readiness"></a>Adım 1: Hazırlık durumunu değerlendirin

1. [Data Lake Storage Gen2 teklifi](https://azure.microsoft.com/services/storage/data-lake-storage/)hakkında bilgi edinin; bu yararları, maliyetleri ve genel mimari. 

2. Gen1'in yeteneklerini Gen2'ninkilerle [karşılaştırın.](#gen1-gen2-feature-comparison) 

3. İşlevsellikteki boşlukları değerlendirmek için [bilinen sorunların](data-lake-storage-known-issues.md) listesini gözden geçirin.

4. [Gen2, tanısal günlük,](../common/storage-analytics-logging.md)erişim [katmanları](storage-blob-storage-tiers.md)ve [Blob depolama yaşam döngüsü yönetim ilkeleri](storage-lifecycle-management-concepts.md)gibi Blob depolama özelliklerini destekler. Bu özelliklerden herhangi birini kullanırken ilginçseniz, geçerli destek düzeyini gözden [geçirin.](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-supported-blob-storage-features)

5. Gen2'nin çözümlerinizin bağlı olduğu hizmetleri desteklediğinden emin olmak için [Azure ekosistem desteğinin](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-multi-protocol-access) geçerli durumunu gözden geçirin.

### <a name="step-2-prepare-to-migrate"></a>Adım 2: Göç etmeye hazırlanın

1. Geçireceğiniz veri kümelerini tanımlayın.

   Artık kullanmadığınız veri kümelerini temizlemek için bu fırsatı değerlendirin. Tüm verilerinizi aynı anda geçirmeyi planlamadığınız sürece, aşamalar halinde geçirebileceğiniz mantıksal veri gruplarını tanımlamak için bu zamanı alın.
   
2. Bir geçişin işletmeniz üzerindeki etkisini belirleyin.

   Örneğin, geçiş gerçekleşirken herhangi bir kapalı kalma süresini karşılayıp karşılayamayacağınızı göz önünde bulundurun. Bu hususlar, uygun bir geçiş deseni belirlemenize ve en uygun araçları seçmenize yardımcı olabilir.

3. Bir geçiş planı oluşturun. 

   Bu [geçiş desenlerini](#migration-patterns)öneririz. Bu desenlerden birini seçebilir, bunları biraraya getirebilir veya kendi özel deseninizi tasarlayabilirsiniz.

### <a name="step-3-migrate-data-workloads-and-applications"></a>Adım 3: Verileri, iş yüklerini ve uygulamaları geçirme

Tercih ettiğiniz deseni kullanarak verileri, iş yüklerini ve uygulamaları geçirin. Senaryoları artımlı olarak doğrulamanızı öneririz.

1. [Bir depolama hesabı oluşturun](data-lake-storage-quickstart-create-account.md) ve hiyerarşik ad alanı özelliğini etkinleştirin. 

2. Verilerinizi geçirin. 

3. İş [yüklerinizdeki hizmetleri](data-lake-storage-integrate-with-azure-services.md) Gen2 bitiş noktanıza işaret etmek için yapılandırın. 
   
4. Gen2 API'lerini kullanmak için uygulamaları güncelleştirin. [.NET,](data-lake-storage-directory-file-acl-dotnet.md) [Java,](data-lake-storage-directory-file-acl-java.md) [Python,](data-lake-storage-directory-file-acl-python.md) [JavaScript](data-lake-storage-directory-file-acl-javascript.md) ve [REST](https://docs.microsoft.com/rest/api/storageservices/data-lake-storage-gen2)için kılavuzlara bakın. 
   
5. Veri Gölü Depolama Gen2 [PowerShell cmdlets](data-lake-storage-directory-file-acl-powershell.md)ve [Azure CLI komutları](data-lake-storage-directory-file-acl-cli.md)kullanmak için komut güncelleştirin.
   
6. Kod dosyalarında veya Databricks not defterlerinde, Apache Hive HQL dosyalarında veya iş yüklerinizin bir parçası olarak kullanılan diğer dosyalarda dize `adl://` içeren URI başvurularını arayın. Bu başvuruları yeni depolama hesabınızın [Gen2 biçimlendirilmiş](data-lake-storage-introduction-abfs-uri.md) URI'si ile değiştirin. Örneğin: Gen1 URI: `adl://mydatalakestore.azuredatalakestore.net/mydirectory/myfile` olabilir `abfss://myfilesystem@mydatalakestore.dfs.core.windows.net/mydirectory/myfile`. 

7. Hesabınızdaki güvenliği Rol tabanlı [erişim denetimi (RBAC) rolleri,](../common/storage-auth-aad-rbac-portal.md) [dosya ve klasör düzeyi güvenliği ve](data-lake-storage-access-control.md)Azure Depolama güvenlik duvarlarını ve sanal ağları içerecek şekilde [yapılandırın.](../common/storage-network-security.md)

### <a name="step-4-cutover-from-gen1-to-gen2"></a>Adım 4: Gen1'den Gen2'ye Cutover

Uygulamalarınızın ve iş yüklerinizin Gen2'de kararlı olduğundan emin olduktan sonra, iş senaryolarınızı karşılamak için Gen2'yi kullanmaya başlayabilirsiniz. Gen1'de çalışan kalan tüm boru hatlarını kapatın ve Gen1 hesabınızı devre dışı bırakın. 

<a id="gen1-gen2-feature-comparison" />

## <a name="gen1-vs-gen2-capabilities"></a>Gen1 vs Gen2 yetenekleri

Bu tablo Gen1'in yeteneklerini Gen2'ninkiyle karşılaştırır.

|Alan |Gen1   |Gen2 |
|---|---|---|
|Veri organizasyonu|[Hiyerarşik ad alanı](data-lake-storage-namespace.md)<br>Dosya ve klasör desteği|[Hiyerarşik ad alanı](data-lake-storage-namespace.md)<br>Kapsayıcı, dosya ve klasör desteği |
|Coğrafi artıklık| [LRS](../common/storage-redundancy.md#locally-redundant-storage)| [LRS](../common/storage-redundancy.md#locally-redundant-storage), [ZRS](../common/storage-redundancy.md#zone-redundant-storage), [GRS](../common/storage-redundancy.md#geo-redundant-storage), [RA-GRS](../common/storage-redundancy.md#read-access-to-data-in-the-secondary-region) |
|Kimlik doğrulaması|[AAD yönetilen kimlik](../../active-directory/managed-identities-azure-resources/overview.md)<br>[Hizmet prensipleri](../../active-directory/develop/app-objects-and-service-principals.md)|[AAD yönetilen kimlik](../../active-directory/managed-identities-azure-resources/overview.md)<br>[Hizmet prensipleri](../../active-directory/develop/app-objects-and-service-principals.md)<br>[Paylaşılan Erişim Anahtarı](https://docs.microsoft.com/rest/api/storageservices/authorize-with-shared-key)|
|Yetkilendirme|Yönetim - [RBAC](../../role-based-access-control/overview.md)<br>Veri - [ALA'lar](data-lake-storage-access-control.md)|Yönetim – [RBAC](../../role-based-access-control/overview.md)<br>Veri - [ALA'lar](data-lake-storage-access-control.md), [RBAC](../../role-based-access-control/overview.md) |
|Şifreleme – Veriler istirahatte|Sunucu tarafı – [Microsoft tarafından yönetilen](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) veya müşteri tarafından [yönetilen](../common/encryption-customer-managed-keys.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) anahtarlarla|Sunucu tarafı – [Microsoft tarafından yönetilen](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) veya müşteri tarafından [yönetilen](../common/encryption-customer-managed-keys.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) anahtarlarla|
|VNET Desteği|[VNET Entegrasyonu](../../data-lake-store/data-lake-store-network-security.md)|[Hizmet Bitiş Noktaları](../common/storage-network-security.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json), [Özel Bitiş Noktaları (genel önizleme)](../common/storage-private-endpoints.md)|
|Geliştirici deneyimi|[REST](../../data-lake-store/data-lake-store-data-operations-rest-api.md), [.NET](../../data-lake-store/data-lake-store-data-operations-net-sdk.md), [Java](../../data-lake-store/data-lake-store-get-started-java-sdk.md), [Python](../../data-lake-store/data-lake-store-data-operations-python.md), [PowerShell](../../data-lake-store/data-lake-store-get-started-powershell.md), [Azure CLI](../../data-lake-store/data-lake-store-get-started-cli-2.0.md)|[REST](/rest/api/storageservices/data-lake-storage-gen2), [.NET](data-lake-storage-directory-file-acl-dotnet.md), [Java](data-lake-storage-directory-file-acl-java.md), [Python](data-lake-storage-directory-file-acl-python.md), [JavaScript](data-lake-storage-directory-file-acl-javascript.md), [PowerShell](data-lake-storage-directory-file-acl-powershell.md), [Azure CLI](data-lake-storage-directory-file-acl-cli.md) (Genel önizleme)|
|Tanılama günlükleri|Klasik günlükler<br>[Azure Monitör entegresi](../../data-lake-store/data-lake-store-diagnostic-logs.md)|[Klasik günlükler](../common/storage-analytics-logging.md) (Genel önizlemede)<br>Azure monitör entegrasyonu – zaman çizelgesi TBD|
|Ekosistem|[HDInsight (3.6)](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md), [Azure Databricks (3.1 ve üzeri)](https://docs.databricks.com/data/data-sources/azure/azure-datalake.html), [SQL DW](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store), [ADF](../../data-factory/load-azure-data-lake-store.md)|[HDInsight (3.6, 4.0)](../../hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2.md), [Azure Databricks (5.1 ve üzeri)](https://docs.microsoft.com/azure/databricks/data/data-sources/azure/azure-datalake-gen2), [SQL DW](../../sql-database/sql-database-vnet-service-endpoint-rule-overview.md), [ADF](../../data-factory/load-azure-data-lake-storage-gen2.md)|

<a id="migration-patterns" />

## <a name="gen1-to-gen2-patterns"></a>Gen1 - Gen2 desenleri

Bir geçiş deseni seçin ve sonra bu deseni gerektiği gibi değiştirin.

|||
|---|---|
|**Asansör ve Vardiya**|En basit model. Veri ardışık hatlarınız kapalı kalma süresini karşılayabilirse idealdir.|
|**Artımlı kopya**|Asansör *ve kaydırma*benzer, ama daha az kesinti ile. Kopyalanması daha uzun süren büyük miktarda veri için idealdir.|
|**Çift boru hattı**|Herhangi bir kesinti yiyemez boru hatları için idealdir.|
|**Çift yönlü eşitleme**|Çift *boru hattı*benzer, ancak daha karmaşık boru hatları için uygundur daha aşamalı bir yaklaşım ile.|

Her desene daha yakından bakalım.
 
### <a name="lift-and-shift-pattern"></a>Kaldırma ve kaydırma deseni

Bu en basit model.

1. Gen1'e yazdığı tüm yazıları durdurun.

2. Verileri Gen1'den Gen2'ye taşıyın. Azure [Veri Fabrikası'nı](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage)öneriyoruz. ALA'lar verilerle kopyalar.

3. İşlemleri ve iş yüklerini Gen2'ye yönlendirin.

4. Görevden Alma Gen1.

> [!div class="mx-imgBorder"]
> ![kaldırma ve kaydırma deseni](./media/data-lake-storage-migrate-gen1-to-gen2/lift-and-shift.png)

#### <a name="considerations-for-using-the-lift-and-shift-pattern"></a>Kaldırma ve vites değiştirme desenini kullanmak için dikkat edilmesi gerekenler

:heavy_check_mark: Tüm iş yükleri için aynı anda Gen1'den Gen2'ye kesit.

:heavy_check_mark: Geçiş ve kesme süresi sırasında kesinti bekleyin.

:heavy_check_mark: Kapalı kalma süresini karşılayabilen ve tüm uygulamaların aynı anda yükseltilebilen boru hatları için idealdir.

### <a name="incremental-copy-pattern"></a>Artımlı kopyalama deseni

1. Verileri Gen1'den Gen2'ye taşımaya başlayın. Azure [Veri Fabrikası'nı](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage)öneriyoruz. ALA'lar verilerle kopyalar.

2. Gen1'den gelen yeni verileri aşamalı olarak kopyalayın.

3. Tüm veriler kopyalandıktan sonra, Gen1'e yazdığı tüm yazıları durdurun ve iş yüklerini Gen2'ye işaret edin.

4. Görevden Alma Gen1.

> [!div class="mx-imgBorder"]
> ![Artımlı kopyalama deseni](./media/data-lake-storage-migrate-gen1-to-gen2/incremental-copy.png)

#### <a name="considerations-for-using-the-incremental-copy-pattern"></a>Artımlı kopyalama deseni kullanmak için dikkat edilmesi gerekenler:

:heavy_check_mark: Tüm iş yükleri için aynı anda Gen1'den Gen2'ye kesit.

:heavy_check_mark: Yalnızca kesme döneminde kesinti bekleyin.

:heavy_check_mark: Tüm uygulamaların aynı anda yükseltildiği, ancak veri kopyalamanın daha fazla zaman gerektirdiği boru hatları için idealdir.

### <a name="dual-pipeline-pattern"></a>Çift boru hattı deseni

1. Verileri Gen1'den Gen2'ye taşıyın. Azure [Veri Fabrikası'nı](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage)öneriyoruz. ALA'lar verilerle kopyalar.

2. Hem Gen1 hem de Gen2'ye yeni veriler sindirin.

3. İş yüklerini Gen2'ye yönlendirin.

4. Gen1'e yazdığı tüm yazıları durdurun ve sonra Gen1'i devre dışı bırakın.

> [!div class="mx-imgBorder"]
> ![Çift boru hattı deseni](./media/data-lake-storage-migrate-gen1-to-gen2/dual-pipeline.png)

#### <a name="considerations-for-using-the-dual-pipeline-pattern"></a>Çift boru hattı deseni kullanmak için dikkat edilmesi gerekenler:

:heavy_check_mark: Gen1 ve Gen2 boru hatları yan yana çalışır.

:heavy_check_mark: Sıfır kapalı kalma süresini destekler.

:heavy_check_mark: İş yüklerinizin ve uygulamalarınızın herhangi bir kesinti yiyemez durumda olduğu ve her iki depolama hesabına da giriş yapabileceğiniz durumlarda idealdir.

### <a name="bi-directional-sync-pattern"></a>Çift yönlü eşitleme deseni

1. Gen1 ve Gen2 arasında çift yönlü çoğaltma ayarlayın. Biz [WanDisco](https://docs.wandisco.com/bigdata/wdfusion/adls/)öneririz. Varolan veriler için bir onarım özelliği sunar.

3. Tüm hareketler tamamlandığında, Gen1'e yazdığı tüm yazıları durdurun ve çift yönlü çoğaltmayı kapatın.

4. Görevden Alma Gen1.

> [!div class="mx-imgBorder"]
> ![Çift yönlü desen](./media/data-lake-storage-migrate-gen1-to-gen2/bidirectional-sync.png)

#### <a name="considerations-for-using-the-bi-directional-sync-pattern"></a>Çift yönlü eşitleme deseni kullanmak için dikkat edilmesi gerekenler:

:heavy_check_mark: Aşamalı bir yaklaşımın daha anlamlı olabileceği çok sayıda ardışık kaynak ve bağımlılık içeren karmaşık senaryolar için idealdir.  

:heavy_check_mark: Göç çabası yüksektir, ancak Gen1 ve Gen2 için yan yana destek sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- Bir depolama hesabı için güvenlik ayarlamanın çeşitli bölümleri hakkında bilgi edinin. Bkz. [Azure Depolama güvenlik kılavuzu.](../common/storage-security-guide.md)
- Veri Gölü Mağazanız için performansı optimize edin. [Performans için Azure Veri Gölü Depolama Gen2'yi Optimize Edin](data-lake-storage-performance-tuning-guidance.md)
- Veri Gölü Mağazanızı yönetmek için en iyi uygulamaları gözden geçirin. [Bkz. Azure Veri Gölü Depolama Gen2'yi kullanmak için en iyi uygulamalar](data-lake-storage-best-practices.md)

