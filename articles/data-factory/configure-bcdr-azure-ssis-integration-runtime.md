---
title: SQL veritabanı yük devretmesi için Azure-SSIS Integration Runtime yapılandırma
description: Bu makalede, SSıSDB veritabanı için Azure SQL veritabanı coğrafi çoğaltma ve yük devretme ile Azure-SSIS Integration Runtime nasıl yapılandırılacağı açıklanmaktadır
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
ms.date: 04/09/2020
ms.openlocfilehash: b4b679b15092531ff9553d221f23d7f030fc1def
ms.sourcegitcommit: 3abadafcff7f28a83a3462b7630ee3d1e3189a0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82592103"
---
# <a name="configure-the-azure-ssis-integration-runtime-with-azure-sql-database-geo-replication-and-failover"></a>Azure-SSIS Integration Runtime Azure SQL veritabanı coğrafi çoğaltma ve yük devretme ile yapılandırma

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-xxx-md.md)]

Bu makalede, SSıSDB veritabanı için Azure SQL veritabanı coğrafi çoğaltmasıyla Azure-SSIS Integration Runtime nasıl yapılandırılacağı açıklanır. Yük devretme gerçekleştiğinde, Azure-SSIS IR ikincil veritabanıyla çalışmayı sürdürdüğünden emin olabilirsiniz.

