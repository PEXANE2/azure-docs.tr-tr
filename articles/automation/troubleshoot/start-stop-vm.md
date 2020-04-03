---
title: Sorun Giderme VM'leri Çalıştırma ve Durdurma - Azure Otomasyonu
description: Bu makalede, Azure Otomasyonu'nda VM'lerin başlatılması ve durdurulması ile ilgili sorun giderme hakkında bilgi verilmektedir.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 04/04/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 73a9680cc570179c47b527a4844488da69193cb3
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80586090"
---
# <a name="troubleshoot-the-startstop-vms-during-off-hours-solution"></a>Mesai saatleri dışında Başlat/Durdur vm'leri çözümlemesini giderme

## <a name="scenario-the-startstop-vm-solution-fails-to-properly-deploy"></a><a name="deployment-failure"></a>Senaryo: Başlat/Durdur VM çözümü düzgün dağıtılamaz

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

1. Seçilen bölgede aynı ada sahip bir Otomasyon Hesabı zaten vardır.
2. Başlat/Durdur VM çözümünün dağıtımına izin verilebilen bir ilke vardır.
3. , `Microsoft.OperationsManagement` `Microsoft.Insights`veya `Microsoft.Automation` kaynak türleri kayıtlı değil.
4. Log Analytics çalışma alanınızda bir kilit vardır.
5. AzureRM modüllerinin veya Başlat/Durdur çözümünün eski bir sürümüne sahipsiniz.

### <a name="resolution"></a>Çözüm

Sorununuzun veya bakılaman Gereken yerlerin olası çözümleri için aşağıdaki listeyi gözden geçirin:

1. Otomasyon hesaplarının, farklı kaynak gruplarında olsalar bile Azure bölgesinde benzersiz olması gerekir. Hedef bölgedeki mevcut Otomasyon Hesaplarınızı kontrol edin.
2. Varolan bir ilke, Başlat/Durdur VM çözümsüiçin gereken bir kaynağın dağıtılmasını önler. Azure portalındaki ilke atamalarınıza gidin ve bu kaynağın dağıtımına izin vermeyen bir ilke atamanız olup olmadığını kontrol edin. Bu konuda daha fazla bilgi edinmek için [RequestDisallowedByPolicy'ye](../../azure-resource-manager/templates/error-policy-requestdisallowedbypolicy.md)bakın.
3. Başlat/Durdur VM çözümünüzü dağıtmak için aboneliğinizin aşağıdaki Azure kaynak ad alanlarına kaydedilmesi gerekir:
    * `Microsoft.OperationsManagement`
    * `Microsoft.Insights`
    * `Microsoft.Automation`

   Bkz. [kaynak sağlayıcı kaydı hatalarının çözümlenerek](../../azure-resource-manager/templates/error-register-resource-provider.md) sağlayıcıları kaydederken hatalar hakkında daha fazla bilgi edinin.
4. Log Analytics çalışma alanınızda bir kilit varsa, Azure portalındaki çalışma alanınıza gidin ve kaynaküzerindeki kilitleri kaldırın.
5. Yukarıdaki çözümler sorununuzu çözmezse, Başlat/Durdur çözümünü yeniden dağıtmak için [Çözümü Güncelleştir'in](../automation-solution-vm-management.md#update-the-solution) altındaki yönergeleri izleyin.

## <a name="scenario-all-vms-fail-to-startstop"></a><a name="all-vms-fail-to-startstop"></a>Senaryo: Tüm VM'ler başlatılamayınca/durduramazlar

### <a name="issue"></a>Sorun

Başlat/Durdur VM çözümlerini yapılandırıldınız, ancak yapılandırılan tüm VM'leri başlatmaz veya durdurmaz.

### <a name="cause"></a>Nedeni

Bu hata aşağıdaki nedenlerden biri neden olabilir:

1. Zamanlama doğru yapılandırılamıyor
2. RunAs hesabı doğru yapılandırılamayabilir
3. Bir runbook hatalarla karşılaşmış olabilir
4. VM'ler dışlanmış olabilir

### <a name="resolution"></a>Çözüm

Sorununuzun veya bakılaman Gereken yerlerin olası çözümleri için aşağıdaki listeyi gözden geçirin:

* Başlat/Durdur VM çözümü için bir zamanlamayı düzgün şekilde yapılandırdığınızdan denetleyin. Zamanlamayı nasıl yapılandırıştırılabildiğini öğrenmek için [Zamanlamalar](../automation-schedules.md) makalesine bakın.

* Herhangi bir hata aramak için [iş akışları](../automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) denetleyin. Portalda, Otomasyon Hesabınıza gidin ve **Süreç Otomasyonu**altındaki **İşleri** seçin. **İşler** sayfasından aşağıdaki runbook'lardan birinde iş arayın:

  * AutoStop_CreateAlert_Child
  * AutoStop_CreateAlert_Parent
  * AutoStop_Disable
  * AutoStop_VM_Child
  * ScheduledStartStop_Base_Classic
  * ScheduledStartStop_Child_Classic
  * ScheduledStartStop_Child
  * ScheduledStartStop_Parent
  * SequencedStartStop_Parent

* [RunAs Hesabınızın](../manage-runas-account.md) başlatmaya veya durdurmaya çalıştığınız VM'lere uygun izinlere sahip olduğunu doğrulayın. Kaynaktaki izinleri nasıl denetleyeceklerini öğrenmek için [Bkz. Hızlı Başlangıç: Azure portalını kullanarak kullanıcıya atanan rolleri görüntüleyin.](../../role-based-access-control/check-access.md) Run As Hesabı tarafından kullanılan hizmet sorumlusuiçin Uygulama Kimliği sağlamanız gerekir. Bu değeri Azure portalındaki Otomasyon Hesabınıza giderek, **Hesap Ayarları** altında hesap **olarak Çalıştır'ı** seçerek ve uygun Hesap Olarak Çalıştır'ı tıklatarak alabilirsiniz.

* VM'ler açıkça dışlanıyorsa başlatılamayabilir veya durdurulamaz. Otomasyon Hesabı'ndaki **External_ExcludeVMNames** değişkeninde belirlenen VM'ler hariç, çözüm dağıtılır. Aşağıdaki örnek, powershell ile bu değeri nasıl sorgulayabileceğinizi gösterir.

  ```powershell-interactive
  Get-AzureRmAutomationVariable -Name External_ExcludeVMNames -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName> | Select-Object Value
  ```

## <a name="scenario-some-of-my-vms-fail-to-start-or-stop"></a><a name="some-vms-fail-to-startstop"></a>Senaryo: Bazı VM'lerim başlatılamaz veya durduramaz

### <a name="issue"></a>Sorun

Başlat/Durdur VM çözümlerini yapılandırıldınız, ancak yapılandırılan Bazı VM'ler başlatmıyor veya durdurmuyor.

### <a name="cause"></a>Nedeni

Bu hata aşağıdaki nedenlerden biri neden olabilir:

1. Sıra senaryosunu kullanıyorsanız, etiket eksik veya yanlış olabilir
2. VM hariç tutulabilir
3. RunAs hesabı VM'de yeterli izine sahip olmayabilir
4. VM'nin başlamasını veya durabını engelleyen bir şey olabilir.

### <a name="resolution"></a>Çözüm

Sorununuzun veya bakılaman Gereken yerlerin olası çözümleri için aşağıdaki listeyi gözden geçirin:

* Mesai saatleri dışında VM'nin [dizi senaryosunu](../automation-solution-vm-management.md) kullanmadan, başlatmak veya durdurmak istediğiniz her VM'nin uygun etikete sahip olduğundan emin olmalısınız. Başlatmak istediğiniz VM'lerin etikete `sequencestart` ve durdurmak istediğiniz VM'lerin `sequencestop` etikete sahip olduğundan emin olun. Her iki etiket de pozitif tamsayı değeri gerektirir. Etiketleri ve değerlerini içeren tüm VM'leri aramak için aşağıdaki örneğe benzer bir sorgu kullanabilirsiniz.

  ```powershell-interactive
  Get-AzureRmResource | ? {$_.Tags.Keys -contains "SequenceStart" -or $_.Tags.Keys -contains "SequenceStop"} | ft Name,Tags
  ```

* VM'ler açıkça dışlanıyorsa başlatılamayabilir veya durdurulamaz. Otomasyon Hesabı'ndaki **External_ExcludeVMNames** değişkeninde belirlenen VM'ler hariç, çözüm dağıtılır. Aşağıdaki örnek, powershell ile bu değeri nasıl sorgulayabileceğinizi gösterir.

  ```powershell-interactive
  Get-AzureRmAutomationVariable -Name External_ExcludeVMNames -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName> | Select-Object Value
  ```

* VM'leri başlatmak ve durdurmak için, Otomasyon hesabının RunAs hesabının VM'ye uygun izinlere sahip olması gerekir. Kaynaktaki izinleri nasıl denetleyeceklerini öğrenmek için [Bkz. Hızlı Başlangıç: Azure portalını kullanarak kullanıcıya atanan rolleri görüntüleyin.](../../role-based-access-control/check-access.md) Run As Hesabı tarafından kullanılan hizmet sorumlusuiçin Uygulama Kimliği sağlamanız gerekir. Bu değeri Azure portalındaki Otomasyon Hesabınıza giderek, **Hesap Ayarları** altında hesap **olarak Çalıştır'ı** seçerek ve uygun Hesap Olarak Çalıştır'ı tıklatarak alabilirsiniz.

* VM'nin başlatılması veya ayrılması nda sorun yaşıyorsa, bu davranış VM'nin kendisiyle ilgili bir sorundan kaynaklanabilir. Bazı örnekler veya olası sorunlar şunlardır, kapatmaya çalışırken bir güncelleştirme uygulanıyor, bir hizmet askıda ve daha fazlası). VM kaynağınıza gidin ve günlüklerde herhangi bir hata olup olmadığını görmek için **Etkinlik Günlükleri'ni** kontrol edin. Ayrıca, Olay günlüklerinde herhangi bir hata olup olmadığını görmek için VM'ye giriş yapmayı da deneyebilirsiniz. VM sorun giderme hakkında daha fazla bilgi edinmek için Azure [sanal makinelerinde Sorun Giderme](../../virtual-machines/troubleshooting/index.yml)

* Herhangi bir hata aramak için [iş akışları](../automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) denetleyin. Portalda, Otomasyon Hesabınıza gidin ve **Süreç Otomasyonu**altındaki **İşleri** seçin.

## <a name="scenario-my-custom-runbook-fails-to-start-or-stop-my-vms"></a><a name="custom-runbook"></a>Senaryo: Özel runbook'um VM'lerimi başlatamıyor veya durduramıyor

### <a name="issue"></a>Sorun

Özel bir runbook'a yazdım veya PowerShell Galerisi'nden indirdiniz ve düzgün çalışmıyor.

### <a name="cause"></a>Nedeni

Başarısızlığın nedeni pek çok şeyden biri olabilir. Azure portalındaki Otomasyon Hesabınıza gidin ve **Süreç Otomasyonu**altındaki **İşler'i** seçin. **İşler** sayfasından, iş hatalarını görüntülemek için runbook'unuzdan iş arayın.

### <a name="resolution"></a>Çözüm

Azure Otomasyonu'nda VM'leri başlatmak ve durdurmak için [mesai saatleri dışında Başlat/Durdur VM'leri](../automation-solution-vm-management.md) kullanmanız önerilir. Bu çözüm Microsoft tarafından yazılır. Özel runbook'lar Microsoft tarafından desteklenmez. [Runbook sorun giderme](runbooks.md) makalesini ziyaret ederek özel runbook'unuzun bir çözüm üslupunu bulabilirsiniz. Bu makalede, her tür runbooks için genel kılavuz ve sorun giderme sağlar. Herhangi bir hata aramak için [iş akışları](../automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) denetleyin. Portalda, Otomasyon Hesabınıza gidin ve **Süreç Otomasyonu**altındaki **İşleri** seçin.

## <a name="scenario-vms-dont-start-or-stop-in-the-correct-sequence"></a><a name="dont-start-stop-in-sequence"></a>Senaryo: VM'ler doğru sırada başlamaz veya durmaz

### <a name="issue"></a>Sorun

Çözümde yapılandırdığınız VM'ler doğru sırada başlayıp durmaz.

### <a name="cause"></a>Nedeni

Bunun nedeni, VM'lerde yanlış etiketlemedir.

### <a name="resolution"></a>Çözüm

Çözümün doğru şekilde yapılandırıldığından emin olmak için aşağıdaki adımları izleyin.

1. Durumunuza bağlı olarak tüm VM'lerin başlatılmasından veya durdurulduğundan `sequencestart` `sequencestop` emin olun. Bu etiketlerin değeri olarak pozitif bir tamsayı gerekir. VM'ler bu değere göre artan sırada işlenir.
2. Başlatılacak veya durdurulacak VM'ler için kaynak `External_Start_ResourceGroupNames` `External_Stop_ResourceGroupNames` gruplarının durumunuza bağlı olarak değişkenlerde olduğundan emin olun.
3. Değişikliklerinizi önizlemek `SequencedStartStop_Parent` için True için WHATIF parametresi ayarlanmış olan runbook'u çalıştırarak değişikliklerinizi test edin.

VM'leri sırayla başlatmak ve durdurmak için çözümün nasıl kullanılacağına ilişkin daha ayrıntılı ve ek talimatlar [için, sırayla Başlat/Durdur VM'leri'ne](../automation-solution-vm-management.md)bakın.

## <a name="scenario-startstop-vm-job-fails-with-403-forbidden-status"></a><a name="403"></a>Senaryo: Başlat/Durdur VM işi 403 yasak durumuyla başarısız olur

### <a name="issue"></a>Sorun

Mesai saatleri dışında çözüm `403 forbidden` çalışma kitaplarında Başlat/Durdur VM'leri için bir hatayla başarısız olan işler bulursunuz.

### <a name="cause"></a>Nedeni

Bu sorun, yanlış yapılandırılmış veya süresi dolmuş bir Run As Hesabı neden olabilir. Bunun nedeni, Otomasyon Hesapları Hesabı Olarak Çalıştırılan VM kaynaklarına yetersiz izinler olması da olabilir.

### <a name="resolution"></a>Çözüm

Hesabınızın düzgün yapılandırılmış olduğunu kontrol etmek için Azure portalındaki Otomasyon Hesabınıza gidin ve **Hesap Ayarları**altında hesap **olarak Çalıştır'ı** seçin. Burada, bir Run As Hesabı yanlış yapılandırılmış veya süresi dolmuşsa, run of your run'ın durumunu hesap olarak görürsünüz.

Run As hesabınız yanlış yapılandırılmışsa, Run As hesabınızı silmeli ve yeniden oluşturmalısınız. Bkz. [Azure Otomasyonu Hesabı Olarak Çalıştır'](../manage-runas-account.md)ı yönetin.

Run As Hesabınız için sertifikanın süresi dolduysa, sertifikayı yenilemek için [Kendi Imzalı sertifika yenilemede](../manage-runas-account.md#cert-renewal) listelenen adımları izleyin.

Sorun, eksik izinlerden kaynaklanıyor olabilir. Kaynaktaki izinleri nasıl denetleyeceklerini öğrenmek için [Bkz. Hızlı Başlangıç: Azure portalını kullanarak kullanıcıya atanan rolleri görüntüleyin.](../../role-based-access-control/check-access.md) Run As Hesabı tarafından kullanılan hizmet sorumlusuiçin Uygulama Kimliği sağlamanız gerekir. Bu değeri Azure portalındaki Otomasyon Hesabınıza giderek, **Hesap Ayarları** altında hesap **olarak Çalıştır'ı** seçerek ve uygun Hesap Olarak Çalıştır'ı tıklatarak alabilirsiniz.

## <a name="scenario-my-problem-isnt-listed-above"></a><a name="other"></a>Senaryo: Sorunum yukarıda listelenmez

### <a name="issue"></a>Sorun

Bu sayfada listelenmeyen mesai dışı çözüm sırasında Başlat/Durdur VM'lerini kullanırken bir sorun veya beklenmeyen bir sonuçla karşılaşırsınız.

### <a name="cause"></a>Nedeni

Çoğu kez hatalar çözümün eski ve eski bir sürümünü kullanarak neden olabilir.

> [!NOTE]
> Mesai saatleri dışında Başlat/Durdur vm'leri, çözümü dağıttığınızda Otomasyon Hesabınıza aktarılan Azure modülleriyle sınanmıştır. Çözüm şu anda Azure modülünün yeni sürümleriyle çalışmıyor. Bu yalnızca mesai dışı çözüm sırasında Başlat/Durdur VM'lerini çalıştırmak için kullandığınız Otomasyon Hesabını etkiler. [Azure PowerShell modüllerini Azure Otomasyonu'nda nasıl güncelleştirebileceğinizde](../automation-update-azure-modules.md) açıklandığı gibi diğer Otomasyon Hesaplarınızda Azure modülünün daha yeni sürümlerini kullanmaya devam edebilirsiniz

### <a name="resolution"></a>Çözüm

Birçok hatanın giderilmesi için çözümü kaldırması ve güncelleştirmesi önerilir. Çözümü nasıl güncelleştireceklerini öğrenmek için, [mesai saatleri dışında başlat/durdur vm'lerini güncelleştir'e](../automation-solution-vm-management.md#update-the-solution)bakın. Ayrıca, herhangi bir hata aramak için [iş akışları](../automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) denetleyebilirsiniz. Portalda, Otomasyon Hesabınıza gidin ve **Süreç Otomasyonu**altındaki **İşleri** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.
