---
title: Kaynak kilitlemeyi anlama
description: Bir plan atarken kaynakları korumak için Azure Planları'ndaki kilitleme seçenekleri hakkında bilgi edinin.
ms.date: 03/25/2020
ms.topic: conceptual
ms.openlocfilehash: 94ed8efd0d6c654cba129dfc69fbfe5add7a0824
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81383603"
---
# <a name="understand-resource-locking-in-azure-blueprints"></a>Azure Planları'nda kaynak kilitlemeyi anlama

Ölçekte tutarlı ortamlar oluşturulması ancak bu tutarlılığı koruyacak bir mekanizma varsa gerçekten değerlidir. Bu makalede, Azure Planları'nda kaynak kilitlemenin nasıl çalıştığı açıklanmaktadır. Kaynak kilitleme ve _reddet atamalarının_uygulanmasına bir örnek görmek için, [koruma yeni kaynaklar](../tutorials/protect-new-resources.md) öğreticisine bakın.

> [!NOTE]
> Azure Planları tarafından dağıtılan kaynak kilitleri yalnızca plan ataması tarafından dağıtılan kaynaklara uygulanır. Kaynak gruplarında zaten var olanlar gibi varolan kaynakların kilidi eklenmez.

## <a name="locking-modes-and-states"></a>Kilitleme modları ve durumları

Kilitleme Modu plan ataması için geçerlidir ve üç seçeneği vardır: **Kilitleme ,** **Salt Oku**, veya **Silmeyin**. Kilitleme modu, bir plan ataması sırasında yapılandırma sırasında yapılandırılır. Plan ataması güncelleştirilerek farklı bir kilitleme modu ayarlanabilir.
Ancak kilitleme modları Azure Planları dışında değiştirilemez.

Bir plan atamasında eserler tarafından oluşturulan kaynaklar dört durum vardır: **Kilitli Değil**, **Yalnızca Oku**, **Düzenleyemiyor / Sil,** veya **Silemez**. Her yapı türü **Kilitli Olmayan** durumda olabilir. Aşağıdaki tablo, kaynağın durumunu belirlemek için kullanılabilir:

|Mod|Artefakt Kaynak Türü|Durum|Açıklama|
|-|-|-|-|
|Kilitleme|*|Kilitli Değil|Kaynaklar Azure Planları tarafından korunmaz. Bu durum, bir plan ataması dışından bir **Salt Oku** veya **Silme yat kaynak** grubu yapısına eklenen kaynaklar için de kullanılır.|
|Salt Okunur|Kaynak grubu|Düzenleyemiyor / Sil|Kaynak grubu yalnızca okunur ve kaynak grubundaki etiketler değiştirilemez. **Kilitli olmayan** kaynaklar bu kaynak grubundan eklenebilir, taşınabilir, değiştirilebilir veya silinebilir.|
|Salt Okunur|Kaynak dışı grup|Salt Okunur|Kaynak hiçbir şekilde değiştirilemez -- değişiklik olmaz ve silinemez.|
|Silme|*|Silemez|Kaynaklar değiştirilebilir, ancak silinemez. **Kilitli olmayan** kaynaklar bu kaynak grubundan eklenebilir, taşınabilir, değiştirilebilir veya silinebilir.|

## <a name="overriding-locking-states"></a>Kilitleme durumlarını geçersiz kılma

Abonelikte uygun [rol tabanlı erişim denetimine](../../../role-based-access-control/overview.md) (RBAC) sahip birinin (RBAC) 'Sahip' rolü gibi herhangi bir kaynağı değiştirmesine veya silmesine izin verilmesi genellikle mümkündür. Azure Blueprints dağıtılan bir atamanın bir parçası olarak kilitleme uyguladığında bu erişim söz konusu değildir. Atama **Yalnızca Oku** veya Silme **seçeneğiyle** ayarlanmışsa, abonelik sahibi bile engellenen eylemi korumalı kaynak üzerinde gerçekleştiremez.

Bu güvenlik önlemi, tanımlanan planın tutarlılığını ve yanlışlıkla veya programlı silme veya değiştirmeden oluşturmak üzere tasarlandığı ortamı korur.

