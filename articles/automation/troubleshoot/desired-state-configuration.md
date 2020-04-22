---
title: Sorun Giderme Azure Otomasyon Durumu Yapılandırması (DSC)
description: Bu makalede, Sorun Giderme Azure Otomasyon Durumu Yapılandırması (DSC) hakkında bilgi sağlar.
services: automation
ms.service: automation
ms.subservice: ''
author: mgoedtel
ms.author: magoedte
ms.date: 04/16/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 4c045e110e21ed201278dcd84f38cb4a376ae8db
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81679315"
---
# <a name="troubleshoot-issues-with-azure-automation-state-configuration-dsc"></a>Azure Otomasyon Durumu Yapılandırması (DSC) ile ilgili sorun giderme sorunları

Bu makalede, Azure Otomasyon Durumu Yapılandırması'nda (DSC) yapılandırmaları derlerken veya dağıtırken ortaya çıkan sorun giderme sorunları hakkında bilgi verilmektedir.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](../automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="diagnosing-an-issue"></a>Bir sorunu tanılama

Yapılandırma için bir derleme veya dağıtım hatası aldığınızda, sorunu tanılamanıza yardımcı olacak birkaç adım aşağıda verebilirsiniz.

### <a name="1-ensure-that-your-configuration-compiles-successfully-on-the-local-machine"></a>1. Yapılandırmanızın yerel makinede başarılı bir şekilde derlediğinden emin olun

