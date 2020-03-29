---
title: Azure-SSIS Tümleştirme Çalışma Süresi için performansı yapılandırma
description: Yüksek performans için Azure-SSIS Tümleştirme Çalışma Süresi'nin özelliklerini nasıl yapılandırılayarıştırmayı öğrenin
services: data-factory
ms.date: 01/10/2018
ms.topic: conceptual
ms.service: data-factory
ms.workload: data-services
author: swinarko
ms.author: sawinark
ms.reviewer: ''
manager: anandsub
ms.openlocfilehash: 15aac35a7ebc505e76ddfd0c538c4fddb7b2d9ff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74930539"
---
# <a name="configure-the-azure-ssis-integration-runtime-for-high-performance"></a>Yüksek performans için Azure-SSIS Tümleştirme Çalışma Süresini yapılandırın

Bu makalede, yüksek performans için bir Azure-SSIS Tümleştirme Çalışma Süresi (IR) nasıl yapılandırılabilen açıklanmaktadır. Azure-SSIS IR, SQL Server Tümleştirme Hizmetleri (SSIS) paketlerini Azure'da dağıtmanıza ve çalıştırmanıza olanak tanır. Azure-SSIS IR hakkında daha fazla bilgi için [Tümleştirme çalışma zamanı](concepts-integration-runtime.md#azure-ssis-integration-runtime) makalesine bakın. Azure'da SSIS paketlerini dağıtma ve çalıştırma hakkında bilgi için [SQL Server Tümleştirme Hizmetleri iş yüklerini kaldır ve buluta kaydırın.](/sql/integration-services/lift-shift/ssis-azure-lift-shift-ssis-packages-overview)

> [!IMPORTANT]
> Bu makalede, SSIS geliştirme ekibi üyeleri tarafından yapılan şirket içi testlerden performans sonuçları ve gözlemler yer almaktadır. Sonuçlarınız değişebilir. Hem maliyeti hem de performansı etkileyen yapılandırma ayarlarınızı tamamlamadan önce kendi testlerinizi yapın.

## <a name="properties-to-configure"></a>Yapılandırmak için özellikler

Yapılandırma komut dosyasının aşağıdaki bölümü, bir Azure-SSIS Tümleştirme Çalışma Zamanı oluşturduğunuzda yapılandırabileceğiniz özellikleri gösterir. PowerShell komut dosyasının tamamı ve açıklaması için [bkz.](tutorial-deploy-ssis-packages-azure-powershell.md)

```powershell
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[specify a description for your Azure-SSIS IR]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2
# Azure-SSIS IR edition/license info: Standard or Enterprise
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, up to 4 parallel executions per node are supported, but for other nodes, up to max(2 x number of cores, 8) are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database with virtual network service endpoints/Managed Instance/on-premises data, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use the same subnet as the one used with your Azure SQL Database with virtual network service endpoints or a different subnet than the one used for your Managed Instance

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name or Managed Instance name.DNS prefix].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database or leave it empty for Managed Instance]"
```

## <a name="azuressislocation"></a>AzureSSISKonum
**AzureSSISLocation,** tümleştirme çalışma zamanı çalışan düğümünün konumudur. Alt düğüm, Azure SQL veritabanındaki SSIS Catalog veritabanına (SSISDB) sabit bir bağlantı tutar. **AzureSSISLocation'ı,** tümleştirme nin çalışma süresinin mümkün olduğunca verimli çalışmasını sağlayan SSISDB'yi barındıran SQL Veritabanı sunucusuyla aynı konuma ayarlayın.

## <a name="azuressisnodesize"></a>AzureSSISNodeSize
Azure-SSIS IR de dahil olmak üzere Veri Fabrikası aşağıdaki seçenekleri destekler:
-   Standart\_A4\_v2
-   Standart\_A8\_v2
-   Standart\_D1\_v2
-   Standart\_D2\_v2
-   Standart\_D3\_v2
-   Standart\_D4\_v2
-   Standart\_D2\_v3
-   Standart\_D4\_v3
-   Standart\_D8\_v3
-   Standart\_D16\_v3
-   Standart\_D32\_v3
-   Standart\_D64\_v3
-   Standart\_E2\_v3
-   Standart\_E4\_v3
-   Standart\_E8\_v3
-   Standart\_E16\_v3
-   Standart\_E32\_v3
-   Standart\_E64\_v3

SSIS mühendislik ekibi tarafından yapılan resmi olmayan şirket içi testlerde, D serisi A serisinden daha fazla SSIS paket yürütmesi için daha uygun görüner.

