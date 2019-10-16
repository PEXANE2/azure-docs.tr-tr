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
ms.openlocfilehash: 89c5c849ffdbe70ae449f41ac08471c84f2fb5bc
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72372638"
---
# <a name="troubleshooting-issues-with-update-management"></a>Güncelleştirme Yönetimi sorunlarını giderme

Bu makalede, Güncelleştirme Yönetimi kullanırken üzerinde çalıştırabileceğiniz sorunları gidermeye yönelik çözümler ele alınmaktadır.

Temeldeki sorunu belirlemede karma çalışan Aracısı için bir aracı sorun giderici vardır. Sorun giderici hakkında daha fazla bilgi için bkz. [Güncelleştirme Aracısı sorunlarını giderme](update-agent-issues.md). Diğer tüm sorunlar için, olası sorunlar hakkında aşağıdaki ayrıntılı bilgilere bakın.

Çözümü bir sanal makineye eklenmeye çalışırken sorunlarla karşılaşırsanız, olay KIMLIĞI **4502** ve olay iletisi olan olaylar için yerel makinedeki **uygulama ve hizmet günlükleri** altında olay günlüğünü **Operations Manager** denetleyin **Microsoft. EnterpriseManagement. HealthService. AzureAutomation. HybridAgent**' ı içerir.

Aşağıdaki bölümde, belirli hata iletileri ve her biri için olası bir çözüm vurgulanmıştır. Diğer ekleme sorunları için bkz. [çözüm ekleme sorunlarını giderme](onboarding.md).

## <a name="nologs"></a>Senaryo: makineler altında portalda gösterilmez Güncelleştirme Yönetimi

### <a name="issue"></a>Sorun

Aşağıdaki senaryolarda çalıştırabilirsiniz:

* Makineniz, bir VM 'nin Güncelleştirme Yönetimi görünümünden **yapılandırılmadığını** gösteriyor

* Otomasyon hesabınızın Güncelleştirme Yönetimi görünümünde makineleriniz eksik

* **Uyumluluk**altında **değerlendirilmeyen** olarak görünen makineleriniz var, ancak karma Runbook Worker için Azure izleyici günlüklerinde sinyal verileri görüyorsunuz, ancak güncelleştirme yönetimi.

### <a name="cause"></a>Nedeni

Bunun nedeni, olası yerel yapılandırma sorunları veya yanlış yapılandırılmış kapsam yapılandırması olabilir.

Karma Runbook Worker 'ın yeniden kaydettirilmesi ve yeniden yüklenmesi gerekebilir.

Çalışma alanınızda erişilen ve verilerin depolanmasından durdurulan bir kota tanımlamış olabilirsiniz.

### <a name="resolution"></a>Çözünürlük

