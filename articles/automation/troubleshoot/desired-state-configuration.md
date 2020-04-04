---
title: Sorun Giderme Azure Otomasyonu İstenilen Durum Yapılandırması (DSC)
description: Bu makalede, sorun giderme Hakkında bilgi sağlar İstenilen Durum Yapılandırması (DSC)
services: automation
ms.service: automation
ms.subservice: ''
author: mgoedtel
ms.author: magoedte
ms.date: 04/16/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9f33dc9528d5f7043dda2c6fad207a9a51347a2b
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80631488"
---
# <a name="troubleshoot-issues-with-azure-automation-desired-state-configuration-dsc"></a>Azure Otomasyonu İstenen Durum Yapılandırması (DSC) ile sorun giderme

Bu makalede, İstenilen Durum Yapılandırması (DSC) ile sorun giderme sorunları hakkında bilgi sağlar.

## <a name="diagnosing-an-issue"></a>Bir sorunu tanılama

Azure Durum Yapılandırması'nda yapılandırmaları derleme veya dağıtırken hatalarınız olduğunda, sorunu tanılamanıza yardımcı olacak birkaç adım aşağıda verilmiştir.

### <a name="1-ensure-that-your-configuration-compiles-successfully-on-the-local-machine"></a>1. Yapılandırmanızın yerel makinede başarılı bir şekilde derlediğinden emin olun

