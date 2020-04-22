---
title: Sorun Giderme Azure Otomasyon Güncelleme Yönetimi
description: Azure Otomasyonu'nda Güncelleştirme Yönetimi çözümüyle ilgili sorunları nasıl gidereceğinizi ve gidereceğinizi öğrenin.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 03/17/2020
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 91ecff311b8820d3b97e1de0e4b4e87c150e749b
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678931"
---
# <a name="troubleshoot-issues-with-the-update-management-solution"></a>Güncelleştirme Yönetimi çözümüyle ilgili sorun giderme sorunları

Bu makalede, Güncelleştirme Yönetimi çözümlerini kullanırken rastlayabildiğiniz sorunlar tartışılmaktadır. Altta yatan sorunu belirlemek için Karma Runbook Worker aracısı için bir aracı sorun giderici vardır. Sorun giderici hakkında daha fazla bilgi edinmek için [Sorun Giderme Windows update aracısı sorunları](update-agent-issues.md) ve [Sorun Giderme Linux güncelleştirme aracısı sorunları.](update-agent-issues-linux.md) Diğer onboarding sorunları için, [onboarding Sorun Giderme çözümüne](onboarding.md)bakın.

>[!NOTE]
>Çözümü sanal bir makinede (VM) uçağa binerken sorun bulursanız, yerel makinedeki **Uygulama ve Hizmetler Günlükleri** altındaki **Operasyon Yöneticisi** günlüğünü kontrol edin. Olay kimliği 4502 ve olay ayrıntıları `Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent`içeren olaylar arayın.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](../automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="scenario-you-receive-the-error-failed-to-enable-the-update-solution"></a>Senaryo: "Güncelleştirme çözümlerini etkinleştirmek için başarısız oldu" hatasını alırsınız

### <a name="issue"></a>Sorun

Otomasyon hesabınızda Güncelleştirme Yönetimi çözümünüzü etkinleştirmeye çalıştığınızda aşağıdaki hatayı alırsınız:

```error
Error details: Failed to enable the Update solution
```

### <a name="cause"></a>Nedeni

Bu hata aşağıdaki nedenlerle oluşabilir:

* Log Analytics aracısı için ağ güvenlik duvarı gereksinimleri doğru şekilde yapılandırılamayabilir. Bu durum, DNS URL'lerini çözerken aracının başarısız lığa neden olabilir.

* Çözüm hedeflemesi yanlış yapılandırılmıştır ve makine beklendiği gibi güncelleştirmeler almıyor.

* Ayrıca makinenin `Non-compliant` **Uyumluluk**altında bir durum gösterdiğini de fark edebilirsiniz. Aynı **zamanda, Agent Desktop Analytics** aracıyı `Disconnected`.

### <a name="resolution"></a>Çözüm

* İşletim sistemi bağlı olarak [Windows](update-agent-issues.md#troubleshoot-offline) veya [Linux](update-agent-issues-linux.md#troubleshoot-offline)için sorun giderici çalıştırın.

* Güncelleştirme Yönetimi'nin çalışması için hangi adreslerin ve bağlantı noktalarının izin verilmesi gerektiğini öğrenmek için [Ağ planlamasına](../automation-hybrid-runbook-worker.md#network-planning) gidin.  

* Log Analytics aracısının çalışması için hangi adreslerin ve bağlantı noktalarının izin verilmesi gerektiğini öğrenmek için [Ağ planlamasına](../../azure-monitor/platform/log-analytics-agent.md#network-requirements) gidin.

* Kapsam yapılandırma sorunları olup yok. [Kapsam yapılandırması,](../automation-onboard-solutions-from-automation-account.md#scope-configuration) çözüm için hangi makinelerin yapılandırılmalarını belirler. Makineniz çalışma alanınızda görünüyorsa ancak **Update Management Portal'da görünmüyorsa, kapsamı yapılandırmasını makineleri hedeflemek için ayarlamanız gerekir. Kapsam yapılandırması hakkında bilgi edinmek için [çalışma alanında Yerleşik makinelere](../automation-onboard-solutions-from-automation-account.md#onboard-machines-in-the-workspace)bakın.

* [Karma runbook çalışmasını silme](../automation-hybrid-runbook-worker.md#remove-a-hybrid-runbook-worker)adımlarını izleyerek alt yapılandırmayı kaldırın. 

## <a name="scenario-superseded-update-indicated-as-missing-in-update-management"></a>Senaryo: Güncelleştirme Yönetimi'nde eksik olarak gösterilen yeni güncelleştirme

### <a name="issue"></a>Sorun

Eski güncelleştirmeler, bir Otomasyon hesabı için, geçersiz hale getirilmiş olsalar bile eksik olarak görünür. Aynı güvenlik açığını düzelten daha sonraki bir güncelleştirme kullanılabilir olduğundan, yerini alan güncelleştirme yüklemek zorunda olmadığınız bir güncelleştirmedir. Update Management, geçersiz kılan güncelleştirmeyi yok sayar ve yerini alan güncelleştirme lehine geçerli değildir. İlgili bir sorun hakkında bilgi için [bkz.](https://docs.microsoft.com/windows/deployment/update/windows-update-troubleshooting#the-update-is-not-applicable-to-your-computer)

### <a name="cause"></a>Nedeni

Değiştirilen güncelleştirmeler, geçerli olmadığı düşünülebilmeleri için reddedilmiş olarak doğru şekilde belirtilmez.

### <a name="resolution"></a>Çözüm

Yerine verilen bir güncelleştirme yüzde 100 geçerli değilse, bu güncelleştirmenin onay `Declined`durumunu ' da değiştirmeniz gerekir. Tüm güncelleştirmeleriniz için onay durumunu değiştirmek için:

1. Otomasyon hesabında, makine durumunu görüntülemek için **Yönetim Güncelleştir'i** seçin. Bkz. [Güncelleştirme değerlendirmelerini görüntüleyin.](../manage-update-multi.md#view-an-update-assessment)

2. Yüzde 100 geçerli olmadığından emin olmak için yenilen güncelleştirmeyi denetleyin. 

3. Güncelleştirme yle ilgili bir sorunuz yoksa güncelleştirmeyi reddedildi olarak işaretleyin. 

4. **Bilgisayarlar'ı** seçin ve **Uyumluluk** sütununda uyumluluk için bir rescan zorlayın. Bkz. [Birden çok makine için güncelleştirmeleri yönet.](../manage-update-multi.md)

5. Değiştirilen diğer güncelleştirmeler için yukarıdaki adımları yineleyin.

6. Reddedilen güncelleştirmelerden dosyaları silmek için temizleme sihirbazını çalıştırın. 

7. Windows Server Update Services (WSUS) için, altyapıyı yenilemek için değiştirilen tüm güncelleştirmeleri el ile temizleyin.

8. Görüntü sorununu düzeltmek ve güncelleştirme yönetimi için kullanılan disk alanı miktarını en aza indirmek için bu yordamı düzenli olarak yineleyin.

## <a name="scenario-machines-dont-show-up-in-the-portal-under-update-management"></a><a name="nologs"></a>Senaryo: Makineler Güncelleme Yönetimi altında portalda görünmüyor

### <a name="issue"></a>Sorun

Makineleriniz aşağıdaki belirtilere sahiptir:

* Makineniz `Not configured` bir VM'nin Güncelleştirme Yönetimi görünümünden gösterir.

* Azure Otomasyon hesabınızın Güncelleme Yönetimi görünümünde makineleriniz eksiktir.

* `Not assessed` **Uyumluluk**altında olduğunu gösteren makineleriniz var. Ancak, Azure Monitor günlüklerinde Karma Runbook Worker için sinyal verilerini görürsünüz, ancak Güncelleştirme Yönetimi için görmezsiniz.

### <a name="cause"></a>Nedeni

Bu sorun, yerel yapılandırma sorunları ndan veya yanlış yapılandırılmış kapsam yapılandırmasından kaynaklanabilir. Olası özel nedenler şunlardır:

* Karma Runbook Worker'ı yeniden kaydetmeniz ve yeniden yüklemeniz gerekebilir.

* Çalışma alanınızda ulaşılan ve daha fazla veri depolamasını engelleyen bir kota tanımlamış olabilirsiniz.

### <a name="resolution"></a>Çözüm

1. İşletim sistemi bağlı olarak [Windows](update-agent-issues.md#troubleshoot-offline) veya [Linux](update-agent-issues-linux.md#troubleshoot-offline)için sorun giderici çalıştırın.

2. Makinenizin doğru çalışma alanına rapor verdiğinden emin olun. Bu yönü nasıl doğrulayabilirsiniz hakkında kılavuz için [bkz.](../../azure-monitor/platform/agent-windows.md#verify-agent-connectivity-to-log-analytics) Ayrıca, bu çalışma alanının Azure Otomasyon hesabınıza bağlı olduğundan emin olun. Onaylamak için Otomasyon hesabınıza gidin ve **İlgili Kaynaklar**altında Bağlantılı **çalışma alanını** seçin.

3. Otomasyon hesabınıza bağlı Log Analytics çalışma alanında makinelerin ortaya çıktıklarından emin olun. Log Analytics çalışma alanında aşağıdaki sorguyu çalıştırın.

   ```kusto
   Heartbeat
   | summarize by Computer, Solutions
   ```

4. Makinenizi sorgu sonuçlarında görmüyorsanız, yakın zamanda iade edilmemiş. Büyük olasılıkla yerel bir yapılandırma sorunu vardır ve [aracıyı yeniden yüklemeniz](../../azure-monitor/learn/quick-collect-windows-computer.md#install-the-agent-for-windows)gerekir. 

5. Makineniz sorgu sonuçlarında ortaya çıkıyorsa, kapsam yapılandırma sorunları olup olmadığını denetleyin. [Kapsam yapılandırması,](../automation-onboard-solutions-from-automation-account.md#scope-configuration) çözüm için hangi makinelerin yapılandırıldırıldırış olduğunu belirler. 

6. Makineniz çalışma alanınızda görünüyorsa ancak Güncelleştirme Yönetimi'nde görünmüyorsa, kapsamı yapılandırmasını makineyi hedeflemesi için yapılandırmanız gerekir. Bunu nasıl yapacağınızı öğrenmek için [çalışma alanında yerleşik makinelere](../automation-onboard-solutions-from-automation-account.md#onboard-machines-in-the-workspace)bakın.

7. Çalışma alanınızda bu sorguyunuzu çalıştırın.

   ```kusto
   Operation
   | where OperationCategory == 'Data Collection Status'
   | sort by TimeGenerated desc
   ```

8. Bir `Data collection stopped due to daily limit of free data reached. Ingestion status = OverQuota` sonuç alırsanız, çalışma alanınızda tanımlanan kotaya ulaşıldı ve bu da verilerin kaydedilmesine engel oldu. Çalışma alanınızda, Kullanım altında **veri hacmi yönetimine** gidin ve tahmini **maliyetler**ve kotayı değiştirin veya kaldırın.

9. Sorununuz hala çözülmemişse, Windows için Karma Çalışan'ı yeniden yüklemek için [Windows Karma Runbook Worker'ı dağıt'taki](../automation-windows-hrw-install.md) adımları izleyin. Linux için, bir [Linux Hybrid Runbook Worker dağıt'taki](../automation-linux-hrw-install.md)adımları izleyin.

## <a name="scenario-unable-to-register-automation-resource-provider-for-subscriptions"></a><a name="rp-register"></a>Senaryo: Abonelikler için Otomasyon kaynak sağlayıcısı nı kaydedemiyoruz

### <a name="issue"></a>Sorun

Otomasyon hesabınızdaki çözümlerle çalışırken aşağıdaki hata oluşur:

```error
Error details: Unable to register Automation Resource Provider for subscriptions
```

### <a name="cause"></a>Nedeni

Otomasyon kaynak sağlayıcısı aboneye kayıtlı değildir.

### <a name="resolution"></a>Çözüm

Otomasyon kaynak sağlayıcısını kaydetmek için Azure portalında aşağıdaki adımları izleyin.

1. Portalın altındaki Azure hizmet listesinde Tüm **hizmetler'i**seçin ve ardından Genel hizmet grubunda **Abonelikleri** seçin.

2. Aboneliğinizi seçin.

3. **Ayarlar** **altında, Kaynak Sağlayıcıları**seçin.

4. Kaynak sağlayıcıları listesinden, Microsoft.Automation kaynak sağlayıcısının kayıtlı olduğunu doğrulayın.

5. Listede yoksa, kaynak sağlayıcı kaydı yla ilgili hataları gider'deki adımları izleyerek Microsoft.Automation [sağlayıcısını kaydedin.](/azure/azure-resource-manager/resource-manager-register-provider-errors)

## <a name="scenario-scheduled-update-with-a-dynamic-schedule-missed-some-machines"></a><a name="scheduled-update-missed-machines"></a>Senaryo: Dinamik bir zamanlama ile zamanlanmış güncelleştirme bazı makineleri cevapsız

### <a name="issue"></a>Sorun

Güncelleştirme önizlemesinde yer alan makinelerin tümü zamanlanan bir çalışma sırasında yamalı makineler listesinde görünmez.

### <a name="cause"></a>Nedeni

Bu sorun aşağıdaki nedenlerden biri olabilir:

* Dinamik bir sorguda kapsamda tanımlanan abonelikler kayıtlı Otomasyon kaynak sağlayıcısı için yapılandırılmamıştır.

* Zamanlama yürütüldüğünde makineler kullanılamadı veya uygun etiketlere sahip değildi.

### <a name="resolution"></a>Çözüm

#### <a name="subscriptions-not-configured-for-registered-automation-resource-provider"></a>Kayıtlı Otomasyon kaynak sağlayıcısı için yapılandırılmamış abonelikler

Aboneliğiniz Otomasyon kaynak sağlayıcısı için yapılandırılmamışsa, bu abonelikteki makinelerhakkında bilgi sorgulayamaz veya getiremezsiniz. Abonelik kaydını doğrulamak için aşağıdaki adımları kullanın.

1. Azure [portalında](https://docs.microsoft.com/azure/azure-resource-manager/management/resource-providers-and-types#azure-portal)Azure hizmet listesine erişin.

2. **Tüm hizmetleri**seçin ve ardından Genel hizmet grubunda **Abonelikleri** seçin. 

3. Dağıtımınızın kapsamında tanımlanan aboneliği bulun.

4. **Ayarlar**altında, **Kaynak Sağlayıcıları**seçin.

5. Microsoft.Automation kaynak sağlayıcısının kayıtlı olduğunu doğrulayın.

6. Listede yoksa, kaynak sağlayıcı kaydı yla ilgili hataları gider'deki adımları izleyerek Microsoft.Automation [sağlayıcısını kaydedin.](/azure/azure-resource-manager/resource-manager-register-provider-errors)

#### <a name="machines-not-available-or-not-tagged-correctly-when-schedule-executed"></a>Zamanlama yürütüldüğünde kullanılamayan veya doğru etiketlenmemiş makineler

Aboneliğiniz Otomasyon kaynak sağlayıcısı için yapılandırıldıysa, ancak güncelleştirme zamanlamasını belirtilen [dinamik gruplarla](../automation-update-management-groups.md) çalıştıran bazı makineleri kaçırdıysa aşağıdaki yordamı kullanın.

1. Azure portalında Otomasyon hesabını açın ve **Yönetim Güncelleştir'i**seçin.

2. Güncelleştirme dağıtımının tam olarak ne zaman çalıştırıldığını belirlemek için [Güncelleştirme Yönetimi geçmişini](https://docs.microsoft.com/azure/automation/manage-update-multi#view-results-of-an-update-deployment) denetleyin. 

3. Güncelleştirme Yönetimi tarafından gözden kaçırıldığınızdan şüphelendiğiniz makineler [için, makine değişikliklerini bulmak](https://docs.microsoft.com/azure/governance/resource-graph/how-to/get-resource-changes#find-detected-change-events-and-view-change-details)için Azure Kaynak Grafiği'ni (ARG) kullanın. 

4. Güncelleştirme dağıtımı çalıştırıldıktan önceki bir gün gibi önemli bir dönemde değişiklik arayın.

5. Bu dönemde makinelerde silme veya güncelleştirme değişiklikleri gibi sistemik değişiklikler için arama sonuçlarını denetleyin. Bu değişiklikler, güncelleştirmeler dağıtıldığında makinelerin makine listesinde seçilmemelerini sağlayacak şekilde makine durumunu veya etiketleri değiştirebilir.

6. Makine durumunu veya etiket sorunlarını düzeltmek için makineleri ve kaynak ayarlarını gerektiği gibi ayarlayın.

7. Belirtilen dinamik gruplarla dağıtımın tüm makineleri içerdiğinden emin olmak için güncelleştirme zamanlamasını yeniden çalıştırın.

## <a name="scenario-expected-machines-dont-appear-in-preview-for-dynamic-group"></a><a name="machines-not-in-preview"></a>Senaryo: Beklenen makineler dinamik grup için önizlemede görünmez

### <a name="issue"></a>Sorun

Dinamik bir grubun seçili kapsamları için sanal makineler Azure portalı önizleme listesinde görünmüyor. Bu liste, seçili kapsamlar için bir ARG sorgusu tarafından alınan tüm makinelerden oluşur. Kapsamlar, Karma Runbook İşçileri yüklü olan ve erişim izinlerine sahip olduğunuz makineler için filtrelenir. 

### <a name="cause"></a>Nedeni
 
Bu sorunun olası nedenleri şunlardır:

* Seçili kapsamlarda doğru erişime sahip değilsiniz.
* ARG sorgusu beklenen makineleri almaz.
* Karma Runbook Worker makinelere yüklü değildir.

### <a name="resolution"></a>Çözüm 

#### <a name="incorrect-access-on-selected-scopes"></a>Seçili kapsamlarda yanlış erişim

Azure portalı yalnızca belirli bir kapsamda yazma erişiminiz olan makineleri görüntüler. Kapsam için doğru erişime sahip değilseniz, [Bkz. Öğretici: RBAC ve Azure portalını kullanarak kullanıcıya Azure kaynaklarına erişim hakkı](https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal)tanıyın.

#### <a name="arg-query-doesnt-return-expected-machines"></a>ARG sorgusu beklenen makineleri döndürmez

Sorgularınızın doğru çalışıp çalışmaz olduğunu öğrenmek için aşağıdaki adımları izleyin.

1. Azure portalındaki Kaynak Grafiği gezgini bıçağında aşağıda gösterildiği gibi biçimlendirilmiş bir ARG sorgusu çalıştırın. Bu sorgu, Güncelleştirme Yönetimi'nde dinamik grubu oluşturduğunuzda seçtiğiniz filtreleri taklit eder. Bkz. [Güncelleştirme Yönetimi ile dinamik grupları kullan.](https://docs.microsoft.com/azure/automation/automation-update-management-groups) 

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
 
2. Aradığınız makinelerin sorgu sonuçlarında listeli olup olmadığını denetleyin. 

3. Makineler listelenmemişse, dinamik grupta seçilen filtreyle ilgili bir sorun olabilir. Grup yapılandırmasını gerektiği gibi ayarlayın.

#### <a name="hybrid-runbook-worker-not-installed-on-machines"></a>Karma Runbook İşçi makinelere yüklü değil

Makineler ARG sorgu sonuçlarında görünür, ancak dinamik grup önizlemesinde yine de görünmez. Bu durumda, makineler karma çalışanlar olarak atanmayabilir ve bu nedenle Azure Otomasyon ve Güncelleştirme Yönetimi işlerini çalıştıramaz. Görmeyi beklediğiniz makinelerin Hibrit Runbook İşçileri olarak ayarlandığından emin olmak için:

1. Azure portalında, doğru görünmeyen bir makine için Otomasyon hesabına gidin.

2. **Proses Otomasyonu**altında **Karma işçi gruplarını** seçin.

3. Sistem **karma işçi grupları** sekmesini seçin.

4. Hibrit işçinin o makine için mevcut olduğunu doğrulayın.

5. Makine karma bir çalışan olarak ayarlanmadıysa, Karma [Runbook Worker'ı kullanarak veri merkezinizdeki veya bulutunuzdaki kaynakları Otomatikleştir'deki](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker)yönergeleri kullanarak ayarlamalar yapın.

6. Makineyi Karma Runbook Worker grubuna katılın.

7. Önizlemede görüntülenmiyor tüm makineler için yukarıdaki adımları yineleyin.

## <a name="scenario-components-for-update-management-solution-enabled-while-vm-continues-to-show-as-being-configured"></a><a name="components-enabled-not-working"></a>Senaryo: Güncelleştirme Yönetimi çözümü için bileşenler etkinken, VM yapılandırıldığını göstermeye devam ediyor

### <a name="issue"></a>Sorun

Uçağa bindikten 15 dakika sonra sanal bir makinede aşağıdaki iletiyi görmeye devam emzebilirsiniz:

```error
The components for the 'Update Management' solution have been enabled, and now this virtual machine is being configured. Please be patient, as this can sometimes take up to 15 minutes.
```

### <a name="cause"></a>Nedeni

Bu hata aşağıdaki nedenlerle oluşabilir:

* Otomasyon hesabı ile iletişim engelleniyor.

* Farklı kaynak bilgisayar tanıları olan yinelenen bir bilgisayar adı vardır. Bu senaryo, belirli bir bilgisayar ada sahip bir VM farklı kaynak gruplarında oluşturulduğunda ve abonelikte aynı Lojistik Aracısı çalışma alanına raporlandığında oluşur.

* Gemiye binen VM görüntüsü, Windows için Log Analytics aracısının yüklü olduğu Sistem Hazırlama (sysprep) ile hazırlanmamış klonlanmış bir makineden gelebilir.

### <a name="resolution"></a>Çözüm

VM ile ilgili sorunun tam olarak belirlenmesine yardımcı olmak için, Otomasyon hesabınıza bağlı Log Analytics çalışma alanında aşağıdaki sorguyu çalıştırın.

```
Update
| where Computer contains "fillInMachineName"
| project TimeGenerated, Computer, SourceComputerId, Title, UpdateState 
```

#### <a name="communication-with-automation-account-blocked"></a>Otomasyon hesabı ile iletişim engellendi

Güncelleştirme Yönetimi'nin çalışması için hangi adreslerin ve bağlantı noktalarının izin verilmesi gerektiğini öğrenmek için [Ağ planlamasına](../automation-update-management.md#ports) gidin.

#### <a name="duplicate-computer-name"></a>Yinelenen bilgisayar adı

Çevrelerinde benzersiz adlar sağlamak için VM'lerinizi yeniden adlandırın.

#### <a name="onboarded-image-from-cloned-machine"></a>Klonlanmış makineden yerleşik görüntü

Klonlanmış bir resim kullanıyorsanız, farklı bilgisayar adları aynı kaynak bilgisayar kimliğine sahiptir. Bu durumda:

1. Log Analytics çalışma alanınızda, gösterilirse kapsam yapılandırması için kaydedilen aramadan `MicrosoftDefaultScopeConfig-Updates` VM'yi kaldırın. Kaydedilen aramalar çalışma alanınızda **Genel** altında bulunabilir.

2. Aşağıdaki cmdlet çalıştırın.

    ```azurepowershell-interactive
    Remove-Item -Path "HKLM:\software\microsoft\hybridrunbookworker" -Recurse -Force
    ```

3. Sağlık `Restart-Service HealthService` hizmetini yeniden başlatmak için çalıştırın. Bu işlem anahtarı yeniden oluşturur ve yeni bir UUID oluşturur.

4. Bu yaklaşım işe yaramazsa, önce görüntüüzerinde Sysprep çalıştırın ve sonra MMA'yı yükleyin.

## <a name="scenario-you-receive-a-linked-subscription-error-when-you-create-an-update-deployment-for-machines-in-another-azure-tenant"></a><a name="multi-tenant"></a>Senaryo: Başka bir Azure kiracısında makineler için güncelleştirme dağıtımı oluşturduğunuzda bağlantılı bir abonelik hatası alırsınız

### <a name="issue"></a>Sorun

Başka bir Azure kiracısında ki makineler için güncelleştirme dağıtımı oluşturmaya çalıştığınızda aşağıdaki hatayla karşılaşırsınız:

```error
The client has permission to perform action 'Microsoft.Compute/virtualMachines/write' on scope '/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Automation/automationAccounts/automationAccountName/softwareUpdateConfigurations/updateDeploymentName', however the current tenant '00000000-0000-0000-0000-000000000000' is not authorized to access linked subscription '00000000-0000-0000-0000-000000000000'.
```

### <a name="cause"></a>Nedeni

Bu hata, güncelleştirme dağıtımına dahil olan başka bir kiracıda Azure VM'leri bulunan bir güncelleştirme dağıtımı oluşturduğunuzda oluşur.

### <a name="resolution"></a>Çözüm

Bu öğeleri zamanlanmış almak için aşağıdaki geçici çözüm kullanın. Bir zamanlama oluşturmak için `ForUpdateConfiguration` parametre ile [Yeni-AzAutomationSchedule](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationschedule?view=azps-3.7.0) cmdlet kullanabilirsiniz. Daha sonra, [New-AzAutomationSoftwareUpdateConfiguration](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationSoftwareUpdateConfiguration?view=azps-3.7.0) cmdlet kullanın ve `NonAzureComputer` parametre için diğer kiracı makineleri geçmek. Aşağıdaki örnekte, bunun nasıl yapılacağını gösterilmektedir:

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$s = New-AzAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdateConfiguration

New-AzAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName $aa -Schedule $s -Windows -AzureVMResourceId $azureVMIdsW -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

## <a name="scenario-unexplained-reboots"></a><a name="node-reboots"></a>Senaryo: Açıklanamayan yeniden başlatmalar

### <a name="issue"></a>Sorun

**Yeniden Başlatma Denetim** seçeneğini Never **Reboot**olarak ayarlasanız bile, güncelleştirmeler yüklendikten sonra makineler yeniden başlatılır.

### <a name="cause"></a>Nedeni

Windows Update, yeniden başlatma davranışını değiştirebilen birkaç kayıt defteri anahtarı yla değiştirilebilir.

### <a name="resolution"></a>Çözüm

Makinelerinizin düzgün şekilde yapılandırıldığından emin olmak [için yeniden başlatmayı yönetmek için kullanılan](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) kayıt defteri ve Kayıt Defteri anahtarlarını [düzenleyerek Otomatik Güncelleştirmeleri Yapılandırma](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry) altında listelenen kayıt defteri anahtarlarını gözden geçirin.

## <a name="scenario-machine-shows-failed-to-start-in-an-update-deployment"></a><a name="failed-to-start"></a>Senaryo: Makine, güncelleştirme dağıtımında "Başlatılamayan"

### <a name="issue"></a>Sorun

Bir makine `Failed to start` durumu gösterir. Makinenin belirli ayrıntılarını görüntülediğinizde, aşağıdaki hatayı görürsünüz:

```error
Failed to start the runbook. Check the parameters passed. RunbookName Patch-MicrosoftOMSComputer. Exception You have requested to create a runbook job on a hybrid worker group that does not exist.
```

### <a name="cause"></a>Nedeni

Bu hatanın oluşmasının nedeni aşağıdakilerden biri olabilir:

* Makine artık yok.
* Makine kapalı ve ulaşılamıyor.
* Makinenin bir ağ bağlantısı sorunu vardır ve bu nedenle makinedeki hibrit işçiye erişilemez.
* MMA'da kaynak bilgisayar kimliğini değiştiren bir güncelleştirme vardı.
* Bir Otomasyon hesabında 2000 eşzamanlı iş sınırına ulaştığınızda güncelleştirme çalıştırmanız kısıtlandı. Her dağıtım bir iş olarak kabul edilir ve güncelleştirme dağıtımındaki her makine bir iş olarak sayılır. Şu anda Otomasyon hesabınızda çalışan diğer otomasyon işi veya güncelleştirme dağıtımı eşzamanlı iş sınırına doğru sayılır.

### <a name="resolution"></a>Çözüm

Mümkün olduğunda, güncelleştirme dağıtımlarınız için [dinamik gruplar](../automation-update-management-groups.md) kullanın. Ayrıca, aşağıdaki adımları atabilirsiniz.

1. Makinenin hala var olduğunu ve erişilebildiğini doğrulayın. 
2. Makine yoksa, dağıtımınızı edin ve makineyi kaldırın.
3. Güncelleştirme Yönetimi için gerekli olan bağlantı noktaları ve adreslerin listesi için [ağ planlama](../automation-update-management.md#ports) bölümüne bakın ve ardından makinenizin bu gereksinimleri karşıladığını doğrulayın.
4. Karma Runbook İşçisi aracısı sorun gidericisini kullanarak Karma Runbook İşçisi ile bağlantıyı doğrulayın. Sorun giderici hakkında daha fazla bilgi edinmek için [Sorun Giderme güncelleştirme aracısı sorunlarına](update-agent-issues.md)bakın.
5. Kaynak bilgisayar kimliğinin değiştiği ortamınızda makineleri bulmak için Log Analytics'te aşağıdaki sorguyu çalıştırın. Aynı değere, ancak `Computer` farklı `SourceComputerId` bir değere sahip bilgisayarları arayın.

   ```kusto
   Heartbeat | where TimeGenerated > ago(30d) | distinct SourceComputerId, Computer, ComputerIP
   ```

6. Etkilenen makineleri bulduktan sonra, bu makineleri hedefleyen güncelleştirme dağıtımlarını düzeltin ve doğru `SourceComputerId` değeri yansıtacak şekilde kaldırın ve okuyun.

## <a name="scenario-updates-are-installed-without-a-deployment"></a><a name="updates-nodeployment"></a>Senaryo: Güncelleştirmeler dağıtım yapılmadan yüklenir

### <a name="issue"></a>Sorun

Güncelleştirme Yönetimi'ne bir Windows makinesi kaydettiğinizde, güncelleştirmeler dağıtım olmadan yüklenmiş görürsünüz.

### <a name="cause"></a>Nedeni

Windows'da güncelleştirmeler kullanılabilir olur olmaz otomatik olarak yüklenir. Makineye dağıtılmak üzere bir güncelleştirme zamanlamadıysanız, bu davranış karışıklığa neden olabilir.

### <a name="resolution"></a>Çözüm

Kayıt `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU` defteri anahtarı varsayılan olarak 4 `auto download and install`ayarına ayarlanır: .

Güncelleştirme Yönetimi istemcileri için bu anahtarı `auto download but do not auto install`3 olarak ayarlamanızı öneririz: .

Daha fazla bilgi için [bkz.](https://docs.microsoft.com/windows/deployment/update/waas-wu-settings#configure-automatic-updates)

## <a name="scenario-machine-is-already-registered-to-a-different-account"></a><a name="machine-already-registered"></a>Senaryo: Makine zaten farklı bir hesaba kayıtlı

### <a name="issue"></a>Sorun

Aşağıdaki hata iletisini alırsınız:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.InvalidOperationException: {"Message":"Machine is already registered to a different account."}
```

### <a name="cause"></a>Nedeni

Makine güncelleştirme yönetimi için başka bir çalışma alanına zaten binilmiştir.

### <a name="resolution"></a>Çözüm

1. Makinelerin doğru çalışma alanına rapor bildirdiğinden emin olmak için [Güncelleştirme Yönetimi altındaki portalda](#nologs) Makineler altındaki adımları izleyin.
2. [Karma runbook grubunu silerek](../automation-hybrid-runbook-worker.md#remove-a-hybrid-worker-group)makinedeki yapıları temizleyin ve sonra yeniden deneyin.

## <a name="scenario-machine-cant-communicate-with-the-service"></a><a name="machine-unable-to-communicate"></a>Senaryo: Makine hizmetle iletişim kuramıyor

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

Proxy, ağ geçidi veya güvenlik duvarı ağ iletişimini engelliyor olabilir. 

### <a name="resolution"></a>Çözüm

Ağınızı gözden geçirin ve uygun bağlantı noktalarına ve adreslerine izin verildiğinden emin olun. Güncelleştirme Yönetimi ve Karma Runbook Çalışanları tarafından gerekli olan bağlantı noktaları ve adreslerin listesi için [ağ gereksinimlerine](../automation-hybrid-runbook-worker.md#network-planning) bakın.

## <a name="scenario-unable-to-create-self-signed-certificate"></a><a name="unable-to-create-selfsigned-cert"></a>Senaryo: Kendi imzalı sertifika oluşturulamıyor

### <a name="issue"></a>Sorun

Aşağıdaki hata iletilerinden birini alırsınız:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception AgentService.HybridRegistration. PowerShell.Certificates.CertificateCreationException: Failed to create a self-signed certificate. ---> System.UnauthorizedAccessException: Access is denied.
```

### <a name="cause"></a>Nedeni

Karma Runbook Worker kendi imzalı bir sertifika oluşturamadı.

### <a name="resolution"></a>Çözüm

Sistem hesabının **C:\ProgramData\Microsoft\Crypto\RSA** klasörüne erişimi okuduğunu doğrulayın ve yeniden deneyin.

## <a name="scenario-the-scheduled-update-failed-with-a-maintenancewindowexceeded-error"></a><a name="mw-exceeded"></a>Senaryo: Zamanlanan güncelleştirme MaintenanceWindowExceeded hatası ile başarısız oldu

### <a name="issue"></a>Sorun

Güncelleştirmeler için varsayılan bakım penceresi 120 dakikadır. Bakım penceresini en fazla 6 saat veya 360 dakikaya yükseltebilirsiniz.

### <a name="resolution"></a>Çözüm

Başarısız zamanlanmış güncelleştirme dağıtımlarını edin ve bakım penceresini artırın.

Bakım pencereleri hakkında daha fazla bilgi için [güncelleştirmeleri yükle'ye](../automation-tutorial-update-management.md#schedule-an-update-deployment)bakın.

## <a name="scenario-machine-shows-as-not-assessed-and-shows-an-hresult-exception"></a><a name="hresult"></a>Senaryo: Makine "Değerlendirilmemiştir" olarak gösterir ve bir HRESULT özel durumu gösterir

### <a name="issue"></a>Sorun

* `Not assessed` **Uyumluluk**altında olarak gösteren makineleriniz vardır ve bunların altında bir özel durum iletisi görürsünüz.
* Portalda bir HRESULT hata kodu görürsünüz.

### <a name="cause"></a>Nedeni

Güncelleştirme Aracısı (Windows'da Windows Update Agent; Linux dağıtımı için paket yöneticisi) doğru şekilde yapılandırılmamıştır. Güncelleştirme Yönetimi, gerekli güncelleştirmeleri, düzeltme ekinin durumunu ve dağıtılan düzeltme emültlerinin sonuçlarını sağlamak için makinenin Güncelleştirme Aracısına güvenir. Bu bilgiler olmadan, Güncelleştirme Yönetimi gereken veya yüklenen düzeltme etekleri düzgün bir şekilde bildiremez.

### <a name="resolution"></a>Çözüm

Güncelleştirmeleri makinede yerel olarak gerçekleştirmeye çalışın. Bu işlem başarısız olursa, genellikle bir güncelleştirme aracısı yapılandırma hatası olduğu anlamına gelir.

Bu sorun genellikle ağ yapılandırması ve güvenlik duvarı sorunları neden olur. Sorunu düzeltmek için aşağıdaki denetimleri kullanın.

* Linux için, paket deponuzun ağ bitiş noktasına ulaşabildiğinizden emin olmak için uygun belgeleri kontrol edin.

* Windows için, güncelleştirmelerde listelenen aracı yapılandırmanızı [intranet bitiş noktasından (WSUS/SCCM) indirilmeyerek](/windows/deployment/update/windows-update-troubleshooting#updates-arent-downloading-from-the-intranet-endpoint-wsussccm)kontrol edin.

  * Makineler Windows Update için yapılandırıldıysa, [HTTP/proxy ile ilgili sorunlarda](/windows/deployment/update/windows-update-troubleshooting#issues-related-to-httpproxy)açıklanan uç noktalara ulaşabileceğinizden emin olun.
  * Makineler Windows Server Update Services (WSUS) için yapılandırıldıysa, [WUServer kayıt defteri anahtarı](/windows/deployment/update/waas-wu-settings)tarafından yapılandırılan WSUS sunucusuna erişebildiğinizden emin olun.

Bir HRESULT görürseniz, özel durum iletisinin tamamını görmek için kırmızı renkte görüntülenen özel durumu çift tıklatın. Olası çözümler veya önerilen eylemler için aşağıdaki tabloyu gözden geçirin.

|Özel durum  |Çözüm veya eylem  |
|---------|---------|
|`Exception from HRESULT: 0x……C`     | Özel durum nedeni hakkında ek ayrıntılar bulmak için [Windows güncelleştirme hata kodu listesinde](https://support.microsoft.com/help/938205/windows-update-error-code-list) ilgili hata kodu arama.        |
|`0x8024402C`</br>`0x8024401C`</br>`0x8024402F`      | Bunlar ağ bağlantısı sorunlarını gösterir. Makinenizin Güncelleştirme Yönetimi'ne ağ bağlantısı olduğundan emin olun. Gerekli bağlantı noktaları ve adreslerin listesi için [ağ planlama](../automation-update-management.md#ports) bölümüne bakın.        |
|`0x8024001E`| Hizmet veya sistem kapandığı için güncelleştirme işlemi tamamlanmadı.|
|`0x8024002E`| Windows Update hizmeti devre dışı bırakıldı.|
|`0x8024402C`     | Bir WSUS sunucusu kullanıyorsanız, `WUServer` `WUStatusServer` `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate` kayıt defteri değerlerinin kayıt defteri için ve altında doğru WSUS sunucusunu belirttiğinden emin olun.        |
|`0x80072EE2`|Yapılandırılmış bir WSUS sunucusuyla konuşurken bir ağ bağlantısı sorunu veya bir sorun var. WSUS ayarlarını kontrol edin ve hizmete istemciden erişilebildiğinden emin olun.|
|`The service cannot be started, either because it is disabled or because it has no enabled devices associated with it. (Exception from HRESULT: 0x80070422)`     | Windows Update hizmetinin (wuauserv) çalışıyor ve devre dışı bırakılmadığından emin olun.        |
|`0x80070005`| Erişim reddedilen bir hata aşağıdakilerden herhangi biri tarafından neden olabilir:<br> Virüslü bilgisayar<br> Windows Update ayarları doğru yapılandırılmamıştır<br> %WinDir%\SoftwareDistribution klasörü ile dosya izni hatası<br> Sistem sürücüsünde yetersiz disk alanı (C:).
|Diğer genel özel durum     | Olası çözümler için internette bir arama yapın ve yerel BT desteğinizle çalışın.         |

**%Windir%\Windowsupdate.log** dosyasını gözden geçirmek de olası nedenleri belirlemenize yardımcı olabilir. Günlüğü nasıl okuyabilirsiniz hakkında daha fazla bilgi için [Windowsupdate.log dosyasının nasıl okunduğuna](https://support.microsoft.com/help/902093/how-to-read-the-windowsupdate-log-file)bakın.

Ayrıca, makinedeki Windows Update ile ilgili sorunları kontrol etmek için [Windows Update sorun gidericisini](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) indirip çalıştırabilirsiniz.

> [!NOTE]
> [Windows Update sorun giderici](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) belgeleri, windows istemcilerinde kullanım için olduğunu gösterir, ancak Windows Server'da da çalışır.

## <a name="scenario-update-run-returns-failed-status-linux"></a>Senaryo: Güncelleştirme çalıştır Başarısız durum (Linux) döndürür

### <a name="issue"></a>Sorun

Güncelleştirme çalışması başlar, ancak çalışma sırasında hatalarla karşılaşır.

### <a name="cause"></a>Nedeni

Olası nedenler:

* Paket yöneticisi sağlıksız.
* Update Agent (Windows için WUA, Linux için dağıtıma özel paket yöneticisi) yanlış yapılandırılmıştır.
* Belirli paketler bulut tabanlı yama ile müdahale ediyor.
* Makineye ulaşılamıyor.
* Güncelleştirmelerin çözülmemiş bağımlılıkları vardı.

### <a name="resolution"></a>Çözüm

Başarılı bir şekilde başladıktan sonra bir güncelleştirme çalışması sırasında hatalar oluşursa, çalışan daki etkilenen makineden [iş çıktısını denetleyin.](../manage-update-multi.md#view-results-of-an-update-deployment) Makinelerinizden araştırma yapabileceğiniz ve harekete geçebileceğiniz belirli hata iletileri bulabilirsiniz. Güncelleştirme Yönetimi, başarılı güncelleştirme dağıtımları için paket yöneticisinin sağlıklı olmasını gerektirir.

Belirli düzeltme eki, paket veya güncelleştirme, iş başarısız olmadan hemen önce görülürse, bu öğeleri bir sonraki güncelleştirme dağıtımından [hariç taslamayı](../automation-tutorial-update-management.md#schedule-an-update-deployment) deneyebilirsiniz. Windows Update'ten günlük bilgilerini toplamak için [Windows Update günlük dosyalarına](/windows/deployment/update/windows-update-logs)bakın.

Bir düzeltme sorununu çözemediyseniz, **/var/opt/microsoft/omsagent/run/automationworker/omsupdatemgmt.log** dosyasının bir kopyasını yapın ve bir sonraki güncelleştirme dağıtımı başlamadan önce sorun giderme amacıyla saklayın.

## <a name="patches-arent-installed"></a>Düzeltme emaları yüklü değil

### <a name="machines-dont-install-updates"></a>Makineler güncelleştirmeleri yüklemiyor

Güncelleştirmeleri doğrudan makinede çalıştırmayı deneyin. Makine güncelleştirmeleri uygulayamazsa, sorun [giderme kılavuzundaki olası hatalar listesine başvurun.](https://docs.microsoft.com/azure/automation/troubleshoot/update-management#hresult)

Güncelleştirmeler yerel olarak çalışıyorsa, [Güncelleştirme Yönetimi'nden Bir VM Kaldır'daki](https://docs.microsoft.com/azure/automation/automation-onboard-solutions-from-browse#clean-up-resources)kılavuzu izleyerek aracıyı kaldırmayı ve makineye yeniden yüklemeyi deneyin.

### <a name="i-know-updates-are-available-but-they-dont-show-as-available-on-my-machines"></a>Güncellemelerin mevcut olduğunu biliyorum, ancak makinelerimde mevcut olarak görünmüyorlar

Bu genellikle, makineler WSUS veya Microsoft Endpoint Configuration Manager'dan güncelleştirmeler alacak şekilde yapılandırılırsa, ancak WSUS ve Configuration Manager güncelleştirmeleri onaylamadıysa sık sık gerçekleşir.

[Bu makalenin Kayıt Defteri bölümünü düzenleyerek](https://support.microsoft.com/help/328010/how-to-configure-automatic-updates-by-using-group-policy-or-registry-s)Otomatik Güncelleştirmeleri Yapılandırma'daki `UseWUServer` kayıt defteri anahtarına kayıt defteri anahtarını çapraz yönlendirerek makinelerin WSUS ve SCCM için yapılandırılıp yapılıp yapılamayacağınızı kontrol edebilirsiniz.

Güncelleştirmeler WSUS'ta onaylanmadıysa, yüklenmez. Log Analytics'te onaylanmamış güncelleştirmeleri aşağıdaki sorguyu çalıştırarak denetleyebilirsiniz.

  ```kusto
  Update | where UpdateState == "Needed" and ApprovalSource == "WSUS" and Approved == "False" | summarize max(TimeGenerated) by Computer, KBID, Title
  ```

### <a name="updates-show-as-installed-but-i-cant-find-them-on-my-machine"></a>Güncelleştirmeler yüklenmiş görünüyor ancak makinemde bulamıyorum

Güncelleştirmeler genellikle başka güncelleştirmelerle değiştirilir. Daha fazla bilgi için bkz. Windows Update Sorun Giderme kılavuzunda [Güncelleştirme'nin yerini alır.](https://docs.microsoft.com/windows/deployment/update/windows-update-troubleshooting#the-update-is-not-applicable-to-your-computer)

### <a name="installing-updates-by-classification-on-linux"></a>Linux'da güncelleştirmeleri sınıflandırmaya göre yükleme

Güncelleştirmeleri Linux'a sınıflandırmaya göre ("Kritik ve güvenlik güncelleştirmeleri") yüklemek, özellikle CentOS için önemli uyarılara tabidir. Bu [sınırlamalar, Güncelleştirme Yönetimi genel bakış sayfasında](https://docs.microsoft.com/azure/automation/automation-update-management#linux-2)belgelenmiştir.

### <a name="kb2267602-is-consistently-missing"></a>KB2267602 sürekli eksik

KB2267602, [Windows Defender tanım güncelleştirmesidir](https://www.microsoft.com/wdsi/definitions). Her gün güncellenir.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemiyorsanız, ek destek için aşağıdaki kanallardan birini deneyin.

* [Azure Forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıt alın.
* [@AzureSupport](https://twitter.com/azuresupport)Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın.
* Azure destek olayı dosyala. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.