* İşletim sistemine bağlı olarak [Windows](update-agent-issues.md#troubleshoot-offline) veya [Linux](update-agent-issues-linux.md#troubleshoot-offline) için sorun gidericiyi çalıştırın.

* Makinenizin doğru çalışma alanına rapor aldığından emin olun. Makinenizin hangi çalışma alanına rapor ettiğini doğrulayın. Bunu doğrulama hakkında yönergeler için bkz. [Log Analytics için aracı bağlantısını doğrulayın](../../azure-monitor/platform/agent-windows.md#verify-agent-connectivity-to-log-analytics). Daha sonra, bunun Azure Otomasyonu hesabınıza bağlı olan çalışma alanı olduğundan emin olun. Bunu onaylamak için Otomasyon hesabınıza gidin ve **Ilgili kaynaklar**altında **bağlantılı çalışma alanı** ' na tıklayın.

* Makinelerin Log Analytics çalışma alanınızda gösterilmesini sağlamak için denetleyin. Aşağıdaki sorguyu Otomasyon hesabınıza bağlı Log Analytics çalışma alanınızda çalıştırın. Makinenizi sorgu sonuçlarında görmüyorsanız, makineniz son zamanlarda iade edilmez, bu da olası bir yerel yapılandırma sorunu olduğu anlamına gelir ve [aracıyı yeniden yükleyebilirsiniz](../../azure-monitor/learn/quick-collect-windows-computer.md#install-the-agent-for-windows). Makineniz sorgu sonuçlarında görünüyorsa, aşağıdaki madde işaretinde belirtilen kapsam yapılandırması gerekir.

  ```loganalytics
  Heartbeat
  | summarize by Computer, Solutions
  ```

* Kapsam yapılandırma sorunlarını denetleyin. [Kapsam yapılandırması](../automation-onboard-solutions-from-automation-account.md#scope-configuration) , hangi makinelerin çözüm için yapılandırıldığını belirler. Makinenizin çalışma alanınızda gösterilmesi, ancak **güncelleştirme yönetimi** portalında gösterilmiyorsa, kapsamları hedeflemek için kapsam yapılandırmasını yapılandırmanız gerekecektir. Bunun nasıl yapılacağını öğrenmek için bkz. [çalışma alanındaki makineler](../automation-onboard-solutions-from-automation-account.md#onboard-machines-in-the-workspace)ekleme.

* Çalışma alanınızda aşağıdaki sorguyu çalıştırın. Sonucu görürseniz `Data collection stopped due to daily limit of free data reached. Ingestion status = OverQuota`, çalışma alanınızda erişilen ve verilerin kaydedilmesini durduran bir kotasına sahip olursunuz. Çalışma alanınızda, **kullanım ve tahmini maliyetler** > **veri hacmi yönetimi** ' ne gidin ve kotayı denetleyin ya da sahip olduğunuz kotayı kaldırın.

  ```loganalytics
  Operation
  | where OperationCategory == 'Data Collection Status'
  | sort by TimeGenerated desc
  ```

* Yukarıdaki adımlar sorununuzu çözmezse, Windows için karma çalışanı veya Linux için [bir Linux karma Runbook Worker 'ı dağıtmak](../automation-linux-hrw-install.md) Için [Windows hibrit Runbook Worker dağıtma](../automation-windows-hrw-install.md) bölümündeki adımları izleyin.

## <a name="rp-register"></a>Senaryo: abonelikler için Otomasyon kaynak sağlayıcısı kaydedilemiyor

### <a name="issue"></a>Sorun

Otomasyon hesabınızdaki çözümlerle çalışırken aşağıdaki hatayı alabilirsiniz.

```error
Error details: Unable to register Automation Resource Provider for subscriptions:
```

### <a name="cause"></a>Nedeni

Otomasyon kaynak sağlayıcısı abonelikte kayıtlı değil.

### <a name="resolution"></a>Çözünürlük

Azure portal aşağıdaki adımları tamamlayarak Otomasyon kaynak sağlayıcılarını kaydedebilirsiniz:

1. Alt Azure hizmeti listesinde **tüm hizmetler** ' e tıklayın ve ardından _genel_ hizmet grubundaki **abonelikler** ' i seçin.
2. Aboneliğinizi seçin.
3. _Ayarlar_altında **kaynak sağlayıcıları** ' na tıklayın.
4. Kaynak sağlayıcıları listesinden **Microsoft. Automation** kaynak sağlayıcısının kayıtlı olduğundan emin olun.
5. Sağlayıcı listelenmiyorsa, **Microsoft. Automation** sağlayıcısını altında [ ](/azure/azure-resource-manager/resource-manager-register-provider-errors)listelenen adımlarla kaydedin.

## <a name="components-enabled-not-working"></a>Senaryo: ' Güncelleştirme Yönetimi ' çözümünün bileşenleri etkinleştirildi ve şimdi bu sanal makine yapılandırılıyor

### <a name="issue"></a>Sorun

Daha sonra bir sanal makinede 15 dakika sonra şu iletiyi görmeye devam edersiniz:

```error
The components for the 'Update Management' solution have been enabled, and now this virtual machine is being configured. Please be patient, as this can sometimes take up to 15 minutes.
```

### <a name="cause"></a>Nedeni

Bu hata aşağıdaki nedenlerden kaynaklanıyor olabilir:

1. Otomasyon hesabına geri iletişim engelleniyor.
2. Eklendi olan VM, Microsoft Monitoring Agent yüklü olarak Sysprep uygulanmamış bir kopyalanmış makineden gelmiş olabilir.

### <a name="resolution"></a>Çözünürlük

1. Güncelleştirme Yönetimi çalışması için hangi adreslere ve bağlantı noktalarına izin verileceğini öğrenmek için [ağ planlaması](../automation-hybrid-runbook-worker.md#network-planning) ' nı ziyaret edin.
2. Kopyalanmış bir görüntü kullanıyorsanız:
   1. Log Analytics çalışma alanınızda, kapsam yapılandırması için kaydedilen aramadan VM 'yi kaldırın `MicrosoftDefaultScopeConfig-Updates` gösterilir. Kayıtlı aramalar, çalışma alanınızda **genel** altında bulunabilir.
   2. `Remove-Item -Path "HKLM:\software\microsoft\hybridrunbookworker" -Recurse -Force` öğesini çalıştırın
   3. @No__t-1 ' i yeniden başlatmak için `Restart-Service HealthService` çalıştırın. Bu, anahtarı yeniden oluşturacak ve yeni bir UUID oluşturacak.
   4. Bu işe yaramazsa, önce görüntüyü Sysprep yapın ve bulduktan sonra MMA aracısını yükler.

## <a name="multi-tenant"></a>Senaryo: başka bir Azure kiracısındaki makineler için bir güncelleştirme dağıtımı oluştururken bağlantılı bir abonelik hatası alırsınız.

### <a name="issue"></a>Sorun

Başka bir Azure kiracısındaki makineler için bir güncelleştirme dağıtımı oluşturmaya çalışırken şu hatayı alıyorsunuz:

```error
The client has permission to perform action 'Microsoft.Compute/virtualMachines/write' on scope '/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Automation/automationAccounts/automationAccountName/softwareUpdateConfigurations/updateDeploymentName', however the current tenant '00000000-0000-0000-0000-000000000000' is not authorized to access linked subscription '00000000-0000-0000-0000-000000000000'.
```

### <a name="cause"></a>Nedeni

Bu hata, bir güncelleştirme dağıtımına dahil edilen başka bir kiracıda Azure sanal makineleri olan bir güncelleştirme dağıtımı oluşturduğunuzda oluşur.

### <a name="resolution"></a>Çözünürlük

Zamanlanmış olmaları için aşağıdaki geçici çözümü kullanmanız gerekir. Bir zamanlama oluşturmak için [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) cmdlet 'ini `-ForUpdate` anahtarıyla kullanabilir ve [New-AzureRmAutomationSoftwareUpdateConfiguration](/powershell/module/azurerm.automation/new-azurermautomationsoftwareupdateconfiguration
) cmdlet 'ini kullanarak diğer Kiracıdaki makineleri `-NonAzureComputer` parametresine geçirebilirsiniz. Aşağıdaki örnek bunun nasıl yapılacağı hakkında bir örnek göstermektedir:

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$s = New-AzureRmAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdate

New-AzureRmAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName $aa -Schedule $s -Windows -AzureVMResourceId $azureVMIdsW -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

## <a name="node-reboots"></a>Senaryo: açıklanamayan yeniden başlatmalar

### <a name="issue"></a>Sorun

Yeniden **başlatma**Işlemi için **yeniden başlatma denetimi** yapılandırdınız, ancak makineler yüklü güncelleştirmelerden sonra hala yeniden başlatılıyor.

### <a name="cause"></a>Nedeni

Windows Update davranış birkaç kayıt defteri anahtarı tarafından değiştirilebilir ve bu, yeniden başlatma davranışlarını değiştirebilir.

### <a name="resolution"></a>Çözünürlük

Makinelerinizin düzgün yapılandırıldığından emin olmak için [yeniden başlatma işlemini yönetmek üzere kullanılan](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) kayıt defteri ve kayıt defteri anahtarlarını [düzenleyerek otomatik güncelleştirmeleri yapılandırma](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-rej7uijui7jgistry) altında listelenen kayıt defteri anahtarlarını gözden geçirin.

## <a name="failed-to-start"></a>Senaryo: bir makine güncelleştirme dağıtımında başlatılamadı

### <a name="issue"></a>Sorun

Makinenin durumu bir makine için **başlatılamadı.** Makinenin belirli ayrıntılarını görüntülediğinizde, aşağıdaki hatayı görürsünüz:

```error
Failed to start the runbook. Check the parameters passed. RunbookName Patch-MicrosoftOMSComputer. Exception You have requested to create a runbook job on a hybrid worker group that does not exist.
```

### <a name="cause"></a>Nedeni

Bu hatanın nedeni aşağıdakilerden biri olabilir:

* Makine artık yok.
* Makine kapalı ve ulaşılamaz durumda.
* Makinede bir ağ bağlantısı sorunu ve makinedeki karma çalışan ulaşılamaz durumda.
* Microsoft Monitoring Agent Sourcecomputerıd 'sini değiştiren bir güncelleştirme vardı
* Bir Otomasyon hesabında 2.000 eşzamanlı iş sınırına ulaşırsanız güncelleştirme çalışması kısıtlanmış olabilir. Her dağıtım, iş olarak bir güncelleştirme dağıtımı sayısında iş ve her makine olarak değerlendirilir. Otomasyon hesabınızda çalışmakta olan diğer herhangi bir Otomasyon işi veya güncelleştirme dağıtımı, eşzamanlı iş sınırına doğru sayılır.

### <a name="resolution"></a>Çözünürlük

Uygun olduğunda, güncelleştirme dağıtımlarınız için [dinamik grupları](../automation-update-management-groups.md) kullanın.

* Makinenin hala var olduğunu ve erişilebilir olduğunu doğrulayın. Mevcut değilse, dağıtımınızı düzenleyin ve makineyi kaldırın.
* Güncelleştirme Yönetimi için gereken bağlantı noktaları ve adreslerin listesi için [ağ planlaması](../automation-update-management.md#ports) bölümüne bakın ve makinenizin bu gereksinimleri karşıladığını doğrulayın.
* Ortamınızdaki `SourceComputerId` ' ın değiştiği makineleri bulmak için Log Analytics aşağıdaki sorguyu çalıştırın. Aynı `Computer` değerine sahip bilgisayarları, ancak farklı `SourceComputerId` değerini arayın. Etkilenen makineleri bulduktan sonra, bu makineleri hedefleyen güncelleştirme dağıtımlarını düzenlemeniz ve `SourceComputerId` ' ın doğru değeri yansıtması için makineleri kaldırıp yeniden eklemeniz gerekir.

   ```loganalytics
   Heartbeat | where TimeGenerated > ago(30d) | distinct SourceComputerId, Computer, ComputerIP
   ```

## <a name="updates-nodeployment"></a>Senaryo: güncelleştirmeler dağıtım olmadan yüklenir

### <a name="issue"></a>Sorun

Bir Windows makinesini Güncelleştirme Yönetimi kaydettiğinizde, güncelleştirme yüklemesini bir dağıtım olmadan görebilirsiniz.

### <a name="cause"></a>Nedeni

Windows 'da, güncelleştirmeler kullanılabilir duruma geldiğinde otomatik olarak yüklenir. Bu, makineye dağıtılacak bir güncelleştirme zamanlamadığı takdirde karışıklık oluşmasına neden olabilir.

### <a name="resolution"></a>Çözünürlük

Windows kayıt defteri anahtarı, `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU` varsayılan olarak "4", **otomatik indir ve yükle**olur.

Güncelleştirme Yönetimi istemcileri için, bu anahtarı "3" olarak ayarlamayı öneririz- **otomatik indir, ancak otomatik olarak yükleme**.

Daha fazla bilgi için bkz. [otomatik güncelleştirmeleri yapılandırma](https://docs.microsoft.com/windows/deployment/update/waas-wu-settings#configure-automatic-updates).

## <a name="machine-already-registered"></a>Senaryo: makine farklı bir hesaba zaten kayıtlı

### <a name="issue"></a>Sorun

Aşağıdaki hata iletisini alırsınız:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.InvalidOperationException: {"Message":"Machine is already registered to a different account."}
```

### <a name="cause"></a>Nedeni

Makine, Güncelleştirme Yönetimi için zaten başka bir çalışma alanına eklendi.

### <a name="resolution"></a>Çözünürlük

1. Makinenin doğru çalışma alanına rapor aldığından emin olmak için, makineler bölümündeki adımları izleyin [güncelleştirme yönetimi altında portalda görünmez](#nologs) .
2. [Karma runbook grubunu silerek](../automation-hybrid-runbook-worker.md#remove-a-hybrid-worker-group) makinede eski yapıtları temizleme işlemini gerçekleştirin ve yeniden deneyin.

## <a name="machine-unable-to-communicate"></a>Senaryo: makine hizmetle iletişim kuramıyor

### <a name="issue"></a>Sorun

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

### <a name="cause"></a>Nedeni

Ağ iletişimini engelleyen bir ara sunucu, ağ geçidi veya güvenlik duvarı olabilir.

### <a name="resolution"></a>Çözünürlük

Ağınızı gözden geçirin ve uygun bağlantı noktalarına ve adreslere izin verildiğinden emin olun. Güncelleştirme Yönetimi ve karma runbook çalışanları için gereken bağlantı noktalarının ve adreslerin listesi için bkz. [ağ gereksinimleri](../automation-hybrid-runbook-worker.md#network-planning).

## <a name="unable-to-create-selfsigned-cert"></a>Senaryo: otomatik olarak imzalanan sertifika oluşturulamıyor

### <a name="issue"></a>Sorun

Aşağıdaki hata iletilerinden birini alırsınız:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception AgentService.HybridRegistration. PowerShell.Certificates.CertificateCreationException: Failed to create a self-signed certificate. ---> System.UnauthorizedAccessException: Access is denied.
```

### <a name="cause"></a>Nedeni

Karma runbook çalışanı kendinden imzalı bir sertifika oluşturamadı

### <a name="resolution"></a>Çözünürlük

Sistem hesabının **C:\programdata\microsoft\crypto\rsa** klasörüne okuma erişimi olduğunu doğrulayın ve yeniden deneyin.

## <a name="mw-exceeded"></a>Senaryo: güncelleştirme yönetimi zamanlandı, MaintenanceWindowExceeded hatasıyla başarısız oldu

### <a name="issue"></a>Sorun

Güncelleştirmeler için varsayılan bakım penceresi 120 dakikadır. Bakım penceresini en fazla altı (6) saat veya 360 dakika olarak artırabilirsiniz.

### <a name="resolution"></a>Çözünürlük

Başarısız olan zamanlanmış güncelleştirme dağıtımlarını düzenleyin ve bakım penceresini arttırın.

Bakım pencereleri hakkında daha fazla bilgi için bkz. [Install Updates](../automation-tutorial-update-management.md#schedule-an-update-deployment).

## <a name="hresult"></a>Senaryo: makine değerlendirilmez olarak gösterilir ve bir HResult özel durumu gösterir

### <a name="issue"></a>Sorun

* **Uyumluluk**altında **değerlendirilmeyen** olarak görünen makineleriniz vardır ve bunun altında bir özel durum iletisi görürsünüz.
* Değerlendirilmeyen olarak gösteren makineleriniz var
* Portalda bir HRESULT hata kodu görürsünüz.

### <a name="cause"></a>Nedeni

Güncelleştirme Aracısı (Linux dağıtım için paket yöneticisi olan Windows 'daki Windows Update Aracısı) doğru yapılandırılmamış. Güncelleştirme Yönetimi, gereken güncelleştirmeleri, düzeltme ekinin durumunu ve dağıtılan düzeltme eklerinin sonuçlarını sağlamak için makinenin güncelleştirme aracısına bağımlıdır. Bu bilgiler olmadan Güncelleştirme Yönetimi, gereken veya yüklenen düzeltme eklerine düzgün şekilde bildiremeyebilir.

### <a name="resolution"></a>Çözünürlük

Güncelleştirmeler makinede yerel olarak gerçekleştirilmeye çalışılıyor. Bu başarısız olursa, bu genellikle güncelleştirme aracısında bir yapılandırma hatası anlamına gelir.

Hatanın yaygın nedenleri şunlardır:

* Ağ yapılandırması ve güvenlik duvarları.
* Linux için, paket deponuzdaki ağ uç noktasına ulaşabildiğinizden emin olmak için uygun belgeleri denetleyin.
* Windows için, [güncelleştirmeler intranet uç noktasından (WSUS/SCCM) indirmeyen güncelleştirmeler](/windows/deployment/update/windows-update-troubleshooting#updates-arent-downloading-from-the-intranet-endpoint-wsussccm)bölümünde listelenen aracı yapılandırmanızı denetleyin.
  * Makine (ler) Windows Update için yapılandırılmışsa, altında [ ](/windows/deployment/update/windows-update-troubleshooting#issues-related-to-httpproxy)listelenen uç noktalara ulaşabildiğinizden emin olun.
  * Makine (ler) WSUS için yapılandırılmışsa, [WUServer kayıt defteri anahtarı](/windows/deployment/update/waas-wu-settings)tarafından yapılandırılan WSUS sunucusuna erişebildiğinizden emin olun.

Bir HRESULT görürseniz, tüm özel durum iletisini görmek için kırmızı renkte görüntülenecek özel duruma çift tıklayın. Gerçekleştirilecek olası çözümler veya eylemler için aşağıdaki tabloyu gözden geçirin:

|Özel durum  |Çözüm veya eylem  |
|---------|---------|
|`Exception from HRESULT: 0x……C`     | Özel durumun nedeni hakkında daha fazla bilgi edinmek için [Windows Update hata kodu listesinde](https://support.microsoft.com/help/938205/windows-update-error-code-list) ilgili hata kodunu arayın.        |
|`0x8024402C`</br>`0x8024401C`</br>`0x8024402F`      | Bu hatalar ağ bağlantısı sorunlarıdır. Makinenizin Güncelleştirme Yönetimi için uygun ağ bağlantısına sahip olduğundan emin olun. Gerekli olan bağlantı noktalarının ve adreslerin listesi için [ağ planlaması](../automation-update-management.md#ports) bölümüne bakın.        |
|`0x8024001E`| Hizmet veya sistem kapatıldığından güncelleştirme işlemi tamamlanamadı.|
|`0x8024002E`| Windows Update hizmeti devre dışı bırakıldı.|
|`0x8024402C`     | Bir WSUS sunucusu kullanıyorsanız, `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate` kayıt defteri anahtarı altındaki `WUServer` ve `WUStatusServer` için kayıt defteri değerlerinin doğru WSUS sunucusuna sahip olduğundan emin olun.        |
|`0x80072EE2`|Ağ bağlantısı sorunu veya yapılandırılmış bir WSUS sunucusuyla iletişim sorunu. WSUS ayarlarını denetleyip istemciden erişilebilir olduğundan emin olun.|
|`The service cannot be started, either because it is disabled or because it has no enabled devices associated with it. (Exception from HRESULT: 0x80070422)`     | Windows Update hizmeti 'nin (wuauserv) çalıştığından ve devre dışı bırakılmadığından emin olun.        |
|Diğer tüm genel durumlar     | Internet 'te olası çözümler için arama yapın ve yerel BT desteğiniz ile çalışın.         |

@No__t-0 ' yı gözden geçirmek olası nedeni de belirlemenize yardımcı olabilir. Günlüğü okuma hakkında daha fazla bilgi için bkz. [windowsupdate. log dosyasını okuma](https://support.microsoft.com/en-ca/help/902093/how-to-read-the-windowsupdate-log-file).

Ayrıca, makinede Windows Update herhangi bir sorun olup olmadığını denetlemek için [Windows Update sorun gidericiyi](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) indirebilir ve çalıştırabilirsiniz.

> [!NOTE]
> [Windows Update sorun giderici](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) Windows istemcilerine yöneliktir, ancak Windows Server 'da da çalışmaktadır.

## <a name="scenario-update-run-returns-status-failed"></a>Senaryo: güncelleştirme çalıştırması geri dönüş durumu "başarısız"

### <a name="issue"></a>Sorun

Bir güncelleştirme çalıştırması başlatılır, ancak çalıştırma sırasında hatalarla karşılaşır.

### <a name="cause"></a>Nedeni

Olası nedenler:

* Paket Yöneticisi sağlam değil.
* Güncelleştirme Aracısı (Windows için WUA, Linux için özel paket yöneticisi) yanlış yapılandırılmış.
* Belirli paketler, bulut tabanlı düzeltme eki uygulamayı etkileyebilir.
* Makineye ulaşılamıyor.
* Güncelleştirmeler çözümlenmedi bağımlılıkları vardı.

### <a name="resolution"></a>Çözünürlük

Bir güncelleştirme çalıştırması sırasında başarılı olduktan sonra bir başarısızlık oluşursa, çalıştırmada etkilenen makinenin [iş çıktısını kontrol](../manage-update-multi.md#view-results-of-an-update-deployment) edin. Çözebileceğinizi ve üzerinde işlem yapmanız gereken makinelerinizden belirli hata iletileri bulabilirsiniz. Güncelleştirme Yönetimi, paket yöneticisinin başarılı güncelleştirme dağıtımları için sağlıklı olmasını gerektirir.

Belirli düzeltme ekleri, paketler veya güncelleştirmeler iş başarısız olmadan hemen önce görülemezse, bunları bir sonraki güncelleştirme dağıtımından [dışlanmasını](../automation-tutorial-update-management.md#schedule-an-update-deployment) deneyebilirsiniz. Windows Update günlük bilgilerini toplamak için bkz. [Windows Update günlük dosyaları](/windows/deployment/update/windows-update-logs).

Düzeltme eki uygulama sorununu çözemezseniz, aşağıdaki günlük dosyasının bir kopyasını oluşturun ve sorun giderme amacıyla bir sonraki güncelleştirme dağıtımı **başlamadan önce** bu dosyayı koruyun:

```bash
/var/opt/microsoft/omsagent/run/automationworker/omsupdatemgmt.log
```

## <a name="patches-are-not-installed"></a>Düzeltme ekleri yüklenmedi

### <a name="machines-do-not-install-updates"></a>Makineler güncelleştirmeleri yüklemez

* Güncelleştirmeleri doğrudan makinede çalıştırmayı deneyin. Makine güncelleştirilemiyorsa [sorun giderme kılavuzundaki olası hatalar listesine](https://docs.microsoft.com/azure/automation/troubleshoot/update-management#hresult) bakın.
* Güncelleştirmeler yerel olarak çalıştırılıyorsa, ["VM'yi Güncelleştirme Yönetimi'nden kaldırma"](https://docs.microsoft.com/azure/automation/automation-update-management#remove-a-vm-from-update-management) bağlantısında sağlanan yönergeleri izleyerek makinedeki aracıyı kaldırmayı ve yeniden yüklemeyi deneyin.

### <a name="i-know-updates-are-available-but-they-dont-show-as-needed-on-my-machines"></a>Güncelleştirmelerin kullanılabildiğini biliyorum, ancak makinelerimde gereken şekilde gösterilmiyorum

* Bu durum genellikle makinelerin güncelleştirmeleri WSUS/SCCM üzerinden alacak şekilde yapılandırılmış olması ancak WSUS/SCCM tarafından güncelleştirmelerin onaylanmamış olması durumunda yaşanır.
* Makinelerinizin WSUS/SCCM ile yapılandırılıp yapılandırılmadığını denetlemek için ["UseWUServer" kayıt defteri anahtarı ile bu belgenin "Kayıt Defterini Düzenleyerek Otomatik Güncelleştirmeleri Yapılandırma" bölümündeki kayıt defteri anahtarları arasında çapraz başvuru oluşturabilirsiniz](https://support.microsoft.com/help/328010/how-to-configure-automatic-updates-by-using-group-policy-or-registry-s)
* Güncelleştirmeler WSUS 'de onaylanmamışsa, yüklenmeyecektir. Aşağıdaki sorguyla Log Analytics onaylanmamış güncelleştirmeleri kontrol edebilirsiniz.

  ```loganalytics
  Update | where UpdateState == "Needed" and ApprovalSource == "WSUS" and Approved == "False" | summarize max(TimeGenerated) by Computer, KBID, Title
  ```

### <a name="updates-show-as-installed-but-i-cant-find-them-on-my-machine"></a>Güncelleştirmeler yüklendi olarak gösteriliyor, ancak makinamda bulamıyorum

* Güncelleştirmeler genellikle başka güncelleştirmelerle değiştirilir. Daha fazla bilgi için [Windows Update Sorun Giderme kılavuzunun "Güncelleştirme değiştirildi" bölümünü](https://docs.microsoft.com/windows/deployment/update/windows-update-troubleshooting#the-update-is-not-applicable-to-your-computer) inceleyin

### <a name="installing-updates-by-classification-on-linux"></a>Linux'da güncelleştirmeleri sınıflandırmaya göre yükleme

* Güncelleştirmeleri Linux'a sınıflandırmaya göre ("Kritik ve güvenlik güncelleştirmeleri") yüklemek, özellikle CentOS için önemli uyarılara tabidir. Bu [sınırlamalar Güncelleştirme Yönetimi'ne genel bakış sayfasında belirtilmiştir](https://docs.microsoft.com/azure/automation/automation-update-management#linux-2)

### <a name="kb2267602-is-consistently--missing"></a>KB2267602, sürekli olarak eksik

* KB2267602, [Windows Defender tanım güncelleştirmesidir](https://www.microsoft.com/wdsi/definitions). Her gün güncelleştirilir.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayı dosyası gönderebilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**' ı seçin.
