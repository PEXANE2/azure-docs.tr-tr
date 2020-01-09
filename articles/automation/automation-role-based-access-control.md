---
title: Azure Otomasyonu’nda rol tabanlı erişim denetimi
description: Rol tabanlı erişim denetimi (RBAC), Azure kaynakları için erişim yönetimi sağlar. Bu makalede, Azure Automation’da RBAC’nin nasıl ayarlanacağı açıklanmaktadır.
keywords: otomasyon rbac, rol tabanlı erişim denetimi, azure rbac
services: automation
ms.subservice: shared-capabilities
ms.date: 05/17/2018
ms.topic: conceptual
ms.openlocfilehash: 122e1f69e952acc00aba3cad2d75cb87b8fd08ee
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75421583"
---
# <a name="role-based-access-control-in-azure-automation"></a>Azure Otomasyonu’nda rol tabanlı erişim denetimi

Rol tabanlı erişim denetimi (RBAC), Azure kaynakları için erişim yönetimi sağlar. [RBAC](../role-based-access-control/overview.md)kullanarak, ekip dahilinde görevleri ayırabilirsiniz ve yalnızca işlerini gerçekleştirmek için ihtiyaç duydukları kullanıcılara, gruplara ve uygulamalara erişim miktarını verebilirsiniz. Kullanıcılara rol tabanlı erişim Azure portalı, Azure Komut Satırı araçları ve Azure Management API'leri kullanılarak verilebilir.

## <a name="roles-in-automation-accounts"></a>Otomasyon hesaplarındaki roller

Azure Automation’da, otomasyon hesabı kapsamında kullanıcılara, gruplara ve uygulamalara uygun RBAC rolü atanarak erişim verilir. Aşağıda Automation hesabının desteklediği yerleşik roller bulunmaktadır:

| **Rol** | **Açıklama** |
|:--- |:--- |
| Sahip |Sahip rolü, Otomasyon hesabını yönetmek için diğer kullanıcılara, gruplara ve uygulamalara erişim sağlamak dahil olmak üzere Otomasyon hesabı içindeki tüm kaynaklara ve eylemlere erişim sağlar. |
| Katılımcı |Katılımcı rolü, başka kullanıcının Otomasyon hesabına erişim izinlerini değiştirme dışında her şeyi yönetmenizi sağlar. |
| Okuyucu |Okuyucu rolü, Otomasyon hesabında tüm kaynakları görmenizi sağlar; ancak değişiklik yapamazsınız. |
| Otomasyon Operatörü |Otomasyon Işletmeni rolü, runbook adını ve özelliklerini görüntülemenize ve bir Otomasyon hesabındaki tüm runbook 'lar için iş oluşturmanıza ve yönetmenize olanak sağlar. Bu rol, kimlik bilgileri varlıkları ve runbook'ları gibi Automation hesabı kaynaklarınızın görüntülenmesini veya değiştirilmesini engellemek, ancak yine de kuruluş üyelerinin bu runbook’ları yürütmesine izin vermek istiyorsanız yararlıdır. |
|Automation Iş Işleci|Otomasyon Iş Işletmeni rolü, bir Otomasyon hesabındaki tüm runbook 'lar için iş oluşturmanıza ve yönetmenize olanak sağlar.|
|Otomasyon Runbook Işleci|Otomasyon Runbook Işleci rolü, bir runbook 'un adını ve özelliklerini görüntülemenize izin verir.|
| Log Analytics Katkıda Bulunan | Log Analytics katkıda bulunan rolü, tüm izleme verilerini okumanızı ve izleme ayarlarını düzenlemenizi sağlar. İzleme ayarlarını Düzenle, VM 'Lerin sanal makinelere eklenmesini, Azure depolama 'dan günlüklerin toplanmasını yapılandırabilmek için depolama hesabı anahtarlarını okumayı, Otomasyon hesapları oluşturmayı ve yapılandırmayı, çözümleri eklemeyi ve üzerinde Azure tanılamayı yapılandırmayı içerir Tüm Azure kaynakları.|
| Log Analytics Okuyucusu | Log Analytics okuyucu rolü, tüm izleme verilerini görüntülemenize ve aramanıza ve izleme ayarlarını görüntülemenize olanak sağlar. Bu, tüm Azure kaynaklarında Azure tanılama 'nın yapılandırılmasını görüntülemeyi içerir. |
| Katkıda bulunan izleniyor | Izleme katılımcısı rolü, tüm izleme verilerini okumanızı ve izleme ayarlarını güncelleştirmenizi sağlar.|
| İzleme okuyucusu | Izleme okuyucu rolü, tüm izleme verilerini okumanızı sağlar. |
| Kullanıcı Erişimi Yöneticisi |Kullanıcı Erişimi Yöneticisi rolü, Azure Otomasyonu hesaplarına kullanıcı erişimini yönetmenizi sağlar. |

