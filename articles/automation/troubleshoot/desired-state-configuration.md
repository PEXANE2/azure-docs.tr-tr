---
title: Azure Otomasyonu Istenen durum yapılandırması (DSC) ile ilgili sorunları giderme
description: Bu makale, Istenen durum yapılandırması (DSC) sorunlarını giderme hakkında bilgi sağlar
services: automation
ms.service: automation
ms.subservice: ''
author: bobbytreed
ms.author: robreed
ms.date: 04/16/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 6de348a19081eba685deafebd8a7c9b9d6556444
ms.sourcegitcommit: d585cdda2afcf729ed943cfd170b0b361e615fae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68688105"
---
# <a name="troubleshoot-desired-state-configuration-dsc"></a>Istenen durum yapılandırması (DSC) sorunlarını giderme

Bu makalede, Istenen durum yapılandırması (DSC) ile ilgili sorunları giderme hakkında bilgi verilmektedir.

## <a name="steps-to-troubleshoot-desired-state-configuration-dsc"></a>Istenen durum yapılandırması (DSC) sorunlarını giderme adımları

Azure Durum Yapılandırması 'nda yapılandırmaları derlerken veya dağıtmada hatalar olduğunda, sorunu tanılamanıza yardımcı olacak birkaç adım aşağıda verilmiştir.

1. **Yapılandırmanızın yerel makinenizde başarıyla derlendiğinden emin olun:**  Azure Durum Yapılandırması PowerShell DSC üzerine kurulmuştur. DSC dilinin ve sözdiziminin belgelerini [POWERSHELL DSC docs](/powershell/dsc/overview/overview)' da bulabilirsiniz.

   Yerel makinenizde DSC yapılandırmanızı derlerken, şu gibi yaygın hataları bulabilir ve çözebilirsiniz:

   - **Eksik modüller**
   - **Sözdizimi hataları**
   - **Mantık hataları**
