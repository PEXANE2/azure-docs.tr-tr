---
title: Azure'daki Update Management dağıtımınızda komut öncesi ve sonrası komut dosyalarını yapılandırın
description: Bu makalede, güncelleştirme dağıtımları için ön komut dosyaları ve komut dosyaları sonrası yapılandırma ve yönetme nasıl açıklanmaktadır.
services: automation
ms.subservice: update-management
ms.date: 05/17/2019
ms.topic: conceptual
ms.openlocfilehash: 35fba966fcdb6d1c5cd7c531bb22c9c78ae16ff3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75417797"
---
# <a name="manage-pre-and-post-scripts"></a>Komut öncesi ve sonrası komut dosyalarını yönetme

Ön komut dosyaları ve komut dosyaları sonrası komut dosyaları, PowerShell runbook'larını Azure Otomasyon hesabınızda bir güncelleştirme dağıtımından önce (görev öncesi) ve sonrasında çalıştırmanızı sağlar. Ön ve post-scriptler yerel olarak değil, Azure bağlamında çalışır. Ön komut dosyaları güncelleştirme dağıtımının başında çalışır. Komut dosyaları sonrası, dağıtımın sonunda ve yapılandırılan yeniden başlatmalardan sonra çalışır.

## <a name="runbook-requirements"></a>Runbook gereksinimleri

