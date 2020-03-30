---
title: SSIS paketini SSIS'i SAklı Yordam Etkinliği ile çalıştırma - Azure
description: Bu makalede, Saklı Yordam Etkinliği'ni kullanarak Bir Azure Veri Fabrikası ardışık hattında bir SQL Server Integration Services (SSIS) paketinin nasıl çalıştırılacak olduğu açıklanmaktadır.
services: data-factory
documentationcenter: ''
author: swinarko
manager: anandsub
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: sawinark
ms.openlocfilehash: 063728c03c689c2eafec889bdee8276772ae685a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75444028"
---
# <a name="run-an-ssis-package-with-the-stored-procedure-activity-in-azure-data-factory"></a>Azure Data Factory'de Saklı Yordam etkinliğiyle bir SSIS paketi çalıştırma
Bu makalede, Bir SSIS paketinin Bir Azure Veri Fabrikası ardışık hattında, Depolanmış Yordam etkinliği kullanılarak nasıl çalıştırılacak olduğu açıklanmaktadır. 

## <a name="prerequisites"></a>Ön koşullar

### <a name="azure-sql-database"></a>Azure SQL Veritabanı 
Bu makaledeki gözden geçirme, SSIS kataloğunu barındıran bir Azure SQL veritabanı kullanır. Azure SQL Veritabanı Yönetilen Örneği de kullanabilirsiniz.

## <a name="create-an-azure-ssis-integration-runtime"></a>Azure SSIS tümleştirme çalışma zamanı oluşturma
Öğreticideki adım adım talimatı takip ederek bir Azure-SSIS tümleştirme çalışma zamanı [oluşturun: SSIS paketlerini dağıtın.](tutorial-create-azure-ssis-runtime-portal.md)

## <a name="data-factory-ui-azure-portal"></a>Veri Fabrikası UI (Azure portalı)
Bu bölümde, SSIS paketini çağıran depolanmış yordam etkinliği içeren bir Veri Fabrikası ardışık birimi oluşturmak için Veri Fabrikası UI'sini kullanırsınız.

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
İlk adım, Azure portalını kullanarak bir veri fabrikası oluşturmaktır. 

1. **Microsoft Edge** veya **Google Chrome** web tarayıcısını açın. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir.
2. [Azure portalına](https://portal.azure.com) gidin. 
3. Soldaki menüde **Yeni**, **Veri + Analiz** ve **Data Factory** öğesine tıklayın. 
   
   ![Yeni->DataFactory](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-azure-data-factory-menu.png)
2. **Yeni veri fabrikası** sayfasında **ad** için **ADFTutorialDataFactory** adını girin. 
      
     ![Yeni veri fabrikası sayfası](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-azure-data-factory.png)
 
   Azure veri fabrikasının adı **genel olarak benzersiz**olmalıdır. Ad alanı için aşağıdaki hatayı görürseniz veri fabrikasının adını değiştirin (örneğin, adınızADFTutorialDataFactory). Data Factory yapıtlarını adlandırma kuralları için [Data Factory - Adlandırma Kuralları](naming-rules.md) makalesine bakın.
  
     ![Ad yok - hata](./media/how-to-invoke-ssis-package-stored-procedure-activity/name-not-available-error.png)
3. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin. 
4. Kaynak **Grubu**için aşağıdaki adımlardan birini yapın:
     
   - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
   - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
     Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/management/overview.md).  
