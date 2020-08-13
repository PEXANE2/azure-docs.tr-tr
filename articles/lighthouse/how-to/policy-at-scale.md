---
title: Azure Ilkesini, uygun ölçekte ücretsiz aboneliklere dağıtın
description: Azure açık Thouse 'ın, bir ilke tanımı ve ilke atamasını birden çok kiracıya dağıtmanıza nasıl olanak sağladığını öğrenin.
ms.date: 08/12/2020
ms.topic: how-to
ms.openlocfilehash: 27d32a68c1a3806e514533efbae581aa97bc6d0c
ms.sourcegitcommit: c28fc1ec7d90f7e8b2e8775f5a250dd14a1622a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88167292"
---
# <a name="deploy-azure-policy-to-delegated-subscriptions-at-scale"></a>Azure Ilkesini, uygun ölçekte ücretsiz aboneliklere dağıtın

Hizmet sağlayıcı olarak, [Azure açık Thouse](../overview.md)için birden fazla müşteri kiracısından eklendi olabilirsiniz. Azure Mathouse, hizmet sağlayıcılarının aynı anda birkaç kiracıda bir ölçekte işlem gerçekleştirmesine olanak sağlayarak yönetim görevlerini daha verimli hale getirir.

Bu konu başlığı altında, PowerShell komutlarını kullanarak birden çok kiracıya bir ilke tanımı ve ilke ataması dağıtmak için [Azure ilkesi](../../governance/policy/index.yml) 'nin nasıl kullanılacağı gösterilmektedir. Bu örnekte, ilke tanımı, yalnızca HTTPS trafiğine izin vererek depolama hesaplarının güvenli olmasını sağlar.

> [!TIP]
> Bu konudaki hizmet sağlayıcılarına ve müşterilere başvurduğumuz halde, [birden çok kiracıyı yöneten kuruluşlar](../concepts/enterprise.md) aynı işlemlerin aynısını kullanabilir.

## <a name="use-azure-resource-graph-to-query-across-customer-tenants"></a>Müşteri kiracılarının tamamında sorgulama yapmak için Azure Kaynak grafiğini kullanma

Yönettiğiniz müşteri kiracılarındaki tüm aboneliklerde sorgulama yapmak için [Azure Kaynak grafiğini](../../governance/resource-graph/index.yml) kullanabilirsiniz. Bu örnekte, bu aboneliklerde Şu anda HTTPS trafiği gerektirmeyen tüm depolama hesaplarını tanımlayacağız.  

```powershell
$MspTenant = "insert your managing tenantId here"

$subs = Get-AzSubscription

$ManagedSubscriptions = Search-AzGraph -Query "ResourceContainers | where type == 'microsoft.resources/subscriptions' | where tenantId != '$($mspTenant)' | project name, subscriptionId, tenantId" -subscription $subs.subscriptionId

Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Storage/storageAccounts' | project name, location, subscriptionId, tenantId, properties.supportsHttpsTrafficOnly" -subscription $ManagedSubscriptions.subscriptionId | convertto-json
```

## <a name="deploy-a-policy-across-multiple-customer-tenants"></a>Birden çok müşteri kiracısından ilke dağıtma

Aşağıdaki örnekte, birden fazla müşteri kiracısında bir ilke tanımı ve ilke atamasını, temsilci olarak verilen abonelikler arasında dağıtmak için nasıl [Azure Resource Manager](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/policy-enforce-https-storage/enforceHttpsStorage.json) kullanacağınız gösterilmektedir. Bu ilke tanımı, tüm depolama hesaplarının HTTPS trafiği kullanmasını gerektirir ve bu ayar, uyumsuz olarak ayarı olmadan, mevcut depolama hesaplarını uyumlu olmayan ve işaretleyen yeni depolama hesaplarının oluşturulmasını önler.

```powershell
Write-Output "In total, there are $($ManagedSubscriptions.Count) delegated customer subscriptions to be managed"

foreach ($ManagedSub in $ManagedSubscriptions)
{
    Select-AzSubscription -SubscriptionId $ManagedSub.subscriptionId

    New-AzSubscriptionDeployment -Name mgmt `
                     -Location eastus `
                     -TemplateUri "https://raw.githubusercontent.com/Azure/Azure-Lighthouse-samples/master/templates/policy-enforce-https-storage/enforceHttpsStorage.json" `
                     -AsJob
}
```

## <a name="validate-the-policy-deployment"></a>İlke dağıtımını doğrulama

Azure Resource Manager şablonunu dağıttıktan sonra, temsilcili aboneliklerinizden birinde **EnableHttpsTrafficOnly** **olarak ayarlanmış** bir depolama hesabı oluşturmaya çalışırken ilke tanımının başarıyla uygulandığını doğrulayabilirsiniz. İlke ataması nedeniyle, bu depolama hesabını oluşturmamalıdır.  

```powershell
New-AzStorageAccount -ResourceGroupName (New-AzResourceGroup -name policy-test -Location eastus -Force).ResourceGroupName `
                     -Name (get-random) `
                     -Location eastus `
                     -EnableHttpsTrafficOnly $false `
                     -SkuName Standard_LRS `
                     -Verbose                  
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

İşiniz bittiğinde, dağıtım tarafından oluşturulan ilke tanımını ve atamayı kaldırın.

```powershell
foreach ($ManagedSub in $ManagedSubscriptions)
{
    select-azsubscription -subscriptionId $ManagedSub.subscriptionId

    Remove-AzSubscriptionDeployment -Name mgmt -AsJob

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

- [Azure ilkesi](../../governance/policy/index.yml)hakkında bilgi edinin.
- [Çapraz kiracı yönetim deneyimleri](../concepts/cross-tenant-management-experience.md)hakkında bilgi edinin.
