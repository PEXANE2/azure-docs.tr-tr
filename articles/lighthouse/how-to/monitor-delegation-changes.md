---
title: Yönetici kiracınızdaki temsilci değişikliklerini izleme
description: Müşteri kiracılarından yönetici kiracınıza kadar temsilcilik etkinliğini nasıl izleyeceğinizi öğrenin.
ms.date: 03/30/2020
ms.topic: conceptual
ms.openlocfilehash: 82c41c392210e088c85af510b9698e0140f660e5
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80421899"
---
# <a name="monitor-delegation-changes-in-your-managing-tenant"></a>Yönetici kiracınızdaki temsilci değişikliklerini izleme

Bir hizmet sağlayıcısı olarak, müşteri abonelikleri veya kaynak grupları [Azure temsilcikaynak yönetimi](../concepts/azure-delegated-resource-management.md)aracılığıyla kiracınıza verildiğinde veya daha önce devredilen kaynaklar kaldırıldığında farkında olmak isteyebilirsiniz.

Yönetici kiracıda, [Azure etkinlik günlüğü](../../azure-monitor/platform/platform-logs-overview.md) temsilcilik etkinliğini kiracı düzeyinde izler. Bu günlüğe kaydedilmiş etkinlik, tüm müşteri kiracılarından eklenen veya kaldırılan tüm delegasyonları içerir.

Bu konu, kiracınıza (tüm müşterilerinizarasında) temsilcilik etkinliğini izlemek için gereken izinleri ve bunu yapmak için en iyi uygulamaları açıklar. Ayrıca, bu verileri sorgulamak ve raporlamak için bir yöntem gösteren bir örnek komut dosyası içerir.

> [!IMPORTANT]
> Tüm bu adımlar, müşteri kiracıları yerine yönetici kiracınızda gerçekleştirilmelidir.

## <a name="enable-access-to-tenant-level-data"></a>Kiracı düzeyindeki verilere erişimi etkinleştirme

