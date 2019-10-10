---
title: PowerShell ile kaynakları denetlemek için ilke oluşturma
description: Uyumlu olmayan kaynakları belirlemek üzere bir Azure Ilkesi ataması oluşturmak için Azure PowerShell kullanın.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/11/2019
ms.topic: quickstart
ms.service: azure-policy
ms.openlocfilehash: 55b7d8509fcf681438b377aa43f5f8aa9bac158d
ms.sourcegitcommit: 1c2659ab26619658799442a6e7604f3c66307a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72255921"
---
# <a name="quickstart-create-a-policy-assignment-to-identify-non-compliant-resources-using-azure-powershell"></a>Hızlı başlangıç: Azure PowerShell kullanarak uyumlu olmayan kaynakları belirlemek için bir ilke ataması oluşturma

Azure 'da uyumluluğu anlamak için ilk adım kaynaklarınızın durumunu belirlemektir. Bu hızlı başlangıçta, yönetilen disk kullanmayan sanal makineleri belirlemek için bir ilke ataması oluşturacaksınız. Bu tamamlandığında, *uyumlu olmayan*sanal makineleri tanımlayacaksınız.

Azure PowerShell modülü, Azure kaynaklarını komut satırından veya betiklerden yönetmek için kullanılır.
Bu kılavuzda, bir ilke ataması oluşturmak için az Module kullanma açıklanmaktadır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="prerequisites"></a>Prerequisites

- Başlamadan önce, Azure PowerShell en son sürümünün yüklü olduğundan emin olun. Ayrıntılı bilgi için bkz. [Azure PowerShell modülünü Install](/powershell/azure/install-az-ps) .
- Azure PowerShell kullanarak Azure Policy Insights kaynak sağlayıcısını kaydedin. Kaynak sağlayıcısını kaydetmek, aboneliğinizin onunla birlikte çalıştığından emin olmanızı sağlar. Kaynak sağlayıcısını kaydetmek için, kaynak sağlayıcısı kaydetme işlemini kaydettirme izninizin olması gerekir. Bu işlem katkıda bulunan ve sahip rollerine dahildir. Kaynak sağlayıcısını kaydetmek için aşağıdaki komutu çalıştırın:

  ```azurepowershell-interactive
  # Register the resource provider if it's not already registered
  Register-AzResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
  ```

  Kaynak sağlayıcılarını kaydetme ve görüntüleme hakkında daha fazla bilgi için bkz. [kaynak sağlayıcıları ve türleri](../../azure-resource-manager/resource-manager-supported-services.md)

## <a name="create-a-policy-assignment"></a>İlke ataması oluşturma

Bu hızlı başlangıçta, *yönetilen diskler olmadan VM 'Leri denetle* tanımı için bir ilke ataması oluşturacaksınız. Bu ilke tanımı, yönetilen diskleri kullanmayan sanal makineleri tanımlar.

Yeni bir ilke ataması oluşturmak için aşağıdaki komutları çalıştırın:

```azurepowershell-interactive
# Get a reference to the resource group that will be the scope of the assignment
$rg = Get-AzResourceGroup -Name '<resourceGroupName>'

# Get a reference to the built-in policy definition that will be assigned
$definition = Get-AzPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Audit VMs that do not use managed disks' }

# Create the policy assignment with the built-in definition against your resource group
New-AzPolicyAssignment -Name 'audit-vm-manageddisks' -DisplayName 'Audit VMs without managed disks Assignment' -Scope $rg.ResourceId -PolicyDefinition $definition
```

Yukarıdaki komutlar aşağıdaki bilgileri kullanır:

- **Ad** -atamanın gerçek adı. Bu örnekte, *audit-VM-manageddisks* kullanılmıştır.
- **DisplayName** -ilke ataması için görünen ad. Bu durumda, *yönetilen disk ataması olmadan VM 'Leri denetleme*kullanıyorsunuz demektir.
- **Tanım** : atamayı oluşturmak için kullandığınız ilke tanımı. Bu durumda, *yönetilen diskleri kullanmayan ilke tanımı denetim VM*'lerinin kimliğidir.
- **Kapsam** -bir kapsam, ilke atamasının hangi kaynaklarda veya gruplandırmaya uygulanacağını belirler. Bir abonelikten kaynak gruplarına kadar değişebilir. @No__t-0scope @ no__t-1 ' i kaynak grubunuzun adıyla değiştirdiğinizden emin olun.

Artık ortamınızın uyumluluk durumunu anlamak için uyumlu olmayan kaynakları belirlemek için hazırsınız demektir.

## <a name="identify-non-compliant-resources"></a>Uyumlu olmayan kaynakları tanımla

Oluşturduğunuz ilke atamasıyla uyumlu olmayan kaynakları belirlemek için aşağıdaki bilgileri kullanın. Aşağıdaki komutları çalıştırın:

```azurepowershell-interactive
# Get the resources in your resource group that are non-compliant to the policy assignment
Get-AzPolicyState -ResourceGroupName $rg.ResourceGroupName -PolicyAssignmentName 'audit-vm-manageddisks' -Filter 'IsCompliant eq false'
```

İlke durumunu alma hakkında daha fazla bilgi için bkz. [Get-AzPolicyState](/powershell/module/az.policyinsights/Get-AzPolicyState).

Sonuçlarınız aşağıdaki örneğe benzer:

```output
Timestamp                   : 3/9/19 9:21:29 PM
ResourceId                  : /subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmId}
PolicyAssignmentId          : /subscriptions/{subscriptionId}/providers/microsoft.authorization/policyassignments/audit-vm-manageddisks
PolicyDefinitionId          : /providers/Microsoft.Authorization/policyDefinitions/06a78e20-9358-41c9-923c-fb736d382a4d
IsCompliant                 : False
SubscriptionId              : {subscriptionId}
ResourceType                : /Microsoft.Compute/virtualMachines
ResourceTags                : tbd
PolicyAssignmentName        : audit-vm-manageddisks
PolicyAssignmentOwner       : tbd
PolicyAssignmentScope       : /subscriptions/{subscriptionId}
PolicyDefinitionName        : 06a78e20-9358-41c9-923c-fb736d382a4d
PolicyDefinitionAction      : audit
PolicyDefinitionCategory    : Compute
ManagementGroupIds          : {managementGroupId}
```

Sonuçlar, Azure portal görünümündeki bir ilke atamasının **kaynak uyumluluğu** sekmesinde gördüklerinize göre eşleşir.

## <a name="clean-up-resources"></a>Kaynakları Temizleme

Oluşturulan atamayı kaldırmak için aşağıdaki komutu kullanın:

```azurepowershell-interactive
# Removes the policy assignment
Remove-AzPolicyAssignment -Name 'audit-vm-manageddisks' -Scope '/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>'
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure ortamınızda uyumlu olmayan kaynakları belirlemek için bir ilke tanımı atadınız.

Yeni kaynakların uyumlu olduğunu doğrulamak üzere ilke atama hakkında daha fazla bilgi edinmek için öğreticiye geçin:

> [!div class="nextstepaction"]
> [İlke oluşturma ve yönetme](./tutorials/create-and-manage.md)