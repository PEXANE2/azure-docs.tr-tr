---
title: Azure Blueprints işlevleri
description: Azure Blueprints tanımlarında ve atamalarında plan yapıtlarıyla kullanılabilir işlevleri açıklar.
ms.date: 12/09/2019
ms.topic: reference
ms.openlocfilehash: 0aab2fe0511ccc11842d0e132a83d6e3f7fac27f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79280683"
---
# <a name="functions-for-use-with-azure-blueprints"></a>Azure Planları ile kullanım için işlevler

Azure Blueprints, plan tanımını daha dinamik hale getiren işlevler sağlar. Bu işlevler plan tanımları ve plan yapıları ile kullanılmak üzere. Kaynak Yöneticisi Şablonu yapı, plan parametresi aracılığıyla dinamik bir değer elde etmeye ek olarak Kaynak Yöneticisi işlevlerinin tam kullanımını destekler.

Aşağıdaki işlevler desteklenir:

- [Yapı](#artifacts)
- [Concat](#concat)
- [Parametre](#parameters)
- [resourceGroup](#resourcegroup)
- [resourceGroups](#resourcegroups)
- [Abonelik](#subscription)

## <a name="artifacts"></a>Yapı

`artifacts(artifactName)`

Bu plan yapıları çıktıları ile doldurulan özellikleri bir nesne döndürür.

> [!NOTE]
> İşlev Kaynak `artifacts()` Yöneticisi Şablonu içinden kullanılamaz. İşlev yalnızca plan tanımıJSON'da veya Plan olarak kod olarak [Blueprints'in](https://github.com/Azure/azure-blueprints/blob/master/README.md)bir parçası olarak Azure PowerShell veya REST API ile planı yönetirken JSON yapıda kullanılabilir.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| artifactName |Evet |string |Bir plan objesinin adı. |

### <a name="return-value"></a>Döndürülen değer

Çıktı özelliklerinin nesnesi. **Çıktı özellikleri** başvurulan plan artifakı türüne bağlıdır. Tüm türler biçimi izleyin:

```json
{
  "outputs": {collectionOfOutputProperties}
}
```

#### <a name="policy-assignment-artifact"></a>İlke atama artifakı

```json
{
    "outputs": {
        "policyAssignmentId": "{resourceId-of-policy-assignment}",
        "policyAssignmentName": "{name-of-policy-assignment}",
        "policyDefinitionId": "{resourceId-of-policy-definition}",
    }
}
```

#### <a name="resource-manager-template-artifact"></a>Kaynak Yöneticisi şablonu yapı

Döndürülen nesnenin **çıktı özellikleri** Kaynak Yöneticisi şablonu içinde tanımlanır ve dağıtım tarafından döndürülür.

#### <a name="role-assignment-artifact"></a>Rol atama artifakı

```json
{
    "outputs": {
        "roleAssignmentId": "{resourceId-of-role-assignment}",
        "roleDefinitionId": "{resourceId-of-role-definition}",
        "principalId": "{principalId-role-is-being-assigned-to}",
    }
}
```

### <a name="example"></a>Örnek

Aşağıdaki örnek çıktı özelliğini içeren _kimlik myTemplate Artifact_ içeren bir Kaynak Yöneticisi şablonu yapı:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    ...
    "outputs": {
        "myArray": {
            "type": "array",
            "value": ["first", "second"]
        },
        "myString": {
            "type": "string",
            "value": "my string value"
        },
        "myObject": {
            "type": "object",
            "value": {
                "myProperty": "my value",
                "anotherProperty": true
            }
        }
    }
}
```

_myTemplateArtifact_ örneğinden veri alma nın bazı örnekleri şunlardır:

| İfadeler | Tür | Değer |
|:---|:---|:---|
|`[artifacts("myTemplateArtifact").outputs.myArray]` | Dizi | \["birinci", "ikinci"\] |
|`[artifacts("myTemplateArtifact").outputs.myArray[0]]` | Dize | "ilk" |
|`[artifacts("myTemplateArtifact").outputs.myString]` | Dize | "dize değerim" |
|`[artifacts("myTemplateArtifact").outputs.myObject]` | Nesne | { "myproperty": "benim değeri", "anotherProperty": true } |
|`[artifacts("myTemplateArtifact").outputs.myObject.myProperty]` | Dize | "benim değerim" |
|`[artifacts("myTemplateArtifact").outputs.myObject.anotherProperty]` | Bool | True |

## <a name="concat"></a>concat

`concat(string1, string2, string3, ...)`

Birden çok dize değerlerini birleştirir ve sıkıştırılmış dizeyi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| string1 |Evet |string |Biraraya gelen ilk değer. |
| ek bağımsız değişkenler |Hayır |string |Concatenation için sıralı sırayla ek değerler |

### <a name="return-value"></a>Döndürülen değer

Bir dizi concatenated değerleri.

### <a name="remarks"></a>Açıklamalar

Azure Blueprint işlevi, yalnızca dizeleri ile çalışması açısından Azure Kaynak Yöneticisi şablon işlevinden farklıdır.

### <a name="example"></a>Örnek

`concat(parameters('organizationName'), '-vm')`

## <a name="parameters"></a>parametreler

`parameters(parameterName)`

Plan parametre değeri verir. Belirtilen parametre adı plan tanımında veya plan yapıtlarında tanımlanmalıdır.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| Parametername |Evet |string |Döndürülecek parametrenin adı. |

### <a name="return-value"></a>Döndürülen değer

Belirtilen plan veya plan yapı parametresi değeri.

### <a name="remarks"></a>Açıklamalar

Azure Blueprint işlevi, yalnızca plan parametreleri ile çalışması açısından Azure Kaynak Yöneticisi şablon işlevinden farklıdır.

### <a name="example"></a>Örnek

Plan tanımında parametre _ilkelerini_ tanımlayın:

```json
{
    "type": "Microsoft.Blueprint/blueprints",
    "properties": {
        ...
        "parameters": {
            "principalIds": {
                "type": "array",
                "metadata": {
                    "displayName": "Principal IDs",
                    "description": "This is a blueprint parameter that any artifact can reference. We'll display these descriptions for you in the info bubble. Supply principal IDs for the users,groups, or service principals for the RBAC assignment.",
                    "strongType": "PrincipalId"
                }
            }
        },
        ...
    }
}
```

Daha sonra bir plan artifakı için `parameters()` argüman olarak _principalIds_ kullanın:

```json
{
    "type": "Microsoft.Blueprint/blueprints/artifacts",
    "kind": "roleAssignment",
    ...
    "properties": {
        "roleDefinitionId": "/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
        "principalIds": "[parameters('principalIds')]",
        ...
    }
}
```

## <a name="resourcegroup"></a>resourceGroup

`resourceGroup()`

Geçerli kaynak grubunu temsil eden bir nesne döndürür.

### <a name="return-value"></a>Döndürülen değer

Döndürülen nesne aşağıdaki biçimdedir:

```json
{
  "name": "{resourceGroupName}",
  "location": "{resourceGroupLocation}",
}
```

### <a name="remarks"></a>Açıklamalar

Azure Blueprint işlevi Azure Kaynak Yöneticisi şablon işlevinden farklıdır. İşlev, `resourceGroup()` abonelik düzeyi artifakı veya plan tanımında kullanılamaz. Yalnızca kaynak grubu yapılarının parçası olan plan yapılarında kullanılabilir.

İşlevin `resourceGroup()` yaygın kullanımı kaynak grubu yapı ile aynı konumda kaynak oluşturmaktır.

### <a name="example"></a>Örnek

Kaynak grubunun konumunu kullanmak için, plan tanımında veya atama sırasında başka bir yapının konumu olarak ayarlanmış, plan tanımınızda bir kaynak grubu yer tutucu nesnesi bildirin. Bu örnekte, _NetworkingPlaceholder_ kaynak grubu yer sahibinin adıdır.

```json
{
    "type": "Microsoft.Blueprint/blueprints",
    "properties": {
        ...
        "resourceGroups": {
            "NetworkingPlaceholder": {
                "location": "eastus"
            }
        }
    }
}
```

`resourceGroup()` Ardından, kaynak grubu yer tutucu nesnesini hedefleyen bir plan yapı bağlamında işlevi kullanın. Bu örnekte, şablon yapı _NetworkingPlaceholder_ kaynak grubuna dağıtılır ve şablona _NetworkingPlaceholder_ kaynak grubu konumu ile dinamik olarak doldurulan parametre _kaynağı_ Sağlar. _NetworkingPlaceholder_ kaynak grubunun konumu, plan tanımında statik olarak tanımlanmış veya atama sırasında dinamik olarak tanımlanmış olabilir. Her iki durumda da, şablon yapı bir parametre olarak bu bilgileri sağlanır ve doğru konuma kaynakları dağıtmak için kullanır.

```json
{
  "type": "Microsoft.Blueprint/blueprints/artifacts",
  "kind": "template",
  "properties": {
      "template": {
        ...
      },
      "resourceGroup": "NetworkingPlaceholder",
      ...
      "parameters": {
        "resourceLocation": {
          "value": "[resourceGroup().location]"
        }
      }
  }
}
```

## <a name="resourcegroups"></a>resourceGroups

`resourceGroups(placeholderName)`

Belirtilen kaynak grubu artifakını temsil eden bir nesne döndürür. Yapının bağlamını gerektiren bu işlevin aksine, `resourceGroup()`bu işlev, bu kaynak grubunun bağlamında olmadığında belirli bir kaynak grubu yer tutucunun özelliklerini almak için kullanılır.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| yer tutucuAdı |Evet |string |Döndürülecek kaynak grubu yapının yer tutucu adı. |

### <a name="return-value"></a>Döndürülen değer

Döndürülen nesne aşağıdaki biçimdedir:

```json
{
  "name": "{resourceGroupName}",
  "location": "{resourceGroupLocation}",
}
```

### <a name="example"></a>Örnek

Kaynak grubunun konumunu kullanmak için, plan tanımında veya atama sırasında başka bir yapının konumu olarak ayarlanmış, plan tanımınızda bir kaynak grubu yer tutucu nesnesi bildirin. Bu örnekte, _NetworkingPlaceholder_ kaynak grubu yer sahibinin adıdır.

```json
{
    "type": "Microsoft.Blueprint/blueprints",
    "properties": {
        ...
        "resourceGroups": {
            "NetworkingPlaceholder": {
                "location": "eastus"
            }
        }
    }
}
```

Ardından, `resourceGroups()` kaynak grubu yer tutucu nesnesine başvuruda bulunulmak için herhangi bir plan yapının bağlamından işlevi kullanın. Bu örnekte, şablon yapı _NetworkingPlaceholder_ kaynak grubunun dışında dağıtılır ve şablona _NetworkingPlaceholder_ kaynak grubu konumuyla dinamik olarak doldurulan parametre _artefaktı_ sağlar. _NetworkingPlaceholder_ kaynak grubunun konumu, plan tanımında statik olarak tanımlanmış veya atama sırasında dinamik olarak tanımlanmış olabilir. Her iki durumda da, şablon yapı bir parametre olarak bu bilgileri sağlanır ve doğru konuma kaynakları dağıtmak için kullanır.

```json
{
  "kind": "template",
  "properties": {
      "template": {
          ...
      },
      ...
      "parameters": {
        "artifactLocation": {
          "value": "[resourceGroups('NetworkingPlaceholder').location]"
        }
      }
  },
  "type": "Microsoft.Blueprint/blueprints/artifacts",
  "name": "myTemplate"
}
```

## <a name="subscription"></a>aboneliği

`subscription()`

Geçerli plan ataması için abonelik le ilgili ayrıntıları verir.

### <a name="return-value"></a>Döndürülen değer

Döndürülen nesne aşağıdaki biçimdedir:

```json
{
    "id": "/subscriptions/{subscriptionId}",
    "subscriptionId": "{subscriptionId}",
    "tenantId": "{tenantId}",
    "displayName": "{name-of-subscription}"
}
```

### <a name="example"></a>Örnek

Şablon yapıya parametre `concat()` _kaynağı_ Adı olarak geçirilen bir adlandırma kuralı oluşturmak için aboneliğin görüntü adını ve işlevini kullanın.

```json
{
  "kind": "template",
  "properties": {
      "template": {
          ...
      },
      ...
      "parameters": {
        "resourceName": {
          "value": "[concat(subscription().displayName, '-vm')]"
        }
      }
  },
  "type": "Microsoft.Blueprint/blueprints/artifacts",
  "name": "myTemplate"
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Plan yaşam döngüsü](../concepts/lifecycle.md)hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- Varolan atamaları nasıl [güncelleştirini](../how-to/update-existing-assignments.md)öğrenin.
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin.