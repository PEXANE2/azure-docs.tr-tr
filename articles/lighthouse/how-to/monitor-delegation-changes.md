---
title: Yönettiğiniz kiracınızdaki yetkilendirme değişikliklerini izleme
description: Müşteri kiracılarından yönetilen kiracınıza olan yetkilendirme etkinliğini izlemeyi öğrenin.
ms.date: 07/10/2020
ms.topic: how-to
ms.openlocfilehash: 63b19f56538f060a158fd665a9bef3bf43a9d087
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86252292"
---
# <a name="monitor-delegation-changes-in-your-managing-tenant"></a>Yönettiğiniz kiracınızdaki yetkilendirme değişikliklerini izleme

Hizmet sağlayıcı olarak, müşteri abonelikleri veya kaynak grupları, kiracınıza [Azure ınthouse](../overview.md)aracılığıyla veya daha önce atanmış kaynaklar kaldırıldığında farkında olmak isteyebilirsiniz.

Yönetim kiracısında [Azure etkinlik günlüğü](../../azure-monitor/platform/platform-logs-overview.md) , yetkilendirme etkinliğini kiracı düzeyinde izler. Günlüğe kaydedilen bu etkinlik, tüm müşteri kiracılarından eklenen veya kaldırılan temsilcileri içerir.

Bu konuda, kiracınıza (tüm müşterileriniz genelinde) ve bunu yapmak için en iyi uygulamalara yönelik olan temsilciyi izlemek için gereken izinler açıklanmaktadır. Ayrıca, bu verileri sorgulamak ve raporlamak için bir yöntem gösteren bir örnek komut dosyası da içerir.

> [!IMPORTANT]
> Bu adımların tümü, tüm müşteri kiracılarında değil, kuruluşunuzda yönetim kiracınızda gerçekleştirilmelidir.

## <a name="enable-access-to-tenant-level-data"></a>Kiracı düzeyindeki verilere erişimi etkinleştir

