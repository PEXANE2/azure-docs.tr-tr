---
title: Azure Automation’da Rol Tabanlı Erişim Denetimi
description: Rol tabanlı erişim denetimi (RBAC), Azure kaynakları için erişim yönetimi sağlar. Bu makalede, Azure Automation’da RBAC’nin nasıl ayarlanacağı açıklanmaktadır.
keywords: otomasyon rbac, rol tabanlı erişim denetimi, azure rbac
services: automation
ms.subservice: shared-capabilities
ms.date: 05/17/2018
ms.topic: conceptual
ms.openlocfilehash: a49f2596df91c44deafa1be83483f8972e223742
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81535579"
---
# <a name="role-based-access-control-in-azure-automation"></a>Azure Automation’da Rol Tabanlı Erişim Denetimi

Rol tabanlı erişim denetimi (RBAC), Azure kaynakları için erişim yönetimi sağlar. [RBAC'ı](../role-based-access-control/overview.md)kullanarak, ekibinizin görevlerini ayırabilir ve yalnızca işlerini gerçekleştirmek için gereken kullanıcılara, gruplara ve uygulamalara erişim miktarı verebilirsiniz. Azure portalını, Azure Komut Satırı araçlarını veya Azure Yönetim API'lerini kullanarak kullanıcılara rol tabanlı erişim izni verebilirsiniz.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="roles-in-automation-accounts"></a>Otomasyon hesaplarındaki Roller

Azure Automation’da, otomasyon hesabı kapsamında kullanıcılara, gruplara ve uygulamalara uygun RBAC rolü atanarak erişim verilir. Aşağıda Automation hesabının desteklediği yerleşik roller bulunmaktadır:

| **Rol** | **Açıklama** |
|:--- |:--- |
| Sahip |Sahip rolü, Otomasyon hesabını yönetmek için diğer kullanıcılara, gruplara ve uygulamalara erişim sağlamak da dahil olmak üzere bir Otomasyon hesabı içindeki tüm kaynaklara ve eylemlere erişim sağlar. |
| Katılımcı |Katılımcı rolü, başka kullanıcının Otomasyon hesabına erişim izinlerini değiştirme dışında her şeyi yönetmenizi sağlar. |
| Okuyucu |Okuyucu rolü, Otomasyon hesabında tüm kaynakları görmenizi sağlar; ancak değişiklik yapamazsınız. |
| Otomasyon Operatörü |Otomasyon Operatörü rolü, runbook adını ve özelliklerini görüntülemenize ve bir Otomasyon hesabındaki tüm runbook'lar için iş oluşturmanıza ve yönetmenize olanak tanır. Bu rol, kimlik bilgileri varlıkları ve runbook'lar gibi Otomasyon hesap kaynaklarınızı görüntülenmekten veya değiştirilmekten korumak, ancak yine de kuruluşunuzun üyelerinin bu runbook'ları yürütmesine izin vermek istiyorsanız yararlıdır. |
|Otomasyon İş Operatörü|Otomasyon İş Operatörü rolü, bir Otomasyon hesabındaki tüm runbook'lar için iş oluşturmanıza ve yönetmenize olanak tanır.|
|Otomasyon Runbook Operatörü|Otomasyon Runbook Operatörü rolü, bir runbook'un adını ve özelliklerini görüntülemenizi sağlar.|
| Log Analytics Katkıda Bulunan | Log Analytics Katılımcısı rolü, tüm izleme verilerini okumanızı ve izleme ayarlarını değiştirmenizi sağlar. İzleme ayarlarını düzenleme, VM'lere VM uzantısı eklemeyi, Azure depolamadan günlük toplamayı yapılandırabilmek için depolama hesabı anahtarlarını okumayı, Otomasyon hesapları oluşturmayı ve yapılandırmayı, çözümler eklemeyi ve Azure tanılamalarını tüm Azure kaynaklarında yapılandırmayı içerir.|
| Log Analytics Okuyucusu | Log Analytics Reader rolü, tüm izleme verilerini görüntülemenize ve aramanıza ve izleme ayarlarını görüntülemenize olanak tanır. Buna, Azure tanılama yapılandırmasının tüm Azure kaynaklarında görüntülenmesi de dahildir. |
| Katılımcıyı İzleme | İzleme Katılımcısı rolü, tüm izleme verilerini okumanızı ve izleme ayarlarını güncelleştirmenizi sağlar.|
| İzleme Okuyucu | İzleme Okuyucusu rolü tüm izleme verilerini okumanızı sağlar. |
| Kullanıcı Erişimi Yöneticisi |Kullanıcı Erişimi Yöneticisi rolü, Azure Otomasyonu hesaplarına kullanıcı erişimini yönetmenizi sağlar. |

## <a name="role-permissions"></a>Rol izinleri

Aşağıdaki tablolar, her role verilen belirli izinleri açıklar. Bu, izin veren Eylemleri ve bunları kısıtlayan NotActions'ı içerebilir.

### <a name="owner"></a>Sahip

Bir Sahibi erişim de dahil olmak üzere her şeyi yönetebilirsiniz. Aşağıdaki tabloda rol için verilen izinler gösterilmektedir:

|Eylemler|Açıklama|
|---|---|
|Microsoft.Automation/automationAccounts/|Her tür kaynak oluşturun ve yönetin.|

### <a name="contributor"></a>Katılımcı

Katılımcı erişim dışında her şeyi yönetebilir. Aşağıdaki tablo, rol için verilen ve reddedilen izinleri gösterir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/|Her türlü kaynak oluşturma ve yönetme|
|**Eylemler Değil**||
|Microsoft.Authorization/*/Delete| Rolleri ve rol atamalarını silin.       |
|Microsoft.Authorization/*/Yazma     |  Roller ve rol atamaları oluşturun.       |
|Microsoft.Authorization/yükseltmeAccess/Action    | Kullanıcı Erişim Yöneticisi oluşturma yeteneğini reddeder.       |

### <a name="reader"></a>Okuyucu

Okuyucu, Otomasyon hesabındaki tüm kaynakları görüntüleyebilir, ancak herhangi bir değişiklik yapamaz.

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/read|Otomasyon hesabındaki tüm kaynakları görüntüleyin. |

### <a name="automation-operator"></a>Otomasyon Operatörü

Bir Otomasyon Operatörü, bir Otomasyon hesabındaki tüm runbook adları ve özellikleri okuyabilir ve işleri yönetebilir.  Not: Operatörlerin tek tek runbook'lara erişimini denetlemek istiyorsanız bu rolü ayarlamayın ve bunun yerine birlikte 'Automation Job Operator' ve 'Automation Runbook Operator' rollerini kullanın. Aşağıdaki tabloda rol için verilen izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|Microsoft.Authorization/*/oku|Yetkilendirmeyi okuyun.|
|Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read|Karma Runbook İşçi Kaynakları'nı okuyun.|
|Microsoft.Automation/automationAccounts/jobs/read|Runbook'un işlerini listele.|
|Microsoft.Automation/automationAccounts/jobs/resume/action|Duraklatılmış bir işe devam edin.|
|Microsoft.Automation/automationAccounts/jobs/stop/action|Devam etmekte olan bir işi iptal edin.|
|Microsoft.Automation/automationAccounts/jobs/streams/read|İş Akışları ve Çıktı'yı okuyun.|
|Microsoft.Automation/automationAccounts/jobs/output/read|Bir işin çıktısını alın.|
|Microsoft.Automation/automationAccounts/jobs/suspend/action|Devam etmekte olan bir işi duraklatın.|
|Microsoft.Automation/automationAccounts/jobs/write|İş yaratın.|
|Microsoft.Automation/automationAccounts/jobSchedules/read|Azure Otomasyoniş programı alın.|
|Microsoft.Automation/automationAccounts/jobSchedules/write|Azure Otomasyoniş programı oluşturun.|
|Microsoft.Automation/automationAccounts/linkedWorkspace/read|Çalışma alanını Otomasyon hesabına bağla.|
|Microsoft.Automation/automationAccounts/read|Azure Otomasyon hesabı alın.|
|Microsoft.Automation/automationAccounts/runbooks/read|Azure Otomasyonu runbook'u alın.|
|Microsoft.Automation/automationAccounts/schedules/read|Azure Otomasyon zamanlama varlık alın.|
|Microsoft.Automation/automationAccounts/schedules/write|Azure Otomasyon zamanlama varlığı oluşturun veya güncelleştirin.|
|Microsoft.Resources/subscriptions/resourceGroups/read      |Rolleri ve rol ödevlerini okuyun.         |
|Microsoft.Resources/deployments/*      |Kaynak grubu dağıtımları oluşturun ve yönetin.         |
|Microsoft.Insights/alertRules/*      | Uyarı kuralları oluşturun ve yönetin.        |
|Microsoft.Support/* |Destek biletleri oluşturun ve yönetin.|

### <a name="automation-job-operator"></a>Otomasyon İş Operatörü

Otomasyon hesap kapsamında Bir Otomasyon İş Operatörü rolü verilir.Bu, operatöre hesaptaki tüm runbook'lar için iş oluşturma ve yönetme izinleri sağlar. Aşağıdaki tabloda rol için verilen izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|Microsoft.Authorization/*/oku|Yetkilendirmeyi okuyun.|
|Microsoft.Automation/automationAccounts/jobs/read|Runbook'un işlerini listele.|
|Microsoft.Automation/automationAccounts/jobs/resume/action|Duraklatılmış bir işe devam edin.|
|Microsoft.Automation/automationAccounts/jobs/stop/action|Devam etmekte olan bir işi iptal edin.|
|Microsoft.Automation/automationAccounts/jobs/streams/read|İş Akışları ve Çıktı'yı okuyun.|
|Microsoft.Automation/automationAccounts/jobs/suspend/action|Devam etmekte olan bir işi duraklatın.|
|Microsoft.Automation/automationAccounts/jobs/write|İş yaratın.|
|Microsoft.Resources/subscriptions/resourceGroups/read      |  Rolleri ve rol ödevlerini okuyun.       |
|Microsoft.Resources/deployments/*      |Kaynak grubu dağıtımları oluşturun ve yönetin.         |
|Microsoft.Insights/alertRules/*      | Uyarı kuralları oluşturun ve yönetin.        |
|Microsoft.Support/* |Destek biletleri oluşturun ve yönetin.|

### <a name="automation-runbook-operator"></a>Otomasyon Runbook Operatörü

Runbook kapsamında Bir Otomasyon Runbook Operatörü rolü verilir. Bir Otomasyon Runbook Operatörü runbook adını ve özelliklerini görüntüleyebilir.Bu rol, 'Otomasyon İş Operatörü' rolüyle birleştiğinde operatörün runbook için iş oluşturmasını ve yönetmesini sağlar. Aşağıdaki tabloda rol için verilen izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/runbooks/read     | Runbook'ları listele.        |
|Microsoft.Authorization/*/oku      | Yetkilendirmeyi okuyun.        |
|Microsoft.Resources/subscriptions/resourceGroups/read      |Rolleri ve rol ödevlerini okuyun.         |
|Microsoft.Resources/deployments/*      | Kaynak grubu dağıtımları oluşturun ve yönetin.         |
|Microsoft.Insights/alertRules/*      | Uyarı kuralları oluşturun ve yönetin.        |
|Microsoft.Support/*      | Destek biletleri oluşturun ve yönetin.        |

### <a name="log-analytics-contributor"></a>Log Analytics Katkıda Bulunan

Log Analytics Katılımcısı tüm izleme verilerini okuyabilir ve izleme ayarlarını düzeltebilir. İzleme ayarlarını düzenleme, VM uzantısını VM'lere eklemeyi içerir; Azure Depolama'dan günlük koleksiyonunu yapılandırabilmek için depolama hesabı anahtarlarını okuma; Otomasyon hesapları oluşturma ve yapılandırma; çözümler ekleme; ve Azure tanılamalarını tüm Azure kaynaklarında yapılandırma. Aşağıdaki tabloda rol için verilen izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|*/okumak|Sırlar hariç her türlü kaynağı okuyun.|
|Microsoft.Automation/automationAccounts/*|Otomasyon hesaplarını yönetin.|
|Microsoft.ClassicCompute/virtualMachines/uzantı/*|Sanal makine uzantıları oluşturun ve yönetin.|
|Microsoft.ClassicStorage/storageAccounts/listKeys/action|Klasik depolama hesabı anahtarlarını listele.|
|Microsoft.Compute/virtualMachines/extensions/*|Klasik sanal makine uzantıları oluşturun ve yönetin.|
|Microsoft.Insights/alertRules/*|Uyarı kurallarını okuma/yazma/silme.|
|Microsoft.Insights/diagnosticSettings/*|Tanı lama ayarlarını okuma/yazma/silme.|
|Microsoft.OperationalInsights/*|Azure Monitör günlüklerini yönetin.|
|Microsoft.OperationsManagement/*|Çalışma alanlarındaki çözümleri yönetin.|
|Microsoft.Resources/deployments/*|Kaynak grubu dağıtımları oluşturun ve yönetin.|
|Microsoft.Resources/subscriptions/resourcegroups/deployments/*|Kaynak grubu dağıtımları oluşturun ve yönetin.|
|Microsoft.Storage/storageAccounts/listKeys/action|Depolama hesabı anahtarlarını listele.|
|Microsoft.Support/*|Destek biletleri oluşturun ve yönetin.|

### <a name="log-analytics-reader"></a>Log Analytics Okuyucusu

Log Analytics Reader, Azure tanılama yapılandırmasını tüm Azure kaynaklarında görüntülemek de dahil olmak üzere tüm izleme verilerinin yanı sıra izleme ayarlarını görüntüleyebilir ve görüntüleyebilir. Aşağıdaki tablo, rol için verilen veya reddedilen izinleri gösterir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|*/okumak|Sırlar hariç her türlü kaynağı okuyun.|
|Microsoft.OperationalInsights/çalışma alanları/analitik/sorgu/eylem|Azure Monitor günlüklerinde sorguları yönetin.|
|Microsoft.OperationalInsights/çalışma alanları/arama/eylem|Azure Monitor günlük verilerini arayın.|
|Microsoft.Support/*|Destek biletleri oluşturun ve yönetin.|
|**Eylemler Değil**| |
|Microsoft.OperationalInsights/çalışma alanları/sharedKeys/read|Paylaşılan erişim anahtarlarını okuyamaz.|

### <a name="monitoring-contributor"></a>Katılımcıyı İzleme

Bir İzleme Katılımcısı tüm izleme verilerini okuyabilir ve izleme ayarlarını güncelleyebilir. Aşağıdaki tabloda rol için verilen izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|*/okumak|Sırlar hariç her türlü kaynağı okuyun.|
|Microsoft.AlertsManagement/alerts/*|Uyarıları yönetin.|
|Microsoft.AlertsManagement/alertsSummary/*|Uyarı panosunu yönetin.|
|Microsoft.Insights/AlertRules/*|Uyarı kurallarını yönetin.|
|Microsoft.Insights/components/*|Uygulama Öngörüleri bileşenlerini yönetin.|
|Microsoft.Insights/DiagnosticSettings/*|Tanılama ayarlarını yönetin.|
|Microsoft.Insights/eventtypes/*|Bir abonelikteki Etkinlik Günlüğü olaylarını (yönetim olaylarını) listele. Bu izin, Etkinlik Günlüğü'ne hem programatik hem de portal erişimi için geçerlidir.|
|Microsoft.Insights/LogDefinitions/*|Bu izin, portal üzerinden Etkinlik Günlükleri'ne erişmesi gereken kullanıcılar için gereklidir. Günlük kategorilerini Etkinlik Günlüğü'nde listeleyin.|
|Microsoft.Insights/MetricDefinitions/*|Metrik tanımları okuyun (kaynak için kullanılabilir metrik türlerinin listesi).|
|Microsoft.Insights/Metrics/*|Kaynak için ölçümleri okuyun.|
|Microsoft.Insights/Register/Action|Microsoft.Insights sağlayıcısını kaydedin.|
|Microsoft.Insights/webtests/*|Uygulama Öngörüleri web testlerini yönetin.|
|Microsoft.OperationalInsights/çalışma alanları/intelligencepacks/*|Azure Monitor günlükleri çözüm paketlerini yönetin.|
|Microsoft.OperationalInsights/çalışma alanları/savedSearches/*|Azure Monitor günlüklerini yönetin aramaları kaydetti.|
|Microsoft.OperationalInsights/çalışma alanları/arama/eylem|Günlük Analizi çalışma alanlarını arayın.|
|Microsoft.OperationalInsights/workspaces/sharedKeys/action|Log Analytics çalışma alanı için liste anahtarları.|
|Microsoft.OperationalInsights/çalışma alanları/storageinsightconfigs/*|Azure Monitor günlükleri depolama içgörü yapılandırmalarını yönetin.|
|Microsoft.Support/*|Destek biletleri oluşturun ve yönetin.|
|Microsoft.WorkloadMonitor/iş yükleri/*|İş Yüklerini Yönetin.|

### <a name="monitoring-reader"></a>İzleme Okuyucu

İzleme Okuyucusu tüm izleme verilerini okuyabilir. Aşağıdaki tabloda rol için verilen izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|*/okumak|Sırlar hariç her türlü kaynağı okuyun.|
|Microsoft.OperationalInsights/çalışma alanları/arama/eylem|Günlük Analizi çalışma alanlarını arayın.|
|Microsoft.Support/*|Destek biletleri oluşturma ve yönetme|

### <a name="user-access-administrator"></a>Kullanıcı Erişimi Yöneticisi

Kullanıcı Erişim Yöneticisi, Kullanıcı'nın Azure kaynaklarına erişimini yönetebilir. Aşağıdaki tabloda rol için verilen izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|*/okumak|Tüm kaynakları okuyun|
|Microsoft.Authorization/*|Yetkilendirmeyi yönetme|
|Microsoft.Support/*|Destek biletleri oluşturma ve yönetme|

## <a name="onboarding-permissions"></a>Onboarding izinleri

Aşağıdaki bölümlerde, değişiklik izleme veya güncelleştirme yönetimi çözümleri için sanal makinelere binmek için gereken minimum izinler açıklanır.

### <a name="permissions-for-onboarding-from-a-vm"></a>VM'den binme izinleri

|**Eylem**  |**İzin**  |**Minimum kapsam**  |
|---------|---------|---------|
|Yeni dağıtım yazma      | Microsoft.Resources/deployments/*          |Abonelik          |
|Yeni kaynak grubu yazma      | Microsoft.Resources/subscriptions/resourceGroups/write        | Abonelik          |
|Yeni varsayılan Çalışma Alanı oluşturma      | Microsoft.OperationalInsights/çalışma alanları/yazma         | Kaynak grubu         |
|Yeni Hesap Oluşturma      |  Microsoft.Automation/automationAccounts/write        |Kaynak grubu         |
|Çalışma alanını ve hesabı bağlama      |Microsoft.OperationalInsights/çalışma alanları/yazma</br>Microsoft.Automation/automationAccounts/read|Çalışma alanı</br>Otomasyon hesabı
|MMA uzantısı oluşturma      | Microsoft.Compute/virtualMachines/write         | Sanal Makine         |
|Kaydedilen arama oluşturma      | Microsoft.OperationalInsights/çalışma alanları/yazma          | Çalışma alanı         |
|Kapsam config oluşturma      | Microsoft.OperationalInsights/çalışma alanları/yazma          | Çalışma alanı         |
|Onboarding durum denetimi - Çalışma alanını okuyun      | Microsoft.OperationalInsights/çalışma alanları/okuma         | Çalışma alanı         |
|Onboarding durum denetimi - Hesabın bağlantılı çalışma alanı özelliğini okuyun     | Microsoft.Automation/automationAccounts/read      | Otomasyon hesabı        |
|Onboarding durum denetimi - Çözüm oku      | Microsoft.OperationalInsights/çalışma alanları/zeka paketleri/okuma          | Çözüm         |
|Onboarding durum denetimi - VM oku      | Microsoft.Compute/virtualMachines/read         | Sanal Makine         |
|Onboarding durum denetimi - Hesabı oku      | Microsoft.Automation/automationAccounts/read  |  Otomasyon hesabı   |
| VM<sup>1</sup> için onboarding çalışma alanı denetimi       | Microsoft.OperationalInsights/çalışma alanları/okuma         | Abonelik         |
| Log Analytics sağlayıcısını kaydedin |Microsoft.Insights/register/action | Abonelik|

<sup>1</sup> VM portal deneyimi aracılığıyla gemiye binmek için bu izin gereklidir.

### <a name="permissions-for-onboarding-from-automation-account"></a>Otomasyon hesabından biniş için izinler

|**Eylem**  |**İzin** |**Minimum Kapsam**  |
|---------|---------|---------|
|Yeni dağıtım oluşturma     | Microsoft.Resources/deployments/*        | Abonelik         |
|Yeni kaynak grubu oluşturma     | Microsoft.Resources/subscriptions/resourceGroups/write         | Abonelik        |
|AutomationOnboarding blade - Yeni çalışma alanı oluşturun     |Microsoft.OperationalInsights/çalışma alanları/yazma           | Kaynak grubu        |
|AutomationOnboarding blade - bağlantılı çalışma alanını okuyun     | Microsoft.Automation/automationAccounts/read        | Otomasyon hesabı       |
|AutomationOnboarding blade - çözüm oku     | Microsoft.OperationalInsights/çalışma alanları/zeka paketleri/okuma         | Çözüm        |
|AutomationOnboarding blade - okuma çalışma alanı     | Microsoft.OperationalInsights/çalışma alanları/zeka paketleri/okuma        | Çalışma alanı        |
|Çalışma alanı ve Hesap için bağlantı oluşturma     | Microsoft.OperationalInsights/çalışma alanları/yazma        | Çalışma alanı        |
|Ayakkabı kutusu için hesap yaz      | Microsoft.Automation/automationAccounts/write        | Hesap        |
|Kaydedilen aramayı oluşturma/değiştirme     | Microsoft.OperationalInsights/çalışma alanları/yazma        | Çalışma alanı        |
|Kapsam config oluşturma/değiştirme     | Microsoft.OperationalInsights/çalışma alanları/yazma        | Çalışma alanı        |
| Log Analytics sağlayıcısını kaydedin |Microsoft.Insights/register/action | Abonelik|
|**Adım 2 - Onboard Çoklu VM'ler**     |         |         |
|VMOnboarding blade - MMA uzantısı oluştur     | Microsoft.Compute/virtualMachines/write           | Sanal Makine        |
|Kaydedilmiş arama oluşturma / değiştirme     | Microsoft.OperationalInsights/çalışma alanları/yazma           | Çalışma alanı        |
|Oluşturma / kapsam config edin  | Microsoft.OperationalInsights/çalışma alanları/yazma   | Çalışma alanı|

## <a name="update-management-permissions"></a>Yönetim izinlerini güncelleştirme

Güncelleştirme yönetimi, hizmetini sağlamak için birden çok hizmete ulaşır. Aşağıdaki tablo, güncelleştirme yönetimi dağıtımlarını yönetmek için gereken izinleri gösterir:

|**Kaynak**  |**Rol**  |**Kapsam**  |
|---------|---------|---------|
|Otomasyon hesabı     | Log Analytics Katkıda Bulunan       | Otomasyon hesabı        |
|Otomasyon hesabı    | Sanal Makine Katılımcısı        | Hesap için Kaynak Grubu        |
|Log Analytics çalışma alanı     | Log Analytics Katkıda Bulunan| Log Analytics çalışma alanı        |
|Log Analytics çalışma alanı |Log Analytics Okuyucusu| Abonelik|
|Çözüm     |Log Analytics Katkıda Bulunan         | Çözüm|
|Sanal Makine     | Sanal Makine Katılımcısı        | Sanal Makine        |

## <a name="configure-rbac-for-your-automation-account"></a>Otomasyon hesabınız için RBAC'ı yapılandırın

Aşağıdaki bölümde, [Azure portalı](#configure-rbac-using-the-azure-portal) ve PowerShell aracılığıyla Otomasyon hesabınızda RBAC'ı nasıl yapılandırabileceğiniz [gösterilmektedir.](#configure-rbac-using-powershell)

### <a name="configure-rbac-using-the-azure-portal"></a>Azure portalını kullanarak RBAC'ı yapılandırma

1. [Azure portalında](https://portal.azure.com/) oturum açın Otomasyon Hesapları sayfasından Otomasyon hesabınızı açın.
2. Access **denetimi (IAM)** sayfasını açmak için Access denetimi 'ni (IAM) tıklatın. Bu sayfayı, Otomasyon hesabınızı yönetmek ve Otomasyon hesabı için yapılandırılabilen mevcut rolleri görüntülemek için yeni kullanıcılar, gruplar ve uygulamalar eklemek için kullanabilirsiniz.
3. **Rol atamaları** sekmesine tıklayın.

   ![Erişim düğmesi](media/automation-role-based-access-control/automation-01-access-button.png)

#### <a name="add-a-new-user-and-assign-a-role"></a>Yeni kullanıcı ekleme ve rol atama

1. Access denetimi (IAM) sayfasından **+ Rol ataması ekle'yi**tıklatın. Bu eylem, kullanıcı, grup veya uygulama ekleyebileceğiniz ve karşılık gelen bir rol atayabileceğiniz rol atama ekle sayfasını açar.

2. Kullanılabilir roller listesinden bir rol seçin. Otomasyon hesabının desteklediği kullanılabilir yerleşik rollerden herhangi birini veya tanımlamış olabileceğiniz herhangi bir özel rolü seçebilirsiniz.

3. **Seç** alanına izin vermek istediğiniz kullanıcının adını yazın. Listeden kullanıcıyı seçin ve **Kaydet'i**tıklatın.

   ![Kullanıcı ekle](media/automation-role-based-access-control/automation-04-add-users.png)

   Şimdi, seçilen rolün atanmış olduğu Kullanıcılar sayfasına eklenen kullanıcıyı görmeniz gerekir.

   ![Kullanıcıları listele](media/automation-role-based-access-control/automation-05-list-users.png)

   Kullanıcıya Roller sayfasından rol atayabilirsiniz.
4. Roller sayfasını açmak için Access denetimi (IAM) sayfasından **Görevler'i** tıklatın. Rolün adını ve bu role atanan kullanıcı ve grup sayısını görüntüleyebilirsiniz.

    ![Kullanıcılar sayfasından rol atama](media/automation-role-based-access-control/automation-06-assign-role-from-users-blade.png)

   > [!NOTE]
   > Yalnızca Otomasyon hesabı kapsamında rol tabanlı erişim denetimi ayarlayabilirsiniz, Otomasyon hesabının altındaki herhangi bir kaynakta değil.

#### <a name="remove-a-user"></a>Kullanıcıyı kaldırma

Otomasyon hesabını yönetmeyen veya artık kuruluş için çalışmayan bir kullanıcının erişim iznini kaldırabilirsiniz. Aşağıdaki adımları kullanarak kullanıcı kaldırabilirsiniz:

1. Access denetimi (IAM) sayfasından, kaldırmak için kullanıcıyı seçin ve **Kaldır'ı**tıklatın.
2. Atama ayrıntıları sayfasında **Kaldır** düğmesine tıklayın.
3. Kaldırmayı onaylamak için **Evet**'e tıklayın.

   ![Kullanıcıları kaldır](media/automation-role-based-access-control/automation-08-remove-users.png)

### <a name="configure-rbac-using-powershell"></a>PowerShell kullanarak RBAC'ı yapılandırın

Ayrıca, aşağıdaki [Azure PowerShell cmdlets'i](../role-based-access-control/role-assignments-powershell.md)kullanarak bir Otomasyon hesabına rol tabanlı erişimi de yapılandırabilirsiniz:

[Get-AzRoleDefinition,](https://docs.microsoft.com/powershell/module/Az.Resources/Get-AzRoleDefinition?view=azps-3.7.0) Azure Etkin Dizini'nde bulunan tüm RBAC rollerini listeler. Belirli bir rolün `Name` gerçekleştirebileceği tüm eylemleri listelemek için parametreli bu cmdlet'i kullanabilirsiniz.

```azurepowershell-interactive
Get-AzRoleDefinition -Name 'Automation Operator'
```

Örnek çıktı aşağıda verilmiştir:

```azurepowershell
Name             : Automation Operator
Id               : d3881f73-407a-4167-8283-e981cbba0404
IsCustom         : False
Description      : Automation Operators are able to start, stop, suspend, and resume jobs
Actions          : {Microsoft.Authorization/*/read, Microsoft.Automation/automationAccounts/jobs/read, Microsoft.Automation/automationAccounts/jobs/resume/action,
                   Microsoft.Automation/automationAccounts/jobs/stop/action...}
NotActions       : {}
AssignableScopes : {/}
```

[Get-AzRoleAssignment,](https://docs.microsoft.com/powershell/module/az.resources/get-azroleassignment?view=azps-3.7.0) belirtilen kapsamda Azure AD RBAC rol atamalarını listeler. Herhangi bir parametre olmadan, bu cmdlet abonelik altında yapılan tüm rol atamaları döndürür. Belirtilen `ExpandPrincipalGroups` kullanıcının erişim atamalarını ve kullanıcının ait olduğu grupları listelemek için parametreyi kullanın.

**Örnek:** Otomasyon hesabındaki tüm kullanıcıları ve rollerini listelemek için aşağıdaki cmdlet'i kullanın.

```azurepowershell-interactive
Get-AzRoleAssignment -Scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

Örnek çıktı aşağıda verilmiştir:

```powershell
RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Automation/automationAccounts/myAutomationAccount/provid
                     ers/Microsoft.Authorization/roleAssignments/cc594d39-ac10-46c4-9505-f182a355c41f
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Automation/automationAccounts/myAutomationAccount
DisplayName        : admin@contoso.com
SignInName         : admin@contoso.com
RoleDefinitionName : Automation Operator
RoleDefinitionId   : d3881f73-407a-4167-8283-e981cbba0404
ObjectId           : 15f26a47-812d-489a-8197-3d4853558347
ObjectType         : User
```

