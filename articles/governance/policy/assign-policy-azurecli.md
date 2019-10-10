---
title: Azure CLı ile kaynakları denetlemek için ilke oluşturma
description: Uyumlu olmayan kaynakları belirlemek üzere bir Azure Ilkesi ataması oluşturmak için Azure CLı 'yi kullanın.
author: DCtheGeek
ms.author: dacoulte
ms.date: 01/23/2019
ms.topic: quickstart
ms.service: azure-policy
ms.openlocfilehash: a7e65e304039c266f92f8068b796323197058cc1
ms.sourcegitcommit: 1c2659ab26619658799442a6e7604f3c66307a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72255949"
---
# <a name="quickstart-create-a-policy-assignment-to-identify-non-compliant-resources-with-azure-cli"></a>Hızlı başlangıç: Azure CLı ile uyumlu olmayan kaynakları belirlemek için bir ilke ataması oluşturma

Azure 'da uyumluluğu anlamak için ilk adım kaynaklarınızın durumunu belirlemektir.
Bu hızlı başlangıç, yönetilen diskleri kullanmayan sanal makineleri belirlemek için bir ilke ataması oluşturma işleminde size kılavuzluk edecek.

Bu işlemin sonunda, yönetilen disk kullanmayan sanal makineleri başarıyla tanımlayacaksınız. İlke atamasıyla *uyumlu değildir* .

Azure CLı, komut satırından veya betiklerden Azure kaynakları oluşturmak ve yönetmek için kullanılır. Bu kılavuz, Azure ortamınızda bir ilke ataması oluşturmak ve uyumlu olmayan kaynakları belirlemek için Azure CLı kullanır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Bu hızlı başlangıç, CLı 'yi yerel olarak yüklemek ve kullanmak için Azure CLı sürüm 2.0.4 veya üstünü çalıştırmanızı gerektirir. Sürümü bulmak için `az --version` ' ı çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse bkz. [Azure CLI 'Yı yüklemek](/cli/azure/install-azure-cli).

## <a name="prerequisites"></a>Prerequisites

Azure CLı kullanarak Azure Policy Insights kaynak sağlayıcısını kaydedin. Kaynak sağlayıcısını kaydetmek, aboneliğinizin onunla birlikte çalıştığından emin olmanızı sağlar. Kaynak sağlayıcısını kaydetmek için, kaynak sağlayıcısı kaydetme işlemini kaydettirme izninizin olması gerekir. Bu işlem katkıda bulunan ve sahip rollerine dahildir. Kaynak sağlayıcısını kaydetmek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az provider register --namespace 'Microsoft.PolicyInsights'
```

Kaynak sağlayıcılarını kaydetme ve görüntüleme hakkında daha fazla bilgi için bkz. [kaynak sağlayıcıları ve türleri](../../azure-resource-manager/resource-manager-supported-services.md)

Henüz yapmadıysanız, [Armclient](https://github.com/projectkudu/ARMClient)' ı yüklemeyin. Azure Resource Manager tabanlı API 'lere HTTP istekleri gönderen bir araçtır.

## <a name="create-a-policy-assignment"></a>İlke ataması oluşturma

Bu hızlı başlangıçta, bir ilke ataması oluşturup **yönetilen diskler tanımını kullanmayan denetim sanal** makinelerini atarsınız. Bu ilke tanımı, ilke tanımında ayarlanan koşullarla uyumlu olmayan kaynakları belirler.

Bir ilke ataması oluşturmak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az policy assignment create --name 'audit-vm-manageddisks' --display-name 'Audit VMs without managed disks Assignment' --scope '<scope>' --policy '<policy definition ID>'
```

Yukarıdaki komut aşağıdaki bilgileri kullanır:

- **Ad** -atamanın gerçek adı. Bu örnekte, *audit-VM-manageddisks* kullanılmıştır.
- **DisplayName** -ilke ataması için görünen ad. Bu durumda, *yönetilen disk ataması olmadan VM 'Leri denetleme*kullanıyorsunuz demektir.
- **İlke** – atamayı oluşturmak için kullandığınız Ilke tanım kimliği. Bu durumda, *yönetilen diskleri kullanmayan ilke tanımı denetim VM*'lerinin kimliğidir. İlke tanımı KIMLIĞINI almak için şu komutu çalıştırın: `az policy definition list --query "[?displayName=='Audit VMs that do not use managed disks']"`
- **Kapsam** -bir kapsam, ilke atamasının hangi kaynaklarda veya gruplandırmaya uygulanacağını belirler. Bir abonelikten kaynak gruplarına kadar değişebilir. @No__t-0scope @ no__t-1 ' i kaynak grubunuzun adıyla değiştirdiğinizden emin olun.

## <a name="identify-non-compliant-resources"></a>Uyumlu olmayan kaynakları tanımla

Bu yeni atama altında uyumlu olmayan kaynakları görüntülemek için aşağıdaki komutları çalıştırarak ilke atama KIMLIĞINI alın:

```azurepowershell-interactive
$policyAssignment = Get-AzPolicyAssignment | Where-Object { $_.Properties.DisplayName -eq 'Audit VMs without managed disks Assignment' }
$policyAssignment.PolicyAssignmentId
```

İlke atama kimlikleri hakkında daha fazla bilgi için bkz. [Get-AzPolicyAssignment](/powershell/module/az.resources/get-azpolicyassignment).

Ardından, bir JSON dosyasına çıktı olan uyumlu olmayan kaynakların kaynak kimliklerini almak için aşağıdaki komutu çalıştırın:

```console
armclient post "/subscriptions/<subscriptionID>/resourceGroups/<rgName>/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2017-12-12-preview&$filter=IsCompliant eq false and PolicyAssignmentId eq '<policyAssignmentID>'&$apply=groupby((ResourceId))" > <json file to direct the output with the resource IDs into>
```

Sonuçlarınız aşağıdaki örneğe benzer:

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest",
    "@odata.count": 3,
    "value": [{
            "@odata.id": null,
            "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
            "ResourceId": "/subscriptions/<subscriptionId>/resourcegroups/<rgname>/providers/microsoft.compute/virtualmachines/<virtualmachineId>"
        },
        {
            "@odata.id": null,
            "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
            "ResourceId": "/subscriptions/<subscriptionId>/resourcegroups/<rgname>/providers/microsoft.compute/virtualmachines/<virtualmachine2Id>"
        },
        {
            "@odata.id": null,
            "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
            "ResourceId": "/subscriptions/<subscriptionName>/resourcegroups/<rgname>/providers/microsoft.compute/virtualmachines/<virtualmachine3ID>"
        }

    ]
}
```

Sonuçlar, genellikle Azure portal görünümünde **uyumlu olmayan kaynaklar** altında listelenmiş olarak göreceğiniz şekilde karşılaştırılabilir.

## <a name="clean-up-resources"></a>Kaynakları Temizleme

Oluşturulan atamayı kaldırmak için aşağıdaki komutu kullanın:

```azurecli-interactive
az policy assignment delete --name 'audit-vm-manageddisks' --scope '/subscriptions/<subscriptionID>/<resourceGroupName>'
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure ortamınızda uyumlu olmayan kaynakları belirlemek için bir ilke tanımı atadınız.

Yeni kaynakların uyumlu olduğunu doğrulamak üzere ilke atama hakkında daha fazla bilgi edinmek için öğreticiye geçin:

> [!div class="nextstepaction"]
> [İlke oluşturma ve yönetme](./tutorials/create-and-manage.md)