Kiracı düzeyindeki etkinlik günlüğü verilerine erişmek için bir hesaba, kök kapsamda (/) [Izleme okuyucusu](../../role-based-access-control/built-in-roles.md#monitoring-reader) yerleşik rolü atanmalıdır. Bu atama, ek yükseltilmiş erişimle genel yönetici rolüne sahip bir kullanıcı tarafından gerçekleştirilmelidir.

### <a name="elevate-access-for-a-global-administrator-account"></a>Genel yönetici hesabına erişimi yükseltme

Kök kapsamda (/) bir rol atamak için, yükseltilmiş erişime sahip genel yönetici rolüne sahip olmanız gerekir. Bu yükseltilmiş erişim yalnızca rol ataması yapmanız gerektiğinde eklenmelidir, sonra işiniz bittiğinde kaldırılır.

Yükseltme ekleme ve kaldırma hakkında ayrıntılı yönergeler için, bkz. [tüm Azure aboneliklerini ve Yönetim gruplarını yönetmek için erişimi yükseltme](../../role-based-access-control/elevate-access-global-admin.md).

Erişiminizi yükselttikten sonra hesabınız, Azure 'da kök kapsamdaki Kullanıcı erişimi Yöneticisi rolüne sahip olacaktır. Bu rol ataması, tüm kaynakları görüntülemenize ve dizindeki herhangi bir abonelikte veya yönetim grubunda erişim atamanıza ve ayrıca kök kapsamda rol atamaları yapmanıza olanak sağlar. 

### <a name="create-a-new-service-principal-account-to-access-tenant-level-data"></a>Kiracı düzeyindeki verilere erişmek için yeni bir hizmet sorumlusu hesabı oluşturun

Erişiminizi yükselttikten sonra, kiracı düzeyindeki etkinlik günlüğü verilerini sorgulayabilmesi için ilgili izinleri hesaba atayabilirsiniz. Bu hesabın, yönetim kiracınızın kök kapsamında, [Izleme okuyucusu](../../role-based-access-control/built-in-roles.md#monitoring-reader) yerleşik rolüne atanmış olması gerekir.

> [!IMPORTANT]
> Kök kapsamda rol ataması verilmesi, Kiracıdaki her kaynak için aynı izinlerin uygulanacağı anlamına gelir.

Bu çok geniş bir erişim düzeyi olduğundan, bu rolü tek bir kullanıcı veya gruba göre değil, bir hizmet sorumlusu hesabına atamanızı öneririz. Ayrıca, aşağıdaki en iyi yöntemleri öneririz:

- Bu rolü, diğer otomasyon için kullanılan mevcut bir hizmet sorumlusuna atamak yerine yalnızca bu işlev için kullanılacak [Yeni bir hizmet sorumlusu hesabı oluşturun](../../active-directory/develop/howto-create-service-principal-portal.md) .
- Bu hizmet sorumlusunun, temsilcili müşteri kaynaklarına erişimi olmadığından emin olun.
- Azure Key Vault kimlik doğrulaması yapmak ve güvenli bir şekilde [depolamak](../../key-vault/general/best-practices.md) [Için bir sertifika kullanın](../../active-directory/develop/howto-create-service-principal-portal.md#upload-a-certificate-or-create-a-secret-for-signing-in) .
- Erişimi olan kullanıcıları hizmet sorumlusu adına hareket etmek için sınırlayın.

Kök kapsam atamalarını yapmak için aşağıdaki yöntemlerden birini kullanın.

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

### <a name="remove-elevated-access-for-the-global-administrator-account"></a>Genel yönetici hesabı için yükseltilmiş erişimi kaldırın

Hizmet sorumlusu hesabınızı oluşturduktan ve kök kapsamda Izleme okuyucusu rolünü atadıktan sonra, bu erişim düzeyi artık gerekli olmayacak olduğundan genel yönetici hesabına yönelik [yükseltilmiş erişimi](../../role-based-access-control/elevate-access-global-admin.md#remove-elevated-access) kaldırdığınızdan emin olun.

## <a name="query-the-activity-log"></a>Etkinlik günlüğünü sorgulama

Yönetim kiracınızın kök kapsamına yönelik Izleme okuyucusu erişimi ile yeni bir hizmet sorumlusu hesabı oluşturduktan sonra, kiracınızdaki yetkilendirme etkinliğini sorgulamak ve raporlamak için kullanabilirsiniz. 

[Bu Azure PowerShell betiği](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/tools/monitor-delegation-changes) , eklenen ya da Kaldırılan temsilci (veya başarılı olmayan denemeler) üzerinde etkinlik ve raporların Son 1 gününü sorgulamak için kullanılabilir. [Kiracı etkinlik günlüğü](/rest/api/monitor/TenantActivityLogs/List) verilerini sorgular, sonra eklenen veya kaldırılan temsilcileri raporlamak için aşağıdaki değerleri oluşturur:

- **Delegatedresourceıd**: atanan aboneliğin veya kaynak grubunun kimliği
- **Customertenantıd**: MÜŞTERI Kiracı kimliği
- **Customersubscriptionıd**: temsilci seçilmiş olan veya temsilci atanan kaynak grubunu IÇEREN abonelik kimliği
- **Customerdelegationstatus**: atanan kaynağın durum değişikliği (başarılı veya başarısız)
- **Eventtimestamp**: temsili değişikliğinin kaydedildiği tarih ve saat

Bu verileri sorgularken şunları göz önünde bulundurun:

- Birden çok kaynak grubu tek bir dağıtımda Temsilcili ise, her kaynak grubu için ayrı girdiler döndürülür.
- Önceki bir temsilciyle yapılan değişiklikler (izin yapısını güncelleştirme gibi), eklenen bir temsili olarak günlüğe kaydedilir.
- Yukarıda belirtildiği gibi, bu kiracı düzeyindeki verilere erişebilmek için bir hesabın, kök kapsamda (/) Izleme okuyucusu yerleşik rolüne sahip olması gerekir.
- Bu verileri kendi iş akışlarında ve raporlarınızda kullanabilirsiniz. Örneğin, Azure Izleyici 'de verileri bir REST API istemcisinden günlüğe kaydetmek için [http veri toplayıcı API 'sini (Genel Önizleme)](../../azure-monitor/platform/data-collector-api.md) kullanabilir ve sonra bildirim veya uyarı oluşturmak için [eylem gruplarını](../../azure-monitor/platform/action-groups.md) kullanabilirsiniz.

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

# Azure Lighthouse: Query Tenant Activity Log for registered/unregistered delegations for the last 1 day

$GetDate = (Get-Date).AddDays((-1))

$dateFormatForQuery = $GetDate.ToUniversalTime().ToString("yyyy-MM-ddTHH:mm:ssZ")

# Getting Azure context for the API call
$currentContext = Get-AzContext

# Fetching new token
$azureRmProfile = [Microsoft.Azure.Commands.Common.Authentication.Abstractions.AzureRmProfileProvider]::Instance.Profile
$profileClient = [Microsoft.Azure.Commands.ResourceManager.Common.RMProfileClient]::new($azureRmProfile)
$token = $profileClient.AcquireAccessToken($currentContext.Tenant.Id)

$listOperations = @{
    Uri     = "https://management.azure.com/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&`$filter=eventTimestamp ge '$($dateFormatForQuery)'"
    Headers = @{
        Authorization  = "Bearer $($token.AccessToken)"
        'Content-Type' = 'application/json'
    }
    Method  = 'GET'
}
$list = Invoke-RestMethod @listOperations

# First link can be empty - and point to a next link (or potentially multiple pages)
# While you get more data - continue fetching and add result
while($list.nextLink){
    $list2 = Invoke-RestMethod $list.nextLink -Headers $listOperations.Headers -Method Get
    $data+=$list2.value;
    $list.nextLink = $list2.nextlink;
}

$showOperations = $data;

if ($showOperations.operationName.value -eq "Microsoft.Resources/tenants/register/action") {
    $registerOutputs = $showOperations | Where-Object -FilterScript { $_.eventName.value -eq "EndRequest" -and $_.resourceType.value -and $_.operationName.value -eq "Microsoft.Resources/tenants/register/action" }
    foreach ($registerOutput in $registerOutputs) {
        $eventDescription = $registerOutput.description | ConvertFrom-Json;
    $registerOutputdata = [pscustomobject]@{
        Event                    = "An Azure customer has registered delegated resources to your Azure tenant";
        DelegatedResourceId      = $eventDescription.delegationResourceId; 
        CustomerTenantId         = $eventDescription.subscriptionTenantId;
        CustomerSubscriptionId   = $eventDescription.subscriptionId;
        CustomerDelegationStatus = $registerOutput.status.value;
        EventTimeStamp           = $registerOutput.eventTimestamp;
        }
        $registerOutputdata | Format-List
    }
}
if ($showOperations.operationName.value -eq "Microsoft.Resources/tenants/unregister/action") {
    $unregisterOutputs = $showOperations | Where-Object -FilterScript { $_.eventName.value -eq "EndRequest" -and $_.resourceType.value -and $_.operationName.value -eq "Microsoft.Resources/tenants/unregister/action" }
    foreach ($unregisterOutput in $unregisterOutputs) {
        $eventDescription = $registerOutput.description | ConvertFrom-Json;
    $unregisterOutputdata = [pscustomobject]@{
        Event                    = "An Azure customer has unregistered delegated resources from your Azure tenant";
        DelegatedResourceId      = $eventDescription.delegationResourceId;
        CustomerTenantId         = $eventDescription.subscriptionTenantId;
        CustomerSubscriptionId   = $eventDescription.subscriptionId;
        CustomerDelegationStatus = $unregisterOutput.status.value;
        EventTimeStamp           = $unregisterOutput.eventTimestamp;
        }
        $unregisterOutputdata | Format-List
    }
}
else {
    Write-Output "No new delegation events for tenant: $($currentContext.Tenant.TenantId)"
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure](../concepts/azure-delegated-resource-management.md)'un nasıl kullanılacağına ilişkin müşterileri nasıl kullanacağınızı öğrenin.
- [Azure izleyici](../../azure-monitor/index.yml) ve [Azure etkinlik günlüğü](../../azure-monitor/platform/platform-logs-overview.md)hakkında bilgi edinin.
