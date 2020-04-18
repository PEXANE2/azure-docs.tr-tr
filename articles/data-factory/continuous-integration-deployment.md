---
title: Azure Veri Fabrikası'nda sürekli entegrasyon ve teslimat
description: Veri Fabrikası ardışık hatlarını bir ortamdan (geliştirme, test, üretim) diğerine taşımak için sürekli tümleştirme ve teslimatı nasıl kullanacağınızı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
author: djpmsft
ms.author: daperlov
ms.reviewer: maghan
manager: jroth
ms.topic: conceptual
ms.date: 02/12/2020
ms.openlocfilehash: 3e6612b30dd8ae6716c0f87687e77c5961275ea5
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81606561"
---
# <a name="continuous-integration-and-delivery-in-azure-data-factory"></a>Azure Veri Fabrikası'nda sürekli entegrasyon ve teslimat

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

## <a name="overview"></a>Genel Bakış

Sürekli tümleştirme, kod tabanınızda yapılan her değişikliği otomatik olarak ve mümkün olduğunca erken test etme uygulamasıdır.Sürekli teslimat, sürekli tümleştirme sırasında gerçekleşen testleri izler ve bir evreleme veya üretim sistemindeki değişiklikleri iter.

Azure Veri Fabrikası'nda, sürekli tümleştirme ve teslim (CI/CD), Veri Fabrikası ardışık hatlarının bir ortamdan (geliştirme, test, üretim) diğerine taşınması anlamına gelir. C/CD yapmak için Azure Kaynak Yöneticisi şablonlarıyla Veri Fabrikası UX tümleştirmesini kullanabilirsiniz.

Veri Fabrikası UX'sinde **ARM Şablonu** açılır menüsünden bir Kaynak Yöneticisi şablonu oluşturabilirsiniz. **Dışa Aktarma KOLU**Şablonu'nu seçtiğinizde, portal veri fabrikası için Kaynak Yöneticisi şablonunu ve tüm bağlantı dizelerinizi ve diğer parametreleri içeren bir yapılandırma dosyasını oluşturur. Sonra her ortam (geliştirme, test, üretim) için bir yapılandırma dosyası oluşturursunuz. Ana Kaynak Yöneticisi şablon dosyası tüm ortamlar için aynı kalır.

Bu özellik ve bir gösteri için dokuz dakikalık bir giriş için, bu videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Continuous-integration-and-deployment-using-Azure-Data-Factory/player]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="cicd-lifecycle"></a>CI/CD yaşam döngüsü