Kiracı düzeyindeki Etkinlik Günlüğü verilerine erişmek için, bir hesaba kök kapsamda (/) yerleşik [İzleme Okuyucurolü](../../role-based-access-control/built-in-roles.md#monitoring-reader) atanması gerekir. Bu atama, ek yüksek erişime sahip Genel Yönetici rolüne sahip bir kullanıcı tarafından gerçekleştirilmelidir.

### <a name="elevate-access-for-a-global-administrator-account"></a>Global Yönetici hesabı için erişimi yükseltme

Kök kapsamda bir rol atamak için (/), yüksek erişime sahip Genel Yönetici rolüne sahip olmanız gerekir. Bu yükseltilmiş erişim yalnızca rol ataması yapmanız gerektiğinde eklenmeli, sonra da bittiğinde kaldırılmalıdır.

Yükseklik ekleme ve kaldırma yla ilgili ayrıntılı talimatlar için [bkz.](../../role-based-access-control/elevate-access-global-admin.md)

Erişiminizi yükselttikten sonra, hesabınızkök kapsamında Azure'da Kullanıcı Erişim Yöneticisi rolüne sahip olur. Bu rol ataması, tüm kaynakları görüntülemenize ve dizindeki herhangi bir abonelik veya yönetim grubuna erişim atamanın yanı sıra kök kapsamda rol atamaları yapmanızı sağlar. 

### <a name="create-a-new-service-principal-account-to-access-tenant-level-data"></a>Kiracı düzeyindeki verilere erişmek için yeni bir hizmet ana hesabı oluşturma

Erişiminizi yükselttikten sonra, kiracı düzeyindeki etkinlik günlüğü verilerini sorgulayabilmesi için bir hesaba uygun izinleri atayabilirsiniz. Bu hesabın, yönetici kiracınızın temel kapsamına atanmış [İzleme Okuyucusu](../../role-based-access-control/built-in-roles.md#monitoring-reader) yerleşik rolünün olması gerekir.

> [!IMPORTANT]
> Kök kapsamda bir rol ataması verilmesi, aynı izinlerin kiracıdaki her kaynak için geçerli olacağı anlamına gelir.

Bu geniş bir erişim düzeyi olduğundan, bu rolü tek bir kullanıcıya veya bir gruba değil, bir hizmet ana hesabına atamanızı öneririz. Ayrıca, aşağıdaki en iyi uygulamaları öneririz:

- Bu rolü diğer otomasyon için kullanılan varolan bir hizmet ilkesine atamak yerine, yalnızca bu işlev için kullanılacak yeni bir [hizmet anahesabı oluşturun.](../../active-directory/develop/howto-create-service-principal-portal.md)
- Bu hizmet sorumlusunun devralınan müşteri kaynaklarına erişimi olmadığından emin olun.
- [Sertifikayı doğrulamak](../../active-directory/develop/howto-create-service-principal-portal.md#certificates-and-secrets) ve [Azure Key Vault'ta güvenli bir şekilde depolamak için](../../key-vault/key-vault-best-practices.md)kullanın.
- Erişim erişimi olan kullanıcıları hizmet ilkesi adına hareket etmek için sınırlandırın.

Kök kapsam atamaları yapmak için aşağıdaki yöntemlerden birini kullanın.

#### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

New-AzRoleAssignment -SignInName <yourLoginName> -Scope "/" -RoleDefinitionName "Monitoring Reader"  -ApplicationId $servicePrincipal.ApplicationId 
```

#### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az role assignment create --assignee 00000000-0000-0000-0000-000000000000 --role "Monitoring Reader" --scope "/"
```

### <a name="remove-elevated-access-for-the-global-administrator-account"></a>Global Administrator hesabı için yükseltilmiş erişimi kaldırma

Hizmet ana hesabınızı oluşturduktan ve Izleme Okuyucurolünü kök kapsamda atadıktan sonra, bu erişim düzeyi artık gerekmediğinden, Global Administrator hesabı için [yükseltilmiş erişimi kaldırdığınızdan](../../role-based-access-control/elevate-access-global-admin.md#remove-elevated-access) emin olun.

## <a name="query-the-activity-log"></a>Etkinlik günlüğünü sorgula

Yönetici kiracınızın kök kapsamına İzleme Okuyucusu erişimi içeren yeni bir hizmet ana hesabı oluşturduktan sonra, bunu kiracınızdaki temsilcilik etkinliğini sorgulamak ve raporlamak için kullanabilirsiniz. 

[Bu Azure PowerShell komut dosyası,](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/tools/monitor-delegation-changes) eklenen veya kaldırılan tümleçlerle (veya başarısız olan girişimler) son 1 günlük etkinlik ve raporları sorgulamak için kullanılabilir. Kiracı Etkinlik [Günlüğü](https://docs.microsoft.com/rest/api/monitor/TenantActivityLogs/List) verilerini sorgular, ardından eklenen veya kaldırılan delegasyonlar hakkında rapor vermek için aşağıdaki değerleri kurar:

- **DelegatedResourceId**: Devredilen aboneliğin veya kaynak grubunun kimliği
- **CustomerTenantId**: Müşteri kiracı kimliği
- **CustomerSubscriptionId**: Devredilen veya devredilen kaynak grubunu içeren abonelik kimliği
- **Müşteri Temsilci Durumu**: Devredilen kaynağın durum değişikliği (başarılı veya başarısız)
- **EventTimeStamp**: Delegasyon değişikliğinin günlüğe kaydedildiği tarih ve saat

Bu verileri sorgularken, aklınızda bulundurun:

- Tek bir dağıtımda birden çok kaynak grubu devredilirse, her kaynak grubu için ayrı girişler döndürülür.
- Önceki bir delegasyonda yapılan değişiklikler (izin yapısının güncelleştirilmesi gibi) ek bir temsilci olarak günlüğe kaydedilir.
- Yukarıda belirtildiği gibi, bir hesabın bu kiracı düzeyindeki verilere erişmek için kök kapsamda (/) yerleşik Olarak İzleme Okuyucurolüne sahip olması gerekir.
- Bu verileri kendi iş akışlarınızda ve raporlamanızda kullanabilirsiniz. Örneğin, bir REST API istemcisinden Azure Monitor'a veri günlüğe kaydetmek için [HTTP Veri Toplayıcı API'sını (genel önizleme)](../../azure-monitor/platform/data-collector-api.md) kullanabilir, ardından bildirim veya uyarı oluşturmak için [eylem gruplarını](../../azure-monitor/platform/action-groups.md) kullanabilirsiniz.

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

# Azure Lighthouse: Query Tenant Activity Log for registered/unregistered delegations for the past 1 day

$GetDate = (Get-Date).AddDays((-1))

$dateFormatForQuery = $GetDate.ToUniversalTime().ToString("yyyy-MM-ddTHH:mm:ssZ")

# Getting Azure context for the API call
$currentContext = Get-AzContext

# Fetching new token
$azureRmProfile = [Microsoft.Azure.Commands.Common.Authentication.Abstractions.AzureRmProfileProvider]::Instance.Profile
$profileClient = [Microsoft.Azure.Commands.ResourceManager.Common.RMProfileClient]::new($azureRmProfile)
$token = $profileClient.AcquireAccessToken($currentContext.Tenant.Id)

$listOperations = @{
    Uri = "https://management.azure.com/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&`$filter=eventTimestamp ge '$($dateFormatForQuery)'"
    Headers = @{
        Authorization = "Bearer $($token.AccessToken)"
        'Content-Type' = 'application/json'
    }
    Method = 'GET'
}
$list = Invoke-RestMethod @listOperations
$showOperations = $list.value

if ($showOperations.operationName.value -eq "Microsoft.Resources/tenants/register/action")
{
    $registerOutputs  = $showOperations | Where-Object -FilterScript {$_.eventName.value -eq "EndRequest" -and $_.resourceType.value -and $_.operationName.value -eq "Microsoft.Resources/tenants/register/action"}
    foreach ($registerOutput in $registerOutputs)
    {
    $registerOutputdata = [pscustomobject]@{
        Event = "An Azure customer has delegated resources to your tenant";
        DelegatedResourceId = $registerOutput.description |%{$_.split('"')[11]};
        CustomerTenantId = $registerOutput.description |%{$_.split('"')[7]};
        CustomerSubscriptionId = $registerOutput.subscriptionId;
        CustomerDelegationStatus = $registerOutput.status.value;
        EventTimeStamp = $registerOutput.eventTimestamp;
        }
        $registerOutputdata | Format-List
    }
}
if ($showOperations.operationName.value -eq "Microsoft.Resources/tenants/unregister/action") 
{
    $unregisterOutputs  = $showOperations | Where-Object -FilterScript {$_.eventName.value -eq "EndRequest" -and $_.resourceType.value -and $_.operationName.value -eq "Microsoft.Resources/tenants/unregister/action"}
    foreach ($unregisterOutput in $unregisterOutputs)
    {
    $unregisterOutputdata = [pscustomobject]@{
        Event = "An Azure customer has removed delegated resources from your tenant";
        DelegatedResourceId = $unregisterOutput.description |%{$_.split('"')[11]};
        CustomerTenantId = $unregisterOutput.description |%{$_.split('"')[7]};
        CustomerSubscriptionId = $unregisterOutput.subscriptionId;
        CustomerDelegationStatus = $unregisterOutput.status.value;
        EventTimeStamp = $unregisterOutput.eventTimestamp;
        }
        $unregisterOutputdata | Format-List
    }
}
else 
{
    Write-Output "No new delegation changes."
}   


```

## <a name="next-steps"></a>Sonraki adımlar

- Azure [temsilcikaynak yönetimine](../concepts/azure-delegated-resource-management.md)müşterilerin nasıl bindirilen öğrenin.
- Azure [Monitörü](../../azure-monitor/index.yml) ve [Azure etkinlik günlüğü](../../azure-monitor/platform/platform-logs-overview.md)hakkında bilgi edinin.
