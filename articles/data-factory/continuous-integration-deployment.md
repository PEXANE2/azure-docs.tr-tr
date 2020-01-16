---
title: Azure Data Factory sürekli tümleştirme ve teslim
description: Data Factory işlem hatlarını bir ortamdan (geliştirme, test, üretim) diğerine taşımak için sürekli tümleştirme ve teslimi nasıl kullanacağınızı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
author: djpmsft
ms.author: daperlov
ms.reviewer: maghan
manager: jroth
ms.topic: conceptual
ms.date: 08/14/2019
ms.openlocfilehash: 0e4b2cd208e11f7696e016d3fa4353b38f3060d8
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75977516"
---
# <a name="continuous-integration-and-delivery-in-azure-data-factory"></a>Azure Data Factory sürekli tümleştirme ve teslim

## <a name="overview"></a>Genel Bakış

Sürekli tümleştirme, kod tabanınızda yapılan her değişikliği otomatik olarak ve mümkün olduğunca erken test etme yöntemidir. Sürekli teslim, sürekli tümleştirme sırasında gerçekleşen testi izler ve bir hazırlama veya üretim sistemine değişiklikleri gönderir.

Azure Data Factory, sürekli tümleştirme ve teslim (CI/CD), Data Factory işlem hatlarını bir ortamdan (geliştirme, test, üretim) diğerine taşıma anlamına gelir. CI/CD yapmak için, Azure Resource Manager şablonlarla Data Factory UX tümleştirmesi kullanabilirsiniz.

Data Factory UX ' de **ARM şablonu** açılır menüsünden bir kaynak yöneticisi şablonu oluşturabilirsiniz. **ARM şablonunu dışarı aktar**' ı seçtiğinizde, portal veri fabrikası için Kaynak Yöneticisi şablonu ve tüm bağlantı dizelerinizi ve diğer parametreleri içeren bir yapılandırma dosyası oluşturur. Ardından, her ortam için bir yapılandırma dosyası oluşturursunuz (geliştirme, test, üretim). Ana Kaynak Yöneticisi Şablon dosyası tüm ortamlar için aynı kalır.

Bu özelliğe ve bir tanıtım 'e dokuz dakikalık bir giriş için şu videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Continuous-integration-and-deployment-using-Azure-Data-Factory/player]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="cicd-lifecycle"></a>CI/CD yaşam döngüsü

Aşağıda, Azure Repos git ile yapılandırılmış bir Azure Data Factory 'deki CI/CD yaşam döngüsüne örnek bir genel bakış sunulmaktadır. Git deposunu yapılandırma hakkında daha fazla bilgi için bkz. [Azure Data Factory kaynak denetimi](source-control.md).

1.  Bir geliştirme Veri Fabrikası oluşturulup Azure Repos git ile yapılandırılır. Tüm geliştiricilerin işlem hatları ve veri kümeleri gibi Data Factory kaynaklarını yazmak için izni olmalıdır.

1.  Geliştiriciler Özellik dallarında değişiklik yaparken, işlem hattı çalıştırmalarının en son değişiklikleriyle ilgili hata ayıklaması yapar. İşlem hattı çalıştırmasında hata ayıklama hakkında daha fazla bilgi için, bkz. [Azure Data Factory yinelemeli geliştirme ve hata ayıklama](iterative-development-debugging.md).

1.  Geliştiriciler yaptıkları değişikliklerle karşılandıktan sonra, yaptıkları değişikliklerin eşler tarafından gözden geçirilmesini sağlamak için özellik dalından ana veya işbirliği dalına bir çekme isteği oluşturur.

1.  Bir çekme isteği onaylandıktan ve değişiklikler ana dalda birleştirildikten sonra, değişiklikler geliştirme fabrikasında yayımlanabilir.

1.  Takım değişiklikleri test fabrikasına ve ardından üretim fabrikasına dağıtmaya hazırsa, takım Kaynak Yöneticisi şablonunu ana daldan dışarı aktarır.

1.  Dışarıya aktarılmış Kaynak Yöneticisi şablonu, test fabrikasına ve üretim fabrikasına farklı parametre dosyaları ile dağıtılır.

## <a name="create-a-resource-manager-template-for-each-environment"></a>Her ortam için bir Kaynak Yöneticisi şablonu oluşturma

1. **ARM şablon** listesinde, geliştirme ortamında veri fabrikanızın Kaynak Yöneticisi şablonunu dışarı aktarmak Için **ARM şablonunu dışarı aktar** ' ı seçin.

   ![Kaynak Yöneticisi şablonu dışarı aktarma](media/continuous-integration-deployment/continuous-integration-image1.png)

1. Test ve üretim verileri fabrikalarınız içinde **ARM şablonunu Içeri aktar**' ı seçin. Bu eylem, dışarı aktarılan şablonu içeri aktarabileceğiniz Azure portal sizi yönlendirir. Kaynak Yöneticisi Şablon düzenleyicisini açmak için **düzenleyicide kendi şablonunuzu oluşturun öğesini** seçin.

   ![Kendi şablonunuzu oluşturun](media/continuous-integration-deployment/custom-deployment-build-your-own-template.png) 

1. **Dosya Yükle**' yi seçin ve ardından oluşturulan kaynak yöneticisi şablonunu seçin.

   ![Şablonu düzenle](media/continuous-integration-deployment/custom-deployment-edit-template.png)

1. Ayarlar bölümünde, bağlı hizmet kimlik bilgileri gibi yapılandırma değerlerini girin. İşiniz bittiğinde Kaynak Yöneticisi şablonunu dağıtmak için **satın al** ' ı seçin.

   ![Ayarlar bölümü](media/continuous-integration-deployment/continuous-integration-image5.png)

### <a name="connection-strings"></a>Bağlantı dizeleri

Bağlantı dizelerini yapılandırma hakkında daha fazla bilgi için bağlayıcının makalesine bakın. Örneğin, Azure SQL veritabanı için, bkz. [Azure Data Factory kullanarak Azure SQL veritabanına veri kopyalama](connector-azure-sql-database.md). Bir bağlantı dizesini doğrulamak için Data Factory UX içindeki kaynak için kod görünümünü açabilirsiniz. Kod görünümünde, bağlantı dizesinin parola veya hesap anahtarı kısmı kaldırılır. Kod görünümünü açmak için, burada vurgulanan simgeyi seçin:

![Bağlantı dizesini görmek için kod görünümünü açın](media/continuous-integration-deployment/continuous-integration-codeview.png)

## <a name="automate-continuous-integration-by-using-azure-pipelines-releases"></a>Azure Pipelines sürümlerini kullanarak sürekli tümleştirmeyi otomatikleştirin

Aşağıda, bir veri fabrikasının birden çok ortama dağıtımını otomatikleştiren Azure Pipelines bir sürümü ayarlamaya yönelik bir kılavuz verilmiştir.

![Azure Pipelines ile sürekli tümleştirme diyagramı](media/continuous-integration-deployment/continuous-integration-image12.png)

### <a name="requirements"></a>Gereksinimler

