---
title: Azure Otomasyonu paylaşılan kaynaklarıyla ilgili sorunları giderme
description: Runbook 'ları destekleyen Azure Otomasyonu paylaşılan kaynaklarıyla ilgili sorunları nasıl giderebileceğinizi ve çözeceğinizi öğrenin.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 03/12/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 4cea558b11d7ee7bbe838cecbd061cd487b536d2
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75769872"
---
# <a name="troubleshoot-errors-with-shared-resources"></a>Paylaşılan kaynaklarla ilgili sorunları giderme

Bu makalede, Azure Otomasyonu 'nda paylaşılan kaynakları kullanırken üzerinde çalıştırabileceğiniz sorunları gidermeye yönelik çözümler ele alınmaktadır.

## <a name="modules"></a>Modüller

### <a name="module-stuck-importing"></a>Senaryo: bir modül içeri aktarılıyor

#### <a name="issue"></a>Sorun

Modüllerinizi Azure Otomasyonu 'nda içeri aktardığınızda veya güncelleştirdiğinizde modül **Içeri aktarma** durumunda takılmış olur.

#### <a name="cause"></a>Nedeni

PowerShell modüllerini içeri aktarmak karmaşık bir çok adımlı işlemdir. Bu işlem, bir modülün doğru bir şekilde içe aktarılmamasının olasılığını tanıtır. Bu sorun oluşursa, içeri aktardığınız modül geçici bir durumda kalmış olabilir. Bu işlem hakkında daha fazla bilgi edinmek için bkz. [PowerShell modülünü Içeri aktarma](/powershell/scripting/developer/module/importing-a-powershell-module#the-importing-process).

#### <a name="resolution"></a>Çözünürlük

Bu sorunu çözmek için, [Remove-AzureRmAutomationModule](/powershell/module/azurerm.automation/remove-azurermautomationmodule) cmdlet 'Ini kullanarak **içeri aktarma** durumunda kalmış olan modülü kaldırmanız gerekir. Daha sonra modülün içeri aktarılmasını yeniden deneyebilirsiniz.

```azurepowershell-interactive
Remove-AzureRmAutomationModule -Name ModuleName -ResourceGroupName ExampleResourceGroup -AutomationAccountName ExampleAutomationAccount -Force
```

### <a name="update-azure-modules-importing"></a>Senaryo: Azurerd modülleri, güncelleştirilmeye çalıştıktan sonra içeri aktarılmaya takıldı

#### <a name="issue"></a>Sorun

Azurere modüllerinizi güncelleştirmeden sonra aşağıdaki iletinin bulunduğu bir başlık hesabınızda kalır:

```error
Azure modules are being updated
```

#### <a name="cause"></a>Nedeni

Azurerd modüllerini, 0 ile başlayan sayısal bir ada sahip bir kaynak grubundaki bir Otomasyon hesabında güncelleştiren bilinen bir sorun vardır.

#### <a name="resolution"></a>Çözünürlük

Otomasyon hesabınızda Azure modüllerinizi güncelleştirmek için, alfasayısal bir ada sahip bir kaynak grubunda olması gerekir. 0 ile başlayan sayısal adlara sahip kaynak grupları Şu anda Azurere modüllerini güncelleştiremiyor.

### <a name="module-fails-to-import"></a>Senaryo: modül içeri aktarılmazsa veya cmdlet 'ler içeri aktarıldıktan sonra yürütülemez

#### <a name="issue"></a>Sorun

Modül başarıyla içeri veya dışarı aktaramazsa, ancak hiçbir cmdlet ayıklanamaz.

#### <a name="cause"></a>Nedeni

Modülün Azure Otomasyonu 'na başarıyla aktarılamayan bazı yaygın nedenler şunlardır:

* Yapı, otomasyonun içinde olması gereken yapıyla eşleşmez.
* Modül, Otomasyon hesabınıza dağıtılmamış başka bir modüle bağımlıdır.
* Modülün içindeki bağımlılıkları eksik.
* `New-AzureRmAutomationModule` cmdlet 'i modülünü karşıya yüklemek için kullanılır ve tam depolama yolu verilmemiş ya da genel olarak erişilebilen bir URL kullanarak modülü yüklemediniz.

#### <a name="resolution"></a>Çözünürlük

Aşağıdaki çözümlerden herhangi biri sorunu çözer:

* Modülün şu biçime uyduğundan emin olun: ModuleName. zip **->** ModuleName veya sürüm numarası **->** (ModuleName. psm1, ModuleName. psd1)
* . Psd1 dosyasını açın ve modülün herhangi bir bağımlılığı olup olmadığını görün. Varsa, bu modülleri Otomasyon hesabına yükleyin.
* Başvurulan tüm. dll dosyalarının modül klasöründe bulunduğundan emin olun.

### <a name="all-modules-suspended"></a>Senaryo: modüller güncelleştirilirken Update-AzureModule. ps1 askıya alınır

#### <a name="issue"></a>Sorun

Azure modüllerinizi güncelleştirmek için [Update-AzureModule. ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1) runbook 'unu kullanırken, güncelleştirme işlemi askıya alınır.

#### <a name="cause"></a>Nedeni

`Update-AzureModule.ps1` betiği kullanılırken aynı anda kaç modülün güncelleştirildiğini belirleme için varsayılan ayar 10 ' dur. Aynı anda çok fazla modül güncelleştirilirken güncelleştirme işlemi hatalara açıktır.

#### <a name="resolution"></a>Çözünürlük

Tüm Azurerd modüllerinin aynı Otomasyon hesabında gerekli olduğu yaygın değildir. Yalnızca ihtiyaç duyduğunuz Azurermmodules içeri aktarılması önerilir.

> [!NOTE]
> **Azurerd** modülünü içeri aktarmaktan kaçının. **Azurere** modüllerini içeri aktarmak tüm **azurere.\*** modüllerinin içeri aktarılmasına neden olur, bu recommened değildir.

Güncelleştirme işlemi askıya alıyorsa, `SimultaneousModuleImportJobCount` parametresini `Update-AzureModules.ps1` betiğine eklemeniz ve 10 ' dan daha düşük bir değer sağlamanız gerekir. 3 veya 5 değeriyle başlamak için bu mantığı uygulamanız önerilir. `SimultaneousModuleImportJobCount`, Azure modüllerini güncelleştirmek için kullanılan `Update-AutomationAzureModulesForAccount` sistem runbook 'unun bir parametresidir. Bu değişiklik işlemin daha uzun süre çalışmasını sağlar, ancak tamamlanması daha iyi bir şansa sahiptir. Aşağıdaki örnek, parametresini ve Runbook 'a nereye yerleştirileceğini gösterir:

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

### <a name="unable-create-update"></a>Senaryo: bir farklı çalıştır hesabı oluşturamaz veya güncelleştiremezsiniz

#### <a name="issue"></a>Sorun

Farklı Çalıştır hesabı oluşturmaya veya güncelleştirmeye çalıştığınızda aşağıdaki hata iletisine benzer bir hata alırsınız:

```error
You do not have permissions to create…
```

#### <a name="cause"></a>Nedeni

Farklı Çalıştır hesabını oluşturmak veya güncelleştirmek için gerekli izinlere sahip değilsiniz veya kaynak bir kaynak grubu düzeyinde kilitli.

#### <a name="resolution"></a>Çözünürlük

Farklı Çalıştır hesabı oluşturmak veya güncelleştirmek için, farklı çalıştır hesabı tarafından kullanılan çeşitli kaynaklara uygun izinlere sahip olmanız gerekir. Farklı Çalıştır hesabı oluşturmak veya güncelleştirmek için gerekli izinler hakkında bilgi edinmek için, bkz. [Farklı Çalıştır hesabı izinleri](../manage-runas-account.md#permissions).

Sorun bir kilit nedeniyle varsa, kilidi kaldırmak için kilidin tamam olduğunu doğrulayın. Ardından kilitli olan kaynağa gidin ve kilidi kaldırmak için Kilitle ' yi sağ tıklatın ve **Sil** ' i seçin.

### <a name="iphelper"></a>Senaryo: bir runbook yürütürken "' iplpapı. dll ' DLL dosyasında ' Getperadapterınfo ' adlı bir giriş noktası bulunamadı" hatasını alıyorsunuz.

#### <a name="issue"></a>Sorun

Bir runbook yürütürken aşağıdaki özel durumu alırsınız:

```error
Unable to find an entry point named 'GetPerAdapterInfo' in DLL 'iplpapi.dll'
```

#### <a name="cause"></a>Nedeni

Bu hata büyük olasılıkla yanlış yapılandırılmış bir [Farklı Çalıştır hesabı](../manage-runas-account.md)nedeniyle oluşur.

#### <a name="resolution"></a>Çözünürlük

[Farklı Çalıştır hesabınızın](../manage-runas-account.md) doğru yapılandırıldığından emin olun. Doğru yapılandırıldıktan sonra, Azure ile kimlik doğrulamak için Runbook 'inizdeki uygun koda sahip olduğunuzdan emin olun. Aşağıdaki örnekte, bir farklı çalıştır hesabı kullanarak bir runbook 'ta Azure 'da kimlik doğrulaması yapmak için bir kod parçacığı gösterilmektedir.

```powershell
$connection = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzureRmAccount -ServicePrincipal -Tenant $connection.TenantID `
-ApplicationID $connection.ApplicationID -CertificateThumbprint $connection.CertificateThumbprint
```

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayı dosyası gönderebilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**' ı seçin.
