---
title: 'Hızlı başlangıç: REST API yeni ilke ataması'
description: Bu hızlı başlangıçta, uyumlu olmayan kaynakları belirlemek üzere bir Azure Ilkesi ataması oluşturmak için REST API kullanırsınız.
ms.date: 08/10/2020
ms.topic: quickstart
ms.openlocfilehash: 04880ef013060bc5ff12618af6a9156295a26a88
ms.sourcegitcommit: 1aef4235aec3fd326ded18df7fdb750883809ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2020
ms.locfileid: "88136531"
---
# <a name="quickstart-create-a-policy-assignment-to-identify-non-compliant-resources-with-rest-api"></a>Hızlı başlangıç: REST API ile uyumlu olmayan kaynakları belirlemek için bir ilke ataması oluşturma

Azure’da uyumluluğu anlamanın ilk adımı, kaynaklarınızın durumunu belirlemektir.
Bu hızlı başlangıç, yönetilen disk kullanmayan sanal makineleri belirlemek üzere ilke ataması oluşturma işleminde size yol gösterir.

Bu işlemin sonunda, yönetilen disk kullanmayan sanal makineleri başarılı bir şekilde belirlemiş olacaksınız. Bu sanal makineler, ilke ataması ile _uyumsuzdur_.

REST API, Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu kılavuz, bir ilke ataması oluşturmak ve Azure ortamınızda uyumlu olmayan kaynakları belirlemek için REST API kullanır.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

- Henüz yapmadıysanız, [Armclient](https://github.com/projectkudu/ARMClient)' ı yüklemeyin. Azure Resource Manager tabanlı REST API 'Lerine HTTP istekleri gönderen bir araçtır. Alternatif olarak, bekleyen belgelerde "dene" özelliğini veya PowerShell 'in [Invoke-RestMethod](/powershell/module/microsoft.powershell.utility/invoke-restmethod) veya [Postman](https://www.postman.com)gibi araçları kullanabilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-policy-assignment"></a>İlke ataması oluşturma

Bu hızlı başlangıçta, bir ilke ataması oluşturup yönetilen diskler () tanımını **kullanmayan denetim sanal** makinelerini atarsınız `06a78e20-9358-41c9-923c-fb736d382a4d` . Bu ilke tanımı, ilke tanımında ayarlanan koşullarla uyumlu olmayan kaynakları belirler.

İlke ataması oluşturmak için aşağıdaki komutu çalıştırın:

   - REST API URI'si

     ```http
     PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/audit-vm-manageddisks?api-version=2019-09-01
     ```

   - İstek Gövdesi

     ```json
     {
       "properties": {
         "displayName": "Audit VMs without managed disks Assignment",
         "description": "Shows all virtual machines not using managed disks",
         "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/06a78e20-9358-41c9-923c-fb736d382a4d",
       }
     }
     ```

Önceki uç nokta ve istek gövdesi aşağıdaki bilgileri kullanır:

REST API URı 'SI:
- **Kapsam** - Kapsam, ilke atamasının hangi kaynaklarda veya kaynak gruplarında uygulanacağını belirler. Bir yönetim grubundan tek bir kaynağa kadar değişebilir. `{scope}`Aşağıdaki desenlerden biriyle değiştirdiğinizden emin olun:
  - Yönetim grubu:`/providers/Microsoft.Management/managementGroups/{managementGroup}`
  - Aboneliğiniz`/subscriptions/{subscriptionId}`
  - Kaynak grubu: `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}`
  - Kaynak`/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/[{parentResourcePath}/]{resourceType}/{resourceName}`
- **Ad** - Atamanın gerçek adı. Bu örnekte _audit-vm-manageddisks_ kullanıldı.

İstek gövdesi:
- **Görünen Ad** - Bu ilke atamasının görünen adı. Bu durumda, _yönetilen disk ataması olmadan VM 'Leri denetleme_kullanıyorsunuz demektir.
- **Açıklama** -ilkenin ne yaptığını veya neden bu kapsama atandığını daha derin bir açıklama.
- **Policydefinitionıd** : atamayı oluşturmak için kullandığınız Ilke tanım kimliği. Bu durumda, _yönetilen diskleri kullanmayan ilke tanımı denetim VM_'lerinin kimliğidir.

## <a name="identify-non-compliant-resources"></a>Uyumlu olmayan kaynakları belirleme

Bu yeni atama altında uyumlu olmayan kaynakları görüntülemek için, bir JSON dosyasına çıktı olan uyumlu olmayan kaynakların kaynak kimliklerini almak için aşağıdaki komutu çalıştırın:

```http
POST https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/06a78e20-9358-41c9-923c-fb736d382a4d/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2019-10-01&$filter=IsCompliant eq false and PolicyAssignmentId eq 'audit-vm-manageddisks'&$apply=groupby((ResourceId))"
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
            "ResourceId": "/subscriptions/<subscriptionName>/resourcegroups/<rgname>/providers/microsoft.compute/virtualmachines/<virtualmachine3Id>"
        }

    ]
}
```

Sonuçlar, Azure portalı görünümünde **Uyumlu olmayan kaynaklar** bölümünde gördüklerinize benzer.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturulan atamayı kaldırmak için aşağıdaki komutu kullanın:

```http
DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/audit-vm-manageddisks?api-version=2019-09-01
```

`{scope}`İlke atamasını ilk oluşturduğunuzda kullandığınız kapsamla değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure ortamınızda uyumlu olmayan kaynakları belirlemek üzere bir ilke tanımı atadınız.

Yeni kaynakların uyumlu olduğunu doğrulamak üzere ilke atama hakkında daha fazla bilgi edinmek için öğreticiye geçin:

> [!div class="nextstepaction"]
> [İlke oluşturma ve yönetme](./tutorials/create-and-manage.md)
