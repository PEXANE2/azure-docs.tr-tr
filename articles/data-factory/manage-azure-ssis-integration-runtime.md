---
title: Azure-SSIS tümleştirme çalışma zamanını yeniden yapılandırma
description: Azure Veri Fabrikası'nda azure-SSIS tümleştirme çalışma süresini, zaten sağlamış olduktan sonra nasıl yeniden yapılandırabileceğinizi öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/22/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: anandsub
ms.openlocfilehash: fbac52d65433f2137d565ca60fcf754e49199640
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74931390"
---
# <a name="reconfigure-the-azure-ssis-integration-runtime"></a>Azure-SSIS tümleştirme çalışma zamanını yeniden yapılandırma
Bu makalede, varolan bir Azure-SSIS tümleştirme çalışma süresinin nasıl yeniden yapılandırılabildiğini açıklanmaktadır. Azure Veri Fabrikası'nda Bir Azure-SSIS tümleştirme çalışma [zamanı](create-azure-ssis-integration-runtime.md)(IR) oluşturmak için bkz.  

## <a name="data-factory-ui"></a>Data Factory Kullanıcı Arabirimi (UI) 
Bir Azure-SSIS IR'sini durdurmak, düzenlemek/yeniden yapılandırmak veya silmek için Veri Fabrikası Kullanıcı Aracı'nı kullanabilirsiniz. 

1. Veri **Fabrikası UI'sinde,** **Edit** sekmesine geçin. Veri Fabrikası Kullanıcı Birası'nı başlatmak için, veri fabrikanızın ana sayfasında **Yazar & Monitör'ü** tıklatın.
2. Sol **bölmede, Bağlantılar'ı**tıklatın.
3. Sağ bölmede, **Tümleştirme Runtimes'a**geçin. 
4. Tümleştirme çalışma süresini **durdurmak,** **düzenlemek**veya **silmek** için Eylemler sütunundaki düğmeleri kullanabilirsiniz. **Eylemler** sütunundaki **Kod** düğmesi, tümleştirme çalışma zamanıyla ilişkili JSON tanımını görüntülemenizi sağlar.  
    
    ![Azure SSIS IR için eylemler](./media/manage-azure-ssis-integration-runtime/actions-for-azure-ssis-ir.png)

### <a name="to-reconfigure-an-azure-ssis-ir"></a>Azure-SSIS IR'yi yeniden yapılandırmak için
1. **Eylemler** sütununda **Durdur'u** tıklatarak tümleştirme çalışma süresini durdurun. Liste görünümünü yenilemek için araç çubuğunda **Yenile'yi** tıklatın. IR durdurulduktan sonra, ilk eylemin IR'yi başlatmanızı engellediğini görürsünüz. 

    ![Azure SSIS IR için eylemler - durdurulduktan sonra](./media/manage-azure-ssis-integration-runtime/actions-after-ssis-ir-stopped.png)
2. **Eylemler** sütunundaki **Düzenleme** düğmesini tıklatarak IR'yi düzenleme/yeniden yapılandırma. **Tümleştirme Çalışma Zamanı Kurulumu** penceresinde, ayarları değiştirin (örneğin, düğümün boyutu, düğüm sayısı veya düğüm başına en büyük paralel yürütmeler). 
3. IR'yi yeniden başlatmak için **Eylemler** sütunundaki **Başlat** düğmesini tıklatın.     

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure-SSIS tümleştirme çalışma süresini sağlayıp başlattıktan sonra, art arda bir `Stop`  -  `Set`  -  `Start` dizi PowerShell cmdlet çalıştırarak yeniden yapılandırabilirsiniz. Örneğin, aşağıdaki PowerShell komut dosyası, Azure-SSIS tümleştirme çalışma zamanı örneği için ayrılan düğüm sayısını beşe değiştirir.

### <a name="reconfigure-an-azure-ssis-ir"></a>Azure-SSIS IR'yi yeniden yapılandırma