## <a name="role-permissions"></a>Rol izinleri

Aşağıdaki tablolar her role verilen belirli izinleri anlatmaktadır. Bu, izinleri veren eylemleri ve bunları kısıtlayan NotActions öğesini içerebilir.

### <a name="owner"></a>Sahip

Bir sahip, erişim dahil her şeyi yönetebilir. Aşağıdaki tabloda rol için verilen izinler gösterilmektedir:

|Eylemler|Açıklama|
|---|---|
|Microsoft. Automation/automationAccounts/|Tüm türlerin kaynaklarını oluşturun ve yönetin.|

### <a name="contributor"></a>Katılımcı

Katkıda bulunan, erişim dışında her şeyi yönetebilir. Aşağıdaki tabloda rol için verilen ve Reddedilen izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|Microsoft. Automation/automationAccounts/|Tüm türlerin kaynaklarını oluşturma ve yönetme|
|**Eylem değil**||
|Microsoft. Authorization/*/Delete| Rolleri ve rol atamalarını silin.       |
|Microsoft. Authorization/*/Write     |  Roller ve rol atamaları oluşturun.       |
|Microsoft.Authorization/elevateAccess/Action    | Kullanıcı Erişim Yöneticisi oluşturma özelliğini reddeder.       |

### <a name="reader"></a>Okuyucu

Okuyucu bir Otomasyon hesabındaki tüm kaynakları görüntüleyebilir, ancak herhangi bir değişiklik yapamaz.

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|Microsoft. Automation/automationAccounts/Read|Otomasyon hesabındaki tüm kaynakları görüntüleyin. |

### <a name="automation-operator"></a>Otomasyon Operatörü