Kullanıcılara, gruplara ve uygulamalara belirli bir kapsama erişim atamak için [Yeni-AzRoleAssignment'ı](https://docs.microsoft.com/powershell/module/Az.Resources/New-AzRoleAssignment?view=azps-3.7.0) kullanın.
    
**Örnek:** Otomasyon hesabı kapsamındaki bir kullanıcıya "Otomasyon Operatörü" rolünü atamak için aşağıdaki komutu kullanın.

```azurepowershell-interactive
New-AzRoleAssignment -SignInName <sign-in Id of a user you wish to grant access> -RoleDefinitionName 'Automation operator' -Scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

Örnek çıktı aşağıda verilmiştir:

```azurepowershell
RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/myResourceGroup/Providers/Microsoft.Automation/automationAccounts/myAutomationAccount/provid
                     ers/Microsoft.Authorization/roleAssignments/25377770-561e-4496-8b4f-7cba1d6fa346
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/myResourceGroup/Providers/Microsoft.Automation/automationAccounts/myAutomationAccount
DisplayName        : admin@contoso.com
SignInName         : admin@contoso.com
RoleDefinitionName : Automation Operator
RoleDefinitionId   : d3881f73-407a-4167-8283-e981cbba0404
ObjectId           : f5ecbe87-1181-43d2-88d5-a8f5e9d8014e
ObjectType         : User
```

Belirli bir kullanıcının, grubun veya uygulamanın belirli bir kapsamdan erişimini kaldırmak için [Kaldır-AzRoleAssignment'ı](https://docs.microsoft.com/powershell/module/Az.Resources/Remove-AzRoleAssignment?view=azps-3.7.0) kullanın.

**Örnek:** Kullanıcıyı Otomasyon hesabı kapsamındaki Otomasyon Operatörü rolünden kaldırmak için aşağıdaki komutu kullanın.

```azurepowershell-interactive
Remove-AzRoleAssignment -SignInName <sign-in Id of a user you wish to remove> -RoleDefinitionName 'Automation Operator' -Scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

Önceki örnekte, hesap `sign-in ID of a user you wish to remove` `SubscriptionID`ayrıntılarınızı `Resource Group Name` `Automation account name` değiştirin. Kullanıcı rol atamalarını kaldırmaya devam etmeden önce onaylamak istendiğinde **evet'i** seçin.

### <a name="user-experience-for-automation-operator-role---automation-account"></a>Otomasyon Operatörü rolü için kullanıcı deneyimi - Otomasyon hesabı

Otomasyon hesabı kapsamında Otomasyon Operatörü rolüne atanan bir kullanıcı, kendisine atandığı Otomasyon hesabını görüntülediğinde, kullanıcı yalnızca Otomasyon hesabında oluşturulan runbook'ların, runbook işlerinin ve zamanlamaların listesini görüntüleyebilir. Bu kullanıcı bu öğelerin tanımlarını görüntüleyemez. Kullanıcı runbook işini başlatabilir, durdurabilir, askıya alabilir, devam ettirebilir veya zamanlayabilir. Ancak, kullanıcıyapılandırmalar, karma alt gruplar veya DSC düğümleri gibi diğer Otomasyon kaynaklarına erişimi yoktur.

![Kaynaklara erişim yok](media/automation-role-based-access-control/automation-10-no-access-to-resources.png)

## <a name="configure-rbac-for-runbooks"></a>Runbook'lar için RBAC'ı yapılandırma

Azure Otomasyonu, RBAC'ı belirli runbook'lara atamanızı sağlar. Bunu yapmak için, belirli bir runbook'a bir kullanıcı eklemek için aşağıdaki komut dosyasını çalıştırın. Bir Otomasyon Hesabı Yöneticisi veya Kiracı Yöneticisi bu komut dosyasini çalıştırabilir.

```azurepowershell-interactive
$rgName = "<Resource Group Name>" # Resource Group name for the Automation account
$automationAccountName ="<Automation account name>" # Name of the Automation account
$rbName = "<Name of Runbook>" # Name of the runbook
$userId = "<User ObjectId>" # Azure Active Directory (AAD) user's ObjectId from the directory

# Gets the Automation account resource
$aa = Get-AzResource -ResourceGroupName $rgName -ResourceType "Microsoft.Automation/automationAccounts" -ResourceName $automationAccountName

# Get the Runbook resource
$rb = Get-AzResource -ResourceGroupName $rgName -ResourceType "Microsoft.Automation/automationAccounts/runbooks" -ResourceName "$automationAccountName/$rbName"

# The Automation Job Operator role only needs to be run once per user.
New-AzRoleAssignment -ObjectId $userId -RoleDefinitionName "Automation Job Operator" -Scope $aa.ResourceId

# Adds the user to the Automation Runbook Operator role to the Runbook scope
New-AzRoleAssignment -ObjectId $userId -RoleDefinitionName "Automation Runbook Operator" -Scope $rb.ResourceId
```

Komut dosyası çalıştırıladıktan sonra, kullanıcının Azure portalında oturum açmasını ve **Tüm Kaynaklar'ı**seçmesini istiyorum. Listede, kullanıcı otomasyon runbook Operatörü olarak eklendiği runbook'u görebilir.

![Runbook RBAC portalda](./media/automation-role-based-access-control/runbook-rbac.png)

### <a name="user-experience-for-automation-operator-role---runbook"></a>Otomasyon operatörü rolü için kullanıcı deneyimi - Runbook

Runbook kapsamındaki Otomasyon Operatörü rolüne atanan bir kullanıcı atanmış bir runbook'u görüntülediğinde, kullanıcı yalnızca runbook'u başlatabilir ve runbook işlerini görüntüleyebilir.

![Yalnızca başlangıç için erişimi vardır](media/automation-role-based-access-control/automation-only-start.png)

## <a name="next-steps"></a>Sonraki adımlar

* Azure Otomasyonu için RBAC'ı yapılandırma yolları hakkında bilgi için Azure [PowerShell ile RBAC'ı yönetmeye](../role-based-access-control/role-assignments-powershell.md)bakın.
* Runbook başlatma yolları hakkında ayrıntılı bilgi [için](automation-starting-a-runbook.md)bkz.
* Runbook türleri hakkında bilgi için [Azure Otomasyon runbook türlerine](automation-runbook-types.md)bakın.