Bir runbook'un komut dosyası öncesi veya sonrası olarak kullanılabilmesi için, runbook'un Otomasyon hesabınıza aktarılması ve yayınlanması gerekir. Bu işlem hakkında daha fazla bilgi edinmek için [bkz.](manage-runbooks.md#publish-a-runbook)

## <a name="using-a-pre-script-or-post-script"></a>Ön komut dosyası veya post-script kullanma

Güncelleştirme dağıtımında ön komut dosyası veya komut dosyası sonrası kullanmak için, güncelleştirme dağıtımı oluşturarak başlayın. **Ön Komut Dosyaları + Post-Scripts'i**seçin. Bu eylem, **Ön Komut Dosyalarını Seç + Komut Dosyası Sonrası** sayfasını açar.

![Komut dosyalarını seçin](./media/pre-post-scripts/select-scripts.png)

Kullanmak istediğiniz komut dosyasını seçin. Bu örnekte, **UpdateManagement-TurnOnVms** runbook'u kullanıyoruz. Runbook'u seçtiğinizde, **Komut Dosyasını Yapılandır** sayfası açılır. **Ön Komut Dosyası'nı**seçin ve ardından **Tamam'ı**seçin.

**UpdateManagement-TurnOffVms** komut dosyası için bu işlemi yineleyin. Ancak **Komut Dosyası türünü**seçtiğinizde, **Post-Script'i**seçin.

**Seçili öğeler** bölümü artık seçilen her iki komut dosyanızı da gösterir. Biri ön-komut dosyası, diğeri ise post-script:

![Seçili öğeler](./media/pre-post-scripts/selected-items.png)

Güncelleştirme dağıtımınızı yapılandırmayı tamamla.

Güncelleştirme dağıtımınız tamamlandığında, sonuçları görüntülemek için **dağıtımları güncelleştir'e** gidebilirsiniz. Gördüğünüz gibi, durum ön komut dosyası ve post-script için sağlanır:

![Sonuçları güncelleştirme](./media/pre-post-scripts/update-results.png)

Güncelleştirme dağıtım çalışmasını seçerek, komut dosyası öncesi ve sonrası için ek ayrıntılar gösterilir. Çalıştırma sırasında komut dosyası kaynağına bir bağlantı sağlanır.

![Dağıtım çalıştırma sonuçları](./media/pre-post-scripts/deployment-run.png)

## <a name="passing-parameters"></a>Parametreleri geçirme

Komut dosyası öncesi ve sonrası yapılandırdığınızda, runbook planlama gibi parametreleri geçirebilirsiniz. Parametreler güncelleştirme dağıtım oluşturma sırasında tanımlanır. Ön ve post-scripts aşağıdaki türleri destekler:

* [char]
* [bayt]
* [int]
* [uzun]
* [ondalık]
* [tek]
* [çift]
* [DateTime]
* [dize]

Başka bir nesne türüne ihtiyacınız varsa, runbook'ta kendi mantığınızla başka bir türe atabilirsiniz.

Standart runbook parametrelerinize ek olarak, başka bir parametre sağlanır: **SoftwareUpdateConfigurationRunContext**

Bu parametre bir JSON dizesidir ve komut dosyası öncesi veya sonrası parametrenizi tanımlarsanız, bu parametre güncelleştirme dağıtımı tarafından otomatik olarak geçirilir. Parametre, [SoftwareUpdateconfigurations API](/rest/api/automation/softwareupdateconfigurations/getbyname#updateconfiguration)tarafından döndürülen bilgilerin bir alt kümesi olan güncelleştirme dağıtımı hakkında bilgi içerir. 

Aşağıdaki tablo, değişkende sağlanan özellikleri gösterir.

### <a name="softwareupdateconfigurationruncontext-properties"></a>SoftwareUpdateConfigurationRunContext özellikleri

|Özellik  |Açıklama  |
|---------|---------|
|YazılımUpdateConfigurationName     | Yazılım güncelleştirme yapılandırmasının adı.        |
|YazılımUpdateConfigurationRunid     | Çalışma için benzersiz kimlik.        |
|YazılımUpdateConfigurationSettings     | Yazılım güncelleştirme yapılandırması ile ilgili özellikler topluluğu.         |
|YazılımUpdateConfigurationSettings.operatingSystem     | Güncelleştirme dağıtımı için hedeflenen işletim sistemleri.         |
|YazılımUpdateConfigurationSettings.duration     | ISO8601 uyarınca `PT[n]H[n]M[n]S` güncelleştirme dağıtımının maksimum süresi; *bakım penceresi*olarak da adlandırılır.          |
|YazılımUpdateConfigurationSettings.Windows     | Windows bilgisayarlarla ilgili özellikler topluluğu.         |
|SoftwareUpdateConfigurationSettings.Windows.excludedKbNumbers     | Güncelleştirme dağıtımının dışında olan KB'lerin listesi.        |
|SoftwareUpdateConfigurationSettings.Windows.includedUpdateClassifications     | Güncelleştirme dağıtımı için seçilen sınıflandırmaları güncelleştirin.        |
|SoftwareUpdateConfigurationSettings.Windows.rebootSetting     | Güncelleştirme dağıtımı için ayarları yeniden başlatın.        |
|azureVirtualMachines     | Güncelleştirme dağıtımındaki Azure VM'leri için kaynak Id'lerinin listesi.        |
|olmayan AzureComputerNames|Güncelleştirme dağıtımındaki Azure olmayan bilgisayarların FQDN'lerinin listesi.|

Aşağıdaki örnek, **SoftwareUpdateConfigurationRunContext** parametresine geçirilen bir JSON dizesidir:

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

Tüm özellikleri ile tam bir örnek bulunabilir: [Adıyla yazılım güncelleştirme yapılandırmaalın.](/rest/api/automation/softwareupdateconfigurations/getbyname#examples)

> [!NOTE]
> Nesne, `SoftwareUpdateConfigurationRunContext` makineler için yinelenen girişler içerebilir. Bu, komut öncesi ve sonrası komut dosyalarının aynı makinede birden çok kez çalışmasına neden olabilir. Bu davranışı n için `Sort-Object -Unique` komut dosyanızda yalnızca benzersiz VM adlarını seçmek için kullanın.


## <a name="stopping-a-deployment"></a>Dağıtımı durdurma

Bir ön komut dosyasına dayalı bir dağıtımı durdurmak istiyorsanız, bir özel durum [atmanız](automation-runbook-execution.md#throw) gerekir. Bunu yapmazsanız, dağıtım ve komut dosyası sonrası yine de çalışır. Aşağıdaki kod parçacığı, bir özel durum atmanın nasıl yapılacağını gösterir.

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

Komut dosyası öncesi ve sonrası örnekler Script Center Gallery ve [PowerShell](https://www.powershellgallery.com/packages?q=Tags%3A%22UpdateManagement%22+Tags%3A%22Automation%22) [Galerisi'nde](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=WindowsAzure&f%5B0%5D.Text=Windows%20Azure&f%5B1%5D.Type=SubCategory&f%5B1%5D.Value=WindowsAzure_automation&f%5B1%5D.Text=Automation&f%5B2%5D.Type=SearchText&f%5B2%5D.Value=update%20management&f%5B3%5D.Type=Tag&f%5B3%5D.Value=Patching&f%5B3%5D.Text=Patching&f%5B4%5D.Type=ProgrammingLanguage&f%5B4%5D.Value=PowerShell&f%5B4%5D.Text=PowerShell) bulunabilir veya bunları Azure portalı üzerinden içe aktarabilirsiniz. Bunu yapmak için, Otomasyon hesabınızda, **Proses Otomasyonu**altında **Runbooks Gallery'yi**seçin. Filtre için **Güncelleştirme Yönetimi'ni** kullanın.

![Galeri listesi](./media/pre-post-scripts/runbook-gallery.png)

Ya da aşağıdaki listede gösterildiği gibi, komut dosyası adlarıyla onları arayabilirsiniz:

* Yönetimi Güncelle - VM'leri Aç
* Yönetim güncelleme - VM'leri Kapat
* Yönetim'i Güncelleştir - Script'i Yerel Olarak Çalıştır
* Güncelleme Yönetimi - Ön/Posta Komut Dosyaları Şablonu
* Yönetim güncelleme - Run Komutu ile Komut Çalıştır

> [!IMPORTANT]
> Runbook'ları aldıktan sonra, kullanılmadan önce bunları yayımlamanız gerekir. Bunu yapmak için, Otomasyon hesabınızda runbook'u bulun, **Edit'i**seçin ve ardından **Yayımla'yı**seçin.

Örneklerin tümü, aşağıdaki örnekte tanımlanan temel şablona dayanır. Bu şablon, komut öncesi ve sonrası komut dosyalarıyla kullanmak üzere kendi runbook'unuzu oluşturmak için kullanılabilir. Azure ile kimlik doğrulaması ve `SoftwareUpdateConfigurationRunContext` parametreyi işlemek için gerekli mantık dahildir.

```powershell
<#
.SYNOPSIS
 Barebones script for Update Management Pre/Post

.DESCRIPTION
  This script is intended to be run as a part of Update Management pre/post-scripts.
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

## <a name="interacting-with-machines"></a>Makinelerle etkileşim

Ön ve sonrası görevler, doğrudan dağıtımınızdaki makinelerde değil, Otomasyon hesabınızda bir runbook olarak çalışır. Ön ve sonrası görevler de Azure bağlamında çalışır ve Azure olmayan makinelere erişimi yoktur. Aşağıdaki bölümler, azure vm'leri veya Azure olmayan makineler olsun, makinelerle doğrudan nasıl etkileşimkurabileceğinizi gösterir.

### <a name="interacting-with-azure-machines"></a>Azure makineleriyle etkileşim

Ön ve sonrası görevler runbook olarak çalıştırılır ve dağıtımınızda Azure VM'lerinizde yerel olarak çalışmaz. Azure VM'lerinizle etkileşimde kalmak için aşağıdaki öğelere sahip olmalısınız:

* Bir Run As hesabı
* Çalıştırmak istediğiniz bir runbook

Azure makineleriyle etkileşimde kalmak için Azure VM'lerinizle etkileşimde kalmak için [Invoke-AzureRmVMRunKomut](/powershell/module/azurerm.compute/invoke-azurermvmruncommand) cmdlet'ini kullanmanız gerekir. Bunun nasıl yapılacağının bir örneği için runbook örneği [Update Management – Komut dosyasını Çalıştır komutuyla çalıştırın.](https://gallery.technet.microsoft.com/Update-Management-Run-40f470dc)

### <a name="interacting-with-non-azure-machines"></a>Azure olmayan makinelerle etkileşim kurma

Ön ve sonrası görevler Azure bağlamında çalışır ve Azure olmayan makinelere erişimi yoktur. Azure olmayan makinelerle etkileşimde kalmak için aşağıdaki öğelere sahip olmalısınız:

* Bir Run As hesabı
* Hibrid Runbook Worker makineye yüklendi
* Yerel olarak çalıştırmak istediğiniz bir runbook
* Bir üst çalışma kitabı

Azure olmayan makinelerle etkileşimkurmak için bir üst çalışma kitabı Azure bağlamında çalıştırılır. Bu runbook [Başlat-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook) cmdlet ile bir çocuk runbook çağırır. Komut dosyasının `-RunOn` çalışması için parametreyi belirtmeniz ve Karma Runbook Worker'ın adını sağlamanız gerekir. Daha fazla bilgi için runbook örneği Update Management 'a bakın [– komut dosyasını yerel olarak çalıştırın.](https://gallery.technet.microsoft.com/Update-Management-Run-6949cc44)

## <a name="abort-patch-deployment"></a>Düzeltme bandı dağıtımını iptal etme

Komut dosyanız bir hata döndürürse, dağıtımınızı iptal etmek isteyebilirsiniz. Bunu yapmak için, hata oluşturacak herhangi bir mantık için komut dosyanızda bir hata [atmanız](/powershell/module/microsoft.powershell.core/about/about_throw) gerekir.

```powershell
if (<My custom error logic>)
{
    #Throw an error to fail the patch deployment.
    throw "There was an error, abort deployment"
}
```

## <a name="known-issues"></a>Bilinen sorunlar

* Komut öncesi ve sonrası komut dosyalarını kullanırken boolean, nesneler veya dizileri parametrelere geçiremezsiniz. Bunu yaparsanız, runbook başarısız olur. Desteklenen türlerin tam listesi için [geç-iş parametrelerine](#passing-parameters)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Windows sanal makineleriniz için güncelleştirmeleri nasıl yöneteceklerini öğrenmek için aşağıdaki öğreticiye gidin:

> [!div class="nextstepaction"]
> [Azure Windows sanal makineleriniz için güncelleştirme ve düzeltme eki yönetimi](automation-tutorial-update-management.md)

