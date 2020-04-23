---
title: VM 'Leri çalışma saatleri dışında Başlat/Durdur
description: Bu VM yönetimi çözümü, Azure sanal makinelerinizi bir zamanlamaya göre başlatır ve sonlandırır ve Azure Izleyici günlüklerinden daha etkin bir izleyici izler.
services: automation
ms.subservice: process-automation
ms.date: 04/01/2020
ms.topic: conceptual
ms.openlocfilehash: 0331678b50d2448013556ab0694d0ca87045c3a3
ms.sourcegitcommit: 086d7c0cf812de709f6848a645edaf97a7324360
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82096925"
---
# <a name="startstop-vms-during-off-hours-solution-in-azure-automation"></a>Azure Otomasyonu 'nda çalışma saatleri dışında VM 'Leri başlatma/durdurma

**VM 'leri çalışma saatleri dışında Başlat/Durdur** çözümü, Azure sanal makinelerinizi başlatır veya durdurur. Kullanıcı tanımlı zamanlamalarda makineleri başlatır veya sonlandırır, Azure Izleyici günlükleri aracılığıyla öngörüler sağlar ve [eylem gruplarını](../azure-monitor/platform/action-groups.md)kullanarak isteğe bağlı e-postalar gönderir. Çözüm çoğu senaryo için hem Azure Resource Manager hem de klasik VM 'Leri destekler. 

Bu çözüm, VM maliyetlerini iyileştirmek isteyen kullanıcılar için merkezileşmemiş düşük maliyetli bir Otomasyon seçeneği sağlar. Bu çözümle şunları yapabilirsiniz:

- [VM 'leri başlatıp durdurulacak şekilde zamanlayın](automation-solution-vm-management-config.md#schedule).
- [Azure etiketlerini kullanarak](automation-solution-vm-management-config.md#tags) (klasik VM 'lerde desteklenmez), sanal makinelerin artan sırada başlamasını ve durdurulmasını zamanlayın.
- VM 'Leri [düşük CPU kullanımına](automation-solution-vm-management-config.md#cpuutil)göre oto durdur.

> [!NOTE]
> **Çalışma saatleri dışında VM 'Leri Başlat/Durdur** çözümü, kullanılabilir Azure modüllerinin en yeni sürümlerini destekleyecek şekilde güncelleştirilmiştir.

Geçerli çözümle ilgili sınırlamalar aşağıda verilmiştir:

- Herhangi bir bölgedeki VM 'Leri yönetir, ancak yalnızca Azure Otomasyonu hesabınızla aynı abonelikte kullanılabilir.
- Azure ve Azure Kamu 'da, bir Log Analytics çalışma alanını, bir Azure Otomasyonu hesabını ve uyarıları destekleyen her bölgede kullanılabilir. Azure Kamu bölgeleri Şu anda e-posta işlevlerini desteklememektedir.

## <a name="solution-prerequisites"></a>Çözüm önkoşulları

Bu çözüme yönelik runbook 'lar bir [Azure farklı çalıştır hesabıyla](automation-create-runas-account.md)çalışır. Bu farklı çalıştır hesabı, kullanım süresini dolacak veya sıklıkla değiştirebilen bir parola yerine sertifika kimlik doğrulaması kullandığından, tercih edilen kimlik doğrulama yöntemidir.

**VM 'leri çalışma saatleri dışında Başlat/Durdur** çözümü için ayrı bir Otomasyon hesabı kullanmanızı öneririz. Azure modül sürümleri sıklıkla yükseltilir ve parametreleri değişebilir. Çözüm aynı temposunda üzerinde yükseltilmemiştir ve kullandığı cmdlet 'lerin daha yeni sürümleriyle çalışmayabilir. Modül güncelleştirmelerini Üretim otomasyon hesabınızda içeri aktarmadan önce bir test Otomasyonu hesabında test etmeniz önerilir.

## <a name="solution-permissions"></a>Çözüm izinleri

**Çalışma saatleri dışında, VM 'Leri Başlat/Durdur** çözümü dağıtmak için belirli izinlere sahip olmanız gerekir. Çözüm önceden oluşturulmuş bir Otomasyon hesabı kullanıyorsa ve çözüm, dağıtım sırasında yeni bir hesap ve çalışma alanı oluşturduğunda gereken izinlerle Log Analytics çalışma alanı kullanıyorsa, izinler farklıdır. 

Abonelik üzerinde bir katkıda bulunsanız ve Azure Active Directory kiracınızda genel yönetici varsa izinleri yapılandırmanız gerekmez. Bu haklara sahip değilseniz veya özel bir rol yapılandırmanız gerekmiyorsa, aşağıda açıklanan izinlere sahip olduğunuzdan emin olun.

### <a name="permissions-for-pre-existing-automation-account-and-log-analytics-workspace"></a>Önceden var olan Otomasyon hesabı ve Log Analytics çalışma alanı için izinler

Mevcut bir Otomasyon hesabına ve Log Analytics çalışma alanına, **saat dışı çözüm sırasında VM 'Leri Başlat/Durdur** çözümü dağıtmak için, çözümü dağıtan Kullanıcı, kaynak grubu kapsamında aşağıdaki izinleri gerektirir. Roller hakkında daha fazla bilgi için bkz. [Azure kaynakları Için özel roller](../role-based-access-control/custom-roles.md).

| İzin | Kapsam|
| --- | --- |
| Microsoft. Automation/automationAccounts/Read | Kaynak Grubu |
| Microsoft. Automation/automationAccounts/değişkenler/yazma | Kaynak Grubu |
| Microsoft. Automation/automationAccounts/zamanlamalar/yazma | Kaynak Grubu |
| Microsoft. Automation/automationAccounts/Runbook/yazma | Kaynak Grubu |
| Microsoft. Automation/automationAccounts/bağlantılar/yazma | Kaynak Grubu |
| Microsoft. Automation/automationAccounts/sertifikalar/yazma | Kaynak Grubu |
| Microsoft. Automation/automationAccounts/modüller/yazma | Kaynak Grubu |
| Microsoft. Automation/automationAccounts/modüller/okuma | Kaynak Grubu |
| Microsoft. Automation/automationAccounts/Jobzamanlamalar/yazma | Kaynak Grubu |
| Microsoft. Automation/automationAccounts/Jobs/Write | Kaynak Grubu |
| Microsoft. Automation/automationAccounts/Jobs/okuma | Kaynak Grubu |
| Microsoft. OperationsManagement/Solutions/Write | Kaynak Grubu |
| Microsoft. Operationalınsights/çalışma alanları/* | Kaynak Grubu |
| Microsoft. Insights/diagnosticSettings/Write | Kaynak Grubu |
| Microsoft. Insights/ActionGroups/Write | Kaynak Grubu |
| Microsoft. Insights/ActionGroups/Read | Kaynak Grubu |
| Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak Grubu |
| Microsoft. resources/dağıtımlar/* | Kaynak Grubu |

### <a name="permissions-for-new-automation-account-and-new-log-analytics-workspace"></a>Yeni Otomasyon hesabı ve yeni Log Analytics çalışma alanı izinleri

Yeni bir Otomasyon hesabına ve Log Analytics çalışma alanına çalışma **saatleri dışında VM 'Leri Başlat/Durdur** çözümü dağıtabilirsiniz. Bu durumda, çözümü dağıtan Kullanıcı, önceki bölümde tanımlanan izinlere ve bu bölümde tanımlanan izinlere ihtiyaç duyuyor. 

Çözümü dağıtan Kullanıcı aşağıdaki rollere ihtiyaç duyuyor:

- Abonelikte coadministrator. Klasik VM 'Leri yönetecekdeyseniz, bu rol klasik farklı çalıştır hesabını oluşturmak için gereklidir. [Klasik farklı çalıştır hesapları](automation-create-standalone-account.md#create-a-classic-run-as-account) artık varsayılan olarak oluşturulmaz.
- [Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md) uygulama geliştirici rolünde üyelik. Farklı Çalıştır hesaplarını yapılandırma hakkında daha fazla bilgi için bkz. [Farklı Çalıştır hesaplarını yapılandırma izinleri](manage-runas-account.md#permissions).
- Abonelik üzerinde veya aşağıdaki izinlerle katkıda bulunan.

| İzin |Kapsam|
| --- | --- |
| Microsoft. Authorization/Işlemler/okuma | Abonelik|
| Microsoft. Authorization/Permissions/Read |Abonelik|
| Microsoft. Authorization/Roleatamalar/okuma | Abonelik |
| Microsoft.Authorization/roleAssignments/write | Abonelik |
| Microsoft. Authorization/Roleatamaları/silme | Abonelik |
| Microsoft. Automation/automationAccounts/Connections/Read | Kaynak Grubu |
| Microsoft. Automation/automationAccounts/sertifikalar/okuma | Kaynak Grubu |
| Microsoft. Automation/automationAccounts/Write | Kaynak Grubu |
| Microsoft. Operationalınsights/çalışma alanları/yazma | Kaynak Grubu |

## <a name="solution-components"></a>Çözüm bileşenleri

**VM 'leri çalışma saatleri dışında Başlat/Durdur çözümü** , önceden yapılandırılmış runbook 'ları, zamanlamaları ve Azure izleyici günlükleri ile tümleştirmeyi içerir. Bu öğeleri, sanal makinelerinizin başlangıç ve kapatılmasını iş gereksinimlerinize uyacak şekilde uyarlamak için kullanabilirsiniz.

### <a name="runbooks"></a>Runbook'lar

Aşağıdaki tabloda, çözümün Otomasyon hesabınıza dağıttığı runbook 'lar listelenmektedir. Runbook kodunda değişiklik yapmayın. Bunun yerine, yeni işlevsellik için kendi runbook 'unuzu yazın.

> [!IMPORTANT]
> **Alt öğesi** adına eklenmiş herhangi bir runbook 'u doğrudan çalıştırmayın.

Tüm üst runbook 'lar `WhatIf` parametresi içerir. True olarak ayarlandığında parametresi, runbook 'un parametre olmadan çalıştırıldığında aldığı tam davranışı ayrıntılandıran şekilde destekler ve doğru VM 'Lerin hedeflendiğine doğrular. Bir runbook yalnızca `WhatIf` parametre false olarak ayarlandığında tanımlı eylemlerini gerçekleştirir.

|Runbook | Parametreler | Açıklama|
| --- | --- | ---|
|AutoStop_CreateAlert_Child | VMObject <br> AlertAction <br> WebHookURI | Üst runbook 'tan çağırılır. Bu runbook otomatik durdurma senaryosu için kaynak temelinde uyarı oluşturur.|
|AutoStop_CreateAlert_Parent | VMList<br> WhatIf: true veya false  | Hedeflenen abonelik veya kaynak gruplarındaki VM 'lerde Azure uyarı kuralları oluşturur veya güncelleştirir. <br> `VMList`, VM 'lerin virgülle ayrılmış listesidir. Örneğin, `vm1, vm2, vm3`.<br> `WhatIf`yürütme olmadan runbook mantığının doğrulanmasına izin vermez.|
|AutoStop_Disable | Hiçbiri | Otomatik durdurma uyarılarını ve Varsayılan zamanlamayı devre dışı bırakır.|
|AutoStop_VM_Child | WebHookData | Üst runbook 'tan çağırılır. Uyarı kuralları, klasik bir sanal makineyi durdurmak için bu runbook 'u çağırır.|
|AutoStop_VM_Child_ARM | WebHookData |Üst runbook 'tan çağırılır. Uyarı kuralları bir sanal makineyi durdurmak için bu runbook 'u çağırır.  |
|ScheduledStartStop_Base_Classic | CloudServiceName<br> Eylem: Başlat veya Durdur<br> VMList  | Cloud Services tarafından klasik VM grubunda Eylem Başlat veya durdur işlemini gerçekleştirir. |
|ScheduledStartStop_Child | VMName <br> Eylem: Başlat veya Durdur <br> ResourceGroupName | Üst runbook 'tan çağırılır. Zamanlanan durdurma için bir Başlat veya Durdur eylemi yürütür.|
|ScheduledStartStop_Child_Classic | VMName<br> Eylem: Başlat veya Durdur<br> ResourceGroupName | Üst runbook 'tan çağırılır. Klasik VM 'Ler için zamanlanmış durdurma için Başlat veya Durdur eylemini yürütür. |
|ScheduledStartStop_Parent | Eylem: Başlat veya Durdur <br>VMList <br> WhatIf: true veya false | Abonelikteki tüm VM 'Leri başlatır veya sonlandırır. Değişkenleri `External_Start_ResourceGroupNames` düzenleyin ve `External_Stop_ResourceGroupNames` yalnızca bu hedeflenmiş kaynak gruplarında yürütülecek şekilde yürütün. Ayrıca, `External_ExcludeVMNames` değişkeni güncelleştirerek belirli VM 'leri hariç bırakabilirsiniz.|
|SequencedStartStop_Parent | Eylem: Başlat veya Durdur <br> WhatIf: true veya false<br>VMList| Başlat/Durdur etkinliğini sıralamak istediğiniz her VM üzerinde **sequencestart** ve **sequencestop** adlı Etiketler oluşturur. Bu etiket adları büyük/küçük harfe duyarlıdır. Etiketin değeri, başlatmak veya durdurmak istediğiniz sıraya karşılık gelen pozitif bir tamsayı (1, 2, 3) olmalıdır. <br>**Note**: VM 'ler `External_Start_ResourceGroupNames`, `External_Stop_ResourceGroupNames`, ve `External_ExcludeVMNames` değişkenlerinde tanımlanan kaynak grupları içinde olmalıdır. Eylemlerin etkili olabilmesi için uygun etiketlere sahip olmaları gerekir.|

### <a name="variables"></a>Değişkenler

Aşağıdaki tabloda, Otomasyon hesabınızda oluşturulan değişkenler listelenmektedir. Yalnızca ile `External`önekli değişkenleri değiştirin. Ön eki olan değişkenlerin `Internal` değiştirilmesi, istenmeyen etkilere neden olur.

> [!NOTE]
> VM adı ve kaynak grubuyla ilgili sınırlamalar büyük ölçüde değişken boyutunun bir sonucudur. Bkz. [Azure Otomasyonu 'Nda değişken varlıkları](https://docs.microsoft.com/azure/automation/shared-resources/variables).

|Değişken | Açıklama|
|---------|------------|
|External_AutoStop_Condition | Bir uyarıyı tetiklemeden önce koşulu yapılandırmak için gereken koşullu işleç. Kabul edilebilir değerler `GreaterThan`, `GreaterThanOrEqual` `LessThan`,, ve `LessThanOrEqual`.|
|External_AutoStop_Description | CPU yüzdesi eşiği aşarsa VM 'yi durdurma uyarısı.|
|External_AutoStop_Frequency | Kural için değerlendirme sıklığı. Bu parametre, girişi TimeSpan biçiminde kabul eder. Olası değerler 5 dakikadan 6 saate kadar sürer. |
|External_AutoStop_MetricName | Azure uyarı kuralının yapılandırıldığı performans ölçümünün adı.|
|External_AutoStop_Severity | Ölçüm uyarısının önem derecesi, 0 ile 4 arasında değişebilir. |
|External_AutoStop_Threshold | Değişkende `External_AutoStop_MetricName`belirtilen Azure uyarı kuralının eşiği. Yüzde değerleri 1 ile 100 arasındadır.|
|External_AutoStop_TimeAggregationOperator | Koşulu değerlendirmek için seçilen pencere boyutuna uygulanan zaman toplama işleci. Kabul edilebilir değerler `Average`, `Minimum` `Maximum` `Total`,, ve `Last`.|
|External_AutoStop_TimeWindow | Azure 'un bir uyarı tetiklenmesi için seçili ölçümleri analiz eden pencerenin boyutu. Bu parametre, girişi TimeSpan biçiminde kabul eder. Olası değerler 5 dakikadan 6 saate kadar sürer.|
|External_EnableClassicVMs| Klasik VM 'Lerin çözüm tarafından hedefleneceğini belirten değer. Varsayılan değer true 'dur. Azure bulut çözümü sağlayıcısı (CSP) abonelikleri için bu değişkeni false olarak ayarlayın. Klasik VM 'Ler, [Klasik farklı çalıştır hesabı](automation-create-standalone-account.md#create-a-classic-run-as-account)gerektirir.|
|External_ExcludeVMNames | 140 VM 'Lerle sınırlı tutulacak VM adlarının virgülle ayrılmış listesi. Listeye 140 'den fazla VM eklerseniz, dışarıda bırakılacak şekilde ayarlanan VM 'Ler istenmeden başlatılabilir veya durmuş olabilir.|
|External_Start_ResourceGroupNames | Başlatma eylemleri için hedeflenen bir veya daha fazla kaynak grubunun virgülle ayrılmış listesi.|
|External_Stop_ResourceGroupNames | Durdurma eylemlerini hedefleyen bir veya daha fazla kaynak grubunun virgülle ayrılmış listesi.|
|External_WaitTimeForVMRetrySeconds |**SequencedStartStop_Parent** runbook 'un VM 'lerde gerçekleştirilmesi için saniye cinsinden bekleme süresi. Bu değişken, runbook 'un bir sonraki eylemle devam etmeden önce belirtilen saniye sayısı için alt işlemleri beklemesine izin verir. En uzun bekleme süresi 10800 veya üç saattir. Varsayılan değer 2100 saniyedir.|
|Internal_AutomationAccountName | Otomasyon hesabının adını belirtir.|
|Internal_AutoSnooze_ARM_WebhookURI | VM 'Ler için oto durdurma senaryosuna çağrılan Web kancası URI 'SI.|
|Internal_AutoSnooze_WebhookUri | Klasik VM 'Ler için oto durdurma senaryosu adlı Web kancası URI 'SI.|
|Internal_AzureSubscriptionId | Azure abonelik KIMLIĞI.|
|Internal_ResourceGroupName | Otomasyon hesabı kaynak grubu adı.|

>[!NOTE]
>Değişken `External_WaitTimeForVMRetryInSeconds`için, varsayılan değer 600 ' den 2100 ' e güncelleştirilmiştir. 

Tüm senaryolarda, `External_Start_ResourceGroupNames`ve `External_Stop_ResourceGroupNames` `External_ExcludeVMNames` değişkenleri, **AutoStop_CreateAlert_Parent**, **SequencedStartStop_Parent**ve **ScheduledStartStop_Parent** runbook 'ları için virgülle ayrılmış VM listeleri hariç olmak üzere VM 'leri hedeflemek için gereklidir. Diğer bir deyişle, sanal makinelerinizin başlatma ve durdurma eylemlerinin gerçekleşmesi için hedef kaynak gruplarına ait olması gerekir. Mantığı Azure ilkesine benzer şekilde çalışarak abonelik veya kaynak grubunu hedefleyebilir ve yeni oluşturulan VM 'Ler tarafından devralınan eylemlere sahip olabilirsiniz. Bu yaklaşım her VM için ayrı bir zamanlamanın korunmasını, sonra da her bir sanal makine için ayrı bir zamanlama olmasını önler.

### <a name="schedules"></a>Zamanlamalar

Aşağıdaki tabloda, Otomasyon hesabınızda oluşturulan varsayılan zamanlamaların her biri listelenmektedir.Bunları değiştirebilir veya kendi özel zamanlamalarınızı oluşturabilirsiniz.Varsayılan olarak, **Scheduled_StartVM** ve **Scheduled_StopVM** zamanlamaları hariç tüm zamanlamalar devre dışı bırakılır.

Tüm zamanlamaları etkinleştirmeyin, çünkü bu durum çakışan zamanlama eylemleri oluşturabilir. En iyi yöntem, hangi iyileştirmelerin yapmak istediğinizi belirlemektir ve bunları uygun şekilde değiştirebilirsiniz. Daha fazla açıklama için genel bakış bölümündeki örnek senaryolar bölümüne bakın.

|Zamanlama adı | Frequency | Açıklama|
|--- | --- | ---|
|Schedule_AutoStop_CreateAlert_Parent | 8 saatte bir | **AutoStop_CreateAlert_Parent** runbook 'u her 8 saatte bir çalıştırır ve bu da, ve `External_Start_ResourceGroupNames` `External_Stop_ResourceGroupNames` `External_ExcludeVMNames` değişkenlerinde VM tabanlı değerleri de durduruyor. Alternatif olarak, `VMList` parametresini kullanarak, VM 'lerin virgülle ayrılmış bir listesini belirtebilirsiniz.|
|Scheduled_StopVM | Kullanıcı tanımlı, günlük | **ScheduledStopStart_Parent** runbook 'u `Stop` her gün belirtilen zamanda bir parametre ile çalıştırır., Değişken varlıklar tarafından tanımlanan kuralları karşılayan tüm VM 'Leri otomatik olarak sonlandırır.**Zamanlanan-StartVM**ilgili zamanlamasını etkinleştirin.|
|Scheduled_StartVM | Kullanıcı tanımlı, günlük | **ScheduledStopStart_Parent** runbook 'u, belirtilen saatte `Start` her gün bir parametre değeri ile çalıştırır. , Değişken varlıklar tarafından tanımlanan kuralları karşılayan tüm VM 'Leri otomatik olarak başlatır.İlgili zamanlamayı **Zamanlanmış-StopVM**' i etkinleştirin.|
|Sıralı-StopVM | 1:00 (UTC), her Cuma | **Sequenced_StopStop_Parent** runbook 'U `Stop` her Cuma için belirtilen zamanda bir parametre değeri ile çalıştırır.Sıralı (artan), uygun değişkenler tarafından tanımlanan bir **SequenceStop** etiketine sahip tüm VM 'leri sonlandırır. Etiket değerleri ve varlık değişkenleri hakkında daha fazla bilgi için bkz. [runbook 'lar](#runbooks).**Sıralı-StartVM**ilgili zamanlamasını etkinleştirin.|
|Sıralı-StartVM | 1:00 PM (UTC), her Pazartesi | **SequencedStopStart_Parent** runbook 'U `Start` her Pazartesi için belirtilen zamanda bir parametre değeri ile çalıştırır. Sıralı (azalan), uygun değişkenlerle tanımlanan **Sequencestart** etiketiyle tüm VM 'leri başlatır. Etiket değerleri ve değişken varlıklar hakkında daha fazla bilgi için bkz. [runbook 'lar](#runbooks). İlgili zamanlamayı, **sıralı-StopVM**'yi etkinleştirin.

## <a name="use-of-the-solution-with-classic-vms"></a>Klasik VM 'lerle çözüm kullanımı

Klasik VM 'Ler için **saat dışında VM 'Leri Başlat/Durdur** çözümü kullanıyorsanız, Otomasyon tüm VM 'lerinizi bulut hizmeti başına sırayla işler. VM 'Ler, farklı bulut hizmetlerinde paralel olarak hala işlenir. 

Klasik VM 'lerle çözümü kullanmak için, varsayılan olarak oluşturulmamış bir klasik Farklı Çalıştır hesabına ihtiyacınız vardır. Klasik farklı çalıştır hesabı oluşturma yönergeleri için bkz. [Klasik farklı çalıştır hesabı oluşturma](automation-create-standalone-account.md#create-a-classic-run-as-account).

Bulut hizmeti başına 20 ' den fazla VM varsa, bazı öneriler aşağıda verilmiştir:

* Üst runbook **ScheduledStartStop_Parent** birden çok zamanlama oluşturun ve zamanlamaya göre 20 VM 'yi belirtin. 
* Zamanlama özelliklerinde, VM adlarını virgülle ayrılmış `VMList` bir liste olarak belirtmek için parametresini kullanın. 

Aksi takdirde, bu çözüm için Otomasyon işi üçten fazla saat çalışırsa, bu, dengeli olarak kaldırılmış veya [dengeli](automation-runbook-execution.md#fair-share) bir şekilde kaldırılır.

Azure CSP abonelikleri yalnızca Azure Resource Manager modelini destekler. Azure Resource Manager olmayan hizmetler programda yok. **Çalışma saatleri dışında VM 'Leri Başlat/Durdur çözümü** çalıştırıldığında, klasik kaynakları yönetmek için cmdlet 'ler olduğundan hatalar alabilirsiniz. CSP hakkında daha fazla bilgi edinmek için bkz. [CSP aboneliklerinde kullanılabilir hizmetler](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services). CSP aboneliği kullanıyorsanız, dağıtımdan sonra [External_EnableClassicVMs](#variables) değişkenini false olarak ayarlamanız gerekir.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="enable-the-solution"></a>Çözümü etkinleştirme

Çözümü kullanmaya başlamak için [VM 'Leri başlatma/durdurma çözümünü etkinleştirme](automation-solution-vm-management-enable.md)bölümündeki adımları izleyin.

## <a name="view-the-solution"></a>Çözümü görüntüleme

Etkinleştirildikten sonra çözüme erişmek için aşağıdaki mekanizmalardan birini kullanın:

* Otomasyon hesabınızdan **Ilgili kaynaklar**altında **sanal makineyi Başlat/Durdur** ' u seçin. VM 'yi Başlat/Durdur sayfasında, sayfanın sağ tarafındaki **çözümü** Yönet ' in altında, **VM çözümlerini Başlat/Durdur**' u seçin.

* Otomasyon hesabınıza bağlı Log Analytics çalışma alanına gidin. Çalışma alanını seçtikten sonra sol bölmeden **çözümler** ' i seçin. Çözümler sayfasında, listeden **Başlat-Durdur-VM [çalışma alanı]** çözümünü seçin.  

Çözümü seçtiğinizde, **Start-Stop-VM [çalışma alanı]** çözüm sayfası görüntülenir. Burada, **Startstopvm** kutucuğundaki bilgiler gibi önemli ayrıntıları gözden geçirebilirsiniz. Log Analytics çalışma alanınızda olduğu gibi, bu kutucuk, başlatılan ve başarıyla tamamlanmış çözüm için Runbook işlerinin bir sayısını ve grafik temsilini görüntüler.

![Otomasyon Güncelleştirme Yönetimi çözüm sayfası](media/automation-solution-vm-management/azure-portal-vmupdate-solution-01.png)

Halka kutucuğuna tıklayarak iş kayıtlarının daha fazla analizini yapabilirsiniz. Çözüm panosu iş geçmişi ve önceden tanımlanmış günlük arama sorguları gösterir. Arama sorgularınıza göre arama yapmak için Log Analytics gelişmiş portala geçiş yapın.

## <a name="update-the-solution"></a>Çözümü güncelleştirme

Bu çözümün önceki bir sürümünü dağıttıysanız, güncelleştirilmiş bir yayını dağıtmadan önce hesabınızı hesabınızdan silin. [Çözümü kaldırmak](#remove-the-solution) için adımları izleyin ve sonra [çözümü dağıtmak](automation-solution-vm-management-enable.md)için adımları izleyin.

## <a name="remove-the-solution"></a>Çözümü kaldırma

Çözümü artık kullanmanız gerekmiyorsa, Otomasyon hesabından silebilirsiniz. Çözümün silinmesi yalnızca runbook 'ları kaldırır. Çözüm eklendiğinde oluşturulan zamanlamaları veya değişkenleri silmez. Bunları diğer runbook 'larla kullanmıyorsanız, bu varlıkları el ile kaldırın.

Çözümü silmek için:

1. Otomasyon hesabınızdan **ilgili kaynaklar**altında **bağlantılı çalışma alanı** ' nı seçin.

2. **Çalışma alanına git**' i seçin.

3. **Genel**altında **çözümler** ' e tıklayın. 

4. Çözümler sayfasında, **Başlat-Durdur-VM [çalışma alanı]** çözümünü seçin. 

5. **Vmmanagementsolution [çalışma alanı]** sayfasında menüden **Sil** ' i seçin.<br><br> ![VM yönetimi çözümünü Sil](media/automation-solution-vm-management/vm-management-solution-delete.png)

6. **Çözümü Sil** penceresinde, çözümü silmek istediğinizi onaylayın.

7. Bilgiler doğrulanırken ve çözüm silinirken, menüden seçili olan **Bildirimler**altında ilerlemeyi izleyebilirsiniz. Çözümü kaldırma işlemi başladıktan sonra çözümler sayfasına dönersiniz.

Otomasyon hesabı ve Log Analytics çalışma alanı bu işlemin bir parçası olarak silinmez. Log Analytics çalışma alanını tutmak istemiyorsanız, Azure portal el ile silmeniz gerekir:

1. **Log Analytics çalışma alanlarını**arayın ve seçin.

2. Log Analytics çalışma alanı sayfasında, çalışma alanını seçin.

3. Çalışma alanı ayarları sayfasındaki menüden **Sil** ' i seçin.

4. Azure Otomasyonu hesap [çözümü bileşenlerini](#solution-components)korumak istemiyorsanız, her bir el ile silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure VM 'leriniz için **çalışma saatleri dışında VM 'Leri başlatma/durdurma** çözümünü [etkinleştirin](automation-solution-vm-management-enable.md) .