Aşağıda, Azure Repos Git ile yapılandırılan bir Azure veri fabrikasındaki CI/CD yaşam döngüsüne örnek bir genel bakış verilmiştir. Git deposunu yapılandırma hakkında daha fazla bilgi için [Azure Veri Fabrikası'nda Kaynak denetimine](source-control.md)bakın.

1.  Azure Repos Git ile bir geliştirme veri fabrikası oluşturulur ve yapılandırılır. Tüm geliştiricilerin veri hatları ve veri kümeleri gibi Veri Fabrikası kaynaklarını yazma izni olmalıdır.

1.  Geliştiriciler özellik dallarında değişiklik yaptıkça, ardışık hatlar denetimlerini en son değişiklikleriyle birlikte hata ayıklarlar. Bir ardışık yapının hata ayıklama hakkında daha fazla bilgi için Azure [Veri Fabrikası ile Yinelemeli geliştirme ve hata ayıklama](iterative-development-debugging.md)bölümüne bakın.

1.  Geliştiriciler değişikliklerinden memnun kaldıktan sonra, değişikliklerini eşler tarafından gözden geçirmek için özellik dallarından ana veya işbirliği dalına bir çekme isteği oluştururlar.

1.  Çekme isteği onaylandıktan ve değişiklikler ana dalda birleştirildikten sonra, değişiklikler geliştirme fabrikasına yayımlanabilir.

1.  Takım değişiklikleri test fabrikasına ve ardından üretim fabrikasına dağıtmaya hazır olduğunda, takım ana daldan Kaynak Yöneticisi şablonu dışa aktarmaz.

1.  Dışa aktarılan Kaynak Yöneticisi şablonu, test fabrikasına ve üretim fabrikasına farklı parametre dosyalarıyla dağıtılır.

## <a name="create-a-resource-manager-template-for-each-environment"></a>Her ortam için Kaynak Yöneticisi şablonu oluşturma

1. ARM **Şablonu** listesinde, geliştirme ortamındaki veri fabrikanız için Kaynak Yöneticisi şablonunu dışa aktarmak için **Dışa** Aktarma KOLU Şablonu'nu seçin.

   ![Kaynak Yöneticisi şablonu dışa aktarma](media/continuous-integration-deployment/continuous-integration-image1.png)

1. Test ve üretim veri fabrikalarında, **ARM Şablonunu Al'ı**seçin. Bu eylem sizi dışa aktarılan şablonu içe aktarabileceğiniz Azure portalına götürür. Kaynak Yöneticisi şablon düzenleyicisini açmak için **düzenleyicide kendi şablonunuzu oluştur'u** seçin.

   ![Kendi şablonunuzu oluşturun](media/continuous-integration-deployment/custom-deployment-build-your-own-template.png) 

1. **Dosyayı Yükle'yi**seçin ve ardından oluşturulan Kaynak Yöneticisi şablonuna bakın. Bu, adım 1'de dışa aktarılan .zip dosyasında bulunan **arm_template.json** dosyasıdır.

   ![Şablonu edin](media/continuous-integration-deployment/custom-deployment-edit-template.png)

1. Ayarlar bölümünde, bağlı hizmet kimlik bilgileri gibi yapılandırma değerlerini girin. İşi bittiğinde, Kaynak Yöneticisi şablonuna dağıtmak için **Satın Al'ı** seçin.

   ![Ayarlar bölümü](media/continuous-integration-deployment/continuous-integration-image5.png)

### <a name="connection-strings"></a>Bağlantı dizeleri

Bağlantı dizelerini yapılandırma hakkında bilgi için bağlayıcının makalesine bakın. Örneğin, Azure SQL Veritabanı için Azure [Veri Fabrikası'nı kullanarak verileri Azure SQL Veritabanına kopyala'ya veya Azure SQL Veritabanından kopyalayın'a](connector-azure-sql-database.md)bakın. Bir bağlantı dizesini doğrulamak için, Veri Fabrikası UX'deki kaynağın kod görünümünü açabilirsiniz. Kod görünümünde, bağlantı dizesinin parola veya hesap anahtarı bölümü kaldırılır. Kod görünümünü açmak için burada vurgulanan simgeyi seçin:

![Bağlantı dizesini görmek için kod görünümünü açma](media/continuous-integration-deployment/continuous-integration-codeview.png)

## <a name="automate-continuous-integration-by-using-azure-pipelines-releases"></a>Azure Pipelines sürümlerini kullanarak sürekli tümleştirmeyi otomatikleştirin

Aşağıda, bir veri fabrikasının birden çok ortama dağıtımını otomatikleştiren bir Azure Ardışık Alanları sürümü ayarlama kılavuzu veremser.

![Azure Boru Hatları ile sürekli tümleştirme diyagramı](media/continuous-integration-deployment/continuous-integration-image12.png)

### <a name="requirements"></a>Gereksinimler

-    [Azure Kaynak Yöneticisi hizmeti bitiş noktasını](https://docs.microsoft.com/azure/devops/pipelines/library/service-endpoints#sep-azure-resource-manager)kullanan Visual Studio Team Foundation Server veya Azure Repos'a bağlı bir Azure aboneliği.

-   Azure Repos Git tümleştirmesi ile yapılandırılan bir veri fabrikası.

-   Her ortamın sırlarını içeren bir [Azure anahtar kasası.](https://azure.microsoft.com/services/key-vault/)

### <a name="set-up-an-azure-pipelines-release"></a>Azure Ardışık Hatları sürümü ayarlama

1.  [Azure DevOps'te,](https://dev.azure.com/)veri fabrikanızla yapılandırılan projeyi açın.

1.  Sayfanın sol tarafında, **Ardışık Hatlar'ı**seçin ve ardından **Sürümler'i**seçin.

    ![Boru Hatları, Sürümler seçin](media/continuous-integration-deployment/continuous-integration-image6.png)

1.  **Yeni ardışık hatlar'ı**seçin veya varolan ardışık hatlar varsa **Yeni** ve ardından Yeni **sürüm ardışık hattını**seçin.

1.  Boş **iş** şablonu'nu seçin.

    ![Boş iş'i seçin](media/continuous-integration-deployment/continuous-integration-image13.png)

1.  Sahne **adı** kutusuna ortamınızın adını girin.

1.  **Yapı Ekle'yi**seçin ve ardından veri fabrikanızla yapılandırılan depoyu seçin. **Varsayılan dal**için **adf_publish'yi** seçin. Varsayılan **sürüm**için, **varsayılan daldan En Son'u**seçin.

    ![Yapıt ekleme](media/continuous-integration-deployment/continuous-integration-image7.png)

1.  Azure Kaynak Yöneticisi Dağıtım görevi ekleyin:

    a.  Sahne görünümünde, **sahne görevlerini görüntüle'yi**seçin.

    ![Sahne görünümü](media/continuous-integration-deployment/continuous-integration-image14.png)

    b.  Yeni bir görev oluşturun. Azure **Kaynak Grubu Dağıtımı'nı**arayın ve sonra **Ekle'yi**seçin.

    c.  Dağıtım görevinde, hedef veri fabrikası için abonelik, kaynak grubu ve konumu seçin. Gerekirse kimlik bilgilerini sağlayın.

    d.  **Eylem** listesinde kaynak **grubu oluştur'u veya güncelleştir'i**seçin.

    e.  **Şablon** kutusunun yanındaki elips düğmesini (**...**) seçin. Bu makalenin her ortam bölümü için Kaynak Yöneticisi Oluştur şablonunda **İçe Aktar KOLU** Şablonu'nu kullanarak oluşturduğunuz Azure Kaynak Yöneticisi [şablonuna](continuous-integration-deployment.md#create-a-resource-manager-template-for-each-environment) göz atın. bu dosyayı <FactoryName> adf_publish dalı klasöründe arayın.

    f.  Seçin **...** parametreler dosyasını seçmek için **Şablon parametreleri** kutusunun yanında. Seçtiğiniz dosya, bir kopyasını oluşturup oluşturmadığınıza veya varsayılan dosya olan ARMTemplateParametersForFactory.json'u kullanıp kullanmadığınıza bağlıdır.

    g.  Seçin **...** **Override şablon parametreleri** kutusunun yanında ve hedef veri fabrikası için bilgileri girin. Azure Key Vault'tan gelen kimlik bilgileri için, çift tırnak işaretleri arasına sırrın adını girin. Örneğin, sırrın adı cred1 ise, bu değer için **"$(cred1)"** girin.

    h. **Dağıtım modu**için **Artıl'ı** seçin.

    > [!WARNING]
    > **Dağıtım modu**için **Tamam'ı** seçerseniz, Kaynak Yöneticisi şablonunda tanımlanmayan hedef kaynak grubundaki tüm kaynaklar da dahil olmak üzere varolan kaynaklar silinebilir.

    ![Veri Fabrikası Prod Dağıtım](media/continuous-integration-deployment/continuous-integration-image9.png)

1.  Serbest bırakma ardışık hattını kaydedin.

1. Bir sürümü tetiklemek **için, sürüm oluştur'u**seçin.

   ![Sürüm Oluştur'u seçin](media/continuous-integration-deployment/continuous-integration-image10.png)

> [!IMPORTANT]
> CI/CD senaryolarında, farklı ortamlardaki tümleştirme çalışma süresi (IR) türü aynı olmalıdır. Örneğin, geliştirme ortamında kendi kendine barındırılan bir IR'niz varsa, aynı IR de test ve üretim gibi diğer ortamlarda kendi kendine barındırılan türde olmalıdır. Benzer şekilde, tümleştirme çalışma sürelerini birden çok aşamada paylaşıyorsanız, tümleştirme çalışma sürelerini geliştirme, test ve üretim gibi tüm ortamlarda kendi kendine barındırılan bağlantılı olarak yapılandırmanız gerekir.

### <a name="get-secrets-from-azure-key-vault"></a>Azure Key Vault'tan sır alın

Azure Kaynak Yöneticisi şablonunda geçirilen sırlarınız varsa, Azure Ardışık Hatları sürümüyle Azure Anahtar Kasası'nı kullanmanızı öneririz.

Sırları ele almanın iki yolu vardır:

1.  Parametreler dosyasına sırları ekleyin. Daha fazla bilgi için, [dağıtım sırasında güvenli parametre değerini geçmek için Azure Anahtar Kasası'nı kullanın'a](../azure-resource-manager/templates/key-vault-parameter.md)bakın.

    Yayımlama dalına yüklenen parametre dosyasının bir kopyasını oluşturun. Bu biçimi kullanarak Key Vault'tan almak istediğiniz parametrelerin değerlerini ayarlayın:

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

    Bu yöntemi kullandığınızda, gizli otomatik olarak anahtar kasasından çekilir.

    Parametreler dosyasının yayımlama dalında da olması gerekir.

1. Önceki bölümde açıklanan Azure Kaynak Yöneticisi Dağıtımı görevinden önce bir [Azure Anahtar Kasası görevi](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-key-vault) ekleyin:

    1.  **Görevler** sekmesinde yeni bir görev oluşturun. Azure **Anahtar Kasası'nı** arayın ve ekleyin.

    1.  Anahtar Kasa görevinde, anahtar kasasını oluşturduğunuz aboneliği seçin. Gerekirse kimlik bilgilerini sağlayın ve ardından anahtar kasasını seçin.

    ![Anahtar Kasası görevi ekleme](media/continuous-integration-deployment/continuous-integration-image8.png)

#### <a name="grant-permissions-to-the-azure-pipelines-agent"></a>Azure Pipelines aracısına izin verme

Doğru izinler ayarlanmazsa, Azure Anahtar Kasası görevi Access Reddedildi hatasıyla başarısız olabilir. Sürüm günlüklerini indirin ve Azure Pipelines aracısına izin vermek için komutu içeren .ps1 dosyasını bulun. Komutu doğrudan çalıştırabilirsiniz. Veya dosyadan asıl kimliği kopyalayabilir ve Azure portalına el ile erişim ilkesini ekleyebilirsiniz. `Get`ve `List` gerekli minimum izinlerdir.

### <a name="update-active-triggers"></a>Etkin tetikleyicileri güncelleştirme

Etkin tetikleyicileri güncelleştirmeye çalışırsanız dağıtım başarısız olabilir. Etkin tetikleyicileri güncelleştirmek için, bunları el ile durdurmanız ve dağıtımdan sonra yeniden başlatmanız gerekir. Bunu bir Azure PowerShell görevini kullanarak yapabilirsiniz:

1.  Sürümün **Görevler** sekmesine bir **Azure PowerShell** görevi ekleyin. Görev sürümü 4.*'u seçin. 

1.  Fabrikanızın içinde olduğu aboneliği seçin.

1.  Komut dosyası türü olarak **Komut Dosyası Dosyası Yolu'nu** seçin. Bunun için PowerShell komut dosyanızı deponuza kaydetmeniz gerekiyor. Aşağıdaki PowerShell komut dosyası tetikleyicileri durdurmak için kullanılabilir:

    ```powershell
    $triggersADF = Get-AzDataFactoryV2Trigger -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName

    $triggersADF | ForEach-Object { Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_.name -Force }
    ```

Dağıtımdan sonra tetikleyicileri `Start-AzDataFactoryV2Trigger` yeniden başlatmak için benzer adımları (işlevle) tamamlayabilirsiniz.

### <a name="sample-pre--and-post-deployment-script"></a>Dağıtım öncesi ve sonrası komut dosyası örneği

Aşağıdaki örnek komut dosyası, dağıtımdan önce tetikleyicileri durdurmak ve daha sonra yeniden başlatmak için kullanılabilir. Komut dosyası, kaldırılan kaynakları silmek için kod da içerir. Komut dosyasını bir Azure DevOps git deposuna kaydedin ve sürüm 4'ü kullanarak azure PowerShell görevi aracılığıyla başvurun.*.

Dağıtım öncesi komut dosyası çalıştırırken, **Komut Dosyası Bağımsız Değişkenleri** alanında aşağıdaki parametrelerin bir varyasyonu belirtmeniz gerekir.

`-armTemplate "$(System.DefaultWorkingDirectory)/<your-arm-template-location>" -ResourceGroupName <your-resource-group-name> -DataFactoryName <your-data-factory-name>  -predeployment $true -deleteDeployment $false`


Dağıtım sonrası komut dosyası çalıştırırken, **Komut Dosyası Bağımsız Değişkenleri** alanında aşağıdaki parametrelerin bir varyasyonu belirtmeniz gerekir.

`-armTemplate "$(System.DefaultWorkingDirectory)/<your-arm-template-location>" -ResourceGroupName <your-resource-group-name> -DataFactoryName <your-data-factory-name>  -predeployment $false -deleteDeployment $true`

![Azure PowerShell görevi](media/continuous-integration-deployment/continuous-integration-image11.png)

Burada dağıtım öncesi ve sonrası için kullanılabilecek komut dosyası ve komut dosyası. Silinen kaynakları ve kaynak başvurularını hesaplar.

```powershell
param
(
    [parameter(Mandatory = $false)] [String] $armTemplate,
    [parameter(Mandatory = $false)] [String] $ResourceGroupName,
    [parameter(Mandatory = $false)] [String] $DataFactoryName,
    [parameter(Mandatory = $false)] [Bool] $predeployment=$true,
    [parameter(Mandatory = $false)] [Bool] $deleteDeployment=$false
)

function getPipelineDependencies {
    param([System.Object] $activity)
    if ($activity.Pipeline) {
        return @($activity.Pipeline.ReferenceName)
    } elseif ($activity.Activities) {
        $result = @()
        return $activity.Activities | ForEach-Object{ $result += getPipelineDependencies -activity $_ }
    } elseif ($activity.ifFalseActivities -or $activity.ifTrueActivities) {
        $result = @()
        $activity.ifFalseActivities | Where-Object {$_ -ne $null} | ForEach-Object{ $result += getPipelineDependencies -activity $_ }
        $activity.ifTrueActivities | Where-Object {$_ -ne $null} | ForEach-Object{ $result += getPipelineDependencies -activity $_ }
    } elseif ($activity.defaultActivities) {
        $result = @()
        $activity.defaultActivities | ForEach-Object{ $result += getPipelineDependencies -activity $_ }
        if ($activity.cases) {
            $activity.cases | ForEach-Object{ $_.activities } | ForEach-Object{$result += getPipelineDependencies -activity $_ }
        }
        return $result
    }
}

function pipelineSortUtil {
    param([Microsoft.Azure.Commands.DataFactoryV2.Models.PSPipeline]$pipeline,
    [Hashtable] $pipelineNameResourceDict,
    [Hashtable] $visited,
    [System.Collections.Stack] $sortedList)
    if ($visited[$pipeline.Name] -eq $true) {
        return;
    }
    $visited[$pipeline.Name] = $true;
    $pipeline.Activities | ForEach-Object{ getPipelineDependencies -activity $_ -pipelineNameResourceDict $pipelineNameResourceDict}  | ForEach-Object{
        pipelineSortUtil -pipeline $pipelineNameResourceDict[$_] -pipelineNameResourceDict $pipelineNameResourceDict -visited $visited -sortedList $sortedList
    }
    $sortedList.Push($pipeline)

}

function Get-SortedPipelines {
    param(
        [string] $DataFactoryName,
        [string] $ResourceGroupName
    )
    $pipelines = Get-AzDataFactoryV2Pipeline -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $ppDict = @{}
    $visited = @{}
    $stack = new-object System.Collections.Stack
    $pipelines | ForEach-Object{ $ppDict[$_.Name] = $_ }
    $pipelines | ForEach-Object{ pipelineSortUtil -pipeline $_ -pipelineNameResourceDict $ppDict -visited $visited -sortedList $stack }
    $sortedList = new-object Collections.Generic.List[Microsoft.Azure.Commands.DataFactoryV2.Models.PSPipeline]
    
    while ($stack.Count -gt 0) {
        $sortedList.Add($stack.Pop())
    }
    $sortedList
}

function triggerSortUtil {
    param([Microsoft.Azure.Commands.DataFactoryV2.Models.PSTrigger]$trigger,
    [Hashtable] $triggerNameResourceDict,
    [Hashtable] $visited,
    [System.Collections.Stack] $sortedList)
    if ($visited[$trigger.Name] -eq $true) {
        return;
    }
    $visited[$trigger.Name] = $true;
    $trigger.Properties.DependsOn | Where-Object {$_ -and $_.ReferenceTrigger} | ForEach-Object{
        triggerSortUtil -trigger $triggerNameResourceDict[$_.ReferenceTrigger.ReferenceName] -triggerNameResourceDict $triggerNameResourceDict -visited $visited -sortedList $sortedList
    }
    $sortedList.Push($trigger)
}

function Get-SortedTriggers {
    param(
        [string] $DataFactoryName,
        [string] $ResourceGroupName
    )
    $triggers = Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName
    $triggerDict = @{}
    $visited = @{}
    $stack = new-object System.Collections.Stack
    $triggers | ForEach-Object{ $triggerDict[$_.Name] = $_ }
    $triggers | ForEach-Object{ triggerSortUtil -trigger $_ -triggerNameResourceDict $triggerDict -visited $visited -sortedList $stack }
    $sortedList = new-object Collections.Generic.List[Microsoft.Azure.Commands.DataFactoryV2.Models.PSTrigger]
    
    while ($stack.Count -gt 0) {
        $sortedList.Add($stack.Pop())
    }
    $sortedList
}

function Get-SortedLinkedServices {
    param(
        [string] $DataFactoryName,
        [string] $ResourceGroupName
    )
    $linkedServices = Get-AzDataFactoryV2LinkedService -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName
    $LinkedServiceHasDependencies = @('HDInsightLinkedService', 'HDInsightOnDemandLinkedService', 'AzureBatchLinkedService')
    $Akv = 'AzureKeyVaultLinkedService'
    $HighOrderList = New-Object Collections.Generic.List[Microsoft.Azure.Commands.DataFactoryV2.Models.PSLinkedService]
    $RegularList = New-Object Collections.Generic.List[Microsoft.Azure.Commands.DataFactoryV2.Models.PSLinkedService]
    $AkvList = New-Object Collections.Generic.List[Microsoft.Azure.Commands.DataFactoryV2.Models.PSLinkedService]

    $linkedServices | ForEach-Object {
        if ($_.Properties.GetType().Name -in $LinkedServiceHasDependencies) {
            $HighOrderList.Add($_)
        }
        elseif ($_.Properties.GetType().Name -eq $Akv) {
            $AkvList.Add($_)
        }
        else {
            $RegularList.Add($_)
        }
    }

    $SortedList = New-Object Collections.Generic.List[Microsoft.Azure.Commands.DataFactoryV2.Models.PSLinkedService]($HighOrderList.Count + $RegularList.Count + $AkvList.Count)
    $SortedList.AddRange($HighOrderList)
    $SortedList.AddRange($RegularList)
    $SortedList.AddRange($AkvList)
    $SortedList
}

$templateJson = Get-Content $armTemplate | ConvertFrom-Json
$resources = $templateJson.resources

#Triggers 
Write-Host "Getting triggers"
$triggersADF = Get-SortedTriggers -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
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
        Remove-AzDataFactoryV2TriggerSubscription -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_ -Force
    $status = Get-AzDataFactoryV2TriggerSubscriptionStatus -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_
    while ($status.Status -ne "Disabled"){
            Start-Sleep -s 15
            $status = Get-AzDataFactoryV2TriggerSubscriptionStatus -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_
    }
    Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_ -Force 
    }
}
else {
    #Deleted resources
    #pipelines
    Write-Host "Getting pipelines"
    $pipelinesADF = Get-SortedPipelines -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $pipelinesTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/pipelines" }
    $pipelinesNames = $pipelinesTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
    $deletedpipelines = $pipelinesADF | Where-Object { $pipelinesNames -notcontains $_.Name }
    #dataflows
    $dataflowsADF = Get-AzDataFactoryV2DataFlow -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $dataflowsTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/dataflows" }
    $dataflowsNames = $dataflowsTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40) }
    $deleteddataflow = $dataflowsADF | Where-Object { $dataflowsNames -notcontains $_.Name }
    #datasets
    Write-Host "Getting datasets"
    $datasetsADF = Get-AzDataFactoryV2Dataset -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $datasetsTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/datasets" }
    $datasetsNames = $datasetsTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40) }
    $deleteddataset = $datasetsADF | Where-Object { $datasetsNames -notcontains $_.Name }
    #linkedservices
    Write-Host "Getting linked services"
    $linkedservicesADF = Get-SortedLinkedServices -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
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
    Write-Host "Deleting dataflows"
    $deleteddataflow | ForEach-Object { 
        Write-Host "Deleting dataflow " $_.Name
        Remove-AzDataFactoryV2DataFlow -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
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
        Add-AzDataFactoryV2TriggerSubscription -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_ -Force
    $status = Get-AzDataFactoryV2TriggerSubscriptionStatus -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_
    while ($status.Status -ne "Enabled"){
            Start-Sleep -s 15
            $status = Get-AzDataFactoryV2TriggerSubscriptionStatus -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_
    }
    Start-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_ -Force 
    }
}
```

## <a name="use-custom-parameters-with-the-resource-manager-template"></a>Resource Manager şablonuyla özel parametreler kullanma

GIT modundaysanız, şablonda parametreli özellikleri ve sabit kodlanmış özellikleri ayarlamak için Kaynak Yöneticisi şablonunuzdavarsayılan özellikleri geçersiz kılabilirsiniz. Bu senaryolarda varsayılan parametreoluşturma şablonunu geçersiz kılmak isteyebilirsiniz:

* Otomatik CI/CD kullanıyorsunuz ve Kaynak Yöneticisi dağıtımı sırasında bazı özellikleri değiştirmek istiyorsunuz, ancak özellikler varsayılan olarak parametreye sahip değil.
* Fabrikanız o kadar büyüktür ki varsayılan Kaynak Yöneticisi şablonu, izin verilen en yüksek parametrelerden daha fazla olduğu için geçersizdir (256).

Bu koşullar altında, varsayılan parametrelendirme şablonunu geçersiz kılmak için, veri fabrikası git tümleştirmesi için kök klasör olarak belirtilen klasörde **kol-şablon-parametreler-definition.json** adlı bir dosya oluşturun. Tam olarak bu dosya adını kullanmalısınız. Veri Fabrikası bu dosyayı yalnızca işbirliği dalından değil, Azure Veri Fabrikası portalında şu anda hangi şubede çalışıyorsanız okur. UI'de **Dışa Aktarma KOLU** Şablonu'nu seçerek değişikliklerinizi test edebileceğiniz özel bir daldan dosyayı oluşturabilir veya düzenleme yapabilirsiniz. Daha sonra dosyayı işbirliği dalında birleştirebilirsiniz. Dosya bulunamazsa, varsayılan şablon kullanılır.

> [!NOTE]
> Özel parametrelendirme şablonu, 256 ARM şablonu parametre sınırını değiştirmez. Parametreli özelliklerin sayısını seçmenize ve azaltmanıza olanak tanır.

### <a name="syntax-of-a-custom-parameters-file"></a>Özel parametreler dosyasının sözdizimi

Özel parametreler dosyasını oluştururken izlenir. Dosya her varlık türü için bir bölümden oluşur: tetikleyici, ardışık hatlar, bağlantılı hizmet, veri kümesi, tümleştirme çalışma süresi ve benzeri.
* İlgili varlık türü altında özellik yolunu girin.
* Bir özellik adını, `*` altındaki tüm özellikleri parametreye getirmek istediğinizi gösterir (yalnızca ilk düzeye kadar, özyinelemeli olarak değil). Bu yapılandırma için özel durumlar da sağlayabilirsiniz.
* Bir özelliğin değerini dize olarak ayarlamak, özelliği parametrelendirmek istediğinizi gösterir. Biçimi `<action>:<name>:<stype>`kullanın.
   *  `<action>` bu karakterlerden biri olabilir:
      * `=` parametrenin varsayılan değeri olarak geçerli değeri tutmak anlamına gelir.
      * `-` parametre için varsayılan değeri tutmayın anlamına gelir.
      * `|` bağlantı dizeleri veya anahtarları için Azure Key Vault'tan gelen sırlar için özel bir durumdur.
   * `<name>` parametrenin adıdır. Boşsa, mülkün adını alır. Değer bir `-` karakterle başlarsa, ad kısaltılır. Örneğin, `AzureStorage1_properties_typeProperties_connectionString` kısaltılmış `AzureStorage1_connectionString`olacaktır.
   * `<stype>` parametre türüdür.  `<stype>`Boşsa, varsayılan tür `string` . Desteklenen değerler: `string` `bool`, `number` `object`, `securestring`, , ve .
* Tanım dosyasında bir dizi belirtilmesi, şablondaki eşleşen özelliğin bir dizi olduğunu gösterir. Veri Fabrikası, dizinin tümleştirme çalışma zamanı nesnesinde belirtilen tanımı kullanarak dizideki tüm nesneleri yineler. İkinci nesne, bir dize, her yineleme için parametre adı olarak kullanılan özelliğin adı olur.
* Bir tanım kaynak örneğine özgü olamaz. Herhangi bir tanım bu türtüm kaynaklar için geçerlidir.
* Varsayılan olarak, Anahtar Vault sırları gibi tüm güvenli dizeleri ve bağlantı dizeleri, anahtarlar ve belirteçleri gibi güvenli dizeleri parametreli.
 
### <a name="sample-parameterization-template"></a>Örnek parametreleme şablonu

Parametrelendirme şablonunun nasıl görünebileceğine bir örnek aşağıda verilmiştir:

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
Aşağıda, önceki şablonun kaynak türüne göre nasıl oluşturulduklarına ilişkin bir açıklama verilmiştir.

#### <a name="pipelines"></a>İşlem hatları
    
* Yoldaki `activities/typeProperties/waitTimeInSeconds` herhangi bir özellik parametreli. Kod düzeyinde bir özelliği olan `waitTimeInSeconds` bir ardışık etki adındaki herhangi bir etkinlik (örneğin, `Wait` etkinlik) varsayılan bir adla bir sayı olarak parametreye ayarlanır. Ancak Kaynak Yöneticisi şablonunda varsayılan bir değer olmaz. Kaynak Yöneticisi dağıtımı sırasında zorunlu bir giriş olacaktır.
* Benzer şekilde, (örneğin, bir `headers` `Web` etkinlikte) adlı bir özellik `object` türü (JObject) ile parametreli. Varsayılan değeri vardır ve bu değer kaynak fabrikayla aynı değere sahiptir.

#### <a name="integrationruntimes"></a>EntegrasyonRuntimes

* Yol `typeProperties` altındaki tüm özellikler, ilgili varsayılan değerleriyle parametreye yerleştirilir. Örneğin, tür özellikleri altında `IntegrationRuntimes` iki `computeProperties` özellik `ssisProperties`vardır: ve . Her iki özellik türü de kendi varsayılan değerleri ve türleri (Nesne) ile oluşturulur.

#### <a name="triggers"></a>Tetikleyiciler

* Altında, `typeProperties`iki özellik parametreli. `maxConcurrency`İlki, varsayılan bir değere sahip olduğu belirtilen`string`ve türünde olan. Varsayılan parametre adı `<entityName>_properties_typeProperties_maxConcurrency`vardır.
* Özellik `recurrence` de parametreli. Altında, bu düzeydeki tüm özellikleri varsayılan değerleri ve parametre adları ile dizeleri olarak parametreli olarak belirtilir. Bir özel `interval` durum türü `number`olarak parametreli özelliktir. Parametre adı `<entityName>_properties_typeProperties_recurrence_triggerSuffix`ile suffixed olduğunu . Benzer şekilde, `freq` özellik bir dize ve bir dize olarak parametreli. Ancak, `freq` özellik varsayılan bir değer olmadan parametrelendirilmiştir. Adı kısaltılmış ve suffixed. Örneğin, `<entityName>_freq`.

#### <a name="linkedservices"></a>Bağlantılı Hizmetler

* Bağlantılı hizmetler benzersizdir. Bağlı hizmetler ve veri kümeleri çok çeşitli türlere sahip olduğundan, türe özgü özelleştirme sağlayabilirsiniz. Bu örnekte, türünün `AzureDataLakeStore`tüm bağlantılı hizmetleri için belirli bir şablon uygulanır. Diğerleri için (üzerinden), `*`farklı bir şablon uygulanır.
* Özellik `connectionString` bir `securestring` değer olarak parametreye olur. Varsayılan değeri olmaz. Bu suffixed's kısaltılmış bir parametre `connectionString`adı olacaktır.
* Özellik `secretAccessKey` bir `AzureKeyVaultSecret` (örneğin, bir Amazon S3 bağlantılı hizmet) olur. Azure Key Vault sırrı olarak otomatik olarak parametreye aktarılır ve yapılandırılan anahtar kasasından getirilir. Anahtar kasasının kendisini de parametrenize edebilirsiniz.

#### <a name="datasets"></a>Veri kümeleri

* Veri kümeleri için türe özgü özelleştirme kullanılabilir olsa da, \*açıkça düzey yapılandırması olmadan yapılandırma sağlayabilirsiniz. Önceki örnekte, altındaki `typeProperties` tüm veri kümesi özellikleri parametrelidir.

### <a name="default-parameterization-template"></a>Varsayılan parametreleme şablonu

Aşağıdaki geçerli varsayılan parametrelendirme şablonudur. Yalnızca birkaç parametre eklemeniz gerekiyorsa, varolan parametreboyutlandırma yapısını kaybetmeyeceğiniz için bu şablonu doğrudan düzenlemek iyi bir fikir olabilir.

```json
{
    "Microsoft.DataFactory/factories/pipelines": {
    },
    "Microsoft.DataFactory/factories/dataflows": {
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
                    "poolName": "=",
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

Aşağıdaki örnekte, varsayılan parametreleştirme şablonuna tek bir değer innasıl eklenir. Yalnızca Veri Tuğlaları bağlantılı bir hizmet için varolan bir Azure Databricks etkileşimli küme kimliği'ni parametreler dosyasına eklemek istiyoruz. Bu dosyanın önceki dosyayla `existingClusterId` aynı olduğunu `Microsoft.DataFactory/factories/linkedServices`unutmayın.

```json
{
    "Microsoft.DataFactory/factories/pipelines": {
    },
    "Microsoft.DataFactory/factories/dataflows": {
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
            "poolName": "=",
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

## <a name="linked-resource-manager-templates"></a>Bağlantılı Kaynak Yöneticisi şablonları

Veri fabrikalarınız için CI/CD ayarladıysanız, fabrikanız büyüdükçe Azure Kaynak Yöneticisi şablon sınırlarını aşabilirsiniz. Örneğin, bir sınır, Kaynak Yöneticisi şablonundaki en büyük kaynak sayısıdır. Bir fabrika için tam Kaynak Yöneticisi şablonu oluştururken büyük fabrikaları barındırmak için, Veri Fabrikası artık bağlantılı Kaynak Yöneticisi şablonları oluşturur. Bu özellik sayesinde, tüm fabrika yükü çeşitli dosyalara bölünerek sınırlar tarafından sınırlandırılmamanız için.

Git'i yapılandırmışsanız, bağlı şablonlar adf_publish dalındaki tam Kaynak Yöneticisi şablonlarının yanında linkedTemplates adlı yeni bir klasörde oluşturulur ve kaydedilir:

![Bağlantılı Kaynak Yöneticisi şablonlar klasörü](media/continuous-integration-deployment/linked-resource-manager-templates.png)

Bağlı Kaynak Yöneticisi şablonları genellikle bir ana şablon ve ana şablona bağlı bir alt şablon kümesinden oluşur. Üst şablon ArmTemplate_master.json olarak adlandırılır ve alt şablonlar ArmTemplate_0.json, ArmTemplate_1.json ve benzeri desenle adlandırılır. 

Tam Kaynak Yöneticisi şablonu yerine bağlantılı şablonları kullanmak için, Ci/CD görevinizi ArmTemplateForFactory.json (tam Kaynak Yöneticisi şablonu) yerine ArmTemplate_master.json'a işaret etmek için güncelleştirin. Kaynak Yöneticisi, Azure'un dağıtım sırasında bunlara erişebilmeleri için bağlantılı şablonları bir depolama hesabına yüklemenizi de gerektirir. Daha fazla bilgi için bkz: [VSTS ile bağlantılı Kaynak Yöneticisi şablonlarını dağıtma.](https://blogs.msdn.microsoft.com/najib/2018/04/22/deploying-linked-arm-templates-with-vsts/)

Dağıtım görevinden önce ve sonra CI/CD ardışık sisteminize Veri Fabrikası komut dosyalarını eklemeyi unutmayın.

Git'i yapılandırmadıysanız, ARM **Şablonu** listesinde dışa aktarma **KOLU Şablonu** aracılığıyla bağlantılı şablonlara erişebilirsiniz.

## <a name="hotfix-production-branch"></a>Hotfix üretim şubesi

Bir fabrikayı üretime dağıyorsanız ve hemen düzeltilmesi gereken bir hata olduğunu fark ederseniz, ancak geçerli işbirliği dalını dağıtamıyorsanız, bir düzeltme dağıtmanız gerekebilir. Bu yaklaşım, hızlı düzeltme mühendisliği veya QFE olarak bilinir.

1.    Azure DevOps'te, üretime dağıtılan sürüme gidin. Dağıtılan son commit'ı bulun.

2.    İletiyi iletme den, işbirliği şubesinin kimlik sini alın.

3.    Bu taahhüt yeni bir hotfix dalı oluşturun.

4.    Azure Veri Fabrikası UX'ye gidin ve hotfix şubesine geçin.

5.    Azure Veri Fabrikası UX'yi kullanarak hatayı düzeltin. Değişikliklerinizi test edin.

6.    Düzeltme doğrulandıktan sonra, hotfix Kaynak Yöneticisi şablonuna ulaşmak için **DıŞA Aktarım KOLU** Şablonu'nu seçin.

7.    Bu yapıyı adf_publish dalına elle kontrol edin.

8.    Sürüm ardışık adf_publish denetime göre otomatik olarak tetiklenecek şekilde yapılandırıldıysanız, yeni bir sürüm otomatik olarak başlatılır. Aksi takdirde, bir sürümü el ile sıralar.

9.    Hotfix yayınını test ve üretim fabrikalarına dağıtın. Bu sürüm, önceki üretim yükünün yanı sıra adım 5'te yaptığınız düzeltmeyi içerir.

10.    Daha sonraki sürümler aynı hatayı içermeyecek şekilde düzeltmedeki değişiklikleri geliştirme dalına ekleyin.

## <a name="best-practices-for-cicd"></a>CI/CD için en iyi uygulamalar

Veri fabrikanızla Git tümleştirmesini kullanıyorsanız ve değişikliklerinizi geliştirmeden teste ve daha sonra üretime dönüştüren bir CI/CD ardışık hattınız varsa, şu en iyi uygulamaları öneririz:

-   **Git entegrasyonu**. Yalnızca geliştirme veri fabrikanızı Git tümleştirmesiyle yapılandırmanız gerekir. Test ve üretimdeki değişiklikler CI/CD aracılığıyla dağıtılır ve Git tümleştirmesine gerek yoktur.

-   **Veri Fabrikası CI/CD komut dosyası.** KAYNAK Yöneticisi dağıtım adımı CI/CD'de tamamlanmadan önce, tetikleyicileri durdurma ve yeniden başlatma ve temizleme gerçekleştirme gibi belirli görevleri tamamlamanız gerekir. Dağıtımdan önce ve sonra PowerShell komut dosyalarını kullanmanızı öneririz. Daha fazla bilgi için [bkz.](#update-active-triggers)

-   **Tümleştirme çalışma saatleri ve paylaşım.** Tümleştirme çalışma süreleri sık sık değişmez ve CI/CD'nizdeki tüm aşamalarda benzerdir. Bu nedenle, Veri Fabrikası, CI/CD'nin tüm aşamalarında aynı ada ve türde tümleştirme çalışma süresine sahip olduğunuzu bekler. Tüm aşamalarda tümleştirme çalışma sürelerini paylaşmak istiyorsanız, yalnızca paylaşılan tümleştirme çalışma sürelerini içerecek şekilde bir üçüncül fabrika kullanmayı düşünün. Bu paylaşılan fabrikayı tüm ortamlarınızda bağlantılı tümleştirme çalışma zamanı türü olarak kullanabilirsiniz.

-   **Anahtar Kasası**. Azure Anahtar Kasası'nı temel alan bağlantılı hizmetleri kullandığınızda, farklı ortamlar için ayrı anahtar kasaları tutarak bu hizmetlerden daha fazla yararlanabilirsiniz. Ayrıca, her anahtar kasası için ayrı izin düzeylerini yapılandırabilirsiniz. Örneğin, ekip üyelerinizin gizli üretim izinlerine sahip olmasını istemeyebilirsiniz. Bu yaklaşımı izlerseniz, tüm aşamalarda aynı gizli adları saklamanızı öneririz. Aynı adları tutarsanız, Kaynak Yöneticisi şablonlarınızı CI/CD ortamlarında değiştirmeniz gerekmez, çünkü değişen tek şey Kaynak Yöneticisi şablon parametrelerinden biri olan anahtar kasa adıdır.

## <a name="unsupported-features"></a>Desteklenmeyen özellikler

- Tasarım gereği, Veri Fabrikası kiraz toplama taahhütleri veya kaynakların seçici yayımlanmasına izin vermez. Yayımlar, veri fabrikasında yapılan tüm değişiklikleri içerir.

    - Veri fabrikası varlıkları birbirine bağlıdır. Örneğin, tetikleyiciler ardışık lıklara, ardışık lıklar da veri kümelerine ve diğer ardışık lıklara bağlıdır. Kaynakların bir alt kümesinin seçici yayımlama beklenmeyen davranışlar ve hatalara yol açabilir.
    - Seçici yayımlama ya ihtiyacınız olduğunda, bir düzeltme kullanmayı düşünün. Daha fazla bilgi için [Hotfix üretim şubesine](#hotfix-production-branch)bakın.

-   Özel şubelerden yayın yapamazsınız.

-   Şu anda Bitbucket'ta proje barındıramaz.
