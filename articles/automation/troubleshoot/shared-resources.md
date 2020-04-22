---
title: Azure Otomasyonu'nda paylaşılan kaynakları sorun giderme
description: Azure Automation paylaşılan kaynaklarla sorunları nasıl gidereceğinizi ve gidereceğinizi öğrenin.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 03/12/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: e83c7074d252083329537e205666374705a31873
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81733570"
---
# <a name="troubleshoot-shared-resources-in-azure-automation"></a>Azure Otomasyonu'nda paylaşılan kaynakları sorun giderme

Bu makalede, Azure Otomasyonu'nda paylaşılan [kaynakları](../automation-intro.md#shared-resources) kullanırken karşılaşabileceğiniz sorunlara yönelik çözümler açıklanmaktadır.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](../automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="modules"></a>Modüller

### <a name="scenario-a-module-is-stuck-during-import"></a><a name="module-stuck-importing"></a>Senaryo: Bir modül alma sırasında sıkışmış

#### <a name="issue"></a>Sorun

Azure Otomasyon modüllerinizi içe aktarırken veya güncellerken bir modül Içe Aktarma durumunda takılır.

#### <a name="cause"></a>Nedeni

PowerShell modüllerini içe aktarma karmaşık bir çok adımlı işlem olduğundan, bir modül doğru içe aktarılamayabilir ve geçici bir durumda sıkışıp kalabilir. Alma işlemi hakkında daha fazla bilgi edinmek için [PowerShell Modülü Alma'ya](/powershell/scripting/developer/module/importing-a-powershell-module#the-importing-process)bakın.

#### <a name="resolution"></a>Çözüm

Bu sorunu gidermek [için, Kaldırma-AzAutomationModülü](https://docs.microsoft.com/powershell/module/Az.Automation/Remove-AzAutomationModule?view=azps-3.7.0) cmdlet kullanarak Alma durumunda sıkışmış modülü kaldırmanız gerekir. Daha sonra modülü yeniden içe aktarmayı deneyebilirsiniz.

```azurepowershell-interactive
Remove-AzAutomationModule -Name ModuleName -ResourceGroupName ExampleResourceGroup -AutomationAccountName ExampleAutomationAccount -Force
```

### <a name="scenario-azurerm-modules-are-stuck-during-import-after-an-update-attempt"></a><a name="update-azure-modules-importing"></a>Senaryo: AzureRM modülleri bir güncelleştirme girişiminden sonra içe aktarma sırasında takılıp kalır

#### <a name="issue"></a>Sorun

AzureRM modüllerinizi güncelleştirmeye çalıştıktan sonra aşağıdaki iletiyi içeren bir başlık hesabınızda kalır:

```error
Azure modules are being updated
```

#### <a name="cause"></a>Nedeni

AzureRM modüllerini 0 ile başlayan sayısal bir ada sahip bir kaynak grubunda bulunan bir Otomasyon hesabında güncelleştirmeyle ilgili bilinen bir sorun vardır.

#### <a name="resolution"></a>Çözüm

AzureRM modüllerinizi Otomasyon hesabınızda güncellemek için hesabın alfasayısal bir ada sahip bir kaynak grubunda olması gerekir. 0 ile başlayan sayısal adlara sahip kaynak grupları şu anda AzureRM modüllerini güncelleştiremiyor.

### <a name="scenario-module-fails-to-import-or-cmdlets-cant-be-executed-after-importing"></a><a name="module-fails-to-import"></a>Senaryo: Modül içe aktarılmaz veya cmdlets içe aktarDıktan sonra yürütülemez

#### <a name="issue"></a>Sorun

Bir modül içe aktarılmaz veya başarılı bir şekilde içe aktarılır, ancak cmdlet ayıklanır.

#### <a name="cause"></a>Nedeni

Bir modülün Azure Otomasyonu'na başarıyla aktarılamayabilmek için sık karşılaşılan bazı nedenler şunlardır:

* Yapı, Otomasyon'un ihtiyaç duyduğu yapıyla eşleşmiyor.
* Modül, Otomasyon hesabınıza dağıtılmamış başka bir modüle bağlıdır.
* Modül klasördeki bağımlılıkları eksik.
* [Yeni-AzAutomationModule](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationModule?view=azps-3.7.0) cmdlet modülü yüklemek için kullanılıyor ve tam depolama yolu vermediniz veya genel olarak erişilebilir bir URL kullanarak modülü yüklemedin.

#### <a name="resolution"></a>Çözüm

Sorunu gidermek için bu çözümlerden herhangi birini kullanın.

* Modülün şu formata sahip olduğundan emin olun: ModuleName.zip -> ModuleName veya Sürüm Numarası -> (ModuleName.psm1, ModuleName.psd1).
* **.psd1** dosyasını açın ve modülün herhangi bir bağımlılığı olup olmadığını görün. Varsa, bu modülleri Otomasyon hesabına yükleyin.
* Modül klasöründe başvurulan **.dll** dosyalarının bulunduğundan emin olun.

### <a name="scenario-update-azuremoduleps1-suspends-when-updating-modules"></a><a name="all-modules-suspended"></a>Senaryo: Güncelleme-AzureModule.ps1 modülleri güncellerken askıya alır

#### <a name="issue"></a>Sorun

Azure modüllerinizi güncellemek için [Update-AzureModule.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1) runbook'u kullanırken, modül güncelleştirme işlemi askıya alınır.

#### <a name="cause"></a>Nedeni

**Güncelleştirme-AzureModule.ps1'i**kullanırken aynı anda kaç modülün güncelleştirilebildiğini belirlemek için varsayılan ayar 10'dur. Güncelleştirme işlemi, aynı anda çok fazla modül güncelleştirildiğinde hatalara açıktır.

#### <a name="resolution"></a>Çözüm

Tüm AzureRM veya Az modüllerinin aynı Otomasyon hesabında gerekli olması yaygın değildir. Yalnızca ihtiyacınız olan belirli modülleri almanız önerilir.

> [!NOTE]
> Tüm içerdiği modülleri içe aktaran tüm `Az.Automation` veya `AzureRM.Automation` modülü almaktan kaçının.

Güncelleştirme işlemi askıya alırsa, `SimultaneousModuleImportJobCount` Parametreyi **Update-AzureModules.ps1** komut dosyasına ekleyin ve 10 varsayılanından daha düşük bir değer sağlayın. Bu mantığı uygularsanız, 3 veya 5 değeriyle başlamanız önerilir. `SimultaneousModuleImportJobCount`Azure modüllerini güncelleştirmek için kullanılan **Update-AutomationAzureModulesForAccount** sistem runbook'unun bir parametresidir. Bu ayarlamayı yaparsanız, güncelleştirme işlemi daha uzun çalışır, ancak tamamlama şansı daha yüksektir. Aşağıdaki örnek, parametreyi ve runbook'a nereye koyacağız gösterir:

 ```powershell
         $Body = @"
            {
               "properties":{
               "runbook":{
                   "name":"Update-AutomationAzureModulesForAccount"
               },
               "parameters":{
                    ...
                    "SimultaneousModuleImportJobCount":"3",
                    ... 
               }
              }
           }
"@
```

## <a name="run-as-accounts"></a>Farklı Çalıştır hesapları

### <a name="scenario-youre-unable-to-create-or-update-a-run-as-account"></a><a name="unable-create-update"></a>Senaryo: Run As hesabı oluşturamıyor veya güncelleştiremiyorsunuz

#### <a name="issue"></a>Sorun

Bir Run As hesabı oluşturmaya veya güncelleştirmeye çalıştığınızda, aşağıdaki hata iletisine benzer bir hata alırsınız:

```error
You do not have permissions to create…
```

#### <a name="cause"></a>Nedeni

Run As hesabı oluşturmak veya güncelleştirmek için gereken izinlere sahip değilsiniz veya kaynak bir kaynak grubu düzeyinde kilitlenir.

#### <a name="resolution"></a>Çözüm

Bir Run As hesabı oluşturmak veya güncelleştirmek için, Run As hesabı tarafından kullanılan çeşitli kaynaklara uygun [izinlere](../manage-runas-account.md#permissions) sahip olmalısınız. 

Sorun bir kilit yüzündense, kilidin kaldırılabildiğinden doğrulayın. Ardından Azure portalında kilitli olan kaynağa gidin, kilidi sağ tıklatın ve **Sil'i**tıklatın.

### <a name="scenario-you-receive-the-error-unable-to-find-an-entry-point-named-getperadapterinfo-in-dll-iplpapidll-when-executing-a-runbook"></a><a name="iphelper"></a>Senaryo: Bir runbook yürürken DLL 'iplpapi.dll'de 'GetPerAdapterInfo' adlı bir giriş noktası bulamıyor" hatasını alırsınız

#### <a name="issue"></a>Sorun

Bir runbook çalıştırırken aşağıdaki özel durum alırsınız:

```error
Unable to find an entry point named 'GetPerAdapterInfo' in DLL 'iplpapi.dll'
```

#### <a name="cause"></a>Nedeni

Bu hata büyük olasılıkla yanlış yapılandırılmış bir [Run As Hesabı'ndan](../manage-runas-account.md)kaynaklanır.

#### <a name="resolution"></a>Çözüm

Run As hesabınızın düzgün şekilde yapılandırıldığından emin olun. Ardından, çalışma kitabınızda Azure ile kimlik doğrulaması yapmak için uygun koda sahip olduğunuzu doğrulayın. Aşağıdaki örnekte, Bir Run As Hesabı kullanarak bir runbook'ta Azure'a doğrulanması gereken bir kod parçası gösterilmektedir.

```powershell
$connection = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzAccount -ServicePrincipal -Tenant $connection.TenantID `
-ApplicationID $connection.ApplicationID -CertificateThumbprint $connection.CertificateThumbprint
```

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu yukarıda görmüyorsanız veya sorununuzu çözemiyorsanız, ek destek için aşağıdaki kanallardan birini deneyin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıt alın.
* [@AzureSupport](https://twitter.com/azuresupport)Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.
* Azure destek olayı dosyala. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.