4. **Sürüm** için **V2**'yi seçin.
5. Data factory için **konum** seçin. Açılan listede yalnızca Data Factory tarafından desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan veri depoları (Azure Depolama, Azure SQL Veritabanı, vb.) ve işlemler (HDInsight, vb.) başka konumlarda olabilir.
6. **Panoya sabitle**’yi seçin.     
7. **Oluştur'u**tıklatın.
8. Panoda şu kutucuğu ve üzerinde şu durumu görürsünüz: **Veri fabrikası dağıtılıyor**. 

     ![veri fabrikası dağıtılıyor kutucuğu](media//how-to-invoke-ssis-package-stored-procedure-activity/deploying-data-factory.png)
9. Oluşturma işlemi tamamlandıktan sonra, resimde gösterildiği gibi **Data Factory** sayfasını görürsünüz.
   
     ![Data factory giriş sayfası](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-home-page.png)
10. Azure Data Factory kullanıcı arabirimi (UI) uygulamasını ayrı bir sekmede açmak için **Yazar ve İzleyici** kutucuğuna tıklayın. 

### <a name="create-a-pipeline-with-stored-procedure-activity"></a>Depolanan yordam etkinliği içeren bir ardışık işlem hattı oluşturma
Bu adımda, bir ardışık hat lar oluşturmak için Veri Fabrikası UI'sini kullanırsınız. Ardışık sp_executesql yordamı kullanarak ssis paketini çalıştıracak şekilde yapılandırırsınız. 

1. Başlat sayfasında, kaynak **oluştur'u**tıklatın: 

    ![Başlarken sayfası](./media/how-to-invoke-ssis-package-stored-procedure-activity/get-started-page.png)
2. **Etkinlikler** araç kutusunda, **Genel'i**genişletin ve **Depolanan Yordam** etkinliğini boru hattı tasarımcısı yüzeyine sürükleyin. 

    ![Sürükle ve bırak depolanan yordam etkinliği](./media/how-to-invoke-ssis-package-stored-procedure-activity/drag-drop-sproc-activity.png)
3. Depolanan yordam etkinliği için özellikler penceresinde, **SQL Hesabı** sekmesine geçin ve **+ Yeni'yi**tıklatın. SSIS Kataloğu'nu (SSIDB veritabanı) barındıran Azure SQL veritabanına bağlantı oluşturursunuz. 
   
    ![Yeni bağlı hizmet düğmesi](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-linked-service-button.png)
4. **Yeni Bağlı Hizmet** penceresinde aşağıdaki adımları izleyin: 

    1. Tür için Azure SQL **Veritabanı'nı**seçin. **Azure SQL Database**
    2. Veritabanını **Default** barındıran Azure SQL Veritabanı'na bağlanmak için `SSISDB` Varsayılan Azure Tümleştirme Çalışma Zamanı'nı seçin.
    3. **Sunucu adı** alanı için SSISDB veritabanını barındıran Azure SQL Veritabanı'nı seçin.
    4. **Veritabanı adı**için **SSISDB'yi** seçin.
    5. **Kullanıcı adı**için, veritabanına erişimi olan kullanıcının adını girin.
    6. **Şifre**için, kullanıcının parolasını girin. 
    7. **Bağlantı Testi** düğmesini tıklatarak veritabanına bağlantıyı test edin.
    8. Kaydet düğmesini tıklatarak bağlantılı hizmeti **kaydedin.** 

        ![Azure SQL Veritabanı bağlı hizmeti](./media/how-to-invoke-ssis-package-stored-procedure-activity/azure-sql-database-linked-service-settings.png)
5. Özellikler penceresinde, **SQL Hesabı** sekmesinden **Saklı Yordam** sekmesine geçin ve aşağıdaki adımları yapın: 

    1. **Edit'i**seçin. 
    2. **Depolanan yordam adı** alanı `sp_executesql`için girin. 
    3. **Depolanan yordam parametreleri** bölümünde **+ Yeni'yi** tıklatın. 
    4. Parametrenin **adı** için **stmt**girin. 
    5. Parametre **türü** için **String'i**girin. 
    6. Parametrenin **değeri** için aşağıdaki SQL sorgusunu girin:

        SQL sorgusunda, **folder_name,** **project_name**ve **package_name** parametreleri için doğru değerleri belirtin. 

        ```sql
        DECLARE @return_value INT, @exe_id BIGINT, @err_msg NVARCHAR(150)    EXEC @return_value=[SSISDB].[catalog].[create_execution] @folder_name=N'<FOLDER name in SSIS Catalog>', @project_name=N'<PROJECT name in SSIS Catalog>', @package_name=N'<PACKAGE name>.dtsx', @use32bitruntime=0, @runinscaleout=1, @useanyworker=1, @execution_id=@exe_id OUTPUT    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1    EXEC [SSISDB].[catalog].[start_execution] @execution_id=@exe_id, @retry_count=0    IF(SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id=@exe_id)<>7 BEGIN SET @err_msg=N'Your package execution did not succeed for execution ID: ' + CAST(@exe_id AS NVARCHAR(20)) RAISERROR(@err_msg,15,1) END
        ```

        ![Azure SQL Veritabanı bağlı hizmeti](./media/how-to-invoke-ssis-package-stored-procedure-activity/stored-procedure-settings.png)
6. Boru hattı yapılandırmasını doğrulamak için araç çubuğunda **Doğrula'yı** tıklatın. **İşlem Hattı Doğrulama Raporu**'nu kapatmak için **>>** seçeneğine tıklayın.

    ![İşlem hattını doğrulama](./media/how-to-invoke-ssis-package-stored-procedure-activity/validate-pipeline.png)
7. **Tümlerini Yayımla** düğmesini tıklatarak veri hattını Veri Fabrikası'na yayımlayın. 

    ![Yayımlama](./media/how-to-invoke-ssis-package-stored-procedure-activity/publish-all-button.png)    

### <a name="run-and-monitor-the-pipeline"></a>Boru hattını çalıştırın ve izleyin
Bu bölümde, bir ardışık işlem yürütmetetik ve sonra onu izleyin. 

1. Bir ardışık hatlar **Trigger** çalışmasını tetiklemek için araç çubuğunda Tetikle'yi tıklatın ve **şimdi Tetikle'yi**tıklatın. 

    ![Şimdi tetikle](media/how-to-invoke-ssis-package-stored-procedure-activity/trigger-now.png)

2. **İşlem Hattı Çalıştırma** penceresinde **Son**’u seçin. 
3. Soldaki **İzleyici** sekmesine geçin. Ardışık hatlar ve durumunu diğer bilgilerle birlikte (Çalıştır Başlangıç saati gibi) görürsünüz. Görünümü yenilemek için **Yenile**’ye tıklayın.

    ![İşlem hattı çalıştırmaları](./media/how-to-invoke-ssis-package-stored-procedure-activity/pipeline-runs.png)

3. **Eylemler** sütunundaki **Etkinlik Çalıştırmalarını Görüntüle** bağlantısına tıklayın. Yalnızca bir etkinlik, ardışık alan yalnızca bir etkinlik (depolanan yordam etkinliği) olduğu için çalışır bakın.

    ![Etkinlik çalıştırmaları](./media/how-to-invoke-ssis-package-stored-procedure-activity/activity-runs.png)

4. Paketin yürütüldünden sonra doğruladığını doğrulamak için aşağıdaki **sorguyu** Azure SQL sunucunuzdaki SSISDB veritabanına karşı çalıştırabilirsiniz. 

    ```sql
    select * from catalog.executions
    ```

    ![Paket yürütmelerini doğrulama](./media/how-to-invoke-ssis-package-stored-procedure-activity/verify-package-executions.png)


> [!NOTE]
> Ayrıca, ardışık hattın bir zamanlamaya göre (saatlik, günlük vb.) çalışır, böylece ardışık hattınız için zamanlanmış bir tetikleyici de oluşturabilirsiniz. Örneğin, [bkz.](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule)

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu bölümde, SSIS paketini çağıran depolanmış yordam etkinliği içeren bir Veri Fabrikası ardışık hattı oluşturmak için Azure PowerShell'i kullanırsınız. 

[Azure PowerShell'in nasıl yüklenir ve yapılandırılabilen](/powershell/azure/install-az-ps)yönergeleri izleyerek en son Azure PowerShell modüllerini yükleyin. 

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Azure-SSIS IR'si olan aynı veri fabrikasını kullanabilir veya ayrı bir veri fabrikası oluşturabilirsiniz. Aşağıdaki yordam, bir veri fabrikası oluşturmak için adımlar sağlar. Bu veri fabrikasında depolanmış yordam etkinliği içeren bir ardışık hatlar oluşturursunuz. Depolanan yordam etkinliği, SSIS paketinizi çalıştırmak için SSISDB veritabanında depolanmış bir yordam yürütür. 

1. Daha sonra PowerShell komutlarında kullanacağınız kaynak grubu adı için bir değişken tanımlayın. Aşağıdaki komut metnini PowerShell'e kopyalayın [Azure kaynak grubu](../azure-resource-manager/management/overview.md) için çift tırnak içinde bir ad belirtin ve ardından komutu çalıştırın. Örneğin: `"adfrg"`. 
   
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

5. Veri fabrikasını oluşturmak için, $ResGrp değişkenindeki Konum ve ResourceGroupName özelliğini kullanarak aşağıdaki **Set-AzDataFactoryV2** cmdlet'i çalıştırın: 
    
    ```powershell       
    $DataFactory = Set-AzDataFactoryV2 -ResourceGroupName $ResGrp.ResourceGroupName -Location $ResGrp.Location -Name $dataFactoryName 
    ```

Aşağıdaki noktalara dikkat edin:

* Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Aşağıdaki hata iletisini alırsanız adı değiştirip yeniden deneyin.

    ```
    The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
    ```
* Data Factory örnekleri oluşturmak için, Azure'da oturum açarken kullandığınız kullanıcı hesabı **katkıda bulunan** veya **sahip** rollerinin üyesi ya da bir Azure aboneliğinin **yöneticisi** olmalıdır.
* Data Factory'nin kullanılabileceği Azure bölgelerinin bir listesi için bir sonraki sayfada ilgilendiğiniz bölgeleri seçin ve **Analytics**'i genişleterek **Data Factory**: [Products available by region](https://azure.microsoft.com/global-infrastructure/services/) (Bölgeye göre kullanılabilir durumdaki ürünler) bölümünü bulun. Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı vb.) ve işlemler (HDInsight vb.) başka bölgelerde olabilir.

### <a name="create-an-azure-sql-database-linked-service"></a>Azure SQL Veritabanı bağlı hizmeti oluşturma
SSIS kataloğunu veri fabrikanıza bağlayan Azure SQL veritabanınızı bağlamak için bağlantılı bir hizmet oluşturun. Veri Fabrikası, SSISDB veritabanına bağlanmak için bu bağlantılı hizmetteki bilgileri kullanır ve bir SSIS paketini çalıştırmak için depolanmış bir yordam yürütür. 

1. **C:\ADF\RunSSISPackage** klasöründe aşağıdaki içeriği içeren **AzureSqlDatabaseLinkedService.json** adlı bir JSON dosyası oluşturun: 

    > [!IMPORTANT]
    > Dosyayı kaydetmeden&gt;önce &lt;sunucu&gt; adını, &lt;&gt; &lt;kullanıcı adını ve parolayı Azure SQL Veritabanınızın değerleriyle değiştirin.

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

3. Bağlı hizmeti oluşturmak için **Set-AzDataFactoryV2LinkedService** cmdlet'i çalıştırın: **AzureSqlDatabaseLinkedService**. 

    ```powershell
    Set-AzDataFactoryV2LinkedService -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "AzureSqlDatabaseLinkedService" -File ".\AzureSqlDatabaseLinkedService.json"
    ```

### <a name="create-a-pipeline-with-stored-procedure-activity"></a>Depolanan yordam etkinliği içeren bir ardışık işlem hattı oluşturma 
Bu adımda, depolanmış yordam etkinliği olan bir ardışık işlem hattı oluşturursunuz. Etkinlik, SSIS paketinizi çalıştırmak için depolanan sp_executesql yordamı çağırır. 

1. **C:\ADF\RunSSISPackage** klasöründe **RunSSISPackagePipeline.json** adlı bir JSON dosyası oluşturun:

    > [!IMPORTANT]
    > Dosyayı kaydetmeden&gt;önce &lt;KLASÖR&gt; ADI, &lt;PROJE ADI, &lt;&gt;PAKET ADI'ni KLASÖR, proje ve paket adlarıyla ssis kataloğunda değiştirin. 

    ```json
    {
        "name": "RunSSISPackagePipeline",
        "properties": {
            "activities": [
                {
                    "name": "My SProc Activity",
                    "description":"Runs an SSIS package",
                    "type": "SqlServerStoredProcedure",
                    "linkedServiceName": {
                        "referenceName": "AzureSqlDatabaseLinkedService",
                        "type": "LinkedServiceReference"
                    },
                    "typeProperties": {
                        "storedProcedureName": "sp_executesql",
                        "storedProcedureParameters": {
                            "stmt": {
                                "value": "DECLARE @return_value INT, @exe_id BIGINT, @err_msg NVARCHAR(150)    EXEC @return_value=[SSISDB].[catalog].[create_execution] @folder_name=N'<FOLDER NAME>', @project_name=N'<PROJECT NAME>', @package_name=N'<PACKAGE NAME>', @use32bitruntime=0, @runinscaleout=1, @useanyworker=1, @execution_id=@exe_id OUTPUT    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1    EXEC [SSISDB].[catalog].[start_execution] @execution_id=@exe_id, @retry_count=0    IF(SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id=@exe_id)<>7 BEGIN SET @err_msg=N'Your package execution did not succeed for execution ID: ' + CAST(@exe_id AS NVARCHAR(20)) RAISERROR(@err_msg,15,1) END"
                            }
                        }
                    }
                }
            ]
        }
    }
    ```

2. Boru hattı oluşturmak için: **RunSSISPackagePipeline**, **Set-AzDataFactoryV2Pipeline** cmdlet çalıştırın.

    ```powershell
    $DFPipeLine = Set-AzDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "RunSSISPackagePipeline" -DefinitionFile ".\RunSSISPackagePipeline.json"
    ```

    Örnek çıktı aşağıdaki gibidir:

    ```
    PipelineName      : Adfv2QuickStartPipeline
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Activities        : {CopyFromBlobToBlob}
    Parameters        : {[inputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification], [outputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification]}
    ```

### <a name="create-a-pipeline-run"></a>İşlem hattı çalıştırması oluşturma
Boru hattını çalıştırmak için **Invoke-AzDataFactoryV2Pipeline** cmdlet'i kullanın. Cmdlet, gelecekte izlemek üzere işlem hattı çalıştırma kimliğini döndürür.

```powershell
$RunId = Invoke-AzDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -PipelineName $DFPipeLine.Name
```

### <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

İşlem hattı çalıştırma durumunu, verileri kopyalama işlemi tamamlanıncaya kadar sürekli olarak denetlemek için aşağıdaki PowerShell betiğini çalıştırın. Aşağıdaki betiği kopyalayıp PowerShell penceresine yapıştırın ve ENTER tuşuna basın. 

```powershell
while ($True) {
    $Run = Get-AzDataFactoryV2PipelineRun -ResourceGroupName $ResGrp.ResourceGroupName -DataFactoryName $DataFactory.DataFactoryName -PipelineRunId $RunId

    if ($Run) {
        if ($run.Status -ne 'InProgress') {
            Write-Output ("Pipeline run finished. The status is: " +  $Run.Status)
            $Run
            break
        }
        Write-Output  "Pipeline is running...status: InProgress"
    }

    Start-Sleep -Seconds 10
}   
```

### <a name="create-a-trigger"></a>Tetikleyici oluşturma
Önceki adımda, ardışık ardışık hattı isteğe bağlı olarak çağırdınız. Ayrıca, ardışık hattı bir zamanlamada çalıştırmak için bir zamanlama tetikleyicisi de oluşturabilirsiniz (saatlik, günlük vb.).

1. **C:\ADF\RunSSISPackage** klasöründe aşağıdaki içeriği içeren **MyTrigger.json** adlı bir JSON dosyası oluşturun: 

    ```json
    {
        "properties": {
            "name": "MyTrigger",
            "type": "ScheduleTrigger",
            "typeProperties": {
                "recurrence": {
                    "frequency": "Hour",
                    "interval": 1,
                    "startTime": "2017-12-07T00:00:00-08:00",
                    "endTime": "2017-12-08T00:00:00-08:00"
                }
            },
            "pipelines": [{
                    "pipelineReference": {
                        "type": "PipelineReference",
                        "referenceName": "RunSSISPackagePipeline"
                    },
                    "parameters": {}
                }
            ]
        }
    }    
    ```
2. **Azure PowerShell'de** **C:\ADF\RunSSISPackage** klasörüne geçin.
3. Tetikleyiciyi oluşturan **Set-AzDataFactoryV2Trigger** cmdlet'i çalıştırın. 

    ```powershell
    Set-AzDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName -DataFactoryName $DataFactory.DataFactoryName -Name "MyTrigger" -DefinitionFile ".\MyTrigger.json"
    ```
4. Varsayılan olarak, tetikleyici durduruldu durumundadır. **Start-AzDataFactoryV2Trigger** cmdlet'i çalıştırarak tetiği başlatın. 

    ```powershell
    Start-AzDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName -DataFactoryName $DataFactory.DataFactoryName -Name "MyTrigger" 
    ```
5. Tetikleyicinin **Get-AzDataFactoryV2Trigger** cmdlet'i çalıştırarak başlatıldığından onaylayın. 

    ```powershell
    Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"     
    ```    
6. Sonraki saatten sonra aşağıdaki komutu çalıştırın. Örneğin, geçerli saat 15:25 UTC ise, komutu 14:00 UTC'de çalıştırın. 
    
    ```powershell
    Get-AzDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-06" -TriggerRunStartedBefore "2017-12-09"
    ```

    Paketin yürütüldünden sonra doğruladığını doğrulamak için aşağıdaki sorguyu Azure SQL sunucunuzdaki SSISDB veritabanına karşı çalıştırabilirsiniz. 

    ```sql
    select * from catalog.executions
    ```


## <a name="next-steps"></a>Sonraki adımlar
Azure portalını kullanarak boru hattını da izleyebilirsiniz. Adım adım talimatlar için [bkz.](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline)