### <a name="assign-at-management-group"></a>Yönetim grubunda atama

Abonelik sahiplerinin plan atamasını kaldırmasını önlemek için ek bir seçenek, planı bir yönetim grubuna atamaktır. Bu senaryoda, yalnızca yönetim grubunun **sahipleri** plan atamasını kaldırmak için gereken izinlere sahiptir.

Planı abonelik yerine bir yönetim grubuna atamak için, REST API değişiklikleri aşağıdaki gibi görünmesi için çağırır:

```http
PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{assignmentMG}/providers/Microsoft.Blueprint/blueprintAssignments/{assignmentName}?api-version=2018-11-01-preview
```

Tarafından `{assignmentMG}` tanımlanan yönetim grubu, yönetim grubu hiyerarşisi içinde olmalı veya plan tanımının kaydedildiği aynı yönetim grubu olmalıdır.

Plan atamasının istek gövdesi aşağıdaki gibi görünür:

```json
{
    "identity": {
        "type": "SystemAssigned"
    },
    "location": "eastus",
    "properties": {
        "description": "enforce pre-defined simpleBlueprint to this XXXXXXXX subscription.",
        "blueprintId": "/providers/Microsoft.Management/managementGroups/{blueprintMG}/providers/Microsoft.Blueprint/blueprints/simpleBlueprint",
        "scope": "/subscriptions/{targetSubscriptionId}",
        "parameters": {
            "storageAccountType": {
                "value": "Standard_LRS"
            },
            "costCenter": {
                "value": "Contoso/Online/Shopping/Production"
            },
            "owners": {
                "value": [
                    "johnDoe@contoso.com",
                    "johnsteam@contoso.com"
                ]
            }
        },
        "resourceGroups": {
            "storageRG": {
                "name": "defaultRG",
                "location": "eastus"
            }
        }
    }
}
```

Bu istek gövdesindeki ve aboneliğe atanan temel `properties.scope` fark, özelliktir. Bu gerekli özellik, plan atamasının geçerli olduğu aboneliğe ayarlanmalıdır. Abonelik, plan atamasının depolandığı yönetim grubu hiyerarşisinin doğrudan bir alt nedeni olmalıdır.

> [!NOTE]
> Yönetim grubu kapsamına atanan bir plan hala bir abonelik düzeyi planı ataması olarak çalışır. Tek fark, abonelik sahiplerinin atamayı ve ilişkili kilitleri kaldırmasını önlemek için plan atamasının depolandığı yerdir.

## <a name="removing-locking-states"></a>Kilitleme durumlarını kaldırma

Bir atama tarafından korunan bir kaynağı değiştirmek veya silmek gerekli hale gelirse, bunu yapmanın iki yolu vardır.

- Plan atamasını **Kilitle'nin** kilitleme moduna güncelleştirme
- Plan atamasını silme

Atama kaldırıldığında, Azure Planları tarafından oluşturulan kilitler kaldırılır. Ancak, kaynak geride bırakılır ve normal yollarla silinmesi gerekir.

## <a name="how-blueprint-locks-work"></a>Plan kilitleri nasıl çalışır?

Bir RBAC [reddet atamaları](../../../role-based-access-control/deny-assignments.md) reddet eylem bir plan atama sırasında yapı kaynaklarına uygulanır atama **Yalnızca Oku** veya **Silmem** seçeneği seçti. Reddet eylemi, plan atamasının yönetilen kimliği tarafından eklenir ve yalnızca aynı yönetilen kimlikle yapı kaynaklarından kaldırılabilir. Bu güvenlik önlemi kilitleme mekanizmasını zorlar ve Azure Planları dışındaki plan kilidinin kaldırılmasını önler.

:::image type="content" source="../media/resource-locking/blueprint-deny-assignment.png" alt-text="Kaynak grubunda plan reddi ataması" border="false":::

