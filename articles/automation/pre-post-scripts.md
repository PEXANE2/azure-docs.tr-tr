---
title: Azure'daki Güncelleme Yönetimi dağıtımınızda ön komut dosyalarını ve komut dosyaları sonrası komut dosyalarını yönetme
description: Bu makalede, güncelleştirme dağıtımları için ön komut dosyaları ve komut dosyaları sonrası yapılandırma ve yönetme nasıl açıklanmaktadır.
services: automation
ms.subservice: update-management
ms.date: 05/17/2019
ms.topic: conceptual
ms.openlocfilehash: 00cde5255f9c9a2baa7c7042ae2a8f73448da0ae
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81679986"
---
# <a name="manage-pre-scripts-and-post-scripts"></a>Ön komut dosyalarını ve komut sonrası komut dosyalarını yönetme

Ön komut dosyaları ve post-scriptler, bir güncelleştirme dağıtımından önce (görev öncesi) ve sonrasında (görev sonrası) Azure Otomasyon hesabınızda çalıştırılan runbook'lardır. Ön komut dosyaları ve komut dosyaları yerel olarak değil, Azure bağlamında çalışır. Ön komut dosyaları güncelleştirme dağıtımının başında çalışır. Komut dosyaları sonrası, dağıtımın sonunda ve yapılandırılan yeniden başlatmalardan sonra çalışır.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="pre-script-and-post-script-requirements"></a>Komut öncesi ve komut dosyası sonrası gereksinimler

Bir runbook'un ön komut dosyası veya komut dosyası sonrası olarak kullanılabilmesi için, kitabı Otomasyon hesabınıza aktarmanız ve [runbook'u yayımlamanız](manage-runbooks.md#publishing-a-runbook)gerekir.

## <a name="pre-script-and-post-script-parameters"></a>Komut öncesi ve komut dosyası sonrası parametreler

Ön komut dosyalarını ve komut dosyaları sonrası yapılandırırken, runbook planlama gibi parametreleri geçirebilirsiniz. Parametreler güncelleştirme dağıtım oluşturma sırasında tanımlanır. Ön komut dosyaları ve post-scripts aşağıdaki türleri destekler:

* [char]
* [bayt]
* [int]
* [uzun]
* [ondalık]
* [tek]
* [çift]
* [DateTime]
* [dize]

Komut dosyası öncesi ve komut dosyası sonrası runbook parametreleri boolean, nesne veya dizi türlerini desteklemez. Bu değerler runbook'ların başarısız olması yla ilgili. 

Başka bir nesne türüne ihtiyacınız varsa, runbook'ta kendi mantığınızla başka bir türe atabilirsiniz.

Standart runbook parametrelerinize ek `SoftwareUpdateConfigurationRunContext` olarak parametre (JSON dizesi tipi) sağlanır. Komut dosyası öncesi veya komut dosyası sonrası çalışma kitabınızdaki parametreyi tanımlarsanız, bu parametre güncelleştirme dağıtımı tarafından otomatik olarak geçirilir. Parametre, [SoftwareUpdateconfigurations API](/rest/api/automation/softwareupdateconfigurations/getbyname#updateconfiguration)tarafından döndürülen bilgilerin bir alt kümesi olan güncelleştirme dağıtımı hakkında bilgi içerir. Aşağıdaki bölümlerilişkili özellikleri tanımlar.

### <a name="softwareupdateconfigurationruncontext-properties"></a>SoftwareUpdateConfigurationRunContext özellikleri

|Özellik  |Açıklama  |
|---------|---------|
|YazılımUpdateConfigurationName     | Yazılım güncelleştirme yapılandırmasının adı.        |
|YazılımUpdateConfigurationRunid     | Çalışma için benzersiz kimlik.        |
|YazılımUpdateConfigurationSettings     | Yazılım güncelleştirme yapılandırması ile ilgili özellikler topluluğu.         |
|YazılımUpdateConfigurationSettings.operatingSystem     | Güncelleştirme dağıtımı için hedeflenen işletim sistemleri.         |
|YazılımUpdateConfigurationSettings.duration     | ISO8601 uyarınca `PT[n]H[n]M[n]S` güncelleştirme dağıtımının maksimum süresi; bakım penceresi olarak da adlandırılır.          |
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
> Nesne, `SoftwareUpdateConfigurationRunContext` makineler için yinelenen girişler içerebilir. Bu, ön komut dosyalarının ve komut dosyalarının aynı makinede birden çok kez çalışmasına neden olabilir. Bu davranışı n için `Sort-Object -Unique` yalnızca benzersiz VM adlarını seçmek için kullanın.

## <a name="using-a-pre-script-or-post-script-in-a-deployment"></a>Dağıtımda ön komut dosyası veya post-script kullanma

Güncelleştirme dağıtımında ön komut dosyası veya komut dosyası sonrası kullanmak için, güncelleştirme dağıtımı oluşturarak başlayın. **Ön Komut Dosyaları + Post-Scripts'i**seçin. Bu eylem, **Ön Komut Dosyalarını Seç + Komut Dosyası Sonrası** sayfasını açar.

![Komut dosyalarını seçin](./media/pre-post-scripts/select-scripts.png)

Kullanmak istediğiniz komut dosyasını seçin. Bu örnekte, **UpdateManagement-TurnOnVms** runbook'u kullanıyoruz. Runbook'u seçtiğinizde, **Komut Dosyasını Yapılandır** sayfası açılır. **Ön Komut Dosyası'nı**seçin ve ardından **Tamam'ı**seçin.

**UpdateManagement-TurnOffVms** komut dosyası için bu işlemi yineleyin. Ancak **Komut Dosyası türünü**seçtiğinizde, **Post-Script'i**seçin.

**Seçili öğeler** bölümü artık seçilen her iki komut dosyanızı da gösterir. Biri ön-komut dosyası, diğeri ise post-script:

![Seçili öğeler](./media/pre-post-scripts/selected-items.png)

Güncelleştirme dağıtımınızı yapılandırmayı tamamla.

Güncelleştirme dağıtımınız tamamlandığında, sonuçları görüntülemek için **dağıtımları güncelleştir'e** gidebilirsiniz. Gördüğünüz gibi, durum ön komut dosyası ve post-script için sağlanır:

![Sonuçları güncelleştirme](./media/pre-post-scripts/update-results.png)

Güncelleştirme dağıtım çalışmasını seçerek, komut öncesi ve komut dosyası sonrası ek ayrıntılar gösterilir. Çalıştırma sırasında komut dosyası kaynağına bir bağlantı sağlanır.

![Dağıtım çalıştırma sonuçları](./media/pre-post-scripts/deployment-run.png)

komut dosyasında es.

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



## <a name="interacting-with-machines"></a>Makinelerle etkileşim

Ön komut dosyaları ve görev sonrası görevler, doğrudan dağıtımınızdaki makinelerde değil, Otomasyon hesabınızda runbook olarak çalışır. Ön görevler ve sonrası görevler de Azure bağlamında çalışır ve Azure olmayan makinelere erişimi yoktur. Aşağıdaki bölümler, azure vm'leri veya Azure olmayan makineler olsun, makinelerle doğrudan nasıl etkileşimkurabileceğinizi gösterir.

### <a name="interact-with-azure-machines"></a>Azure makineleriyle etkileşim

Ön görevler ve görev sonrası görevler runbook olarak çalışır ve dağıtımınızda Azure VM'lerinizde yerel olarak çalışmaz. Azure VM'lerinizle etkileşimde kalmak için aşağıdaki öğelere sahip olmalısınız:

* Bir Run As hesabı
* Çalıştırmak istediğiniz bir runbook

Azure makineleriyle etkileşimde kalmak için, Azure VM'lerinizle etkileşimde kalmak için [Invoke-AzVMRunKomut](https://docs.microsoft.com/powershell/module/az.compute/invoke-azvmruncommand?view=azps-3.7.0) cmdlet'ini kullanmanız gerekir. Bunun nasıl yapılacağının bir örneği için runbook örneği [Update Management – Komut dosyasını Çalıştır komutuyla çalıştırın.](https://gallery.technet.microsoft.com/Update-Management-Run-40f470dc)

### <a name="interact-with-non-azure-machines"></a>Azure olmayan makinelerle etkileşim

Ön görevler ve görev sonrası görevler Azure bağlamında çalışır ve Azure olmayan makinelere erişimi yoktur. Azure olmayan makinelerle etkileşimde kalmak için aşağıdaki öğelere sahip olmalısınız:

* Bir Run As hesabı
* Hibrid Runbook Worker makineye yüklendi
* Yerel olarak çalıştırmak istediğiniz bir runbook
* Bir üst çalışma kitabı

Azure olmayan makinelerle etkileşimkurmak için bir üst çalışma kitabı Azure bağlamında çalıştırılır. Bu runbook [Start-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/Az.Automation/Start-AzAutomationRunbook?view=azps-3.7.0) cmdlet ile bir çocuk runbook çağırır. Komut dosyasının `RunOn` çalışması için parametreyi belirtmeniz ve Karma Runbook Worker'ın adını sağlamanız gerekir. Runbook örneği [Update Management 'a bakın – komut dosyasını yerel olarak çalıştırın.](https://gallery.technet.microsoft.com/Update-Management-Run-6949cc44)

## <a name="aborting-patch-deployment"></a>Yama dağıtımını iptal etme

Komut dosyanız bir hata döndürürse, dağıtımınızı iptal etmek isteyebilirsiniz. Bunu yapmak için, hata oluşturacak herhangi bir mantık için komut dosyanızda bir hata [atmanız](/powershell/module/microsoft.powershell.core/about/about_throw) gerekir.

```powershell
if (<My custom error logic>)
{
    #Throw an error to fail the patch deployment.
    throw "There was an error, abort deployment"
}
```

## <a name="samples"></a>Örnekler

Komut öncesi ve post-scriptörnekleri Script Center Gallery ve [PowerShell](https://www.powershellgallery.com/packages?q=Tags%3A%22UpdateManagement%22+Tags%3A%22Automation%22) [Galerisi'nde](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=WindowsAzure&f%5B0%5D.Text=Windows%20Azure&f%5B1%5D.Type=SubCategory&f%5B1%5D.Value=WindowsAzure_automation&f%5B1%5D.Text=Automation&f%5B2%5D.Type=SearchText&f%5B2%5D.Value=update%20management&f%5B3%5D.Type=Tag&f%5B3%5D.Value=Patching&f%5B3%5D.Text=Patching&f%5B4%5D.Type=ProgrammingLanguage&f%5B4%5D.Value=PowerShell&f%5B4%5D.Text=PowerShell) bulunabilir veya bunları Azure portalı üzerinden içe aktarabilirsiniz. Bunu yapmak için, Otomasyon hesabınızda, **Proses Otomasyonu**altında **Runbooks Gallery'yi**seçin. Filtre için **Güncelleştirme Yönetimi'ni** kullanın.

![Galeri listesi](./media/pre-post-scripts/runbook-gallery.png)

Ya da aşağıdaki listede gösterildiği gibi, komut dosyası adlarıyla onları arayabilirsiniz:

* Yönetimi Güncelle - VM'leri Aç
* Yönetim güncelleme - VM'leri Kapat
* Yönetim'i Güncelleştir - Script'i Yerel Olarak Çalıştır
* Güncelleme Yönetimi - Ön/Posta Komut Dosyaları Şablonu
* Yönetim güncelleme - Run Komutu ile Komut Çalıştır

> [!IMPORTANT]
> Runbook'ları aldıktan sonra, kullanılmadan önce bunları yayımlamanız gerekir. Bunu yapmak için, Otomasyon hesabınızda runbook'u bulun, **Edit'i**seçin ve ardından **Yayımla'yı**seçin.

Örneklerin tümü, aşağıdaki örnekte tanımlanan temel şablona dayanır. Bu şablon, ön komut dosyaları ve komut dosyaları sonrası kullanmak üzere kendi runbook oluşturmak için kullanılabilir. Azure ile kimlik doğrulaması ve `SoftwareUpdateConfigurationRunContext` parametreyi işlemek için gerekli mantık dahildir.

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

Add-AzAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID
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
New-AzAutomationVariable -ResourceGroupName $ResourceGroup –AutomationAccountName $AutomationAccount –Name $runId -Value "" –Encrypted $false
#Set value of variable
Set-AutomationVariable –Name $runId -Value $vmIds
#>

#Example: How to retrieve information from a variable set during the pre-script
<#
$variable = Get-AutomationVariable -Name $runId
#>
```

> [!NOTE]
> Grafiksel olmayan PowerShell runbook'ları için `Add-AzAccount` ve `Add-AzureRMAccount` [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.5.0)için diğer adlardır. Bu cmdlets'i kullanabilir veya Otomasyon hesabınızdaki [modüllerinizi](automation-update-azure-modules.md) en son sürümlere güncelleyebilirsiniz. Yeni bir Otomasyon hesabı oluşturmuş olsanız bile modüllerinizi güncellemeniz gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

Windows sanal makineleriniz için güncelleştirmeleri nasıl yöneteceklerini öğrenmek için aşağıdaki öğreticiye gidin:

> [!div class="nextstepaction"]
> [Azure Windows sanal makineleriniz için güncelleştirme ve düzeltme eki yönetimi](automation-tutorial-update-management.md)