SQL veritabanı için coğrafi çoğaltma ve yük devretme hakkında daha fazla bilgi için bkz. [genel bakış: etkin coğrafi çoğaltma ve otomatik yük devretme grupları](../sql-database/sql-database-geo-replication-overview.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="azure-ssis-ir-failover-with-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği ile yük devretmeyi Azure-SSIS IR

### <a name="prerequisites"></a>Ön koşullar

Azure SQL veritabanı yönetilen örneği, veritabanında depolanan verilerin, kimlik bilgilerinin ve bağlantı bilgilerinin güvenliğinin sağlanmasına yardımcı olmak için **veritabanı ana anahtarı 'nı (DMK)** kullanır. DMK otomatik şifre çözmeyi etkinleştirmek için, anahtarın bir kopyası **sunucu ana anahtarı (SMK)** kullanılarak şifrelenir. Ancak SMK, yük devretme grubunda çoğaltılmaz, bu nedenle yük devretmeden sonra DMK şifre çözme için hem birincil hem de ikincil örneklere ek bir parola eklemeniz gerekir.

1. Birincil örnekteki SSıSDB üzerinde aşağıdaki komutu yürütün. Bu adım yeni bir şifreleme parolası ekliyor.

    ```sql
    ALTER MASTER KEY ADD ENCRYPTION BY PASSWORD = 'password'
    ```

2. Azure SQL veritabanı yönetilen örneği üzerinde yük devretme grubu oluşturun.

3. Yeni şifreleme parolasını kullanarak ikincil örnekte **sp_control_dbmasterkey_password** çalıştırın.

    ```sql
    EXEC sp_control_dbmasterkey_password @db_name = N'SSISDB',   
        @password = N'<password>', @action = N'add';  
    GO
    ```

### <a name="scenario-1---azure-ssis-ir-is-pointing-to-read-write-listener-endpoint"></a>Senaryo 1-Azure-SSIS IR okuma-yazma dinleyicisi uç noktasını işaret ediyor

Azure-SSIS IR, okuma-yazma dinleyicisi uç noktasına işaret etmek istiyorsanız öncelikle birincil sunucu uç noktasını işaret etmeniz gerekir. SSıSDB 'yi yük devretme grubuna geçirdikten sonra, okuma-yazma dinleyicisi uç noktasına geçiş yapabilir ve Azure-SSIS IR yeniden başlatabilirsiniz.

#### <a name="solution"></a>Çözüm

Yük devretme gerçekleştiğinde, aşağıdaki işlemleri yapmanız gerekir:

1. Birincil bölgedeki Azure-SSIS IR durdurun.

2. İkincil örnek için yeni bölge, VNET ve özel kurulum SAS URI 'SI bilgileriyle Azure-SSIS IR düzenleyin. Azure-SSIS IR okuma-yazma dinleyicisine işaret ettiğinden ve uç nokta Azure-SSIS IR olarak saydamsa, uç noktayı düzenlemeniz gerekmez.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
                -VNetId "new VNet" `
                -Subnet "new subnet" `
                -SetupScriptContainerSasUri "new custom setup SAS URI"
    ```

3. Azure-SSIS IR yeniden başlatın.

### <a name="scenario-2---azure-ssis-ir-is-pointing-to-primary-server-endpoint"></a>Senaryo 2-Azure-SSIS IR birincil sunucu uç noktasını gösteriyor

Azure-SSIS IR birincil sunucu uç noktasını işaret ettiğinden senaryo uygundur.

#### <a name="solution"></a>Çözüm

Yük devretme gerçekleştiğinde, aşağıdaki işlemleri yapmanız gerekir:

1. Birincil bölgedeki Azure-SSIS IR durdurun.

2. İkincil örneğin yeni bölgesi, uç nokta ve VNET bilgileriyle Azure-SSIS IR düzenleyin.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
                -CatalogServerEndpoint "Azure SQL Database server endpoint" `
                -CatalogAdminCredential "Azure SQL Database server admin credentials" `
                -VNetId "new VNet" `
                -Subnet "new subnet" `
                -SetupScriptContainerSasUri "new custom setup SAS URI"
    ```

3. Azure-SSIS IR yeniden başlatın.

### <a name="scenario-3---azure-ssis-ir-is-pointing-to-public-endpoint-of-azure-sql-database-managed-instance"></a>Senaryo 3-Azure-SSIS IR Azure SQL veritabanı yönetilen örneği 'nin Genel uç noktasını işaret ediyor

Azure-SSIS IR senaryo, Azure SQL veritabanı yönetilen örneği 'nin Genel uç noktasını işaret ediyor ve VNET 'e katılamasa da uygundur. Senaryo 2 ve bu senaryolar arasındaki tek fark, yük devretmeden sonra Azure-SSIS IR VNET bilgilerini düzenlemeniz gerekmez.

#### <a name="solution"></a>Çözüm

Yük devretme gerçekleştiğinde, aşağıdaki işlemleri yapmanız gerekir:

1. Birincil bölgedeki Azure-SSIS IR durdurun.

2. İkincil örneğin yeni bölgesi ve uç nokta bilgileriyle Azure-SSIS IR düzenleyin.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
                -CatalogServerEndpoint "Azure SQL Database server endpoint" `
                -CatalogAdminCredential "Azure SQL Database server admin credentials" `
                -SetupScriptContainerSasUri "new custom setup SAS URI"
    ```

3. Azure-SSIS IR yeniden başlatın.

### <a name="scenario-4---attaching-an-existing-ssisdb-ssis-catalog-to-a-new-azure-ssis-ir"></a>Senaryo 4-mevcut bir SSıSDB (SSIS Kataloğu) yeni bir Azure-SSIS IR Iliştirme

Bu senaryo, ikincil bölgede yeni bir Azure-SSIS IR sağlamak istiyorsanız veya geçerli bölgede bir ADF veya Azure-SSIS IR olağanüstü durum oluştuğunda SSSıSDB 'nin yeni bir bölgede yeni bir Azure-SSIS IR çalışmaya devam etmesini istiyorsanız uygundur.

#### <a name="solution"></a>Çözüm

Yük devretme gerçekleştiğinde, aşağıdaki işlemleri yapmanız gerekir:

> [!NOTE]
> 4. adım (IR oluşturma) PowerShell aracılığıyla yapılması gerekir. Azure portal, SSıSDB 'nin zaten var olduğunu belirten bir hata bildirir.

1. Birincil bölgedeki Azure-SSIS IR durdurun.

2. ** \<New_data_factory_name\> ** **ve \<new_integration_runtime_name\>** bağlantıları kabul etmek için sssısdb 'deki meta verileri güncelleştirmek üzere saklı yordamı yürütün.
   
    ```sql
    EXEC [catalog].[failover_integration_runtime] @data_factory_name='<new_data_factory_name>', @integration_runtime_name='<new_integration_runtime_name>'
    ```

3. Yeni bölgede ** \<new_data_factory_name\> ** adlı yeni bir veri fabrikası oluşturun. Daha fazla bilgi için bkz. Veri Fabrikası oluşturma.

    ```powershell
    Set-AzDataFactoryV2 -ResourceGroupName "new resource group name" `
                      -Location "new region"`
                      -Name "<new_data_factory_name>"
    ```
    
    Bu PowerShell komutu hakkında daha fazla bilgi için bkz. [PowerShell kullanarak Azure Veri Fabrikası oluşturma](quickstart-create-data-factory-powershell.md)

4. Yeni bölgede Azure PowerShell kullanarak ** \<new_integration_runtime_name\> ** adlı yeni bir Azure-SSIS IR oluşturun.

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

    Bu PowerShell komutu hakkında daha fazla bilgi için bkz [. Azure Data Factory Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md)



## <a name="azure-ssis-ir-failover-with-azure-sql-database"></a>Azure SQL veritabanı ile yük devretmeyi Azure-SSIS IR

### <a name="scenario-1---azure-ssis-ir-is-pointing-to-read-write-listener-endpoint"></a>Senaryo 1-Azure-SSIS IR okuma-yazma dinleyicisi uç noktasını işaret ediyor

Bu senaryo, yük devretme grubunun okuma/yazma dinleyicisi uç noktasını işaret ettiğinden ve SQL veritabanı sunucusunun sanal ağ hizmeti uç noktası kuralıyla *yapılandırılmadığı Azure-SSIS IR* uygundur. Azure-SSIS IR, okuma-yazma dinleyicisi uç noktasına işaret etmek istiyorsanız öncelikle birincil sunucu uç noktasını işaret etmeniz gerekir. SSıSDB 'yi yük devretme grubuna geçirdikten sonra, okuma-yazma dinleyicisi uç noktasına geçiş yapabilir ve Azure-SSIS IR yeniden başlatabilirsiniz.

#### <a name="solution"></a>Çözüm

Yük devretme gerçekleştiğinde Azure-SSIS IR saydamdır. Azure-SSIS IR, yük devretme grubunun yeni birincili otomatik olarak bağlanır. Azure-SSIS IR bölge veya diğer bilgileri güncelleştirmek istiyorsanız, bunu durdurabilir, düzenleyebilir ve yeniden başlatabilirsiniz.


### <a name="scenario-2---azure-ssis-ir-is-pointing-to-primary-server-endpoint"></a>Senaryo 2-Azure-SSIS IR birincil sunucu uç noktasını gösteriyor

Azure-SSIS IR birincil sunucu uç noktasını işaret ettiğinden senaryo uygundur.

#### <a name="solution"></a>Çözüm

Yük devretme gerçekleştiğinde, aşağıdaki işlemleri yapmanız gerekir:

1. Birincil bölgedeki Azure-SSIS IR durdurun.

2. İkincil örneğin yeni bölgesi, uç noktası ve VNET bilgileriyle Azure-SSIS IR düzenleyin.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
                    -CatalogServerEndpoint "Azure SQL Database server endpoint" `
                    -CatalogAdminCredential "Azure SQL Database server admin credentials" `
                    -VNetId "new VNet" `
                    -Subnet "new subnet" `
                    -SetupScriptContainerSasUri "new custom setup SAS URI"
    ```

3. Azure-SSIS IR yeniden başlatın.

### <a name="scenario-3---attaching-an-existing-ssisdb-ssis-catalog-to-a-new-azure-ssis-ir"></a>Senaryo 3-var olan bir SSıSDB (SSIS Kataloğu) yeni bir Azure-SSIS IR Iliştirme

Bu senaryo, ikincil bölgede yeni bir Azure-SSIS IR sağlamak istiyorsanız veya geçerli bölgede bir ADF veya Azure-SSIS IR olağanüstü durum oluştuğunda SSSıSDB 'nin yeni bir bölgede yeni bir Azure-SSIS IR çalışmaya devam etmesini istiyorsanız uygundur.

#### <a name="solution"></a>Çözüm

> [!NOTE]
> 4. adım (IR oluşturma) PowerShell aracılığıyla yapılması gerekir. Azure portal, SSıSDB 'nin zaten var olduğunu belirten bir hata bildirir.

1. Birincil bölgedeki Azure-SSIS IR durdurun.

2. ** \<New_data_factory_name\> ** **ve \<new_integration_runtime_name\>** bağlantıları kabul etmek için sssısdb 'deki meta verileri güncelleştirmek üzere saklı yordamı yürütün.
   
    ```sql
    EXEC [catalog].[failover_integration_runtime] @data_factory_name='<new_data_factory_name>', @integration_runtime_name='<new_integration_runtime_name>'
    ```

3. Yeni bölgede ** \<new_data_factory_name\> ** adlı yeni bir veri fabrikası oluşturun. Daha fazla bilgi için bkz. Veri Fabrikası oluşturma.

    ```powershell
    Set-AzDataFactoryV2 -ResourceGroupName "new resource group name" `
                         -Location "new region"`
                         -Name "<new_data_factory_name>"
    ```
    
    Bu PowerShell komutu hakkında daha fazla bilgi için bkz. [PowerShell kullanarak Azure Veri Fabrikası oluşturma](quickstart-create-data-factory-powershell.md)

4. Yeni bölgede Azure PowerShell kullanarak ** \<new_integration_runtime_name\> ** adlı yeni bir Azure-SSIS IR oluşturun.

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

    Bu PowerShell komutu hakkında daha fazla bilgi için bkz [. Azure Data Factory Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md)


## <a name="next-steps"></a>Sonraki adımlar

Azure-SSIS IR için diğer yapılandırma seçeneklerini göz önünde bulundurun:

- [Yüksek performans için Azure-SSIS Integration Runtime yapılandırma](configure-azure-ssis-integration-runtime-performance.md)

- [Azure-SSIS Integration Runtime için kurulumu özelleştirme](how-to-configure-azure-ssis-ir-custom-setup.md)

- [Azure-SSIS Integration Runtime için Enterprise Edition sağlama](how-to-configure-azure-ssis-ir-enterprise-edition.md)
