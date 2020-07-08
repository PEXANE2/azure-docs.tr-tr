---
title: Azure Otomasyonu Güncelleştirme Yönetimi sorunlarını giderme
description: Bu makalede, Azure Otomasyonu Güncelleştirme Yönetimi sorunları nasıl giderebileceğiniz ve giderebileceğiniz açıklanır.
services: automation
ms.date: 06/30/2020
ms.topic: conceptual
ms.service: automation
ms.openlocfilehash: 95e3fc12a77124c32e220d700a112f52cbad08fb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85801895"
---
# <a name="troubleshoot-update-management-issues"></a>Güncelleştirme Yönetimi sorunlarını giderme

Bu makalede, makinelerinizde Güncelleştirme Yönetimi özelliğini dağıttığınızda çalıştırabileceğiniz sorunlar ele alınmaktadır. Temeldeki sorunu belirlemede karma Runbook Worker Aracısı için bir aracı sorun giderici vardır. Sorun giderici hakkında daha fazla bilgi edinmek için bkz. [Windows Update Aracısı sorunlarını giderme](update-agent-issues.md) ve [Linux Güncelleştirme Aracısı sorunlarını giderme](update-agent-issues-linux.md). Diğer özellik dağıtım sorunları için bkz. [özellik dağıtım sorunlarını giderme](onboarding.md).

>[!NOTE]
>Bir Windows makinesinde Güncelleştirme Yönetimi dağıttığınızda sorunlarla karşılaşırsanız, Windows Olay Görüntüleyicisi açın ve yerel makinedeki **uygulama ve hizmet günlükleri** altında **Operations Manager** olay günlüğünü kontrol edin. Olay KIMLIĞI 4502 ve olay ayrıntıları içeren olayları arayın `Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent` .

## <a name="scenario-you-receive-the-error-failed-to-enable-the-update-solution"></a><a name="failed-to-enable-error"></a>Senaryo: "güncelleştirme çözümü etkinleştirilemedi" hatasını alıyorsunuz

### <a name="issue"></a>Sorun

Otomasyon hesabınızda Güncelleştirme Yönetimi etkinleştirmeye çalıştığınızda şu hatayı alırsınız:

```error
Error details: Failed to enable the Update solution
```

### <a name="cause"></a>Nedeni

Bu hata, aşağıdaki nedenlerden dolayı oluşabilir:

* Log Analytics aracısına yönelik ağ güvenlik duvarı gereksinimleri doğru şekilde yapılandırılmamış olabilir. Bu durum, DNS URL 'Leri çözümlenirken aracının başarısız olmasına neden olabilir.

* Güncelleştirme Yönetimi hedefleme yanlış yapılandırılmış ve makine, güncelleştirmeleri beklendiği gibi almıyor.

* Ayrıca, makinenin uyumluluk altında durumunun olduğunu da fark edebilirsiniz `Non-compliant` . **Compliance** Aynı zamanda, **aracı masaüstü Analizi** aracıyı olarak bildirir `Disconnected` .

### <a name="resolution"></a>Çözüm