-   D serisinin performans/fiyat oranı A serisinden, v3 serisinin performans/fiyat oranı ise v2 serisinden daha yüksektir.
-   D serisinin iş bilgililiği aynı fiyata A serisinden daha yüksektir ve v3 serisinin elde edilen işeymi aynı fiyata v2 serisinden daha yüksektir.
-   Azure-SSIS IR'nin v2 serisi düğümleri özel kurulum için uygun değildir, bu nedenle lütfen bunun yerine v3 serisi düğümlerini kullanın. V2 serisi düğümlerini zaten kullanıyorsanız, lütfen v3 serisi düğümlerini mümkün olan en kısa sürede kullanmak için geçiş yap.
-   E serisi, diğer makinelere göre daha yüksek bellek-CPU oranı sağlayan bellek en iyi duruma getirilmiş VM boyutlarıdır. Paketiniz çok fazla bellek gerektiriyorsa, E serisi VM'yi seçmeyi düşünebilirsiniz.

### <a name="configure-for-execution-speed"></a>Yürütme hızı için yapılandırın
Çalışacak çok paketiniz yoksa ve paketlerin hızlı çalışmasını istiyorsanız, senaryonuza uygun sanal bir makine türü seçmek için aşağıdaki grafikteki bilgileri kullanın.

Bu veriler, tek bir işçi düğümünde tek bir paket yürütmeyi temsil eder. Paket, Azure Blob Depolama'dan ad ve soyad sütunlarıyla 3 milyon kayıt yükler, tam ad sütunu oluşturur ve tam adı 20 karakterden uzun olan kayıtları Azure Blob Depolama'ya yazar.

![SSIS Entegrasyon Runtime paket yürütme hızı](media/configure-azure-ssis-integration-runtime-performance/ssisir-execution-speedV2.png)

### <a name="configure-for-overall-throughput"></a>Genel iş için yapılandırın

Çalıştırmanız gereken çok sayıda paket varsa ve genel iş kısmını en çok önemsiyorsanız, senaryonuza uygun sanal bir makine türü seçmek için aşağıdaki grafikteki bilgileri kullanın.

![SSIS Entegrasyon Çalışma süresi maksimum genel verim](media/configure-azure-ssis-integration-runtime-performance/ssisir-overall-throughputV2.png)

## <a name="azuressisnodenumber"></a>AzureSSISNodeNumber

**AzureSSISNodeNumber,** tümleştirme çalışma zamanının ölçeklenebilirliğini ayarlar. Tümleştirme çalışma zamanının iş bölümü **AzureSSISNodeNumber**ile orantılıdır. **AzureSSISNodeNumber'ı** ilk başta küçük bir değere ayarlayın, tümleştirme çalışma süresini izleyin ve senaryonuz için değeri ayarlayın. Alt düğüm sayısını yeniden yapılandırmak için [bkz.](manage-azure-ssis-integration-runtime.md)

## <a name="azuressismaxparallelexecutionspernode"></a>AzureSSISMaxParallelExecutionsPerNode

Paketleri çalıştırmak için zaten güçlü bir alt düğüm kullanıyorsanız, **AzureSSISMaxParallelExecutionsPerNode'yi** artırmak tümleştirme çalışma süresinin genel iş artışını artırabilir. düğümStandard_D1_v2 için düğüm başına 1-4 paralel yürütme desteklenir. Diğer tüm düğüm türleri için, düğüm başına 1-max(2 x çekirdek sayısı, 8) paralel yürütmeler desteklenir. Desteklediğimiz maksimum değerin ötesinde **bir AzureSSISMaxParallelExecutionsPerNode** istiyorsanız, bir destek bileti açabilir ve sizin için maksimum değeri artırabilir ve bundan sonra **AzureSSSSMaxParallelExecutionsPerNode'yi**güncellemek için Azure Powershell'i kullanmanız gerekir.
Paketinizin maliyetini ve işçi düğümleri için aşağıdaki yapılandırmaları temel alan uygun değeri tahmin edebilirsiniz. Daha fazla bilgi için [genel amaçlı sanal makine boyutlarına](../virtual-machines/windows/sizes-general.md)bakın.