2. **Düğümünüz için DSC günlüklerini görüntüleyin:** Yapılandırmanız başarıyla derlenir, ancak bir düğüme uygulandığında başarısız olursa, günlüklerde ayrıntılı bilgiler bulabilirsiniz. DSC günlüklerinin nerede bulunacağı hakkında daha fazla bilgi için, bkz. [WHERE DSC olay günlükleri](/powershell/dsc/troubleshooting/troubleshooting#where-are-dsc-event-logs).

   Futhermore, [Xdscdiagnostics](https://github.com/PowerShell/xDscDiagnostics) , DSC günlüklerinden ayrıntılı bilgiler ayrıştırılırken size yardımcı olabilir. Desteğe başvurursanız, bu günlüklere bu günlüklerin sizin için gerekli olmaları gerekir.

   [Sabit sürüm modülünü Install](https://github.com/PowerShell/xDscDiagnostics#install-the-stable-version-module)altında bulunan yönergeleri kullanarak, **xdscdiagnostics** 'i yerel makinenize yükleyebilirsiniz.

   Azure makinenizde **Xdscdiagnostics** 'i yüklemek için [az VM Run-Command](/cli/azure/vm/run-command) veya [Invoke-azvmruncommand](/powershell/module/azurerm.compute/invoke-azurermvmruncommand)komutunu kullanabilirsiniz. Ayrıca, [Çalıştır komutuyla WINDOWS sanal makinenizde PowerShell betikleri çalıştırma](../../virtual-machines/windows/run-command.md)bölümünde bulunan adımları izleyerek portaldan **Çalıştır komut** seçeneğini de kullanabilirsiniz.

   **Xdscdiagnostics**kullanımı hakkında bilgi için bkz. [XDSCDIAGNOSTICS kullanarak DSC günlüklerini analiz etme](/powershell/dsc/troubleshooting/troubleshooting#using-xdscdiagnostics-to-analyze-dsc-logs)ve [xdscdiagnostics cmdlet 'leri](https://github.com/PowerShell/xDscDiagnostics#cmdlets).
3. **Düğümlerinizin ve Otomasyon çalışma alanınızın gerekli modüllere sahip olduğundan emin olun:** İstenen durum yapılandırması, düğümde yüklü olan modüllere bağımlıdır.  Azure Otomasyonu durum yapılandırması kullanılırken, [Içeri aktarma modülleri](../shared-resources/modules.md#import-modules)' nde listelenen adımları kullanarak, gerekli tüm modülleri Otomasyon hesabınıza aktarın. Yapılandırmaların belirli modül sürümlerine de bağımlılığı olabilir.  Daha fazla bilgi için bkz. [modüller sorunlarını giderme](shared-resources.md#modules).

## <a name="common-errors-when-working-with-desired-state-configuration-dsc"></a>Istenen durum yapılandırması (DSC) ile çalışırken sık karşılaşılan hatalar

### <a name="unsupported-characters"></a>Senaryon Özel karakterler içeren bir yapılandırma portaldan silinemiyor

#### <a name="issue"></a>Sorun

Portaldan bir DSC yapılandırmasını silmeye çalışırken şu hatayı görürsünüz:

```error
An error occurred while deleting the DSC configuration '<name>'.  Error-details: The argument configurationName with the value <name> is not valid.  Valid configuration names can contain only letters,  numbers, and underscores.  The name must start with a letter.  The length of the name must be between 1 and 64 characters.
```

#### <a name="cause"></a>Nedeni

Bu hata, çözülmesi planlanan geçici bir sorundur.

#### <a name="resolution"></a>Çözüm

* Yapılandırmayı silmek için az "Remove-AzAutomationDscConfiguration" cmdlet 'Ini kullanın.
* Bu cmdlet 'in belgeleri henüz güncelleştirilmedi.  Bu durumda, Azurere modülünün belgelerine başvurun.
  * [Remove-AzureRmAutomationDSCConfiguration](/powershell/module/azurerm.automation/Remove-AzureRmAutomationDscConfiguration)

### <a name="failed-to-register-agent"></a>Senaryon DSC Aracısı kaydettirilemedi

#### <a name="issue"></a>Sorun

Ya da başka bir `Set-DscLocalConfigurationManager` DSC cmdlet 'i çalışmaya çalışırken şu hatayı alıyorsunuz:

```error
Registration of the Dsc Agent with the server
https://<location>-agentservice-prod-1.azure-automation.net/accounts/00000000-0000-0000-0000-000000000000 failed. The
underlying error is: Failed to register Dsc Agent with AgentId 00000000-0000-0000-0000-000000000000 with the server htt
ps://<location>-agentservice-prod-1.azure-automation.net/accounts/00000000-0000-0000-0000-000000000000/Nodes(AgentId='00000000-0000-0000-0000-000000000000'). .
    + CategoryInfo          : InvalidResult: (root/Microsoft/...gurationManager:String) [], CimException
    + FullyQualifiedErrorId : RegisterDscAgentCommandFailed,Microsoft.PowerShell.DesiredStateConfiguration.Commands.Re
   gisterDscAgentCommand
    + PSComputerName        : <computerName>
```

#### <a name="cause"></a>Nedeni

Bu hata, normalde bir güvenlik duvarının, bir proxy sunucusunun arkasında olduğu makinenin veya diğer ağ hatalarının oluşmasına neden olur.

#### <a name="resolution"></a>Çözüm

Makinenizin Azure Automation DSC için uygun uç noktalara erişimi olduğunu doğrulayın ve yeniden deneyin. Gereken bağlantı noktaları ve adreslerin listesi için bkz. [ağ planlama](../automation-dsc-overview.md#network-planning)

### <a name="failed-not-found"></a>Senaryon Düğüm, "bulunamadı" hatası ile başarısız durumda

#### <a name="issue"></a>Sorun

Düğüm, başarısız durumuna sahip bir rapora sahip ve **Şu** hatayı içerir:

```error
The attempt to get the action from server https://<url>//accounts/<account-id>/Nodes(AgentId=<agent-id>)/GetDscAction failed because a valid configuration <guid> cannot be found.
```

#### <a name="cause"></a>Nedeni

Bu hata genellikle düğüm bir düğüm yapılandırma adı (örneğin, ABC) yerine bir yapılandırma adına (örneğin, ABC) atandığında oluşur. Web sunucusu).

#### <a name="resolution"></a>Çözüm

* Düğümü "düğüm yapılandırma adı" ile ("yapılandırma adı" değil) atadığınızdan emin olun.
* Azure portal veya PowerShell cmdlet 'i kullanarak bir düğüme düğüm yapılandırması atayabilirsiniz.

  * Azure portal kullanarak bir düğüme düğüm yapılandırması atamak için **DSC düğümleri** sayfasını açın, ardından bir düğüm seçin ve **düğüm yapılandırması ata** düğmesine tıklayın.
  * PowerShell cmdlet 'ini kullanarak bir düğüme düğüm yapılandırması atamak için **set-AzureRmAutomationDscNode** cmdlet 'ini kullanın

### <a name="no-mof-files"></a>Senaryon Bir yapılandırma derlendiğinde hiçbir düğüm yapılandırması (MOF dosyası) üretilmedi

#### <a name="issue"></a>Sorun

DSC derleme işiniz şu hatayla askıya alınır:

```error
Compilation completed successfully, but no node configuration.mofs were generated.
```

#### <a name="cause"></a>Nedeni

DSC yapılandırmasındaki **düğüm** anahtar sözcüğünü izleyen ifade olarak `$null`değerlendirilirse, hiçbir düğüm yapılandırması üretilmez.

#### <a name="resolution"></a>Çözüm

Aşağıdaki çözümlerden herhangi biri sorunu çözer:

* Yapılandırma tanımındaki **node** anahtar sözcüğünün yanındaki ifadenin $null olarak değerlendirilmediğinden emin olun.
* Yapılandırmayı derlerken ConfigurationData geçiriyorken, yapılandırmanın [configurationData](../automation-dsc-compile.md#configurationdata)tarafından gerek duyduğu beklenen değerleri geçirdiğinizden emin olun.

### <a name="dsc-in-progress"></a>Senaryon DSC düğüm raporu "sürüyor" durumunda kalmış olur

#### <a name="issue"></a>Sorun

DSC Aracısı çıkışları:

```error
No instance found with given property values
```

#### <a name="cause"></a>Nedeni

WMF sürümünüzü yükselttiniz ve WMI 'yi bozmuş olursunuz.

#### <a name="resolution"></a>Çözüm

Sorunu gidermek için, [DSC bilinen sorunlar ve sınırlamalar](https://msdn.microsoft.com/powershell/wmf/5.0/limitation_dsc) makalesindeki yönergeleri izleyin.

### <a name="issue-using-credential"></a>Senaryon DSC yapılandırmasında kimlik bilgileri kullanılamıyor

#### <a name="issue"></a>Sorun

DSC derleme işiniz şu hatayla askıya alındı:

```error
System.InvalidOperationException error processing property 'Credential' of type <some resource name>: Converting and storing an encrypted password as plaintext is allowed only if PSDscAllowPlainTextPassword is set to true.
```

#### <a name="cause"></a>Nedeni

Yapılandırmada bir kimlik bilgisi kullandınız, ancak her düğüm yapılandırması için **Psdscallowplaintextpassword** değerini true olarak ayarlamak Için uygun **configurationData** sağlamadık.

#### <a name="resolution"></a>Çözüm

* Yapılandırmada belirtilen her düğüm yapılandırması için **Psdscallowplaintextpassword** öğesini true olarak ayarlamak üzere doğru **configurationData** ' ın geçdiğinizden emin olun. Daha fazla bilgi için bkz. [Azure Automation DSC varlıklar](../automation-dsc-compile.md#assets).

### <a name="failure-processing-extension"></a>Senaryon DSC uzantısından ekleme, "uzantı işleme hatası" hatası

#### <a name="issue"></a>Sorun

DSC uzantısını kullanarak ekleme yaparken, şu hatayı içeren bir hata oluşur:

```error
VM has reported a failure when processing extension 'Microsoft.Powershell.DSC'. Error message: \"DSC COnfiguration 'RegistrationMetaConfigV2' completed with error(s). Following are the first few: Registration of the Dsc Agent with the server <url> failed. The underlying error is: The attempt to register Dsc Agent with Agent Id <ID> with the server <url> return unexpected response code BadRequest. .\".
```

#### <a name="cause"></a>Nedeni

Bu hata genellikle, düğüme hizmette olmayan bir düğüm yapılandırma adı atandığında oluşur.

#### <a name="resolution"></a>Çözüm

* Düğümü, hizmette adıyla tam olarak eşleşen bir düğüm yapılandırma adı ile atadığınızdan emin olun.
* Düğüm yapılandırma adını eklemeyi tercih edebilirsiniz; Bu, düğüm ekleme, ancak düğüm yapılandırması atamakla sonuçlanır

### <a name="failure-linux-temp-noexec"></a>Senaryon Linux 'ta yapılandırma uygulama genel hata ile bir hata oluşur

#### <a name="issue"></a>Sorun

Linux 'ta bir yapılandırma uygularken, şu hatayı içeren bir hata oluşur:

```error
This event indicates that failure happens when LCM is processing the configuration. ErrorId is 1. ErrorDetail is The SendConfigurationApply function did not succeed.. ResourceId is [resource]name and SourceInfo is ::nnn::n::resource. ErrorMessage is A general error occurred, not covered by a more specific error code..
```

#### <a name="cause"></a>Nedeni

Müşteriler/tmp konumu noexec olarak ayarlandıysa, geçerli DSC sürümü yapılandırmaların uygulanmamasının başarısız olacağını belirledi.

#### <a name="resolution"></a>Çözüm

* Noexec seçeneğini/tmp konumundan kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayı dosyası gönderebilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**' ı seçin.
