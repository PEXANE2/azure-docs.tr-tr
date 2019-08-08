---
title: Güncelleştirme Yönetimi hata giderme sorunları
description: Güncelleştirme Yönetimi sorunları nasıl giderebileceğinizi öğrenin
services: automation
author: bobbytreed
ms.author: robreed
ms.date: 05/31/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 884ded67c25aca78225baef2d7e4c5de1cc94fd0
ms.sourcegitcommit: f7998db5e6ba35cbf2a133174027dc8ccf8ce957
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68782283"
---
# <a name="troubleshooting-issues-with-update-management"></a>Güncelleştirme Yönetimi sorunlarını giderme

Bu makalede, Güncelleştirme Yönetimi kullanırken üzerinde çalıştırabileceğiniz sorunları gidermeye yönelik çözümler ele alınmaktadır.

Temeldeki sorunu belirlemede karma çalışan Aracısı için bir aracı sorun giderici vardır. Sorun giderici hakkında daha fazla bilgi için bkz. [Güncelleştirme Aracısı sorunlarını giderme](update-agent-issues.md). Diğer tüm sorunlar için, olası sorunlar hakkında aşağıdaki ayrıntılı bilgilere bakın.

## <a name="general"></a>Genel

### <a name="components-enabled-not-working"></a>Senaryon ' Güncelleştirme Yönetimi ' çözümünün bileşenleri etkinleştirildi ve şimdi bu sanal makine yapılandırılıyor

#### <a name="issue"></a>Sorun

Daha sonra bir sanal makinede 15 dakika sonra şu iletiyi görmeye devam edersiniz:

```error
The components for the 'Update Management' solution have been enabled, and now this virtual machine is being configured. Please be patient, as this can sometimes take up to 15 minutes.
```

#### <a name="cause"></a>Nedeni

Bu hata aşağıdaki nedenlerden kaynaklanıyor olabilir:

1. Otomasyon hesabına geri iletişim engelleniyor.
2. Eklendi olan VM, Microsoft Monitoring Agent yüklü olarak Sysprep uygulanmamış bir kopyalanmış makineden gelmiş olabilir.

#### <a name="resolution"></a>Çözüm

1. Güncelleştirme Yönetimi çalışması için hangi adreslere ve bağlantı noktalarına izin verileceğini öğrenmek için [ağ planlaması](../automation-hybrid-runbook-worker.md#network-planning) ' nı ziyaret edin.
2. Kopyalanmış bir görüntü kullanıyorsanız:
   1. Log Analytics çalışma alanınızda, gösterilen kapsam yapılandırması `MicrosoftDefaultScopeConfig-Updates` için VM 'yi kayıtlı aramadan kaldırın. Kayıtlı aramalar, çalışma alanınızda **genel** altında bulunabilir.
   2.           `Remove-Item -Path "HKLM:\software\microsoft\hybridrunbookworker" -Recurse -Force`'i çalıştırın.
   3. Uygulamasını yeniden başlatmak için çalıştırın `Restart-Service HealthService` . `HealthService` Bu, anahtarı yeniden oluşturacak ve yeni bir UUID oluşturacak.
   4. Bu işe yaramazsa, önce görüntüyü Sysprep yapın ve bulduktan sonra MMA aracısını yükler.

### <a name="multi-tenant"></a>Senaryon Başka bir Azure kiracısındaki makineler için bir güncelleştirme dağıtımı oluştururken bağlantılı bir abonelik hatası alırsınız.

#### <a name="issue"></a>Sorun

Başka bir Azure kiracısındaki makineler için bir güncelleştirme dağıtımı oluşturmaya çalışırken şu hatayı alıyorsunuz:

```error
The client has permission to perform action 'Microsoft.Compute/virtualMachines/write' on scope '/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Automation/automationAccounts/automationAccountName/softwareUpdateConfigurations/updateDeploymentName', however the current tenant '00000000-0000-0000-0000-000000000000' is not authorized to access linked subscription '00000000-0000-0000-0000-000000000000'.
```

#### <a name="cause"></a>Nedeni

Bu hata, bir güncelleştirme dağıtımına dahil edilen başka bir kiracıda Azure sanal makineleri olan bir güncelleştirme dağıtımı oluşturduğunuzda oluşur.

#### <a name="resolution"></a>Çözüm

Zamanlanmış olmaları için aşağıdaki geçici çözümü kullanmanız gerekir. [Yeni-azurermautomationschedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) cmdlet 'ini bir zamanlama oluşturmak için anahtarıyla `-ForUpdate` kullanabilir [, New-azurermautomationsoftwareupdateconfiguration](/powershell/module/azurerm.automation/new-azurermautomationsoftwareupdateconfiguration
) cmdlet 'ini kullanabilir ve makineleri diğerine geçirebilirsiniz `-NonAzureComputer` parametreye kiracı. Aşağıdaki örnek bunun nasıl yapılacağı hakkında bir örnek göstermektedir:

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$s = New-AzureRmAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdate

New-AzureRmAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName $aa -Schedule $s -Windows -AzureVMResourceId $azureVMIdsW -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

### <a name="nologs"></a>Senaryon Makineler Güncelleştirme Yönetimi altında portalda gösterilmez

#### <a name="issue"></a>Sorun

Aşağıdaki senaryolarda çalıştırabilirsiniz:

* Makineniz, bir VM 'nin Güncelleştirme Yönetimi görünümünden **yapılandırılmadığını** gösteriyor

* Otomasyon hesabınızın Güncelleştirme Yönetimi görünümünde makineleriniz eksik

* **Uyumluluk**altında **değerlendirilmeyen** olarak görünen makineleriniz var, ancak karma Runbook Worker için Azure izleyici günlüklerinde sinyal verileri görüyorsunuz, ancak güncelleştirme yönetimi.

#### <a name="cause"></a>Nedeni

Bunun nedeni, olası yerel yapılandırma sorunları veya yanlış yapılandırılmış kapsam yapılandırması olabilir.

Karma Runbook Worker 'ın yeniden kaydettirilmesi ve yeniden yüklenmesi gerekebilir.

Çalışma alanınızda erişilen ve verilerin depolanmasından durdurulan bir kota tanımlamış olabilirsiniz.

#### <a name="resolution"></a>Çözüm

