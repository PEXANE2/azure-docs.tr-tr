---
title: Azure Veri Fabrikası'nı kullanarak SSIS paketini çağırma - Depolanan Yordam Etkinliği
description: Bu makalede, Saklı Yordam Etkinliği'ni kullanarak Bir Azure Veri Fabrikası ardışık hattından bir SQL Server Tümleştirme Hizmetleri (SSIS) paketinin nasıl çağrılması açıklanmaktadır.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 01/19/2018
ms.author: jingwang
ms.openlocfilehash: ea86c4670a8eb6dc5e2133ed01045e8aada0f707
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75438796"
---
# <a name="invoke-an-ssis-package-using-stored-procedure-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'nda depolanan yordam etkinliğini kullanarak bir SSIS paketi çağırma
Bu makalede, depolanmış yordam etkinliği kullanarak bir Azure Veri Fabrikası ardışık kaynaktan bir SSIS paketinin nasıl çağrılması açıklanmaktadır. 

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, ['de depolanan yordam etkinliğini kullanarak SSIS paketlerini çağırın](../how-to-invoke-ssis-package-stored-procedure-activity.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

### <a name="azure-sql-database"></a>Azure SQL Veritabanı 
Bu makaledeki gözden geçirme, SSIS kataloğunu barındıran bir Azure SQL veritabanı kullanır. Azure SQL Veritabanı Yönetilen Örneği de kullanabilirsiniz.

### <a name="create-an-azure-ssis-integration-runtime"></a>Azure SSIS tümleştirme çalışma zamanı oluşturma
Öğreticideki adım adım talimatı takip ederek bir Azure-SSIS tümleştirme çalışma zamanı [oluşturun: SSIS paketlerini dağıtın.](../tutorial-create-azure-ssis-runtime-portal.md) Azure-SSIS tümleştirme çalışma zamanı oluşturmak için Veri Fabrikası sürüm 1'i kullanamazsınız. 

## <a name="azure-powershell"></a>Azure PowerShell
Bu bölümde, SSIS paketini çağıran depolanmış yordam etkinliği içeren bir Veri Fabrikası ardışık hattı oluşturmak için Azure PowerShell'i kullanırsınız.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[Azure PowerShell'in nasıl yüklenir ve yapılandırılabilen](/powershell/azure/install-az-ps)yönergeleri izleyerek en son Azure PowerShell modüllerini yükleyin.

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Aşağıdaki yordam, bir veri fabrikası oluşturmak için adımlar sağlar. Bu veri fabrikasında depolanmış yordam etkinliği içeren bir ardışık hatlar oluşturursunuz. Depolanan yordam etkinliği, SSIS paketinizi çalıştırmak için SSISDB veritabanında depolanmış bir yordam yürütür.

1. Daha sonra PowerShell komutlarında kullanacağınız kaynak grubu adı için bir değişken tanımlayın. Aşağıdaki komut metnini PowerShell'e kopyalayın [Azure kaynak grubu](../../azure-resource-manager/management/overview.md) için çift tırnak içinde bir ad belirtin ve ardından komutu çalıştırın. Örneğin: `"adfrg"`. 
   
     ```powershell
    $resourceGroupName = "ADFTutorialResourceGroup";
    ```

    Kaynak grubu zaten varsa, üzerine yazılmasını istemeyebilirsiniz. `$ResourceGroupName` değişkenine farklı bir değer atayın ve komutu yeniden çalıştırın
2. Azure kaynak grubunu oluşturmak için aşağıdaki komutu çalıştırın: 

    ```powershell
    $ResGrp = New-AzResourceGroup $resourceGroupName -location 'eastus'
    ``` 
    Kaynak grubu zaten varsa, üzerine yazılmasını istemeyebilirsiniz. `$ResourceGroupName` değişkenine farklı bir değer atayın ve komutu yeniden çalıştırın. 
3. Veri fabrikasının adı için bir değişken tanımlayın. 

    > [!IMPORTANT]
    >  Veri fabrikasının adını genel olarak benzersiz olacak şekilde güncelleştirin. 

    ```powershell
    $DataFactoryName = "ADFTutorialFactory";
    ```

5. Veri fabrikasını oluşturmak için, $ResGrp değişkenindeki Konum ve ResourceGroupName özelliğini kullanarak aşağıdaki **New-AzDataFactory** cmdlet'i çalıştırın: 
    
    ```powershell       
    $df = New-AzDataFactory -ResourceGroupName $ResourceGroupName -Name $dataFactoryName -Location "East US"
    ```

Aşağıdaki noktalara dikkat edin:

* Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Aşağıdaki hata iletisini alırsanız adı değiştirip yeniden deneyin.

    ```
    The specified Data Factory name 'ADFTutorialFactory' is already in use. Data Factory names must be globally unique.
    ```
* Data Factory örnekleri oluşturmak için, Azure'da oturum açarken kullandığınız kullanıcı hesabı **katkıda bulunan** veya **sahip** rollerinin üyesi ya da bir Azure aboneliğinin **yöneticisi** olmalıdır.

### <a name="create-an-azure-sql-database-linked-service"></a>Azure SQL Veritabanı bağlı hizmeti oluşturma
SSIS kataloğunu veri fabrikanıza bağlayan Azure SQL veritabanınızı bağlamak için bağlantılı bir hizmet oluşturun. Veri Fabrikası, SSISDB veritabanına bağlanmak için bu bağlantılı hizmetteki bilgileri kullanır ve bir SSIS paketini çalıştırmak için depolanmış bir yordam yürütür. 