* İşletim sistemine bağlı olarak [Windows](update-agent-issues.md#troubleshoot-offline) veya [Linux](update-agent-issues-linux.md#troubleshoot-offline)için sorun gidericiyi çalıştırın.

* Güncelleştirme Yönetimi çalışması için hangi adreslere ve bağlantı noktalarına izin verileceğini öğrenmek için [ağ yapılandırması](../automation-hybrid-runbook-worker.md#network-planning) ' na gidin.  

* Kapsam yapılandırma sorunlarını denetleyin. [Kapsam yapılandırması](../automation-scope-configurations-update-management.md) , hangi makinelerin güncelleştirme yönetimi yapılandırıldığını belirler. Makinenizin çalışma alanınızda gösterilmesi, ancak Güncelleştirme Yönetimi istemiyorsanız, kapsam yapılandırmasını makineleri hedefleyecek şekilde ayarlamanız gerekir. Kapsam yapılandırması hakkında bilgi edinmek için bkz. [çalışma alanındaki makineleri etkinleştirme](../automation-onboard-solutions-from-automation-account.md#enable-machines-in-the-workspace).

* [Karma runbook çalışanını şirket içi bir Windows bilgisayarından kaldırma](../automation-windows-hrw-install.md#remove-windows-hybrid-runbook-worker) bölümündeki adımları Izleyerek veya [karma Runbook Worker 'ı şirket içi bir Linux bilgisayarından kaldırarak](../automation-linux-hrw-install.md#remove-linux-hybrid-runbook-worker)çalışan yapılandırmasını kaldırın. 

## <a name="scenario-superseded-update-indicated-as-missing-in-update-management"></a>Senaryo: yenisiyle değiştirilen güncelleştirme Güncelleştirme Yönetimi eksik olarak belirtildi

### <a name="issue"></a>Sorun

Eski güncelleştirmeler, yenisiyle değiştirilseler bile bir Otomasyon hesabı için eksik olarak görünür. Aynı güvenlik açığını düzelten sonraki bir güncelleştirme mevcut olduğundan, yenisiyle değiştirilen bir güncelleştirme, yüklemenize gerek kalmaz. Güncelleştirme Yönetimi, yenisiyle değiştirilen güncelleştirmeyi yoksayar ve yerine geçen güncelleştirmenin yerine uygulanabilir değildir. İlgili bir sorun hakkında daha fazla bilgi için bkz. [Update 'in yerini almıştır](https://docs.microsoft.com/windows/deployment/update/windows-update-troubleshooting#the-update-is-not-applicable-to-your-computer).

### <a name="cause"></a>Nedeni

Yenisiyle değiştirilen güncelleştirmeler uygun değil olarak kabul edilebilmesi için reddedildi olarak gösterilmemektedir.

### <a name="resolution"></a>Çözüm

Yenisiyle değiştirilen bir güncelleştirme %100 ' a uygulanamıyorsa, bu güncelleştirmenin onay durumunu olarak değiştirmelisiniz `Declined` . Tüm güncelleştirmelerinizin onay durumunu değiştirmek için:

1. Otomasyon hesabında, makine durumunu görüntülemek için **güncelleştirme yönetimi** ' yi seçin. Bkz. [güncelleştirme değerlendirmelerini görüntüleme](../manage-update-multi.md#view-an-update-assessment).

2. %100 olduğundan emin olmak için yenisiyle değiştirilen güncelleştirmeyi denetleyin. 

3. Güncelleştirme hakkında sorularınız yoksa güncelleştirme reddedildi olarak işaretleyin. 

4. **Bilgisayarlar** ' ı seçin ve **Uyumluluk** sütununda uyumluluk için yeniden taramaya zorlayın. Bkz. [birden çok makine için güncelleştirmeleri yönetme](../manage-update-multi.md).

5. Yenisiyle değiştirilen diğer güncelleştirmeler için yukarıdaki adımları tekrarlayın.

6. Reddedilen güncelleştirmelerden dosyaları silmek için Temizleme Sihirbazı 'nı çalıştırın. 

7. Windows Server Update Services (WSUS) için, tüm yenisiyle değiştirilen güncelleştirmeleri altyapıyı yenilemek için el ile temizleyin.

8. Görüntüleme sorununu düzeltmek ve güncelleştirme yönetimi için kullanılan disk alanı miktarını en aza indirmek için bu yordamı düzenli olarak tekrarlayın.

## <a name="scenario-machines-dont-show-up-in-the-portal-under-update-management"></a><a name="nologs"></a>Senaryo: makineler altında portalda gösterilmez Güncelleştirme Yönetimi

### <a name="issue"></a>Sorun

Makineleriniz aşağıdaki belirtilerle sahiptir:

* Makineniz `Not configured` , BIR VM 'nin güncelleştirme yönetimi görünümünden gösterilir.

* Azure Otomasyonu hesabınızın Güncelleştirme Yönetimi görünümünde makineleriniz eksik.

* Uyumluluk altında olarak görünen makineleriniz `Not assessed` vardır **Compliance**. Ancak, karma Runbook Worker için Azure Izleyici günlüklerinde sinyal verilerini görürsünüz, ancak Güncelleştirme Yönetimi için değildir.

### <a name="cause"></a>Nedeni

Bu soruna yerel yapılandırma sorunları veya yanlış yapılandırılmış kapsam yapılandırması neden olabilir. Olası belirli nedenler şunlardır:

* Karma runbook çalışanını yeniden kaydetmeniz ve yeniden yüklemeniz gerekebilir.

* Çalışma alanınızda ulaşılan ve daha fazla veri depolamayı engelleyen bir kota tanımlamış olabilirsiniz.

### <a name="resolution"></a>Çözüm

1. İşletim sistemine bağlı olarak [Windows](update-agent-issues.md#troubleshoot-offline) veya [Linux](update-agent-issues-linux.md#troubleshoot-offline)için sorun gidericiyi çalıştırın.

2. Makinenizin doğru çalışma alanına bildirimde bulunduğundan emin olun. Bu yönü doğrulamaya ilişkin yönergeler için bkz. [Log Analytics için aracı bağlantısını doğrulayın](../../azure-monitor/platform/agent-windows.md#verify-agent-connectivity-to-log-analytics). Ayrıca, bu çalışma alanının Azure Otomasyonu hesabınıza bağlı olduğundan emin olun. Onaylamak için Otomasyon hesabınıza gidin ve **Ilgili kaynaklar**altında **bağlantılı çalışma alanı** ' nı seçin.

3. Makinelerin Otomasyon hesabınıza bağlı Log Analytics çalışma alanında göründüğünden emin olun. Log Analytics çalışma alanında aşağıdaki sorguyu çalıştırın.

   ```kusto
   Heartbeat
   | summarize by Computer, Solutions
   ```

4. Makinenizi sorgu sonuçlarında görmüyorsanız, son zamanlarda iade edilmedi demektir. Büyük olasılıkla yerel bir yapılandırma sorunu var ve [aracıyı yeniden yüklemeniz](../../azure-monitor/learn/quick-collect-windows-computer.md#install-the-agent-for-windows)gerekir. 

5. Makineniz sorgu sonuçlarında görünüyorsa, kapsam yapılandırma sorunlarını kontrol edin. [Kapsam yapılandırması](../automation-scope-configurations-update-management.md) , hangi makinelerin güncelleştirme yönetimi yapılandırıldığını belirler. 

6. Makinenizin çalışma alanınızda gösterilmesi, ancak Güncelleştirme Yönetimi istemiyorsanız, kapsam yapılandırmasını makineyi hedeflemek üzere yapılandırmanız gerekir. Bunun nasıl yapılacağını öğrenmek için bkz. [çalışma alanındaki makineleri etkinleştirme](../automation-onboard-solutions-from-automation-account.md#enable-machines-in-the-workspace).

7. Çalışma alanınızda bu sorguyu çalıştırın.

   ```kusto
   Operation
   | where OperationCategory == 'Data Collection Status'
   | sort by TimeGenerated desc
   ```

8. Bir `Data collection stopped due to daily limit of free data reached. Ingestion status = OverQuota` sonuç alırsanız, çalışma alanınızda tanımlanan kotaya ulaşıldı ve bu da verilerin kaydedilmesini durdurdu. Çalışma alanınızda, **kullanım ve tahmini maliyetler**altında **veri hacmi yönetimi** ' ne gidin ve kotayı değiştirin veya kaldırın.

9. Sorununuz hala çözülmedi ise, Windows için karma çalışanı yeniden yüklemek üzere [Windows karma Runbook Worker dağıtma](../automation-windows-hrw-install.md) bölümündeki adımları izleyin. Linux için, [Linux karma runbook çalışanı dağıtma](../automation-linux-hrw-install.md)' daki adımları izleyin.

## <a name="scenario-unable-to-register-automation-resource-provider-for-subscriptions"></a><a name="rp-register"></a>Senaryo: abonelikler için Otomasyon kaynak sağlayıcısı kaydedilemiyor

### <a name="issue"></a>Sorun

Otomasyon hesabınızda Özellik dağıtımlarıyla çalışırken aşağıdaki hata oluşur:

```error
Error details: Unable to register Automation Resource Provider for subscriptions
```

### <a name="cause"></a>Nedeni

Otomasyon kaynak sağlayıcısı abonelikte kayıtlı değil.

### <a name="resolution"></a>Çözüm

Otomasyon kaynak sağlayıcısını kaydetmek için Azure portal aşağıdaki adımları izleyin.

1. Portalın altındaki Azure hizmeti listesinde, **tüm hizmetler**' i seçin ve ardından Genel hizmet grubundaki **abonelikler** ' i seçin.

2. Aboneliğinizi seçin.

3. **Ayarlar**altında **kaynak sağlayıcıları**' nı seçin.

4. Kaynak sağlayıcıları listesinden, Microsoft. Automation kaynak sağlayıcısının kayıtlı olduğunu doğrulayın.

5. Listede yoksa, [kaynak sağlayıcısı kaydı hatalarını giderme](/azure/azure-resource-manager/resource-manager-register-provider-errors)adımlarını izleyerek Microsoft. Automation sağlayıcısını kaydedin.

## <a name="scenario-scheduled-update-with-a-dynamic-schedule-missed-some-machines"></a><a name="scheduled-update-missed-machines"></a>Senaryo: dinamik bir zamanlamaya sahip zamanlanmış güncelleştirme bazı makineler kaçırdı

### <a name="issue"></a>Sorun

Güncelleştirme önizlemesindeki makineler, zamanlanmış çalıştırma sırasında düzeltme eki eklenen makineler listesinde görünmez.

### <a name="cause"></a>Nedeni

Bu sorunun nedeni aşağıdakilerden biri olabilir:

* Dinamik bir sorgudaki kapsamda tanımlanan abonelikler, kayıtlı Otomasyon kaynak sağlayıcısı için yapılandırılmamış.

* Zamanlama yürütüldüğünde makineler kullanılabilir değil veya uygun etiketlere sahip değil.

### <a name="resolution"></a>Çözüm

#### <a name="subscriptions-not-configured-for-registered-automation-resource-provider"></a>Kayıtlı Otomasyon kaynak sağlayıcısı için abonelikler yapılandırılmadı

Aboneliğiniz Otomasyon kaynak sağlayıcısı için yapılandırılmamışsa, Bu abonelikteki makinelere ilişkin bilgileri sorgulayabilir veya alamazsınız. Abonelik kaydını doğrulamak için aşağıdaki adımları kullanın.

1. [Azure Portal](https://docs.microsoft.com/azure/azure-resource-manager/management/resource-providers-and-types#azure-portal)Azure hizmet listesine erişin.

2. **Tüm hizmetler**' i seçin ve ardından Genel hizmet grubundaki **abonelikler** ' i seçin. 

3. Dağıtımınızın kapsamında tanımlanan aboneliği bulun.

4. **Ayarlar**altında **kaynak sağlayıcıları**' nı seçin.

5. Microsoft. Automation kaynak sağlayıcısının kayıtlı olduğunu doğrulayın.

6. Listede yoksa, [kaynak sağlayıcısı kaydı hatalarını giderme](/azure/azure-resource-manager/resource-manager-register-provider-errors)adımlarını izleyerek Microsoft. Automation sağlayıcısını kaydedin.

#### <a name="machines-not-available-or-not-tagged-correctly-when-schedule-executed"></a>Zamanlama yürütüldüğünde makineler kullanılamıyor veya doğru şekilde etiketlenmedi

Aboneliğiniz Otomasyon kaynak sağlayıcısı için yapılandırılmışsa, ancak belirli [dinamik gruplarla](../automation-update-management-groups.md) güncelleştirme zamanlaması çalıştırıldığında bazı makineler kaçırıldığında aşağıdaki yordamı kullanın.

1. Azure portal Otomasyon hesabını açın ve **güncelleştirme yönetimi**' ı seçin.

2. Güncelleştirme dağıtımının çalıştırıldığı zamanı tam olarak öğrenmek için [güncelleştirme yönetimi geçmişi](https://docs.microsoft.com/azure/automation/manage-update-multi#view-results-of-an-update-deployment) kontrol edin. 

3. Güncelleştirme Yönetimi tarafından kaçırıldığınıza şüphelendiğiniz makinelerde, [makine değişikliklerini bulmak](https://docs.microsoft.com/azure/governance/resource-graph/how-to/get-resource-changes#find-detected-change-events-and-view-change-details)Için Azure Kaynak Grafiği 'NI (arg) kullanın. 

4. Güncelleştirme dağıtımının çalıştırılmasından önce, bir gün gibi önemli bir süre içinde değişiklikler arayın.

5. Değişiklik silme veya güncelleştirme gibi tüm systemik değişikliklerinin bu dönemdeki makinelere yönelik arama sonuçlarını denetleyin. Bu değişiklikler makine durumunu veya etiketlerini değiştirebilir, böylece Güncelleştirmeler dağıtıldığında makinelerin makine listesinde seçilmemesi sağlanır.

6. Makine durumu veya etiket sorunlarını düzeltmek için gereken makineleri ve kaynak ayarlarını ayarlayın.

7. Belirtilen dinamik gruplara sahip dağıtımın tüm makineleri içerdiğinden emin olmak için güncelleştirme zamanlamasını yeniden çalıştırın.

## <a name="scenario-expected-machines-dont-appear-in-preview-for-dynamic-group"></a><a name="machines-not-in-preview"></a>Senaryo: beklenen makineler dinamik grup için önizlemede görünmüyor

### <a name="issue"></a>Sorun

Dinamik bir grubun seçili kapsamlarının VM 'Leri Azure portal önizleme listesinde gösterilmiyor. Bu liste, seçilen kapsamlar için bir ARG sorgusu tarafından alınan tüm makinelerden oluşur. Kapsamlar, karma runbook çalışanları yüklü olan ve erişim izinlerine sahip olan makineler için filtrelenir. 

### <a name="cause"></a>Nedeni
 
Bu sorunun olası nedenleri şunlardır:

* Seçili kapsamlar üzerinde doğru erişiminiz yok.
* ARG sorgusu beklenen makineleri almaz.
* Karma Runbook Worker makinelerde yüklü değil.

### <a name="resolution"></a>Çözüm 

#### <a name="incorrect-access-on-selected-scopes"></a>Seçili kapsamlar üzerinde yanlış erişim

Azure portal yalnızca belirli bir kapsamda yazma erişiminizin bulunduğu makineleri görüntüler. Kapsam için doğru erişiminiz yoksa, bkz. [öğretici: Azure KAYNAKLARıNA RBAC ve Azure Portal kullanarak Kullanıcı erişimi verme](https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal).

#### <a name="arg-query-doesnt-return-expected-machines"></a>ARG sorgusu beklenen makineleri döndürmüyor

Sorgularınızın doğru şekilde çalışıp çalışmadığını öğrenmek için aşağıdaki adımları izleyin.

1. Azure portal içinde kaynak grafik Gezgini dikey penceresinde aşağıda gösterildiği gibi biçimlendirilen bir ARG sorgusu çalıştırın. Bu sorgu, Güncelleştirme Yönetimi içinde dinamik grubu oluştururken seçtiğiniz filtreleri taklit eder. Bkz. [güncelleştirme yönetimi dinamik grupları kullanma](https://docs.microsoft.com/azure/automation/automation-update-management-groups). 

    ```kusto
    where (subscriptionId in~ ("<subscriptionId1>", "<subscriptionId2>") and type =~ "microsoft.compute/virtualmachines" and properties.storageProfile.osDisk.osType == "<Windows/Linux>" and resourceGroup in~ ("<resourceGroupName1>","<resourceGroupName2>") and location in~ ("<location1>","<location2>") )
    | project id, location, name, tags = todynamic(tolower(tostring(tags)))
    | where  (tags[tolower("<tagKey1>")] =~ "<tagValue1>" and tags[tolower("<tagKey2>")] =~ "<tagValue2>") // use this if "All" option selected for tags
    | where  (tags[tolower("<tagKey1>")] =~ "<tagValue1>" or tags[tolower("<tagKey2>")] =~ "<tagValue2>") // use this if "Any" option selected for tags
    | project id, location, name, tags
    ```

   Örnek aşağıda verilmiştir:

    ```kusto
    where (subscriptionId in~ ("20780d0a-b422-4213-979b-6c919c91ace1", "af52d412-a347-4bc6-8cb7-4780fbb00490") and type =~ "microsoft.compute/virtualmachines" and properties.storageProfile.osDisk.osType == "Windows" and resourceGroup in~ ("testRG","withinvnet-2020-01-06-10-global-resources-southindia") and location in~ ("australiacentral","australiacentral2","brazilsouth") )
    | project id, location, name, tags = todynamic(tolower(tostring(tags)))
    | where  (tags[tolower("ms-resource-usage")] =~ "azure-cloud-shell" and tags[tolower("temp")] =~ "temp")
    | project id, location, name, tags
    ```
 
2. Aradığınız makinelerin sorgu sonuçlarında listelenip listelenmediğini denetleyin. 

3. Makineler listede yoksa, dinamik grupta Seçili filtre ile ilgili bir sorun olabilir. Grup yapılandırmasını gereken şekilde ayarlayın.

#### <a name="hybrid-runbook-worker-not-installed-on-machines"></a>Karma Runbook Worker makinelerde yüklü değil

Makineler, bağımsız değişken sorgu sonuçlarında görünür, ancak yine de dinamik grup önizlemede gösterilmez. Bu durumda, makineler karma çalışanlar olarak atanmayabilir, bu nedenle Azure Otomasyonu ve Güncelleştirme Yönetimi işleri çalıştırılamaz. Görmeyi beklediğiniz makinelerin karma runbook çalışanları olarak ayarlandığından emin olmak için:

1. Azure portal, doğru şekilde görünmeyen bir makine için Otomasyon hesabına gidin.

2. **Işlem Otomasyonu**altında **karma çalışan grupları** ' nı seçin.

3. **Sistem karma çalışanı grupları** sekmesini seçin.

4. Karma çalışanın bu makine için mevcut olduğunu doğrulayın.

5. Makine karma çalışan olarak ayarlanmamışsa, [karma runbook çalışanı kullanarak veri merkezinizdeki veya buluttaki kaynakları otomatik hale](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker)getirmeye yönelik yönergeleri kullanarak ayarlamalar yapın.

6. Makineyi karma Runbook Worker grubuna ekleyin.

7. Önizlemede görüntülemediğiniz tüm makineler için yukarıdaki adımları tekrarlayın.

## <a name="scenario-update-management-components-enabled-while-vm-continues-to-show-as-being-configured"></a><a name="components-enabled-not-working"></a>Senaryo: Güncelleştirme Yönetimi bileşenleri etkin, ancak VM yapılandırılmakta olmaya devam ediyor

### <a name="issue"></a>Sorun

Dağıtım başladıktan sonra VM 15 dakika üzerinde aşağıdaki iletiyi görmeye devam edersiniz:

```error
The components for the 'Update Management' solution have been enabled, and now this virtual machine is being configured. Please be patient, as this can sometimes take up to 15 minutes.
```

### <a name="cause"></a>Nedeni

Bu hata, aşağıdaki nedenlerden dolayı oluşabilir:

* Otomasyon hesabıyla iletişim engelleniyor.

* Farklı kaynak bilgisayar kimliklerine sahip yinelenen bir bilgisayar adı vardır. Bu senaryo, belirli bir bilgisayar adına sahip bir VM farklı kaynak gruplarında oluşturulduğunda ve abonelikte aynı lojistik Aracısı çalışma alanına raporlarken oluşur.

* Dağıtılan VM görüntüsü, Windows için Log Analytics Aracısı ile sistem hazırlığı (Sysprep) ile hazırlanmamış bir kopyalanmış makineden gelebilir.

### <a name="resolution"></a>Çözüm

VM ile ilgili tam sorunu belirlemede yardımcı olması için, Otomasyon hesabınıza bağlı Log Analytics çalışma alanında aşağıdaki sorguyu çalıştırın.

```
Update
| where Computer contains "fillInMachineName"
| project TimeGenerated, Computer, SourceComputerId, Title, UpdateState 
```

#### <a name="communication-with-automation-account-blocked"></a>Otomasyon hesabı ile iletişim engellendi

Güncelleştirme Yönetimi çalışması için hangi adreslere ve bağlantı noktalarına izin verileceğini öğrenmek için [ağ planlamasına](../automation-update-management.md#ports) gidin.

#### <a name="duplicate-computer-name"></a>Yinelenen bilgisayar adı

Ortamlarında benzersiz adlar sağlamak için sanal makinelerinizi yeniden adlandırın.

#### <a name="deployed-image-from-cloned-machine"></a>Kopyalanmış makineden dağıtılan görüntü

Kopyalanmış bir görüntü kullanıyorsanız, farklı bilgisayar adlarında aynı kaynak bilgisayar KIMLIĞI vardır. Bu durumda:

1. Log Analytics çalışma alanınızda, gösterilen kapsam yapılandırması için VM 'yi kayıtlı aramadan kaldırın `MicrosoftDefaultScopeConfig-Updates` . Kayıtlı aramalar, çalışma alanınızda **genel** altında bulunabilir.

2. Aşağıdaki cmdlet 'i çalıştırın.

    ```azurepowershell-interactive
    Remove-Item -Path "HKLM:\software\microsoft\hybridrunbookworker" -Recurse -Force
    ```

3. `Restart-Service HealthService`Sistem sağlığı hizmetini yeniden başlatmak için ' i çalıştırın. Bu işlem anahtarı yeniden oluşturur ve yeni bir UUID oluşturur.

4. Bu yaklaşım işe yaramazsa, önce görüntüde Sysprep 'i çalıştırın ve ardından MMA 'yı yüklemeniz gerekir.

## <a name="scenario-you-receive-a-linked-subscription-error-when-you-create-an-update-deployment-for-machines-in-another-azure-tenant"></a><a name="multi-tenant"></a>Senaryo: başka bir Azure kiracısındaki makineler için bir güncelleştirme dağıtımı oluştururken bağlantılı bir abonelik hatası alıyorsunuz

### <a name="issue"></a>Sorun

Başka bir Azure kiracısındaki makineler için bir güncelleştirme dağıtımı oluşturmaya çalıştığınızda aşağıdaki hatayla karşılaşırsınız:

```error
The client has permission to perform action 'Microsoft.Compute/virtualMachines/write' on scope '/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Automation/automationAccounts/automationAccountName/softwareUpdateConfigurations/updateDeploymentName', however the current tenant '00000000-0000-0000-0000-000000000000' is not authorized to access linked subscription '00000000-0000-0000-0000-000000000000'.
```

### <a name="cause"></a>Nedeni

Bu hata, bir güncelleştirme dağıtımında yer alan başka bir kiracıda Azure VM 'Leri olan bir güncelleştirme dağıtımı oluşturduğunuzda oluşur.

### <a name="resolution"></a>Çözüm

Zamanlanmış bu öğeleri almak için aşağıdaki geçici çözümü kullanın. Bir zamanlama oluşturmak için [New-AzAutomationSchedule](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationschedule?view=azps-3.7.0) cmdlet 'ini parametresiyle birlikte kullanabilirsiniz `ForUpdateConfiguration` . Ardından [New-AzAutomationSoftwareUpdateConfiguration](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationSoftwareUpdateConfiguration?view=azps-3.7.0) cmdlet 'ini kullanın ve diğer Kiracıdaki makineleri `NonAzureComputer` parametresine geçirin. Aşağıdaki örnek bunun nasıl yapılacağını göstermektedir:

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$s = New-AzAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdateConfiguration

New-AzAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName $aa -Schedule $s -Windows -AzureVMResourceId $azureVMIdsW -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

## <a name="scenario-unexplained-reboots"></a><a name="node-reboots"></a>Senaryo: açıklanamayan yeniden başlatmalar

### <a name="issue"></a>Sorun

**Yeniden başlatma denetimi** seçeneğini **hiçbir şekilde yeniden başlatmaya**ayarlamış olsanız bile, güncelleştirmeler yüklendikten sonra makineler yeniden başlatılıyor.

### <a name="cause"></a>Nedeni

Windows Update, herhangi biri yeniden başlatma davranışını değiştirebilecek çeşitli kayıt defteri anahtarları tarafından değiştirilebilir.

### <a name="resolution"></a>Çözüm

Makinelerinizin düzgün yapılandırıldığından emin olmak için [yeniden başlatmayı yönetmek üzere kullanılan](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) kayıt defteri ve kayıt defteri anahtarlarını [düzenleyerek otomatik güncelleştirmeleri yapılandırma](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry) altında listelenen kayıt defteri anahtarlarını gözden geçirin.

## <a name="scenario-machine-shows-failed-to-start-in-an-update-deployment"></a><a name="failed-to-start"></a>Senaryo: bir güncelleştirme dağıtımında makine "başlatılamadı" olarak gösteriliyor

### <a name="issue"></a>Sorun

Bir makine bir `Failed to start` durumu gösterir. Makinenin belirli ayrıntılarını görüntülediğinizde, aşağıdaki hatayı görürsünüz:

```error
Failed to start the runbook. Check the parameters passed. RunbookName Patch-MicrosoftOMSComputer. Exception You have requested to create a runbook job on a hybrid worker group that does not exist.
```

### <a name="cause"></a>Nedeni

Bu hatanın oluşmasının nedeni aşağıdakilerden biri olabilir:

* Makine artık yok.
* Makine kapalı ve ulaşılamaz durumda.
* Makine bir ağ bağlantısı sorununa sahiptir ve bu nedenle makinedeki karma çalışana ulaşılamıyor.
* MMA 'nın kaynak bilgisayar KIMLIĞINI değiştiren bir güncelleştirmesi vardı.
* Bir Otomasyon hesabında 2000 eşzamanlı iş sınırına ulaşırsanız güncelleştirme çalışması kısıtlanıyor. Her dağıtım bir iş olarak değerlendirilir ve bir güncelleştirme dağıtımındaki her makine iş olarak sayılır. Otomasyon hesabınızda çalışmakta olan diğer herhangi bir Otomasyon işi veya güncelleştirme dağıtımı, eşzamanlı iş sınırına doğru sayılır.

### <a name="resolution"></a>Çözüm

Uygun olduğunda, güncelleştirme dağıtımlarınız için [dinamik grupları](../automation-update-management-groups.md) kullanın. Ayrıca, aşağıdaki adımları gerçekleştirebilirsiniz.

1. Makinenin hala var olduğunu ve erişilebilir olduğunu doğrulayın. 
2. Makine yoksa, dağıtımınızı düzenleyin ve makineyi kaldırın.
3. Güncelleştirme Yönetimi için gereken bağlantı noktaları ve adreslerin listesi için [ağ planlama](../automation-update-management.md#ports) bölümüne bakın ve sonra makinenizin bu gereksinimleri karşıladığını doğrulayın.
4. Karma Runbook Worker Aracısı sorun gidericisini kullanarak karma Runbook Worker bağlantısını doğrulayın. Sorun giderici hakkında daha fazla bilgi için bkz. [Güncelleştirme Aracısı sorunlarını giderme](update-agent-issues.md).
5. Ortamınızdaki kaynak bilgisayar KIMLIĞININ değiştiği makineleri bulmak için Log Analytics aşağıdaki sorguyu çalıştırın. Aynı `Computer` değere ancak farklı bir değere sahip bilgisayarları arayın `SourceComputerId` .

   ```kusto
   Heartbeat | where TimeGenerated > ago(30d) | distinct SourceComputerId, Computer, ComputerIP
   ```

6. Etkilenen makineleri bulduktan sonra, bu makineleri hedefleyen güncelleştirme dağıtımlarını düzenleyin ve ardından doğru değeri yansıtacak şekilde bunları kaldırın ve yeniden ekleyin `SourceComputerId` .

## <a name="scenario-updates-are-installed-without-a-deployment"></a><a name="updates-nodeployment"></a>Senaryo: güncelleştirmeler dağıtım olmadan yüklenir

### <a name="issue"></a>Sorun

Bir Windows makinesini Güncelleştirme Yönetimi kaydettiğinizde, bir dağıtım olmadan yüklenen güncelleştirmeleri görürsünüz.

### <a name="cause"></a>Nedeni

Windows 'da, güncelleştirmeler kullanılabilir duruma geldiğinde otomatik olarak yüklenir. Bu davranış, makineye dağıtılacak bir güncelleştirme zamanalmadıysanız karışıklığına neden olabilir.

### <a name="resolution"></a>Çözüm

`HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU`Kayıt defteri anahtarı varsayılan olarak 4: `auto download and install` .

Güncelleştirme Yönetimi istemciler için, bu anahtarı 3: olarak ayarlamayı öneririz `auto download but do not auto install` .

Daha fazla bilgi için bkz. [otomatik güncelleştirmeleri yapılandırma](https://docs.microsoft.com/windows/deployment/update/waas-wu-settings#configure-automatic-updates).

## <a name="scenario-machine-is-already-registered-to-a-different-account"></a><a name="machine-already-registered"></a>Senaryo: makine farklı bir hesaba zaten kayıtlı

### <a name="issue"></a>Sorun

Aşağıdaki hata iletisini alırsınız:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.InvalidOperationException: {"Message":"Machine is already registered to a different account."}
```

### <a name="cause"></a>Nedeni

Makine Güncelleştirme Yönetimi için zaten başka bir çalışma alanına dağıtıldı.

### <a name="resolution"></a>Çözüm

1. Makinenin doğru çalışma alanına rapor olduğundan emin olmak için, makineler bölümündeki adımları izleyin [güncelleştirme yönetimi altında portalda görünmez](#nologs) .
2. [Karma runbook grubunu silerek](../automation-windows-hrw-install.md#remove-a-hybrid-worker-group)makinedeki yapıtları temizleyin ve sonra yeniden deneyin.

## <a name="scenario-machine-cant-communicate-with-the-service"></a><a name="machine-unable-to-communicate"></a>Senaryo: makine hizmetle iletişim kuramıyor

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

```error
Access is denied. (Exception form HRESULT: 0x80070005(E_ACCESSDENIED))
```

### <a name="cause"></a>Nedeni

Bir proxy, ağ geçidi veya güvenlik duvarı ağ iletişimini engelliyor olabilir. 

### <a name="resolution"></a>Çözüm

Ağınızı gözden geçirin ve uygun bağlantı noktalarına ve adreslere izin verildiğinden emin olun. Güncelleştirme Yönetimi ve karma runbook çalışanları için gereken bağlantı noktalarının ve adreslerin listesi için bkz. [ağ gereksinimleri](../automation-hybrid-runbook-worker.md#network-planning) .

## <a name="scenario-unable-to-create-self-signed-certificate"></a><a name="unable-to-create-selfsigned-cert"></a>Senaryo: otomatik olarak imzalanan sertifika oluşturulamıyor

### <a name="issue"></a>Sorun

Aşağıdaki hata iletilerinden birini alırsınız:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception AgentService.HybridRegistration. PowerShell.Certificates.CertificateCreationException: Failed to create a self-signed certificate. ---> System.UnauthorizedAccessException: Access is denied.
```

### <a name="cause"></a>Nedeni

Karma Runbook Worker otomatik olarak imzalanan bir sertifika oluşturamadı.

### <a name="resolution"></a>Çözüm

Sistem hesabının **C:\programdata\microsoft\crypto\rsa** klasörüne okuma erişimi olduğunu doğrulayın ve yeniden deneyin.

## <a name="scenario-the-scheduled-update-failed-with-a-maintenancewindowexceeded-error"></a><a name="mw-exceeded"></a>Senaryo: zamanlanan güncelleştirme bir MaintenanceWindowExceeded hatasıyla başarısız oldu

### <a name="issue"></a>Sorun

Güncelleştirmeler için varsayılan bakım penceresi 120 dakikadır. Bakım penceresini en fazla 6 saat veya 360 dakika olarak artırabilirsiniz.

### <a name="resolution"></a>Çözüm

Başarısız olan zamanlanmış güncelleştirme dağıtımlarını düzenleyin ve bakım penceresini arttırın.

Bakım pencereleri hakkında daha fazla bilgi için bkz. [Install Updates](../automation-tutorial-update-management.md#schedule-an-update-deployment).

## <a name="scenario-machine-shows-as-not-assessed-and-shows-an-hresult-exception"></a><a name="hresult"></a>Senaryo: makine "değerlendirilmedi" olarak gösterilir ve bir HRESULT özel durumu gösterir

### <a name="issue"></a>Sorun

* Uyumluluk altında olarak görünen makineleriniz `Not assessed` vardır **Compliance**ve bunların altında bir özel durum iletisi görürsünüz.
* Portalda bir HRESULT hata kodu görürsünüz.

### <a name="cause"></a>Nedeni

Güncelleştirme Aracısı (Windows üzerindeki Windows Update Aracısı; bir Linux dağıtımı için Paket Yöneticisi) doğru yapılandırılmamış. Güncelleştirme Yönetimi, gereken güncelleştirmeleri, düzeltme ekinin durumunu ve dağıtılan düzeltme eklerinin sonuçlarını sağlamak için makinenin güncelleştirme aracısına bağımlıdır. Bu bilgiler olmadan, Güncelleştirme Yönetimi gereken veya yüklenen düzeltme eklerine düzgün şekilde bildiremiyor.

### <a name="resolution"></a>Çözüm

Güncelleştirmeleri makinede yerel olarak gerçekleştirmeyi deneyin. Bu işlem başarısız olursa, genellikle bir güncelleştirme Aracısı yapılandırma hatası olduğu anlamına gelir.

Bu sorun genellikle ağ yapılandırması ve güvenlik duvarı sorunlarından kaynaklanır. Sorunu gidermek için aşağıdaki denetimleri kullanın.

* Linux için, paket deponuzdaki ağ uç noktasına ulaşabildiğinizden emin olmak için uygun belgeleri denetleyin.

* Windows için, [güncelleştirmeler intranet uç noktasından (WSUS/SCCM) indirmeyen güncelleştirmeler](/windows/deployment/update/windows-update-troubleshooting#updates-arent-downloading-from-the-intranet-endpoint-wsussccm)bölümünde listelenen aracı yapılandırmanızı denetleyin.

  * Makineler Windows Update için yapılandırılmışsa, [http/proxy ile Ilgili sorunlar](/windows/deployment/update/windows-update-troubleshooting#issues-related-to-httpproxy)bölümünde açıklanan uç noktalara ulaşabildiğinizden emin olun.
  * Makineler Windows Server Update Services (WSUS) için yapılandırılmışsa, [WUServer kayıt defteri anahtarı](/windows/deployment/update/waas-wu-settings)tarafından yapılandırılan WSUS sunucusuna erişebildiğinizden emin olun.

Bir HRESULT görürseniz, tüm özel durum iletisini görmek için kırmızı renkte görüntülenecek özel duruma çift tıklayın. Olası çözümler veya önerilen eylemler için aşağıdaki tabloyu gözden geçirin.

|Özel durum  |Çözüm veya eylem  |
|---------|---------|
|`Exception from HRESULT: 0x……C`     | Özel durumun nedeni hakkında ek ayrıntılar bulmak için [Windows Update hata kodu listesinde](https://support.microsoft.com/help/938205/windows-update-error-code-list) ilgili hata kodunu arayın.        |
|`0x8024402C`</br>`0x8024401C`</br>`0x8024402F`      | Bunlar ağ bağlantısı sorunlarını gösterir. Makinenizin Güncelleştirme Yönetimi için ağ bağlantısı olduğundan emin olun. Gerekli bağlantı noktalarının ve adreslerin listesi için [ağ planlama](../automation-update-management.md#ports) bölümüne bakın.        |
|`0x8024001E`| Hizmet veya sistem kapatıldığından güncelleştirme işlemi tamamlanmadı.|
|`0x8024002E`| Windows Update hizmeti devre dışı bırakıldı.|
|`0x8024402C`     | Bir WSUS sunucusu kullanıyorsanız, kayıt defteri anahtarı için kayıt defteri değerlerinin `WUServer` `WUStatusServer` `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate` doğru WSUS sunucusunu belirtdiğinizden emin olun.        |
|`0x80072EE2`|Bir ağ bağlantısı sorunu veya yapılandırılmış bir WSUS sunucusuyla iletişim kurulurken bir sorun var. WSUS ayarlarını denetleyin ve hizmetin istemciden erişilebilir olduğundan emin olun.|
|`The service cannot be started, either because it is disabled or because it has no enabled devices associated with it. (Exception from HRESULT: 0x80070422)`     | Windows Update hizmeti 'nin (wuauserv) çalıştığından ve devre dışı bırakılmadığından emin olun.        |
|`0x80070005`| Bir erişim reddedildi hatası, aşağıdakilerden biri nedeniyle oluşabilir:<br> Virüslü bilgisayar<br> Windows Update ayarları doğru yapılandırılmadı<br> %WinDir%\SoftwareDistribution klasörüyle dosya izni hatası<br> Sistem sürücüsünde yetersiz disk alanı (C:).
|Diğer tüm genel durumlar     | Olası çözümler için internet 'te bir arama çalıştırın ve yerel BT desteğiniz ile çalışın.         |

**%Windir%\WindowsUpdate.log** dosyasını gözden geçirmek olası nedenleri belirlemenize de yardımcı olabilir. Günlüğü okuma hakkında daha fazla bilgi için bkz. [windowsupdate. log dosyasını okuma](https://support.microsoft.com/help/902093/how-to-read-the-windowsupdate-log-file).

Ayrıca, makinede Windows Update sorunları denetlemek için [Windows Update sorun gidericiyi](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) indirebilir ve çalıştırabilirsiniz.

> [!NOTE]
> [Windows Update sorun giderici](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) belgeleri Windows istemcilerinde kullanılması gerektiğini gösterir, ancak Windows Server 'da da kullanılabilir.

## <a name="scenario-update-run-returns-failed-status-linux"></a>Senaryo: güncelleştirme çalıştırması geri dönüş başarısız durumu (Linux)

### <a name="issue"></a>Sorun

Bir güncelleştirme çalıştırması başlatılır, ancak çalıştırma sırasında hatalarla karşılaşır.

### <a name="cause"></a>Nedeni

Olası nedenler:

* Paket Yöneticisi sağlam değil.
* Güncelleştirme Aracısı (Windows için WUA, Linux için özel paket yöneticisi) yanlış yapılandırılmış.
* Belirli paketler, bulut tabanlı düzeltme eki uygulama ile müdahale ediyor.
* Makineye ulaşılamıyor.
* Güncelleştirmeler çözümlenmeyen bağımlılıklara sahipti.

### <a name="resolution"></a>Çözüm

Bir güncelleştirme çalıştırması sırasında başarılı olduktan sonra bir başarısızlık oluşursa, çalıştırmada etkilenen makinenin [iş çıktısını kontrol](../manage-update-multi.md#view-results-of-an-update-deployment) edin. Çözebileceğinizi ve üzerinde işlem yapmanız gereken makinelerinizden belirli hata iletileri bulabilirsiniz. Güncelleştirme Yönetimi, paket yöneticisinin başarılı güncelleştirme dağıtımları için sağlıklı olmasını gerektirir.

İş başarısız olmadan hemen önce belirli düzeltme ekleri, paketler veya güncelleştirmeler görülemezse, bu öğelerin bir sonraki güncelleştirme dağıtımından [dışlanmasını](../automation-tutorial-update-management.md#schedule-an-update-deployment) deneyebilirsiniz. Windows Update günlük bilgilerini toplamak için bkz. [Windows Update günlük dosyaları](/windows/deployment/update/windows-update-logs).

Düzeltme eki uygulama sorununu çözemezseniz, **/var/opt/Microsoft/omsagent/Run/automationworker/omsupdatemgmt.log** dosyasının bir kopyasını oluşturun ve bir sonraki güncelleştirme dağıtımı başlamadan önce sorun giderme amacıyla koruyun.

## <a name="patches-arent-installed"></a>Düzeltme ekleri yüklenmedi

### <a name="machines-dont-install-updates"></a>Makineler güncelleştirmeleri yüklemiyor

Güncelleştirmeleri doğrudan makinede çalıştırmayı deneyin. Makine güncelleştirmeleri uygulayamıyorum, [sorun giderme kılavuzunda olası hataların listesine](https://docs.microsoft.com/azure/automation/troubleshoot/update-management#hresult)bakın.

Güncelleştirmeler yerel olarak çalışıyorsa, [GÜNCELLEŞTIRME YÖNETIMI VM 'Yi kaldırma](https://docs.microsoft.com/azure/automation/automation-onboard-solutions-from-browse#clean-up-resources)konusunda yer alan kılavuzu izleyerek aracıyı kaldırıp yeniden yüklemeyi deneyin.

### <a name="i-know-updates-are-available-but-they-dont-show-as-available-on-my-machines"></a>Güncelleştirmelerin kullanılabildiğini biliyorum, ancak makinelerimde kullanılabilir olarak gösterilmiyorum

Bu durum genellikle makineler WSUS veya Microsoft uç nokta Configuration Manager güncelleştirmeleri almak üzere yapılandırılmışsa, ancak WSUS ve Configuration Manager güncelleştirmeleri onaylanmamış olur.

`UseWUServer` [Bu makalenin kayıt defterini düzenleyerek otomatik güncelleştirmeleri yapılandırma bölümünde](https://support.microsoft.com/help/328010/how-to-configure-automatic-updates-by-using-group-policy-or-registry-s)kayıt defteri ANAHTARLARıNA, MAKINELERIN WSUS ve SCCM için yapılandırılıp yapılandırılmadığını kontrol edebilirsiniz.

Güncelleştirmeler WSUS 'de onaylanmamışsa, bunlar yüklenmez. Aşağıdaki sorguyu çalıştırarak Log Analytics onaylanmamış güncelleştirmeleri kontrol edebilirsiniz.

  ```kusto
  Update | where UpdateState == "Needed" and ApprovalSource == "WSUS" and Approved == "False" | summarize max(TimeGenerated) by Computer, KBID, Title
  ```

### <a name="updates-show-as-installed-but-i-cant-find-them-on-my-machine"></a>Güncelleştirmeler yüklenmiş görünüyor ancak makinemde bulamıyorum

Güncelleştirmeler genellikle başka güncelleştirmelerle değiştirilir. Daha fazla bilgi için, Windows Update sorun giderme kılavuzunda [güncelleştirme 'nin yerini almıştır](https://docs.microsoft.com/windows/deployment/update/windows-update-troubleshooting#the-update-is-not-applicable-to-your-computer) .

### <a name="installing-updates-by-classification-on-linux"></a>Linux'da güncelleştirmeleri sınıflandırmaya göre yükleme

Güncelleştirmeleri Linux'a sınıflandırmaya göre ("Kritik ve güvenlik güncelleştirmeleri") yüklemek, özellikle CentOS için önemli uyarılara tabidir. Bu sınırlamalar [güncelleştirme yönetimi genel bakış sayfasında](https://docs.microsoft.com/azure/automation/automation-update-management#linux-2)belgelenmiştir.

### <a name="kb2267602-is-consistently-missing"></a>KB2267602, sürekli olarak eksik

KB2267602, [Windows Defender tanım güncelleştirmesidir](https://www.microsoft.com/wdsi/definitions). Günlük olarak güncelleştirilir.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemezseniz, ek destek için aşağıdaki kanallardan birini deneyin.

* Azure [forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıtlar alın.
* [@AzureSupport](https://twitter.com/azuresupport)Müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabıyla bağlanın.
* Azure destek olayı dosyası oluşturma. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**' ı seçin.
