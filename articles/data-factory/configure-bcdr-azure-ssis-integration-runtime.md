---
title: SQL Veritabanı failover için Azure-SSIS Tümleştirme Çalışma Süresini Yapılandırma
description: Bu makalede, Azure-SSIS Tümleştirme Çalışma Süresi'nin Azure SQL Veritabanı coğrafi çoğaltma ve SSISDB veritabanı için başarısız olması yla nasıl yapılandırılanınca açıklanmaktadır.
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.devlang: powershell
author: swinarko
ms.author: sawinark
manager: mflasko
ms.reviewer: douglasl
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 08/14/2018
ms.openlocfilehash: 92f7d25a9c19409b220b6a71fba87da91e51a415
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74928492"
---
# <a name="configure-the-azure-ssis-integration-runtime-with-azure-sql-database-geo-replication-and-failover"></a>Azure-SSIS Tümleştirme Çalışma Süresini Azure SQL Veritabanı coğrafi çoğaltma ve başarısızlıkla yapılandırın

Bu makalede, SSISDB veritabanı için Azure-SSIS Tümleştirme Çalışma Süresi'nin Azure SQL Veritabanı coğrafi çoğaltmaile nasıl yapılandırılacaklığı açıklanmaktadır. Bir hata olduğunda, Azure-SSIS IR'nin ikincil veritabanıyla çalışmaya devam ettiğinden emin olabilirsiniz.

SQL Veritabanı için coğrafi çoğaltma ve başarısızlık hakkında daha fazla bilgi için genel [bakış: Etkin coğrafi çoğaltma ve otomatik hata grupları.](../sql-database/sql-database-geo-replication-overview.md)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="scenario-1---azure-ssis-ir-is-pointing-to-read-write-listener-endpoint"></a>Senaryo 1 - Azure-SSIS IR okuma-yazma dinleyici bitiş noktasını işaret ediyor

### <a name="conditions"></a>Koşullar

Bu bölüm, aşağıdaki koşullar doğru olduğunda geçerlidir:

- Azure-SSIS IR, başarısız grubun okuma yazma dinleyici uç noktasını işaret ediyor.

  AND

- SQL Veritabanı sunucusu *not* sanal ağ hizmeti bitiş noktası kuralı ile yapılandırılmamıştır.

### <a name="solution"></a>Çözüm

Başarısız olduğunda, Azure-SSIS IR için saydamdır. Azure-SSIS IR, başarısız grubun yeni birincil ine otomatik olarak bağlanır.

## <a name="scenario-2---azure-ssis-ir-is-pointing-to-primary-server-endpoint"></a>Senaryo 2 - Azure-SSIS IR birincil sunucu bitiş noktasını işaret ediyor

### <a name="conditions"></a>Koşullar

Bu bölüm, aşağıdaki koşullardan biri doğru olduğunda geçerlidir:

- Azure-SSIS IR, başarısız grubun birincil sunucu bitiş noktasını işaret ediyor. Bu bitiş noktası, başarısız olduğunda değişir.

  OR

- Azure SQL Veritabanı sunucusu sanal ağ hizmeti bitiş noktası kuralıyla yapılandırılır.

  OR

- Veritabanı sunucusu, sanal ağ ile yapılandırılan bir SQL Veritabanı Yönetilen Örneğidir.

### <a name="solution"></a>Çözüm

Başarısız olduğunda, aşağıdaki şeyleri yapmanız gerekir:

1. Azure-SSIS IR'yi durdurun.

2. IR'yi yeni birincil bitiş noktasına ve yeni bölgedeki sanal ağa işaret etmek için yeniden yapılandırın.

3. IR'yi yeniden başlatın.

Aşağıdaki bölümlerde bu adımları daha ayrıntılı olarak açıklayınız.

### <a name="prerequisites"></a>Ön koşullar

- Sunucunun aynı anda kesintisi olması durumunda Azure SQL Veritabanı sunucunuz için olağanüstü durum kurtarma özelliğini etkinleştirdiğinizden emin olun. Daha fazla bilgi için Azure [SQL Veritabanı ile iş sürekliliğine genel bakış](../sql-database/sql-database-business-continuity.md)bölümüne bakın.

- Geçerli bölgede sanal ağ kullanıyorsanız, Azure-SSIS tümleştirme çalışma sürenizi bağlamak için yeni bölgede başka bir sanal ağ kullanmanız gerekir. Daha fazla bilgi için bkz: [Sanal ağa Azure-SSIS tümleştirme çalışma zamanı katılın.](join-azure-ssis-integration-runtime-virtual-network.md)

- Özel bir kurulum kullanıyorsanız, özel kurulum komut dosyanızı ve ilişkili dosyaları depolayan blob kapsayıcısı için başka bir SAS URI hazırlamanız gerekebilir, bu nedenle kesinti sırasında erişilebilir olmaya devam eder. Daha fazla bilgi için bkz: [Azure-SSIS tümleştirme çalışma zamanında özel bir kurulum yapıla.](how-to-configure-azure-ssis-ir-custom-setup.md)

