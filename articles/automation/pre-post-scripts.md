---
title: Azure 'da Güncelleştirme Yönetimi dağıtımınızda ön ve son betik yapılandırma
description: Bu makalede, güncelleştirme dağıtımları için ön ve son betiklerin nasıl yapılandırılacağı ve yönetileceği açıklanmaktadır
services: automation
ms.service: automation
ms.subservice: update-management
author: bobbytreed
ms.author: robreed
ms.date: 05/17/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 84cd5db812d995f1160a02917eac5857ee076c7f
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72374451"
---
# <a name="manage-pre-and-post-scripts"></a>Ön ve son betikleri yönetme

Öncesi ve sonrası betikleri, Otomasyon hesabınızda (ön görev) ve sonra (görev sonrası) bir güncelleştirme dağıtımı yapmadan önce PowerShell runbook 'larını çalıştırmanızı sağlar. Ön ve son betikler Azure bağlamında çalışır ve yerel olarak değildir. Ön betikler güncelleştirme dağıtımının başlangıcında çalışır. Post betikleri dağıtımın sonunda ve yapılandırılan tüm yeniden başlatmalar sonrasında çalışır.

## <a name="runbook-requirements"></a>Runbook gereksinimleri

Runbook 'un ön veya gönderi betiği olarak kullanılabilmesi için Runbook 'un Otomasyon hesabınıza aktarılması ve yayımlanması gerekir. Bu işlem hakkında daha fazla bilgi edinmek için bkz. [runbook yayımlama](manage-runbooks.md#publish-a-runbook).

## <a name="using-a-prepost-script"></a>Bir ön/son betik kullanma

Bir güncelleştirme dağıtımında bir ön ve veya post betiği kullanmak için, bir güncelleştirme dağıtımı oluşturarak başlayın. **Betikler öncesi ve post betikleri**' ni seçin. Bu eylem, **betikleri ön betikleri Seç + betikleri sonrası** sayfasını açar.

![Betikleri seçin](./media/pre-post-scripts/select-scripts.png)

Kullanmak istediğiniz betiği seçin, bu örnekte **updatemanagement-TurnOnVms** runbook 'unu kullandınız. Runbook 'u seçtiğinizde **betiği Yapılandır** sayfası açıldığında, **ön betik**' ı seçin. Bittiğinde **Tamam** ' a tıklayın.

**Updatemanagement-TurnOffVms** betiği için bu işlemi tekrarlayın. Ancak **komut dosyası türünü**seçerken, **Post-SCRIPT**' ı seçin.

**Seçili öğeler** bölümü, her iki betiğin de seçili ve açık bir ön betik olduğunu ve diğeri de bir komut dosyası olduğunu gösterir.

![Seçili öğeler](./media/pre-post-scripts/selected-items.png)

Güncelleştirme dağıtımınızı yapılandırmayı tamamlama.

Güncelleştirme dağıtımınız tamamlandığında, sonuçları görüntülemek için **güncelleştirme dağıtımları** ' na gidebilirsiniz. Gördüğünüz gibi, ön betik ve son betik durumu sağlanır.

![Sonuçları Güncelleştir](./media/pre-post-scripts/update-results.png)

Güncelleştirme dağıtımı çalıştırmasına tıklayarak, öncesi ve sonrası betiklerine ek ayrıntılar sunuyoruz. Çalıştırma sırasında betik kaynağına bir bağlantı sağlanır.

![Dağıtım çalıştırma sonuçları](./media/pre-post-scripts/deployment-run.png)

## <a name="passing-parameters"></a>Parametreleri geçirme

Ön ve son betikleri yapılandırdığınızda, bir runbook 'u zamanlama gibi parametreleri geçirebilirsiniz. Parametreler, güncelleştirme dağıtımı oluşturma sırasında tanımlanmıştır. Ön ve son betikler aşağıdaki türleri destekler:

* Char
* bayt
* 'tir
* kalacağını
* Kategori
* sunuculu
* Çift
* Hem
* dizisinde

Başka bir nesne türüne ihtiyacınız varsa, runbook 'ta kendi mantığınızla başka bir türe çevirebilirsiniz.

Standart runbook parametrelerinizin yanı sıra ek bir parametre sağlanır. Bu parametre, **Softwareupdateconfigurationruncontext**' dir. Bu parametre bir JSON dizesidir ve bir parametreyi ön veya gönderi betiğinizdeki olarak tanımlarsanız, güncelleştirme dağıtımı tarafından otomatik olarak geçirilir. Parametresi, [Softwareupdateconfigurations API](/rest/api/automation/softwareupdateconfigurations/getbyname#updateconfiguration) tarafından döndürülen bilgilerin bir alt kümesi olan güncelleştirme dağıtımı hakkında bilgiler içerir aşağıdaki tabloda, değişkende sunulan özellikler gösterilmektedir:


### <a name="softwareupdateconfigurationruncontext-properties"></a>SoftwareUpdateConfigurationRunContext özellikleri

|Özellik  |Açıklama  |
|---------|---------|
|SoftwareUpdateConfigurationName     | Yazılım güncelleştirme yapılandırmasının adı        |
|Softwareupdateconfigurationrunıd     | Çalıştırmanın benzersiz KIMLIĞI.        |
|SoftwareUpdateConfigurationSettings     | Yazılım güncelleştirme yapılandırmasıyla ilgili özellikler koleksiyonu         |
|SoftwareUpdateConfigurationSettings. operatingSystem     | Güncelleştirme dağıtımı için hedeflenen işletim sistemleri         |
|SoftwareUpdateConfigurationSettings. Duration     | Güncelleştirme dağıtımının en uzun süresi, ıSO8601 başına `PT[n]H[n]M[n]S` olarak çalışır ve "bakım penceresi" olarak da adlandırılır          |
|SoftwareUpdateConfigurationSettings. Windows     | Windows bilgisayarlarıyla ilgili özellikler koleksiyonu         |
|SoftwareUpdateConfigurationSettings. Windows. excludedKbNumbers     | Güncelleştirme dağıtımından dışlanan KBs 'lerin listesi        |
|SoftwareUpdateConfigurationSettings. Windows. ıncludedupdatesınıflandırmaları     | Güncelleştirme dağıtımı için seçilen sınıflandırmaları güncelleştirme        |
|SoftwareUpdateConfigurationSettings. Windows. rebootSetting     | Güncelleştirme dağıtımı için yeniden başlatma ayarları        |
|azureVirtualMachines     | Güncelleştirme dağıtımındaki Azure VM 'Leri için Resourceıds listesi        |
|nonAzureComputerNames|Güncelleştirme dağıtımında Azure olmayan bilgisayarların FQDN 'lerinin listesi|

Aşağıdaki örnek, **Softwareupdateconfigurationruncontext** parametresine GEÇIRILEN bir JSON dizesidir:

```json
"SoftwareUpdateConfigurationRunContext":{
      "SoftwareUpdateConfigurationName":"sampleConfiguration",
      "SoftwareUpdateConfigurationRunId":"00000000-0000-0000-0000-000000000000",
      "SoftwareUpdateConfigurationSettings":{
         "operatingSystem":"Windows",
         "duration":"PT2H0M",
         "windows":{
            "excludedKbNumbers":[
               "168934",
               "168973"
            ],
            "includedUpdateClassifications":"Critical",
            "rebootSetting":"IfRequired"
         },
         "azureVirtualMachines":[
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-01",
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-02",
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-03"
         ],
         "nonAzureComputerNames":[
            "box1.contoso.com",
            "box2.contoso.com"
         ]
      }
   }
```

Tüm özelliklere sahip tam bir örnek şurada bulunabilir: [yazılım güncelleştirme yapılandırması-ada göre Al](/rest/api/automation/softwareupdateconfigurations/getbyname#examples)

> [!NOTE]
> @No__t-0 nesnesi makineler için yinelenen girdiler içerebilir. Bu, ön ve son betiklerin aynı makinede birden fazla kez çalışmasına neden olabilir. Bu davranışa geçici bir çözüm olarak, komut dosyasında yalnızca benzersiz VM adlarını seçmek için `Sort-Object -Unique` kullanın.


## <a name="stopping-a-deployment"></a>Dağıtım durduruluyor

Bir ön koda dayalı bir dağıtımı durdurmak isterseniz, bir özel durum [oluşturmanız gerekir.](automation-runbook-execution.md#throw) Bir özel durum belirtmezseniz dağıtım ve gönderi betiği çalışmaya devam edecektir. Aşağıdaki kod parçacığı bir özel durum oluşturmayı gösterir.

```powershell
#In this case, we want to terminate the patch job if any run fails.
#This logic might not hold for all cases - you might want to allow success as long as at least 1 run succeeds
foreach($summary in $finalStatus)
{
    if ($summary.Type -eq "Error")
    {
        #We must throw in order to fail the patch deployment.
        throw $summary.Summary
    }
}
```

## <a name="samples"></a>Örnekler

Ön ve son betiklerin örnekleri, [betik Merkezi galerisinde](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=WindowsAzure&f%5B0%5D.Text=Windows%20Azure&f%5B1%5D.Type=SubCategory&f%5B1%5D.Value=WindowsAzure_automation&f%5B1%5D.Text=Automation&f%5B2%5D.Type=SearchText&f%5B2%5D.Value=update%20management&f%5B3%5D.Type=Tag&f%5B3%5D.Value=Patching&f%5B3%5D.Text=Patching&f%5B4%5D.Type=ProgrammingLanguage&f%5B4%5D.Value=PowerShell&f%5B4%5D.Text=PowerShell)bulunabilir, [PowerShell Galerisi](https://www.powershellgallery.com/packages?q=Tags%3A%22UpdateManagement%22+Tags%3A%22Automation%22)veya Azure Portal aracılığıyla içeri aktarılabilir. Portal üzerinden içeri aktarmak için, Otomasyon hesabınızda, **Işlem Otomasyonu**altında **runbook 'lar Galerisi**' ni seçin. Filtre için **güncelleştirme yönetimi** kullanın.

![Galeri listesi](./media/pre-post-scripts/runbook-gallery.png)

Ya da aşağıdaki listede görüldüğü gibi bunları komut dosyası adına göre de arayabilirsiniz:

* Güncelleştirme Yönetimi-VM 'Leri açma
* Güncelleştirme Yönetimi-VM 'Leri kapatma
* Güncelleştirme Yönetimi betiği yerel olarak çalıştır
* Ön/son betiklerin Güncelleştirme Yönetimi şablonu
* Güncelleştirme Yönetimi-Betiği Çalıştır komutuyla Çalıştır

> [!IMPORTANT]
> Runbook 'ları içeri aktardıktan sonra, kullanılmadan önce bunları **yayımlamanız** gerekir. Otomasyon hesabınızda runbook 'u bulun, **Düzenle**' yi seçin ve **Yayımla**' ya tıklayın.

Örnekler, aşağıdaki örnekte tanımlanan temel şablona göre belirlenir. Bu şablon, ön ve son betiklerle kullanmak üzere kendi runbook 'unuzu oluşturmak için kullanılabilir. Azure ile kimlik doğrulaması yapmak ve `SoftwareUpdateConfigurationRunContext` parametresini işlemek için gerekli mantık dahil edilmiştir.

```powershell
<#
.SYNOPSIS
 Barebones script for Update Management Pre/Post

.DESCRIPTION
  This script is intended to be run as a part of Update Management Pre/Post scripts.
  It requires a RunAs account.

.PARAMETER SoftwareUpdateConfigurationRunContext
  This is a system variable which is automatically passed in by Update Management during a deployment.
#>

param(
    [string]$SoftwareUpdateConfigurationRunContext
)
#region BoilerplateAuthentication
#This requires a RunAs account
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID
#endregion BoilerplateAuthentication

#If you wish to use the run context, it must be converted from JSON
$context = ConvertFrom-Json  $SoftwareUpdateConfigurationRunContext
#Access the properties of the SoftwareUpdateConfigurationRunContext
$vmIds = $context.SoftwareUpdateConfigurationSettings.AzureVirtualMachines | Sort-Object -Unique
$runId = $context.SoftwareUpdateConfigurationRunId

Write-Output $context

#Example: How to create and write to a variable using the pre-script:
<#
#Create variable named after this run so it can be retrieved
New-AzureRmAutomationVariable -ResourceGroupName $ResourceGroup –AutomationAccountName $AutomationAccount –Name $runId -Value "" –Encrypted $false
#Set value of variable
Set-AutomationVariable –Name $runId -Value $vmIds
#>

#Example: How to retrieve information from a variable set during the pre-script
<#
$variable = Get-AutomationVariable -Name $runId
#>
```

## <a name="interacting-with-machines"></a>Makinelerle etkileşim kurma

Ön ve son görevler, doğrudan dağıtımınızdaki makinelere değil, Otomasyon hesabınızda runbook olarak çalışır. Ön ve son görevler Azure bağlamında da çalışır ve Azure olmayan makinelere erişemez. Aşağıdaki bölümlerde, Azure VM veya Azure olmayan bir makine olmak üzere makinelerle doğrudan nasıl etkileşim kuracağınızı gösterilmektedir:

### <a name="interacting-with-azure-machines"></a>Azure makinelerle etkileşim kurma

Ön ve son görevler runbook olarak çalıştırılır ve dağıtımınızda Azure sanal makinelerinizdeki yerel olarak çalışmaz. Azure VM 'larınızla etkileşim kurmak için aşağıdaki öğelere sahip olmanız gerekir:

* Farklı Çalıştır hesabı
* Çalıştırmak istediğiniz runbook

Azure makinelerle etkileşim kurmak için [Invoke-AzureRmVMRunCommand](/powershell/module/azurerm.compute/invoke-azurermvmruncommand) cmdlet 'Ini kullanarak Azure VM 'larınızla etkileşim kurun. Bunu nasıl yapacağınızı gösteren bir örnek için, [Çalıştır komutuyla güncelleştirme yönetimi-Betiği Çalıştır komutuyla](https://gallery.technet.microsoft.com/Update-Management-Run-40f470dc)runbook örneği ' ne bakın.

### <a name="interacting-with-non-azure-machines"></a>Azure olmayan makinelerle etkileşim kurma

Ön ve son görevler Azure bağlamında çalışır ve Azure olmayan makinelere erişemez. Azure olmayan makinelerle etkileşim kurmak için aşağıdaki öğelere sahip olmanız gerekir:

* Farklı Çalıştır hesabı
* Makinede yüklü karma Runbook Worker
* Yerel olarak çalıştırmak istediğiniz runbook
* Üst runbook

Azure olmayan makinelerle etkileşim kurmak için Azure bağlamında bir üst runbook çalıştırılır. Bu runbook, [Start-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook) cmdlet 'ini içeren bir alt runbook 'u çağırır. @No__t-0 parametresini belirtmeniz ve betiğin üzerinde çalışacağı karma Runbook Worker adını sağlamanız gerekir. Bir örnek için bkz. runbook örneği [güncelleştirme yönetimi-betiği yerel olarak çalıştır](https://gallery.technet.microsoft.com/Update-Management-Run-6949cc44).

## <a name="abort-patch-deployment"></a>Düzeltme Eki dağıtımını durdur

Ön komutlarınız bir hata döndürürse, dağıtımınızı iptal etmek isteyebilirsiniz. Bunu yapmak için, komut dosyasında hata oluşturabilecek herhangi bir mantık için bir hata [oluşturmanız gerekir.](/powershell/module/microsoft.powershell.core/about/about_throw)

```powershell
if (<My custom error logic>)
{
    #Throw an error to fail the patch deployment.
    throw "There was an error, abort deployment"
}
```

## <a name="known-issues"></a>Bilinen sorunlar

* Ön ve son betik kullanımı sırasında parametrelere Boole, nesne veya dizi geçirilemez. Runbook başarısız olur. Desteklenen türlerin tüm listesi için bkz. [Parametreler](#passing-parameters).

## <a name="next-steps"></a>Sonraki adımlar

Windows sanal makineleriniz için güncelleştirmelerin nasıl yönetileceğini öğrenmek için öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Azure Windows VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme](automation-tutorial-update-management.md)

