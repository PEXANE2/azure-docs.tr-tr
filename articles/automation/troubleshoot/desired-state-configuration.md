---
title: Azure Otomasyonu Istenen durum yapılandırması (DSC) sorunlarını giderme
description: Bu makale, Istenen durum yapılandırması (DSC) sorunlarını giderme hakkında bilgi sağlar
services: automation
ms.service: automation
ms.subservice: ''
author: mgoedtel
ms.author: magoedte
ms.date: 04/16/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 3c3c9950aab9a5a422ebc9e858daded2888fd82e
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75834266"
---
# <a name="troubleshoot-issues-with-azure-automation-desired-state-configuration-dsc"></a>Azure Otomasyonu Istenen durum yapılandırması (DSC) ile ilgili sorunları giderme

Bu makalede, Istenen durum yapılandırması (DSC) ile ilgili sorunları giderme hakkında bilgi verilmektedir.

## <a name="steps-to-troubleshoot-desired-state-configuration-dsc"></a>Istenen durum yapılandırması (DSC) sorunlarını giderme adımları

Azure Durum Yapılandırması 'nda yapılandırmaları derlerken veya dağıtmada hatalar olduğunda, sorunu tanılamanıza yardımcı olacak birkaç adım aşağıda verilmiştir.

1. **Yapılandırmanızın yerel makinenizde başarıyla derlendiğinden emin olun:**  Azure Durum Yapılandırması PowerShell DSC üzerine kurulmuştur. DSC dilinin ve sözdiziminin belgelerini [POWERSHELL DSC docs](https://docs.microsoft.com/powershell/scripting/overview)' da bulabilirsiniz.

   Yerel makinenizde DSC yapılandırmanızı derleyerek, şu gibi yaygın hataları bulabilir ve çözebilirsiniz:

   - **Eksik modüller**
   - **Sözdizimi hataları**
   - **Mantık hataları**

2. **Düğümünüz IÇIN DSC günlüklerini görüntüleyin:** Yapılandırmanız başarıyla derlenir, ancak bir düğüme uygulandığında başarısız olursa, günlüklerde ayrıntılı bilgiler bulabilirsiniz. DSC günlüklerinin nerede bulunacağı hakkında daha fazla bilgi için, bkz. [WHERE DSC olay günlükleri](/powershell/scripting/dsc/troubleshooting/troubleshooting#where-are-dsc-event-logs).

   Ayrıca, [Xdscdiagnostics](https://github.com/PowerShell/xDscDiagnostics) , DSC günlüklerinden ayrıntılı bilgiler ayrıştırılırken size yardımcı olabilir. Desteğe başvurursanız, bu günlüklerin sorununuzu tanılamak için bu günlüklere ihtiyacı olacaktır.

   [Sabit sürüm modülünü Install](https://github.com/PowerShell/xDscDiagnostics#install-the-stable-version-module)altında bulunan yönergeleri kullanarak, **xdscdiagnostics** 'i yerel makinenize yükleyebilirsiniz.

   Azure makinenizde **Xdscdiagnostics** 'i yüklemek için [az VM Run-Command](/cli/azure/vm/run-command) veya [Invoke-azvmruncommand](/powershell/module/azurerm.compute/invoke-azurermvmruncommand)komutunu kullanabilirsiniz. Ayrıca, [Çalıştır komutuyla WINDOWS sanal makinenizde PowerShell betikleri çalıştırma](../../virtual-machines/windows/run-command.md)bölümünde bulunan adımları izleyerek portaldan **Çalıştır komut** seçeneğini de kullanabilirsiniz.

   **Xdscdiagnostics**kullanımı hakkında bilgi için bkz. [XDSCDIAGNOSTICS kullanarak DSC günlüklerini analiz etme](/powershell/scripting/dsc/troubleshooting/troubleshooting#using-xdscdiagnostics-to-analyze-dsc-logs)ve [xdscdiagnostics cmdlet 'leri](https://github.com/PowerShell/xDscDiagnostics#cmdlets).
3. **Düğümlerinizin ve Otomasyon çalışma alanınızın gerekli modüllere sahip olduğundan emin olun:** İstenen durum yapılandırması, düğümde yüklü olan modüllere bağımlıdır.  Azure Otomasyonu durum yapılandırması kullanılırken, [Içeri aktarma modülleri](../shared-resources/modules.md#import-modules)' nde listelenen adımları kullanarak, gerekli tüm modülleri Otomasyon hesabınıza aktarın. Yapılandırmaların belirli modül sürümlerine de bağımlılığı olabilir.  Daha fazla bilgi için bkz., [modüller sorunlarını giderme](shared-resources.md#modules).

## <a name="common-errors-when-working-with-desired-state-configuration-dsc"></a>Istenen durum yapılandırması (DSC) ile çalışırken sık karşılaşılan hatalar

### <a name="unsupported-characters"></a>Senaryo: özel karakterler içeren bir yapılandırma portaldan silinemiyor

#### <a name="issue"></a>Sorun

Portaldan bir DSC yapılandırmasını silmeye çalışırken şu hatayı görürsünüz:

```error
An error occurred while deleting the DSC configuration '<name>'.  Error-details: The argument configurationName with the value <name> is not valid.  Valid configuration names can contain only letters,  numbers, and underscores.  The name must start with a letter.  The length of the name must be between 1 and 64 characters.
```

#### <a name="cause"></a>Nedeni

Bu hata, çözülmesi planlanan geçici bir sorundur.

#### <a name="resolution"></a>Çözünürlük

* Yapılandırmayı silmek için az "Remove-AzAutomationDscConfiguration" cmdlet 'Ini kullanın.
* Bu cmdlet 'in belgeleri henüz güncelleştirilmedi.  Bu durumda, Azurere modülünün belgelerine başvurun.
  * [Remove-AzureRmAutomationDSCConfiguration](/powershell/module/azurerm.automation/Remove-AzureRmAutomationDscConfiguration)

### <a name="failed-to-register-agent"></a>Senaryo: DSC aracısının kaydı yapılamadı

#### <a name="issue"></a>Sorun

`Set-DscLocalConfigurationManager` veya başka bir DSC cmdlet 'ini çalıştırmaya çalışırken şu hatayı alıyorsunuz:

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

#### <a name="resolution"></a>Çözünürlük

Makinenizin Azure Automation DSC için uygun uç noktalara erişimi olduğunu doğrulayın ve yeniden deneyin. Gereken bağlantı noktaları ve adreslerin listesi için bkz. [ağ planlama](../automation-dsc-overview.md#network-planning)

### <a name="a-nameunauthorizedascenario-status-reports-return-response-code-unauthorized"></a><a name="unauthorized"><a/>senaryo: durum raporları "yetkisiz" yanıt kodunu döndürüyor

#### <a name="issue"></a>Sorun

Bir düğümü durum yapılandırması (DSC) ile kaydederken aşağıdaki hata iletilerinden birini alırsınız:

```error
The attempt to send status report to the server https://{your automation account url}/accounts/xxxxxxxxxxxxxxxxxxxxxx/Nodes(AgentId='xxxxxxxxxxxxxxxxxxxxxxxxx')/SendReport returned unexpected response code Unauthorized.
```

```error
VM has reported a failure when processing extension 'Microsoft.Powershell.DSC / Registration of the Dsc Agent with the server failed.
```

### <a name="cause"></a>Nedeni

Bu sorun, hatalı veya geçerliliği olumsuz bir sertifika nedeniyle oluşur.  Daha fazla bilgi için bkz. [sertifika süre sonu ve yapılabilir](../automation-dsc-onboarding.md#certificate-expiration-and-re-registration).

### <a name="resolution"></a>Çözünürlük

Hatalı DSC düğümünü yeniden kaydetmek için aşağıda listelenen adımları izleyin.

İlk olarak, aşağıdaki adımları kullanarak düğümü kaydını kaldırın.

1. Azure portal, **giriş** -> **otomasyon hesapları**-> {Automation hesabınız}-> **Durum Yapılandırması (DSC)** altında
2. "Düğümler" e tıklayın ve sorun yaşayan düğüme tıklayın.
3. Düğümün kaydını silmek için "kaydını kaldır" a tıklayın.

İkinci olarak, düğümden DSC uzantısını kaldırın.

1. Azure portal, **ana** -> **sanal makine** -> {başarısız olan düğüm}-> **uzantıları** altında
2. "Microsoft. PowerShell. DSC" ye tıklayın.
3. PowerShell DSC uzantısını kaldırmak için "Kaldır" a tıklayın.

Üçüncü olarak, düğümdeki tüm hatalı veya süre dolma sertifikaları kaldırın.

Yükseltilmiş bir PowerShell Isteminde başarısız olan düğümde aşağıdakileri çalıştırın:

```powershell
$certs = @()
$certs += dir cert:\localmachine\my | ?{$_.FriendlyName -like "DSC"}
$certs += dir cert:\localmachine\my | ?{$_.FriendlyName -like "DSC-OaaS Client Authentication"}
$certs += dir cert:\localmachine\CA | ?{$_.subject -like "CN=AzureDSCExtension*"}
"";"== DSC Certificates found: " + $certs.Count
$certs | FL ThumbPrint,FriendlyName,Subject
If (($certs.Count) -gt 0)
{ 
    ForEach ($Cert in $certs) 
    {
        RD -LiteralPath ($Cert.Pspath) 
    }
}
```

Son olarak, aşağıdaki adımları kullanarak başarısız olan düğümü yeniden kaydedin.

1. Azure portal, **giriş** -> **otomasyon hesapları** -> {Automation hesabınız}-> **Durum Yapılandırması (DSC)** altında
2. "Düğümler" e tıklayın.
3. "Ekle" düğmesine tıklayın.
4. Hatalı düğümü seçin.
5. "Bağlan" a tıklayın ve istediğiniz seçenekleri belirleyin.

### <a name="failed-not-found"></a>Senaryo: düğüm, "bulunamadı" hatası ile başarısız durumda

#### <a name="issue"></a>Sorun

Düğüm, başarısız durumuna sahip bir rapora sahip ve **Şu** hatayı içerir:

```error
The attempt to get the action from server https://<url>//accounts/<account-id>/Nodes(AgentId=<agent-id>)/GetDscAction failed because a valid configuration <guid> cannot be found.
```

#### <a name="cause"></a>Nedeni

Bu hata genellikle düğüm bir düğüm yapılandırma adı (örneğin, ABC) yerine bir yapılandırma adına (örneğin, ABC) atandığında oluşur. Web sunucusu).

#### <a name="resolution"></a>Çözünürlük

* Düğümü "düğüm yapılandırma adı" ile ("yapılandırma adı" değil) atadığınızdan emin olun.
* Azure portal veya PowerShell cmdlet 'i kullanarak bir düğüme düğüm yapılandırması atayabilirsiniz.

  * Azure portal kullanarak bir düğüme düğüm yapılandırması atamak için **DSC düğümleri** sayfasını açın, ardından bir düğüm seçin ve **düğüm yapılandırması ata** düğmesine tıklayın.
  * PowerShell cmdlet 'ini kullanarak bir düğüme düğüm yapılandırması atamak için **set-AzureRmAutomationDscNode** cmdlet 'ini kullanın

### <a name="no-mof-files"></a>Senaryo: bir yapılandırma derlendiğinde hiçbir düğüm yapılandırması (MOF dosyası) üretilmedi

#### <a name="issue"></a>Sorun

DSC derleme işiniz şu hatayla askıya alınır:

```error
Compilation completed successfully, but no node configuration.mofs were generated.
```

#### <a name="cause"></a>Nedeni

DSC yapılandırmasındaki **düğüm** anahtar sözcüğünü izleyen ifade `$null`olarak değerlendirilirse, hiçbir düğüm yapılandırması üretilmez.

#### <a name="resolution"></a>Çözünürlük

Aşağıdaki çözümlerden herhangi biri sorunu çözer:

* Yapılandırma tanımındaki **node** anahtar sözcüğünün yanındaki ifadenin $null olarak değerlendirilmediğinden emin olun.
* Yapılandırmayı derlerken ConfigurationData geçiriyorken, yapılandırmanın [configurationData](../automation-dsc-compile.md)tarafından gerek duyduğu beklenen değerleri geçirdiğinizden emin olun.

### <a name="dsc-in-progress"></a>Senaryo: DSC düğüm raporu "sürüyor" durumunda takılmış olur

#### <a name="issue"></a>Sorun

DSC Aracısı çıkışları:

```error
No instance found with given property values
```

#### <a name="cause"></a>Nedeni

WMF sürümünüzü yükselttiniz ve WMI 'yi bozmuş olursunuz.

#### <a name="resolution"></a>Çözünürlük

Sorunu gidermek için, [DSC bilinen sorunlar ve sınırlamalar](https://docs.microsoft.com/powershell/scripting/wmf/known-issues/known-issues-dsc) makalesindeki yönergeleri izleyin.

### <a name="issue-using-credential"></a>Senaryo: DSC yapılandırmasında kimlik bilgileri kullanılamıyor

#### <a name="issue"></a>Sorun

DSC derleme işiniz şu hatayla askıya alındı:

```error
System.InvalidOperationException error processing property 'Credential' of type <some resource name>: Converting and storing an encrypted password as plaintext is allowed only if PSDscAllowPlainTextPassword is set to true.
```

#### <a name="cause"></a>Nedeni

Yapılandırmada bir kimlik bilgisi kullandınız, ancak her düğüm yapılandırması için **Psdscallowplaintextpassword** değerini true olarak ayarlamak Için uygun **configurationData** sağlamadık.

#### <a name="resolution"></a>Çözünürlük

* Yapılandırmada belirtilen her düğüm yapılandırması için **Psdscallowplaintextpassword** öğesini true olarak ayarlamak üzere doğru **configurationData** ' ın geçdiğinizden emin olun. Daha fazla bilgi için bkz. [Azure Automation DSC varlıklar](../automation-dsc-compile.md#working-with-assets-in-azure-automation-during-compilation).

### <a name="failure-processing-extension"></a>Senaryo: DSC uzantısından ekleme, "uzantı işleme hatası" hatası

#### <a name="issue"></a>Sorun

DSC uzantısını kullanarak ekleme yaparken, şu hatayı içeren bir hata oluşur:

```error
VM has reported a failure when processing extension 'Microsoft.Powershell.DSC'. Error message: \"DSC COnfiguration 'RegistrationMetaConfigV2' completed with error(s). Following are the first few: Registration of the Dsc Agent with the server <url> failed. The underlying error is: The attempt to register Dsc Agent with Agent Id <ID> with the server <url> return unexpected response code BadRequest. .\".
```

#### <a name="cause"></a>Nedeni

Bu hata genellikle, düğüme hizmette olmayan bir düğüm yapılandırma adı atandığında oluşur.

#### <a name="resolution"></a>Çözünürlük

* Düğümü, hizmette adıyla tam olarak eşleşen bir düğüm yapılandırma adı ile atadığınızdan emin olun.
* Düğüm yapılandırma adını eklemeyi tercih edebilirsiniz; Bu, düğüm ekleme, ancak düğüm yapılandırması atamakla sonuçlanır

### <a name="cross-subscription"></a>Senaryo: PowerShell ile bir düğümü kaydetme, "bir veya daha fazla hata oluştu" hatasını döndürüyor

#### <a name="issue"></a>Sorun

`Register-AzAutomationDSCNode` veya `Register-AzureRMAutomationDSCNode`kullanarak bir düğüm kaydederken, aşağıdaki hatayı alırsınız.

```error
One or more errors occurred.
```

#### <a name="cause"></a>Nedeni

Bu hata, Otomasyon hesabından ayrı bir abonelikte bulunan bir düğümü kaydetmeye çalıştığınızda oluşur.

#### <a name="resolution"></a>Çözünürlük

Çapraz abonelik düğümünü ayrı bir bulutta veya şirket içinde bulunan gibi değerlendirin.

Düğümü kaydetmek için aşağıdaki adımları izleyin.

* Windows- [Şirket içinde veya Azure/AWS dışındaki bir bulutta bulunan Windows-fiziksel/sanal Windows makineleri](../automation-dsc-onboarding.md#physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azure-including-aws-ec2-instances).
* Linux- [Şirket içi veya Azure dışındaki bir bulutta Linux-fiziksel/sanal Linux makineleri](../automation-dsc-onboarding.md#physicalvirtual-linux-machines-on-premises-or-in-a-cloud-other-than-azure).

### <a name="agent-has-a-problem"></a>Senaryo: hata iletisi-"sağlama başarısız oldu"

#### <a name="issue"></a>Sorun

Bir düğüm kaydederken şu hatayı görürsünüz:

```error
Provisioning has failed
```

#### <a name="cause"></a>Nedeni

Bu ileti, düğüm ile Azure arasında bir bağlantı sorunu olduğunda oluşur.

#### <a name="resolution"></a>Çözünürlük

Düğümünüz özel bir sanal ağda olup olmadığını veya Azure 'a bağlanan başka sorunlar olup olmadığını belirleme.

Daha fazla bilgi için bkz. [çözüm ekleme sırasında hata giderme](onboarding.md).

### <a name="failure-linux-temp-noexec"></a>Senaryo: Linux 'ta bir yapılandırma uygulama genel hata ile bir hata oluşur

#### <a name="issue"></a>Sorun

Linux 'ta bir yapılandırma uygularken, şu hatayı içeren bir hata oluşur:

```error
This event indicates that failure happens when LCM is processing the configuration. ErrorId is 1. ErrorDetail is The SendConfigurationApply function did not succeed.. ResourceId is [resource]name and SourceInfo is ::nnn::n::resource. ErrorMessage is A general error occurred, not covered by a more specific error code..
```

#### <a name="cause"></a>Nedeni

Müşteriler `/tmp` konum `noexec`olarak ayarlanırsa, geçerli DSC sürümü yapılandırmaların uygulanmamasının başarısız olacağını belirledi.

#### <a name="resolution"></a>Çözünürlük

* `noexec` seçeneğini `/tmp` konumundan kaldırın.

### <a name="compilation-node-name-overlap"></a>Senaryo: çakışan düğüm yapılandırma adları bozuk bir yayına neden olabilir

#### <a name="issue"></a>Sorun

Birden çok düğüm yapılandırması oluşturmak için tek bir yapılandırma betiği kullanılıyorsa ve bazı düğüm yapılandırmalarının başkalarının alt kümesi olan bir adı varsa, derleme hizmetindeki bir sorun yanlış yapılandırma atamaya neden olabilir.  Bu yalnızca, düğüm başına yapılandırma verileri olan yapılandırmalar oluşturmak için tek bir komut dosyası kullanıldığında ve yalnızca ad örtüşme dizenin başlangıcında oluştuğunda oluşur.

Örnek olarak, cmdlet 'ler kullanılarak bir karma tablosu olarak geçirilen düğüm verilerine dayalı yapılandırmalar oluşturmak için tek bir yapılandırma betiği kullanılırsa ve düğüm verileri "sunucu" ve "1Sunucu" adlı bir sunucu içeriyorsa.

#### <a name="cause"></a>Nedeni

Derleme hizmeti ile ilgili bilinen sorun.

#### <a name="resolution"></a>Çözünürlük

En iyi geçici çözüm, yerel olarak veya bir CI/CD ardışık düzeninde derlemek ve MOF dosyalarını doğrudan hizmete yüklemek olacaktır.  Hizmette derleme bir gereksinimle karşılaşırsanız, bir sonraki en iyi geçici çözüm, ad içinde çakışma olmaması için derleme işlerinin bölünmesi olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayı dosyası gönderebilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**' ı seçin.