1. İlk olarak, [Stop-AzDataFactoryV2IntegrationRuntime](/powershell/module/az.datafactory/stop-Azdatafactoryv2integrationruntime) cmdlet'i kullanarak Azure-SSIS tümleştirme çalışma süresini durdurun. Bu komut tüm düğümlerini serbest bırakır ve faturalandırmayı durdurur.

    ```powershell
    Stop-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName 
    ```
2. Ardından, [Set-AzDataFactoryV2IntegrationRuntime](/powershell/module/az.datafactory/set-Azdatafactoryv2integrationruntime) cmdlet'i kullanarak Azure-SSIS IR'yi yeniden yapılandırın. Aşağıdaki örnek komut, bir Azure-SSIS tümleştirme çalışma süresini beş düğüme ölçeklendirür.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -NodeCount 5
    ```  
3. Ardından, [Start-AzDataFactoryV2IntegrationRuntime](/powershell/module/az.datafactory/start-Azdatafactoryv2integrationruntime) cmdlet'i kullanarak Azure-SSIS tümleştirme çalışma saatini başlatın. Bu komut, tüm düğümlerini SSIS paketlerini çalıştırmak için ayırır.   

    ```powershell
    Start-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName
    ```

### <a name="delete-an-azure-ssis-ir"></a>Azure-SSIS IR silme
1. İlk olarak, veri fabrikanızın altındaki mevcut tüm Azure SSIS IRs'lerini listele.

    ```powershell
    Get-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName -Status
    ```
2. Ardından, veri fabrikanızdaki tüm mevcut Azure SSIS IRs'lerini durdurun.

    ```powershell
    Stop-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
    ```
3. Ardından, veri fabrikanızdaki mevcut tüm Azure SSIS IRs'lerini tek tek kaldırın.

    ```powershell
    Remove-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
    ```
4. Son olarak, veri fabrikanızı kaldırın.

    ```powershell
    Remove-AzDataFactoryV2 -Name $DataFactoryName -ResourceGroupName $ResourceGroupName -Force
    ```
5. Yeni bir kaynak grubu oluşturduysanız, kaynak grubunu kaldırın.

    ```powershell
    Remove-AzResourceGroup -Name $ResourceGroupName -Force 
    ```

## <a name="next-steps"></a>Sonraki adımlar
Azure-SSIS çalışma zamanı hakkında daha fazla bilgi için aşağıdaki konulara bakın: 

- [Azure-SSIS Tümleştirme Çalışma Zamanı.](concepts-integration-runtime.md#azure-ssis-integration-runtime) Bu makalede, Azure-SSIS IR dahil olmak üzere genel olarak tümleştirme çalışma saatleri hakkında kavramsal bilgiler sağlar. 
- [Öğretici: SSIS paketlerini Azure’a dağıtma](tutorial-create-azure-ssis-runtime-portal.md). Bu makale bir Azure-SSIS IR oluşturmaya ilişkin adım adım yönergeler sağlar ve SSIS kataloğunu barındırmak için bir Azure SQL veritabanı kullanır. 
- [Nasıl yapılır: Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md). Bu makale, öğreticiyi genişletir ve Azure SQL Veritabanı Yönetilen Örneği'ni kullanma ve IR'yi sanal bir ağa katılma hakkında yönergeler sağlar. 
- [Azure-SSIS IR’yi bir sanal ağa ekleyin](join-azure-ssis-integration-runtime-virtual-network.md). Bu makale Azure-SSIS IR’yi bir Azure sanal ağına ekleme hakkında kavramsal bilgiler sağlar. Ayrıca, Azure portalını kullanarak Azure-SSIS IR’nin sanal ağa katılmasını sağlayacak şekilde sanal ağı yapılandırma adımlarını da sunar. 
- [Azure-SSIS IR’yi izleme](monitor-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede bir Azure-SSIS IR ile ilgili bilgileri ve döndürülen bilgilerdeki durumların açıklamalarını alma işlemi gösterilmektedir. 
 