Azure Durum Yapılandırması PowerShell DSC'de yerleşiktir. [PowerShell DSC Dokümanları'nda](https://docs.microsoft.com/powershell/scripting/overview)DSC dili ve sözdizimi için belgeleri bulabilirsiniz.

DSC yapılandırmanızı yerel makinenizde derleyerek, şu gibi sık karşılaşılan hataları keşfedebilir ve çözümleyebilirsiniz:

   - Eksik modüller
   - Sözdizimi hataları
   - Mantık hataları

### <a name="2-view-dsc-logs-on-your-node"></a>2. Düğümünüzdeki DSC günlüklerini görüntüleyin

Yapılandırmanız başarılı bir şekilde derlenmişse, ancak bir düğüme uygulandığında başarısız olursa, DSC günlüklerinde ayrıntılı bilgileri bulabilirsiniz. Bu günlükleri nerede bulacağınız hakkında bilgi için [DSC Olay Günlükleri nerede](/powershell/scripting/dsc/troubleshooting/troubleshooting#where-are-dsc-event-logs)dir.

[xDscDiagnostics](https://github.com/PowerShell/xDscDiagnostics) modülü, DSC günlüklerinden ayrıntılı bilgileri ayrıştmanıza yardımcı olabilir. Desteğe başvurursanız, sorununuzu tanılamak için bu günlükleri gerektirirler.

Kararlı sürüm modülünde bulunan yönergeleri kullanarak xDscDiagnostics [modüllerini](https://github.com/PowerShell/xDscDiagnostics#install-the-stable-version-module)yerel makinenize yükleyebilirsiniz.

XDscDiagnostics modüllerini Azure makinenize yüklemek için [Invoke-AzVMRunCommand'ı](/powershell/module/azurerm.compute/invoke-azurermvmruncommand)kullanın. [Windows VM'nizde PowerShell komut dosyalarında](../../virtual-machines/windows/run-command.md)bulunan adımları Run Komutu ile izleyerek portaldan **Çalıştır komutseçeneğini** de kullanabilirsiniz.

xDscDiagnostics'i kullanma hakkında bilgi [için](/powershell/scripting/dsc/troubleshooting/troubleshooting#using-xdscdiagnostics-to-analyze-dsc-logs)bkz. Ayrıca bakınız [xDscDiagnostics Cmdlets](https://github.com/PowerShell/xDscDiagnostics#cmdlets).

### <a name="3-ensure-that-nodes-and-the-automation-workspace-have-required-modules"></a>3. Düğümlerin ve Otomasyon çalışma alanının gerekli modüllere sahip olduğundan emin olun

DSC düğüme yüklenen modüllere bağlıdır. Azure Otomasyon Durumu Yapılandırması'nı kullanırken, [İçe Aktarma Modüllerinde](../shared-resources/modules.md#importing-modules)listelenen adımları kullanarak gerekli modülleri Otomasyon hesabınıza aktarın. Yapılandırmalar, modüllerin belirli sürümlerine de bağımlılık sağlayabilir. Daha fazla bilgi için [Sorun Giderme Modülleri'ne](shared-resources.md#modules)bakın.

## <a name="common-errors-when-working-with-dsc"></a>DSC ile çalışırken sık karşılaşılan hatalar

### <a name="scenario-a-configuration-with-special-characters-cannot-be-deleted-from-the-portal"></a><a name="unsupported-characters"></a>Senaryo: Özel karakterleriçeren bir yapılandırma portaldan silinemez

#### <a name="issue"></a>Sorun

Portaldan bir DSC yapılandırmasını silmeye çalışırken aşağıdaki hatayı görürsünüz:

```error
An error occurred while deleting the DSC configuration '<name>'.  Error-details: The argument configurationName with the value <name> is not valid.  Valid configuration names can contain only letters,  numbers, and underscores.  The name must start with a letter.  The length of the name must be between 1 and 64 characters.
```

#### <a name="cause"></a>Nedeni

Bu hata, çözülmesi planlanan geçici bir sorundur.

#### <a name="resolution"></a>Çözüm

* Yapılandırmayı silmek için Az Cmdlet "Remove-AzAutomationDscConfiguration" kullanın.
* Bu cmdlet için belgeler henüz güncelleştirilmemiştir.  O zamana kadar AzureRM modülü için belgelere bakın.
  * [Kaldır-AzureRmAutomationDSCConfiguration](/powershell/module/azurerm.automation/Remove-AzureRmAutomationDscConfiguration)

### <a name="scenario-failed-to-register-dsc-agent"></a><a name="failed-to-register-agent"></a>Senaryo: Dsc Aracısını kaydettiremedi

#### <a name="issue"></a>Sorun

Çalıştırmayı veya `Set-DscLocalConfigurationManager` başka bir DSC cmdlet'i çalıştırmayı denerken aşağıdaki hatayı alırsınız:

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

Bu hata normalde bir güvenlik duvarı, makinenin proxy sunucusunun arkasında olması veya diğer ağ hatalarından kaynaklanır.

#### <a name="resolution"></a>Çözüm

Makinenizin Azure Automation DSC için uygun uç noktalara erişene sahip olduğunu doğrulayın ve yeniden deneyin. Gerekli bağlantı noktaları ve adreslerin listesi için [ağ planlamasına](../automation-dsc-overview.md#network-planning) bakın

### <a name="a-nameunauthorizedscenario-status-reports-return-response-code-unauthorized"></a><a name="unauthorized"><a/>Senaryo: Durum raporları iade yanıt kodu "Yetkisiz"

#### <a name="issue"></a>Sorun

Durum Yapılandırması (DSC) ile bir Düğüm kaydederken aşağıdaki hata iletilerinden birini alırsınız:

```error
The attempt to send status report to the server https://{your Automation account URL}/accounts/xxxxxxxxxxxxxxxxxxxxxx/Nodes(AgentId='xxxxxxxxxxxxxxxxxxxxxxxxx')/SendReport returned unexpected response code Unauthorized.
```

```error
VM has reported a failure when processing extension 'Microsoft.Powershell.DSC / Registration of the Dsc Agent with the server failed.
```

### <a name="cause"></a>Nedeni

Bu sorun, kötü veya süresi dolmuş bir sertifikadan kaynaklanır.  Daha fazla bilgi için [Bkz. Sertifika nın süresi nin dolması ve yeniden kaydedilmesi.](../automation-dsc-onboarding.md#re-registering-a-node)

### <a name="resolution"></a>Çözüm

Başarısız DSC düğümlerini yeniden kaydetmek için aşağıda listelenen adımları izleyin.

İlk olarak, aşağıdaki adımları kullanarak düğümü niçin kaydını silin.

1. Azure portalından, **Ev** -> **Otomasyon hesapları**altında -> {Otomasyon hesabınız} -> Durum **yapılandırması (DSC)**
2. "Düğümler"i tıklatın ve sorun olan düğüme tıklayın.
3. Düğümün kaydını çıkarmak için "Kaydını Açın" seçeneğini tıklayın.

İkinci olarak, düğümden DSC uzantısını kaldırın.

1. Azure portalından, **Ev** -> **Sanal Makinesi** altında -> {Failing node} -> **Uzantıları**
2. "Microsoft.Powershell.DSC" seçeneğini tıklayın.
3. PowerShell DSC uzantısını kaldırmak için "Kaldır"ı tıklatın.

Üçüncü olarak, düğümden tüm bozuk veya süresi dolmuş sertifikaları kaldırın.

Yükseltilmiş powershell isteminden gelen başarısız düğümde aşağıdakileri çalıştırın:

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

Son olarak, aşağıdaki adımları kullanarak başarısız düğümü yeniden kaydedin.

1. Azure portalından, **Ev** -> **Otomasyon hesapları** altında -> {Otomasyon hesabınız} -> Durum **yapılandırması (DSC)**
2. "Düğümler"i tıklatın.
3. "Ekle" düğmesini tıklatın.
4. Başarısız düğümü seçin.
5. "Bağlan"ı tıklatın ve istediğiniz seçenekleri seçin.

### <a name="scenario-node-is-in-failed-status-with-a-not-found-error"></a><a name="failed-not-found"></a>Senaryo: Düğüm "Bulunamadı" hatasıyla başarısız durumda

#### <a name="issue"></a>Sorun

Düğüm, **Başarısız** durumu olan ve hatayı içeren bir rapora sahiptir:

```error
The attempt to get the action from server https://<url>//accounts/<account-id>/Nodes(AgentId=<agent-id>)/GetDscAction failed because a valid configuration <guid> cannot be found.
```

#### <a name="cause"></a>Nedeni

Bu hata genellikle düğüm bir düğüm yapılandırma adı (örneğin, ABC) yerine bir yapılandırma adı (örneğin, ABC) atandığında oluşur. WebServer).

#### <a name="resolution"></a>Çözüm

* Düğümü "yapılandırma adı" ile değil de "düğüm yapılandırma adı" ile atadığınızdan emin olun.
* Azure portalını kullanarak veya PowerShell cmdlet kullanarak düğüm yapılandırması atayabilirsiniz.

  * Azure portalını kullanarak bir düğüm ekibe düğüm yapılandırması atamak için **DSC Düğümleri** sayfasını açın, ardından bir düğüm seçin ve düğüm yapılandırma düğmesini **atayın** düğmesini tıklatın.
  * PowerShell cmdlet kullanarak bir düğüme düğüm yapılandırması atamak için **Set-AzureRmAutomationDscNode** cmdlet'i kullanın

### <a name="scenario-no-node-configurations-mof-files-were-produced-when-a-configuration-is-compiled"></a><a name="no-mof-files"></a>Senaryo: Yapılandırma derlendiğinde düğüm yapılandırmaları (MOF dosyaları) üretildi

#### <a name="issue"></a>Sorun

DSC derleme işiniz hata yla askıya alınır:

```error
Compilation completed successfully, but no node configuration.mofs were generated.
```

#### <a name="cause"></a>Nedeni

DSC yapılandırmasında **düğüm** anahtar sözcüğü nden sonraki `$null`ifade ,düğüm yapılandırmaları için değerlendirildiğinde, düğüm yapılandırmaları üretilmez.

#### <a name="resolution"></a>Çözüm

Aşağıdaki çözümlerden herhangi biri sorunu giderir:

* Yapılandırma tanımındaki **Düğüm** anahtar kelimesinin yanındaki ifadenin $null için değerlendirilmediğinden emin olun.
* Yapılandırmayı derlerken ConfigurationData'dan geçiyorsanız, yapılandırmanın [ConfigurationData'dan](../automation-dsc-compile.md)gerektirdiği beklenen değerleri geçtiğinizden emin olun.

### <a name="scenario-the-dsc-node-report-becomes-stuck-in-progress-state"></a><a name="dsc-in-progress"></a>Senaryo: DSC düğüm raporu "devam ediyor" durumuna takılıp kalır

#### <a name="issue"></a>Sorun

DSC aracı çıktıları:

```error
No instance found with given property values
```

#### <a name="cause"></a>Nedeni

WMF sürümünüzü yükselttiniz ve WMI'yi bozdunuz.

#### <a name="resolution"></a>Çözüm

Sorunu gidermek için, [DSC bilinen sorunları ve sınırlamaları](https://docs.microsoft.com/powershell/scripting/wmf/known-issues/known-issues-dsc) makalesinde yönergeleri izleyin.

### <a name="scenario-unable-to-use-a-credential-in-a-dsc-configuration"></a><a name="issue-using-credential"></a>Senaryo: DSC yapılandırmasında kimlik bilgisi kullanılamıyor

#### <a name="issue"></a>Sorun

DSC derleme işiniz hata ile askıya alındı:

```error
System.InvalidOperationException error processing property 'Credential' of type <some resource name>: Converting and storing an encrypted password as plaintext is allowed only if PSDscAllowPlainTextPassword is set to true.
```

#### <a name="cause"></a>Nedeni

Bir yapılandırmada bir kimlik bilgisi kullandınız, ancak **PSDscAllowPlainTextPassword'i** her düğüm yapılandırması için doğru ayarlamak için uygun **ConfigurationData** sağlamadınız.

#### <a name="resolution"></a>Çözüm

* **PSDscAllowPlainTextPassword'i** yapılandırmada belirtilen her düğüm yapılandırması için doğru ayarlamak için uygun **ConfigurationData'dan** geçtiğinden emin olun. Daha fazla bilgi için bkz: [Azure Otomasyon Durumu Yapılandırmasında DSC yapılandırmalarını derleme.](../automation-dsc-compile.md)

### <a name="scenario-onboarding-from-dsc-extension-failure-processing-extension-error"></a><a name="failure-processing-extension"></a>Senaryo: DSC uzantısı onboarding, "Hata işleme uzantısı" hatası

#### <a name="issue"></a>Sorun

DSC uzantısı kullanılarak uçağa binerken, hatayı içeren bir hata oluşur:

```error
VM has reported a failure when processing extension 'Microsoft.Powershell.DSC'. Error message: \"DSC COnfiguration 'RegistrationMetaConfigV2' completed with error(s). Following are the first few: Registration of the Dsc Agent with the server <url> failed. The underlying error is: The attempt to register Dsc Agent with Agent Id <ID> with the server <url> return unexpected response code BadRequest. .\".
```

#### <a name="cause"></a>Nedeni

Bu hata genellikle düğüm hizmette olmayan bir düğüm yapılandırma adı atandığında oluşur.

#### <a name="resolution"></a>Çözüm

* Düğümdüğüme, hizmetteki ada tam olarak uyan bir düğüm yapılandırma adı verdiğinden emin olun.
* Düğüm ekine neden olacak ancak düğüm yapılandırması atamayan düğüm yapılandırma adını eklememeyi seçebilirsiniz

### <a name="scenario-registering-a-node-with-powershell-returns-the-error-one-or-more-errors-occurred"></a><a name="cross-subscription"></a>Senaryo: PowerShell ile bir düğüm kaydetme hatası "Bir veya daha fazla hata oluştu" döndürür

#### <a name="issue"></a>Sorun

Bir düğüm kullanarak `Register-AzAutomationDSCNode` veya `Register-AzureRMAutomationDSCNode`, aşağıdaki hatayı alırsınız kaydederken.

```error
One or more errors occurred.
```

#### <a name="cause"></a>Nedeni

Bu hata, Otomasyon hesabından ayrı bir abonelikte yaşayan bir düğümü kaydetmeye çalıştığınızda oluşur.

#### <a name="resolution"></a>Çözüm

Çapraz abonelik düğümlerini ayrı bir bulutta veya şirket içinde yaşıyormuş gibi davranın.

Düğümü kaydetmek için aşağıdaki adımları izleyin.

* Windows - [Fiziksel/sanal Windows makineleri şirket içinde veya Azure/AWS dışındaki bir bulutta.](../automation-dsc-onboarding.md#onboarding-physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azure-including-aws-ec2-instances)
* Linux - [Fiziksel/sanal Linux makineleri şirket içinde veya Azure dışındaki bir bulutta.](../automation-dsc-onboarding.md#onboarding-physicalvirtual-linux-machines-on-premises-or-in-a-cloud-other-than-azure)

### <a name="scenario-error-message---provisioning-failed"></a><a name="agent-has-a-problem"></a>Senaryo: Hata iletisi - "Sağlama Başarısız"

#### <a name="issue"></a>Sorun

Bir düğüm kaydederken, hatayı görürsünüz:

```error
Provisioning has failed
```

#### <a name="cause"></a>Nedeni

Bu ileti, düğüm ve Azure arasında bir bağlantı sorunu olduğunda oluşur.

#### <a name="resolution"></a>Çözüm

Düğümünüzün özel bir sanal ağda mı yoksa Azure'a bağlanan başka sorunları mı olduğunu belirleyin.

Daha fazla bilgi [için, çözümleri onboarding yaparken Sorun Giderme hataları](onboarding.md)na bakın.

### <a name="scenario-applying-a-configuration-in-linux-a-failure-occurs-with-a-general-error"></a><a name="failure-linux-temp-noexec"></a>Senaryo: Linux'ta yapılandırma uygulama, genel bir hata ile bir hata oluşur

#### <a name="issue"></a>Sorun

Linux'ta bir yapılandırma uygularken, hatayı içeren bir hata oluşur:

```error
This event indicates that failure happens when LCM is processing the configuration. ErrorId is 1. ErrorDetail is The SendConfigurationApply function did not succeed.. ResourceId is [resource]name and SourceInfo is ::nnn::n::resource. ErrorMessage is A general error occurred, not covered by a more specific error code..
```

#### <a name="cause"></a>Nedeni

`noexec`Müşteriler, `/tmp` konum ayarlanırsa, DSC'nin geçerli sürümünün yapılandırmaları uygulayamayacağını belirlemiş olur.

#### <a name="resolution"></a>Çözüm

* `noexec` Seçeneği konumdan `/tmp` kaldırın.

### <a name="scenario-node-configuration-names-that-overlap-could-result-in-bad-release"></a><a name="compilation-node-name-overlap"></a>Senaryo: Çakışan düğüm yapılandırma adları kötü sürümneden olabilir

#### <a name="issue"></a>Sorun

Birden çok düğüm yapılandırması oluşturmak için tek bir yapılandırma komut dosyası kullanılırsa ve bazı düğüm yapılandırmalarında başkalarının alt kümesi olan bir ad varsa, derleme hizmetindeki bir sorun yanlış yapılandırma atanmasına neden olabilir.  Bu yalnızca düğüm başına yapılandırma verileri içeren yapılandırmalar oluşturmak için tek bir komut dosyası kullanırken ve yalnızca dize başında ad çakışması oluştuğunda oluşur.

Örneğin, cmdlets kullanılarak karma olarak geçirilen düğüm verilerine dayalı yapılandırmalar oluşturmak için tek bir yapılandırma komut dosyası kullanılırsa ve düğüm verileri "sunucu" ve "1server" adlı bir sunucu içerir.

#### <a name="cause"></a>Nedeni

Derleme hizmetiyle ilgili bilinen sorun.

#### <a name="resolution"></a>Çözüm

En iyi geçici çözüm yerel olarak veya bir CI/CD ardışık alanda derlemek ve MOF dosyalarını doğrudan hizmete yüklemek olacaktır.  Hizmette derleme bir gereksinimse, sonraki en iyi geçici çözüm, adlarda çakışma olmaması için derleme işlerini bölmektir.

### <a name="scenario-gateway-timeout-error-on-dsc-configuration-upload"></a><a name="gateway-timeout"></a>Senaryo: DSC yapılandırma yüklemeağ geçidi zaman hatası

#### <a name="issue"></a>Sorun

Bir DSC yapılandırması yüklerken bir `GatewayTimeout` hata alırsınız. 

#### <a name="cause"></a>Nedeni

Derlemesi uzun zaman alan DSC yapılandırmaları bu hataya neden olabilir.

#### <a name="resolution"></a>Çözüm

Herhangi bir `ModuleName` `Import-DscResource` arama için parametreyi açıkça ekleyerek DSC yapılandırmalarınızı daha hızlı ayrışturabilirsiniz. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/powershell/scripting/dsc/configurations/import-dscresource?view=powershell-5.1)

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıt alın.
* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.