1. **C:\ADF\RunSSISPackage** klasöründe aşağıdaki içeriği içeren **AzureSqlDatabaseLinkedService.json** adlı bir JSON dosyası oluşturun: 

    > [!IMPORTANT]
    > Dosyayı&gt;kaydetmeden önce sunucu adı, &lt;kullanıcı adı sunucu&gt;@&lt;adı&gt; ve &lt;parolayı&gt; Azure SQL Veritabanınızın değerleriyle değiştirin. &lt;

    ```json
    {
        "name": "AzureSqlDatabaseLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=SSISDB;User ID=<username>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        }
        }
    ```
2. **Azure PowerShell'de** **C:\ADF\RunSSISPackage** klasörüne geçin.
3. Bağlantılı hizmeti oluşturmak için **New-AzDataFactoryLinkedService** cmdlet'i çalıştırın: **AzureSqlDatabaseLinkedService**. 

    ```powershell
    New-AzDataFactoryLinkedService $df -File ".\AzureSqlDatabaseLinkedService.json"
    ```

### <a name="create-an-output-dataset"></a>Çıktı veri kümesi oluşturma
Bu çıktı veri kümesi, ardışık programın zamanlamasını yönlendiren sahte bir veri kümesidir. Frekansın Saat ve aralığın 1 olarak ayarlı olduğuna dikkat edin. Bu nedenle, ardışık hatlar, ardışık hatbaşlangıç ve bitiş saatleri içinde saatte bir kez çalışır. 

1. Aşağıdaki içeriği içeren bir OutputDataset.json dosyası oluşturun: 
    
    ```json
    {
        "name": "sprocsampleout",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": { },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```
2. Bir veri kümesi oluşturmak için **New-AzDataFactoryDataset** cmdlet'i çalıştırın. 

    ```powershell
    New-AzDataFactoryDataset $df -File ".\OutputDataset.json"
    ```

### <a name="create-a-pipeline-with-stored-procedure-activity"></a>Depolanan yordam etkinliği içeren bir ardışık işlem hattı oluşturma 
Bu adımda, depolanmış yordam etkinliği olan bir ardışık işlem hattı oluşturursunuz. Etkinlik, SSIS paketinizi çalıştırmak için depolanan sp_executesql yordamı çağırır. 

1. **C:\ADF\RunSSISPackage** klasöründe Aşağıdaki içeriği içeren **MyPipeline.json** adlı bir JSON dosyası oluşturun:

    > [!IMPORTANT]
    > Dosyayı&gt;kaydetmeden önce Klasör adı, &lt;proje adı,&gt; &lt;paket adını&gt; klasör, proje ve paket adlarıyla ssis kataloğunda değiştirin. &lt;

    ```json
    {
        "name": "MyPipeline",
        "properties": {
            "activities": [{
                "name": "SprocActivitySample",
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_executesql",
                    "storedProcedureParameters": {
                        "stmt": "DECLARE @return_value INT, @exe_id BIGINT, @err_msg NVARCHAR(150)    EXEC @return_value=[SSISDB].[catalog].[create_execution] @folder_name=N'<folder name>', @project_name=N'<project name>', @package_name=N'<package name>', @use32bitruntime=0, @runinscaleout=1, @useanyworker=1, @execution_id=@exe_id OUTPUT    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1    EXEC [SSISDB].[catalog].[start_execution] @execution_id=@exe_id, @retry_count=0    IF(SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id=@exe_id)<>7 BEGIN SET @err_msg=N'Your package execution did not succeed for execution ID: ' + CAST(@exe_id AS NVARCHAR(20)) RAISERROR(@err_msg,15,1) END"
                    }
                },
                "outputs": [{
                    "name": "sprocsampleout"
                }],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }],
            "start": "2017-10-01T00:00:00Z",
            "end": "2017-10-01T05:00:00Z",
            "isPaused": false
        }
    }    
    ```

2. Boru hattı oluşturmak için: **RunSSISPackagePipeline**, **New-AzDataFactoryPipeline** cmdlet çalıştırın.

    ```powershell
    $DFPipeLine = New-AzDataFactoryPipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "RunSSISPackagePipeline" -DefinitionFile ".\RunSSISPackagePipeline.json"
    ```

### <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

1. Ardışık hattın çıkış tablosu olan çıktı veri kümesi**'nin tüm dilimleri hakkında ayrıntılı bilgi almak için **Get-AzDataFactorySlice'ı** çalıştırın.

    ```powershell
    Get-AzDataFactorySlice $df -DatasetName sprocsampleout -StartDateTime 2017-10-01T00:00:00Z
    ```
    Burada belirttiğiniz StartDateTime ile JSON işlem hattında belirtilen başlangıç zamanıyla aynı olmasına özen gösterin. 
1. Belirli bir dilim için etkinlik çalışır ayrıntılarını almak için **Get-AzDataFactoryRun** çalıştırın.

    ```powershell
    Get-AzDataFactoryRun $df -DatasetName sprocsampleout -StartDateTime 2017-10-01T00:00:00Z
    ```

    Dilimi **Hazır** durumunda veya **Başarısız** durumunda görene kadar bu cmdlet’i çalışır halde tutun. 

    Paketin yürütüldünden sonra doğruladığını doğrulamak için aşağıdaki sorguyu Azure SQL sunucunuzdaki SSISDB veritabanına karşı çalıştırabilirsiniz. 

    ```sql
    select * from catalog.executions
    ```

## <a name="next-steps"></a>Sonraki adımlar
Depolanan yordam etkinliği hakkında ayrıntılı bilgi [için, Depolanan Yordam etkinlik](data-factory-stored-proc-activity.md) makalesine bakın.

