---
title: Mesai dışı çözüm sırasında VM'leri başlatma/durdurma
description: Bu VM yönetimi çözümü, Azure sanal makinelerinizi bir zamanlamayla başlatır ve durdurur ve Azure Monitor günlüklerini proaktif olarak izler.
services: automation
ms.subservice: process-automation
ms.date: 04/01/2020
ms.topic: conceptual
ms.openlocfilehash: 968e609772e08814a9943734d30c16bf6f5972e8
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81604721"
---
# <a name="startstop-vms-during-off-hours-solution-in-azure-automation"></a>Azure Otomasyonu'nda mesai dışı çözüm sırasında VM'leri başlatma/durdurma

**Çalışma saatleri dışında başlat/durdur çözümü** Azure sanal makinelerinizi başlatır veya durdurur. Kullanıcı tanımlı zamanlamalarda makineleri başlatır veya durdurur, Azure Monitor günlükleri aracılığıyla öngörüler sağlar ve [eylem gruplarını](../azure-monitor/platform/action-groups.md)kullanarak isteğe bağlı e-postalar gönderir. Çözüm, çoğu senaryo için hem Azure Kaynak Yöneticisi'ni hem de klasik VM'leri destekler. 

Bu çözüm, VM maliyetlerini optimize etmek isteyen kullanıcılar için merkezi olmayan düşük maliyetli bir otomasyon seçeneği sağlar. Bu çözümle şunları yapabilirsiniz:

- [Başlatmak ve durdurmak için VM'leri zamanlayın.](automation-solution-vm-management-config.md#schedule)
- [Azure Etiketleri kullanarak](automation-solution-vm-management-config.md#tags) (klasik VM'ler için desteklenmeyen) vm'leri artan sırada başlatmak ve durdurmak için zamanlama.
- [Düşük CPU kullanımına](automation-solution-vm-management-config.md#cpuutil)dayalı Otomatik Durdurma VM'leri.

> [!NOTE]
> **Çalışma saatleri dışında başlat/durdur** çözümü, kullanılabilir Azure modüllerinin en yeni sürümlerini destekleyecek şekilde güncelleştirildi.

Geçerli çözümle ilgili sınırlamalar şunlardır:

- Herhangi bir bölgedeki VM'leri yönetir, ancak yalnızca Azure Otomasyon hesabınızla aynı abonelikte kullanılabilir.
- Azure ve Azure Kamu'da, Günlük Analizi çalışma alanını, Azure Otomasyon hesabını ve uyarıları destekleyen tüm bölgeleriçin kullanılabilir. Azure Genel bölgeleri şu anda e-posta işlevselliğini desteklemiyor.

## <a name="solution-prerequisites"></a>Çözüm önkoşulları

Bu çözümün runbook'ları bir [Azure Run As hesabıyla](automation-create-runas-account.md)çalışır. Run As hesabı, süresi dolan veya sık sık değişebilecek bir parola yerine sertifika kimlik doğrulaması kullandığından tercih edilen kimlik doğrulama yöntemidir.

Mesai dışı çözüm **sırasında Start/stop VM'ler** için ayrı bir Otomasyon hesabı kullanmanızı öneririz. Azure modül sürümleri sık sık yükseltilir ve parametreleri değişebilir. Çözüm aynı cadence yükseltilmiş değil ve kullandığı cmdlets yeni sürümleri ile çalışmayabilir. Modül güncelleştirmelerini üretim Otomasyonu hesabınıza (lar) aktarmadan önce bir test Otomasyonu hesabında test etmeniz önerilir.

## <a name="solution-permissions"></a>Çözüm izinleri

**Mesai saatleri dışında başlat/durdur un vm'lerini** dağıtmak için belirli izinlere sahip olmalısınız. Çözüm dağıtım sırasında yeni bir hesap ve çalışma alanı oluşturursa, çözüm önceden oluşturulmuş bir Otomasyon hesabı ve Log Analytics çalışma alanı kullanıyorsa izinler farklıdır. 

Abonelikte Katkıda Bulunan ve Azure Etkin Dizin kiracınızda Global Yöneticiyseniz izinleri yapılandırmanız gerekmez. Bu haklara sahip değilseniz veya özel bir rolü yapılandırmanız gerekiyorsa, aşağıda açıklanan izinlere sahip olduğundan emin olun.

### <a name="permissions-for-pre-existing-automation-account-and-log-analytics-workspace"></a>Önceden varolan Otomasyon hesabı ve Log Analytics çalışma alanı için izinler

**Mesai saatleri dışında VM'leri** varolan bir Otomasyon hesabına ve Log Analytics çalışma alanına dağıtmak için, çözümü dağıtan kullanıcı Kaynak Grubu kapsamında aşağıdaki izinleri gerektirir. Roller hakkında daha fazla bilgi edinmek [için Azure kaynakları için Özel rolleri](../role-based-access-control/custom-roles.md)görün.

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

### <a name="permissions-for-new-automation-account-and-new-log-analytics-workspace"></a>Yeni Otomasyon hesabı ve yeni Log Analytics çalışma alanı için izinler

Mesai saatleri **dışında VM'leri** yeni bir Otomasyon hesabına ve Log Analytics çalışma alanına dağıtabilirsiniz. Bu durumda, çözümü dağıtan kullanıcı, önceki bölümde tanımlanan izinlerin yanı sıra bu bölümde tanımlanan izinlere ihtiyaç duyar. 

Çözümü dağıtan kullanıcının aşağıdaki rollere ihtiyacı vardır:

- Abonelikte yardımcı yönetici. Bu rol, Klasik VM'leri yönetecekseniz Klasik Çalıştır hesabı oluşturmak için gereklidir. [Klasik Çalıştır Hesapları](automation-create-standalone-account.md#create-a-classic-run-as-account) artık varsayılan olarak oluşturulmaz.
- [Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md) Application Developer rolüne üyelik. Hesap Olarak Çalıştır'ı yapılandırma hakkında daha fazla bilgi [için, Çalıştır'ı hesaplar olarak yapılandırmak için İzinler'e](manage-runas-account.md#permissions)bakın.
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

**Mesai dışı çözüm sırasında Başlat/durdur VM'leri** önceden yapılandırılmış runbook'ları, zamanlamaları ve Azure Monitor günlükleriyle tümleştirmeyi içerir. Bu öğeleri, VM'lerinizin başlatılmasını ve kapatılmasını iş gereksinimlerinize göre uyarlamak için kullanabilirsiniz.

### <a name="runbooks"></a>Runbook'lar

Aşağıdaki tabloda, çözümün Otomasyon hesabınıza dağıtdığı runbook'lar listelenebilmektedir. Runbook kodunda değişiklik yapma. Bunun yerine, yeni işlevler için kendi runbook yazın.

> [!IMPORTANT]
> Adına eklenen **alt** kitap içeren herhangi bir runbook'u doğrudan çalıştırmayın.

Tüm üst runbook'lar parametreiçerir. `WhatIf` Parametre True olarak ayarlandığında, parametre, parametre olmadan çalıştırıldığında runbook'un tam olarak aldığı davranışı ayrıntılarıyla ölçer ve doğru VM'lerin hedeflenildiğinden doğruolduğunu doğrular. Runbook yalnızca `WhatIf` parametre False olarak ayarlandığında tanımlanan eylemlerini gerçekleştirir.

|Runbook | Parametreler | Açıklama|
| --- | --- | ---|
|AutoStop_CreateAlert_Child | VMObject <br> AlertAction <br> WebHookURI | Üst çalışma kitabından çağrılır. Bu runbook, Otomatik Durdurma senaryosu için kaynak başına uyarılar oluşturur.|
|AutoStop_CreateAlert_Parent | VMList<br> WhatIf: Doğru veya Yanlış  | Hedeflenen abonelik veya kaynak gruplarında VM'lerde Azure uyarı kuralları oluşturur veya güncelleştirir. <br> `VMList`Virgülle ayrılmış bir VM listesidir. Örneğin, `vm1, vm2, vm3`.<br> `WhatIf`yürütmeden runbook mantığının doğrulanmasını sağlar.|
|AutoStop_Disable | Hiçbiri | Otomatik Durdurma uyarılarını ve varsayılan zamanlamayı devre dışı bırakır.|
|AutoStop_VM_Child | WebHookData | Üst çalışma kitabından çağrılır. Uyarı kuralları klasik bir VM'yi durdurmak için bu runbook'u çağırır.|
|AutoStop_VM_Child_ARM | WebHookData |Üst çalışma kitabından çağrılır. Uyarı kuralları, bir VM'yi durdurmak için bu runbook'u çağırır.  |
|ScheduledStartStop_Base_Classic | CloudServiceName<br> Eylem: Başlat veya Durdur<br> VMList  | Bulut Hizmetleri tarafından klasik VM grubunda eylem başlatma veya durdurma gerçekleştirir. |
|ScheduledStartStop_Child | VMName <br> Eylem: Başlat veya Durdur <br> ResourceGroupName | Üst çalışma kitabından çağrılır. Zamanlanan durak için bir başlatma veya durdurma eylemini yürütür.|
|ScheduledStartStop_Child_Classic | VMName<br> Eylem: Başlat veya Durdur<br> ResourceGroupName | Üst çalışma kitabından çağrılır. Klasik VM'ler için zamanlanan durak için bir başlatma veya durdurma eylemi yürütür. |
|ScheduledStartStop_Parent | Eylem: Başlat veya Durdur <br>VMList <br> WhatIf: Doğru veya Yanlış | Abonelikteki tüm VM'leri başlatır veya durdurur. Değişkenleri `External_Start_ResourceGroupNames` ve yalnızca `External_Stop_ResourceGroupNames` bu hedeflenen kaynak gruplarında yürütmek için edin. `External_ExcludeVMNames` Değişkeni güncelleyerek belirli VM'leri de hariç tutabilirsiniz.|
|SequencedStartStop_Parent | Eylem: Başlat veya Durdur <br> WhatIf: Doğru veya Yanlış<br>VMList| Başlat/durdurma etkinliğini sıralamak istediğiniz her VM'de **sequencestart** ve **sequencestop** adlı etiketler oluşturur. Bu etiket adları büyük/küçük harf duyarlıdır. Etiketin değeri, başlamak veya durdurmak istediğiniz sıraya karşılık gelen pozitif bir tamsayı (1, 2, 3) olmalıdır. <br>**Not**: VM'ler `External_Start_ResourceGroupNames`, , `External_Stop_ResourceGroupNames`ve `External_ExcludeVMNames` değişkenlerde tanımlanan kaynak grupları içinde olmalıdır. Eylemlerin etkili olması için uygun etiketlere sahip olmaları gerekir.|

### <a name="variables"></a>Değişkenler

Aşağıdaki tabloda Otomasyon hesabınızda oluşturulan değişkenler listeleneb.r.a. Yalnızca `External`. Değişkenleri önceden belirlenmiş `Internal` değişkenleri değiştirmek istenmeyen etkilere neden olur.

> [!NOTE]
> VM adı ve kaynak grubundaki sınırlamalar büyük ölçüde değişken boyutun bir sonucudur.

|Değişken | Açıklama|
|---------|------------|
|External_AutoStop_Condition | Bir uyarıtetiklemeden önce koşulu yapılandırmak için gereken koşullu işleç. Kabul edilebilir `GreaterThan` `GreaterThanOrEqual`değerler `LessThan`, `LessThanOrEqual`, , ve .|
|External_AutoStop_Description | CPU yüzdesi eşiği aşarsa VM durdurmak için uyarı.|
|External_AutoStop_Frequency | Kural için değerlendirme frekansı. Bu parametre, girişin zaman alanı biçiminde kabul edilir. Olası değerler 5 dakika ile 6 saat arasındadır. |
|External_AutoStop_MetricName | Azure Uyarı kuralının yapılandırılması gereken performans ölçümünün adı.|
|External_AutoStop_Severity | 0 ile 4 arasında değişen metrik uyarının önem derecesi. |
|External_AutoStop_Threshold | Değişkende `External_AutoStop_MetricName`belirtilen Azure Uyarısı kuralı için eşik . Yüzde değerleri 1 ile 100 arasında değişir.|
|External_AutoStop_TimeAggregationOperator | Durumu değerlendirmek için seçili pencere boyutuna uygulanan zaman toplama işleci. Kabul edilebilir `Average` `Minimum`değerler `Maximum` `Total`, `Last`, , , ve .|
|External_AutoStop_TimeWindow | Azure'un bir uyarıyı tetiklemek için seçili ölçümleri analiz ettiği pencerenin boyutu. Bu parametre, girişin zaman alanı biçiminde kabul edilir. Olası değerler 5 dakika ile 6 saat arasındadır.|
|External_EnableClassicVMs| Klasik VM'lerin çözüm tarafından hedeflenip hedeflenmemesini belirten değer. Varsayılan değer True'dur. Bu değişkeni Azure Bulut Çözüm Sağlayıcısı (CSP) abonelikleri için False olarak ayarlayın. Klasik VM'ler, [Klasik Çalıştır Hesabı](automation-create-standalone-account.md#create-a-classic-run-as-account)gerektirir.|
|External_ExcludeVMNames | 140 VM ile sınırlı, dışlandırılacak VM adlarının virgülle ayrılmış listesi. Listeye 140'tan fazla VM eklerseniz, hariç tutulacak şekilde ayarlanan VM'ler yanlışlıkla başlatılabilir veya durdurulabilir.|
|External_Start_ResourceGroupNames | Başlangıç eylemleri için hedeflenen bir veya daha fazla kaynak grubundan virgülle ayrılmış liste.|
|External_Stop_ResourceGroupNames | Durdurma eylemleri için hedeflenen bir veya daha fazla kaynak grubundan virgülle ayrılmış liste.|
|External_WaitTimeForVMRetrySeconds |**SequencedStartStop_Parent** çalışma kitabı için VM'lerde yapılacak eylemlerin saniye cinsinden bekleme süresi. Bu değişken, runbook'un bir sonraki eyleme geçmeden önce belirli sayıda saniye boyunca alt işlemleri beklemesini sağlar. Maksimum bekleme süresi 10800 veya üç saattir. Varsayılan değer 2100 saniyedir.|
|Internal_AutomationAccountName | Otomasyon hesabının adını belirtir.|
|Internal_AutoSnooze_ARM_WebhookURI | Webhook URI VMs için AutoStop senaryo çağrısında bulundu.|
|Internal_AutoSnooze_WebhookUri | Webhook URI klasik VM'ler için AutoStop senaryosu için çağrıda bulundu.|
|Internal_AzureSubscriptionId | Azure abonelik kimliği.|
|Internal_ResourceGroupName | Otomasyon hesabı kaynak grup adı.|

>[!NOTE]
>Değişken `External_WaitTimeForVMRetryInSeconds`için varsayılan değer 600'den 2100'e güncelleştirildi. 

Tüm senaryolarda, değişkenler `External_Start_ResourceGroupNames` `External_Stop_ResourceGroupNames`, `External_ExcludeVMNames` ve AutoStop_CreateAlert_Parent için virgülayrılmış VM listeleri hariç, VM'leri **SequencedStartStop_Parent**hedeflemek için gerekli olan **SequencedStartStop_Parent**ve **ScheduledStartStop_Parent** runbooks. Diğer bir süre, eylem başlatma ve durdurma eylemlerinin gerçekleşmesi için VM'lerinizin hedef kaynak gruplarına ait olması gerekir. Bu mantık, aboneliği veya kaynak grubunu hedefleyebilmeniz ve yeni oluşturulan VM'ler tarafından devralınan eylemleri oluşturabileceğiniz için Azure ilkesine benzer şekilde çalışır. Bu yaklaşım, her VM için ayrı bir zamanlama korumak zorunda önler ve ölçek başlar ve durur yönetir.

### <a name="schedules"></a>Zamanlamalar

Aşağıdaki tablo, Otomasyon hesabınızda oluşturulan varsayılan zamanlamaların her birini listeler.Bunları değiştirebilir veya kendi özel zamanlamalarınızı oluşturabilirsiniz.Varsayılan olarak, **Scheduled_StartVM** ve **Scheduled_StopVM** zamanlamaları dışında tüm zamanlamalar devre dışı bırakılır.

Bunu yapmak çakışan zamanlama eylemleri oluşturabileceğinden, tüm zamanlamaları etkinleştirme. En iyisi, hangi optimizasyonları yapmak istediğinizi belirlemek ve bunları buna göre değiştirmek. Daha fazla açıklama için genel bakış bölümündeki örnek senaryolara bakın.

|Zamanlama adı | Frequency | Açıklama|
|--- | --- | ---|
|Schedule_AutoStop_CreateAlert_Parent | 8 saatte bir | **AutoStop_CreateAlert_Parent** runbook'u her 8 saatte bir çalıştırAn bu `External_Start_ResourceGroupNames`kitap, VM tabanlı değerleri , ve `External_Stop_ResourceGroupNames` `External_ExcludeVMNames` değişkenlerde durdurur. Alternatif olarak, `VMList` parametreyi kullanarak virgülle ayrılmış bir VM listesi belirtebilirsiniz.|
|Scheduled_StopVM | Kullanıcı tanımlı, günlük | **ScheduledStopStart_Parent** runbook'u belirtilen zamanda `Stop` her günün parametresi ile çalıştırın.Değişken varlıklar tarafından tanımlanan kurallara uyarlanan tüm VM'leri otomatik olarak durdurur.İlgili **zamanlamaYı Etkinleştirin Zamanlanmış-BaşlangıçVM.**|
|Scheduled_StartVM | Kullanıcı tanımlı, günlük | **ScheduledStopStart_Parent** runbook'u belirtilen zamanda `Start` her günün parametre değeriyle çalıştırın. Değişken varlıklar tarafından tanımlanan kurallara uyarak tüm VM'leri otomatik olarak başlatır.İlgili **zamanlamaYı Etkinleştirin Zamanlanmış-StopVM.**|
|Sıralı-StopVM | 01:00 (UTC), her Cuma | Belirtilen zamanda her Cuma parametre `Stop` değeri ile Sequenced_Parent runbook çalışır.Sırayla (artan) uygun değişkenler tarafından tanımlanan **SequenceStop** etiketi yle tüm VM'leri durdurur. Etiket değerleri ve varlık değişkenleri hakkında daha fazla bilgi için RunBook'lar bölümüne bakın.İlgili zamanlama, **Sequenced-StartVM**etkinleştirin.|
|Sıralı BaşlangıçVM | 13:00 (UTC), her Pazartesi | **SequencedStopStart_Parent** runbook'u belirtilen zamanda `Start` her Pazartesi'nin parametre değeriyle çalıştırın. Sırayla (azalan) tüm VM'leri uygun değişkenler tarafından tanımlanan **SequenceStart** etiketiyle başlatır. Etiket değerleri ve değişken varlıklar hakkında daha fazla bilgi için [RunBook'a](#runbooks)bakın. İlgili zamanlama, **Sequenced-StopVM**etkinleştirin.

## <a name="use-of-the-solution-with-classic-vms"></a>Klasik VM'ler ile çözeltinin kullanımı

Klasik VM'ler için mesai dışı çözüm **sırasında Start/stop VM'lerini** kullanıyorsanız, Otomasyon tüm VM'lerinizi bulut hizmeti başına sırayla işler. VM'ler hala farklı bulut hizmetleri boyunca paralel olarak işlenir. 

Çözümün klasik VM'lerle kullanımı için, varsayılan olarak oluşturulmayan bir Klasik Çalıştır Hesabı gerekir. Klasik Çalıştır Hesabı oluşturma yönergeleri için [bkz.](automation-create-standalone-account.md#create-a-classic-run-as-account)

Bulut hizmeti başına 20'den fazla VM'niz varsa, bazı öneriler şunlardır:

* Ana çalışma kitabı **ScheduledStartStop_Parent** ve zamanlama başına 20 VM belirterek birden çok zamanlama oluşturun. 
* Zamanlama özelliklerinde, Virgülle ayrılmış bir liste olarak VM adlarını belirtmek için `VMList` parametreyi kullanın. 

Aksi takdirde, bu çözümün Otomasyon işi üç saatten fazla çalışıyorsa, geçici olarak boşaltılır veya [adil paylaşım](automation-runbook-execution.md#fair-share) sınırına göre durdurulur.

Azure CSP abonelikleri yalnızca Azure Kaynak Yöneticisi modelini destekler. Programda Azure Kaynak Yöneticisi olmayan hizmetler kullanılamaz. Mesai **dışı çözüm sırasında VM'leri Başlat/durdurun** çalıştığında, klasik kaynakları yönetmek için cmdlets olduğundan hata alabilirsiniz. CSP hakkında daha fazla bilgi edinmek için [CSP aboneliklerinde kullanılabilir hizmetlere](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services)bakın. Bir CSP aboneliği kullanıyorsanız, dağıtımdan sonra [External_EnableClassicVMs](#variables) değişkenini False olarak ayarlamanız gerekir.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="enable-the-solution"></a>Çözümü etkinleştirme

Çözümü kullanmaya başlamak için [VM'leri Başlat/durdur](automation-solution-vm-management-enable.md)çözümle'yi etkinleştir'deki adımları izleyin.

## <a name="view-the-solution"></a>Çözümü görüntüleme

Etkinleştirdikten sonra çözüme erişmek için aşağıdaki mekanizmalardan birini kullanın:

* Otomasyon hesabınızdan, **İlgili Kaynaklar**altında **Başlat/Durdur VM'yi** seçin. Başlat/Durdur VM sayfasında, **Başlat/Durdur VM Çözümlerini Yönet'in**altında sayfanın sağ tarafından **çözümü yönet'i** seçin.

* Otomasyon hesabınıza bağlı Günlük Analizi çalışma alanına gidin. Çalışma alanını seçtikten sonra sol bölmeden **Çözümler'i** seçin. Çözümler sayfasında, listeden **Start-Stop-VM[çalışma alanı]** çözümünü seçin.  

Çözümü seçmek, **Start-Stop-VM[çalışma alanı]** çözüm sayfasını görüntüler. Burada **StartStopVM** döşemesindeki bilgiler gibi önemli ayrıntıları gözden geçirebilirsiniz. Log Analytics çalışma alanınızda olduğu gibi, bu döşeme, başlatılan ve başarıyla tamamlanan çözüm için runbook işlerinin bir sayısını ve grafik gösterimini görüntüler.

![Otomasyon Güncelleme Yönetimi çözüm sayfası](media/automation-solution-vm-management/azure-portal-vmupdate-solution-01.png)

Donut karosu tıklayarak iş kayıtlarının daha fazla analizini gerçekleştirebilirsiniz. Çözüm panosu, iş geçmişini ve önceden tanımlanmış günlük arama sorgularını gösterir. Arama sorgularınıza göre arama yapmak için günlük analitiği gelişmiş portalına geçin.

## <a name="update-the-solution"></a>Çözümü güncelleştirme

Bu çözümün önceki bir sürümünü dağıttıysanız, güncelleştirilmiş bir sürüm dağıtmadan önce bu çözümü hesabınızdan silin. [Çözümü kaldırmak](#remove-the-solution) için adımları izleyin ve ardından [çözümü dağıtmak](automation-solution-vm-management-enable.md)için adımları izleyin.

## <a name="remove-the-solution"></a>Çözümü kaldırma

Çözümü artık kullanmanız gerekmiyorsa, çözümü Otomasyon hesabından silebilirsiniz. Çözümü siler, yalnızca runbook'ları kaldırır. Çözüm eklendiğinde oluşturulan zamanlamaları veya değişkenleri silmez. Diğer runbook'larla kullanmıyorsanız, bu varlıkları el ile kaldırın.

Çözümü silmek için:

1. Otomasyon hesabınızdan, **İlgili kaynaklar**altında Bağlantılı **çalışma alanını** seçin.

2. **Çalışma alanına git'i**seçin.

3. **Genel**altında **Çözümler'e** tıklayın. 

4. Çözümler sayfasında, **başlat-durdur-VM[Çalışma Alanı]** çözümünü seçin. 

5. **VMManagementSolution[Workspace]** sayfasında menüden **Sil'i** seçin.<br><br> ![VM Mgmt Çözümünü Sil](media/automation-solution-vm-management/vm-management-solution-delete.png)

6. Çözüm **Sil** penceresinde, çözümü silmek istediğinizi onaylayın.

7. Bilgiler doğrulanır ve çözüm silinir, **bildirimler**altında ilerleme izleyebilirsiniz , menüden seçilen. Çözümü kaldırmak için işlem başladıktan sonra Çözümler sayfasına döndürülür.

Otomasyon hesabı ve Log Analytics çalışma alanı bu işlemin bir parçası olarak silinmez. Log Analytics çalışma alanını korumak istemiyorsanız, azure portalından el ile silmeniz gerekir:

1. Günlük Analizi **çalışma alanlarını**arayın ve seçin.

2. Günlük Analitiği çalışma alanı sayfasında çalışma alanını seçin.

3. Çalışma alanı ayarları sayfasındaki menüden **Sil'i** seçin.

4. Azure Otomasyon hesap [çözüm bileşenlerini](#solution-components)saklamak istemiyorsanız, her birini el ile silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure VM'leriniz [için](automation-solution-vm-management-enable.md) mesai dışı çözüm **sırasında VM'leri başlat/durdurun.**
