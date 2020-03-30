---
title: Azure Monitor'da roller, izinler ve güvenlik
description: İzleme kaynaklarına erişimi kısıtlamak için Azure Monitor'un yerleşik rollerini ve izinlerini nasıl kullanacağınızı öğrenin.
author: johnkemnetz
services: azure-monitor
ms.topic: conceptual
ms.date: 11/27/2017
ms.author: johnkem
ms.subservice: ''
ms.openlocfilehash: 81309f0b5781e6302887a5b079ed359e70659834
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658991"
---
# <a name="roles-permissions-and-security-in-azure-monitor"></a>Azure Monitor'da roller, izinler ve güvenlik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Birçok takımın izleme verilerine ve ayarlarına erişimi sıkı bir şekilde düzenlemesi gerekir. Örneğin, yalnızca izleme (destek mühendisleri, DevOps mühendisleri) üzerinde çalışan ekip üyeleriniz varsa veya yönetilen bir hizmet sağlayıcısı kullanıyorsanız, bunları yalnızca izleme verilerine erişirken oluşturma, değiştirme veya değiştirme yeteneklerini kısıtlamak isteyebilirsiniz kaynakları silin. Bu makalede, Azure'daki bir kullanıcıya yerleşik izleme RBAC rolünün nasıl hızlı bir şekilde uygulanacağı veya sınırlı izleme izinlerine ihtiyaç duyan bir kullanıcı için kendi özel rolünüzü oluşturma şekligösterilmektedir. Daha sonra Azure Monitor ile ilgili kaynaklarınız için güvenlik hususlarını ve içerdikleri verilere erişimi nasıl sınırlayabileceğinizi tartışır.

## <a name="built-in-monitoring-roles"></a>Yerleşik izleme rolleri
Azure Monitor'un yerleşik rolleri, abonelikteki kaynaklara erişimi sınırlamaya yardımcı olmak ve altyapıizlemeden sorumlu kişilerin ihtiyaç duydukları verileri elde etmelerine ve yapılandırmalarına olanak sağlamak üzere tasarlanmıştır. Azure Monitor iki yer değiştirme rolü sağlar: Bir İzleme Okuyucusu ve Bir İzleme Katılımcısı.

### <a name="monitoring-reader"></a>İzleme Okuyucu
İzleme Okuyucusu rolünü atayan kişiler, abonelikteki tüm izleme verilerini görüntüleyebilir, ancak herhangi bir kaynağı değiştirebilir veya izleme kaynaklarıyla ilgili ayarları değiştiremez. Bu rol, destek veya operasyon mühendisleri gibi bir kuruluştaki kullanıcılar için uygundur:

