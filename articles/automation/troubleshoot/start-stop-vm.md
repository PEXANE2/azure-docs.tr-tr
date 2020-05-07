---
title: Çalışma saatleri dışında VM 'Leri Başlat/Durdur çözümü sorunlarını giderme
description: Bu makale, çalışma saatleri dışında sanal makineyi Başlat/Durdur çözümünde sorun giderme hakkında bilgi sağlar.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 04/04/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 611e8441fab56114ca010d0b555c9ed156ae9d40
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82855066"
---
# <a name="troubleshoot-the-startstop-vms-during-off-hours-solution"></a>Çalışma saatleri dışında VM 'Leri Başlat/Durdur çözümü sorunlarını giderme

Bu makalede, Azure Otomasyonu 'nda çalışma saatleri dışında sanal makineler çözümü ile çalışırken ortaya çıkan sorunları giderme hakkında bilgi sağlanır.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Azure Otomasyonu hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](../automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="scenario-the-startstop-vms-during-off-hours-solution-fails-to-properly-deploy"></a><a name="deployment-failure"></a>Senaryo: kapalı saatlerin dışında VM 'Leri Başlat/Durdur çözümü düzgün bir şekilde dağıtılamazsa

### <a name="issue"></a>Sorun

[Çalışma saatleri dışında VM 'Leri Başlat/Durdur çözümü](../automation-solution-vm-management.md)dağıttığınızda, aşağıdaki hatalardan birini alırsınız:

```error
Account already exists in another resourcegroup in a subscription. ResourceGroupName: [MyResourceGroup].
```

```error
Resource 'StartStop_VM_Notification' was disallowed by policy. Policy identifiers: '[{\\\"policyAssignment\\\":{\\\"name\\\":\\\"[MyPolicyName]".
```

```error
The subscription is not registered to use namespace 'Microsoft.OperationsManagement'.
```

```error
The subscription is not registered to use namespace 'Microsoft.Insights'.
```

```error
The scope '/subscriptions/000000000000-0000-0000-0000-00000000/resourcegroups/<ResourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<WorkspaceName>/views/StartStopVMView' cannot perform write operation because following scope(s) are locked: '/subscriptions/000000000000-0000-0000-0000-00000000/resourceGroups/<ResourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<WorkspaceName>/views/StartStopVMView'. Please remove the lock and try again
```

```error
A parameter cannot be found that matches parameter name 'TagName'
```

```error
Start-AzureRmVm : Run Login-AzureRmAccount to login
```

### <a name="cause"></a>Nedeni

Dağıtımlar aşağıdaki nedenlerden biri nedeniyle başarısız olabilir:

- Seçilen bölgede aynı ada sahip bir Otomasyon hesabı zaten var.
- İlke, çalışma saatleri dışında VM 'Leri Başlat/Durdur çözümü dağıtımına izin vermez.
- `Microsoft.OperationsManagement`, `Microsoft.Insights`Veya `Microsoft.Automation` kaynak türü kayıtlı değil.
- Log Analytics çalışma alanınız kilitli.
- Azurerd modüllerinin güncel olmayan bir sürümüne veya saatler dışında VM 'Leri Başlat/Durdur çözümünde sahipsiniz.

### <a name="resolution"></a>Çözüm

Sorununuza yönelik olası çözümler için aşağıdaki düzeltmeleri gözden geçirin:

* Otomasyon hesaplarının farklı kaynak gruplarında olsalar bile, bir Azure bölgesi içinde benzersiz olması gerekir. Hedef bölgede mevcut Otomasyon hesaplarınızı kontrol edin.
* Mevcut bir ilke, çalışma saatleri dışında VM 'Leri başlatma/durdurma çözümünün dağıtılması için gereken bir kaynağı engeller. Azure portal ilke atamalarınız ' ne gidin ve bu kaynağın dağıtımına izin vermeyen bir ilke atamamı olup olmadığınızı denetleyin. Daha fazla bilgi için bkz. [Requestdisallowedbypolicy hatası](../../azure-resource-manager/templates/error-policy-requestdisallowedbypolicy.md).
* VM 'Leri Başlat/Durdur çözümünü dağıtmak için aboneliğinizin aşağıdaki Azure Kaynak ad alanlarına kayıtlı olması gerekir:

    * `Microsoft.OperationsManagement`
    * `Microsoft.Insights`
    * `Microsoft.Automation`

   Sağlayıcıları kaydettiğinizde hatalar hakkında daha fazla bilgi edinmek için bkz. [kaynak sağlayıcısı kaydı hatalarını giderme](../../azure-resource-manager/templates/error-register-resource-provider.md).
* Log Analytics çalışma alanınızda bir kilit varsa, Azure portal çalışma alanınıza gidin ve kaynaktaki kilitleri kaldırın.
* Bu çözümler sorununuzu çözmezse, çalışma saatleri dışında VM 'Leri Başlat/Durdur çözümünü yeniden dağıtmak için [çözümü Güncelleştir](../automation-solution-vm-management.md#update-the-solution) bölümündeki yönergeleri izleyin.

## <a name="scenario-all-vms-fail-to-start-or-stop"></a><a name="all-vms-fail-to-startstop"></a>Senaryo: tüm VM 'Ler başlatılamadı veya durdurulamıyor

### <a name="issue"></a>Sorun

Çalışma saatleri dışında VM 'Leri Başlat/Durdur çözümünü yapılandırdınız, ancak tüm VM 'Leri başlatamaz veya durdurmaz.

### <a name="cause"></a>Nedeni

Bu hata, aşağıdakilerden biri nedeniyle oluşabilir:

- Bir zamanlama doğru yapılandırılmamış.
- Farklı Çalıştır hesabı doğru şekilde yapılandırılmamış olabilir.
- Bir runbook 'ta çalıştırma hataları olabilir.
- VM 'Ler dışarıda bırakılmış olabilir.

### <a name="resolution"></a>Çözüm

Sorununuza yönelik olası çözümler için aşağıdaki listeyi gözden geçirin:

* Çalışma saatleri dışında VM 'Leri Başlat/Durdur çözümü için bir zamanlamayı doğru şekilde yapılandırdığınızdan emin olun. Zamanlamayı yapılandırma hakkında bilgi edinmek için bkz. [zamanlamalar](../automation-schedules.md).

* Herhangi bir hata bulmak için [iş akışlarını](../automation-runbook-execution.md#job-statuses) denetleyin. Aşağıdaki runbook 'lardan birindeki işleri arayın:

  * **AutoStop_CreateAlert_Child**
  * **AutoStop_CreateAlert_Parent**
  * **AutoStop_Disable**
  * **AutoStop_VM_Child**
  * **ScheduledStartStop_Base_Classic**
  * **ScheduledStartStop_Child_Classic**
  * **ScheduledStartStop_Child**
  * **ScheduledStartStop_Parent**
  * **SequencedStartStop_Parent**

* [Farklı Çalıştır hesabınızın](../manage-runas-account.md) başlatmaya çalıştığınız veya durdurduğunuz VM 'ler için uygun izinlere sahip olduğunu doğrulayın. Bir kaynaktaki izinleri nasıl denetleyeceğinizi öğrenmek için bkz. [hızlı başlangıç: Azure Portal kullanarak bir kullanıcıya atanan rolleri görüntüleme](../../role-based-access-control/check-access.md). Farklı Çalıştır hesabı tarafından kullanılan hizmet sorumlusu için uygulama KIMLIĞINI sağlamanız gerekir. Azure portal Otomasyon hesabınıza giderek bu değeri alabilirsiniz. **Hesap ayarları**' nın altında **Farklı Çalıştır hesapları** ' nı seçin ve uygun farklı çalıştır hesabını seçin.

* VM 'Ler açıkça dışlandıklarında, sanal makineler başlatılmamış veya durdurulmuş olabilir. Dışlanan VM 'Ler, Çözümün dağıtıldığı `External_ExcludeVMNames` Otomasyon hesabındaki değişkende ayarlanır. Aşağıdaki örnek, bu değeri PowerShell ile nasıl sorgulayakullanabileceğinizi gösterir.

  ```powershell-interactive
  Get-AzAutomationVariable -Name External_ExcludeVMNames -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName> | Select-Object Value
  ```

## <a name="scenario-some-of-my-vms-fail-to-start-or-stop"></a><a name="some-vms-fail-to-startstop"></a>Senaryo: bazı sanal makinelerimin başlatılması veya durdurulması başarısız oluyor

### <a name="issue"></a>Sorun

Çalışma saatleri dışında VM 'Leri Başlat/Durdur çözümünü yapılandırdınız, ancak yapılandırılmış VM 'lerin bazılarını başlatmıyor veya durdurmayacak.

### <a name="cause"></a>Nedeni

Bu hata, aşağıdakilerden biri nedeniyle oluşabilir:

- Sıra senaryosunda, bir etiket eksik veya yanlış olabilir.
- VM dışlanıyor olabilir.
- Farklı Çalıştır hesabı VM 'de yeterli izinlere sahip olmayabilir.
- VM 'nin başlamasını veya durdurulmasını durduran bir sorun olabilir.

### <a name="resolution"></a>Çözüm

Sorununuz için olası çözümleri veya aranacak konumları görmek için aşağıdaki listeyi gözden geçirin:

* Çalışma saatleri dışında VM 'Leri Başlat/Durdur çözümünün [sıra senaryosunu](../automation-solution-vm-management.md) kullandığınızda, başlatmak veya durdurmak ISTEDIĞINIZ her VM 'nin doğru etikete sahip olduğundan emin olmanız gerekir. Başlatmak istediğiniz sanal makinelerin `sequencestart` etiketine ve durdurmak `sequencestop` istediğiniz VM 'lere sahip olduğundan emin olun. Her iki etiket de pozitif bir tamsayı değeri gerektirir. Etiketleri ve bunların değerlerini içeren tüm VM 'Leri aramak için aşağıdaki örneğe benzer bir sorgu kullanabilirsiniz.

  ```powershell-interactive
  Get-AzResource | ? {$_.Tags.Keys -contains "SequenceStart" -or $_.Tags.Keys -contains "SequenceStop"} | ft Name,Tags
  ```

* VM 'Ler açıkça dışlandıklarında, sanal makineler başlatılmamış veya durdurulmuş olabilir. Dışlanan VM 'Ler, Çözümün dağıtıldığı `External_ExcludeVMNames` Otomasyon hesabındaki değişkende ayarlanır. Aşağıdaki örnek, bu değeri PowerShell ile nasıl sorgulayakullanabileceğinizi gösterir.

  ```powershell-interactive
  Get-AzAutomationVariable -Name External_ExcludeVMNames -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName> | Select-Object Value
  ```

* VM 'Leri başlatmak ve durdurmak için, Otomasyon hesabının farklı çalıştır hesabı VM 'de uygun izinlere sahip olmalıdır. Bir kaynaktaki izinleri nasıl denetleyeceğinizi öğrenmek için bkz. [hızlı başlangıç: Azure Portal kullanarak bir kullanıcıya atanan rolleri görüntüleme](../../role-based-access-control/check-access.md). Farklı Çalıştır hesabı tarafından kullanılan hizmet sorumlusu için uygulama KIMLIĞINI sağlamanız gerekir. Azure portal Otomasyon hesabınıza giderek bu değeri alabilirsiniz. **Hesap ayarları** altında **Farklı Çalıştır hesapları** ' nı seçin ve uygun farklı çalıştır hesabını seçin.
* VM 'de başlatma veya ayırmayı kaldırma konusunda bir sorun varsa, sanal makinenin kendisi üzerinde bir sorun olabilir. Örnekler, sanal makine kapanmaya çalışırken uygulanan bir güncelleştirmedir, askıda olan bir hizmet ve daha fazlasını sağlar. Günlüklerde hata olup olmadığını görmek için VM kaynağına gidin ve **etkinlik günlükleri** ' ne bakın. Ayrıca, olay günlüklerinde herhangi bir hata olup olmadığını görmek için VM 'de oturum açma girişiminde bulunabilir. VM 'nizin sorunlarını giderme hakkında daha fazla bilgi edinmek için bkz. [Azure sanal makinelerinde sorun giderme](../../virtual-machines/troubleshooting/index.yml).
* Herhangi bir hata bulmak için [iş akışlarını](../automation-runbook-execution.md#job-statuses) denetleyin. Portalda Otomasyon hesabınıza gidin ve **Işlem Otomasyonu**altında **işler** ' i seçin.

## <a name="scenario-my-custom-runbook-fails-to-start-or-stop-my-vms"></a><a name="custom-runbook"></a>Senaryo: özel runbook sunucum sanal makinelerimi başlatamıyor veya durdurabilir

### <a name="issue"></a>Sorun

Özel bir runbook 'u yazdı veya PowerShell Galerisi bir tane indirdiğiniz için düzgün çalışmıyor.

### <a name="cause"></a>Nedeni

Hatanın pek çok nedeni olabilir. Azure portal Otomasyon hesabınıza gidin ve **Işlem Otomasyonu**altında **işler** ' i seçin. **İşler** sayfasından, iş başarısızlıklarını görüntülemek için Runbook 'ınızdan işler ' i arayın.

### <a name="resolution"></a>Çözüm

Şunları yapmanızı öneririz:

* Azure Otomasyonu 'nda VM 'Leri başlatmak ve durdurmak için [VM 'leri saatlerde Başlat/Durdur çözümünü](../automation-solution-vm-management.md) kullanın. Bu çözüm Microsoft tarafından yazılmıştır. 
* Microsoft 'un özel runbook 'ları desteklemediğini unutmayın. [Runbook sorun gidermesinden](runbooks.md)özel runbook için bir çözüm bulabilirsiniz. Herhangi bir hata bulmak için [iş akışlarını](../automation-runbook-execution.md#job-statuses) denetleyin. 

## <a name="scenario-vms-dont-start-or-stop-in-the-correct-sequence"></a><a name="dont-start-stop-in-sequence"></a>Senaryo: VM 'Ler doğru sırada başlamıyor veya durdurulmayacak

### <a name="issue"></a>Sorun

Çözümde yapılandırdığınız VM 'Ler doğru sırada başlatılmaz veya durduramaz.

### <a name="cause"></a>Nedeni

Bu sorun, VM 'lerde hatalı etiketlemesinin oluşmasına neden olur.

### <a name="resolution"></a>Çözüm

Çözümün doğru yapılandırıldığından emin olmak için aşağıdaki adımları izleyin.

1. Durumunuza bağlı olarak, tüm VM 'Lerin başlatılmış veya durdurulmuş `sequencestart` `sequencestop` olduğundan emin olun. Bu etiketlerin değeri olarak pozitif bir tamsayı olması gerekir. VM 'Ler, bu değere göre artan sırada işlenir.
1. Başlatılacak veya durdurulacak VM 'Ler için kaynak gruplarının, durumunuza bağlı olarak `External_Start_ResourceGroupNames` veya `External_Stop_ResourceGroupNames` değişkenlerinde olduğundan emin olun.
1. Değişikliklerinizi önizlemek için `WHATIF` parametresi true olarak `SequencedStartStop_Parent` ayarlanmış şekilde runbook 'u yürüterek yaptığınız değişiklikleri test edin.

VM 'Leri sırayla başlatmak ve durdurmak için çözümü kullanma hakkında daha fazla bilgi için, bkz. [sırasıyla VM 'Leri başlatma/durdurma](../automation-solution-vm-management.md).

## <a name="scenario-startstop-vms-during-off-hours-job-fails-with-403-forbidden-error"></a><a name="403"></a>Senaryo: çalışma saatleri dışında VM 'Leri başlatma/durdurma işi 403 yasaklanmış hatasıyla başarısız oluyor

### <a name="issue"></a>Sorun

Çalışma saatleri dışında VM 'Leri Başlat/ `403 forbidden` durdur çözüm runbook 'ları için hata vererek başarısız olan işleri bulabilirsiniz.

### <a name="cause"></a>Nedeni

Bu sorun, yanlış yapılandırılmış veya zaman aşımına uğramamış farklı çalıştır hesabından kaynaklanabilir. Ayrıca, farklı çalıştır hesabı tarafından VM kaynaklarına yetersiz izinler de olabilir.

### <a name="resolution"></a>Çözüm

Farklı Çalıştır hesabınızın düzgün yapılandırıldığını doğrulamak için, Azure portal Otomasyon hesabınıza gidin ve **Hesap ayarları**altında **Farklı Çalıştır hesapları** ' nı seçin. Farklı Çalıştır hesabı yanlış yapılandırılmamışsa veya süre dolmuşsa, durum koşulu gösterir.

Farklı Çalıştır hesabınız yanlış yapılandırılmış ise, farklı çalıştır hesabınızı silin ve yeniden oluşturun. Daha fazla bilgi için bkz. [Azure Otomasyonu farklı çalıştır hesaplarını yönetme](../manage-runas-account.md).

Farklı Çalıştır hesabınız için sertifikanın geçerliliği dolmuşsa, sertifikayı yenilemek için [otomatik olarak imzalanan sertifika yenileme](../manage-runas-account.md#cert-renewal) bölümündeki adımları izleyin.

Eksik izinler varsa bkz. [hızlı başlangıç: Azure Portal kullanarak bir kullanıcıya atanan rolleri görüntüleme](../../role-based-access-control/check-access.md). Farklı Çalıştır hesabı tarafından kullanılan hizmet sorumlusu için uygulama KIMLIĞINI sağlamanız gerekir. Azure portal Otomasyon hesabınıza giderek bu değeri alabilirsiniz. **Hesap ayarları**' nın altında **Farklı Çalıştır hesapları** ' nı seçin ve uygun farklı çalıştır hesabını seçin.

## <a name="scenario-my-problem-isnt-listed-here"></a><a name="other"></a>Senaryo: sorunum burada listelenmiyor

### <a name="issue"></a>Sorun

Bu sayfada listelenmeyen saat dışı VM 'Leri Başlat/Durdur çözümünü kullandığınızda bir sorunla veya beklenmedik sonuca karşılaşabilirsiniz.

### <a name="cause"></a>Nedeni

Birçok kez hatanın nedeni çözümün eski ve güncel olmayan bir sürümü kullanılıyor olabilir.

> [!NOTE]
> Çalışma saatleri dışında VM 'Leri Başlat/Durdur çözümü, çözümü dağıtırken Otomasyon hesabınıza içeri aktarılan Azure modülleriyle test edilmiştir. Çözüm şu anda Azure modülünün daha yeni sürümleriyle çalışmıyor. Bu kısıtlama yalnızca saat dışı saatlerde VM 'Leri Başlat/Durdur çözümü çalıştırmak için kullandığınız Otomasyon hesabını etkiler. Azure [Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](../automation-update-azure-modules.md)bölümünde açıklandığı gibi diğer otomasyon hesaplarınızda Azure modülünün daha yeni sürümlerini kullanmaya devam edebilirsiniz.

### <a name="resolution"></a>Çözüm

Birçok hatayı çözümlemek için, [çalışma saatleri dışında VM 'Leri Başlat/Durdur çözümünü](../automation-solution-vm-management.md#update-the-solution)kaldırın ve güncelleştirin. Ayrıca, herhangi bir hata aramak için [iş akışlarını](../automation-runbook-execution.md#job-statuses) kontrol edebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu burada görmüyorsanız veya sorununuzu çözemezseniz, ek destek için aşağıdaki kanallardan birini deneyin:

* Azure [forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıtlar alın.
* [@AzureSupport](https://twitter.com/azuresupport)Müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabıyla bağlanın. Azure desteği, Azure Community 'yi yanıtlar, destek ve uzmanlar için bağlar.
* Azure destek olayı dosyası oluşturma. [Azure destek sitesine](https://azure.microsoft.com/support/options/)gidin ve **Destek Al**' ı seçin.