---
title: Sorun Giderme Azure OtomasyonKarma Runbook İşçileri
description: Bu makalede, Sorun Giderme Azure Otomasyon Karma Runbook Çalışanları için bilgi sağlar.
services: automation
ms.service: automation
ms.subservice: ''
author: mgoedtel
ms.author: magoedte
ms.date: 11/25/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 2b3bf6706e977bdb6915335dee59da3c250e7895
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81679331"
---
# <a name="troubleshoot-hybrid-runbook-workers"></a>Sorun Giderme Hibrid Runbook İşçileri

Bu makalede, Karma Runbook Çalışanları ile sorun giderme sorunları hakkında bilgi sağlar.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](../automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="general"></a>Genel

Karma Runbook Çalışanı, çalışanı kaydetmek, runbook işlerini almak ve durumu bildirmek için Otomasyon hesabınızla iletişim kuracak bir aracıya bağlıdır. Windows için bu aracı, Windows için Log Analytics aracısıdır. Linux için, Linux için Log Analytics aracısıdır.

### <a name="scenario-runbook-execution-fails"></a><a name="runbook-execution-fails"></a>Senaryo: Runbook yürütme başarısız olur

#### <a name="issue"></a>Sorun

Runbook yürütme başarısız olur ve aşağıdaki hatayı alırsınız.

```error
"The job action 'Activate' cannot be run, because the process stopped unexpectedly. The job action was attempted three times."
```

Runbook'unuzun üç kez yürütmeye çalışmasından kısa bir süre sonra askıya alınır. Runbook'un tamamlanmasını engelleyebilir koşullar vardır. İlgili hata iletisi ek bilgi içermeyebilir.

#### <a name="cause"></a>Nedeni

Olası nedenler şunlardır:

* Runbook'lar yerel kaynaklarla doğrulaşamaz.
* Karma çalışan bir proxy veya güvenlik duvarı arkasındadır.
* Karma Runbook Worker'ı çalıştırmak üzere yapılandırılan bilgisayar minimum donanım gereksinimlerini karşılamıyor.

#### <a name="resolution"></a>Çözüm

Bilgisayarın 443 portundaki ***.azure-automation.net'a** giden erişimi olduğunu doğrulayın.

Karma Runbook Worker çalıştıran bilgisayarlar, alt yapı bu özelliği barındıracak şekilde yapılandırılmadan önce minimum donanım gereksinimlerini karşılamalıdır. Runbook'lar ve kullandıkları arka plan işlemi, sistemin aşırı kullanılmasına ve runbook iş gecikmelerine veya zaman aşımlarına neden olabilir.

Karma Runbook Worker özelliğini çalıştıracak bilgisayarın minimum donanım gereksinimlerini karşıladığını doğrulayın. Varsa, Karma Runbook Worker işlemlerinin performansı ile Windows arasındaki ilişkiyi belirlemek için CPU ve bellek kullanımını izleyin. Herhangi bir bellek veya CPU basıncı kaynakları yükseltme gereksinimini gösterebilir. Ayrıca, iş yükü talepleri bir artışın gerekli olduğunu gösterdiğinde minimum gereksinimleri ve ölçeği destekleyen farklı bir bilgi işlem kaynağı da seçebilirsiniz.

Açıklama `Win32 Process Exited with code [4294967295]`içeren ilgili bir olay için **Microsoft-SMA** olay günlüğünü denetleyin. Bu hatanın nedeni, çalışma kitaplarınızda kimlik doğrulamasını yapılandırmamış veya Karma Runbook Worker grubu için Run As kimlik bilgileri olarak belirtmemiş sinizdir. Runbook'larınız için doğru şekilde yapılandırılan kimlik doğruluğunu onaylamak için [Bir Karma Runbook Worker'da Runbook'ları çalıştırma](../automation-hrw-run-runbooks.md) çalışma izinlerini gözden geçirin.

### <a name="scenario-event-15011-in-hybrid-runbook-worker"></a><a name="cannot-connect-signalr"></a>Senaryo: Etkinlik 15011 Hibrid Runbook Worker

#### <a name="issue"></a>Sorun

Karma Runbook Worker, sorgu sonucunun geçerli olmadığını belirten 15011 olayını alır. Aşağıdaki hata, alt çalışan [SignalR sunucusu](https://docs.microsoft.com/aspnet/core/signalr/introduction?view=aspnetcore-3.1)ile bir bağlantı açmaya çalıştığında görüntülenir.

```error
[AccountId={c7d22bd3-47b2-4144-bf88-97940102f6ca}]
[Uri=https://cc-jobruntimedata-prod-su1.azure-automation.net/notifications/hub][Exception=System.TimeoutException: Transport timed out trying to connect
   at System.Runtime.ExceptionServices.ExceptionDispatchInfo.Throw()
   at System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)
   at JobRuntimeData.NotificationsClient.JobRuntimeDataServiceSignalRClient.<Start>d__45.MoveNext()
```

#### <a name="cause"></a>Nedeni

Karma Runbook İşçisi, otomatik dağıtım çözümü için doğru şekilde yapılandırılmamıştır. Bu çözüm, VM'yi Log Analytics çalışma alanına bağlayan bir parça içerir. PowerShell komut dosyası, abonelikteki çalışma alanını sağlanan adla birlikte arar. Bu durumda, Log Analytics çalışma alanı farklı bir aboneliktedir. Komut dosyası çalışma alanını bulamıyor ve bir tane oluşturmaya çalışıyor, ancak ad zaten alınmış. Böylece dağıtım başarısız olur.

#### <a name="resolution"></a>Çözüm

Bu sorunu çözmek için iki seçeneğiniz var:

* Başka bir abonelikte Log Analytics çalışma alanını aramak için PowerShell komut dosyasını değiştirin. Gelecekte birçok Hybrid Runbook Worker makinesidağıtmayı planlıyorsanız, bu iyi bir çözümdür.

* İşçi makinesini orchestrator sandbox'ta çalışacak şekilde el ile yapılandırın. Ardından, işlevselliği test etmek için azure otomasyon hesabında oluşturulan bir runbook çalıştırın.

### <a name="scenario-windows-azure-vms-automatically-dropped-from-hybrid-worker-group"></a><a name="vm-automatically-dropped"></a>Senaryo: Windows Azure VM'leri otomatik olarak karma alt gruptan çıkarıldı

#### <a name="issue"></a>Sorun

İşçi makinesi uzun süre kapalıyken Karma Runbook Worker'ı veya VM'leri göremezsiniz.

#### <a name="cause"></a>Nedeni

Karma Runbook Worker makinesi Azure Otomasyonu'nu 30 günden uzun süredir pinglememiştir. Sonuç olarak, Otomasyon Karma Runbook Worker grubunu veya Sistem Worker grubunu temizlemiştir. 

#### <a name="resolution"></a>Çözüm

Alt makineyi çalıştırın ve Azure Otomasyonu ile yeniden kaydedin. [Windows Karma Runbook Worker'ı dağıt'ta](../automation-windows-hrw-install.md)runbook ortamını yükleme ve Azure Otomasyonuna bağlanma yönergelerine bakın.

### <a name="scenario-no-certificate-was-found-in-the-certificate-store-on-hybrid-runbook-worker"></a><a name="no-cert-found"></a>Senaryo: Hibrit Runbook Worker'da sertifika deposunda sertifika bulunamadı

#### <a name="issue"></a>Sorun

Karma Runbook Çalışması'nda çalışan bir runbook aşağıdaki hata iletisiyle başarısız olur.

```error
Connect-AzAccount : No certificate was found in the certificate store with thumbprint 0000000000000000000000000000000000000000
At line:3 char:1
+ Connect-AzAccount -ServicePrincipal -Tenant $Conn.TenantID -Appl ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Connect-AzAccount], ArgumentException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.Profile.ConnectAzAccountCommand
```
#### <a name="cause"></a>Nedeni

Bu hata, Hesap Olarak Çalıştır sertifikasının bulunmadığı karma runbook Çalışanı üzerinde çalışan bir runbook'ta Run [As hesabı](../manage-runas-account.md) kullanmaya çalıştığınızda oluşur. Karma Runbook Çalışanları varsayılan olarak sertifika varlığına sahip değildir. Run As hesabı, bu varlığın düzgün çalışmasını gerektirir.

#### <a name="resolution"></a>Çözüm

Karma Runbook Worker'ınız bir Azure VM'siyse, Bunun yerine [Azure kaynakları için Yönetilen kimlikleri](../automation-hrw-run-runbooks.md#managed-identities-for-azure-resources) kullanabilirsiniz. Bu senaryo, Gerçek Olarak Çalıştır hesabı yerine Azure VM'nin yönetilen kimliğini kullanarak Azure kaynaklarına kimlik doğrulamanızı sağlayan kimlik doğrulamasını kolaylaştırır. Karma Runbook Worker şirket içi bir makine olduğunda, Makineye Hesap Olarak Çalıştır sertifikasını yüklemeniz gerekir. Sertifikayı nasıl yükleyeriz öğrenmek için, PowerShell runbook **Export-RunAsCertificateToHybridWorker** çalıştırılabilen adımları bir [Karma Runbook Worker üzerinde runbook'larda çalıştırın.](../automation-hrw-run-runbooks.md)

### <a name="scenario-error-403-during-registration-of-hybrid-runbook-worker"></a><a name="error-403-on-registration"></a>Senaryo: Karma Runbook Worker'ın kaydı sırasında hata 403

#### <a name="issue"></a>Sorun

İşçinin ilk kayıt aşaması başarısız olur ve aşağıdaki hatayı alırsınız (403).

```error
"Forbidden: You don't have permission to access / on this server."
```

#### <a name="cause"></a>Nedeni

Olası nedenler şunlardır:

* Aracının ayarlarında yanlış yazılmış bir çalışma alanı kimliği veya çalışma alanı anahtarı (birincil) vardır. 

* Karma Runbook Çalışanı yapılandırmayı karşıdan yükleyemez ve hesap bağlama hatasına neden olur. Azure çözümleri etkinleştirdiğinde, Bir Log Analytics çalışma alanını ve Otomasyon hesabını bağlamak için yalnızca belirli bölgeleri destekler. Bilgisayarda yanlış bir tarih ve/veya saat ayarlanmış olabilir. Geçerli zamana +/-15 dakika mesafede ise, onboarding başarısız olur.

#### <a name="resolution"></a>Çözüm

##### <a name="mistyped-workspace-idkey"></a>Yanlış yazılmış çalışma alanı kimliği/anahtarı
Aracının çalışma alanı kimliği veya çalışma alanı anahtarının yanlış yazıldığını doğrulamak [için](../../azure-monitor/platform/agent-manage.md#linux-agent) [bkz.](../../azure-monitor/platform/agent-manage.md#windows-agent)  Azure portalından tam dizeyi seçtiğinizden ve dikkatlice kopyalayıp yapıştırdığından emin olun.

##### <a name="configuration-not-downloaded"></a>Yapılandırma indirilmedi

Log Analytics çalışma alanı ve Otomasyon hesabınız bağlantılı bir bölgede olmalıdır. Desteklenen bölgelerin listesi için [Azure Otomasyonu ve Günlük Analizi çalışma alanı eşlemelerine](../how-to/region-mappings.md)bakın.

Ayrıca bilgisayarınızın tarih ve/veya saat dilimini güncelleştirmeniz gerekebilir. Özel bir zaman aralığı seçerseniz, aralığın yerel saat diliminizden farklı olabilecek UTC'de olduğundan emin olun.

## <a name="linux"></a>Linux

Linux Hybrid Runbook Worker, çalışanı kaydetmek, runbook işlerini almak ve durumu bildirmek için Otomasyon hesabınızla iletişim kurabilmesi [için Linux'un Log Analytics aracısına](../../azure-monitor/platform/log-analytics-agent.md) bağlıdır. İşçinin kaydı başarısız olursa, hatanın olası bazı nedenleri şunlardır:

### <a name="scenario-the-log-analytics-agent-for-linux-isnt-running"></a><a name="oms-agent-not-running"></a>Senaryo: Linux için Log Analytics aracısı çalışmıyor

#### <a name="issue"></a>Sorun

Linux için Log Analytics aracısı çalışmıyor

#### <a name="cause"></a>Nedeni

Aracı çalışmıyorsa, Linux Karma Runbook Çalışanı'nın Azure Otomasyonu ile iletişim kurmasını engeller. Aracı çeşitli nedenlerle çalışmıyor olabilir.

#### <a name="resolution"></a>Çözüm

 Komutu girerek aracının çalıştığını `ps -ef | grep python`doğrulayın. Aşağıdakilere benzer çıktıları görmeniz gerekir, Python işlemleri **nxautomation** kullanıcı hesabı ile. Güncelleştirme Yönetimi veya Azure Otomasyonu çözümü etkinleştirilemezse, aşağıdaki işlemlerin hiçbiri çalışmıyor.

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

Aşağıdaki liste, bir Linux Hybrid Runbook Worker için başlatılan işlemleri gösterir. Hepsi **/var/opt/microsoft/omsagent/state/automationworker/** dizininde bulunur.

* **oms.conf** - İşçi yöneticisi süreci. Doğrudan DSC'den başladı.

* **işçi.conf** - Otomatik Kayıtlı hibrid işçi süreci. İşçi yöneticisi tarafından başlatıldı. Bu işlem Güncelleştirme Yönetimi tarafından kullanılır ve kullanıcı için saydamdır. Güncelleştirme Yönetimi çözümü makinede etkinleştirilemezse, bu işlem mevcut değildir.

* **diy /worker.conf** - DIY hibrid işçi süreci. DIY karma alt işleme, Hibrit Runbook Worker kullanıcı runbook'larını çalıştırmak için kullanılır. Yalnızca, farklı bir yapılandırma kullandığı anahtar ayrıntısındaki Otomatik Kayıtlı karma alt işlemden farklıdır. Azure Otomasyon çözümü devre dışı bırakılmışsa ve DIY Linux Karma İşçisi kayıtlı değilse, bu işlem mevcut değildir.

Aracı çalışmıyorsa, hizmeti başlatmak için aşağıdaki komutu `sudo /opt/microsoft/omsagent/bin/service_control restart`çalıştırın: .

### <a name="scenario-the-specified-class-doesnt-exist"></a><a name="class-does-not-exist"></a>Senaryo: Belirtilen sınıf yok

`The specified class does not exist..` **/var/opt/microsoft/omsconfig/omsconfig.log'daki**hatayı görürseniz, Linux için Log Analytics aracısının güncellenmesi gerekir. Aracıyı yeniden yüklemek için aşağıdaki komutu çalıştırın:

```bash
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
```

## <a name="windows"></a>Windows

Windows Karma Runbook Worker, çalışanı kaydetmek, runbook işleri almak ve durum bildirmek için Otomasyon hesabınızla iletişim kurabilmesi [için Windows'un Günlük Analizi aracısına](../../azure-monitor/platform/log-analytics-agent.md) bağlıdır. İşçinin kaydı başarısız olursa, bu bölüm bazı olası nedenleri içerir.

### <a name="scenario-the-log-analytics-agent-for-windows-isnt-running"></a><a name="mma-not-running"></a>Senaryo: Windows için Log Analytics aracısı çalışmıyor

#### <a name="issue"></a>Sorun

`healthservice` Hybrid Runbook Worker makinesinde çalışmıyor.

#### <a name="cause"></a>Nedeni

Windows için Günlük Analizi çalışmıyorsa, Karma Runbook Çalışanı Azure Otomasyonu ile iletişim kuramaz.

#### <a name="resolution"></a>Çözüm

PowerShell'de aşağıdaki komutu girerek aracının `Get-Service healthservice`çalıştığını doğrulayın: . Hizmet durdurulursa, hizmeti başlatmak için PowerShell'e `Start-Service healthservice`aşağıdaki komutu girin: .

### <a name="scenario-event-4502-in-operations-manager-log"></a><a name="event-4502"></a>Senaryo: Operasyon Yöneticisi günlüğünde Olay 4502

#### <a name="issue"></a>Sorun

Uygulama **ve Hizmet Günlükleri\Operations Manager** olay günlüğünde, aşağıdaki açıklamayla birlikte `Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent` 4502 olay ve EventMessage'ı görürsünüz:<br>`The certificate presented by the service \<wsid\>.oms.opinsights.azure.com was not issued by a certificate authority used for Microsoft services. Please contact your network administrator to see if they are running a proxy that intercepts TLS/SSL communication.`

#### <a name="cause"></a>Nedeni

Bu sorun, proxy'nizin veya ağ güvenlik duvarının Microsoft Azure ile iletişimini engellemesi nedeniyle olabilir. Bilgisayarın 443 portundaki ***.azure-automation.net'a** giden erişimi olduğunu doğrulayın. 

#### <a name="resolution"></a>Çözüm

Günlükler **C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes'da**her karma işçide yerel olarak depolanır. **Uygulama ve Hizmet Günlükleri\Microsoft-SMA\İşlemler** ve **Uygulama ve Hizmet Günlükleri\Operations Manager** olay günlüklerinde herhangi bir uyarı veya hata olayı olup olmadığını doğrulayabilirsiniz. Bu günlükler, rolün Azure Otomasyonu'na kayda alımını etkileyen bir bağlantı veya başka bir sorun türünü veya normal işlemler altında karşılaşılan bir sorunu gösterir. Log Analytics aracısıyla ilgili ek yardım sorun giderme sorunları için, [Log Analytics Windows aracısıyla ilgili Sorun Giderme sorunlarına](../../azure-monitor/platform/agent-windows-troubleshoot.md)bakın.

Karma çalışanlar, Bulutta çalışan runbook işlerinin çıktı ve ileti gönderdiği gibi Azure Otomasyonu'na [Runbook çıktısı ve iletigönderir.](../automation-runbook-output-and-messages.md) Tıpkı runbook'larda yaptığınız gibi Verbose ve Progress akışlarını etkinleştirebilirsiniz.

### <a name="scenario-orchestratorsandboxexe-cant-connect-to-office-365-through-proxy"></a><a name="no-orchestrator-sandbox-connect-O365"></a>Senaryo: Orchestrator.Sandbox.exe proxy üzerinden Office 365'e bağlanamıyor

#### <a name="issue"></a>Sorun

Windows Karma Runbook Worker üzerinde çalışan bir komut dosyası, Bir Orchestrator kum havuzunda Office 365'e beklendiği gibi bağlanamıyor. Komut dosyası bağlantı için [Connect-MsolService](https://docs.microsoft.com/powershell/module/msonline/connect-msolservice?view=azureadps-1.0) kullanıyor. 

Proxy ve baypas listesini ayarlamak için **Orchestrator.Sandbox.exe.config** ayarlarsanız, sandbox hala düzgün bağlanmaz. Aynı proxy ve bypass listesi ayarlarına sahip **bir Powershell_ise.exe.config** dosyası beklediğiniz gibi çalışıyor gibi görünüyor. Service Management Automation (SMA) günlükleri ve PowerShell günlükleri proxy ile ilgili herhangi bir bilgi sağlamaz.

#### <a name="cause"></a>Nedeni

Sunucudaki Active Directory Federation Services (ADFS) bağlantısı proxy'yi atlayamamaktadır. Bir PowerShell kum havuzunun günlüğe kaydedilmiş kullanıcı olarak çalıştığını unutmayın. Ancak, bir Orchestrator sandbox ağır özelleştirilmiş ve **Orchestrator.Sandbox.exe.config** dosya ayarlarını yok sayabilir. Bu işleme makinesi veya MMA proxy ayarları için özel bir kodu vardır, ancak diğer özel proxy ayarlarını işlemek için değil. 

#### <a name="resolution"></a>Çözüm

PowerShell cmdlets için MSOnline modülü yerine Azure AD modüllerini kullanmak üzere komut dosyanızı geçirerek Orchestrator kum havuzu sorununu çözebilirsiniz. Bkz. [Orkestratörden Azure Otomasyonuna Geçiş (Beta)](https://docs.microsoft.com/azure/automation/automation-orchestrator-migration).

MSOnline modül cmdlets kullanmaya devam etmek istiyorsanız, [Invoke-Command](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/invoke-command?view=powershell-7)kullanmak için komut değiştirin. Parametreler ve `ComputerName` parametreler `Credential` için değerleri belirtin. 

```powershell
$Credential = Get-AutomationPSCredential -Name MyProxyAccessibleCredential
Invoke-Command -ComputerName $env:COMPUTERNAME -Credential $Credential 
{ Connect-MsolService … }
```

Bu kod değişikliği, belirtilen kimlik bilgileri bağlamında tamamen yeni bir PowerShell oturumu başlatır. Trafiğin etkin kullanıcının kimliğini doğrulayan bir proxy sunucusu ndan akmasını sağlamalıdır.

>[!NOTE]
>Bu çözüm, sandbox yapılandırma dosyasını manipüle etmeyi gereksiz kılar. Yapılandırma dosyasının komut dosyanızla çalışmasını sağlamada başarılı olsanız bile, Karma Runbook Worker aracısı her güncelleştirildiğinde dosya silinir.

### <a name="scenario-hybrid-runbook-worker-not-reporting"></a><a name="corrupt-cache"></a>Senaryo: Karma Runbook Worker raporlama değil

#### <a name="issue"></a>Sorun

Karma Runbook Worker makineniz çalışıyor, ancak çalışma alanında makinenin sinyal verilerini görmüyorsunuz.

Aşağıdaki örnek sorgu, çalışma alanındaki makineleri ve son kalp atışlarını gösterir:

```loganalytics
// Last heartbeat of each computer
Heartbeat
| summarize arg_max(TimeGenerated, *) by Computer
```

#### <a name="cause"></a>Nedeni

Bu sorun, Karma Runbook Worker'daki bozuk önbellekten kaynaklanabilir.

#### <a name="resolution"></a>Çözüm

Bu sorunu gidermek için, Karma Runbook Worker'da oturum açın ve aşağıdaki komut dosyasını çalıştırın. Bu komut dosyası, Windows için Log Analytics aracısını durdurur, önbelleğini kaldırır ve hizmeti yeniden başlatır. Bu eylem, Karma Runbook Çalışanı'nı yapılandırmasını Azure Otomasyonu'ndan yeniden indirmeye zorlar.

```powershell
Stop-Service -Name HealthService

Remove-Item -Path 'C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State' -Recurse

Start-Service -Name HealthService
```

### <a name="scenario-you-cant-add-a-hybrid-runbook-worker"></a><a name="already-registered"></a>Senaryo: Karma Runbook Worker ekemezsiniz

#### <a name="issue"></a>Sorun

`Add-HybridRunbookWorker` Cmdlet'i kullanarak Bir Karma Runbook Worker eklemeye çalışırken aşağıdaki iletiyi alırsınız.

```error
Machine is already registered
```

#### <a name="cause"></a>Nedeni

Makine zaten farklı bir Otomasyon hesabına kayıtlıysa veya bir makineden çıkardıktan sonra Karma Runbook Worker'ı okumaya çalışıyorsanız, bu soruna neden olabilir.

#### <a name="resolution"></a>Çözüm

Bu sorunu gidermek için aşağıdaki kayıt defteri anahtarını kaldırın, cmdlet'i `HealthService`yeniden başlatın ve cmdlet'i `Add-HybridRunbookWorker` yeniden deneyin.

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\HybridRunbookWorker`

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu yukarıda görmüyorsanız veya sorununuzu çözemiyorsanız, ek destek için aşağıdaki kanallardan birini deneyin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıt alın.
* [@AzureSupport](https://twitter.com/azuresupport)Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.
* Azure destek olayı dosyala. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.