* Portaldaki izleme panolarını görüntüleyin ve kendi özel izleme panolarını oluşturun.
* [Azure Uyarıları'nda](alerts-overview.md) tanımlanan uyarı kurallarını görüntüleme
* Azure Monitor REST [API,](https://msdn.microsoft.com/library/azure/dn931930.aspx) [PowerShell cmdlets](powershell-quickstart-samples.md)veya [çapraz platform CLI'yi](cli-samples.md)kullanarak ölçümler için sorgula.
* Portal, Azure Monitor REST API, PowerShell cmdlets veya çapraz platform CLI'yi kullanarak Etkinlik Günlüğü'ni sorgula.
* Bir kaynağın [tanılama ayarlarını](diagnostic-settings.md) görüntüleyin.
* Aboneliğin [günlük profilini](activity-log-export.md) görüntüleyin.
* Otomatik ölçek ayarlarını görüntüleyin.
* Uyarı etkinliğini ve ayarlarını görüntüleyin.
* Uygulama Öngörüleri verilerine erişin ve AI Analytics'te verileri görüntüleyin.
* Günlük Analizi çalışma alanı verilerini ve çalışma alanı kullanım verilerini arayın.
* Günlük Analizi yönetim gruplarını görüntüleyin.
* Log Analytics çalışma alanında arama şemasını alın.
* Log Analytics çalışma alanında izleme paketlerini listele.
* Log Analytics çalışma alanında kayıtlı aramaları alın ve çalıştırın.
* Log Analytics çalışma alanı depolama yapılandırmasını alın.

> [!NOTE]
> Bu rol, bir olay hub'ına aktarılan veya bir depolama hesabında depolanan günlük verilerine okuma erişimi sağlamaz. Bu kaynaklara erişimi yapılandırma hakkında bilgi için [aşağıya bakın.](#security-considerations-for-monitoring-data)
> 
> 

### <a name="monitoring-contributor"></a>Katılımcıyı İzleme
İzleme Katılımcısı rolünü atayan kişiler abonelikteki tüm izleme verilerini görüntüleyebilir ve izleme ayarları oluşturabilir veya değiştirebilir, ancak başka kaynakları değiştiremez. Bu rol, İzleme Okuyucurolünün bir üst kümesidir ve yukarıdaki izinlere ek olarak aşağıdakileri yapabilmesi gereken bir kuruluşun izleme ekibinin veya yönetilen hizmet sağlayıcılarının üyeleri için uygundur:

* İzleme panolarını paylaşılan bir pano olarak yayımlayın.
* Bir kaynak için [tanılama ayarlarını](diagnostic-settings.md) ayarlayın.\*
* Abonelik için [günlük profilini](activity-log-export.md) ayarlayın.\*
* [Azure Uyarıları](alerts-overview.md)aracılığıyla uyarı kuralları etkinliğini ve ayarlarını ayarlayın.
* Uygulama Öngörüleri web testleri ve bileşenleri oluşturun.
* Liste Günlüğü Analytics çalışma alanı paylaşılan anahtarları.
* Log Analytics çalışma alanında izleme paketlerini etkinleştirin veya devre dışı edin.
* Log Analytics çalışma alanında kayıtlı aramaları oluşturun ve silin ve çalıştırın.
* Log Analytics çalışma alanı depolama yapılandırmasını oluşturun ve silin.

\*kullanıcıya ayrıca, bir günlük profili veya tanılama ayarı ayarlamak için hedef kaynakta (depolama hesabı veya olay merkezi ad alanı) ListKeys izni de verilmelidir.

> [!NOTE]
> Bu rol, bir olay hub'ına aktarılan veya bir depolama hesabında depolanan günlük verilerine okuma erişimi sağlamaz. Bu kaynaklara erişimi yapılandırma hakkında bilgi için [aşağıya bakın.](#security-considerations-for-monitoring-data)
> 
> 

## <a name="monitoring-permissions-and-custom-rbac-roles"></a>İzinleri ve özel RBAC rollerini izleme
Yukarıdaki yerleşik roller takımınızın tam gereksinimlerini karşılamazsa, daha ayrıntılı izinlerle [özel bir RBAC rolü oluşturabilirsiniz.](../../role-based-access-control/custom-roles.md) Aşağıda, açıklamalarıyla birlikte ortak Azure Monitor RBAC işlemleri verilmiştir.

| İşlem | Açıklama |
| --- | --- |
| Microsoft.Insights/ActionGroups/[Oku, Yaz, Sil] |Eylem gruplarını okuma/yazma/silme. |
| Microsoft.Insights/ActivityLogAlerts/[Oku, Yaz, Sil] |Etkinlik günlüğü uyarılarını okuma/yazma/silme. |
| Microsoft.Insights/AlertRules/[Oku, Yaz, Sil] |Uyarı kurallarını okuma/yazma/silme (klasik uyarılardan). |
| Microsoft.Insights/AlertRules/Incidents/Oku |Uyarı kuralları için olayları (tetiklenen uyarı kuralının geçmişi) listele. Bu sadece portal için geçerlidir. |
| Microsoft.Insights/AutoscaleSettings/[Oku, Yaz, Sil] |Otomatik ölçek ayarlarını okuma/yazma/silme. |
| Microsoft.Insights/DiagnosticSettings/[Oku, Yaz, Sil] |Tanı lama ayarlarını okuma/yazma/silme. |
| Microsoft.Insights/EventCategories/Oku |Etkinlik Günlüğünde mümkün olan tüm kategorileri sayısal.'yı ayırın. Azure portalı tarafından kullanılır. |
| Microsoft.Insights/eventtypes/digestevents/Oku |Bu izin, portal üzerinden Etkinlik Günlükleri'ne erişmesi gereken kullanıcılar için gereklidir. |
| Microsoft.Insights/eventtypes/values/Read |Bir abonelikteki Etkinlik Günlüğü olaylarını (yönetim olaylarını) listele. Bu izin, Etkinlik Günlüğü'ne hem programatik hem de portal erişimi için geçerlidir. |
| Microsoft.Insights/ExtendedDiagnosticSettings/[Oku, Yaz, Sil] | Ağ akışı günlükleri için tanılama ayarlarını okuma/yazma/silme. |
| Microsoft.Insights/LogDefinitions/Oku |Bu izin, portal üzerinden Etkinlik Günlükleri'ne erişmesi gereken kullanıcılar için gereklidir. |
| Microsoft.Insights/LogProfiles/[Oku, Yaz, Sil] |Günlük profillerini okuma/yazma/silme (Etkinlik Günlüğü'nü etkinlik merkezine veya depolama hesabına aktarın). |
| Microsoft.Insights/MetricAlerts/[Oku, Yaz, Sil] |Gerçek zamanlı metrik uyarıların yakınında okuma/yazma/silme |
| Microsoft.Insights/MetricDefinitions/Oku |Metrik tanımları okuyun (kaynak için kullanılabilir metrik türlerinin listesi). |
| Microsoft.Insights/Metrics/Oku |Kaynak için ölçümleri okuyun. |
| Microsoft.Insights/Register/Action |Azure Monitör kaynak sağlayıcısını kaydedin. |
| Microsoft.Insights/ScheduledQueryRules/[Oku, Yaz, Sil] |Azure Monitor'daki günlük uyarılarını okuma/yazma/silme. |



> [!NOTE]
> Bir kaynak için uyarılara, tanılama ayarlarına ve ölçümlere erişim, kullanıcının bu kaynağın kaynak türüne ve kapsamına Okuma erişimine sahip olmasını gerektirir. Bir depolama hesabına arşivleyen veya olay hub'larına akış yapan bir tanılama ayarı veya günlük profili oluşturmak ("yazma"), kullanıcının hedef kaynakta ListKeys iznine sahip olmasını gerektirir.
> 
> 

Örneğin, yukarıdaki tabloyu kullanarak aşağıdaki gibi bir "Etkinlik Günlüğü Okuyucusu" için özel bir RBAC rolü oluşturabilirsiniz:

```powershell
$role = Get-AzRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Activity Log Reader"
$role.Description = "Can view activity logs."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Insights/eventtypes/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription")
New-AzRoleDefinition -Role $role 
```

## <a name="security-considerations-for-monitoring-data"></a>İzleme verileri için güvenlik konuları
Verileri izleme -özellikle günlük dosyaları- IP adresleri veya kullanıcı adları gibi hassas bilgiler içerebilir. Azure'dan gelen verileri izleme üç temel formda gelir:

1. Azure aboneliğinizdeki tüm denetim düzlemi eylemlerini açıklayan Etkinlik Günlüğü.
2. kaynak tarafından yayılan günlükleri olan kaynak günlükleri.
3. Kaynaklar tarafından yayılan ölçümler.

Bu veri türlerinin üçü de bir depolama hesabında depolanabilir veya her ikisi de genel amaçlı Azure kaynakları olan Event Hub'a aktarılabilir. Bunlar genel amaçlı kaynaklar olduğundan, bunları oluşturmak, siler ve bunlara erişmek bir yönetici için ayrılmış ayrıcalıklı bir işlemdir. Kötüye kullanımı önlemek için izleme yle ilgili kaynakları izlemek için aşağıdaki uygulamaları kullanmanızı öneririz:

* Verileri izlemek için tek ve özel bir depolama hesabı kullanın. İzleme verilerini birden çok depolama hesabına ayırmanız gerekiyorsa, bir depolama hesabının kullanımını izleme ve izlemeyen veriler arasında asla paylaşmayın, çünkü bu durum yanlışlıkla yalnızca izleme verilerine erişim eihtiyacı olanlara (örneğin, bir üçüncü taraf SIEM) izlemeyen verilere erişim.
* Yukarıdaki gibi aynı nedenle tüm tanılama ayarlarında tek, özel Bir Hizmet Veri Merkezi veya Olay Hub ad alanı kullanın.
* İzlemeyle ilgili depolama hesaplarına veya olay hub'larına erişimi ayrı bir kaynak grubunda tutarak sınırlandırın ve yalnızca bu kaynak grubuna erişimi sınırlamak için izleme rollerinizdeki [kapsamı kullanın.](../../role-based-access-control/overview.md#scope)
* Kullanıcının yalnızca izleme verilerine erişmesi gerektiğinde, listKeys'e abonelik kapsamındaki depolama hesapları veya olay hub'ları için izin vermeyin. Bunun yerine, bu izinleri bir kaynak veya kaynak grubunda kullanıcıya verin (özel bir izleme kaynak grubunuz varsa) kapsamı.

### <a name="limiting-access-to-monitoring-related-storage-accounts"></a>İzlemeyle ilgili depolama hesaplarına erişimi sınırlama
Bir kullanıcı veya uygulamanın bir depolama hesabındaki verileri izlemeye erişmesi gerektiğinde, depolama hesabında blob depolamasına yalnızca hizmet düzeyi okuma erişimi olan izleme verilerini içeren [bir Hesap SAS oluşturmanız](https://msdn.microsoft.com/library/azure/mt584140.aspx) gerekir. PowerShell'de bu durum şu şekilde görünebilir:

```powershell
$context = New-AzStorageContext -ConnectionString "[connection string for your monitoring Storage Account]"
$token = New-AzStorageAccountSASToken -ResourceType Service -Service Blob -Permission "rl" -Context $context
```

Daha sonra, bu depolama hesabından okunması gereken varlığa belirteci verebilirsiniz ve bu depo hesabındaki tüm lekeleri listeleyebilir ve okuyabilir.

Alternatif olarak, bu izni RBAC ile denetlemeniz gerekiyorsa, bu varlığa microsoft.storage/storageAccounts/listkeys/action permission'u belirli depolama hesabında verebilirsiniz. Bu, bir depolama hesabına arşivlemek için tanılama ayarı veya günlük profili ayarlayabilmesi gereken kullanıcılar için gereklidir. Örneğin, yalnızca bir depolama hesabından okunması gereken bir kullanıcı veya uygulama için aşağıdaki özel RBAC rolünü oluşturabilirsiniz:

```powershell
$role = Get-AzRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Monitoring Storage Account Reader"
$role.Description = "Can get the storage account keys for a monitoring storage account."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/storageAccounts/listkeys/action")
$role.Actions.Add("Microsoft.Storage/storageAccounts/Read")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/myMonitoringStorageAccount")
New-AzRoleDefinition -Role $role 
```

> [!WARNING]
> ListKeys izni, kullanıcının birincil ve ikincil depolama hesabı anahtarlarını listelemesini sağlar. Bu anahtarlar, kullanıcıya bu depolama hesabındaki tüm imzalı hizmetlerde (blob, sıra, tablo, dosya) tüm imzalı izinleri (okuma, yazma, blob oluşturma, blobs silme vb.) verir. Mümkün olduğunda yukarıda açıklanan Bir Hesap SAS kullanmanızı öneririz.
> 
> 

### <a name="limiting-access-to-monitoring-related-event-hubs"></a>İzlemeyle ilgili olay merkezlerine erişimi sınırlama
Benzer bir desen olay hub'ları ile izlenebilir, ancak önce özel bir Dinleme yetkilendirme kuralı oluşturmanız gerekir. Yalnızca izlemeyle ilgili olay hub'larını dinlemesi gereken bir uygulamaya erişim izni vermek istiyorsanız, aşağıdakileri yapın:

1. Yalnızca İzleme verilerini yalnızca Dinle talepleriyle akışiçin oluşturulan olay hub'ında paylaşılan bir erişim ilkesi oluşturun. Bu portalda yapılabilir. Örneğin, buna "yalnızca izleme" diyebilirsiniz. Mümkünse, bu anahtarı doğrudan tüketiciye vermek ve bir sonraki adımı atlamak isteyeceksiniz.
2. Tüketicinin anahtar geçici olarak alabilmesi gerekiyorsa, kullanıcıya bu olay merkezi için ListKeys eylemini ver. Bu, olay hub'larına akış için tanılama ayarı veya günlük profili ayarlayabilmesi gereken kullanıcılar için de gereklidir. Örneğin, bir RBAC kuralı oluşturabilirsiniz:
   
   ```powershell
   $role = Get-AzRoleDefinition "Reader"
   $role.Id = $null
   $role.Name = "Monitoring Event Hub Listener"
   $role.Description = "Can get the key to listen to an event hub streaming monitoring data."
   $role.Actions.Clear()
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/authorizationrules/listkeys/action")
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/Read")
   $role.AssignableScopes.Clear()
   $role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.ServiceBus/namespaces/mySBNameSpace")
   New-AzRoleDefinition -Role $role 
   ```

## <a name="monitoring-within-a-secured-virtual-network"></a>Güvenli bir Sanal Ağ içinde izleme

Azure Monitor'un etkinleştirdiğiniz hizmetleri sağlamak için Azure kaynaklarınıza erişmesi gerekir. Azure kaynaklarınızı Genel Internet erişiminden güvence altına alırken izlemek istiyorsanız, aşağıdaki ayarları etkinleştirebilirsiniz.

### <a name="secured-storage-accounts"></a>Güvenli Depolama Hesapları 

İzleme verileri genellikle bir depolama hesabına yazılır. Depolama Hesabına kopyalanan verilerin yetkisiz kullanıcılar tarafından erişilemeyeceğinden emin olmak isteyebilirsiniz. Ek güvenlik için, bir depolama hesabının "seçili ağları" kullanmasını kısıtlayarak yalnızca yetkili kaynaklarınıza ve güvenilir Microsoft hizmetlerine bir depolama hesabına erişmesine izin vermek için ağ erişimini kilitleyebilirsiniz.
![Azure Depolama Ayarları](./media/roles-permissions-security/secured-storage-example.png) İletişim Kurulu Azure Monitörü, güvenilen Microsoft hizmetlerinin Güvenli Depolama Alanınıza erişmesine izin verirseniz, Azure monitörü güvenli Depolama Hesabınıza erişebilir; bu korumalı koşullar altında Azure Monitor kaynak günlüklerini, etkinlik günlüğünü ve ölçümleri Depolama Hesabınıza yazmanızı etkinleştirme. Bu, Log Analytics'in güvenli depolama dan günlükleri okumasına da olanak tanır.   


Daha fazla bilgi için [Ağ güvenliği ve Azure Depolama](../../storage/common/storage-network-security.md)

## <a name="next-steps"></a>Sonraki adımlar
* [Kaynak Yöneticisi'nde RBAC ve izinler hakkında bilgi edinin](../../role-based-access-control/overview.md)
* [Azure'da izlemeyle ilgili genel bakışı okuyun](../../azure-monitor/overview.md)


