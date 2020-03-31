---
title: Azure Otomasyon paylaşılan kaynaklarla hataları giderme
description: Azure Automation paylaşılan kaynakları runbook'ları destekleyen sorunlarla ilgili sorunları nasıl gidereceğinizi ve çözeceğinizi öğrenin.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 03/12/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 4cea558b11d7ee7bbe838cecbd061cd487b536d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79278330"
---
# <a name="troubleshoot-errors-with-shared-resources"></a>Paylaşılan kaynaklarla hataları giderme

Bu makalede, Azure Otomasyonu'nda paylaşılan kaynakları kullanırken karşılaşabileceğiniz sorunları gidermek için çözümler açıklanmaktadır.

## <a name="modules"></a>Modules

### <a name="scenario-a-module-is-stuck-importing"></a><a name="module-stuck-importing"></a>Senaryo: Bir Modül içe aktarma sıkışmış

#### <a name="issue"></a>Sorun

Modüllerinizi Azure otomasyonunda içe aktarırken veya güncellediğinizde bir modül **Içe Aktarma** durumunda takılır.

#### <a name="cause"></a>Nedeni

PowerShell modüllerini almak karmaşık bir çok adımlı işlemdir. Bu işlem, bir modülün doğru içe aktarılmayabilme olasılığını ortaya konur. Bu sorun oluşursa, içe aktardığınız modül geçici bir durumda sıkışmış olabilir. Bu işlem hakkında daha fazla bilgi edinmek için [PowerShell Modülü Alma'ya](/powershell/scripting/developer/module/importing-a-powershell-module#the-importing-process)bakın.

#### <a name="resolution"></a>Çözüm

Bu sorunu gidermek için, [Kaldır-AzureRmAutomationModule](/powershell/module/azurerm.automation/remove-azurermautomationmodule) cmdlet kullanarak **Alma** durumunda sıkışmış modülü kaldırmanız gerekir. Daha sonra modülü yeniden içe aktarmayı deneyebilirsiniz.

```azurepowershell-interactive
Remove-AzureRmAutomationModule -Name ModuleName -ResourceGroupName ExampleResourceGroup -AutomationAccountName ExampleAutomationAccount -Force
```

### <a name="scenario-azurerm-modules-are-stuck-importing-after-trying-to-update-them"></a><a name="update-azure-modules-importing"></a>Senaryo: AzureRM modülleri güncelleştirmeye çalıştıktan sonra içe aktarmaya takılıp kalır

#### <a name="issue"></a>Sorun

AzureRM modüllerinizi güncelleştirmeye çalıştıktan sonra aşağıdaki iletiyi içeren bir başlık hesabınızda kalır:

```error
Azure modules are being updated
```

#### <a name="cause"></a>Nedeni

0 ile başlayan sayısal bir ada sahip bir kaynak grubunda bulunan bir Otomasyon HesabındaAzureRM modüllerinin güncelleştirilmesiyle ilgili bilinen bir sorun vardır.

#### <a name="resolution"></a>Çözüm

Azure modüllerinizi Otomasyon Hesabınızda güncelleştirmek için, alfasayısal bir ada sahip bir kaynak grubunda olması gerekir. 0 ile başlayan sayısal adlara sahip kaynak grupları şu anda AzureRM modüllerini güncelleştiremiyor.

### <a name="scenario-module-fails-to-import-or-cmdlets-cant-be-executed-after-importing"></a><a name="module-fails-to-import"></a>Senaryo: Modül içe aktarılmaz veya cmdlets içe aktarDıktan sonra yürütülemez

#### <a name="issue"></a>Sorun

Bir modül başarılı bir şekilde içe aktarılmaz veya içe aktarılmaz, ancak cmdlets ayıklanır.

#### <a name="cause"></a>Nedeni

Bir modülün Azure Otomasyonu'na başarıyla aktarılamayabilmek için sık karşılaşılan bazı nedenler şunlardır:

* Yapı, Otomasyon'un olması gereken yapıyla eşleşmiyor.
* Modül, Otomasyon hesabınıza dağıtılmamış başka bir modüle bağlıdır.
* Modül klasördeki bağımlılıkları eksik.
* Cmdlet `New-AzureRmAutomationModule` modülü yüklemek için kullanılıyor ve tüm depolama yolunu vermediniz veya herkese açık bir URL kullanarak modülü yüklemedin.

#### <a name="resolution"></a>Çözüm

Aşağıdaki çözümlerden herhangi biri sorunu giderir:

* Modülün aşağıdaki formata sahip olduğundan emin **->** olun: ModuleName.Zip ModuleName veya Sürüm Numarası **->** (ModuleName.psm1, ModuleName.psd1)
* .psd1 dosyasını açın ve modülün herhangi bir bağımlılığı olup olmadığını görün. Varsa, bu modülleri Otomasyon hesabına yükleyin.
* Modül klasöründe başvurulan .dll'lerin bulunduğundan emin olun.

### <a name="scenario-update-azuremoduleps1-suspends-when-updating-modules"></a><a name="all-modules-suspended"></a>Senaryo: Güncelleme-AzureModule.ps1 modülleri güncellerken askıya alır

#### <a name="issue"></a>Sorun

Azure modüllerinizi güncellemek için [Update-AzureModule.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1) runbook'u kullanırken modül güncelleme işlemini günceller.

#### <a name="cause"></a>Nedeni

`Update-AzureModule.ps1` Komut dosyasını kullanırken aynı anda kaç modülün güncelleştirilebildiğini belirlemek için varsayılan ayar 10'dur. Güncelleştirme işlemi, aynı anda çok fazla modül güncelleştirildiğinde hatalara açıktır.

#### <a name="resolution"></a>Çözüm

Tüm AzureRM modüllerinin aynı Otomasyon hesabında gerekli olması yaygın değildir. Yalnızca ihtiyacınız olan AzureRM modüllerini almanız önerilir.

> [!NOTE]
> **AzureRM** modüllerini almaktan kaçının. **AzureRM** modüllerinin içe aktarılması tüm **\* AzureRM.** modüllerinin içe alınmasına neden olur, bu yeniden biraraya gelmez.

Güncelleştirme işlemi askıya alırsa, `SimultaneousModuleImportJobCount` `Update-AzureModules.ps1` komut dosyasına parametre eklemeniz ve varsayılan değerden 10 daha düşük bir değer sağlamanız gerekir. Bu mantığı uygularsanız, 3 veya 5 değeriyle başlamak önerilir. `SimultaneousModuleImportJobCount`Azure modüllerini güncelleştirmek için kullanılan `Update-AutomationAzureModulesForAccount` sistem runbook'unun bir parametresi. Bu değişiklik işlemin daha uzun çalışmasını sağlar, ancak tamamlama şansı daha yüksektir. Aşağıdaki örnek, parametreyi ve runbook'a nereye koyacağız gösterir:

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

Bir Run As hesabı oluşturmak veya güncelleştirmek için, Run As hesabı tarafından kullanılan çeşitli kaynaklara uygun izinlere sahip olmalısınız. Run As hesabı oluşturmak veya güncelleştirmek için gereken izinler hakkında bilgi edinmek için, [bkz.](../manage-runas-account.md#permissions)

Sorun bir kilit yüzündense, kilidin kaldırmak için tamam olduğunu doğrulayın. Ardından kilitli olan kaynağa gidin, kilidi sağ tıklatın ve kilidi kaldırmak için **Sil'i** seçin.

### <a name="scenario-you-receive-the-error-unable-to-find-an-entry-point-named-getperadapterinfo-in-dll-iplpapidll-when-executing-a-runbook"></a><a name="iphelper"></a>Senaryo: Bir runbook yürürken DLL 'iplpapi.dll'de 'GetPerAdapterInfo' adlı bir giriş noktası bulamıyor" hatasını alırsınız.

#### <a name="issue"></a>Sorun

Bir runbook çalıştırırken aşağıdaki özel durum alırsınız:

```error
Unable to find an entry point named 'GetPerAdapterInfo' in DLL 'iplpapi.dll'
```

#### <a name="cause"></a>Nedeni

Bu hata büyük olasılıkla yanlış yapılandırılmış bir [Run As Hesabı'ndan](../manage-runas-account.md)kaynaklanır.

#### <a name="resolution"></a>Çözüm

[Run As Hesabınızın](../manage-runas-account.md) düzgün şekilde yapılandırıldığından emin olun. Doğru şekilde yapılandırıldıktan sonra, azure ile kimlik doğrulaması yapmak için runbook'unuzda uygun kodun olduğundan emin olun. Aşağıdaki örnekte, Bir Run As Hesabı kullanarak bir runbook'ta Azure'a doğrulanması gereken bir kod parçası gösterilmektedir.

```powershell
$connection = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzureRmAccount -ServicePrincipal -Tenant $connection.TenantID `
-ApplicationID $connection.ApplicationID -CertificateThumbprint $connection.CertificateThumbprint
```

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.
