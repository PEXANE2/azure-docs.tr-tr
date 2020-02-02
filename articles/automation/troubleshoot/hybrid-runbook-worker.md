---
title: Sorun giderme-Azure Otomasyonu karma runbook çalışanları
description: Bu makalede, Azure Otomasyonu karma runbook çalışanları sorunlarını giderme hakkında bilgi sağlanır
services: automation
ms.service: automation
ms.subservice: ''
author: mgoedtel
ms.author: magoedte
ms.date: 11/25/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: a5885df67464095061d9a95aa59010a1629fb8f8
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76930341"
---
# <a name="troubleshoot-hybrid-runbook-workers"></a>Karma runbook çalışanları sorunlarını giderme

Bu makale, karma runbook çalışanları ile ilgili sorun giderme hakkında bilgi sağlar.

## <a name="general"></a>Genel

Karma Runbook Worker, çalışan, runbook işlerini alacak ve durumu raporlamak üzere otomasyon hesabınızla iletişim kurmak için bir aracıya bağımlıdır. Windows için bu aracı, Windows için Log Analytics aracısıdır (Ayrıca, Microsoft Monitoring Agent (MMA) olarak da adlandırılır). Linux için bu, Linux için Log Analytics aracısıdır.

### <a name="runbook-execution-fails"></a>Senaryo: runbook yürütmesi başarısız oluyor

#### <a name="issue"></a>Sorun

Runbook yürütmesi başarısız olur ve şu hatayı alırsınız:

```error
"The job action 'Activate' cannot be run, because the process stopped unexpectedly. The job action was attempted three times."
```

Runbook 'unuzu üç kez yürütmeyi denediğinde kısa süre sonra askıya alınır. Runbook 'un tamamlanmasını kesintiye uğratan durumlar vardır. İlgili hata iletisi herhangi bir ek bilgi içermeyebilir.

#### <a name="cause"></a>Nedeni

Olası olası nedenler şunlardır:

* Runbook 'lar yerel kaynaklarla kimlik doğrulayamıyorum

* Karma çalışanı bir ara sunucu veya güvenlik duvarının arkasında

* Runbook 'lar yerel kaynaklarla kimlik doğrulayamıyorum

* Karma Runbook Worker özelliğini çalıştıracak şekilde yapılandırılan bilgisayar, en düşük donanım gereksinimlerini karşılamıyor.

#### <a name="resolution"></a>Çözünürlük

443 numaralı bağlantı noktasında, bilgisayarın *. azure-automation.net 'ye giden erişimi olduğunu doğrulayın.

Karma runbook çalışanı çalıştıran bilgisayarların, çalışan bu özelliği barındıracak şekilde yapılandırmadan önce en düşük donanım gereksinimlerini karşılaması gerekir. Runbook 'lar ve kullandıkları arka plan işlemi sistemin aşırı kullanılmasına ve runbook iş gecikmelerine veya zaman aşımına neden olmasına neden olabilir.

Karma Runbook Worker özelliğinin çalışacağı bilgisayarın en düşük donanım gereksinimlerini karşıladığını onaylayın. Varsa, karma Runbook Worker işlemlerinin ve pencerelerinin performansı arasındaki bağıntıyı öğrenmek için CPU ve bellek kullanımını izleyin. Herhangi bir bellek veya CPU baskısı, kaynakları yükseltme gereksinimini gösterebilir. Ayrıca, iş yükü beklentilerinin bir artış belirtmesi durumunda en düşük gereksinimleri destekleyebilen ve ölçeklendirilen farklı bir işlem kaynağı seçebilirsiniz.

# *[4294967295] kodlu Win32 Işlemine çıkıldı*açıklaması ile ilgili bir olay için **Microsoft-SMA** olay günlüğü 'nü denetleyin. Bu hatanın nedeni, runbook 'larınızda kimlik doğrulaması yapılandırmadınız veya karma çalışan grubu için farklı çalıştır kimlik bilgilerini belirtmezsiniz. Runbook 'larınız için kimlik doğrulamasını doğru şekilde yapılandırdığınızdan emin olmak için [runbook izinlerini](../automation-hrw-run-runbooks.md#runbook-permissions) gözden geçirin.

### <a name="no-cert-found"></a>Senaryo: karma Runbook Worker 'daki sertifika deposunda sertifika bulunamadı

#### <a name="issue"></a>Sorun

Karma Runbook Worker üzerinde çalışan bir runbook şu hata iletisiyle başarısız oluyor:

```error
Connect-AzureRmAccount : No certificate was found in the certificate store with thumbprint 0000000000000000000000000000000000000000
At line:3 char:1
+ Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -Appl ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Connect-AzureRmAccount], ArgumentException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.Profile.ConnectAzureRmAccountCommand
```

#### <a name="cause"></a>Nedeni

Bu hata, farklı çalıştır hesabı sertifikasının bulunmadığı bir karma runbook çalışanında çalışan bir runbook 'ta [Farklı Çalıştır hesabını](../manage-runas-account.md) kullanmaya çalıştığınızda oluşur. Karma runbook çalışanları varsayılan olarak, farklı çalıştır hesabının düzgün çalışması için gerekli olan sertifika varlığını yerel olarak içermez.

#### <a name="resolution"></a>Çözünürlük

Karma runbook çalışanınız bir Azure sanal makinesi ise bunun yerine [Azure kaynakları Için Yönetilen kimlikler](../automation-hrw-run-runbooks.md#managed-identities-for-azure-resources) kullanabilirsiniz. Bu senaryo, farklı çalıştır hesabı yerine Azure VM 'nin yönetilen kimliğini kullanarak Azure kaynaklarında kimlik doğrulaması yapma olanağı sunarak kimlik doğrulamasını basitleştirir. Karma Runbook Worker şirket içi bir makinedir ve farklı çalıştır hesabı sertifikasını makineye yüklemeniz gerekir. Sertifikayı yüklemeyi öğrenmek için [bir karma runbook çalışanındaki runbook 'ları](../automation-hrw-run-runbooks.md)çalıştırmak için PowerShell runbook 'Unu dışarı aktarma-Runascercertificateatetohybridworker ' ı çalıştırma adımlarına bakın.

## <a name="linux"></a>Linux

Linux hibrit Runbook Worker, çalışan, runbook işlerini alacak ve durumu raporlamak üzere otomasyon hesabınızla iletişim kurmak için [Linux 'un Log Analytics aracısına](../../azure-monitor/platform/log-analytics-agent.md) bağımlıdır. Çalışan kaydı başarısız olursa, hatanın olası nedenleri aşağıda verilmiştir:

### <a name="oms-agent-not-running"></a>Senaryo: Linux için Log Analytics Aracısı çalışmıyor

#### <a name="issue"></a>Sorun

Linux için Log Analytics Aracısı çalışmıyor

#### <a name="cause"></a>Nedeni

Aracı çalışmıyorsa, Linux hibrit Runbook Worker 'ın Azure Otomasyonu ile iletişim kurmasını engeller. Aracı çeşitli nedenlerle çalışmıyor olabilir.

#### <a name="resolution"></a>Çözünürlük

 Aşağıdaki komutu girerek aracının çalıştığını doğrulayın: `ps -ef | grep python`. Aşağıdakine benzer bir çıktı görmeniz gerekir, Python, **nxautomation** Kullanıcı hesabı ile işlenir. Güncelleştirme Yönetimi veya Azure Otomasyonu çözümleri etkinleştirilmemişse, aşağıdaki işlemlerden hiçbiri çalışmıyor.

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

Aşağıdaki listede bir Linux karma Runbook Worker için başlatılan süreçler gösterilmektedir. Bunlar `/var/opt/microsoft/omsagent/state/automationworker/` dizinde bulunur.


* **OMS. conf** -bu değer çalışan Yöneticisi işlemidir. Doğrudan DSC 'den başlatılır.

* **Worker. conf** -bu Işlem, otomatik kayıtlı karma çalışan sürecdir, çalışan Yöneticisi tarafından başlatılır. Bu işlem, Güncelleştirme Yönetimi tarafından kullanılır ve Kullanıcı için saydamdır. Güncelleştirme Yönetimi çözümü makinede etkinleştirilmemişse bu işlem yoktur.

* **diy/Worker. conf** -bu Işlem, diy karma çalışan işlemidir. Karma Runbook Worker 'daki Kullanıcı runbook 'larını yürütmek için DIY karma çalışan işlemi kullanılır. Bu, yalnızca farklı bir yapılandırma kullandığı anahtar ayrıntısı içindeki otomatik kayıtlı karma çalışan işleminden farklıdır. Bu işlem, Azure Otomasyonu çözümü devre dışıysa ve DIY Linux hibrit çalışanı kayıtlı değilse mevcut değildir.

Aracı çalışmıyorsa, hizmeti başlatmak için şu komutu çalıştırın: `sudo /opt/microsoft/omsagent/bin/service_control restart`.

### <a name="error-403-on-registration"></a>Senaryo: karma Runbook Worker kaydı sırasında hata 403

#### <a name="issue"></a>Sorun

Çalışan ilk kayıt aşaması başarısız olur ve şu hatayı alırsınız (403).

```error
"Forbidden: You don't have permission to access / on this server."
```

#### <a name="cause"></a>Nedeni

Olası nedenler şunlardır:
* Aracının ayarlarında yanlış yazılmış bir çalışma alanı KIMLIĞI veya çalışma alanı anahtarı (birincil) var. 
* Karma runbook çalışanı yapılandırmayı indiremez ve hesap bağlama hatasına neden olur. Azure çözümleri etkinleştirdiğinde, yalnızca bir Log Analytics çalışma alanı ve bir Otomasyon hesabı bağlamak için yalnızca belirli bölgeleri destekler. Bilgisayarda yanlış bir tarih ve/veya saat kümesi de mümkündür. Saat, geçerli zamandan itibaren +/-15 dakika ise, ekleme başarısız olur.

#### <a name="resolution"></a>Çözünürlük

##### <a name="mistyped-workspace-idkey"></a>Yanlış yazılan çalışma alanı KIMLIĞI/anahtarı
Aracının çalışma alanı KIMLIĞI veya çalışma alanı anahtarının yanlış yazılmış olup olmadığını doğrulamak için, bkz. [bir çalışma alanı ekleme veya kaldırma –](../../azure-monitor/platform/agent-manage.md#windows-agent) Windows Aracısı için Windows Aracısı veya [bir çalışma alanı ekleme veya kaldırma –](../../azure-monitor/platform/agent-manage.md#linux-agent) Linux Aracısı için Linux Aracısı.  Azure portal tam dizeyi seçtiğinizden emin olun ve dikkatle kopyalayıp yapıştırın.

##### <a name="configuration-not-downloaded"></a>Yapılandırma indirilmedi

Log Analytics çalışma alanınız ve otomasyon hesabınızın bağlı bir bölgede olması gerekir. Desteklenen bölgelerin listesi için bkz. [Azure Otomasyonu ve Log Analytics çalışma alanı eşlemeleri](../how-to/region-mappings.md).

Ayrıca, bilgisayarınızın tarih ve saat dilimini de güncelleştirmeniz gerekebilir. Özel bir zaman aralığı seçerseniz, aralığın UTC 'de olduğundan emin olun, bu da yerel saat diliminizden farklı olabilir.

### <a name="class-does-not-exist"></a>Senaryo: belirtilen sınıf yok

Hatayı görürseniz **belirtilen sınıf yok..** `/var/opt/microsoft/omsconfig/omsconfig.log`, Linux için Log Analytics aracısının güncelleştirilmesi gerekir. Aracıyı yeniden yüklemek için aşağıdaki komutu çalıştırın:

```bash
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
```

## <a name="windows"></a>Windows

Windows hibrit Runbook Worker, çalışan, runbook işlerini alacak ve durumu raporlamak üzere otomasyon hesabınızla iletişim kurması için [Windows 'un Log Analytics aracısına](../../azure-monitor/platform/log-analytics-agent.md) bağımlıdır. Çalışan kaydı başarısız olursa, hatanın olası nedenleri aşağıda verilmiştir:

### <a name="mma-not-running"></a>Senaryo: Microsoft Monitoring Agent çalışmıyor

#### <a name="issue"></a>Sorun

Karma Runbook Worker makinesinde `healthservice` hizmeti çalışmıyor.

#### <a name="cause"></a>Nedeni

Microsoft Monitoring Agent Microsoft hizmeti çalışmıyorsa, bu durum karma Runbook Worker 'ın Azure Otomasyonu ile iletişim kurmasını engeller.

#### <a name="resolution"></a>Çözünürlük

PowerShell 'de aşağıdaki komutu girerek aracının çalıştığını doğrulayın: `Get-Service healthservice`. Hizmet durdurulmuşsa, hizmeti başlatmak için PowerShell 'de aşağıdaki komutu girin: `Start-Service healthservice`.

### <a name="event-4502"></a>Senaryo: Operations Manager günlüğünde olay 4502

#### <a name="issue"></a>Sorun

**Uygulama ve hizmetler Logs\Operations Manager** olay günlüğünde, şu açıklamayla **Microsoft. EnterpriseManagement. HealthService. AzureAutomation. Hybridadgent** Içeren olay 4502 ve eventmessage iletisini görürsünüz: *WSID\>. OMS.opinsights.Azure.com \<hizmeti tarafından sunulan sertifika Microsoft Hizmetleri için kullanılan bir sertifika yetkilisi tarafından verilmemiş. Lütfen, TLS/SSL iletişimini engelleyen bir proxy çalıştırıp çalıştırmadığını görmek için ağ yöneticinize başvurun.*

#### <a name="cause"></a>Nedeni

Bu sorunun nedeni, proxy 'niz veya ağ güvenlik duvarınız Microsoft Azure ile iletişimi engelliyor olabilir. 443 bağlantı noktalarında, bilgisayarın *. azure-automation.net 'ye giden erişimi olduğunu doğrulayın. 

#### <a name="resolution"></a>Çözünürlük

Günlükler, C:\ProgramData\Microsoft\System Center\orchestrator\7.2\sma\sandboxeson konumundaki her karma çalışan üzerinde yerel olarak depolanır. **Uygulama ve hizmetler Logs\Microsoft-SMA\Operations** ve **Application and Services logs\operations Manager** olay günlüğünde bir uyarı veya hata olayı olup olmadığını kontrol edebilirsiniz. Bu, normal Işlemler altında rolü Azure Otomasyonu veya sorununa eklemeyi etkileyen bir bağlantı ya da başka bir sorunu gösterir. Log Analytics aracısıyla ilgili sorunları gidermeye yönelik ek yardım için bkz. [Log Analytics Windows Agent ile ilgili sorunları giderme](../../azure-monitor/platform/agent-windows-troubleshoot.md).

[Runbook çıkışı ve iletileri](../automation-runbook-output-and-messages.md) , yalnızca bulutta çalışan runbook işleri gibi karma çalışanların Azure Otomasyonu 'na gönderilir. Ayrıca ayrıntılı ve Ilerleme akışlarını diğer runbook 'larda yaptığınız şekilde de etkinleştirebilirsiniz.

### <a name="corrupt-cache"></a>Senaryo: karma Runbook Worker raporlama değil

#### <a name="issue"></a>Sorun

Karma Runbook Worker makineniz çalışıyor ancak çalışma alanındaki makinenin sinyal verilerini görmüyor olabilirsiniz.

Aşağıdaki örnek sorguda, bir çalışma alanındaki makineler ve bunların son sinyali gösterilmektedir:

```loganalytics
// Last heartbeat of each computer
Heartbeat
| summarize arg_max(TimeGenerated, *) by Computer
```

#### <a name="cause"></a>Nedeni

Bu sorun, karma Runbook Worker 'daki bozuk bir önbellekten kaynaklanıyor olabilir.

#### <a name="resolution"></a>Çözünürlük

Bu sorunu çözmek için karma runbook çalışanında oturum açın ve aşağıdaki betiği çalıştırın. Bu betik Microsoft Monitoring Agent sonlandırır, önbelleğini kaldırır ve hizmeti yeniden başlatır. Bu eylem, karma Runbook Worker 'ın yapılandırmasını Azure Otomasyonu 'ndan yeniden indirmesini zorlar.

```powershell
Stop-Service -Name HealthService

Remove-Item -Path 'C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State' -Recurse

Start-Service -Name HealthService
```

### <a name="already-registered"></a>Senaryo: karma Runbook Worker ekleyemezsiniz

#### <a name="issue"></a>Sorun

`Add-HybridRunbookWorker` cmdlet 'ini kullanarak bir karma runbook çalışanı eklemeye çalışırken aşağıdaki iletiyi alırsınız.

```error
Machine is already registered
```

#### <a name="cause"></a>Nedeni

Bu sorun, makinenin farklı bir Otomasyon hesabıyla zaten kayıtlı olması veya bir makineden kaldırıldıktan sonra karma Runbook Worker 'ı yeniden eklemeye çalışırsanız meydana gelir.

#### <a name="resolution"></a>Çözünürlük

Bu sorunu çözmek için, aşağıdaki kayıt defteri anahtarını kaldırın ve `HealthService` yeniden başlatın ve `Add-HybridRunbookWorker` cmdlet 'ini yeniden deneyin:

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\HybridRunbookWorker`

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayı dosyası gönderebilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**' ı seçin.