---
title: Azure Izleyici ile arşiv & raporu-Azure AD Yetkilendirme Yönetimi
description: Azure Active Directory yetkilendirme yönetimi 'nde Azure Izleyici ile günlükleri arşivlemeyi ve rapor oluşturmayı öğrenin.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 02/27/2020
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: f89af42e32783de479c4302b19c0a7ddc1289bb8
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78202188"
---
# <a name="archive-logs-and-reporting-on-azure-ad-entitlement-management-in-azure-monitor"></a>Azure Izleyici 'de Azure AD yetkilendirme yönetiminde arşiv günlükleri ve raporları

Azure AD denetim günlüğünde en fazla 30 gün boyunca denetim olaylarını depolar. Bununla birlikte, denetim verilerini varsayılan saklama süresinden daha uzun bir süre boyunca tutabilirsiniz. Azure [ad depolama, verileri](../reports-monitoring/reference-reports-data-retention.md)bir Azure depolama hesabına yönlendirerek veya Azure izleyici 'yi kullanarak verileri raporlama. Daha sonra bu veriler üzerinde çalışma kitaplarını ve özel sorguları ve raporları kullanabilirsiniz.


## <a name="configure-azure-ad-to-use-azure-monitor"></a>Azure AD 'yi Azure Izleyici kullanacak şekilde yapılandırma
Azure Izleyici çalışma kitaplarını kullanmadan önce Azure AD 'yi, denetim günlüklerinin bir kopyasını Azure Izleyici 'ye gönderecek şekilde yapılandırmanız gerekir.