Her modun [reddet atama özellikleri](../../../role-based-access-control/deny-assignments.md#deny-assignment-properties) aşağıdaki gibidir:

|Mod |İzinler.Eylemler |İzinler.NotActions |Müdürler[i]. Türü |DışlamaMüdürleri[i]. Kimliği | DoNotApplyToChildScopes |
|-|-|-|-|-|-|
|Salt Okunur |**\*** |**\*/oku** |SystemDefined (Herkes) |plan atama ve **dışlanmış Principals** kullanıcı tanımlı |Kaynak grubu - _doğru_; Kaynak - _yanlış_ |
|Silme |**\*/silme** | |SystemDefined (Herkes) |plan atama ve **dışlanmış Principals** kullanıcı tanımlı |Kaynak grubu - _doğru_; Kaynak - _yanlış_ |

> [!IMPORTANT]
> Azure Kaynak Yöneticisi rol atama ayrıntılarını 30 dakikaya kadar önbelleğe alabilirsiniz. Sonuç olarak, reddet atamaları plan kaynakları üzerinde eylem inkar hemen tam etkili olmayabilir. Bu süre zarfında, plan kilitleri tarafından korunması amaçlanan bir kaynağı silmek mümkün olabilir.

## <a name="exclude-a-principal-from-a-deny-assignment"></a>Bir asıl atamayı reddetme atamasından hariç tutma

Bazı tasarım veya güvenlik senaryolarında, bir asıl'ı plan atamasının oluşturduğu [reddet atamasından](../../../role-based-access-control/deny-assignments.md) hariç tutmak gerekebilir. Bu adım, [atama yı oluştururken](/rest/api/blueprints/assignments/createorupdate) **kilitler** özelliğindeki **dışlanmış Principals** dizisine en fazla beş değer eklenerek REST API'da yapılır. Aşağıdaki atama tanımı **dışlanmış Principals**içeren bir istek gövdesi örneğidir:

```json
{
  "identity": {
    "type": "SystemAssigned"
  },
  "location": "eastus",
  "properties": {
    "description": "enforce pre-defined simpleBlueprint to this XXXXXXXX subscription.",
    "blueprintId": "/providers/Microsoft.Management/managementGroups/{mgId}/providers/Microsoft.Blueprint/blueprints/simpleBlueprint",
    "locks": {
        "mode": "AllResourcesDoNotDelete",
        "excludedPrincipals": [
            "7be2f100-3af5-4c15-bcb7-27ee43784a1f",
            "38833b56-194d-420b-90ce-cff578296714"
        ]
    },
    "parameters": {
      "storageAccountType": {
        "value": "Standard_LRS"
      },
      "costCenter": {
        "value": "Contoso/Online/Shopping/Production"
      },
      "owners": {
        "value": [
          "johnDoe@contoso.com",
          "johnsteam@contoso.com"
        ]
      }
    },
    "resourceGroups": {
      "storageRG": {
        "name": "defaultRG",
        "location": "eastus"
      }
    }
  }
}
```

## <a name="exclude-an-action-from-a-deny-assignment"></a>Reddet atamasından bir eylemi hariç tutma

Bir plan [atamasında bir atamayı reddetme](../../../role-based-access-control/deny-assignments.md) [atamasında bir asıl hariç tutmak](#exclude-a-principal-from-a-deny-assignment) gibi, belirli [RBAC işlemlerini](../../../role-based-access-control/resource-provider-operations.md)hariç tutabilirsiniz. **Properties.locks** bloğu içinde, **dışlanan Principals** aynı yerde, **dışlanmış Eylemler** eklenebilir:

```json
"locks": {
    "mode": "AllResourcesDoNotDelete",
    "excludedPrincipals": [
        "7be2f100-3af5-4c15-bcb7-27ee43784a1f",
        "38833b56-194d-420b-90ce-cff578296714"
    ],
    "excludedActions": [
        "Microsoft.ContainerRegistry/registries/push/write",
        "Microsoft.Authorization/*/read"
    ]
},
```

**Dışlanan Principals** açık olmalıdır, **dışlanmış Eylemler** girişleri `*` RBAC işlemlerinin joker karakter eşleştirilmesi için yararlanabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Yeni [kaynakları koru](../tutorials/protect-new-resources.md) öğreticisini izleyin.
- [Plan yaşam döngüsü](lifecycle.md)hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](sequencing-order.md) özelleştirmeyi öğrenin.
- Varolan atamaları nasıl [güncelleştirini](../how-to/update-existing-assignments.md)öğrenin.
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin.