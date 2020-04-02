---
title: Mesai dışı çözüm sırasında VM'leri Başlat/Durdur
description: Bu VM yönetimi çözümü, Azure sanal makinelerinizi bir zamanlamayla başlatır ve durdurur ve Azure Monitor günlüklerini proaktif olarak izler.
services: automation
ms.subservice: process-automation
ms.date: 04/01/2020
ms.topic: conceptual
ms.openlocfilehash: cef3176c99cd57ae229b602feb3c825081fcfe3e
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80548366"
---
# <a name="startstop-vms-during-off-hours-solution-in-azure-automation"></a>Azure Otomasyonu'nda VM’leri çalışma saatleri dışında Başlatma/Durdurma çözümü

Mesai dışı çözüm başlangıcında Başlat/Durdur VM'leri, Azure sanal makinelerinizi kullanıcı tanımlı zamanlamalarda durdurur, Azure Monitor günlükleri aracılığıyla öngörüler sağlar ve [eylem gruplarını](../azure-monitor/platform/action-groups.md)kullanarak isteğe bağlı e-postalar gönderir. Çoğu senaryo için hem Azure Kaynak Yöneticisi'ni hem de klasik VM'leri destekler. 

Bu çözümü Klasik VM'lerle kullanmak için varsayılan olarak oluşturulmayan bir Klasik RunAs hesabına ihtiyacınız vardır. Klasik RunAs hesabı oluşturma yönergeleri [için, Klasik Run-As Hesapları'na](automation-create-standalone-account.md#classic-run-as-accounts)bakın.

> [!NOTE]
> Çalışma saatleri dışında başlat/durdur çözümü, kullanılabilir Azure modüllerinin en yeni sürümlerini destekleyecek şekilde güncelleştirildi.

Bu çözüm, VM maliyetlerini optimize etmek isteyen kullanıcılar için merkezi olmayan düşük maliyetli bir otomasyon seçeneği sağlar. Bu çözümle şunları yapabilirsiniz:

- [Başlatmak ve durdurmak için VM'leri zamanlayın.](automation-solution-vm-management-config.md#schedule)
- [Azure Etiketleri kullanarak](automation-solution-vm-management-config.md#tags) (klasik VM'ler için desteklenmeyen) vm'leri artan sırada başlatmak ve durdurmak için zamanlama.
- [Düşük CPU kullanımına](automation-solution-vm-management-config.md#cpuutil)dayalı Otomatik Durdurma VM'leri.

Geçerli çözümle ilgili sınırlamalar şunlardır:

- Bu çözüm, herhangi bir bölgedeki VM'leri yönetir, ancak yalnızca Azure Otomasyon hesabınızla aynı abonelikte kullanılabilir.
- Bu çözüm, Bir Günlük Analizi çalışma alanını, Azure Otomasyon hesabını ve Uyarıları destekleyen tüm bölgeler için Azure ve Azure Kamu'da kullanılabilir. Azure Genel bölgeleri şu anda e-posta işlevselliğini desteklemiyor.

> [!NOTE]
> Çözümü klasik VM'ler için kullanıyorsanız, tüm VM'leriniz bulut hizmeti başına sırayla işlenir. Sanal makineler hala farklı bulut hizmetleri arasında paralel olarak işlenir. Bulut hizmeti başına 20'den fazla VM'niz varsa, üst çalışma kitabı **ScheduledStartStop_Parent** birden çok zamanlama oluşturmanızı ve zamanlama başına 20 VM belirtmenizi öneririz. Zamanlama özelliklerinde, virgülle ayrılmış bir liste olarak belirtin, **VMList** parametresinde VM adları. Aksi takdirde, bu çözüm için Otomasyon işi üç saatten fazla çalışırsa, geçici olarak boşaltılır veya [adil paylaşım](automation-runbook-execution.md#fair-share) limitine göre durdurulur.
>
> Azure Bulut Çözüm Sağlayıcısı (Azure CSP) abonelikleri yalnızca Azure Kaynak Yöneticisi modelini destekler, Programda Azure Kaynak Yöneticisi olmayan hizmetler kullanılamaz. Başlat/Durdur çözümü çalıştığında, klasik kaynakları yönetmek için cmdlets olduğu gibi hatalar alabilirsiniz. CSP hakkında daha fazla bilgi edinmek için [CSP aboneliklerinde kullanılabilir hizmetlere](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services)bakın. Bir CSP aboneliği kullanıyorsanız, [**dağıtımdan**](#variables) sonra External_EnableClassicVMs değişkenini **False** olarak değiştirmelisiniz.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu çözümün runbook'ları bir [Azure Run As hesabıyla](automation-create-runas-account.md)çalışır. Run As hesabı tercih edilen kimlik doğrulama yöntemidir, çünkü süresi dolan veya sık sık değişebilecek bir parola yerine sertifika kimlik doğrulaması kullanır.

Başlat/Durdur VM çözümü için ayrı bir Otomasyon Hesabı kullanmanızı öneririz. Bunun nedeni, Azure modül sürümlerinin sık sık yükseltilmesi ve parametrelerinin değişebileceğidir. Başlat/Durdur VM çözümü aynı cadence üzerinde yükseltilmez, bu nedenle kullandığı cmdletlerin yeni sürümleriyle çalışmayabilir. Ayrıca, modül güncellemelerini üretim Otomasyon Hesabınıza (lar) aktarmadan önce bir test Otomasyon Uyrması Hesabında test etmenizi öneririz.

### <a name="permissions"></a>İzinler

Bir kullanıcının mesai saatleri dışında Başlat/Durdur VM'lerini dağıtması gereken bazı izinler vardır. Önceden oluşturulmuş bir Otomasyon Hesabı ve Log Analytics çalışma alanı kullanıyorsanız veya dağıtım sırasında yenilerini oluştururken bu izinler farklıdır. Azure Active Directory kiracınızda abonelikte Katkıda Bulunan ve Global Yöneticiyseniz, aşağıdaki izinleri yapılandırmanız gerekmez. Bu haklara sahip değilseniz veya özel bir rolü yapılandırmanız gerekiyorsa, aşağıda gerekli izinlere bakın.

#### <a name="pre-existing-automation-account-and-log-analytics-workspace"></a>Önceden varolan Otomasyon Hesabı ve Log Analytics çalışma alanı

Çalışma saatleri dışında Başlat/Durdur VM'lerini mevcut bir Otomasyon Hesabı ve Log Analytics çalışma alanına dağıtmak için, çözümü dağıtan kullanıcı **Kaynak Grubu'nda**aşağıdaki izinleri gerektirir. Roller hakkında daha fazla bilgi edinmek [için Azure kaynakları için Özel rolleri](../role-based-access-control/custom-roles.md)görün.

| İzin | Kapsam|
| --- | --- |
| Microsoft.Automation/automationAccounts/read | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/variables/write | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/schedules/write | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/runbooks/write | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/connections/write | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/certificates/write | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/modules/write | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/modules/read | Kaynak Grubu |
| Microsoft.automation/automationAccounts/jobSchedules/write | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/jobs/write | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/jobs/read | Kaynak Grubu |
| Microsoft.OperationsManagement/solutions/write | Kaynak Grubu |
| Microsoft.OperationalInsights/çalışma alanları/* | Kaynak Grubu |
| Microsoft.Insights/diagnosticSettings/write | Kaynak Grubu |
| Microsoft.Insights/ActionGroups/Write | Kaynak Grubu |
| Microsoft.Insights/ActionGroups/read | Kaynak Grubu |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak Grubu |
| Microsoft.Resources/deployments/* | Kaynak Grubu |

#### <a name="new-automation-account-and-a-new-log-analytics-workspace"></a>Yeni Otomasyon Hesabı ve yeni Log Analytics çalışma alanı

Mesai saatleri dışında Başlat/Durdur VM'lerini yeni bir Otomasyon Hesabı ve Log Analytics çalışma alanına dağıtmak için, çözümü dağıtan kullanıcının önceki bölümde tanımlanan izinlerin yanı sıra aşağıdaki izinlere ihtiyacı vardır:

- Abonelikte yardımcı yönetici - Yalnızca Klasik VM'leri yönetecekseniz, Klasik Çalıştır Hesabı'nı oluşturmak gerekir. [Klasik RunAs Hesapları](automation-create-standalone-account.md#classic-run-as-accounts) artık varsayılan olarak oluşturulmaz.
- Azure Active [Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md) **Application Developer** rolünün bir üyesi. Hesap Olarak Çalıştır'ı yapılandırma hakkında daha fazla bilgi [için, Çalıştır'ı hesaplar olarak yapılandırmak için İzinler'e](manage-runas-account.md#permissions)bakın.
- Aboneye veya aşağıdaki izinlere katkıda bulunan kişi.

| İzin |Kapsam|
| --- | --- |
| Microsoft.Authorization/Operations/read | Abonelik|
| Microsoft.Authorization/permissions/read |Abonelik|
| Microsoft.Authorization/roleAssignments/read | Abonelik |
| Microsoft.Authorization/roleAssignments/write | Abonelik |
| Microsoft.Authorization/roleAssignments/delete | Abonelik |
| Microsoft.Automation/automationAccounts/connections/read | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/certificates/read | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/write | Kaynak Grubu |
| Microsoft.OperationalInsights/çalışma alanları/yazma | Kaynak Grubu |

## <a name="solution-components"></a>Çözüm bileşenleri

Bu çözüm, sanal makinelerinizin başlangıç ve kapatma işlemlerini işletme gereksinimlerinize göre uyarlayabilmeniz için önceden yapılandırılmış runbook'lar, zamanlamalar ve Azure Monitor günlükleriyle tümleştirmeyi içerir.

### <a name="runbooks"></a>Runbook'lar

Aşağıdaki tabloda, bu çözüme göre Otomasyon hesabınıza dağıtılan runbook'lar listelenir. Runbook kodunda değişiklik yapmayın. Bunun yerine, yeni işlevler için kendi runbook yazın.

> [!IMPORTANT]
> Doğrudan adına eklenen *alt* kitap çalıştırmayın.

Tüm üst runbook'lar _WhatIf_ parametresini içerir. **True**olarak ayarlandığında _WhatIf,_ _WhatIf_ parametresi olmadan çalıştırıldığında runbook'un tam olarak aldığı davranışı niçin ayrıntılı olarak destekler ve doğru VM'lerin hedeflendiğini doğrular. Runbook yalnızca _WhatIf_ parametresi **False**olarak ayarlandığında tanımlanan eylemlerini gerçekleştirir.

|Runbook | Parametreler | Açıklama|
| --- | --- | ---|
|AutoStop_CreateAlert_Child | VMObject <br> AlertAction <br> WebHookURI | Üst çalışma kitabından çağrılır. Bu runbook, Otomatik Durdurma senaryosu için kaynak başına uyarılar oluşturur.|
|AutoStop_CreateAlert_Parent | VMList<br> WhatIf: Doğru veya Yanlış  | Hedeflenen abonelik veya kaynak gruplarında VM'lerde Azure uyarı kuralları oluşturur veya güncelleştirir. <br> VMList: Virgülden ayrılmış VM listesi. Örneğin, _vm1, vm2, vm3_.<br> *WhatIf* yürütmeden runbook mantığını doğrular.|
|AutoStop_Disable | yok | Otomatik Stop uyarılarını ve varsayılan zamanlamayı devre dışı bırakır.|
|AutoStop_VM_Child | WebHookData | Üst çalışma kitabından çağrılır. Uyarı kuralları klasik VM'yi durdurmak için bu runbook'u çağırır.|
|AutoStop_VM_Child_ARM | WebHookData |Üst çalışma kitabından çağrılır. Uyarı kuralları VM'yi durdurmak için bu runbook'u çağırır.  |
|ScheduledStartStop_Base_Classic | CloudServiceName<br> Eylem: Başlat veya Durdur<br> VMList  | Bu runbook Bulut Hizmetleri tarafından klasik VM grubunda eylem başlatma veya durdurmak gerçekleştirmek için kullanılır.<br> VMList: Virgülden ayrılmış VM listesi. Örneğin, _vm1, vm2, vm3_. |
|ScheduledStartStop_Child | VMName <br> Eylem: Başlat veya Durdur <br> ResourceGroupName | Üst çalışma kitabından çağrılır. Zamanlanan durak için bir başlatma veya durdurma eylemini yürütür.|
|ScheduledStartStop_Child_Classic | VMName<br> Eylem: Başlat veya Durdur<br> ResourceGroupName | Üst çalışma kitabından çağrılır. Klasik VM'ler için zamanlanan durak için bir başlatma veya durdurma eylemi yürütür. |
|ScheduledStartStop_Parent | Eylem: Başlat veya Durdur <br>VMList <br> WhatIf: Doğru veya Yanlış | Bu ayar, abonelikteki tüm VM'leri etkiler. Yalnızca bu hedeflenen kaynak gruplarında yürütmek için **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupNames'yi** edin. **External_ExcludeVMNames** değişkenini güncelleyerek belirli VM'leri de dışlayabilirsiniz.<br> VMList: Virgülden ayrılmış VM listesi. Örneğin, _vm1, vm2, vm3_.<br> _WhatIf_ yürütmeden runbook mantığını doğrular.|
|SequencedStartStop_Parent | Eylem: Başlat veya Durdur <br> WhatIf: Doğru veya Yanlış<br>VMList| Başlat/durdurma etkinliğini sıralamak istediğiniz her VM'de **sequencestart** ve **sequencestop** adlı etiketler oluşturun. Bu etiket adları büyük/küçük harf duyarlıdır. Etiketin değeri, başlamak veya durdurmak istediğiniz sıraya karşılık gelen pozitif bir tamsayı (1, 2, 3) olmalıdır. <br> VMList: Virgülden ayrılmış VM listesi. Örneğin, _vm1, vm2, vm3_. <br> _WhatIf_ yürütmeden runbook mantığını doğrular. <br> **Not**: VM'ler Azure Otomasyon değişkenlerinde External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames ve External_ExcludeVMNames olarak tanımlanan kaynak gruplarında olmalıdır. Eylemlerin etkili olması için uygun etiketlere sahip olmaları gerekir.|

### <a name="variables"></a>Değişkenler

Aşağıdaki tabloda Otomasyon hesabınızda oluşturulan değişkenler listeleneb.r.a. Yalnızca **Harici**ile önceden belirlenmiş değişkenleri değiştirin. **Internal** ile önceden belirlenmiş değişkenleri değiştirmek istenmeyen etkilere neden olur.

|Değişken | Açıklama|
|---------|------------|
|External_AutoStop_Condition | Bir uyarıtetiklemeden önce koşulu yapılandırmak için gereken koşullu işleç. Kabul edilebilir değerler **GreaterThan**, **GreaterThanOrEqual**, **LessThan**, ve **LessThanOrEqual**.|
|External_AutoStop_Description | CPU yüzdesi eşiği aşarsa VM durdurmak için uyarı.|
|External_AutoStop_Frequency | Kural için değerlendirme frekansı. Bu parametre, girişin zaman alanı biçiminde kabul edilir. Olası değerler 5 dakika ile 6 saat arasındadır. |
|External_AutoStop_MetricName | Azure Uyarı kuralının yapılandırılması gereken performans ölçümünün adı.|
|External_AutoStop_Severity | 0 ile 4 arasında değişen metrik uyarının önem derecesi. |
|External_AutoStop_Threshold | External_AutoStop_MetricName değişkeninde belirtilen Azure Uyarısı _External_AutoStop_MetricName_kuralıiçin eşik. Yüzde değerleri 1 ile 100 arasında değişebilir.|
|External_AutoStop_TimeAggregationOperator | Durumu değerlendirmek için seçili pencere boyutuna uygulanan zaman toplama işleci. Kabul edilebilir değerler **Ortalama,** **Minimum,** **Maksimum,** **Toplam**ve **Son**' dır.|
|External_AutoStop_TimeWindow | Azure'un bir uyarıyı tetiklemek için seçili ölçümleri analiz ettiği pencere boyutu. Bu parametre, girişin zaman alanı biçiminde kabul edilir. Olası değerler 5 dakika ile 6 saat arasındadır.|
|External_EnableClassicVMs| Klasik VM'lerin çözüm tarafından hedeflenip hedeflenmediğini belirtir. Varsayılan değer True'dur. Bu, CSP abonelikleri için False olarak ayarlanmalıdır. Klasik VM'ler [klasik Run-As Hesabı](automation-create-standalone-account.md#classic-run-as-accounts)gerektirir.|
|External_ExcludeVMNames | Boşluk olmayan bir virgül kullanarak adları ayırarak dışlanacak VM adlarını girin. Bu 140 VM ile sınırlıdır. Bu virgülle ayrılmış listeye 140'tan fazla VM eklerseniz, hariç tutulacak şekilde ayarlanan VM'ler yanlışlıkla başlatılabilir veya durdurulabilir.|
|External_Start_ResourceGroupNames | Başlangıç eylemleri için hedeflenen virgül kullanarak değerleri ayıran bir veya daha fazla kaynak grubu belirtir.|
|External_Stop_ResourceGroupNames | Durdurma eylemleri için hedeflenen virgül kullanarak değerleri ayıran bir veya daha fazla kaynak grubu belirtir.|
|External_WaitTimeForVMRetrySeconds |Sıralı başlat/durdur runbook için VM'lerde yapılacak eylemlerin saniye cinsinden bekleme süresi.<br> Varsayılan değer 2100 saniyedir ve en yüksek 10800 veya üç saatlik bir değere yapılandırmayı destekler.|
|Internal_AutomationAccountName | Otomasyon hesabının adını belirtir.|
|Internal_AutoSnooze_ARM_WebhookURI | Webhook URI'nin klasik VM'ler için AutoStop senaryosu için çağrıda bulunmasını belirtir.|
|Internal_AutoSnooze_WebhookUri | AutoStop senaryosu için çağrılan Webhook URI'yi belirtir.|
|Internal_AzureSubscriptionId | Azure Abonelik Kimliğini belirtir.|
|Internal_ResourceGroupName | Otomasyon hesabı kaynak grup adını belirtir.|

>[!NOTE]
>Değişken **External_WaitTimeForVMRetryInSeconds**için varsayılan değer 600'den 2100'e güncelleştirildi. Bu değişken, **Sıralı başlat/durdur senaryosu** runbook sonraki eylem ile devam etmeden önce belirli sayıda saniye için alt işlemleri beklemek için izin verir.
>

Tüm senaryolarda, **AutoStop_CreateAlert_Parent,** **SequencedStartStop_Parent**ve **AutoStop_CreateAlert_Parent** **ScheduledStartStop_Parent** runbook'lar için virgülle ayrılmış vm listesi sağlama dışında, vm'leri hedeflemek için External_Start_ResourceGroupNames, **External_Stop_ResourceGroupNames**ve **External_ExcludeVMNames** değişkenleri gereklidir. Diğer bir deyişle, eylemin gerçekleşmesi için VM'lerinizin hedef kaynak gruplarında yer alması gerekir. Bu mantık, aboneliği veya kaynak grubunu hedefleyebilmeniz ve yeni oluşturulan VM'ler tarafından devralınan eylemleri oluşturabileceğiniz için Azure ilkesine benzer şekilde çalışır. Bu yaklaşım, her VM için ayrı bir zamanlama korumak zorunda önler ve ölçek başlar ve durur yönetir.

### <a name="schedules"></a>Zamanlamalar

Aşağıdaki tablo, Otomasyon hesabınızda oluşturulan varsayılan zamanlamaların her birini listeler.Bunları değiştirebilir veya kendi özel zamanlamalarınızı oluşturabilirsiniz.Varsayılan olarak, **Scheduled_StartVM** ve **Scheduled_StopVM**dışında tüm zamanlamaları devre dışı bırakılır.

Bu çakışan zamanlama eylemleri oluşturabileceğinden, tüm zamanlamaları etkinleştirmemelisiniz. En iyisi, hangi optimizasyonları gerçekleştirmek ve buna göre değiştirmek istediğinizi belirlemek. Daha fazla açıklama için genel bakış bölümündeki örnek senaryolara bakın.

|Zamanlama adı | Frequency | Açıklama|
|--- | --- | ---|
|Schedule_AutoStop_CreateAlert_Parent | 8 saatte bir | AutoStop_CreateAlert_Parent çalışma kitabını her 8 saatte bir çalıştıran bu kitap, azure otomasyon değişkenlerinde External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames ve External_ExcludeVMNames VM tabanlı değerleri durdurur. Alternatif olarak, VMList parametresini kullanarak virgülle ayrılmış vm listesini belirtebilirsiniz.|
|Scheduled_StopVM | Kullanıcı tanımlı, günlük | Scheduled_Parent runbook'u belirtilen saatte her gün _Stop_ parametresi ile çalıştırın.Varlık değişkenleri tarafından tanımlanan kurallara uyarlanan tüm VM'leri otomatik olarak durdurur.İlgili zamanlama, **Zamanlanmış-StartVM**etkinleştirin.|
|Scheduled_StartVM | Kullanıcı tanımlı, günlük | Scheduled_Parent runbook'u her gün belirtilen saatte _Başlat_ parametresi ile çalıştırın. Uygun değişkenler tarafından tanımlanan kurallara uygun tüm VM'leri otomatik olarak başlatır.İlgili zamanlama, **Zamanlanmış-StopVM**etkinleştirin.|
|Sıralı-StopVM | 01:00 (UTC), her Cuma | Sequenced_Parent runbook'u her Cuma belirtilen saatte _Durdur_ parametresi ile çalıştırır.Sırayla (artan) uygun değişkenler tarafından tanımlanan **SequenceStop** etiketi yle tüm VM'leri durdurur. Etiket değerleri ve varlık değişkenleri hakkında daha fazla bilgi için RunBook'lar bölümüne bakın.İlgili zamanlama, **Sequenced-StartVM**etkinleştirin.|
|Sıralı BaşlangıçVM | 13:00 (UTC), her Pazartesi | Her Pazartesi günü belirtilen zamanda _Başlat_ parametresi ile Sequenced_Parent runbook çalıştırın. Sırayla (azalan) tüm VM'leri uygun değişkenler tarafından tanımlanan **SequenceStart** etiketiyle başlatır. Etiket değerleri ve varlık değişkenleri hakkında daha fazla bilgi için RunBook'lar bölümüne bakın. İlgili zamanlama, **Sequenced-StopVM**etkinleştirin.|

## <a name="enable-the-solution"></a>Çözümü etkinleştirme

Çözümü kullanmaya başlamak [için, Başlat/Durdur VM çözümlü](automation-solution-vm-management-enable.md)adımları gerçekleştirin.

## <a name="viewing-the-solution"></a>Çözümü görüntüleme

Çözüme aşağıdaki yollardan birini etkinleştirdikten sonra erişebilirsiniz:

* Otomasyon hesabınızdan, **İlgili Kaynaklar**altında **Başlat/Durdur VM'yi** seçin. **Başlat/Durdur VM** sayfasında, Başlat/Durdur **VM Çözümlerini Yönet**bölümünün altındaki sayfanın sağ tarafından **çözümü yönet'i** seçin.

* Otomasyon hesabınıza bağlı Log Analytics çalışma alanına gidin ve çalışma alanını seçtikten sonra sol bölmeden **Çözümler'i** seçin. **Çözümler** sayfasında, listeden **Start-Stop-VM[çalışma alanı]** çözümünü seçin.  

Çözümü seçmek, **Start-Stop-VM[çalışma alanı]** çözüm sayfasını görüntüler. Burada **StartStopVM** döşemesi gibi önemli ayrıntıları gözden geçirebilirsiniz. Log Analytics çalışma alanınızda olduğu gibi, bu döşeme, başlatılan ve başarıyla tamamlanan çözüm için runbook işlerinin bir sayısını ve grafik gösterimini görüntüler.

![Otomasyon Güncelleme Yönetimi çözüm sayfası](media/automation-solution-vm-management/azure-portal-vmupdate-solution-01.png)

Buradan, donut karosu tıklayarak iş kayıtlarının daha fazla analiz gerçekleştirebilirsiniz. Çözüm panosu, iş geçmişini ve önceden tanımlanmış günlük arama sorgularını gösterir. Arama sorgularınıza göre arama yapmak için günlük analitiği gelişmiş portalına geçin.

## <a name="update-the-solution"></a>Çözümü güncelleştirme

Bu çözümün önceki bir sürümünü dağıttıysanız, güncelleştirilmiş bir sürümü dağıtmadan önce bu çözümü hesabınızdan silmeniz gerekir. [Çözümü kaldırmak](#remove-the-solution) için adımları izleyin ve ardından [çözümü dağıtmak](automation-solution-vm-management-enable.md)için adımları izleyin.

## <a name="remove-the-solution"></a>Çözümü kaldırma

Çözümü artık kullanmanıza gerek olmadığına karar verirseniz, çözümü Otomasyon hesabından silebilirsiniz. Çözümü siler, yalnızca runbook'ları kaldırır. Çözüm eklendiğinde oluşturulan zamanlamaları veya değişkenleri silmez. Diğer runbook'larla kullanmıyorsanız, el ile silmeniz gereken kıymetler.

Çözümü silmek için aşağıdaki adımları gerçekleştirin:

1. Otomasyon hesabınızdan, **İlgili kaynaklar** **altında, Bağlantılı çalışma alanını**seçin.

2. **Çalışma alanına git'i**seçin.

3. **Genel**altında, **Çözümler**seçin. 

4. **Çözümler** sayfasında, **başlat-durdur-VM[Çalışma Alanı]** çözümünü seçin. **VMManagementSolution[Workspace]** sayfasında, menüden **Sil'i**seçin.<br><br> ![VM Mgmt Çözümünü Sil](media/automation-solution-vm-management/vm-management-solution-delete.png)

5. Çözüm **Sil** penceresinde, çözümü silmek istediğinizi onaylayın.

6. Bilgiler doğrulanır ve çözüm silinir, menüden **Bildirimler** altında ilerlemesini izleyebilirsiniz. Çözüm densonra **Çözümler** sayfasına döndürülür.

Otomasyon hesabı ve Log Analytics çalışma alanı bu işlemin bir parçası olarak silinmez. Log Analytics çalışma alanını korumak istemiyorsanız, el ile silmeniz gerekir. Bu, Azure portalından gerçekleştirilebilir:

1. Azure portalında, Log **Analytics çalışma alanlarını**arayın ve seçin.

2. Günlük **Analizi çalışma alanları** sayfasında çalışma alanını seçin.

3. Çalışma alanı ayarları sayfasındaki menüden **Sil'i** seçin.

Azure Otomasyon hesap bileşenlerini korumak istemiyorsanız, her birini el ile silebilirsiniz. Çözüm tarafından oluşturulan runbook'lar, değişkenler ve zamanlamalar listesi için [Çözüm bileşenlerine](#solution-components)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure VM'leriniz için mesai dışı çözüm sırasında Başlat/Durdur'u [etkinleştirin.](automation-solution-vm-management-enable.md)
