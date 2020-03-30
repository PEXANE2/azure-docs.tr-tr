---
title: Azure Monitor ile & raporu arşivleyin - Azure AD yetkilendirme yönetimi
description: Azure Active Directory yetkilendirme yönetiminde Azure Monitor ile günlükleri nasıl arşivlendirecek ve rapor oluşturabilirsiniz öğrenin.
services: active-directory
documentationCenter: ''
author: barclayn
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 03/22/2020
ms.author: barclayn
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 070b7c5e0fef7d50f84271190432a65d29699bdf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80128633"
---
# <a name="archive-logs-and-reporting-on-azure-ad-entitlement-management-in-azure-monitor"></a>Azure Monitor'da Azure AD yetkilendirme yönetimi yle ilgili arşiv günlükleri ve raporlama

Azure AD, denetim olaylarını denetim günlüğünde 30 güne kadar saklar. Ancak, azure [AD'nin raporlama verilerini ne kadar süreyle depoladığı?](../reports-monitoring/reference-reports-data-retention.md) Daha sonra bu verilerle ilgili çalışma kitaplarını ve özel sorguları ve raporları kullanabilirsiniz.


## <a name="configure-azure-ad-to-use-azure-monitor"></a>Azure Monitörünü kullanacak şekilde Azure AD'yi yapılandırma
Azure Monitor çalışma kitaplarını kullanmadan önce, denetim günlüklerinin bir kopyasını Azure Monitor'a gönderecek şekilde Azure AD'yi yapılandırmanız gerekir.

Azure AD denetim günlüklerini arşivlemek için Azure Monitor aboneliğinde Azure Monitor'a sahip olmak gerekir. Azure Monitörü'nde [Azure REKLAM etkinlik günlüklerinde](../reports-monitoring/concept-activity-logs-azure-monitor.md)Azure Monitor kullanmanın ön koşulları ve tahmini maliyetleri hakkında daha fazla bilgi edinebilirsiniz.

**Önkoşul rolü**: Global Admin

1. Azure portalında Global Yönetici olan bir kullanıcı olarak oturum açın. Azure Monitörü çalışma alanını içeren kaynak grubuna erişebildiğinizden emin olun.
 
1. **Azure Etkin Dizin'i** seçin ve sol daki gezinme menüsünde İzleme altında **Tanılama ayarlarını** tıklatın. Denetim günlüklerini bu çalışma alanına göndermek için zaten bir ayar olup olmadığını denetleyin.

1. Zaten bir ayar yoksa, **tanı ayarını ekle'yi**tıklatın. Azure AD denetim günlüğünü Azure Monitor çalışma alanına göndermek için [Azure AD günlüklerini Azure Monitor günlükleriyle tümleştir](../reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md#send-logs-to-azure-monitor) makaledeki yönergeleri kullanın.

    ![Tanılama ayarları bölmesi](./media/entitlement-management-logs-and-reporting/audit-log-diagnostics-settings.png)


1. Günlük Azure Monitor'a gönderildikten **sonra, Günlük Analizi çalışma alanlarını**seçin ve Azure AD denetim günlüklerini içeren çalışma alanını seçin.

1. **Kullanımı ve tahmini maliyetleri** seçin ve Veri **Saklama'yı**tıklatın. Kaydırıcıyı, denetim gereksinimlerinizi karşılamak için verileri tutmak istediğiniz gün sayısıyla değiştirin.

    ![Günlük Analizi çalışma alanları bölmesi](./media/entitlement-management-logs-and-reporting/log-analytics-workspaces.png)

## <a name="create-custom-azure-monitor-queries-using-the-azure-portal"></a>Azure portalını kullanarak özel Azure Monitör sorguları oluşturun
Azure AD denetim etkinliklerinde yetki yönetimi etkinlikleri de dahil olmak üzere kendi sorgularınızı oluşturabilirsiniz.  

1. Azure portalının Azure Etkin Dizini'nde, yeni bir sorgu sayfası oluşturmak için sol daki gezinme menüsündeki İzleme bölümünün altındaki **Günlükler'i** tıklatın.

1. Çalışma alanınız sorgu sayfasının sol üst kısmında gösterilmelidir. Birden çok Azure Monitor çalışma alanınız varsa ve Azure REKLAM denetim olaylarını depolamak için kullandığınız çalışma alanı görünmüyorsa, **Kapsamı Seç'i**tıklatın. Ardından, doğru aboneliği ve çalışma alanını seçin.

1. Ardından, sorgu metni alanında "search *" dizesini silin ve aşağıdaki sorguyla değiştirin:

    ```
    AuditLogs | where Category == "EntitlementManagement"
    ```

1. Ardından **Çalıştır'ı**tıklatın. 

    ![Sorguyı başlatmak için Çalıştır'ı tıklatın](./media/entitlement-management-logs-and-reporting/run-query.png)

Tablo, varsayılan olarak son saatten itibaren yetkilendirme yönetimi için Denetim günlüğü olaylarını gösterir. Eski olayları görüntülemek için "Zaman aralığı" ayarını değiştirebilirsiniz. Ancak, bu ayarı değiştirmek yalnızca Azure AD'nin Azure Monitor'a olay gönderecek şekilde yapılandırılmasından sonra meydana gelen olayları gösterir.

Azure Monitor'da düzenlenen en eski ve en yeni denetim olaylarını öğrenmek istiyorsanız, aşağıdaki sorguyu kullanın:

```
AuditLogs | where TimeGenerated > ago(3653d) | summarize OldestAuditEvent=min(TimeGenerated), NewestAuditEvent=max(TimeGenerated) by Type
```

Azure Monitor'da denetim etkinlikleri için depolanan [sütunlar](../reports-monitoring/reference-azure-monitor-audit-log-schema.md)hakkında daha fazla bilgi için bkz.

## <a name="create-custom-azure-monitor-queries-using-azure-powershell"></a>Azure PowerShell'i kullanarak özel Azure Monitör sorguları oluşturun

Azure Monitor'a günlük göndermek için Azure AD'yi yapılandırdıktan sonra PowerShell üzerinden günlüklere erişebilirsiniz. Ardından, kiracıda Global Yönetici olmanıza gerek kalmadan komut dosyalarından veya PowerShell komut satırından sorgu gönderin. 

### <a name="ensure-the-user-or-service-principal-has-the-correct-role-assignment"></a>Kullanıcı veya hizmet sorumlusunun doğru rol atamaya sahip olduğundan emin olun

Azure AD kimliğine kimlik tayini yapacak kullanıcı veya hizmet sorumlusunun, Log Analytics çalışma alanında uygun Azure rolünde olduğundan emin olun. Rol seçenekleri Log Analytics Reader veya Log Analytics Katılımcısı'dır. Bu rollerden birindeyseniz, [tek bir Azure aboneliğiyle Günlük Analizi Kimliğini Al'a](#retrieve-log-analytics-id-with-one-azure-subscription)atlayın.

Rol atamasını ayarlamak ve sorgu oluşturmak için aşağıdaki adımları yapın:
1. Azure [portalında, Log Analytics çalışma alanını](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.OperationalInsights%2Fworkspaces
)bulun.

1. **Erişim Denetimi 'ni (IAM)** seçin.

1. Ardından rol ataması eklemek için **Ekle'yi** tıklatın.

    ![Rol ataması ekleyin](./media/entitlement-management-logs-and-reporting/workspace-set-role-assignment.png)

### <a name="install-azure-powershell-module"></a>Azure PowerShell modüllerini yükleyin

Uygun rol atamasını yaptıktan sonra PowerShell'i başlatın ve [Azure PowerShell modüllerini](/powershell/azure/install-az-ps?view=azps-3.3.0) (henüz yapmadıysanız) yazarak yükleyin:

```azurepowershell
install-module -Name az -allowClobber -Scope CurrentUser
```
    
Artık Azure AD'ye kimlik doğrulamaya ve sorguladığınız Log Analytics çalışma alanının kimliğini almaya hazırsınız.

### <a name="retrieve-log-analytics-id-with-one-azure-subscription"></a>Tek bir Azure aboneliğiyle Günlük Analizi Kimliğini alma
Yalnızca tek bir Azure aboneliğiniz ve tek bir Log Analytics çalışma alanınız varsa, Azure AD'de kimlik doğrulaması yapmak, bu aboneğe bağlanmak ve bu çalışma alanını almak için aşağıdakileri yazın:
 
```azurepowershell
Connect-AzAccount
$wks = Get-AzOperationalInsightsWorkspace
```
 
### <a name="retrieve-log-analytics-id-with-multiple-azure-subscriptions"></a>Birden fazla Azure aboneliğiyle Günlük Analizi Kimliği'ni alın

 [Get-AzOperationalInsightsWorkspace](/powershell/module/Az.OperationalInsights/Get-AzOperationalInsightsWorkspace) aynı anda tek bir abonelikle çalışır. Bu nedenle, birden çok Azure aboneliğiniz varsa, Azure AD günlükleriyle Birlikte Günlük Analizi çalışma alanına sahip olan aboneye bağlandığınızdan emin olmak istersiniz. 
 
 Aşağıdaki cmdlets aboneliklerin bir listesini görüntüler ve Log Analytics çalışma alanına sahip aboneliğin kimliğini bulun:
 
```azurepowershell
Connect-AzAccount
$subs = Get-AzSubscription
$subs | ft
```
 
PowerShell oturumunuzu yeniden authenticate ve bu abonelik le `Connect-AzAccount –Subscription $subs[0].id`ilişkilendirebilirsiniz. Etkileşimli olmayan lar da dahil olmak üzere PowerShell'den Azure'a nasıl kimlik doğrulaması yapılacağını öğrenmek için Azure [PowerShell ile Oturum Aç'a](/powershell/azure/authenticate-azureps?view=azps-3.3.0&viewFallbackFrom=azps-2.5.0
)bakın.

Bu abonelikte birden çok Log Analytics çalışma alanınız varsa, cmdlet [Get-AzOperationalInsightsWorkspace](/powershell/module/Az.OperationalInsights/Get-AzOperationalInsightsWorkspace) çalışma alanları listesini döndürür. Ardından Azure AD günlüklerine sahip olanı bulabilirsiniz. Bu `CustomerId` cmdlet tarafından döndürülen alan, Log Analytics çalışma alanına genel bakışta Azure portalında görüntülenen "Çalışma Alanı kimliği" değeriyle aynıdır.
 
```powershell
$wks = Get-AzOperationalInsightsWorkspace
$wks | ft CustomerId, Name
```

### <a name="send-the-query-to-the-log-analytics-workspace"></a>Sorguyu Log Analytics çalışma alanına gönderme
Son olarak, bir çalışma alanı tanımlandıktan sonra, bu çalışma alanına bir Kusto sorgusu göndermek için [Invoke-AzOperationalInsightsQuery'yi](/powershell/module/az.operationalinsights/Invoke-AzOperationalInsightsQuery?view=azps-3.3.0
) kullanabilirsiniz. Bu sorgular [Kusto sorgu dilinde](https://docs.microsoft.com/azure/kusto/query/)yazılır.
 
Örneğin, denetim olay kayıtlarının tarih aralığını Log Analytics çalışma alanından powershell cmdlet'ler gibi bir sorgu göndermek için alabilirsiniz:
 
```powershell
$aQuery = "AuditLogs | where TimeGenerated > ago(3653d) | summarize OldestAuditEvent=min(TimeGenerated), NewestAuditEvent=max(TimeGenerated) by Type"
$aResponse = Invoke-AzOperationalInsightsQuery -WorkspaceId $wks[0].CustomerId -Query $aQuery
$aResponse.Results |ft
```

Ayrıca, şu gibi bir sorgu kullanarak yetkilendirme yönetimi olaylarını da alabilirsiniz:

```azurepowershell
$bQuery = = 'AuditLogs | where Category == "EntitlementManagement"'
$bResponse = Invoke-AzOperationalInsightsQuery -WorkspaceId $wks[0].CustomerId -Query $Query
$bResponse.Results |ft 
```

## <a name="next-steps"></a>Sonraki adımlar:
- [Azure Monitor çalışma kitaplarıyla etkileşimli raporlar oluşturun](../../azure-monitor/app/usage-workbooks.md) 