* Makinenizin doğru çalışma alanına rapor aldığından emin olun. Makinenizin hangi çalışma alanına rapor ettiğini doğrulayın. Bunu doğrulama hakkında yönergeler için bkz. [Log Analytics için aracı bağlantısını doğrulayın](../../azure-monitor/platform/agent-windows.md#verify-agent-connectivity-to-log-analytics). Daha sonra, bunun Azure Otomasyonu hesabınıza bağlı olan çalışma alanı olduğundan emin olun. Bunu onaylamak için Otomasyon hesabınıza gidin ve **Ilgili kaynaklar**altında **bağlantılı çalışma alanı** ' na tıklayın.

* Makinelerin Log Analytics çalışma alanınızda gösterilmesini sağlamak için denetleyin. Aşağıdaki sorguyu Otomasyon hesabınıza bağlı Log Analytics çalışma alanınızda çalıştırın. Makinenizi sorgu sonuçlarında görmüyorsanız, makineniz sinyal gönderiliyor değildir, bu da büyük olasılıkla yerel bir yapılandırma sorunudur. İşletim sistemine bağlı olarak [Windows](update-agent-issues.md#troubleshoot-offline) veya [Linux](update-agent-issues-linux.md#troubleshoot-offline) için sorun gidericiyi çalıştırabilir veya [aracıyı yeniden yükleyebilirsiniz](../../azure-monitor/learn/quick-collect-windows-computer.md#install-the-agent-for-windows). Makineniz sorgu sonuçlarında görünüyorsa, aşağıdaki madde işaretinde belirtilen kapsam yapılandırması gerekir.

  ```loganalytics
  Heartbeat
  | summarize by Computer, Solutions
  ```

* Kapsam yapılandırma sorunlarını denetleyin. [Kapsam yapılandırması](../automation-onboard-solutions-from-automation-account.md#scope-configuration) , hangi makinelerin çözüm için yapılandırıldığını belirler. Makinenizin çalışma alanınızda gösterilmesi, ancak gösterilmiyorsa, makineleri hedeflemek için kapsam yapılandırmasını yapılandırmanız gerekecektir. Bunun nasıl yapılacağını öğrenmek için bkz. [çalışma alanındaki makineler](../automation-onboard-solutions-from-automation-account.md#onboard-machines-in-the-workspace)ekleme.

* Yukarıdaki adımlar sorununuzu çözmezse, Windows için karma çalışanı veya Linux için [bir Linux karma Runbook Worker 'ı dağıtmak](../automation-linux-hrw-install.md) Için [Windows hibrit Runbook Worker dağıtma](../automation-windows-hrw-install.md) bölümündeki adımları izleyin.

* Çalışma alanınızda aşağıdaki sorguyu çalıştırın. Sonuç `Data collection stopped due to daily limit of free data reached. Ingestion status = OverQuota` olarak, çalışma alanınızda erişilen ve verilerin kaydedilmesini durduran bir kotayı görürsünüz. Çalışma alanınızda, **kullanım ve tahmini maliyet** > **veri hacmi yönetimi** ' ne gidin ve kotayı denetleyin ya da sahip olduğunuz kotayı kaldırın.

  ```loganalytics
  Operation
  | where OperationCategory == 'Data Collection Status'
  | sort by TimeGenerated desc
  ```

## <a name="windows"></a>Windows

Çözümü bir sanal makineye eklenmeye çalışırken sorunlarla karşılaşırsanız, olay KIMLIĞI **4502** ve olay iletisi olan olaylar için yerel makinedeki **uygulama ve hizmet günlükleri** altında olay günlüğünü **Operations Manager** denetleyin **Microsoft. EnterpriseManagement. HealthService. AzureAutomation. HybridAgent**' ı içerir.

Aşağıdaki bölümde, belirli hata iletileri ve her biri için olası bir çözüm vurgulanmıştır. Diğer ekleme sorunları için bkz. [çözüm ekleme sorunlarını giderme](onboarding.md).

### <a name="machine-already-registered"></a>Senaryon Makine farklı bir hesaba zaten kayıtlı

#### <a name="issue"></a>Sorun

Aşağıdaki hata iletisini alırsınız:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.InvalidOperationException: {"Message":"Machine is already registered to a different account."}
```

#### <a name="cause"></a>Nedeni

Makine, Güncelleştirme Yönetimi için zaten başka bir çalışma alanına eklendi.

#### <a name="resolution"></a>Çözüm

[Karma runbook grubunu silerek](../automation-hybrid-runbook-worker.md#remove-a-hybrid-worker-group) makinede eski yapıtları temizleme işlemini gerçekleştirin ve yeniden deneyin.

### <a name="machine-unable-to-communicate"></a>Senaryon Makine hizmetle iletişim kuramıyor

#### <a name="issue"></a>Sorun

Aşağıdaki hata iletilerinden birini alırsınız:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.Net.Http.HttpRequestException: An error occurred while sending the request. ---> System.Net.WebException: The underlying connection was closed: An unexpected error occurred on a receive. ---> System.ComponentModel.Win32Exception: The client and server can't communicate, because they do not possess a common algorithm
```

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception Newtonsoft.Json.JsonReaderException: Error parsing positive infinity value.
```

```error
The certificate presented by the service <wsid>.oms.opinsights.azure.com was not issued by a certificate authority used for Microsoft services. Contact your network administrator to see if they are running a proxy that intercepts TLS/SSL communication.
```

#### <a name="cause"></a>Nedeni

Ağ iletişimini engelleyen bir ara sunucu, ağ geçidi veya güvenlik duvarı olabilir.

#### <a name="resolution"></a>Çözüm

Ağınızı gözden geçirin ve uygun bağlantı noktalarına ve adreslere izin verildiğinden emin olun. Güncelleştirme Yönetimi ve karma runbook çalışanları için gereken bağlantı noktalarının ve adreslerin listesi için bkz. [ağ gereksinimleri](../automation-hybrid-runbook-worker.md#network-planning).

### <a name="unable-to-create-selfsigned-cert"></a>Senaryon Otomatik olarak imzalanan sertifika oluşturulamıyor

#### <a name="issue"></a>Sorun

Aşağıdaki hata iletilerinden birini alırsınız:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception AgentService.HybridRegistration. PowerShell.Certificates.CertificateCreationException: Failed to create a self-signed certificate. ---> System.UnauthorizedAccessException: Access is denied.
```

#### <a name="cause"></a>Nedeni

Karma runbook çalışanı kendinden imzalı bir sertifika oluşturamadı

#### <a name="resolution"></a>Çözüm

Sistem hesabının **C:\programdata\microsoft\crypto\rsa** klasörüne okuma erişimi olduğunu doğrulayın ve yeniden deneyin.

### <a name="failed-to-start"></a>Senaryon Bir makine, güncelleştirme dağıtımında başlatılamadı

#### <a name="issue"></a>Sorun

Makinenin durumu bir makine için başlatılamadı. Makinenin belirli ayrıntılarını görüntülediğinizde, aşağıdaki hatayı görürsünüz:

```error
Failed to start the runbook. Check the parameters passed. RunbookName Patch-MicrosoftOMSComputer. Exception You have requested to create a runbook job on a hybrid worker group that does not exist.
```

#### <a name="cause"></a>Nedeni

Bu hatanın nedeni aşağıdakilerden biri olabilir:

* Makine artık yok.
* Makine kapalı ve ulaşılamaz durumda.
* Makinede bir ağ bağlantısı sorunu ve makinedeki karma çalışan ulaşılamaz durumda.
* Microsoft Monitoring Agent Sourcecomputerıd 'sini değiştiren bir güncelleştirme vardı
* Bir Otomasyon hesabında 2.000 eşzamanlı iş sınırına ulaşırsanız güncelleştirme çalışması kısıtlanmış olabilir. Her dağıtım, iş olarak bir güncelleştirme dağıtımı sayısında iş ve her makine olarak değerlendirilir. Otomasyon hesabınızda çalışmakta olan diğer herhangi bir Otomasyon işi veya güncelleştirme dağıtımı, eşzamanlı iş sınırına doğru sayılır.

#### <a name="resolution"></a>Çözüm

Uygun olduğunda, güncelleştirme dağıtımlarınız için [dinamik grupları](../automation-update-management.md#using-dynamic-groups) kullanın.

* Makinenin hala var olduğunu ve erişilebilir olduğunu doğrulayın. Mevcut değilse, dağıtımınızı düzenleyin ve makineyi kaldırın.
* Güncelleştirme Yönetimi için gereken bağlantı noktaları ve adreslerin listesi için [ağ planlaması](../automation-update-management.md#ports) bölümüne bakın ve makinenizin bu gereksinimleri karşıladığını doğrulayın.
* Ortamınızdaki `SourceComputerId` makineleri bulmak için Log Analytics aşağıdaki sorguyu çalıştırın. Aynı `Computer` değere sahip olan, ancak farklı `SourceComputerId` bir değere sahip bilgisayarları arayın. Etkilenen makineleri bulduktan sonra, bu makineleri hedefleyen güncelleştirme dağıtımlarını düzenlemeniz ve makineleri `SourceComputerId` kaldırıp yeniden ekleyerek doğru değeri yansıtması gerekir.

   ```loganalytics
   Heartbeat | where TimeGenerated > ago(30d) | distinct SourceComputerId, Computer, ComputerIP
   ```

### <a name="hresult"></a>Senaryon Makine değerlendirilmez olarak gösterilir ve bir HResult özel durumu gösterir

#### <a name="issue"></a>Sorun

**Uyumluluk**altında **değerlendirilmeyen** olarak görünen makineleriniz vardır ve bunun altında bir özel durum iletisi görürsünüz.

#### <a name="cause"></a>Nedeni

Windows Update veya WSUS makinede doğru şekilde yapılandırılmadı. Güncelleştirme Yönetimi, gereken güncelleştirmeleri, düzeltme ekinin durumunu ve dağıtılan düzeltme eklerinin sonuçlarını sağlamak için Windows Update veya WSUS 'tan yararlanır. Bu bilgiler olmadan Güncelleştirme Yönetimi, gereken veya yüklenen düzeltme eklerine düzgün şekilde bildiremeyebilir.

#### <a name="resolution"></a>Çözüm

Tüm özel durum iletisini görmek için kırmızı renkte görüntülenecek özel duruma çift tıklayın. Gerçekleştirilecek olası çözümler veya eylemler için aşağıdaki tabloyu gözden geçirin:

|Özel durum  |Çözüm veya eylem  |
|---------|---------|
|`Exception from HRESULT: 0x……C`     | Özel durumun nedeni hakkında daha fazla bilgi edinmek için [Windows Update hata kodu listesinde](https://support.microsoft.com/help/938205/windows-update-error-code-list) ilgili hata kodunu arayın.        |
|`0x8024402C`</br>`0x8024401C`</br>`0x8024402F`      | Bu hatalar ağ bağlantısı sorunlarıdır. Makinenizin Güncelleştirme Yönetimi için uygun ağ bağlantısına sahip olduğundan emin olun. Gerekli olan bağlantı noktalarının ve adreslerin listesi için [ağ planlaması](../automation-update-management.md#ports) bölümüne bakın.        |
|`0x8024001E`| Hizmet veya sistem kapatıldığından güncelleştirme işlemi tamamlanamadı.|
|`0x8024002E`| Windows Update hizmeti devre dışı bırakıldı.|
|`0x8024402C`     | Bir WSUS sunucusu kullanıyorsanız, kayıt defteri anahtarı `WUServer` `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate` için `WUStatusServer` kayıt defteri değerlerinin doğru WSUS sunucusuna sahip olduğundan emin olun.        |
|`The service cannot be started, either because it is disabled or because it has no enabled devices associated with it. (Exception from HRESULT: 0x80070422)`     | Windows Update hizmeti 'nin (wuauserv) çalıştığından ve devre dışı bırakılmadığından emin olun.        |
|Diğer tüm genel durumlar     | Internet 'te olası çözümler için arama yapın ve yerel BT desteğiniz ile çalışın.         |

' İ gözden geçirmek olası nedeni de belirlemenize yardımcı olabilir.`windowsupdate.log` Günlüğü okuma hakkında daha fazla bilgi için bkz. [windowsupdate. log dosyasını okuma](https://support.microsoft.com/en-ca/help/902093/how-to-read-the-windowsupdate-log-file).

Ayrıca, makinede Windows Update herhangi bir sorun olup olmadığını denetlemek için [Windows Update sorun gidericiyi](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) indirebilir ve çalıştırabilirsiniz.

> [!NOTE]
> [Windows Update sorun giderici](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) Windows istemcilerine yöneliktir, ancak Windows Server 'da da çalışmaktadır.

## <a name="linux"></a>Linux

### <a name="scenario-update-run-fails-to-start"></a>Senaryo: Güncelleştirme çalıştırması başlatılamadı

#### <a name="issue"></a>Sorun

Bir güncelleştirme çalışması bir Linux makinesinde başlatılamaz.

#### <a name="cause"></a>Nedeni

Linux hibrit çalışanı sağlam değil.

#### <a name="resolution"></a>Çözüm

Aşağıdaki günlük dosyasının bir kopyasını oluşturun ve sorun giderme amacıyla koruyun:

```bash
/var/opt/microsoft/omsagent/run/automationworker/worker.log
```

### <a name="scenario-update-run-starts-but-encounters-errors"></a>Senaryo: Güncelleştirme çalıştırması başlıyor, ancak hatalarla karşılaşırsa

#### <a name="issue"></a>Sorun

Bir güncelleştirme çalıştırması başlatılır, ancak çalıştırma sırasında hatalarla karşılaşır.

#### <a name="cause"></a>Nedeni

Olası nedenler:

* Paket Yöneticisi uygun değil
* Belirli paketler, bulut tabanlı düzeltme eki uygulamayı etkileyebilir
* Diğer nedenler

#### <a name="resolution"></a>Çözüm

Linux 'ta başarılı bir şekilde başladıktan sonra bir güncelleştirme çalıştırması sırasında oluşan başarısızlıklar varsa, çalıştırmada etkilenen makinenin iş çıktısını kontrol edin. Çözebileceğinizi ve üzerinde işlem yapmanız gereken makinenizin paket yöneticisinden belirli hata mesajlarını bulabilirsiniz. Güncelleştirme Yönetimi, paket yöneticisinin başarılı güncelleştirme dağıtımları için sağlıklı olmasını gerektirir.

Bazı durumlarda, paket güncelleştirmeleri bir güncelleştirme dağıtımının tamamlanmasını Güncelleştirme Yönetimi engel olabilir. Bunu görürseniz, bu paketleri gelecekteki güncelleştirme çalıştırmalarından hariç tutmaları veya el ile kendiniz kurmanız gerekecektir.

Düzeltme eki uygulama sorununu çözemezseniz, aşağıdaki günlük dosyasının bir kopyasını oluşturun ve sorun giderme amacıyla bir sonraki güncelleştirme dağıtımı **başlamadan önce** bu dosyayı koruyun:

```bash
/var/opt/microsoft/omsagent/run/automationworker/omsupdatemgmt.log
```

### <a name="other"></a>Senaryon Sorun yukarıda listelenmiyor

### <a name="issue"></a>Sorun

Listelenen diğer senaryolar tarafından çözümlenemeyen bir sorununuz var.

### <a name="cause"></a>Nedeni

Yanlış yapılandırılmış veya eksik kayıt defteri anahtarları Güncelleştirme Yönetimi soruna neden olabilir.

### <a name="resolution"></a>Çözüm

Kayıt defteri anahtarını `HKLM:\SOFTWARE\Microsoft\HybridRunbookWorker` silin ve **HealthService**'i yeniden başlatın.

Aşağıdaki PowerShell komutlarını da kullanabilirsiniz.

```powershell
Remove-Item -Path "HKLM:\software\microsoft\hybridrunbookworker" -Recurse -Force
Restart-Service healthservice
```

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayı dosyası gönderebilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**' ı seçin.
