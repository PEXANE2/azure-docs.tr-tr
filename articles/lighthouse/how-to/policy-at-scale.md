---
title: Azure İlkesini ölçekte devralınan aboneliklere dağıtma
description: Azure temsilci kaynak yönetiminin bir ilke tanımı nı ve ilke atamasını birden çok kiracıarasında dağıtmanıza nasıl olanak tanıdığını öğrenin.
ms.date: 11/8/2019
ms.topic: conceptual
ms.openlocfilehash: 9e061995b728e2864d1bd33a32d530634ab794d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75456851"
---
# <a name="deploy-azure-policy-to-delegated-subscriptions-at-scale"></a>Azure İlkesini ölçekte devralınan aboneliklere dağıtma

Bir hizmet sağlayıcısı olarak, Azure temsilci kaynak yönetimi için birden çok müşteri kiracısı yerleşik olabilir. [Azure Deniz Feneri,](../overview.md) hizmet sağlayıcıların aynı anda birden fazla kiracı da ölçekte işlem gerçekleştirmesine olanak sağlayarak yönetim görevlerini daha verimli hale getirir.

Bu konu, PowerShell komutlarını kullanarak birden çok kiracıya ilke tanımı ve ilke ataması dağıtmak için [Azure İlkesi'ni](../../governance/policy/index.yml) nasıl kullanacağınızı gösterir. Bu örnekte, ilke tanımı depolama hesaplarının yalnızca HTTPS trafiğine izin vererek güvenli olmasını sağlar.

## <a name="use-azure-resource-graph-to-query-across-customer-tenants"></a>Müşteri kiracıları arasında sorgu yapmak için Azure Kaynak Grafiği'ni kullanma

Yönettiğiniz müşteri kiracılarında tüm abonelikleri sorgulamak için [Azure Kaynak Grafiği'ni](../../governance/resource-graph/index.yml) kullanabilirsiniz. Bu örnekte, bu aboneliklerde şu anda HTTPS trafiği gerektirmeyen depolama hesapları tanımlanır.  

```powershell
$MspTenant = "insert your managing tenantId here"

$subs = Get-AzSubscription

$ManagedSubscriptions = Search-AzGraph -Query "ResourceContainers | where type == 'microsoft.resources/subscriptions' | where tenantId != '$($mspTenant)' | project name, subscriptionId, tenantId" -subscription $subs.subscriptionId

Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Storage/storageAccounts' | project name, location, subscriptionId, tenantId, properties.supportsHttpsTrafficOnly" -subscription $ManagedSubscriptions.subscriptionId | convertto-json
```

## <a name="deploy-a-policy-across-multiple-customer-tenants"></a>Birden çok müşteri kiracısına bir ilke dağıtma

Aşağıdaki örnekte, birden çok müşteri kiracısında temsilci abonelikleri arasında bir ilke tanımı ve ilke ataması dağıtmak için [Azure Kaynak Yöneticisi şablonunun](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/policy-enforce-https-storage/enforceHttpsStorage.json) nasıl kullanılacağı gösterilmektedir. Bu ilke tanımı, tüm depolama hesaplarının HTTPS trafiğini kullanmasını gerektirerek, uymayan yeni depolama hesaplarının oluşturulmasını engeller ve ayar uyumsuz olarak varolan depolama hesaplarını işaretler.

```powershell
Write-Output "In total, there are $($ManagedSubscriptions.Count) delegated customer subscriptions to be managed"

foreach ($ManagedSub in $ManagedSubscriptions)
{
    Select-AzSubscription -SubscriptionId $ManagedSub.subscriptionId

    New-AzDeployment -Name mgmt `
                     -Location eastus `
                     -TemplateUri "https://raw.githubusercontent.com/Azure/Azure-Lighthouse-samples/master/Azure-Delegated-Resource-Management/templates/policy-enforce-https-storage/enforceHttpsStorage.json" `
                     -AsJob
}
```

## <a name="validate-the-policy-deployment"></a>İlke dağıtımını doğrulama

Azure Kaynak Yöneticisi şablonunu dağıttıktan sonra, devredilen aboneliklerinizden birinde **false** olarak ayarlanmış **EnableHttpsTrafficOnly** ile bir depolama hesabı oluşturmaya çalışarak ilke tanımının başarıyla uygulandığını doğrulayabilirsiniz. İlke ataması nedeniyle, bu depolama hesabını oluşturamamalısınız.  

```powershell
New-AzStorageAccount -ResourceGroupName (New-AzResourceGroup -name policy-test -Location eastus -Force).ResourceGroupName `
                     -Name (get-random) `
                     -Location eastus `
                     -EnableHttpsTrafficOnly $false `
                     -SkuName Standard_LRS `
                     -Verbose                  
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

İşi bittiğinde, dağıtım tarafından oluşturulan ilke tanımını ve atamasını kaldırın.

```powershell
foreach ($ManagedSub in $ManagedSubscriptions)
{
    select-azsubscription -subscriptionId $ManagedSub.subscriptionId

    Remove-AzDeployment -Name mgmt -AsJob

    $Assignment = Get-AzPolicyAssignment | where-object {$_.Name -like "enforce-https-storage-assignment"}

    if ([string]::IsNullOrEmpty($Assignment))
    {
        Write-Output "Nothing to clean up - we're done"
    }
    else
    {

    Remove-AzPolicyAssignment -Name 'enforce-https-storage-assignment' -Scope "/subscriptions/$($ManagedSub.subscriptionId)" -Verbose

    Write-Output "Deployment has been deleted - we're done"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure [İlkesi](../../governance/policy/index.yml)hakkında bilgi edinin.
- Kiracılar [arası yönetim deneyimleri](../concepts/cross-tenant-management-experience.md)hakkında bilgi edinin.