Azure AD denetim günlüklerini arşivleme, bir Azure aboneliğinde Azure Izleyici olmasını gerektirir. Azure izleyici ['de](../reports-monitoring/concept-activity-logs-azure-monitor.md)Azure izleyici 'de Azure izleyici 'yi kullanmaya ilişkin ön koşullar ve tahmini maliyetler hakkında daha fazla bilgi edinebilirsiniz.

**Önkoşul rolü**: genel yönetici

1. Azure portal, genel yönetici olan bir kullanıcı olarak oturum açın. Azure Izleyici çalışma alanını içeren kaynak grubuna erişiminizin olduğundan emin olun.
 
1. **Azure Active Directory** öğesini seçin ve ardından sol gezinti menüsünde Izleme altında **Tanılama ayarları** ' na tıklayın. Denetim günlüklerini bu çalışma alanına göndermek için zaten bir ayar olup olmadığını denetleyin.

1. Zaten bir ayar yoksa, **Tanılama ayarı Ekle**' ye tıklayın. Azure AD denetim günlüğünü Azure Izleyici çalışma alanına göndermek için Azure [ad günlüklerini Azure izleyici günlükleriyle tümleştirme](../reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md#send-logs-to-azure-monitor) makalesindeki yönergeleri kullanın.

    ![Tanılama ayarları bölmesi](./media/entitlement-management-logs-and-reporting/audit-log-diagnostics-settings.png)


1. Günlük Azure Izleyici 'ye gönderildikten sonra **Log Analytics çalışma alanları**' nı seçin ve Azure AD denetim günlüklerini içeren çalışma alanını seçin.

1. **Kullanım ve tahmini maliyetler** ' i seçin ve **veri saklama**' ye tıklayın. Kaydırıcıyı, verileri denetim gereksinimlerinize uyacak şekilde tutmak istediğiniz gün sayısıyla değiştirin.

    ![Log Analytics çalışma alanları bölmesi](./media/entitlement-management-logs-and-reporting/log-analytics-workspaces.png)

## <a name="create-custom-azure-monitor-queries-using-the-azure-portal"></a>Azure portal kullanarak özel Azure Izleyici sorguları oluşturma
Yetkilendirme Yönetimi olayları dahil olmak üzere Azure AD denetim olayları üzerinde kendi sorgularınızı oluşturabilirsiniz.  

1. Azure portal Azure Active Directory, yeni bir sorgu sayfası oluşturmak için sol gezinti menüsündeki Izleme bölümünde bulunan **Günlükler** ' e tıklayın.

1. Çalışma alanınızın, sorgu sayfasının sol üst kısmında gösterilmesi gerekir. Birden çok Azure Izleyici çalışma alanınız varsa ve Azure AD denetim olaylarını depolamak için kullandığınız çalışma alanı gösterilmemişse, **kapsam Seç**' e tıklayın. Ardından, doğru aboneliği ve çalışma alanını seçin.

1. Sonra sorgu metin alanında "Search *" dizesini silin ve aşağıdaki sorguyla değiştirin:

    ```
    AuditLogs | where Category == "EntitlementManagement"
    ```

1. Ardından **Çalıştır**' a tıklayın. 

    ![Sorguyu başlatmak için Çalıştır 'a tıklayın](./media/entitlement-management-logs-and-reporting/run-query.png)

Tablo, varsayılan olarak son saatten yetkilendirme yönetimi için denetim günlüğü olaylarını gösterir. Eski olayları görüntülemek için "zaman aralığı" ayarını değiştirebilirsiniz. Ancak, bu ayarın değiştirilmesi yalnızca Azure AD, olayları Azure Izleyici 'ye göndermek üzere yapılandırıldıktan sonra gerçekleşen olayları gösterir.

Azure Izleyici 'de tutulan en eski ve en yeni denetim olaylarını bildirmek istiyorsanız aşağıdaki sorguyu kullanın:

```
AuditLogs | where TimeGenerated > ago(3653d) | summarize OldestAuditEvent=min(TimeGenerated), NewestAuditEvent=max(TimeGenerated) by Type
```

Azure Izleyici 'de denetim olayları için depolanan sütunlar hakkında daha fazla bilgi için bkz. [Azure izleyici 'de Azure AD denetim günlükleri şemasını yorumlama](../reports-monitoring/reference-azure-monitor-audit-log-schema.md).

## <a name="create-custom-azure-monitor-queries-using-azure-powershell"></a>Azure PowerShell kullanarak özel Azure Izleyici sorguları oluşturma

Azure AD 'yi Azure Izleyici 'ye Günlükler gönderecek şekilde yapılandırdıktan sonra, PowerShell aracılığıyla günlüklere erişebilirsiniz. Ardından, kiracınızda genel yönetici olması gerekmeden, komut dosyalarından veya PowerShell komut satırından sorgular gönderin. 

### <a name="ensure-the-user-or-service-principal-has-the-correct-role-assignment"></a>Kullanıcı veya hizmet sorumlusunun doğru rol atamasına sahip olduğundan emin olun

Azure AD 'de kimlik doğrulaması yapılacak Kullanıcı veya hizmet sorumlusu Log Analytics çalışma alanında uygun Azure rolünde olduğundan emin olun. Rol seçenekleri Log Analytics okuyucu veya Log Analytics katılımcısı olabilir. Bu rollerden birinde zaten varsa, [bir Azure aboneliğiyle log ANALYTICS kimliği almak](#retrieve-log-analytics-id-with-one-azure-subscription)için atlayın.

Rol atamasını ayarlamak ve bir sorgu oluşturmak için aşağıdaki adımları uygulayın:
1. Azure portal, [Log Analytics çalışma alanını](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.OperationalInsights%2Fworkspaces
)bulun.

1. **Access Control (IAM)** seçeneğini belirleyin.

1. Rol ataması eklemek için **Ekle** ' ye tıklayın.

    ![Rol ataması ekleme](./media/entitlement-management-logs-and-reporting/workspace-set-role-assignment.png)

### <a name="install-azure-powershell-module"></a>Azure PowerShell modülünü Install

Uygun rol atamasını aldıktan sonra PowerShell 'i başlatın ve [Azure PowerShell modülünü](/powershell/azure/install-az-ps?view=azps-3.3.0) (henüz yapmadıysanız), şunu yazarak yükleyebilirsiniz:

```azurepowershell
install-module -Name az -allowClobber -Scope CurrentUser
```
    
Artık Azure AD 'ye kimlik doğrulaması yapmaya hazırsınız ve sorguladığınız Log Analytics çalışma alanının kimliğini alma.

### <a name="retrieve-log-analytics-id-with-one-azure-subscription"></a>Tek bir Azure aboneliğiyle Log Analytics KIMLIĞINI alma
Yalnızca tek bir Azure aboneliğiniz ve tek bir Log Analytics çalışma alanınız varsa, Azure AD 'de kimlik doğrulaması yapmak, bu aboneliğe bağlanmak ve bu çalışma alanını almak için aşağıdakileri yazın:
 
```azurepowershell
Connect-AzAccount
$wks = Get-AzOperationalInsightsWorkspace
```
 
### <a name="retrieve-log-analytics-id-with-multiple-azure-subscriptions"></a>Birden çok Azure aboneliğine sahip Log Analytics KIMLIĞINI alma

 [Get-Azoperationalınsightsworkspace](/powershell/module/Az.OperationalInsights/Get-AzOperationalInsightsWorkspace) her seferinde bir abonelikte çalışır. Bu nedenle, birden çok Azure aboneliğiniz varsa, Azure AD günlükleriyle Log Analytics çalışma alanına sahip olan birine bağlandığınızdan emin olmak isteyeceksiniz. 
 
 Aşağıdaki cmdlet 'ler, aboneliklerin bir listesini görüntüler ve Log Analytics çalışma alanına sahip olan aboneliğin kimliğini bulur:
 
```azurepowershell
Connect-AzAccount
$subs = Get-AzSubscription
$subs | ft
```
 
`Connect-AzAccount –Subscription $subs[0].id`gibi bir komut kullanarak PowerShell oturumunuzu yeniden kimlik doğrulaması yapabilir ve bu abonelikle ilişkilendirebilirsiniz. Etkileşimli olmayanlar dahil olmak üzere PowerShell 'den Azure 'da kimlik doğrulaması yapma hakkında daha fazla bilgi edinmek için bkz. [Azure PowerShell oturum açma](/powershell/azure/authenticate-azureps?view=azps-3.3.0&viewFallbackFrom=azps-2.5.0
).

Bu abonelikte birden çok Log Analytics çalışma alanınız varsa, [Get-Azoperationalınsightsworkspace](/powershell/module/Az.OperationalInsights/Get-AzOperationalInsightsWorkspace) cmdlet 'i, çalışma alanlarının listesini döndürür. Daha sonra Azure AD günlüklerine sahip olan bir tane bulabilirsiniz. Bu cmdlet tarafından döndürülen `CustomerId` alanı, Log Analytics çalışma alanına genel bakış alanındaki Azure portal görünen "çalışma alanı kimliği" değeri ile aynıdır.
 
```powershell
$wks = Get-AzOperationalInsightsWorkspace
$wks | ft CustomerId, Name
```

### <a name="send-the-query-to-the-log-analytics-workspace"></a>Sorguyu Log Analytics çalışma alanına gönder
Son olarak, bir çalışma alanı tanımlandıktan sonra, bu çalışma alanına bir kusto sorgusu göndermek için [Invoke-Azoperationalınsightsquery](/powershell/module/az.operationalinsights/Invoke-AzOperationalInsightsQuery?view=azps-3.3.0
) komutunu kullanabilirsiniz. Bu sorgular [kusto sorgu dilinde](https://docs.microsoft.com/azure/kusto/query/)yazılır.
 
Örneğin, Log Analytics çalışma alanından denetim olay kayıtlarının tarih aralığını, PowerShell cmdlet 'leri ile birlikte kullanarak, şunun gibi bir sorgu gönderebilmeniz için alabilirsiniz:
 
```powershell
$aQuery = "AuditLogs | where TimeGenerated > ago(3653d) | summarize OldestAuditEvent=min(TimeGenerated), NewestAuditEvent=max(TimeGenerated) by Type"
$aResponse = Invoke-AzOperationalInsightsQuery -WorkspaceId $wks[0].CustomerId -Query $aQuery
$aResponse.Results |ft
```

Yetkilendirme yönetimi olaylarını, şunun gibi bir sorgu kullanarak da alabilirsiniz:

```azurepowershell
$bQuery = = 'AuditLogs | where Category == "EntitlementManagement"'
$bResponse = Invoke-AzOperationalInsightsQuery -WorkspaceId $wks[0].CustomerId -Query $Query
$bResponse.Results |ft 
```

## <a name="next-steps"></a>Sonraki adımlar:
- [Azure Izleyici çalışma kitapları ile etkileşimli raporlar oluşturma](../../azure-monitor/app/usage-workbooks.md) 

