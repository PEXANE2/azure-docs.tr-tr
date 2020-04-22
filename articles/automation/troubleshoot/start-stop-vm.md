---
title: Mesai saatleri dışında Başlat/Durdur vm'leri çözümlemesini sorun giderme
description: Bu makalede, Başlat/Durdur VM çözümlerini sorun giderme hakkında bilgi verilmektedir.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 04/04/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 003c2c5a2c09957e7a3a4ac0a26b87a9ac43dace
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81679156"
---
# <a name="troubleshoot-the-startstop-vms-during-off-hours-solution"></a>Mesai saatleri dışında Başlat/Durdur vm'leri çözümlemesini giderme

Bu makalede, mesai saatleri dışında Başlat/Durdur VM'lerle çalışırken ortaya çıkan sorun giderme sorunları hakkında bilgi verilmektedir.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](../automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="scenario-the-startstop-vms-during-off-hours-solution-fails-to-properly-deploy"></a><a name="deployment-failure"></a>Senaryo: Mesai saatleri dışında Başlat/Durdur VM çözümü düzgün bir şekilde dağıtılamaz

### <a name="issue"></a>Sorun

Mesai saatleri [dışında Başlat/Durdur VM'lerini](../automation-solution-vm-management.md)dağıtırken aşağıdaki hatalardan birini alırsınız:

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

1. Seçilen bölgede aynı ada sahip bir Otomasyon hesabı zaten var.
2. Bir ilke, mesai saatleri dışında başlat/durdur VM'lerinin dağıtımına izin vermiyor.
3. , `Microsoft.OperationsManagement` `Microsoft.Insights`veya `Microsoft.Automation` kaynak türü kayıtlı değil.
4. Log Analytics çalışma alanınız kilitli.
5. Mesai saatleri dışında AzureRM modüllerinin veya Start/Stop VM'lerinin eski bir sürümüne sahipsiniz.

### <a name="resolution"></a>Çözüm

Sorununuzun olası çözümleri için aşağıdaki düzeltmeleri gözden geçirin:

* Otomasyon hesaplarının, farklı kaynak gruplarında olsalar bile Azure bölgesinde benzersiz olması gerekir. Hedef bölgedeki mevcut Otomasyon hesaplarınızı kontrol edin.
* Varolan bir ilke, mesai saatleri dışında Başlat/Durdur vm'leri için gereken kaynağın dağıtılmasını önler. Azure portalındaki ilke atamalarınıza gidin ve bu kaynağın dağıtımına izin vermeyen bir ilke atamanız olup olmadığını kontrol edin. Bu konuda daha fazla bilgi edinmek için [RequestDisallowedByPolicy'ye](../../azure-resource-manager/templates/error-policy-requestdisallowedbypolicy.md)bakın.
* Başlat/Durdur VM çözümlerini dağıtmak için aboneliğinizin aşağıdaki Azure kaynak ad alanlarına kaydedilmesi gerekir:

    * `Microsoft.OperationsManagement`
    * `Microsoft.Insights`
    * `Microsoft.Automation`

   Sağlayıcılar kaydederken hatalar hakkında daha fazla bilgi edinmek [için kaynak sağlayıcı kaydı hatalarının giderilmesine](../../azure-resource-manager/templates/error-register-resource-provider.md) bakın.
* Log Analytics çalışma alanınızda bir kilit varsa, Azure portalındaki çalışma alanınıza gidin ve kaynaküzerindeki kilitleri kaldırın.
* Yukarıdaki çözümler sorununuzu çözmezse, Başlat/Durdur çözümünü yeniden dağıtmak için [Çözümü Güncelleştir'in](../automation-solution-vm-management.md#update-the-solution) altındaki yönergeleri izleyin.

## <a name="scenario-all-vms-fail-to-start-or-stop"></a><a name="all-vms-fail-to-startstop"></a>Senaryo: Tüm VM'ler başlatılamaz veya durduramaz

### <a name="issue"></a>Sorun

Mesai saatleri dışında Başlat/Durdur VM'leri yapılandırıldınız, ancak tüm VM'leri başlatmaz veya durdurmaz.

### <a name="cause"></a>Nedeni

Bu hata aşağıdaki nedenlerden biri neden olabilir:

1. Zamanlama doğru yapılandırılmıyor.
2. Çalıştır Hesabı doğru yapılandırılamayabilir.
3. Bir runbook hatalarla karşıya koşmuş olabilir.
4. VM'ler dışlanmış olabilir.

### <a name="resolution"></a>Çözüm

Sorununuzun olası çözümleri için aşağıdaki listeyi gözden geçirin:

* Mesai saatleri dışında başlat/durdur vm'leri için bir zamanlama yıkmış sınız mı denetleyin. Zamanlamayı nasıl yapılandırıştırılabildiğini öğrenmek için [Zamanlamalar](../automation-schedules.md) makalesine bakın.

* Herhangi bir hata aramak için [iş akışları](../automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) denetleyin. Aşağıdaki runbook'lardan birinde iş arayın:

  * **AutoStop_CreateAlert_Child**
  * **AutoStop_CreateAlert_Parent**
  * **AutoStop_Disable**
  * **AutoStop_VM_Child**
  * **ScheduledStartStop_Base_Classic**
  * **ScheduledStartStop_Child_Classic**
  * **ScheduledStartStop_Child**
  * **ScheduledStartStop_Parent**
  * **SequencedStartStop_Parent**

* [Çalıştır](../manage-runas-account.md) Hesabınızın başlatmaya veya durdurmaya çalıştığınız VM'lere uygun izinlere sahip olduğunu doğrulayın. Kaynaktaki izinleri nasıl denetleyeceklerini öğrenmek için [Bkz. Hızlı Başlangıç: Azure portalını kullanarak kullanıcıya atanan rolleri görüntüleyin.](../../role-based-access-control/check-access.md) Run As hesabı tarafından kullanılan hizmet sorumlusunun başvuru kimliğini sağlamanız gerekir. Bu değeri Azure portalındaki Otomasyon hesabınıza giderek, **Hesap Ayarları**altında hesap **olarak Çalıştır'ı** seçerek ve uygun Çalıştır hesabını tıklayarak alabilirsiniz.

* VM'ler açıkça dışlanıyorsa başlatılamayabilir veya durdurulamaz. Dışlanan VM'ler, `External_ExcludeVMNames` çözümün dağıtıldığı Otomasyon hesabındaki değişkende ayarlanır. Aşağıdaki örnek, powershell ile bu değeri nasıl sorgulayabileceğinizi gösterir.

  ```powershell-interactive
  Get-AzAutomationVariable -Name External_ExcludeVMNames -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName> | Select-Object Value
  ```

## <a name="scenario-some-of-my-vms-fail-to-start-or-stop"></a><a name="some-vms-fail-to-startstop"></a>Senaryo: Bazı VM'lerim başlatılamaz veya durduramaz

### <a name="issue"></a>Sorun

Kapalı saatlerde Başlat/Durdur VM'leri yapılandırdın, ancak yapılandırılan Bazı VM'ler başlamaz veya durdurmaz.

### <a name="cause"></a>Nedeni

Bu hata aşağıdaki nedenlerden biri neden olabilir:

1. Sıra senaryosunda, bir etiket eksik veya yanlış olabilir.
2. VM hariç tutulabilir.
3. Run As hesabının VM'de yeterli izinleri olmayabilir.
4. VM'nin başlatılmasını veya durabını engelleyen bir sorunu olabilir.

### <a name="resolution"></a>Çözüm

Sorununuzun veya bakılaman Gereken yerlerin olası çözümleri için aşağıdaki listeyi gözden geçirin:

* Mesai saatleri dışında başlat/durdur un vm'lerinin [dizi senaryosunu](../automation-solution-vm-management.md) kullanırken, başlatmak veya durdurmak istediğiniz her VM'nin uygun etikete sahip olduğundan emin olmalısınız. Başlatmak istediğiniz VM'lerin etikete `sequencestart` ve durdurmak istediğiniz VM'lerin `sequencestop` etikete sahip olduğundan emin olun. Her iki etiket de pozitif tamsayı değeri gerektirir. Etiketleri ve değerlerini içeren tüm VM'leri aramak için aşağıdaki örneğe benzer bir sorgu kullanabilirsiniz.

  ```powershell-interactive
  Get-AzResource | ? {$_.Tags.Keys -contains "SequenceStart" -or $_.Tags.Keys -contains "SequenceStop"} | ft Name,Tags
  ```

* VM'ler açıkça dışlanıyorsa başlatılamayabilir veya durdurulabilir. Dışlanan VM'ler, `External_ExcludeVMNames` çözümün dağıtıldığı Otomasyon hesabındaki değişkende ayarlanır. Aşağıdaki örnek, powershell ile bu değeri nasıl sorgulayabileceğinizi gösterir.

  ```powershell-interactive
  Get-AzAutomationVariable -Name External_ExcludeVMNames -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName> | Select-Object Value
  ```

* VM'leri başlatmak ve durdurmak için, Otomasyon hesabının Run As hesabının VM'ye uygun izinlere sahip olması gerekir. Kaynaktaki izinleri nasıl denetleyeceklerini öğrenmek için [Bkz. Hızlı Başlangıç: Azure portalını kullanarak kullanıcıya atanan rolleri görüntüleyin.](../../role-based-access-control/check-access.md) Run As hesabı tarafından kullanılan hizmet sorumlusunun başvuru kimliğini sağlamanız gerekir. Bu değeri Azure portalındaki Otomasyon hesabınıza giderek, **Hesap Ayarları** altında hesap **olarak Çalıştır'ı** seçerek ve uygun Çalıştır hesabını tıklayarak alabilirsiniz.

* VM'nin başlatılması veya ayrılması nda sorun yaşıyorsa, VM'nin kendisinde bir sorun olabilir. Örneğin, VM kapanmaya çalışırken, bir hizmet askıda ve daha fazlası olduğunda bir güncelleştirme uygulanır. VM kaynağınıza gidin ve günlüklerde herhangi bir hata olup olmadığını görmek için **Etkinlik Günlükleri'ni** kontrol edin. Ayrıca, olay günlüklerinde herhangi bir hata olup olmadığını görmek için VM'de oturum açmaya da çalışabilirsiniz. VM sorun giderme hakkında daha fazla bilgi edinmek için Azure [sanal makinelerinde Sorun Giderme](../../virtual-machines/troubleshooting/index.yml)

* Herhangi bir hata aramak için [iş akışları](../automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) denetleyin. Portalda, Otomasyon hesabınıza gidin ve **Süreç Otomasyonu**altındaki **İşler'i** seçin.

## <a name="scenario-my-custom-runbook-fails-to-start-or-stop-my-vms"></a><a name="custom-runbook"></a>Senaryo: Özel runbook'um VM'lerimi başlatamıyor veya durduramıyor

### <a name="issue"></a>Sorun

Özel bir runbook'a yazdım veya PowerShell Galerisi'nden indirdiniz ve düzgün çalışmıyor.

### <a name="cause"></a>Nedeni

Hatanın birçok nedeni olabilir. Azure portalındaki Otomasyon hesabınıza gidin ve **Süreç Otomasyonu**altındaki **İşler'i** seçin. İşler sayfasından, iş hatalarını görüntülemek için runbook'unuzdan iş arayın.

### <a name="resolution"></a>Çözüm

Bu önerilir:

* Azure Otomasyonu'nda VM'leri başlatmak ve durdurmak için [mesai saatleri dışında Başlat/Durdur VM'leri](../automation-solution-vm-management.md) kullanın. Bu çözüm Microsoft tarafından yazılır. 

* Microsoft'un özel runbook'ları desteklemediğini unutmayın. Runbook [sorun giderme](runbooks.md)özel runbook için bir çözüm bulabilirsiniz. Herhangi bir hata aramak için [iş akışları](../automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) denetleyin. 

## <a name="scenario-vms-dont-start-or-stop-in-the-correct-sequence"></a><a name="dont-start-stop-in-sequence"></a>Senaryo: VM'ler doğru sırada başlamaz veya durmaz

### <a name="issue"></a>Sorun

Çözümde yapılandırdığınız VM'ler doğru sırada başlayıp durmaz.

### <a name="cause"></a>Nedeni

Bu sorun, VM'lerde yanlış etiketlemeden kaynaklanır.

### <a name="resolution"></a>Çözüm

Çözümün doğru şekilde yapılandırıldığından emin olmak için aşağıdaki adımları izleyin.

1. Durumunuza bağlı olarak tüm VM'lerin başlatılmasından veya durdurulduğundan `sequencestart` `sequencestop` emin olun. Bu etiketlerin değeri olarak pozitif bir tamsayı gerekir. VM'ler bu değere göre artan sırada işlenir.
2. Başlatılacak veya durdurulacak VM'ler için kaynak `External_Start_ResourceGroupNames` `External_Stop_ResourceGroupNames` gruplarının durumunuza bağlı olarak değişkenlerde olduğundan emin olun.
3. Değişikliklerinizi önizlemek `SequencedStartStop_Parent` için True `WHATIF` için ayarlanan parametre ile runbook'u çalıştırarak değişikliklerinizi test edin.
4. VM'leri sırayla başlatmak ve durdurmak için çözümü kullanma hakkında daha fazla şey [için, sırayla Başlat/Durdur VM'leri'ne](../automation-solution-vm-management.md)bakın.

## <a name="scenario-startstop-vms-during-off-hours-job-fails-with-403-forbidden-error"></a><a name="403"></a>Senaryo: Mesai saatleri dışında VM'leri başlat/durdurun iş 403 yasak hatayla başarısız

### <a name="issue"></a>Sorun

Mesai saatleri dışında çözüm `403 forbidden` çalışma kitaplarında Başlat/Durdur VM'leri için bir hatayla başarısız olan işler bulursunuz.

### <a name="cause"></a>Nedeni

Bu sorun, yanlış yapılandırılmış veya süresi dolmuş bir Run As hesabı neden olabilir. Bunun nedeni, Run As hesabı tarafından VM kaynaklarına yetersiz izinler olması da olabilir.

### <a name="resolution"></a>Çözüm

Run As hesabınızın düzgün şekilde yapılandırıldığından doğrulamak için Azure portalındaki Otomasyon hesabınıza gidin ve **Hesap Ayarları** **altında hesap olarak Çalıştır'ı** seçin. Bir Çalıştır Çalıştır Hesabı yanlış yapılandırılmış veya süresi geçmişse, durum durumu gösterir.

Run As hesabınız yanlış yapılandırılmışsa, Run As hesabınızı silmeli ve yeniden oluşturmalısınız. Bkz. [Azure Otomasyonu Hesabı Olarak Çalıştır'](../manage-runas-account.md)ı yönetin.

Run As hesabınız için sertifikanın süresi dolmuşsa, sertifikayı yenilemek için [Kendi Imzalı sertifika yenileme](../manage-runas-account.md#cert-renewal) adımlarına bakın.

Eksik izinler varsa, [Bkz. Hızlı Başlangıç: Azure portalını kullanarak bir kullanıcıya atanan rolleri görüntüleyin.](../../role-based-access-control/check-access.md) Run As hesabı tarafından kullanılan hizmet sorumlusunun başvuru kimliğini sağlamanız gerekir. Bu değeri Azure portalındaki Otomasyon hesabınıza giderek, **Hesap Ayarları**altında hesap **olarak Çalıştır'ı** seçerek ve uygun Çalıştır hesabını tıklayarak alabilirsiniz.

## <a name="scenario-my-problem-isnt-listed-above"></a><a name="other"></a>Senaryo: Sorunum yukarıda listelenmez

### <a name="issue"></a>Sorun

Bu sayfada listelenmeyen mesai saatleri dışında Başlat/Durdur VM'lerini kullanırken bir sorun veya beklenmeyen bir sonuçla karşılaşırsınız.

### <a name="cause"></a>Nedeni

Çoğu kez hatalar çözümün eski ve eski bir sürümünü kullanarak neden olabilir.

> [!NOTE]
> Mesai saatleri dışında Başlat/Durdur vm'leri, çözümü dağıttığınızda Otomasyon hesabınıza aktarılan Azure modülleriyle sınanmıştır. Çözüm şu anda Azure modülünün yeni sürümleriyle çalışmıyor. Bu yalnızca mesai saatleri dışında Başlat/Durdur VM'lerini çalıştırmak için kullandığınız Otomasyon hesabını etkiler. [Azure PowerShell modüllerini Azure Otomasyonu'nda nasıl güncelleştirebileceğinizde](../automation-update-azure-modules.md) açıklandığı gibi, diğer Otomasyon hesaplarınızda Azure modülünün daha yeni sürümlerini kullanmaya devam edebilirsiniz

### <a name="resolution"></a>Çözüm

Birçok hatanın giderilmesi için, [mesai saatleri dışında Başlat/Durdur VM çözümlerini](../automation-solution-vm-management.md#update-the-solution)kaldırması ve güncelleştirmesi önerilir. Ayrıca, herhangi bir hata aramak için [iş akışları](../automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) denetleyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu yukarıda görmüyorsanız veya sorununuzu çözemiyorsanız, ek destek için aşağıdaki kanallardan birini deneyin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıt alın.
* [@AzureSupport](https://twitter.com/azuresupport)Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.
* Azure destek olayı dosyala. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.