-    [Azure Resource Manager hizmeti uç noktasını](https://docs.microsoft.com/azure/devops/pipelines/library/service-endpoints#sep-azure-rm)kullanan Visual Studio Team Foundation Server veya Azure Repos bağlantılı bir Azure aboneliği.

-   Azure Repos git tümleştirmesi ile yapılandırılmış bir veri fabrikası.

-   Her ortam için gizli dizileri içeren bir [Azure Anahtar Kasası](https://azure.microsoft.com/services/key-vault/) .

### <a name="set-up-an-azure-pipelines-release"></a>Azure Pipelines yayını ayarlama

1.  [Azure DevOps](https://dev.azure.com/)'da, Data Factory 'niz ile yapılandırılmış projeyi açın.

1.  Sayfanın sol tarafında, işlem **hatları**' nı seçin ve ardından **yayınlar**' ı seçin.

    ![İşlem hatları, yayınlar seçin](media/continuous-integration-deployment/continuous-integration-image6.png)

1.  **Yeni işlem hattı**' nı seçin veya mevcut işlem hatlarınız varsa **Yeni** ' yi ve ardından **Yeni yayın**işlem hattını seçin.

1.  **Boş iş** şablonunu seçin.

    ![Boş işi seçin](media/continuous-integration-deployment/continuous-integration-image13.png)

1.  **Aşama adı** kutusuna ortamınızın adını girin.

1.  **Yapıt Ekle**' yi seçin ve ardından veri fabrikayla yapılandırılmış depoyu seçin. **Varsayılan dal**için **adf_publish** seçin. **Varsayılan sürüm**için **varsayılan daldan en son**' u seçin.

    ![Yapıt ekle](media/continuous-integration-deployment/continuous-integration-image7.png)

1.  Azure Resource Manager Dağıtım görevi ekleyin:

    a.  Aşama görünümünde, **aşama görevlerini görüntüle**' yi seçin.

    ![Aşama görünümü](media/continuous-integration-deployment/continuous-integration-image14.png)

    b.  Yeni bir görev oluşturun. **Azure Kaynak grubu dağıtımı**' nı arayın ve ardından **Ekle**' yi seçin.

    c.  Dağıtım görevinde, hedef veri fabrikası için abonelik, kaynak grubu ve konum ' u seçin. Gerekirse kimlik bilgilerini sağlayın.

    d.  **Eylem** listesinde, **kaynak grubunu oluştur veya Güncelleştir**' i seçin.

    e.  **Şablon** kutusunun yanındaki üç nokta düğmesini ( **...** ) seçin. Bu makalenin [her bir ortam için Kaynak Yöneticisi şablonu oluşturma](continuous-integration-deployment.md#create-a-resource-manager-template-for-each-environment) bölümünde **ARM şablonunu içeri aktar** ' a tıklayarak oluşturduğunuz Azure Resource Manager şablonuna gözatamazsınız. Bu dosyayı adf_publish dalının <FactoryName> klasöründe arayın.

    f.  Seç **...** **şablon parametreleri** kutusunun yanındaki parametreler dosyasını seçin. Seçtiğiniz dosya, bir kopya oluşturup oluşturdığınıza veya ARMTemplateParametersForFactory. JSON adlı varsayılan dosyayı kullanmaktan bağımsız olarak değişir.

    g.  Seç **...** **şablon parametrelerini geçersiz kıl** kutusunun yanında, hedef veri fabrikasının bilgilerini girin. Azure Key Vault gelen kimlik bilgileri için, çift tırnak işaretleri arasında gizli dizi adını girin. Örneğin, gizli dizinin adı cred1 ise, bu değer için **"$ (cred1)"** girin.

    h. **Dağıtım modu**için **artımlı** ' ı seçin.

    > [!WARNING]
    > **Dağıtım modu**için **Tamam** ' ı seçerseniz, hedef kaynak grubundaki tüm kaynaklar da dahil olmak üzere, Kaynak Yöneticisi şablonunda tanımlanmamış olan kaynaklar silinebilir.

    ![Data Factory üretim dağıtımı](media/continuous-integration-deployment/continuous-integration-image9.png)

1.  Yayın ardışık düzenini kaydedin.

1. Bir yayını tetiklemek için **yayın oluştur**' u seçin.

   ![Yayın oluştur ' u seçin](media/continuous-integration-deployment/continuous-integration-image10.png)

### <a name="get-secrets-from-azure-key-vault"></a>Azure Key Vault parolaları al

Azure Resource Manager şablonunda geçiş yapmak için gizli dizileri varsa, Azure Pipelines sürümü ile Azure Key Vault kullanmanızı öneririz.

Gizli dizileri ele almanın iki yolu vardır:

1.  Gizli dizileri parametreler dosyasına ekleyin. Daha fazla bilgi için bkz. [dağıtım sırasında güvenli parametre değeri geçirmek için Azure Key Vault kullanma](../azure-resource-manager/templates/key-vault-parameter.md).

    Yayımla dalına yüklenen parametreler dosyasının bir kopyasını oluşturun. Key Vault almak istediğiniz parametrelerin değerlerini şu biçimi kullanarak ayarlayın:

    ```json
    {
        "parameters": {
            "azureSqlReportingDbPassword": {
                "reference": {
                    "keyVault": {
                        "id": "/subscriptions/<subId>/resourceGroups/<resourcegroupId> /providers/Microsoft.KeyVault/vaults/<vault-name> "
                    },
                    "secretName": " < secret - name > "
                }
            }
        }
    }
    ```

    Bu yöntemi kullandığınızda, gizli anahtar, anahtar kasasından otomatik olarak çekilir.

    Parametre dosyasının da yayımlama dalında olması gerekir.

-  Önceki bölümde açıklanan Azure Resource Manager dağıtım görevinin önüne bir [Azure Key Vault görevi](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-key-vault) ekleyin:

    1.  **Görevler** sekmesinde yeni bir görev oluşturun. **Azure Key Vault** arayın ve ekleyin.

    1.  Key Vault görevinde, anahtar kasasını oluşturduğunuz aboneliği seçin. Gerekirse kimlik bilgilerini sağlayın ve ardından anahtar kasasını seçin.

    ![Key Vault görevi ekleme](media/continuous-integration-deployment/continuous-integration-image8.png)

   #### <a name="grant-permissions-to-the-azure-pipelines-agent"></a>Azure Pipelines aracısına izin verme

   Doğru izinler ayarlanmamışsa Azure Key Vault görev erişim reddedildi hatasıyla başarısız olabilir. Yayın için günlükleri indirin ve Azure Pipelines aracısına izin vermek için komutunu içeren. ps1 dosyasını bulun. Komutu doğrudan çalıştırabilirsiniz. Ya da asıl KIMLIĞI dosyadan kopyalayabilir ve Azure portal erişim ilkesini el ile ekleyebilirsiniz. `Get` ve `List` gereken en düşük izinlerdir.

### <a name="update-active-triggers"></a>Etkin Tetikleyicileri Güncelleştir

Etkin Tetikleyicileri güncelleştirmeye çalışırsanız dağıtım başarısız olabilir. Etkin Tetikleyicileri güncelleştirmek için, bunları el ile durdurmanız ve dağıtımdan sonra yeniden başlatmanız gerekir. Bunu bir Azure PowerShell görevi kullanarak yapabilirsiniz:

1.  Sürümün **Görevler** sekmesinde **Azure PowerShell** bir görev ekleyin.

1.  Bağlantı türü olarak **Azure Resource Manager** ' yi seçin ve ardından aboneliğinizi seçin.

1.  Komut dosyası türü olarak **satır Içi betik** ' ı seçin ve kodunuzu girin. Aşağıdaki kod Tetikleyicileri durduruyor:

    ```powershell
    $triggersADF = Get-AzDataFactoryV2Trigger -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName

    $triggersADF | ForEach-Object { Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_.name -Force }
    ```

    ![Azure PowerShell görev](media/continuous-integration-deployment/continuous-integration-image11.png)

Dağıtımdan sonra Tetikleyicileri yeniden başlatmak için benzer adımları (`Start-AzDataFactoryV2Trigger` işleviyle birlikte) tamamlayabilirsiniz.

> [!IMPORTANT]
> CI/CD senaryolarında, farklı ortamlardaki Integration Runtime (IR) türü aynı olmalıdır. Örneğin, geliştirme ortamında şirket içinde barındırılan bir IR varsa, aynı IR, test ve üretim gibi diğer ortamlarda da kendi kendine barındırılan türde olmalıdır. Benzer şekilde, tümleştirme çalışma zamanlarını birden çok aşamada paylaşıyorsanız, tümleştirme çalışma zamanlarını, geliştirme, test ve üretim gibi tüm ortamlarda bağlanmış bir şekilde yapılandırmanız gerekir.

#### <a name="sample-pre--and-post-deployment-script"></a>Örnek ön ve dağıtım sonrası betiği

Aşağıdaki örnek betik, dağıtımdan önce Tetikleyicileri durdurmayı ve daha sonra yeniden başlatmayı gösterir. Betik Ayrıca kaldırılan kaynakları silmek için kod içerir. Azure PowerShell 'ın en son sürümünü yüklemek için bkz. [PowerShellGet Ile Windows 'a Azure PowerShell yüklemesi](https://docs.microsoft.com/powershell/azure/install-az-ps).

```powershell
param
(
    [parameter(Mandatory = $false)] [String] $rootFolder,
    [parameter(Mandatory = $false)] [String] $armTemplate,
    [parameter(Mandatory = $false)] [String] $ResourceGroupName,
    [parameter(Mandatory = $false)] [String] $DataFactoryName,
    [parameter(Mandatory = $false)] [Bool] $predeployment=$true,
    [parameter(Mandatory = $false)] [Bool] $deleteDeployment=$false
)

$templateJson = Get-Content $armTemplate | ConvertFrom-Json
$resources = $templateJson.resources

#Triggers 
Write-Host "Getting triggers"
$triggersADF = Get-AzDataFactoryV2Trigger -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
$triggersTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/triggers" }
$triggerNames = $triggersTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
$activeTriggerNames = $triggersTemplate | Where-Object { $_.properties.runtimeState -eq "Started" -and ($_.properties.pipelines.Count -gt 0 -or $_.properties.pipeline.pipelineReference -ne $null)} | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
$deletedtriggers = $triggersADF | Where-Object { $triggerNames -notcontains $_.Name }
$triggerstostop = $triggerNames | where { ($triggersADF | Select-Object name).name -contains $_ }

if ($predeployment -eq $true) {
    #Stop all triggers
    Write-Host "Stopping deployed triggers"
    $triggerstostop | ForEach-Object { 
        Write-host "Disabling trigger " $_
        Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_ -Force 
    }
}
else {
    #Deleted resources
    #pipelines
    Write-Host "Getting pipelines"
    $pipelinesADF = Get-AzDataFactoryV2Pipeline -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $pipelinesTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/pipelines" }
    $pipelinesNames = $pipelinesTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
    $deletedpipelines = $pipelinesADF | Where-Object { $pipelinesNames -notcontains $_.Name }
    #datasets
    Write-Host "Getting datasets"
    $datasetsADF = Get-AzDataFactoryV2Dataset -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $datasetsTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/datasets" }
    $datasetsNames = $datasetsTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40) }
    $deleteddataset = $datasetsADF | Where-Object { $datasetsNames -notcontains $_.Name }
    #linkedservices
    Write-Host "Getting linked services"
    $linkedservicesADF = Get-AzDataFactoryV2LinkedService -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $linkedservicesTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/linkedservices" }
    $linkedservicesNames = $linkedservicesTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
    $deletedlinkedservices = $linkedservicesADF | Where-Object { $linkedservicesNames -notcontains $_.Name }
    #Integrationruntimes
    Write-Host "Getting integration runtimes"
    $integrationruntimesADF = Get-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $integrationruntimesTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/integrationruntimes" }
    $integrationruntimesNames = $integrationruntimesTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
    $deletedintegrationruntimes = $integrationruntimesADF | Where-Object { $integrationruntimesNames -notcontains $_.Name }

    #Delete resources
    Write-Host "Deleting triggers"
    $deletedtriggers | ForEach-Object { 
        Write-Host "Deleting trigger "  $_.Name
        $trig = Get-AzDataFactoryV2Trigger -name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName
        if ($trig.RuntimeState -eq "Started") {
            Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_.Name -Force 
        }
        Remove-AzDataFactoryV2Trigger -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting pipelines"
    $deletedpipelines | ForEach-Object { 
        Write-Host "Deleting pipeline " $_.Name
        Remove-AzDataFactoryV2Pipeline -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting datasets"
    $deleteddataset | ForEach-Object { 
        Write-Host "Deleting dataset " $_.Name
        Remove-AzDataFactoryV2Dataset -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting linked services"
    $deletedlinkedservices | ForEach-Object { 
        Write-Host "Deleting Linked Service " $_.Name
        Remove-AzDataFactoryV2LinkedService -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting integration runtimes"
    $deletedintegrationruntimes | ForEach-Object { 
        Write-Host "Deleting integration runtime " $_.Name
        Remove-AzDataFactoryV2IntegrationRuntime -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }

    if ($deleteDeployment -eq $true) {
        Write-Host "Deleting ARM deployment ... under resource group: " $ResourceGroupName
        $deployments = Get-AzResourceGroupDeployment -ResourceGroupName $ResourceGroupName
        $deploymentsToConsider = $deployments | Where { $_.DeploymentName -like "ArmTemplate_master*" -or $_.DeploymentName -like "ArmTemplateForFactory*" } | Sort-Object -Property Timestamp -Descending
        $deploymentName = $deploymentsToConsider[0].DeploymentName

       Write-Host "Deployment to be deleted: " $deploymentName
        $deploymentOperations = Get-AzResourceGroupDeploymentOperation -DeploymentName $deploymentName -ResourceGroupName $ResourceGroupName
        $deploymentsToDelete = $deploymentOperations | Where { $_.properties.targetResource.id -like "*Microsoft.Resources/deployments*" }

        $deploymentsToDelete | ForEach-Object { 
            Write-host "Deleting inner deployment: " $_.properties.targetResource.id
            Remove-AzResourceGroupDeployment -Id $_.properties.targetResource.id
        }
        Write-Host "Deleting deployment: " $deploymentName
        Remove-AzResourceGroupDeployment -ResourceGroupName $ResourceGroupName -Name $deploymentName
    }

    #Start active triggers - after cleanup efforts
    Write-Host "Starting active triggers"
    $activeTriggerNames | ForEach-Object { 
        Write-host "Enabling trigger " $_
        Start-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_ -Force 
    }
}
```

## <a name="use-custom-parameters-with-the-resource-manager-template"></a>Kaynak Yöneticisi şablonuyla özel parametreleri kullanma

GIT modundaysanız, şablonda parametreli olan özellikleri ve sabit kodlanmış özellikleri ayarlamak için Kaynak Yöneticisi şablonunuzda varsayılan özellikleri geçersiz kılabilirsiniz. Bu senaryolarda varsayılan parameterleştirme şablonunu geçersiz kılmak isteyebilirsiniz:

* Otomatik CI/CD kullanıyorsunuz ve Kaynak Yöneticisi dağıtımı sırasında bazı özellikleri değiştirmek istiyorsunuz, ancak özellikler varsayılan olarak parametreleştirimez.
* Fabrikanızın izin verilen en fazla sayıda parametreye (256) sahip olduğu için varsayılan Kaynak Yöneticisi şablonunun geçersiz olması çok büyük.

Bu koşullar altında, varsayılan parameterleştirme şablonunu geçersiz kılmak için, deponun kök klasöründe ARM-Template-Parameters-Definition. JSON adlı bir dosya oluşturun. Bu tam dosya adını kullanmanız gerekir. Data Factory, bu dosyayı, yalnızca işbirliği dalından değil Azure Data Factory portalında yaptığınız daldan okur. Bir özel daldan dosya oluşturabilir veya düzenleyebilirsiniz, burada, Kullanıcı arabiriminde **ARM şablonunu dışarı aktar** ' ı seçerek yaptığınız değişiklikleri test edebilirsiniz. Daha sonra dosyayı işbirliği dalında birleştirebilirsiniz. Dosya bulunamazsa, varsayılan şablon kullanılır.

### <a name="syntax-of-a-custom-parameters-file"></a>Özel parametre dosyasının sözdizimi

Özel parametreler dosyasını oluştururken izlenecek bazı yönergeler aşağıda verilmiştir. Dosya her varlık türü için bir bölümden oluşur: tetikleyici, işlem hattı, bağlı hizmet, veri kümesi, tümleştirme çalışma zamanı vb.
* İlgili varlık türünün altında özellik yolunu girin.
* Bir özellik adının `*` olarak ayarlanması, altındaki tüm özellikleri parametreleştirmek istediğinizi gösterir (özyinelemeli değil, yalnızca ilk düzeye kadar). Bu yapılandırmaya özel durumlar da sağlayabilirsiniz.
* Bir özelliğin değerini dize olarak ayarlamak, özelliği parametreleştirmek istediğinizi gösterir.  `<action>:<name>:<stype>`biçimini kullanın.
   *  `<action>` şu karakterlerden biri olabilir:
      * `=` , geçerli değeri parametresi için varsayılan değer olarak tutacağı anlamına gelir.
      * `-` , parametresi için varsayılan değeri saklama anlamına gelir.
      * `|` , bağlantı dizeleri veya anahtarlar için Azure Key Vault parolalar için özel bir durumdur.
   * `<name>` , parametrenin adıdır. Boşsa, özelliğin adını alır. Değer bir `-` karakteriyle başlıyorsa, ad kısaltılmıştır. Örneğin, `AzureStorage1_properties_typeProperties_connectionString` `AzureStorage1_connectionString`kısaltıldı.
   * `<stype>` parametrenin türüdür.  `<stype>` boş ise, varsayılan tür `string`olur. Desteklenen değerler: `string`, `bool`, `number`, `object`ve `securestring`.
* Tanım dosyasında bir dizi belirtilmesi, şablondaki eşleşen özelliğin bir dizi olduğunu gösterir. Data Factory, dizinin tümleştirme çalışma zamanı nesnesinde belirtilen tanımı kullanarak dizideki tüm nesneler arasında yinelenir. İkinci nesne, bir dize, her yineleme için parametresinin adı olarak kullanılan özelliğin adı olur.
* Bir tanım, kaynak örneğine özgü olamaz. Herhangi bir tanım, bu türdeki tüm kaynaklar için geçerlidir.
* Varsayılan olarak, Key Vault gizli dizileri ve bağlantı dizeleri, anahtarlar ve belirteçler gibi güvenli dizeler gibi tüm güvenli dizeler parametrelenir.
 
### <a name="sample-parameterization-template"></a>Örnek Parametreleştirme şablonu

Parametreleştirme şablonunun nasıl görünebileceğini aşağıda görebilirsiniz:

```json
{
    "Microsoft.DataFactory/factories/pipelines": {
        "properties": {
            "activities": [{
                "typeProperties": {
                    "waitTimeInSeconds": "-::number",
                    "headers": "=::object"
                }
            }]
        }
    },
    "Microsoft.DataFactory/factories/integrationRuntimes": {
        "properties": {
            "typeProperties": {
                "*": "="
            }
        }
    },
    "Microsoft.DataFactory/factories/triggers": {
        "properties": {
            "typeProperties": {
                "recurrence": {
                    "*": "=",
                    "interval": "=:triggerSuffix:number",
                    "frequency": "=:-freq"
                },
                "maxConcurrency": "="
            }
        }
    },
    "Microsoft.DataFactory/factories/linkedServices": {
        "*": {
            "properties": {
                "typeProperties": {
                    "accountName": "=",
                    "username": "=",
                    "connectionString": "|:-connectionString:secureString",
                    "secretAccessKey": "|"
                }
            }
        },
        "AzureDataLakeStore": {
            "properties": {
                "typeProperties": {
                    "dataLakeStoreUri": "="
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/datasets": {
        "properties": {
            "typeProperties": {
                "*": "="
            }
        }
    }
}
```
Yukarıdaki şablonun nasıl oluşturulduğu ve kaynak türüne göre nasıl bölündüğü hakkında bir açıklama aşağıda verilmiştir.

#### <a name="pipelines"></a>İşlem hatları
    
* Yol `activities/typeProperties/waitTimeInSeconds` herhangi bir özellik parametrelenir. `waitTimeInSeconds` adlı bir işlem hattındaki herhangi bir etkinlik (örneğin, `Wait` etkinliği), varsayılan bir ada sahip bir sayı olarak parametrelenir. Ancak Kaynak Yöneticisi şablonunda varsayılan bir değere sahip olmaz. Kaynak Yöneticisi dağıtımı sırasında zorunlu bir giriş olacaktır.
* Benzer şekilde, `headers` adlı bir Özellik (örneğin, bir `Web` etkinliğinde), tür `object` (JObject) ile parametrelenir. Kaynak fabrikasının değeriyle aynı değer olan varsayılan bir değere sahiptir.

#### <a name="integrationruntimes"></a>Tümleştirme çalışma zamanları

* `typeProperties` yolu altındaki tüm özellikler, kendi varsayılan değerleriyle parametreleştirilenir. Örneğin, `IntegrationRuntimes` türü Özellikler altında iki özellik vardır: `computeProperties` ve `ssisProperties`. Her iki özellik türü de ilgili varsayılan değerleri ve türleri (nesne) ile oluşturulur.

#### <a name="triggers"></a>Tetikleyiciler

* `typeProperties`altında iki özellik parametrelenir. Birincisi, varsayılan bir değere sahip ve`string`türünde olan `maxConcurrency`. `<entityName>_properties_typeProperties_maxConcurrency`varsayılan parametre adı vardır.
* `recurrence` özelliği de parametreleştirilenir. Bu düzeyin altında, bu düzeydeki tüm özellikler, varsayılan değerler ve parametre adlarıyla dize olarak parametreleştirime olarak belirtilir. Özel durum, tür `number`olarak parametreleştirilen `interval` özelliğidir. Parametre adı `<entityName>_properties_typeProperties_recurrence_triggerSuffix`ile Sonya düzeltildi. Benzer şekilde, `freq` özelliği bir dizedir ve bir dize olarak parametrelenir. Ancak, `freq` özelliği varsayılan değer olmadan parametrelenir. Ad kısaltılmıştır ve Sonya düzeltildi. Örneğin, `<entityName>_freq`.

#### <a name="linkedservices"></a>LinkedServices

* Bağlı hizmetler benzersizdir. Bağlı hizmetler ve veri kümelerinin çok sayıda türü olduğundan, türe özgü özelleştirme sağlayabilirsiniz. Bu örnekte, `AzureDataLakeStore`türündeki tüm bağlı hizmetler için belirli bir şablon uygulanır. Tüm diğerleri için (`*`aracılığıyla), farklı bir şablon uygulanır.
* `connectionString` özelliği bir `securestring` değeri olarak parametrelendirilecektir. Varsayılan bir değere sahip olmayacaktır. `connectionString`, sonekli olan kısaltılmış bir parametre adına sahip olur.
* Özellik `secretAccessKey` `AzureKeyVaultSecret` (örneğin, bir Amazon S3 bağlantılı hizmetinde) olur. Otomatik olarak Azure Key Vault gizli dizi olarak parametrelenir ve yapılandırılan anahtar kasasından alınır. Ayrıca, anahtar kasasının kendisini parametreleştirebilirsiniz.

#### <a name="datasets"></a>Veri kümeleri

* Veri kümeleri için türe özgü özelleştirme kullanılabilir olsa da, açıkça \*düzeyinde bir yapılandırmaya sahip olmadan yapılandırma sağlayabilirsiniz. Yukarıdaki örnekte, `typeProperties` altındaki tüm veri kümesi özellikleri parametrelenir.

### <a name="default-parameterization-template"></a>Varsayılan parameterleştirme şablonu

Geçerli varsayılan parameterleştirme şablonu aşağıda verilmiştir. Yalnızca birkaç parametre eklemeniz gerekiyorsa, varolan parameterleştirme yapısını kaybetmemeniz nedeniyle bu şablonu doğrudan düzenlemeniz iyi bir fikir olabilir.

```json
{
    "Microsoft.DataFactory/factories/pipelines": {
    },
    "Microsoft.DataFactory/factories/integrationRuntimes":{
        "properties": {
            "typeProperties": {
                "ssisProperties": {
                    "catalogInfo": {
                        "catalogServerEndpoint": "=",
                        "catalogAdminUserName": "=",
                        "catalogAdminPassword": {
                            "value": "-::secureString"
                        }
                    },
                    "customSetupScriptProperties": {
                        "sasToken": {
                            "value": "-::secureString"
                        }
                    }
                },
                "linkedInfo": {
                    "key": {
                        "value": "-::secureString"
                    },
                    "resourceId": "="
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/triggers": {
        "properties": {
            "pipelines": [{
                    "parameters": {
                        "*": "="
                    }
                },  
                "pipelineReference.referenceName"
            ],
            "pipeline": {
                "parameters": {
                    "*": "="
                }
            },
            "typeProperties": {
                "scope": "="
            }

        }
    },
    "Microsoft.DataFactory/factories/linkedServices": {
        "*": {
            "properties": {
                "typeProperties": {
                    "accountName": "=",
                    "username": "=",
                    "userName": "=",
                    "accessKeyId": "=",
                    "servicePrincipalId": "=",
                    "userId": "=",
                    "clientId": "=",
                    "clusterUserName": "=",
                    "clusterSshUserName": "=",
                    "hostSubscriptionId": "=",
                    "clusterResourceGroup": "=",
                    "subscriptionId": "=",
                    "resourceGroupName": "=",
                    "tenant": "=",
                    "dataLakeStoreUri": "=",
                    "baseUrl": "=",
                    "database": "=",
                    "serviceEndpoint": "=",
                    "batchUri": "=",
                    "databaseName": "=",
                    "systemNumber": "=",
                    "server": "=",
                    "url":"=",
                    "aadResourceId": "=",
                    "connectionString": "|:-connectionString:secureString"
                }
            }
        },
        "Odbc": {
            "properties": {
                "typeProperties": {
                    "userName": "=",
                    "connectionString": {
                        "secretName": "="
                    }
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/datasets": {
        "*": {
            "properties": {
                "typeProperties": {
                    "folderPath": "=",
                    "fileName": "="
                }
            }
        }}
}
```

Aşağıdaki örnek, varsayılan parameterleştirme şablonuna tek bir değerin nasıl ekleneceğini gösterir. Yalnızca bir Databricks bağlı hizmeti için mevcut Azure Databricks etkileşimli küme KIMLIĞINI parametreler dosyasına eklemek istiyoruz. Bu dosyanın, `Microsoft.DataFactory/factories/linkedServices`Özellikler alanında `existingClusterId` eklenmesi dışında önceki dosyayla aynı olduğunu unutmayın.

```json
{
    "Microsoft.DataFactory/factories/pipelines": {
    },
    "Microsoft.DataFactory/factories/integrationRuntimes":{
        "properties": {
            "typeProperties": {
                "ssisProperties": {
                    "catalogInfo": {
                        "catalogServerEndpoint": "=",
                        "catalogAdminUserName": "=",
                        "catalogAdminPassword": {
                            "value": "-::secureString"
                        }
                    },
                    "customSetupScriptProperties": {
                        "sasToken": {
                            "value": "-::secureString"
                        }
                    }
                },
                "linkedInfo": {
                    "key": {
                        "value": "-::secureString"
                    },
                    "resourceId": "="
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/triggers": {
        "properties": {
            "pipelines": [{
                    "parameters": {
                        "*": "="
                    }
                },  
                "pipelineReference.referenceName"
            ],
            "pipeline": {
                "parameters": {
                    "*": "="
                }
            },
            "typeProperties": {
                "scope": "="
            }
 
        }
    },
    "Microsoft.DataFactory/factories/linkedServices": {
        "*": {
            "properties": {
                "typeProperties": {
                    "accountName": "=",
                    "username": "=",
                    "userName": "=",
                    "accessKeyId": "=",
                    "servicePrincipalId": "=",
                    "userId": "=",
                    "clientId": "=",
                    "clusterUserName": "=",
                    "clusterSshUserName": "=",
                    "hostSubscriptionId": "=",
                    "clusterResourceGroup": "=",
                    "subscriptionId": "=",
                    "resourceGroupName": "=",
                    "tenant": "=",
                    "dataLakeStoreUri": "=",
                    "baseUrl": "=",
                    "database": "=",
                    "serviceEndpoint": "=",
                    "batchUri": "=",
                    "databaseName": "=",
                    "systemNumber": "=",
                    "server": "=",
                    "url":"=",
                    "aadResourceId": "=",
                    "connectionString": "|:-connectionString:secureString",
                    "existingClusterId": "-"
                }
            }
        },
        "Odbc": {
            "properties": {
                "typeProperties": {
                    "userName": "=",
                    "connectionString": {
                        "secretName": "="
                    }
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/datasets": {
        "*": {
            "properties": {
                "typeProperties": {
                    "folderPath": "=",
                    "fileName": "="
                }
            }
        }}
}
```

## <a name="linked-resource-manager-templates"></a>Bağlı Kaynak Yöneticisi şablonları

Veri fabrikalarınız için CI/CD ayarladıysanız, fabrikanızın daha Büyük büyüdüğü için Azure Resource Manager şablonu sınırlarını aşabilirsiniz. Örneğin, bir sınır Kaynak Yöneticisi şablonundaki en fazla kaynak sayısıdır. Fabrika için tam Kaynak Yöneticisi şablonu oluştururken büyük fabrikalara uyum sağlamak için, Data Factory artık bağlantılı Kaynak Yöneticisi şablonları oluşturuyor. Bu özellikle, tüm fabrika yükü, sınırlara göre sınırlandırılmaması için çeşitli dosyalara bölünür.

Git 'i yapılandırdıysanız, bağlantılı şablonlar oluşturulur ve adf_publish dalındaki tam Kaynak Yöneticisi şablonlarıyla birlikte linkedTemplates adlı yeni bir klasöre kaydedilir:

![Bağlı Kaynak Yöneticisi şablonları klasörü](media/continuous-integration-deployment/linked-resource-manager-templates.png)

Bağlantılı Kaynak Yöneticisi şablonları genellikle ana şablondan ve ana şablon kümesinden oluşur. Üst şablon ArmTemplate_master. JSON olarak adlandırılır ve alt şablonlar, ArmTemplate_0. JSON, ArmTemplate_1. JSON ve benzeri düzeniyle adlandırılır. 

Tam Kaynak Yöneticisi şablonu yerine bağlantılı şablonlar kullanmak için, CI/CD görevinizi ArmTemplateForFactory. JSON (tam Kaynak Yöneticisi şablonu) yerine ArmTemplate_master. json ' a işaret etmek üzere güncelleştirin. Kaynak Yöneticisi ayrıca, Azure 'un dağıtım sırasında erişebilmesi için bağlantılı şablonları bir depolama hesabına yüklemenizi gerektirir. Daha fazla bilgi için bkz. [VSTS ile bağlantılı kaynak yöneticisi şablonlarını dağıtma](https://blogs.msdn.microsoft.com/najib/2018/04/22/deploying-linked-arm-templates-with-vsts/).

Dağıtım görevinden önce ve sonra, CI/CD işlem hattınızda Data Factory betikleri eklemeyi unutmayın.

Git 'in yapılandırılıp yapılandırılmadığını, **ARM şablon** listesindeki **ARM şablonunu dışarı aktar** aracılığıyla bağlı şablonlara erişebilirsiniz.

## <a name="hotfix-production-branch"></a>Düzeltme üretim Dalı

Bir üretime fabrika dağıtımı yaptıysanız ve hemen düzeltilmesi gereken bir hata olduğunu fark ederseniz, ancak geçerli işbirliği dalını dağıtamazsınız, bir düzeltme dağıtmanız gerekebilir. Bu yaklaşım, hızlı çözüm Mühendisliği veya QFE olarak bilinir.

1.  Azure DevOps 'da üretime dağıtılan sürüme gidin. Dağıtılan son yürütmeyi bulun.

2.  Tamamlama iletisinden işbirliği dalının kayıt KIMLIĞINI alın.

3.  Bu işlemeden yeni bir düzeltme dalı oluşturun.

4.  Azure Data Factory UX ' e gidin ve düzeltme dalına geçiş yapın.

5.  Azure Data Factory UX kullanarak hatayı düzeltemedi. Değişikliklerinizi test edin.

6.  Düzeltme doğrulandıktan sonra, düzeltme Kaynak Yöneticisi şablonunu almak için **ARM şablonunu dışarı aktar** ' ı seçin.

7.  Bu derlemeyi adf_publish dalına el ile kontrol edin.

8.  Yayın işlem hattınızı adf_publish iadelerine göre otomatik olarak tetiklemek üzere yapılandırdıysanız, yeni bir yayın otomatik olarak başlatılır. Aksi takdirde, bir yayını el ile sıraya alın.

9.  Düzeltme sürümünü test ve üretim fabrikasına dağıtın. Bu sürüm, önceki üretim yükünü ve 5. adımda yaptığınız çözümü içerir.

10. Sonraki sürümlerin aynı hatayı içermemesi için düzeltmeden değişiklikleri geliştirme dalına ekleyin.

## <a name="best-practices-for-cicd"></a>CI/CD için en iyi yöntemler

Veri fabrikanınızla git tümleştirmesi kullanıyorsanız ve değişikliklerinizi geliştirmeden test ve daha sonra üretime taşıyan bir CI/CD işlem hattına sahipseniz, bu en iyi yöntemleri öneririz:

-   **Git tümleştirmesi**. Yalnızca geliştirme veri fabrikanızı git tümleştirmesi ile yapılandırmanız gerekir. Test ve üretimde yapılan değişiklikler CI/CD aracılığıyla dağıtılır ve git tümleştirmesi gerekmez.

-   **Data Factory CI/CD betiği**. CI/CD 'deki Kaynak Yöneticisi dağıtım adımından önce, Tetikleyicileri durdurma ve yeniden başlatma ve temizleme işlemlerini yapma gibi belirli görevleri gerçekleştirmeniz gerekir. Dağıtımdan önce ve sonra PowerShell betikleri kullanmanızı öneririz. Daha fazla bilgi için bkz. [etkin Tetikleyicileri güncelleştirme](#update-active-triggers).

-   **Tümleştirme çalışma zamanları ve paylaşma**. Tümleştirme çalışma zamanları sıklıkla değişmez ve CI/CD 'inizdeki tüm aşamalar arasında benzerdir. Data Factory, CI/CD 'nin tüm aşamalarında aynı ad ve türde tümleştirme çalışma zamanı olmasını bekler. Tümleştirme çalışma zamanlarını tüm aşamalarda paylaşmak istiyorsanız, paylaşılan tümleştirme çalışma zamanlarını içerecek şekilde, Üçlü bir fabrika kullanmayı düşünün. Bu paylaşılan fabrikası tüm ortamlarınızda bağlantılı tümleştirme çalışma zamanı türü olarak kullanabilirsiniz.

-   **Key Vault**. Bağlı hizmetleri Azure Key Vault göre kullandığınızda, farklı ortamlar için ayrı anahtar kasaları tutarak bundan daha fazla avantaj sağlayabilirsiniz. Ayrıca, her Anahtar Kasası için ayrı izin düzeyleri yapılandırabilirsiniz. Örneğin, ekip üyelerinizin üretim gizli dizileri için izinleri olmasını istemeyebilirsiniz. Bu yaklaşımı izlerseniz, tüm aşamalar genelinde aynı gizli adları tutmanız önerilir. Aynı adı tutarsanız, tek şey, Kaynak Yöneticisi şablonu parametrelerinden biri olan Anahtar Kasası adı olduğundan, Kaynak Yöneticisi şablonlarını CI/CD ortamları arasında değiştirmeniz gerekmez.

## <a name="unsupported-features"></a>Desteklenmeyen özellikler

- Tasarım yaparak Data Factory işleme veya kaynakların seçmeli yayımlamasına izin vermez. Yayınlar, veri fabrikasında yapılan tüm değişiklikleri içerir.

    - Data Factory varlıkları birbirlerine bağlıdır. Örneğin, tetikler, işlem hatlarına ve işlem hatları, veri kümelerine ve diğer işlem hattına bağlıdır. Bir kaynak alt kümesinin seçmeli olarak yayımlanması beklenmeyen davranışlara ve hatalara neden olabilir.
    - Seçmeli yayımlamaya ihtiyacınız olduğunda nadir olarak bir düzeltme kullanmayı düşünün. Daha fazla bilgi için bkz. [Düzeltme üretim Dalı](#hotfix-production-branch).

-   Özel dallardan yayımlayamazsınız.

-   Şu anda Bitbucket üzerinde projeler barındıramıyoruz.