Azure Otomasyon Durumu Yapılandırması (DSC), PowerShell İstenen Durum Yapılandırması (DSC) üzerine kuruludur. [PowerShell DSC Dokümanları'nda](https://docs.microsoft.com/powershell/scripting/overview)DSC dili ve sözdizimi için belgeleri bulabilirsiniz.

Yerel makinenizde bir DSC yapılandırması derleyerek, örneğin sık karşılaşılan hataları keşfedebilir ve çözümleyebilirsiniz:

   - Eksik modüller
   - Sözdizimi hataları
   - Mantık hataları

### <a name="2-view-dsc-logs-on-your-node"></a>2. Düğümünüzdeki DSC günlüklerini görüntüleyin

Yapılandırmanız başarılı bir şekilde derlenmişse, ancak bir düğüme uygulandığında başarısız olursa, DSC günlüklerinde ayrıntılı bilgileri bulabilirsiniz. Bu günlükleri nerede bulacağınız hakkında bilgi için [DSC Olay Günlükleri nerede](/powershell/scripting/dsc/troubleshooting/troubleshooting#where-are-dsc-event-logs)dir.

[xDscDiagnostics](https://github.com/PowerShell/xDscDiagnostics) modülü, DSC günlüklerinden ayrıntılı bilgileri ayrıştmanıza yardımcı olabilir. Desteğe başvurursanız, sorununuzu tanılamak için bu günlükleri gerektirirler.

Modülü, kararlı `xDscDiagnostics` [sürüm modülünde](https://github.com/PowerShell/xDscDiagnostics#install-the-stable-version-module)bulunan yönergeleri kullanarak yerel makinenize yükleyebilirsiniz.

`xDscDiagnostics` Modülü Azure makinenize yüklemek için [Invoke-AzVMRunCommand'ı](https://docs.microsoft.com/powershell/module/az.compute/invoke-azvmruncommand?view=azps-3.7.0)kullanın. [Windows VM'nizde PowerShell komut dosyalarında](../../virtual-machines/windows/run-command.md)bulunan adımları Run Komutu ile izleyerek Azure portalındaki **Çalıştır komutseçeneğini** de kullanabilirsiniz.

**xDscDiagnostics**kullanma hakkında bilgi için bkz: [DSC günlüklerini analiz etmek için xDscDiagnostics'i kullanma.](/powershell/scripting/dsc/troubleshooting/troubleshooting#using-xdscdiagnostics-to-analyze-dsc-logs) Ayrıca bakınız [xDscDiagnostics Cmdlets](https://github.com/PowerShell/xDscDiagnostics#cmdlets).

### <a name="3-ensure-that-nodes-and-the-automation-workspace-have-required-modules"></a>3. Düğümlerin ve Otomasyon çalışma alanının gerekli modüllere sahip olduğundan emin olun

DSC düğüme yüklenen modüllere bağlıdır. Azure Otomasyon Durumu Yapılandırması'nı kullanırken, [İçe Aktarma Modülleri'ndeki](../shared-resources/modules.md#importing-modules)adımları kullanarak gerekli modülleri Otomasyon hesabınıza aktarın. Yapılandırmalar, modüllerin belirli sürümlerine de bağımlılık sağlayabilir. Daha fazla bilgi için [Sorun Giderme modüllerine](shared-resources.md#modules)bakın.

## <a name="scenario-a-configuration-with-special-characters-cannot-be-deleted-from-the-portal"></a><a name="unsupported-characters"></a>Senaryo: Özel karakterleriçeren bir yapılandırma portaldan silinemez

### <a name="issue"></a>Sorun

Portaldan bir DSC yapılandırmasını silmeye çalışırken aşağıdaki hatayı görürsünüz:

```error
An error occurred while deleting the DSC configuration '<name>'.  Error-details: The argument configurationName with the value <name> is not valid.  Valid configuration names can contain only letters,  numbers, and underscores.  The name must start with a letter.  The length of the name must be between 1 and 64 characters.
```

### <a name="cause"></a>Nedeni

Bu hata, çözülmesi planlanan geçici bir sorundur.

### <a name="resolution"></a>Çözüm

Yapılandırmayı silmek için cmdlet olanhttps://docs.microsoft.com/powershell/module/Az.Automation/Remove-AzAutomationDscConfiguration?view=azps-3.7.0 [Remove-AzAutomationDscConfiguration]'ı kullanın.

### <a name="scenario-failed-to-register-dsc-agent"></a><a name="failed-to-register-agent"></a>Senaryo: Dsc Aracısını kaydettiremedi

### <a name="issue"></a>Sorun

[Set-DscLocalConfigurationManager](https://docs.microsoft.com/powershell/module/psdesiredstateconfiguration/set-dsclocalconfigurationmanager?view=powershell-5.1) veya başka bir DSC cmdlet olduğunda, hata alırsınız:

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

### <a name="cause"></a>Nedeni

Bu hata normalde bir güvenlik duvarı, makinenin proxy sunucusunun arkasında olması veya diğer ağ hatalarından kaynaklanır.

### <a name="resolution"></a>Çözüm

Makinenizin DSC için uygun uç noktalara erişebilene sahip olduğunu doğrulayın ve yeniden deneyin. Gerekli bağlantı noktaları ve adreslerin listesi için [ağ planlamasına](../automation-dsc-overview.md#network-planning) bakın

## <a name="a-nameunauthorizedscenario-status-reports-return-response-code-unauthorized"></a><a name="unauthorized"><a/>Senaryo: Durum raporları iade yanıt kodu Yetkisiz

### <a name="issue"></a>Sorun

Azure Otomasyon Durumu Yapılandırması'na düğüm kaydederken aşağıdaki hata iletilerinden birini alırsınız:

```error
The attempt to send status report to the server https://{your Automation account URL}/accounts/xxxxxxxxxxxxxxxxxxxxxx/Nodes(AgentId='xxxxxxxxxxxxxxxxxxxxxxxxx')/SendReport returned unexpected response code Unauthorized.
```

```error
VM has reported a failure when processing extension 'Microsoft.Powershell.DSC / Registration of the Dsc Agent with the server failed.
```

### <a name="cause"></a>Nedeni

Bu sorun, kötü veya süresi dolmuş bir sertifikadan kaynaklanır. Bkz. [Sertifika nın süresi nin dolması ve yeniden kaydedilmesi.](../automation-dsc-onboarding.md#re-registering-a-node)

Bu sorun, ***.azure-automation.net**erişime izin vermeyen bir proxy yapılandırması da neden olabilir. Daha fazla bilgi için [bkz.](../automation-dsc-overview.md#network-planning) 

### <a name="resolution"></a>Çözüm

Başarısız DSC düğümlerini yeniden kaydetmek için aşağıdaki adımları kullanın.

Adım 1 - Düğümün kaydını silin.

1. Azure portalında, **Ev** -> **Otomasyon hesapları** (> (Otomasyon hesabınız) -> Durum **yapılandırmasına (DSC)** gidin.
2. **Düğümleri**seçin ve sorun olan düğüme tıklayın.
3. Düğümün kaydını çıkarmak için **Kaydı Nı Kınık'ı** tıklatın.

Adım 2 - Düğümden DSC uzantısını kaldırın.

1. Azure portalında, **Ana Sayfa** -> **Sanal Makine** -> (başarısız düğüm) -> **Uzantıları'na**gidin.
2. **Microsoft.Powershell.DSC,** PowerShell DSC uzantısını seçin.
3. Uzantıyı kaldırmak için **Kaldır'ı** tıklatın.

Adım 3 - Düğümden tüm bozuk veya süresi dolmuş sertifikaları kaldırın.

Yükseltilmiş powershell isteminden gelen başarısız düğümde şu komutları çalıştırın:

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

Adım 4 - Başarısız düğümü yeniden kaydedin.

1. Azure **portalında, Ev** -> **Otomasyon hesapları** (> (Otomasyon hesabınız) -> **Durum yapılandırmasına (DSC)** gidin
2. **Düğümleri**seçin.
3. **Ekle**'ye tıklayın.
4. Başarısız düğümü seçin.
5. **Bağlan'ı** tıklatın ve istediğiniz seçenekleri seçin.

## <a name="scenario-node-is-in-failed-status-with-a-not-found-error"></a><a name="failed-not-found"></a>Senaryo: Düğüm "Bulunamadı" hatasıyla başarısız durumda

### <a name="issue"></a>Sorun

Düğüm, Başarısız durumu olan ve hatayı içeren bir rapora sahiptir:

```error
The attempt to get the action from server https://<url>//accounts/<account-id>/Nodes(AgentId=<agent-id>)/GetDscAction failed because a valid configuration <guid> cannot be found.
```

### <a name="cause"></a>Nedeni

Bu hata genellikle düğüm bir yapılandırma adı atandığında oluşur, örneğin, **ABC**, düğüm yapılandırması yerine (MOF dosyası) adı, örneğin, **ABC. WebServer**.

### <a name="resolution"></a>Çözüm

* Düğümü yapılandırma adı yla değil, düğüm yapılandırma adıyla atadığınızdan emin olun.
* Azure portalını kullanarak veya PowerShell cmdlet kullanarak düğüm yapılandırması atayabilirsiniz.

  * Azure **portalında, Ev** -> **Otomasyon hesapları** (Otomasyon hesabınız) > -> **Durum yapılandırmasına (DSC)** gidin, ardından bir düğüm seçin ve **düğüm yapılandırmasını ata'yı**tıklatın.
  * [Set-AzAutomationDscNode](https://docs.microsoft.com/powershell/module/Az.Automation/Set-AzAutomationDscNode?view=azps-3.7.0) cmdlet kullanın.

## <a name="scenario-no-node-configurations-mof-files-were-produced-when-a-configuration-was-compiled"></a><a name="no-mof-files"></a>Senaryo: Yapılandırma derlendiğinde düğüm yapılandırmaları (MOF dosyaları) üretildi

### <a name="issue"></a>Sorun

DSC derleme işiniz hata yla askıya alınır:

```error
Compilation completed successfully, but no node configuration **.mof** files were generated.
```

### <a name="cause"></a>Nedeni

DSC yapılandırmasında anahtar kelimeyi `Node` izleyen ifade `$null`değerlendirdiğinde, düğüm yapılandırmaları üretilir.

### <a name="resolution"></a>Çözüm

Sorunu gidermek için aşağıdaki çözümlerden birini kullanın:

* Yapılandırma tanımındaki anahtar kelimenin `Node` yanındaki ifadenin Null olarak değerlendirilmediğinden emin olun.
* Yapılandırmayı derlerken [ConfigurationData'dan](../automation-dsc-compile.md) geçiyorsanız, yapılandırma verilerinden beklediği değerleri geçtiğinizden emin olun.

### <a name="scenario-the-dsc-node-report-becomes-stuck-in-the-in-progress-state"></a><a name="dsc-in-progress"></a>Senaryo: DSC düğüm raporu Devam Durumunda takılıp kalır

### <a name="issue"></a>Sorun

DSC aracı çıktıları:

```error
No instance found with given property values
```

### <a name="cause"></a>Nedeni

Windows Yönetim Çerçevesi (WMF) sürümünüzü yükselttiniz ve Windows Yönetim Araçları'nı (WMI) bozdunuz.

### <a name="resolution"></a>Çözüm

[DSC bilinen sorunları ve sınırlamaları](https://docs.microsoft.com/powershell/scripting/wmf/known-issues/known-issues-dsc)yönergeleri izleyin.

## <a name="scenario-unable-to-use-a-credential-in-a-dsc-configuration"></a><a name="issue-using-credential"></a>Senaryo: DSC yapılandırmasında kimlik bilgisi kullanılamıyor

### <a name="issue"></a>Sorun

DSC derleme işiniz hata yla askıya alındı:

```error
System.InvalidOperationException error processing property 'Credential' of type <some resource name>: Converting and storing an encrypted password as plaintext is allowed only if PSDscAllowPlainTextPassword is set to true.
```

### <a name="cause"></a>Nedeni

Bir yapılandırmada bir kimlik bilgisi kullandınız, ancak her `ConfigurationData` düğüm `PSDscAllowPlainTextPassword` yapılandırması için doğru ayarlamak için uygun sağlamadın.

### <a name="resolution"></a>Çözüm

Yapılandırmada belirtilen her `ConfigurationData` düğüm `PSDscAllowPlainTextPassword` yapılandırması için doğru ayarlamak için uygun geçmek emin olun. Bkz. [Azure Otomasyon Durumu Yapılandırmasında DSC yapılandırmalarını derleme.](../automation-dsc-compile.md)

## <a name="scenario-failure-processing-extension-error-when-onboarding-from-dsc-extension"></a><a name="failure-processing-extension"></a>Senaryo: DSC uzantısından binerken "Hata işleme uzantısı" hatası

### <a name="issue"></a>Sorun

Bir DSC uzantısı kullanılarak uçağa binerken, hatayı içeren bir hata oluşur:

```error
VM has reported a failure when processing extension 'Microsoft.Powershell.DSC'. Error message: \"DSC COnfiguration 'RegistrationMetaConfigV2' completed with error(s). Following are the first few: Registration of the Dsc Agent with the server <url> failed. The underlying error is: The attempt to register Dsc Agent with Agent Id <ID> with the server <url> return unexpected response code BadRequest. .\".
```

### <a name="cause"></a>Nedeni

Bu hata genellikle düğüm hizmette olmayan bir düğüm yapılandırma adı atandığında oluşur.

### <a name="resolution"></a>Çözüm

* Düğüme, hizmetteki ada tam olarak uyan bir adla atadığınızdan emin olun.
* Düğüm onboarding ama bir düğüm yapılandırma atamaz sonuçlanır düğüm yapılandırma adı içermemeyi seçebilirsiniz.

## <a name="scenario-one-or-more-errors-occurred-error-when-registering-a-node-using-powershell"></a><a name="cross-subscription"></a>Senaryo: PowerShell kullanarak düğüm kaydederken "Bir veya daha fazla hata oluştu" hatası

### <a name="issue"></a>Sorun

[Register-AzAutomationDSCNode](https://docs.microsoft.com/powershell/module/az.automation/register-azautomationdscnode?view=azps-3.7.0) veya [Register-AzureRMAutomationDSCNode](https://docs.microsoft.com/powershell/module/azurerm.automation/register-azurermautomationdscnode?view=azurermps-6.13.0)kullanarak bir düğüm kaydederken aşağıdaki hatayı alırsınız:

```error
One or more errors occurred.
```

### <a name="cause"></a>Nedeni

Bu hata, Bir düğümü Otomasyon hesabı tarafından kullanılan abonelikten ayrı bir aboneye kaydetmeye çalıştığınızda oluşur.

### <a name="resolution"></a>Çözüm

Çapraz abonelik düğümini ayrı bir bulut veya şirket içinde tanımlanmış gibi ele adedi. Bu onboarding seçeneklerinden birini kullanarak düğümü kaydedin:

* Windows - [Fiziksel/sanal Windows makineleri şirket içinde veya Azure/AWS dışındaki bir bulutta.](../automation-dsc-onboarding.md#onboarding-physicalvirtual-windows-machines)
* Linux - [Fiziksel/sanal Linux makineleri şirket içinde veya Azure dışındaki bir bulutta.](../automation-dsc-onboarding.md#onboarding-physicalvirtual-linux-machines)

## <a name="scenario-error-message---provisioning-failed"></a><a name="agent-has-a-problem"></a>Senaryo: Hata iletisi - "Sağlama Başarısız"

### <a name="issue"></a>Sorun

Bir düğüm kaydederken, hatayı görürsünüz:

```error
Provisioning has failed
```

### <a name="cause"></a>Nedeni

Bu ileti, düğüm ve Azure arasında bağlantı yla ilgili bir sorun olduğunda oluşur.

### <a name="resolution"></a>Çözüm

Düğümünüzün sanal özel ağda mı (VPN) olduğunu veya Azure'a bağlanan başka sorunları olup olmadığını belirleyin. [Çözümlere binerken Sorun Giderme hatalarına](onboarding.md)bakın.

## <a name="scenario-failure-with-a-general-error-when-applying-a-configuration-in-linux"></a><a name="failure-linux-temp-noexec"></a>Senaryo: Linux'ta yapılandırma uygularken genel bir hata ile hata

### <a name="issue"></a>Sorun

Linux'ta bir yapılandırma uygularken, hatayı içeren bir hata oluşur:

```error
This event indicates that failure happens when LCM is processing the configuration. ErrorId is 1. ErrorDetail is The SendConfigurationApply function did not succeed.. ResourceId is [resource]name and SourceInfo is ::nnn::n::resource. ErrorMessage is A general error occurred, not covered by a more specific error code..
```

### <a name="cause"></a>Nedeni

**/tmp** konumu `noexec`ayarlanmışsa, DSC'nin geçerli sürümü yapılandırmaları uygulamazsa.

### <a name="resolution"></a>Çözüm

`noexec` **Seçeneği /tmp** konumundan kaldırın.

## <a name="scenario-node-configuration-names-that-overlap-can-result-in-bad-release"></a><a name="compilation-node-name-overlap"></a>Senaryo: Çakışan düğüm yapılandırma adları kötü sürüm neden olabilir

### <a name="issue"></a>Sorun

Birden çok düğüm yapılandırması oluşturmak için tek bir yapılandırma komut dosyası kullandığınızda ve bazı düğüm yapılandırma adları diğer adların alt kümeleri olduğunda, derleme hizmeti yanlış yapılandırma atayarak sona erebilir. Bu sorun yalnızca düğüm başına yapılandırma verileri içeren yapılandırmalar oluşturmak için tek bir komut dosyası kullanılırken ve yalnızca dize başında ad çakışması oluştuğunda oluşur. Bir örnek, cmdlets kullanılarak karma olarak geçirilen düğüm verilerine dayalı yapılandırmalar oluşturmak için kullanılan tek bir yapılandırma komut dosyasıdır ve düğüm verileri **sunucu** ve **1server**adlı sunucuları içerir.

### <a name="cause"></a>Nedeni

Bu derleme hizmeti ile bilinen bir sorundur.

### <a name="resolution"></a>Çözüm

En iyi geçici çözüm, yerel olarak veya bir CI/CD ardışık alt yapıda derlemek ve düğüm yapılandırmaMOF dosyalarını doğrudan hizmete yüklemektir. Hizmetteki derleme bir gereksinimse, sonraki en iyi geçici çözüm, adlarda çakışma olmaması için derleme işlerini bölmektir.

## <a name="scenario-gateway-timeout-error-on-dsc-configuration-upload"></a><a name="gateway-timeout"></a>Senaryo: DSC yapılandırma yüklemeağ geçidi zaman hatası

#### <a name="issue"></a>Sorun

Bir DSC yapılandırması yüklerken bir `GatewayTimeout` hata alırsınız. 

### <a name="cause"></a>Nedeni

Derlemesi uzun zaman alan DSC yapılandırmaları bu hataya neden olabilir.

### <a name="resolution"></a>Çözüm

Herhangi bir [Alma-DSCResource](https://docs.microsoft.com/powershell/scripting/dsc/configurations/import-dscresource?view=powershell-5.1) araması için parametreyi `ModuleName` açıkça ekleyerek DSC yapılandırmalarınızı daha hızlı ayrıştırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu yukarıda görmüyorsanız veya sorununuzu çözemiyorsanız, ek destek için aşağıdaki kanallardan birini deneyin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıt alın.
* [@AzureSupport](https://twitter.com/azuresupport)Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.
* Azure destek olayı dosyala. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.