| Boyut             | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum geçici depolama aktarım hızı: IOPS / Okuma MB/sn / Yazma MB/sn | Maksimum veri diski/aktarım hızı: IOPS | Maks NIC / Beklenen ağ performansı (Mbps) |
|------------------|------|-------------|------------------------|------------------------------------------------------------|-----------------------------------|------------------------------------------------|
| Standart\_D1\_v2 | 1    | 3,5         | 50                     | 3000/46/23                                             | 2/2x500                         | 2 / 750                                        |
| Standart\_D2\_v2 | 2    | 7           | 100                    | 6000/93/46                                             | 4/4x500                         | 2 / 1500                                       |
| Standart\_D3\_v2 | 4    | 14          | 200                    | 12000/187/93                                           | 8/8x500                         | 4 / 3000                                       |
| Standart\_D4\_v2 | 8    | 28          | 400                    | 24000/375/187                                          | 16/16x500                       | 8 / 6000                                       |
| Standart\_A4\_v2 | 4    | 8           | 40                     | 4000/80/40                                             | 8/8x500                         | 4 / 1000                                       |
| Standart\_A8\_v2 | 8    | 16          | 80                     | 8000/160/80                                            | 16/16x500                       | 8 / 2000                                       |
| Standart\_D2\_v3 | 2    | 8           | 50                     | 3000/46/23                                             | 4 / 6x500                         | 2 / 1000                                       |
| Standart\_D4\_v3 | 4    | 16          | 100                    | 6000/93/46                                             | 8 / 12x500                        | 2 / 2000                                       |
| Standart\_D8\_v3 | 8    | 32          | 200                    | 12000/187/93                                           | 16 / 24x500                       | 4 / 4000                                       |
| Standart\_D16\_v3| 16   | 64          | 400                    | 24000/375/187                                          | 32/ 48x500                        | 8 / 8000                                       |
| Standart\_D32\_v3| 32   | 128         | 800                    | 48000/750/375                                          | 32 / 96x500                       | 8 / 16000                                      |
| Standart\_D64\_v3| 64   | 256         | 1600                   | 96000 / 1000 / 500                                         | 32 / 192x500                      | 8 / 30000                                      |
| Standart\_E2\_v3 | 2    | 16          | 50                     | 3000/46/23                                             | 4 / 6x500                         | 2 / 1000                                       |
| Standart\_E4\_v3 | 4    | 32          | 100                    | 6000/93/46                                             | 8 / 12x500                        | 2 / 2000                                       |
| Standart\_E8\_v3 | 8    | 64          | 200                    | 12000/187/93                                           | 16 / 24x500                       | 4 / 4000                                       |
| Standart\_E16\_v3| 16   | 128         | 400                    | 24000/375/187                                          | 32 / 48x500                       | 8 / 8000                                       |
| Standart\_E32\_v3| 32   | 256         | 800                    | 48000/750/375                                          | 32 / 96x500                       | 8 / 16000                                      |
| Standart\_E64\_v3| 64   | 432         | 1600                   | 96000 / 1000 / 500                                         | 32 / 192x500                      | 8 / 30000                                      |

**AzureSSISMaxParallelExecutionsPerNode** özelliği için doğru değeri ayarlamaya yönelik yönergeler şunlardır: 

1. İlk başta küçük bir değere ayarlayın.
2. Genel iş miktarının iyileştirilip iyileştirilmediğini kontrol etmek için küçük bir miktar artırın.
3. Genel verim maksimum değere ulaştığında değeri artırmayı durdurun.

## <a name="ssisdbpricingtier"></a>SSISDBPricingTier

**SSISDBPricingTier,** Azure SQL veritabanındaki SSIS Catalog veritabanının (SSISDB) fiyatlandırma katmanıdır. Bu ayar, IR örneğindeki maksimum işçi sayısını, paket yürütmeyi sıraya koyma hızını ve yürütme günlüğünü yükleme hızını etkiler.

-   Paket yürütme sırası nın hızını ve yürütme günlüğünü yüklemeyi umursamıyorsanız, en düşük veritabanı fiyatlandırma katmanını seçebilirsiniz. Temel fiyatlandırmaya sahip Azure SQL Veritabanı, bir tümleştirme çalışma zamanı örneğinde 8 çalışanı destekler.

-   Alt sayı 8'den fazlaysa veya çekirdek sayısı 50'den fazlaysa Temel'den daha güçlü bir veritabanı seçin. Aksi takdirde veritabanı tümleştirme çalışma zamanı örneğinin darboğazı na olur ve genel performans olumsuz etkilenir.

-   Günlüğe kaydetme düzeyi ayrıntılı olarak ayarlanmışsa s3 gibi daha güçlü bir veritabanı seçin. Resmi olmayan şirket içi testlerimize göre, s3 fiyatlandırma katmanı 2 düğüm, 128 paralel sayım ve ayrıntılı günlük seviyesiyle SSIS paket yürütmeyi destekleyebilir.

Azure portalında bulunan [veritabanı işlem birimi](../sql-database/sql-database-what-is-a-dtu.md) (DTU) kullanım bilgilerine göre veritabanı fiyatlandırma katmanını da ayarlayabilirsiniz.

## <a name="design-for-high-performance"></a>Yüksek performans tasarımı
Azure'da çalışacak bir SSIS paketi tasarlamak, şirket içi yürütme için bir paket tasarlamaktan farklıdır. Birden çok bağımsız görevi aynı pakette birleştirmek yerine, Azure-SSIS IR'de daha verimli yürütülmesi için bunları birkaç pakete ayırın. Birbirlerinin bitmesini beklemek zorunda kalmamak için her paket için bir paket yürütme oluşturun. Bu yaklaşım, Azure-SSIS tümleştirme çalışma zamanının ölçeklenebilirliğinden yararlanır ve genel iş verimliliğini artırır.

## <a name="next-steps"></a>Sonraki adımlar
Azure-SSIS Tümleştirme Çalışma Süresi hakkında daha fazla bilgi edinin. Bkz. [Azure-SSIS Tümleştirme Çalışma Zamanı.](concepts-integration-runtime.md#azure-ssis-integration-runtime)