### <a name="steps"></a>Adımlar

Azure-SSIS IR'nizi durdurmak, IR'yi yeni bir bölgeye geçmek ve yeniden başlatmak için aşağıdaki adımları izleyin.

1. Orijinal bölgede IR durdurun.

2. Yeni ayarlarla IR'yi güncellemek için PowerShell'deki aşağıdaki komutu arayın.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
                    -CatalogServerEndpoint "Azure SQL Database server endpoint" `
                    -CatalogAdminCredential "Azure SQL Database server admin credentials" `
                    -VNetId "new VNet" `
                    -Subnet "new subnet" `
                    -SetupScriptContainerSasUri "new custom setup SAS URI"
    ```

    Bu PowerShell komutu hakkında daha fazla bilgi için Azure [Veri Fabrikası'nda Azure-SSIS tümleştirme çalışma süresini oluştur'a](create-azure-ssis-integration-runtime.md) bakın

3. IR'yi yeniden başlatın.

## <a name="scenario-3---attaching-an-existing-ssisdb-ssis-catalog-to-a-new-azure-ssis-ir"></a>Senaryo 3 - Yeni bir Azure-SSIS IR'sine varolan bir SSISDB (SSIS kataloğu) ekleme

Geçerli bölgede bir ADF veya Azure-SSIS IR felaketi meydana geldiğinde, SSISDB'nizin yeni bir bölgede yeni bir Azure-SSIS IR ile çalışmaya devam edin.

### <a name="prerequisites"></a>Ön koşullar

- Geçerli bölgede sanal ağ kullanıyorsanız, Azure-SSIS tümleştirme çalışma sürenizi bağlamak için yeni bölgede başka bir sanal ağ kullanmanız gerekir. Daha fazla bilgi için bkz: [Sanal ağa Azure-SSIS tümleştirme çalışma zamanı katılın.](join-azure-ssis-integration-runtime-virtual-network.md)

- Özel bir kurulum kullanıyorsanız, özel kurulum komut dosyanızı ve ilişkili dosyaları depolayan blob kapsayıcısı için başka bir SAS URI hazırlamanız gerekebilir, bu nedenle kesinti sırasında erişilebilir olmaya devam eder. Daha fazla bilgi için bkz: [Azure-SSIS tümleştirme çalışma zamanında özel bir kurulum yapıla.](how-to-configure-azure-ssis-ir-custom-setup.md)

### <a name="steps"></a>Adımlar

Azure-SSIS IR'nizi durdurmak, IR'yi yeni bir bölgeye geçmek ve yeniden başlatmak için aşağıdaki adımları izleyin.

1. SSISDB'yi ** \<\> new_data_factory_name** veya ** \<new_integration_runtime_name\>** bağlı hale getirmek için depolanan yordamı çalıştırın.
   
  ```SQL
    EXEC [catalog].[failover_integration_runtime] @data_factory_name='<new_data_factory_name>', @integration_runtime_name='<new_integration_runtime_name>'
   ```

2. Yeni bölgede ** \<new_data_factory_name\> ** adlı yeni bir veri fabrikası oluşturun. Daha fazla bilgi için bkz.

     ```powershell
     Set-AzDataFactoryV2 -ResourceGroupName "new resource group name" `
                         -Location "new region"`
                         -Name "<new_data_factory_name>"
     ```
    Bu PowerShell komutu hakkında daha fazla bilgi için [PowerShell kullanarak bir Azure veri fabrikası oluşturma](quickstart-create-data-factory-powershell.md)

3. Azure PowerShell'i kullanarak yeni bölgede ** \<new_integration_runtime_name\> ** adında yeni bir Azure-SSIS IR oluşturun.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName "new resource group name" `
                                           -DataFactoryName "new data factory name" `
                                           -Name "<new_integration_runtime_name>" `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -VnetId "new vnet" `
                                           -Subnet "new subnet" `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogPricingTier $SSISDBPricingTier
    ```

    Bu PowerShell komutu hakkında daha fazla bilgi için Azure [Veri Fabrikası'nda Azure-SSIS tümleştirme çalışma süresini oluştur'a](create-azure-ssis-integration-runtime.md) bakın

4. IR'yi yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

Azure-SSIS IR için diğer yapılandırma seçeneklerini göz önünde bulundurun:

- [Yüksek performans için Azure-SSIS Tümleştirme Çalışma Süresini yapılandırın](configure-azure-ssis-integration-runtime-performance.md)

- [Azure-SSIS Integration Runtime için kurulumu özelleştirme](how-to-configure-azure-ssis-ir-custom-setup.md)

- [Azure-SSIS Tümleştirme Çalışma Süresi için Provizyon Enterprise Edition](how-to-configure-azure-ssis-ir-enterprise-edition.md)