Otomasyon operatörü işleri oluşturabilir ve yönetebilir ve bir Otomasyon hesabındaki tüm runbook 'lar için Runbook adlarını ve özellikleri okuyabilir.  Note: tek tek runbook 'lara yönelik operatör erişimini denetlemek istiyorsanız bu rolü ayarlamazsanız ve bunun yerine ' Automation Iş Işleci ' ve ' Otomasyon Runbook Işleci ' rollerini birlikte kullanın. Aşağıdaki tabloda rol için verilen izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|Microsoft. Authorization/*/Read|Yetkilendirmeyi okuyun.|
|Microsoft. Automation/automationAccounts/hybridRunbookWorkerGroups/Read|Karma runbook çalışanı kaynaklarını okuyun.|
|Microsoft. Automation/automationAccounts/Jobs/okuma|Runbook 'un işlerini listeleyin.|
|Microsoft. Automation/automationAccounts/Jobs/sürdürülecek/Action|Duraklatılmış bir işi sürdürür.|
|Microsoft. Automation/automationAccounts/Jobs/durdur/eylem|Sürmekte olan bir işi iptal edin.|
|Microsoft. Automation/automationAccounts/Jobs/akışlar/okuma|Iş akışlarını ve çıktıyı okuyun.|
|Microsoft. Automation/automationAccounts/Jobs/output/Read|Bir işin çıkışını alın.|
|Microsoft. Automation/automationAccounts/Jobs/beklet/eylem|Sürmekte olan bir işi duraklatın.|
|Microsoft. Automation/automationAccounts/Jobs/Write|İş oluşturun.|
|Microsoft. Automation/automationAccounts/Jobzamanlamalar/okuma|Azure Otomasyonu iş zamanlaması alın.|
|Microsoft.Automation/automationAccounts/jobSchedules/write|Azure Otomasyonu iş zamanlaması oluşturma.|
|Microsoft. Automation/automationAccounts/linkedWorkspace/Read|Otomasyon hesabına bağlı çalışma alanını alın.|
|Microsoft. Automation/automationAccounts/Read|Azure Otomasyonu hesabı alın.|
|Microsoft. Automation/automationAccounts/runbook 'lar/okuma|Azure Otomasyonu runbook 'u alın.|
|Microsoft. Automation/automationAccounts/zamanlamalar/okuma|Azure Otomasyonu zamanlama varlığı alın.|
|Microsoft. Automation/automationAccounts/zamanlamalar/yazma|Azure Otomasyonu zamanlama varlığı oluşturun veya güncelleştirin.|
|Microsoft. resources/abonelikler/resourceGroups/Read      |Rolleri ve rol atamalarını okuyun.         |
|Microsoft. resources/dağıtımlar/*      |Kaynak grubu dağıtımlarını oluşturun ve yönetin.         |
|Microsoft.Insights/alertRules/*      | Uyarı kuralları oluşturun ve yönetin.        |
|Microsoft. support/* |Destek biletleri oluşturun ve yönetin.|

### <a name="automation-job-operator"></a>Automation Iş Işleci

Otomasyon hesabı kapsamında bir Otomasyon Işi operatörü rolü verilir. Bu, İşletmen izinlerinin hesaptaki tüm runbook 'lar için iş oluşturmasına ve yönetmesine izin verir. Aşağıdaki tabloda rol için verilen izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|Microsoft. Authorization/*/Read|Yetkilendirmeyi okuyun.|
|Microsoft. Automation/automationAccounts/Jobs/okuma|Runbook 'un işlerini listeleyin.|
|Microsoft. Automation/automationAccounts/Jobs/sürdürülecek/Action|Duraklatılmış bir işi sürdürür.|
|Microsoft. Automation/automationAccounts/Jobs/durdur/eylem|Sürmekte olan bir işi iptal edin.|
|Microsoft. Automation/automationAccounts/Jobs/akışlar/okuma|Iş akışlarını ve çıktıyı okuyun.|
|Microsoft. Automation/automationAccounts/Jobs/beklet/eylem|Sürmekte olan bir işi duraklatın.|
|Microsoft. Automation/automationAccounts/Jobs/Write|İş oluşturun.|
|Microsoft. resources/abonelikler/resourceGroups/Read      |  Rolleri ve rol atamalarını okuyun.       |
|Microsoft. resources/dağıtımlar/*      |Kaynak grubu dağıtımlarını oluşturun ve yönetin.         |
|Microsoft.Insights/alertRules/*      | Uyarı kuralları oluşturun ve yönetin.        |
|Microsoft. support/* |Destek biletleri oluşturun ve yönetin.|

### <a name="automation-runbook-operator"></a>Otomasyon Runbook Işleci

Runbook kapsamında bir Otomasyon Runbook Işleç rolü verilir. Bir Automation runbook Işleci, runbook 'un adını ve özelliklerini görüntüleyebilir.  ' Automation Iş Işleci ' rolüyle birleştirilmiş bu rol, işlecin runbook için işler oluşturmasına ve yönetmesine olanak sağlar. Aşağıdaki tabloda rol için verilen izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|Microsoft. Automation/automationAccounts/runbook 'lar/okuma     | Runbook 'ları listeleyin.        |
|Microsoft. Authorization/*/Read      | Yetkilendirmeyi okuyun.        |
|Microsoft. resources/abonelikler/resourceGroups/Read      |Rolleri ve rol atamalarını okuyun.         |
|Microsoft. resources/dağıtımlar/*      | Kaynak grubu dağıtımlarını oluşturun ve yönetin.         |
|Microsoft.Insights/alertRules/*      | Uyarı kuralları oluşturun ve yönetin.        |
|Microsoft. support/*      | Destek biletleri oluşturun ve yönetin.        |

### <a name="log-analytics-contributor"></a>Log Analytics Katkıda Bulunan

Log Analytics katkıda bulunan, tüm izleme verilerini okuyabilir ve izleme ayarlarını düzenleyebilir. İzleme ayarlarını düzenlediğinizde VM 'lere VM uzantısının eklenmesi dahildir; Azure depolama 'dan günlüklerin toplanmasını yapılandırabilmek için depolama hesabı anahtarlarını okuma; Otomasyon hesapları oluşturma ve yapılandırma; çözümler ekleme; ve Azure tanılama 'yı tüm Azure kaynaklarında yapılandırma. Aşağıdaki tabloda rol için verilen izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|*/Read|Gizli dizileri hariç tüm türlerin kaynaklarını okuyun.|
|Microsoft. Automation/automationAccounts/*|Otomasyon hesaplarını yönetin.|
|Microsoft.ClassicCompute/virtualMachines/extensions/*|Sanal makine uzantıları oluşturun ve yönetin.|
|Microsoft.ClassicStorage/storageAccounts/listKeys/action|Klasik depolama hesabı anahtarlarını listeleyin.|
|Microsoft. COMPUTE/virtualMachines/Extensions/*|Klasik sanal makine uzantıları oluşturun ve yönetin.|
|Microsoft.Insights/alertRules/*|Uyarı kurallarını okuma/yazma/silme.|
|Microsoft. Insights/diagnosticSettings/*|Tanılama ayarlarını okuma/yazma/silme.|
|Microsoft.OperationalInsights/*|Azure Izleyici günlüklerini yönetin.|
|Microsoft.OperationsManagement/*|Çalışma alanlarındaki çözümleri yönetin.|
|Microsoft. resources/dağıtımlar/*|Kaynak grubu dağıtımlarını oluşturun ve yönetin.|
|Microsoft. resources/abonelikler/ResourceGroups/dağıtımlar/*|Kaynak grubu dağıtımlarını oluşturun ve yönetin.|
|Microsoft.Storage/storageAccounts/listKeys/action|Depolama hesabı anahtarlarını listeleyin.|
|Microsoft. support/*|Destek biletleri oluşturun ve yönetin.|

### <a name="log-analytics-reader"></a>Log Analytics Okuyucusu

Log Analytics okuyucu tüm izleme verilerini görüntüleyip arayabilir ve tüm Azure kaynaklarında Azure tanılama 'nın yapılandırılmasını görüntüleme dahil olmak üzere izleme ayarlarını görüntüleyebilir. Aşağıdaki tabloda rol için verilen veya reddedilen izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|*/Read|Gizli dizileri hariç tüm türlerin kaynaklarını okuyun.|
|Microsoft.OperationalInsights/workspaces/analytics/query/action|Azure Izleyici günlüklerinde sorguları yönetin.|
|Microsoft. Operationalınsights/çalışma alanları/arama/eylem|Azure Izleyici günlük verilerini arayın.|
|Microsoft. support/*|Destek biletleri oluşturun ve yönetin.|
|**Eylem değil**| |
|Microsoft. Operationalınsights/çalışma alanları/sharedKeys/Read|Paylaşılan erişim anahtarları okunamıyor.|

### <a name="monitoring-contributor"></a>Katkıda bulunan izleniyor

Bir Izleme katılımcısı, tüm izleme verilerini okuyabilir ve izleme ayarlarını güncelleştirebilir. Aşağıdaki tabloda rol için verilen izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|*/Read|Gizli dizileri hariç tüm türlerin kaynaklarını okuyun.|
|Microsoft.AlertsManagement/alerts/*|Uyarıları yönetin.|
|Microsoft.AlertsManagement/alertsSummary/*|Uyarı panosunu yönetin.|
|Microsoft.Insights/AlertRules/*|Uyarı kurallarını yönetin.|
|Microsoft. Insights/bileşenler/*|Application Insights bileşenlerini yönetin.|
|Microsoft. Insights/DiagnosticSettings/*|Tanılama ayarlarını yönetin.|
|Microsoft.Insights/eventtypes/*|Bir abonelikteki etkinlik günlüğü olaylarını (yönetim olayları) listeleyin. Bu izin, etkinlik günlüğüne hem programlı hem de portala erişim için geçerlidir.|
|Microsoft.Insights/LogDefinitions/*|Bu izin, Portal aracılığıyla etkinlik günlüklerine erişmesi gereken kullanıcılar için gereklidir. Etkinlik günlüğündeki günlük kategorilerini listeleyin.|
|Microsoft.Insights/MetricDefinitions/*|Ölçüm tanımlarını oku (bir kaynak için kullanılabilen ölçüm türlerinin listesi).|
|Microsoft. Insights/ölçümler/*|Bir kaynak için ölçümleri okuyun.|
|Microsoft.Insights/Register/Action|Microsoft. Insights sağlayıcısını kaydedin.|
|Microsoft.Insights/webtests/*|Application Insights Web testlerini yönetin.|
|Microsoft.OperationalInsights/workspaces/intelligencepacks/*|Azure Izleyici günlükleri çözüm paketlerini yönetin.|
|Microsoft. Operationalınsights/çalışma alanları/Savedaramalar/*|Azure Izleyici günlükleri kaydedilmiş aramalarını yönetme.|
|Microsoft. Operationalınsights/çalışma alanları/arama/eylem|Log Analytics çalışma alanlarını ara.|
|Microsoft. Operationalınsights/Workspaces/sharedKeys/Action|Log Analytics çalışma alanı için anahtarları listeleyin.|
|Microsoft. Operationalınsights/Workspaces/storageınsiizconfigs/*|Azure Izleyici günlük depolama öngörülerini yönetin.|
|Microsoft. support/*|Destek biletleri oluşturun ve yönetin.|
|Microsoft. WorkloadMonitor/iş yükleri/*|Iş yüklerini yönetin.|

### <a name="monitoring-reader"></a>İzleme okuyucusu

Izleme okuyucusu tüm izleme verilerini okuyabilir. Aşağıdaki tabloda rol için verilen izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|*/Read|Gizli dizileri hariç tüm türlerin kaynaklarını okuyun.|
|Microsoft. Operationalınsights/çalışma alanları/arama/eylem|Log Analytics çalışma alanlarını ara.|
|Microsoft. support/*|Destek biletleri oluşturma ve yönetme|

### <a name="user-access-administrator"></a>Kullanıcı Erişimi Yöneticisi

Kullanıcı erişimi Yöneticisi, Azure kaynaklarına Kullanıcı erişimini yönetebilir. Aşağıdaki tabloda rol için verilen izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|*/Read|Tüm kaynakları okuyun|
|Microsoft. Authorization/*|Yetkilendirmeyi Yönetme|
|Microsoft. support/*|Destek biletleri oluşturma ve yönetme|

## <a name="onboarding"></a>Hazırlama

Aşağıdaki tablolarda, değişiklik izleme veya güncelleştirme yönetimi çözümleri için sanal makineleri ekleme için gereken en düşük izinler gösterilmektedir.

### <a name="onboarding-from-a-virtual-machine"></a>Bir sanal makineden ekleme

|**Eylem**  |**Yetkisi**  |**En düşük kapsam**  |
|---------|---------|---------|
|Yeni dağıtım yaz      | Microsoft. resources/dağıtımlar/*          |Abonelik          |
|Yeni kaynak grubu yaz      | Microsoft. resources/abonelikler/resourceGroups/Write        | Abonelik          |
|Yeni varsayılan çalışma alanı oluştur      | Microsoft. Operationalınsights/çalışma alanları/yazma         | Kaynak grubu         |
|Yeni hesap oluştur      |  Microsoft. Automation/automationAccounts/Write        |Kaynak grubu         |
|Çalışma alanını ve hesabı bağla      |Microsoft. Operationalınsights/çalışma alanları/yazma</br>Microsoft. Automation/automationAccounts/Read|Çalışma Alanı</br>Otomasyon hesabı
|Çözüm oluştur      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write |Kaynak grubu          |
|MMA uzantısı oluştur      | Microsoft. COMPUTE/virtualMachines/Write         | Sanal Makine         |
|Kayıtlı arama oluştur      | Microsoft. Operationalınsights/çalışma alanları/yazma          | Çalışma Alanı         |
|Kapsam yapılandırması oluştur      | Microsoft. Operationalınsights/çalışma alanları/yazma          | Çalışma Alanı         |
|Çözümü Scope config 'e bağlama      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write         | Çözüm         |
|Ekleme durumu denetimi-okuma çalışma alanı      | Microsoft. Operationalınsights/çalışma alanları/okuma         | Çalışma Alanı         |
|Ekleme durumu denetimi-hesabın bağlantılı çalışma alanı özelliğini oku     | Microsoft. Automation/automationAccounts/Read      | Otomasyon hesabı        |
|Ekleme durumu denetimi-çözümü oku      | Microsoft.OperationalInsights/workspaces/intelligencepacks/read          | Çözüm         |
|Ekleme durumu denetimi-VM okuma      | Microsoft. COMPUTE/virtualMachines/okuma         | Sanal Makine         |
|Ekleme durumu denetimi-oku hesabı      | Microsoft. Automation/automationAccounts/Read  |  Otomasyon hesabı   |
| VM<sup>1</sup> için çalışma alanı denetimini ekleme       | Microsoft. Operationalınsights/çalışma alanları/okuma         | Abonelik         |
| Log Analytics sağlayıcıyı kaydetme |Microsoft. Insights/Register/ACTION | Abonelik|

<sup>1</sup> VM portalı deneyimi aracılığıyla eklemek için bu izin gerekir.

### <a name="onboarding-from-automation-account"></a>Otomasyon hesabından ekleme

|**Eylem**  |**Yetkisi** |**En düşük kapsam**  |
|---------|---------|---------|
|Yeni dağıtım oluştur     | Microsoft. resources/dağıtımlar/*        | Abonelik         |
|Yeni kaynak grubu oluştur     | Microsoft. resources/abonelikler/resourceGroups/Write         | Abonelik        |
|Automationekleme dikey penceresi-yeni çalışma alanı oluştur     |Microsoft. Operationalınsights/çalışma alanları/yazma           | Kaynak grubu        |
|Automationekleme dikey penceresi-bağlantılı çalışma alanını oku     | Microsoft. Automation/automationAccounts/Read        | Otomasyon hesabı       |
|Automationekleme dikey penceresi-çözümü oku     | Microsoft.OperationalInsights/workspaces/intelligencepacks/read         | Çözüm        |
|Automationekleme dikey penceresi-çalışma alanını oku     | Microsoft.OperationalInsights/workspaces/intelligencepacks/read        | Çalışma Alanı        |
|Çalışma alanı ve hesap için bağlantı oluştur     | Microsoft. Operationalınsights/çalışma alanları/yazma        | Çalışma Alanı        |
|ShoeBox için yazma hesabı      | Microsoft. Automation/automationAccounts/Write        | Hesap        |
|Çözüm oluştur      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write        | Kaynak Grubu         |
|Kayıtlı aramayı Oluştur/Düzenle     | Microsoft. Operationalınsights/çalışma alanları/yazma        | Çalışma Alanı        |
|Kapsam yapılandırması oluştur/Düzenle     | Microsoft. Operationalınsights/çalışma alanları/yazma        | Çalışma Alanı        |
|Çözümü Scope config 'e bağlama      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write         | Çözüm         |
| Log Analytics sağlayıcıyı kaydetme |Microsoft. Insights/Register/ACTION | Abonelik|
|**2. adım-birden çok VM ekleme**     |         |         |
|Vmontaslak dikey penceresi-MMA uzantısı oluşturma     | Microsoft. COMPUTE/virtualMachines/Write           | Sanal Makine        |
|Kayıtlı aramayı Oluştur/Düzenle     | Microsoft. Operationalınsights/çalışma alanları/yazma           | Çalışma Alanı        |
|Kapsam yapılandırması oluştur/Düzenle  | Microsoft. Operationalınsights/çalışma alanları/yazma   | Çalışma Alanı|

## <a name="update-management"></a>Güncelleştirme yönetimi

Güncelleştirme yönetimi, hizmeti sağlamak için birden çok hizmete ulaşır. Aşağıdaki tabloda, güncelleştirme yönetimi dağıtımlarını yönetmek için gereken izinler gösterilmektedir:

|**Kaynak**  |**Rol**  |**Kapsam**  |
|---------|---------|---------|
|Otomasyon hesabı     | Log Analytics Katkıda Bulunan       | Otomasyon hesabı        |
|Otomasyon hesabı    | Sanal Makine Katkıda Bulunanı        | Hesabın kaynak grubu        |
|Log Analytics çalışma alanı     | Log Analytics Katkıda Bulunan| Log Analytics çalışma alanı        |
|Log Analytics çalışma alanı |Log Analytics Okuyucusu| Abonelik|
|Çözüm     |Log Analytics Katkıda Bulunan         | Çözüm|
|Sanal Makine     | Sanal Makine Katkıda Bulunanı        | Sanal Makine        |

## <a name="configure-rbac-for-your-automation-account"></a>Otomasyon hesabınız için RBAC yapılandırma

Aşağıdaki bölümde [Portal](#configure-rbac-using-the-azure-portal) ve [PowerShell](#configure-rbac-using-powershell) aracılığıyla Otomasyon hesabınızda RBAC 'nin nasıl yapılandırılacağı gösterilmektedir

### <a name="configure-rbac-using-the-azure-portal"></a>Azure portal kullanarak RBAC yapılandırma

1. [Azure portalında](https://portal.azure.com/) oturum açın Otomasyon Hesapları sayfasından Otomasyon hesabınızı açın.
2. Sol üst köşedeki **erişim denetimi (IAM)** denetimine tıklayın. Bu, Otomasyon hesabınızı yönetmek için Yeni Kullanıcı, Grup ve uygulama ekleyebileceğiniz ve Otomasyon hesabı için yapılandırılabilecek mevcut rolleri görüntüleyebileceğiniz **erişim denetimi (IAM)** sayfasını açar.
3. Tıklayın **rol atamaları** sekmesi.

   ![Erişim düğmesi](media/automation-role-based-access-control/automation-01-access-button.png)

#### <a name="add-a-new-user-and-assign-a-role"></a>Yeni kullanıcı ekleme ve rol atama

1. **Erişim denetimi (IAM)** sayfasında **+ rol ataması** Ekle ' ye tıklayarak bir Kullanıcı, Grup veya uygulama ekleyebileceğiniz **rol ataması Ekle** sayfasını açın ve bunlara bir rol atayabilirsiniz.

2. Kullanılabilir roller listesinden bir rol seçin. Otomasyon hesabının desteklediği kullanılabilir yerleşik rolleri veya tanımladığınız herhangi bir özel rolü seçebilirsiniz.

3. **Seçim** alanında izinleri vermek istediğiniz kullanıcının Kullanıcı adını yazın. Listeden kullanıcıyı seçin ve **Kaydet**' e tıklayın.

   ![Kullanıcı ekle](media/automation-role-based-access-control/automation-04-add-users.png)

   Şimdi, seçilen rolün atandığı **Kullanıcılar** sayfasına eklenen kullanıcıyı görmeniz gerekir

   ![Kullanıcıları listele](media/automation-role-based-access-control/automation-05-list-users.png)

   Kullanıcıya **Roller** sayfasından rol atayabilirsiniz.
4. **Roller** sayfasını açmak için **ERIŞIM denetimi (IAM)** sayfasındaki **Roller** ' e tıklayın. Bu sayfadan rolün adını, bu role atanan kullanıcıların ve grupların sayısını görebilirsiniz.

    ![Kullanıcılar sayfasından rol atama](media/automation-role-based-access-control/automation-06-assign-role-from-users-blade.png)

   > [!NOTE]
   > Rol tabanlı erişim denetimi yalnızca Otomasyon hesabı kapsamında ayarlanabilir, Otomasyon hesabının altındaki herhangi bir kaynakta bulunamaz.

#### <a name="remove-a-user"></a>Kullanıcıyı kaldırma

Otomasyon hesabını yönetmesiz veya kuruluş için artık çalışan bir kullanıcının erişim iznini kaldırabilirsiniz. Aşağıdaki adımları kullanarak kullanıcı kaldırabilirsiniz:

1. **Erişim denetimi (IAM)** sayfasında, kaldırmak istediğiniz kullanıcıyı seçin ve **Kaldır**' ı tıklatın.
2. Atama ayrıntıları sayfasında **Kaldır** düğmesine tıklayın.
3. Kaldırmayı onaylamak için **Evet**'e tıklayın.

   ![Kullanıcıları kaldır](media/automation-role-based-access-control/automation-08-remove-users.png)

### <a name="configure-rbac-using-powershell"></a>PowerShell kullanarak RBAC yapılandırma

Rol tabanlı erişim, aşağıdaki [Azure PowerShell cmdlet 'leri](../role-based-access-control/role-assignments-powershell.md)kullanılarak bir Otomasyon hesabına de yapılandırılabilir:

[Get-AzureRmRoleDefinition](/previous-versions/azure/mt603792(v=azure.100)) , Azure Active Directory bulunan tüm RBAC rollerini listeler. Belirli bir rol tarafından gerçekleştirilebilen tüm eylemleri listelemek için bu komutu **Ad** özelliğiyle birlikte bu komutu kullanabilirsiniz.

```azurepowershell-interactive
Get-AzureRmRoleDefinition -Name 'Automation Operator'
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

[Get-Azurermroleatama](/previous-versions/azure/mt619413(v=azure.100)) , belirtilen KAPSAMDAKI Azure AD RBAC rol atamalarını listeler. Hiçbir parametre olmadan, bu komut abonelik altında yapılan tüm rol atamalarını döndürür. Belirli kullanıcıların yanı sıra bu kullanıcıların üyesi olduğu gruplara da erişim atamalarını listelemek için **ExpandPrincipalGroups** parametresini kullanın.
    **Örnek:** Otomasyon hesabı içinde tüm kullanıcıları ve rollerini listelemek için aşağıdaki komutu kullanın.

```azurepowershell-interactive
Get-AzureRMRoleAssignment -scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
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

Kullanıcılara, gruplara ve uygulamalara belirli bir kapsama erişim atamak için [New-Azurermroleatama](/previous-versions/azure/mt603580(v=azure.100)) .
    **Örnek:** Otomasyon hesabı kapsamındaki bir kullanıcı için "Otomasyon Işleci" rolünü atamak için aşağıdaki komutu kullanın.

```azurepowershell-interactive
New-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to grant access> -RoleDefinitionName 'Automation operator' -Scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
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

Belirli bir kapsamdaki belirtilen kullanıcı, Grup veya uygulamanın erişimini kaldırmak için [Remove-Azurermroleatamasını](/previous-versions/azure/mt603781(v=azure.100)) kullanın.
    **Örnek:** Kullanıcıyı Otomasyon hesabı kapsamındaki "otomasyon operatörü" rolünden kaldırmak için aşağıdaki komutu kullanın.

```azurepowershell-interactive
Remove-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to remove> -RoleDefinitionName 'Automation Operator' -Scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

Yukarıdaki örneklerde, **oturum açma kimliğini**, **abonelik kimliğini**, **kaynak grubu adını**ve **Otomasyon hesabı adını** hesap ayrıntılarınız ile değiştirin. Kullanıcı rolü atamasını kaldırmak için devam etmeden önce onaylamanız istendiğin **Evet**’i seçin.

### <a name="user-experience-for-automation-operator-role---automation-account"></a>Otomasyon işletmeni rolü için Kullanıcı deneyimi-Otomasyon hesabı

Otomasyon hesabı kapsamındaki otomasyon operatörü rolüne atanan bir Kullanıcı, kendilerine atanan Otomasyon hesabını görüntülediğinde, yalnızca runbook 'ların, runbook işlerinin ve otomasyon hesabında oluşturulan zamanlamaların listesini görüntüleyebilir, ancak bunları görüntüleyemez tanımı. Runbook işini başlatabilir, durdurabilir, askıya alabilir, sürdürebilir veya zamanlayabilirler. Kullanıcının, konfigürasyonlar, karma çalışan grupları veya DSC düğümleri gibi diğer otomasyon kaynaklarına erişimi yoktur.

![Kaynaklara erişim yok](media/automation-role-based-access-control/automation-10-no-access-to-resources.png)

## <a name="configure-rbac-for-runbooks"></a>Runbook 'Lar için RBAC yapılandırma

Azure Otomasyonu, belirli runbook 'lara RBAC atamanıza olanak tanır. Bunu yapmak için, belirli bir runbook 'a bir kullanıcı eklemek üzere aşağıdaki betiği çalıştırın. Aşağıdaki betik bir Otomasyon hesabı Yöneticisi veya kiracı yöneticisi tarafından çalıştırılabilir.

```azurepowershell-interactive
$rgName = "<Resource Group Name>" # Resource Group name for the Automation Account
$automationAccountName ="<Automation Account Name>" # Name of the Automation Account
$rbName = "<Name of Runbook>" # Name of the runbook
$userId = "<User ObjectId>" # Azure Active Directory (AAD) user's ObjectId from the directory

# Gets the Automation Account resource
$aa = Get-AzureRmResource -ResourceGroupName $rgName -ResourceType "Microsoft.Automation/automationAccounts" -ResourceName $automationAccountName

# Get the Runbook resource
$rb = Get-AzureRmResource -ResourceGroupName $rgName -ResourceType "Microsoft.Automation/automationAccounts/runbooks" -ResourceName "$automationAccountName/$rbName"

# The Automation Job Operator role only needs to be ran once per user.
New-AzureRmRoleAssignment -ObjectId $userId -RoleDefinitionName "Automation Job Operator" -Scope $aa.ResourceId

# Adds the user to the Automation Runbook Operator role to the Runbook scope
New-AzureRmRoleAssignment -ObjectId $userId -RoleDefinitionName "Automation Runbook Operator" -Scope $rb.ResourceId
```

Çalıştırıldıktan sonra, kullanıcının Azure portal oturum açmasını ve **tüm kaynakları**görüntülemesini sağlar. Listede, runbook 'un için bir **Otomasyon Runbook işleci** olarak eklendiğini görürsünüz.

![Portalda runbook RBAC](./media/automation-role-based-access-control/runbook-rbac.png)

### <a name="user-experience-for-automation-operator-role---runbook"></a>Otomasyon işletmeni rolü için Kullanıcı deneyimi-runbook

Runbook kapsamındaki otomasyon operatörü rolüne atanan bir Kullanıcı, atandığı bir runbook 'u görüntülediğinde, runbook 'u başlatabilir ve Runbook işlerini görüntüleyebilir.

![Yalnızca başlangıç erişimi vardır](media/automation-role-based-access-control/automation-only-start.png)

## <a name="next-steps"></a>Sonraki adımlar

* Azure Otomasyonu’nda RBAC yapılandırmak için çeşitli yollar hakkında daha fazla bilgi için bkz. [Azure PowerShell ile RBAC yönetme](../role-based-access-control/role-assignments-powershell.md).
* Runbook başlatmak için çeşitli yollar hakkında daha fazla ayrıntı için bkz. [runbook başlatma](automation-starting-a-runbook.md)
* Farklı runbook türleri hakkında daha fazla bilgi için bkz. [Azure Otomasyonu runbook türleri](automation-runbook-types.md)

