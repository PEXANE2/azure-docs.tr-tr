---
title: Azure Otomasyonu karma runbook çalışanları sorunlarını giderme
description: Bu makalede, Azure Otomasyonu karma runbook çalışanları sorunlarını gidermeye yönelik bilgiler sağlanmaktadır.
services: automation
ms.service: automation
ms.subservice: ''
author: mgoedtel
ms.author: magoedte
ms.date: 11/25/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 6d734c910cc966cfd83f1e1c7f9cbd728643fbc4
ms.sourcegitcommit: 11572a869ef8dbec8e7c721bc7744e2859b79962
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82836521"
---
# <a name="troubleshoot-hybrid-runbook-workers"></a>Karma runbook çalışanları sorunlarını giderme

Bu makale, Azure Otomasyonu karma runbook çalışanları ile ilgili sorun giderme hakkında bilgi sağlar.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Azure Otomasyonu hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](../automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="general"></a>Genel

Karma Runbook Worker, çalışan, runbook işlerini alacak ve rapor durumunu kaydetmek için Azure Otomasyonu hesabınızla iletişim kurmak üzere bir aracıya bağımlıdır. Windows için bu aracı Windows için Log Analytics aracısıdır. Linux için bu, Linux için Log Analytics aracısıdır.

### <a name="scenario-runbook-execution-fails"></a><a name="runbook-execution-fails"></a>Senaryo: runbook yürütmesi başarısız oluyor

#### <a name="issue"></a>Sorun

Runbook yürütmesi başarısız olur ve aşağıdaki hata iletisini alırsınız:

```error
"The job action 'Activate' cannot be run, because the process stopped unexpectedly. The job action was attempted three times."
```

Runbook 'unuzu üç kez yürütmeyi denediğinde kısa süre sonra askıya alınır. Runbook 'un tamamlanmasını kesintiye uğratan durumlar vardır. İlgili hata iletisi herhangi bir ek bilgi içermeyebilir.

#### <a name="cause"></a>Nedeni

Olası nedenler şunlardır:

* Runbook 'lar yerel kaynaklarla kimlik doğrulaması yapamaz.
* Karma çalışanı bir ara sunucu veya güvenlik duvarının arkasında.
* Karma runbook çalışanını çalıştırmak için yapılandırılan bilgisayar, en düşük donanım gereksinimlerini karşılamıyor.

#### <a name="resolution"></a>Çözüm

Bilgisayarın 443 numaralı bağlantı noktasında ***. Azure-Automation.net** 'ye giden erişimi olduğunu doğrulayın.

Karma runbook çalışanı çalıştıran bilgisayarların, çalışan bu özelliği barındıracak şekilde yapılandırmadan önce en düşük donanım gereksinimlerini karşılaması gerekir. Runbook 'lar ve kullandıkları arka plan işlemi, sistemin aşırı kullanılmasına ve runbook iş gecikmeleri veya zaman aşımları oluşmasına neden olabilir.

Karma Runbook Worker özelliğinin çalıştırılacağı bilgisayarın en düşük donanım gereksinimlerini karşıladığını onaylayın. Varsa, karma Runbook Worker işlemlerinin ve pencerelerinin performansı arasındaki bağıntıyı öğrenmek için CPU ve bellek kullanımını izleyin. Herhangi bir bellek veya CPU baskısı, kaynakları yükseltme gereksinimini gösterebilir. Ayrıca, iş yükü beklentilerinin gerekli olduğunu belirtmesi durumunda en düşük gereksinimleri ve ölçeği destekleyen farklı bir bilgi işlem kaynağı da seçebilirsiniz.

Açıklamasıyla `Win32 Process Exited with code [4294967295]`ilgili bir olay için **Microsoft-SMA** olay günlüğü ' ne bakın. Bu hatanın nedeni, runbook 'larınızda kimlik doğrulamasını yapılandırmadığınıza veya karma Runbook Worker grubu için farklı çalıştır kimlik bilgilerini belirtmemiş olmanız olabilir. Runbook 'larınız için kimlik doğrulamasını doğru şekilde yapılandırdığınızdan emin olmak için [bir karma runbook çalışanındaki runbook 'Ların çalıştırıldığı](../automation-hrw-run-runbooks.md) runbook izinlerini gözden geçirin.

### <a name="scenario-event-15011-in-the-hybrid-runbook-worker"></a><a name="cannot-connect-signalr"></a>Senaryo: karma runbook çalışanında olay 15011

#### <a name="issue"></a>Sorun

Karma Runbook Worker, bir sorgu sonucunun geçerli olmadığını belirten Event 15011 alır. Çalışan, [SignalR sunucusuyla](https://docs.microsoft.com/aspnet/core/signalr/introduction?view=aspnetcore-3.1)bir bağlantı açmayı denediğinde aşağıdaki hata görüntülenir.

```error
[AccountId={c7d22bd3-47b2-4144-bf88-97940102f6ca}]
[Uri=https://cc-jobruntimedata-prod-su1.azure-automation.net/notifications/hub][Exception=System.TimeoutException: Transport timed out trying to connect
   at System.Runtime.ExceptionServices.ExceptionDispatchInfo.Throw()
   at System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)
   at JobRuntimeData.NotificationsClient.JobRuntimeDataServiceSignalRClient.<Start>d__45.MoveNext()
```

#### <a name="cause"></a>Nedeni

Karma Runbook Worker otomatik dağıtım çözümü için doğru yapılandırılmamış. Bu çözüm, sanal makineyi Log Analytics çalışma alanına bağlayan bir bölüm içerir. PowerShell betiği, abonelik içinde sağlanan ada sahip çalışma alanını arar. Bu durumda, Log Analytics çalışma alanı farklı bir abonelikte bulunur. Betik, çalışma alanını bulamıyor ve bir tane oluşturmaya çalışıyor, ancak ad zaten alınmış. Sonuç olarak, dağıtım başarısız olur.

#### <a name="resolution"></a>Çözüm

Bu sorunu çözmek için iki seçeneğiniz vardır:

* Başka bir abonelikteki Log Analytics çalışma alanını aramak için PowerShell betiğini değiştirin. Bu, gelecekte birçok karma Runbook Worker makinesi dağıtmayı planlıyorsanız iyi bir çözümdür.

* Çalışan makineyi bir Orchestrator korumalı alanında çalışacak şekilde el ile yapılandırın. Ardından, işlevleri test etmek için çalışan üzerinde Azure Otomasyonu hesabında oluşturulmuş bir runbook 'u çalıştırın.

### <a name="scenario-windows-azure-vms-automatically-dropped-from-a-hybrid-worker-group"></a><a name="vm-automatically-dropped"></a>Senaryo: Windows Azure VM 'Leri karma çalışanı grubundan otomatik olarak bırakılır

#### <a name="issue"></a>Sorun

Çalışan makine uzun süredir kapalıyken karma Runbook Worker veya VM 'lerini göremezsiniz.

#### <a name="cause"></a>Nedeni

Karma Runbook Worker makinesi, 30 günden daha uzun bir süre boyunca Azure Otomasyonu 'nu Pingdi. Sonuç olarak, Otomasyon karma Runbook Worker grubunu veya sistem çalışan grubunu temizlemiştir. 

#### <a name="resolution"></a>Çözüm

Çalışan makinesini başlatın ve ardından Azure Otomasyonu ile yeniden yükleyin. Runbook ortamının nasıl yükleneceğine ve Azure Otomasyonu 'na nasıl bağlanacağınız hakkında yönergeler için bkz. [Windows karma Runbook Worker dağıtımı](../automation-windows-hrw-install.md).

### <a name="scenario-no-certificate-was-found-in-the-certificate-store-on-the-hybrid-runbook-worker"></a><a name="no-cert-found"></a>Senaryo: karma runbook çalışanındaki sertifika deposunda sertifika bulunamadı

#### <a name="issue"></a>Sorun

Karma Runbook Worker üzerinde çalışan bir runbook şu hata iletisiyle başarısız oluyor:

```error
Connect-AzAccount : No certificate was found in the certificate store with thumbprint 0000000000000000000000000000000000000000
At line:3 char:1
+ Connect-AzAccount -ServicePrincipal -Tenant $Conn.TenantID -Appl ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Connect-AzAccount], ArgumentException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.Profile.ConnectAzAccountCommand
```
#### <a name="cause"></a>Nedeni

Bu hata, farklı çalıştır hesabı sertifikasının bulunmadığı bir karma runbook çalışanında çalışan bir runbook 'ta [Farklı Çalıştır hesabını](../manage-runas-account.md) kullanmayı denediğinizde oluşur. Karma runbook çalışanları varsayılan olarak sertifika varlığını yerel olarak içermez. Farklı Çalıştır hesabı, bu varlığın düzgün şekilde çalışmasını gerektirir.

#### <a name="resolution"></a>Çözüm

Karma runbook çalışanınız bir Azure sanal makinesi ise bunun yerine [yönetilen kimliklerle runbook kimlik doğrulamasını](../automation-hrw-run-runbooks.md#runbook-auth-managed-identities) kullanabilirsiniz. Bu senaryo, farklı çalıştır hesabı yerine Azure VM 'nin yönetilen kimliğini kullanarak Azure kaynaklarında kimlik doğrulaması yapma olanağı sunarak kimlik doğrulamasını basitleştirir. Karma Runbook Worker şirket içi bir makinedir ve farklı çalıştır hesabı sertifikasını makineye yüklemeniz gerekir. Sertifikayı yüklemeyi öğrenmek için [bir karma runbook çalışanında runbook 'Ları çalıştırma](../automation-hrw-run-runbooks.md)içindeki PowerShell runbook **Export-Runascercertificateatetohybridworker** ' ı çalıştırma adımlarına bakın.

### <a name="scenario-error-403-during-registration-of-a-hybrid-runbook-worker"></a><a name="error-403-on-registration"></a>Senaryo: karma Runbook Worker kaydı sırasında hata 403

#### <a name="issue"></a>Sorun

Çalışan ilk kayıt aşaması başarısız olur ve şu hatayı alırsınız (403):

```error
"Forbidden: You don't have permission to access / on this server."
```

#### <a name="cause"></a>Nedeni

Olası nedenler şunlardır:

* Aracının ayarlarında yanlış yazılmış bir çalışma alanı KIMLIĞI veya çalışma alanı anahtarı (birincil) var. 
* Karma Runbook Worker, bir hesap bağlama hatasına neden olan yapılandırmayı indiremez. Azure çözümleri etkinleştirdiğinde, yalnızca bir Log Analytics çalışma alanı ve bir Otomasyon hesabı bağlamak için yalnızca belirli bölgeleri destekler. Bilgisayarda yanlış bir tarih veya saat kümesi de mümkündür. Süre artı veya geçerli saatten 15 dakikadan fazla olursa ekleme başarısız olur.

#### <a name="resolution"></a>Çözüm

##### <a name="mistyped-workspace-id-or-key"></a>Yanlış yazılmış çalışma alanı KIMLIĞI veya anahtarı
Aracının çalışma alanı KIMLIĞI veya çalışma alanı anahtarının yanlış yazılmış olup olmadığını doğrulamak için, bkz. [bir çalışma alanı ekleme veya kaldırma –](../../azure-monitor/platform/agent-manage.md#windows-agent) Windows Aracısı için Windows Aracısı veya [bir çalışma alanı ekleme veya kaldırma –](../../azure-monitor/platform/agent-manage.md#linux-agent) Linux Aracısı için Linux Aracısı. Azure portal tam dizeyi seçtiğinizden emin olun ve dikkatlice kopyalayıp yapıştırın.

##### <a name="configuration-not-downloaded"></a>Yapılandırma indirilmedi

Log Analytics çalışma alanınız ve otomasyon hesabınızın bağlı bir bölgede olması gerekir. Desteklenen bölgelerin listesi için bkz. [Azure Otomasyonu ve Log Analytics çalışma alanı eşlemeleri](../how-to/region-mappings.md).

Ayrıca, bilgisayarınızın tarih veya saat dilimini güncelleştirmeniz gerekebilir. Özel bir zaman aralığı seçerseniz, aralığın UTC 'de olduğundan emin olun, bu da yerel saat diliminizden farklı olabilir.

## <a name="linux"></a>Linux

Linux hibrit Runbook Worker, çalışan, runbook işlerini alacak ve durumu raporlamak üzere otomasyon hesabınızla iletişim kurmak için [Linux 'un Log Analytics aracısına](../../azure-monitor/platform/log-analytics-agent.md) bağımlıdır. Çalışan kaydı başarısız olursa, hatanın olası nedenleri aşağıda verilmiştir.

### <a name="scenario-linux-hybrid-runbook-worker-receives-prompt-for-a-password-when-signing-a-runbook"></a><a name="prompt-for-password"></a>Senaryo: Linux karma Runbook Worker, bir runbook imzalanırken parola istemi alır

#### <a name="issue"></a>Sorun

Linux karma `sudo` Runbook Worker için komutu çalıştırmak, parola için beklenmeyen bir istem alır.

#### <a name="cause"></a>Nedeni

Linux için Log Analytics aracısına ait **nxautomationuser** hesabı, **sudoers** dosyasında doğru şekilde yapılandırılmamış. Karma Runbook Worker 'ın, Linux Runbook Worker 'daki runbook 'ları imzalayabilmesi için hesap izinlerinin ve diğer verilerin uygun yapılandırmasına ihtiyacı vardır.

#### <a name="resolution"></a>Çözüm

* Karma runbook çalışanının makinede GnuPG (GPG) yürütülebilirine sahip olduğundan emin olun.

* **Sudoers** dosyasındaki **nxautomationuser** hesabının yapılandırmasını doğrulayın. Bkz. [runbook 'Ları karma Runbook Worker üzerinde çalıştırma](../automation-hrw-run-runbooks.md).

### <a name="scenario-the-log-analytics-agent-for-linux-isnt-running"></a><a name="oms-agent-not-running"></a>Senaryo: Linux için Log Analytics Aracısı çalışmıyor

#### <a name="issue"></a>Sorun

Linux için Log Analytics Aracısı çalışmıyor.

#### <a name="cause"></a>Nedeni

Aracı çalışmıyorsa, Linux hibrit Runbook Worker 'ın Azure Otomasyonu ile iletişim kurmasını engeller. Aracı çeşitli nedenlerle çalışmıyor olabilir.

#### <a name="resolution"></a>Çözüm

 Komutu `ps -ef | grep python`girerek aracının çalıştığını doğrulayın. Aşağıdakine benzer bir çıktı görmeniz gerekir. Python, **nxautomation** kullanıcı hesabıyla işlenir. Güncelleştirme Yönetimi veya Azure Otomasyonu çözümü etkinleştirilmemişse, aşağıdaki işlemlerden hiçbiri çalışmıyor.

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

Aşağıdaki listede bir Linux karma Runbook Worker için başlatılan süreçler gösterilmektedir. Bunlar tamamen/var/seçenek/Microsoft/omsagent/State/automationworker/dizininde bulunur.

* **OMS. conf**: Worker Manager işlemi. Doğrudan DSC 'den başlatılır.
* **Worker. conf**: otomatik kaydedilmiş karma çalışan işlemi. Çalışan Yöneticisi tarafından başlatılır. Bu işlem, Güncelleştirme Yönetimi tarafından kullanılır ve Kullanıcı için saydamdır. Güncelleştirme Yönetimi çözümü makinede etkinleştirilmemişse bu işlem yoktur.
* **diy/Worker. conf**: diy karma çalışan işlemi. Karma Runbook Worker 'daki Kullanıcı runbook 'larını yürütmek için DIY karma çalışan işlemi kullanılır. Bu, yalnızca farklı bir yapılandırma kullandığı anahtar ayrıntısıyla otomatik kaydedilmiş karma çalışan işleminden farklıdır. Bu işlem, Azure Otomasyonu çözümü devre dışıysa ve DIY Linux hibrit çalışanı kayıtlı değilse mevcut değildir.

Aracı çalışmıyorsa, hizmeti başlatmak için şu komutu çalıştırın: `sudo /opt/microsoft/omsagent/bin/service_control restart`.

### <a name="scenario-the-specified-class-doesnt-exist"></a><a name="class-does-not-exist"></a>Senaryo: belirtilen sınıf yok

`The specified class does not exist..` **/Var/seçenek/Microsoft/omsconfig/omsconfig.log**dosyasında hata iletisini görürseniz, Linux için Log Analytics aracısının güncelleştirilmesi gerekir. Aracıyı yeniden yüklemek için aşağıdaki komutu çalıştırın.

```bash
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
```

## <a name="windows"></a>Windows

Windows hibrit Runbook Worker, çalışan, runbook işlerini alacak ve durumu raporlamak üzere otomasyon hesabınızla iletişim kurması için [Windows 'un Log Analytics aracısına](../../azure-monitor/platform/log-analytics-agent.md) bağımlıdır. Çalışan kaydı başarısız olursa, bu bölümde bazı olası nedenler bulunur.

### <a name="scenario-the-log-analytics-agent-for-windows-isnt-running"></a><a name="mma-not-running"></a>Senaryo: Windows için Log Analytics Aracısı çalışmıyor

#### <a name="issue"></a>Sorun

`healthservice` Karma Runbook Worker makinesinde çalışmıyor.

#### <a name="cause"></a>Nedeni

Windows hizmeti için Log Analytics çalışmıyorsa, karma Runbook Worker Azure Otomasyonu ile iletişim kuramaz.

#### <a name="resolution"></a>Çözüm

PowerShell 'de aşağıdaki komutu girerek aracının çalıştığını doğrulayın: `Get-Service healthservice`. Hizmet durdurulmuşsa, hizmeti başlatmak için PowerShell 'de aşağıdaki komutu girin: `Start-Service healthservice`.

### <a name="scenario-event-4502-in-the-operations-manager-log"></a><a name="event-4502"></a>Senaryo: Operations Manager günlüğünde olay 4502

#### <a name="issue"></a>Sorun

**Uygulama ve hizmetler Logs\Operations Manager** olay günlüğünde, olay 4502 ve aşağıdaki açıklamayla içeren `Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent` bir olay iletisi görürsünüz:<br>`The certificate presented by the service \<wsid\>.oms.opinsights.azure.com was not issued by a certificate authority used for Microsoft services. Please contact your network administrator to see if they are running a proxy that intercepts TLS/SSL communication.`

#### <a name="cause"></a>Nedeni

Bu sorunun nedeni, proxy 'niz veya ağ güvenlik duvarınız Microsoft Azure ile iletişimi engelliyor olabilir. Bilgisayarın 443 numaralı bağlantı noktasında ***. Azure-Automation.net** 'ye giden erişimi olduğunu doğrulayın.

#### <a name="resolution"></a>Çözüm

Günlükler, C:\ProgramData\Microsoft\System Center\orchestrator\7.2\sma\sandboxeson konumundaki her karma çalışan üzerinde yerel olarak depolanır. **Uygulama ve hizmetler Logs\Microsoft-SMA\Operations** ve **uygulama ve hizmetler logs\operations Manager** olay günlüklerinde bir uyarı veya hata olayı olup olmadığını doğrulayabilirsiniz. Bu Günlükler, rolün Azure Otomasyonu 'na verilmesinin veya normal işlemlerde karşılaşılan bir sorundan etkilenen bir bağlantıyı veya diğer tür bir sorunu belirtir. Log Analytics aracısıyla ilgili sorunları gidermeye yönelik ek yardım için bkz. [Log Analytics Windows Agent ile ilgili sorunları giderme](../../azure-monitor/platform/agent-windows-troubleshoot.md).

Hibrit çalışanlar, bulutta çalışan runbook işlerinin çıkış ve mesaj gönderme gibi runbook [çıkışlarını ve Iletileri](../automation-runbook-output-and-messages.md) Azure Otomasyonu 'na gönderir. Ayrıntılı ve Ilerleme akışlarını runbook 'lar için yaptığınız gibi etkinleştirebilirsiniz.

### <a name="scenario-orchestratorsandboxexe-cant-connect-to-office-365-through-proxy"></a><a name="no-orchestrator-sandbox-connect-O365"></a>Senaryo: Orchestrator. Sandbox. exe, proxy üzerinden Office 365 'e bağlanamıyor

#### <a name="issue"></a>Sorun

Windows karma Runbook Worker üzerinde çalışan bir betik, bir Orchestrator korumalı alanı üzerinde Office 365 ' e istendiği şekilde bağlanamaz. Betik, bağlantı için [Connect-MsolService](https://docs.microsoft.com/powershell/module/msonline/connect-msolservice?view=azureadps-1.0) ' i kullanıyor. 

Proxy ve atlama listesini ayarlamak için **Orchestrator. Sandbox. exe. config** ' i ayarlarsanız, korumalı alan hala düzgün şekilde bağlanmaz. Aynı ara sunucu ve atlama listesi ayarlarına sahip bir **Powershell_ise. exe. config** dosyası, beklendiği gibi çalışır. Service Management Automation (SMA) günlükleri ve PowerShell günlükleri ara sunucu ile ilgili herhangi bir bilgi sağlamaz.

#### <a name="cause"></a>Nedeni

Sunucu üzerindeki Active Directory Federasyon Hizmetleri (AD FS) (AD FS) bağlantısı proxy 'yi atlayamaz. Bir PowerShell korumalı alanının, oturum açmış kullanıcı olarak çalıştığını unutmayın. Ancak, bir Orchestrator korumalı alanı yoğun bir şekilde özelleştirilir ve **Orchestrator. Sandbox. exe. config** dosya ayarlarını yok sayabilir. Makine veya Log Analytics Aracısı proxy ayarlarını işlemek için özel koda sahiptir, ancak diğer özel proxy ayarlarını işlemeye yönelik değildir. 

#### <a name="resolution"></a>Çözüm

Komut dosyanızı PowerShell cmdlet 'leri için MSOnline modülü yerine Azure Active Directory modüllerini kullanacak şekilde geçirerek Orchestrator korumalı alanı için sorunu çözebilirsiniz. Daha fazla bilgi için bkz. [Orchestrator 'Dan Azure Otomasyonu 'na (Beta) geçiş](https://docs.microsoft.com/azure/automation/automation-orchestrator-migration).

MSOnline modülü cmdlet 'lerini kullanmaya devam etmek istiyorsanız, komut dosyanızı [Invoke-komutunu](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/invoke-command?view=powershell-7)kullanacak şekilde değiştirin. `ComputerName` Ve `Credential` parametreleri için değerleri belirtin. 

```powershell
$Credential = Get-AutomationPSCredential -Name MyProxyAccessibleCredential
Invoke-Command -ComputerName $env:COMPUTERNAME -Credential $Credential 
{ Connect-MsolService … }
```

Bu kod değişikliği, belirtilen kimlik bilgileri bağlamında tamamen yeni bir PowerShell oturumu başlatır. Trafiğin, etkin kullanıcının kimliğini doğrulayan bir ara sunucu üzerinden akmasını sağlamalıdır.

>[!NOTE]
>Bu çözüm, korumalı alan yapılandırma dosyasını işlemeyi gereksiz hale getirir. Yapılandırma dosyasını betiğiyle birlikte çalışırken başarılı olsanız bile, karma Runbook Worker aracısının her güncelleştirildiği her seferinde dosya silinir.

### <a name="scenario-hybrid-runbook-worker-not-reporting"></a><a name="corrupt-cache"></a>Senaryo: karma Runbook Worker raporlama değil

#### <a name="issue"></a>Sorun

Karma Runbook Worker makineniz çalışıyor, ancak çalışma alanında makine için sinyal verisi görmüyor.

Aşağıdaki örnek sorguda, bir çalışma alanındaki makineler ve bunların son sinyali gösterilmektedir:

```loganalytics
// Last heartbeat of each computer
Heartbeat
| summarize arg_max(TimeGenerated, *) by Computer
```

#### <a name="cause"></a>Nedeni

Bu sorun, karma Runbook Worker 'daki bozuk bir önbellekten kaynaklanıyor olabilir.

#### <a name="resolution"></a>Çözüm

Bu sorunu çözmek için karma runbook çalışanında oturum açın ve aşağıdaki betiği çalıştırın. Bu betik, Windows için Log Analytics aracısını durduruyor, önbelleğini kaldırır ve hizmeti yeniden başlatır. Bu eylem, karma runbook çalışanını Azure Otomasyonu 'ndan yapılandırmasını yeniden indirmeye zorlar.

```powershell
Stop-Service -Name HealthService

Remove-Item -Path 'C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State' -Recurse

Start-Service -Name HealthService
```

### <a name="scenario-you-cant-add-a-hybrid-runbook-worker"></a><a name="already-registered"></a>Senaryo: karma Runbook Worker ekleyemezsiniz

#### <a name="issue"></a>Sorun

`Add-HybridRunbookWorker` Cmdlet 'ini kullanarak bir karma Runbook Worker eklemeye çalıştığınızda aşağıdaki iletiyi alırsınız:

```error
Machine is already registered
```

#### <a name="cause"></a>Nedeni

Bu sorun, makinenin farklı bir Otomasyon hesabıyla zaten kayıtlı olması veya bir makineden kaldırıldıktan sonra karma Runbook Worker 'ı yeniden eklemeye çalışırsanız meydana gelir.

#### <a name="resolution"></a>Çözüm

Bu sorunu çözmek için, aşağıdaki kayıt defteri anahtarını kaldırın, yeniden `HealthService`başlatın ve `Add-HybridRunbookWorker` cmdlet 'i yeniden deneyin.

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\HybridRunbookWorker`

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu burada görmüyorsanız veya sorununuzu çözemezseniz, ek destek için aşağıdaki kanallardan birini deneyin:

* Azure [forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıtlar alın.
* [@AzureSupport](https://twitter.com/azuresupport)Müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabıyla bağlanın. Azure desteği, Azure Community 'yi yanıtlar, destek ve uzmanlar için bağlar.
* Azure destek olayı dosyası oluşturma. [Azure destek sitesine](https://azure.microsoft.com/support/options/)gidin ve **Destek Al**' ı seçin.
