---
title: İlke tanımı yapısının ayrıntıları
description: İlkenin ne zaman zorlandığını ve hangi etkiyi uygulanacağını açıklayarak, kuruluşunuzdaki kaynaklara yönelik kurallar oluşturmak için kaynak ilkesi tanımının Azure Ilkesi tarafından nasıl kullanıldığını açıklar.
author: DCtheGeek
ms.author: dacoulte
ms.date: 11/04/2019
ms.topic: conceptual
ms.service: azure-policy
ms.openlocfilehash: d415075bda4ff58d4a3a633fe820f22d8a157459
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73464036"
---
# <a name="azure-policy-definition-structure"></a>Azure İlkesi tanım yapısı

Kaynak ilkesi tanımları, Azure Ilkesi tarafından kaynak kuralları oluşturmak için kullanılır. Her tanım, kaynak uyumluluğunu ve bir kaynak uyumsuz olduğunda yapılacak etkiyi açıklar.
Kuralları tanımlayarak, maliyetlerinizi denetleyebilir ve kaynaklarınızı daha kolay yönetebilirsiniz. Örneğin, yalnızca belirli türlerdeki sanal makinelere izin verileceğini belirtebilirsiniz. Ya da, tüm kaynakların belirli bir etikete sahip olmasını zorunlu kılabilirsiniz. İlkeler tüm alt kaynaklar tarafından devralınır. Bir ilke bir kaynak grubuna uygulanmışsa, bu kaynak grubundaki tüm kaynaklar için geçerlidir.

İlke tanımı şeması şurada bulunur: [https://schema.management.azure.com/schemas/2019-06-01/policyDefinition.json](https://schema.management.azure.com/schemas/2019-06-01/policyDefinition.json)

Bir ilke tanımı oluşturmak için JSON kullanırsınız. İlke tanımı öğeleri içerir:

- Modundaysa
- parametreler
- görünen ad
- açıklama
- İlke kuralı
  - mantıksal değerlendirme
  - Etkinleşmesi

Örneğin, aşağıdaki JSON kaynakların dağıtıldığı yeri sınırlayan bir ilke gösterir:

```json
{
    "properties": {
        "mode": "all",
        "parameters": {
            "allowedLocations": {
                "type": "array",
                "metadata": {
                    "description": "The list of locations that can be specified when deploying resources",
                    "strongType": "location",
                    "displayName": "Allowed locations"
                },
                "defaultValue": [ "westus2" ]
            }
        },
        "displayName": "Allowed locations",
        "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
        "policyRule": {
            "if": {
                "not": {
                    "field": "location",
                    "in": "[parameters('allowedLocations')]"
                }
            },
            "then": {
                "effect": "deny"
            }
        }
    }
}
```

Tüm Azure Ilke örnekleri [Azure ilke örneklerimizle](../samples/index.md).

## <a name="mode"></a>Mod

İlke bir Azure Resource Manager özelliğini veya kaynak sağlayıcısı özelliğini hedeflediğinden, **mod** yapılandırılır.

### <a name="resource-manager-modes"></a>Kaynak Yöneticisi modları

**Mod** , ilke için hangi kaynak türlerinin değerlendirileceğini belirler. Desteklenen modlar şunlardır:

- `all`: kaynak gruplarını ve tüm kaynak türlerini değerlendir
- `indexed`: yalnızca etiketleri ve konumu destekleyen kaynak türlerini değerlendir

Çoğu durumda **modu** `all` ayarlamanızı öneririz. Portal üzerinden oluşturulan tüm ilke tanımları `all` modunu kullanır. PowerShell veya Azure CLı kullanıyorsanız, **mod** parametresini el ile belirtebilirsiniz. İlke tanımı bir **mod** değeri içermiyorsa, Azure PowerShell ve Azure clı 'de `null` `all` varsayılan olarak olur. `null` mod, geriye dönük uyumluluğu desteklemek için `indexed` kullanmayla aynıdır.

Etiketler veya konumlar uygulayan ilkeler oluşturulurken `indexed` kullanılmalıdır. Gerekli olmasa da, etiketleri ve konumları desteklemeyen kaynakların, uyumluluk sonuçlarında uyumlu değil olarak gösterilmesini engeller. Özel durum **kaynak gruplarıdır**. Bir kaynak grubunda konum veya etiket uygulayan ilkelerin **modu** `all` olarak ayarlaması ve özel olarak `Microsoft.Resources/subscriptions/resourceGroups` türünü hedeflemesi gerekir. Bir örnek için bkz. [kaynak grubu etiketlerini zorlama](../samples/enforce-tag-rg.md). Etiketleri destekleyen kaynakların listesi için bkz. [Azure kaynakları Için etiket desteği](../../../azure-resource-manager/tag-support.md).

### <a name="a-nameresource-provider-modes-resource-provider-modes-preview"></a><a name="resource-provider-modes" />kaynak sağlayıcısı modları (Önizleme)

Şu anda önizleme sırasında şu kaynak sağlayıcısı modları destekleniyor:

- [Azure Kubernetes hizmetinde](../../../aks/intro-kubernetes.md)giriş denetleyicisi kurallarını yönetmek için `Microsoft.ContainerService.Data`. Bu kaynak sağlayıcısı modunu kullanan ilkelerin [Enforceregopolicy](./effects.md#enforceregopolicy) efektini kullanması **gerekir** .
- Azure 'da kendi kendine yönetilen AKS motoru Kubernetes kümelerini yönetmeye yönelik `Microsoft.Kubernetes.Data`.
  Bu kaynak sağlayıcısı modunu kullanan ilkelerde [Enforceopaconstraint](./effects.md#enforceopaconstraint) etkisi **kullanılmalıdır** .
- [Azure Key Vault](../../../key-vault/key-vault-overview.md)içindeki kasaların ve sertifikaların yönetilmesine yönelik `Microsoft.KeyVault.Data`.

> [!NOTE]
> Kaynak sağlayıcısı modları yalnızca yerleşik ilke tanımlarını destekler ve önizleme aşamasında girişimleri desteklemez.

## <a name="parameters"></a>Parametreler

Parametreler, ilke tanımlarının sayısını azaltarak ilke yönetiminizi basitleştirmeye yardımcı olur. Parametreleri, `name`, `address`, `city``state`alanları gibi düşünün. Bu parametreler her zaman aynı kalır, ancak değerleri formu dolduran kişiye göre değişir.
Parametreler, ilke oluştururken de aynı şekilde çalışır. Bir ilke tanımına parametreler ekleyerek, farklı değerler kullanarak bu ilkeyi farklı senaryolar için yeniden kullanabilirsiniz.

> [!NOTE]
> Parametreler, var olan ve atanmış bir tanıma eklenebilir. Yeni parametre **DefaultValue** özelliğini içermelidir. Bu, ilke veya girişim için mevcut atamaların dolaylı olarak geçersiz hale getirilmelerini engeller.

### <a name="parameter-properties"></a>Parametre özellikleri

Bir parametre, ilke tanımında kullanılan aşağıdaki özelliklere sahiptir:

- **ad**: parametresinin adı. İlke kuralı içinde `parameters` dağıtım işlevi tarafından kullanılır. Daha fazla bilgi için bkz. [parametre değeri kullanma](#using-a-parameter-value).
- `type`: parametrenin **dize**, **dizi**, **nesne**, **Boole**, **tamsayı**, **float**veya **TarihSaat**olduğunu belirler.
- `metadata`: Kullanıcı dostu bilgileri göstermek için öncelikle Azure portal tarafından kullanılan alt özellikleri tanımlar:
  - `description`: parametresinin hangi amaçla kullanıldığına ilişkin açıklama. , Kabul edilebilir değer örnekleri sağlamak için kullanılabilir.
  - `displayName`: parametre için portalda gösterilen kolay ad.
  - `strongType`: (Isteğe bağlı) Portal aracılığıyla ilke tanımı atanırken kullanılır. Bağlama duyarlı bir liste sağlar. Daha fazla bilgi için bkz. [Strongtype](#strongtype).
  - `assignPermissions`: (Isteğe bağlı) ilke ataması sırasında Azure portal rol atamaları oluşturmak için _true_ olarak ayarlayın. Bu özellik, izinleri atama kapsamının dışına atamak istemeniz durumunda faydalıdır. İlkede rol tanımı başına bir rol ataması vardır (veya girişim içindeki tüm ilkelerin her biri için rol tanımı). Parametre değeri geçerli bir kaynak veya kapsam olmalıdır.
- `defaultValue`: (Isteğe bağlı) değer verilmezse bir atamadaki parametresinin değerini ayarlar.
  Atanan mevcut bir ilke tanımı güncelleştirilirken gereklidir.
- `allowedValues`: (Isteğe bağlı), atama sırasında parametrenin kabul ettiği bir değer dizisi sağlar.

Örnek olarak, kaynakların dağıtılabileceği konumları sınırlandırmak için bir ilke tanımı tanımlayabilirsiniz. Bu ilke tanımı için bir parametre **Allowedlocations**olabilir. Bu parametre, kabul edilen değerleri sınırlamak için her ilke tanımının ataması tarafından kullanılır. **Strongtype** kullanımı, Portal üzerinden atama tamamlanırken gelişmiş bir deneyim sağlar:

```json
"parameters": {
    "allowedLocations": {
        "type": "array",
        "metadata": {
            "description": "The list of allowed locations for resources.",
            "displayName": "Allowed locations",
            "strongType": "location"
        },
        "defaultValue": [ "westus2" ],
        "allowedValues": [
            "eastus2",
            "westus2",
            "westus"
        ]
    }
}
```

### <a name="using-a-parameter-value"></a>Parametre değeri kullanma

İlke kuralında, parametrelere aşağıdaki `parameters` işlevi sözdizimiyle başvurulamıyor:

```json
{
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

Bu örnek, [parametre özelliklerinde](#parameter-properties)gösterilen **allowedlocations** parametresine başvurur.

### <a name="strongtype"></a>strongType

`metadata` özelliği içinde, Azure portal içinde çok seçimli bir seçenek listesi sağlamak için **Strongtype** kullanabilirsiniz. **Strongtype** için izin verilen değerler şu anda şunları içerir:

- `location`
- `resourceTypes`
- `storageSkus`
- `vmSKUs`
- `existingResourceGroups`
- `omsWorkspace`
- `Microsoft.EventHub/Namespaces/EventHubs`
- `Microsoft.EventHub/Namespaces/EventHubs/AuthorizationRules`
- `Microsoft.EventHub/Namespaces/AuthorizationRules`
- `Microsoft.RecoveryServices/vaults`
- `Microsoft.RecoveryServices/vaults/backupPolicies`

## <a name="definition-location"></a>Tanım konumu

Bir girişim veya ilke oluştururken, tanım konumunu belirtmeniz gerekir. Tanım konumu bir yönetim grubu veya abonelik olmalıdır. Bu konum, girişim veya ilkenin atanabileceği kapsamı belirler. Kaynaklar, atama için hedeflemek üzere tanım konumunun hiyerarşisinde doğrudan veya alt üyeleri olmalıdır.

Tanım konumu bir ise:

- Yalnızca **abonelik** kapsamındaki kaynaklara ilke atanabilir.
- **Yönetim grubu** -yalnızca alt yönetim grupları ve alt abonelikler içindeki kaynaklara ilke atanabilir. İlke tanımını birkaç aboneliğe uygulamayı planlıyorsanız, konumun bu abonelikleri içeren bir yönetim grubu olması gerekir.

## <a name="display-name-and-description"></a>Görünen ad ve açıklama

**DisplayName** ve **Description** kullanarak ilke tanımını tanımlayabilir ve ne zaman kullanılacağı için bağlam sağlayabilirsiniz. **DisplayName** , en fazla _128_ karakter uzunluğunda ve en fazla _512_ karakter uzunluğunda bir **Açıklama** içeriyor.

## <a name="policy-rule"></a>İlke kuralı

İlke kuralı **IF** ve **then** bloklarından oluşur. **IF** bloğunda, ilkenin ne zaman uygulanacağını belirten bir veya daha fazla koşul tanımlarsınız. Bir ilke için senaryoyu tam olarak tanımlamak üzere bu koşullara mantıksal işleçler uygulayabilirsiniz.

**Sonra** bloğunda, **IF** koşulları yerine geldiğinde gerçekleşen etkiyi tanımlarsınız.

```json
{
    "if": {
        <condition> | <logical operator>
    },
    "then": {
        "effect": "deny | audit | append | auditIfNotExists | deployIfNotExists | disabled"
    }
}
```

### <a name="logical-operators"></a>Mantıksal işleçler

Desteklenen mantıksal işleçler şunlardır:

- `"not": {condition  or operator}`
- `"allOf": [{condition or operator},{condition or operator}]`
- `"anyOf": [{condition or operator},{condition or operator}]`

**Not** söz dizimi koşulun sonucunu tersine çevirir. **Allof** söz dizimi (mantıksal **ve** işleme benzer şekilde) tüm koşulların doğru olmasını gerektirir. Söz dizimi (mantıksal **veya** işleme benzer şekilde **), bir** veya daha fazla koşulun doğru olmasını gerektirir.

Mantıksal işleçleri iç içe geçirebilirsiniz. Aşağıdaki örnek, **allof** işleminin içinde iç içe **geçmiş bir işlem** gösterir.

```json
"if": {
    "allOf": [{
            "not": {
                "field": "tags",
                "containsKey": "application"
            }
        },
        {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
        }
    ]
},
```

### <a name="conditions"></a>Koşullar

Bir koşul, bir **alanın** veya **değer** erişimcisinin belirli ölçütlere uyup uymadığını değerlendirir. Desteklenen koşullar şunlardır:

- `"equals": "stringValue"`
- `"notEquals": "stringValue"`
- `"like": "stringValue"`
- `"notLike": "stringValue"`
- `"match": "stringValue"`
- `"matchInsensitively": "stringValue"`
- `"notMatch": "stringValue"`
- `"notMatchInsensitively": "stringValue"`
- `"contains": "stringValue"`
- `"notContains": "stringValue"`
- `"in": ["stringValue1","stringValue2"]`
- `"notIn": ["stringValue1","stringValue2"]`
- `"containsKey": "keyName"`
- `"notContainsKey": "keyName"`
- `"less": "value"`
- `"lessOrEquals": "value"`
- `"greater": "value"`
- `"greaterOrEquals": "value"`
- `"exists": "bool"`

**LIKE** ve **NOTLIKE** koşullarını kullanırken, değerinde bir joker karakter `*` sağlarsınız.
Değerin birden fazla joker karakter `*`olamaz.

**Match** ve **notmatch** koşullarını kullanırken, bir sayıyla eşleşecek `#`, bir harf için `?`, herhangi bir karakterle eşleşecek `.` ve bu gerçek karakterle eşleşecek başka herhangi bir karakter sağlayın.
**Match** ve **notmatch** büyük/küçük harfe duyarlıdır. Büyük/küçük harf duyarsız alternatifler **matchInsensitively** ve **notMatchInsensitively**' de mevcuttur. Örnekler için bkz. [birkaç ad desenlerine Izin verme](../samples/allow-multiple-name-patterns.md).

### <a name="fields"></a>Alanlar

Koşullar, alanları kullanılarak oluşturulur. Bir alan, kaynak isteği yükünde özelliklerle eşleşir ve kaynağın durumunu açıklar.

Aşağıdaki alanlar desteklenir:

- `name`
- `fullName`
  - Kaynağın tam adını döndürür. Kaynağın tam adı herhangi bir üst kaynak adı (örneğin, "sunucum/myDatabase") tarafından eklenmiş kaynak adıdır.
- `kind`
- `type`
- `location`
  - Konum belirsiz olan kaynaklar için **küresel** kullanın. Örnek için bkz. [örneklere Izin verilen konumlar](../samples/allowed-locations.md).
- `identity.type`
  - Kaynak üzerinde etkin [yönetilen kimliğin](../../../active-directory/managed-identities-azure-resources/overview.md) türünü döndürür.
- `tags`
- `tags['<tagName>']`
  - Bu köşeli ayraç sözdizimi, noktalama işareti, nokta veya boşluk gibi noktalama işaretlerinin bulunduğu etiket adlarını destekler.
  - **\<tagName\>** , koşulun doğrulanması için etiketin adıdır.
  - Örnekler: **ACCT. CostCenter** 'ın etiketin adı olduğu `tags['Acct.CostCenter']`.
- `tags['''<tagName>''']`
  - Bu köşeli ayraç sözdizimi, çift tırnak işaretiyle kaçış ile kesme işareti olan etiket adlarını destekler.
  - Burada **'\<tagName\>'** , koşulun doğrulanması için etiketin adıdır.
  - Örnek: **' My. kesme. Tag '** öğesinin etiketin adı olduğu `tags['''My.Apostrophe.Tag''']`.
- Özellik diğer adları-bir liste için bkz. [diğer adlar](#aliases).

> [!NOTE]
> `tags.<tagName>`, `tags[tagName]`ve `tags[tag.with.dots]`, bir Etiketler alanı bildirmek için kabul edilebilir yollarda kalır. Ancak, tercih edilen ifadeler yukarıda listelenmiş olanlardır.

#### <a name="use-tags-with-parameters"></a>Parametrelerle etiketleri kullanma

Bir parametre değeri, bir etiket alanına geçirilebilir. Bir parametreyi bir etiket alanına geçirmek, ilke ataması sırasında ilke tanımının esnekliğini artırır.

Aşağıdaki örnekte `concat` **TagName** parametresinin değeri adlı etiket için bir etiket alanı araması oluşturmak üzere kullanılır. Bu etiket yoksa, `resourcegroup()` arama işlevini kullanarak, denetlenen kaynaklar üst kaynak grubunda ayarlanan aynı adlandırılmış etiketin değerini kullanarak etiketi eklemek için **değişiklik** efekti kullanılır.

```json
{
    "if": {
        "field": "[concat('tags[', parameters('tagName'), ']')]",
        "exists": "false"
    },
    "then": {
        "effect": "modify",
        "details": {
            "operations": [{
                "operation": "add",
                "field": "[concat('tags[', parameters('tagName'), ']')]",
                "value": "[resourcegroup().tags[parameters('tagName')]]"
            }],
            "roleDefinitionIds": [
                "/providers/microsoft.authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c"
            ]
        }
    }
}
```

### <a name="value"></a>Değer

Koşullar, **değer**kullanılarak da oluşturulabilir. **değer** [parametrelere](#parameters), [desteklenen şablon işlevlerine](#policy-functions)veya değişmez değerlere karşı koşulları denetler.
**değer** , desteklenen herhangi bir [koşulla](#conditions)eşleştirildi.

> [!WARNING]
> Bir _şablon işlevinin_ sonucu bir hata ise, ilke değerlendirmesi başarısız olur. Başarısız bir değerlendirme örtük bir **reddetme**. Daha fazla bilgi için bkz. [şablon arızalarını önleme](#avoiding-template-failures).

#### <a name="value-examples"></a>Değer örnekleri

Bu ilke kuralı örneği, `resourceGroup()` işlevinin sonucunu ve döndürülen **ad** özelliğini `*netrg`**benzer** bir koşula göre karşılaştırmak için **değeri** kullanır. Kural, adı `*netrg`biten herhangi bir kaynak grubundaki `Microsoft.Network/*` **türünden** olmayan herhangi bir kaynağı reddeder.

```json
{
    "if": {
        "allOf": [{
                "value": "[resourceGroup().name]",
                "like": "*netrg"
            },
            {
                "field": "type",
                "notLike": "Microsoft.Network/*"
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```

Bu ilke kuralı örneği, birden çok iç içe işlevlerin sonucunun `true`**eşit** olup olmadığını denetlemek için **değeri** kullanır. Kural, en az üç etiketi olmayan tüm kaynakları reddeder.

```json
{
    "mode": "indexed",
    "policyRule": {
        "if": {
            "value": "[less(length(field('tags')), 3)]",
            "equals": true
        },
        "then": {
            "effect": "deny"
        }
    }
}
```

#### <a name="avoiding-template-failures"></a>Şablon hatalarından kaçınma

**Değer** 'teki _şablon işlevlerinin_ kullanımı, birçok karmaşık iç içe işleve izin verir. Bir _şablon işlevinin_ sonucu bir hata ise, ilke değerlendirmesi başarısız olur. Başarısız bir değerlendirme örtük bir **reddetme**. Belirli senaryolarda başarısız olan **değere** bir örnek:

```json
{
    "policyRule": {
        "if": {
            "value": "[substring(field('name'), 0, 3)]",
            "equals": "abc"
        },
        "then": {
            "effect": "audit"
        }
    }
}
```

Yukarıdaki örnek ilke kuralı, **adın** ilk üç karakterini **ABC**olarak karşılaştırmak için [substring ()](../../../azure-resource-manager/resource-group-template-functions-string.md#substring) kullanır. **Ad** üç karakterden kısaysa, `substring()` işlevi bir hatayla sonuçlanır. Bu hata, ilkenin **reddetme** efekti olmasına neden olur.

Bunun yerine **, adın ilk** üç karakterinin bir hataya neden olmak üzere üç **karakterden kısa olmasına** izin vermeden eşittir **ABC** değerine eşit olup olmadığını denetlemek için [IF ()](../../../azure-resource-manager/resource-group-template-functions-logical.md#if) işlevini kullanın:

```json
{
    "policyRule": {
        "if": {
            "value": "[if(greaterOrEquals(length(field('name')), 3), substring(field('name'), 0, 3), 'not starting with abc')]",
            "equals": "abc"
        },
        "then": {
            "effect": "audit"
        }
    }
}
```

Düzeltilen ilke kuralıyla `if()`, üç karakterden kısa bir değerde `substring()` almaya çalışmadan önce **adın** uzunluğunu denetler. **Ad** çok kısaysa, bunun yerine "ABC ile başlamıyor" değeri döndürülür ve **ABC**ile karşılaştırılır. **ABC** ile başlamayan kısa bir ada sahip bir kaynak, hala ilke kuralına neden oluyor, ancak değerlendirme sırasında hataya neden olmaz.

### <a name="effect"></a>Etki

Azure Ilkesi aşağıdaki efekt türlerini destekler:

- **Append**: isteğe tanımlı alan kümesini isteğe ekler
- **Denetim**: etkinlik günlüğünde bir uyarı olayı oluşturur, ancak bu istek başarısız olmaz
- **Auditınotexists**: ilgili bir kaynak yoksa, etkinlik günlüğünde bir uyarı olayı oluşturur
- **Reddet**: etkinlik günlüğünde bir olay oluşturur ve istekte başarısız olur
- **Deployifnotexists**: zaten yoksa ilgili bir kaynak dağıtır
- **Devre dışı**: kaynakları ilke kuralına uyum için değerlendirmez
- **Enforceopaconstraint** (Önizleme): Azure 'da kendi kendine yönetilen Kubernetes kümeleri için ağ geçidi denetleyicisi ile açık ilke aracısı sayede denetleyicisini yapılandırır (Önizleme)
- **Enforceregopolicy** (Önizleme): Azure Kubernetes hizmetinde Gatekeeper v2 ile açık ilke aracısı sayede denetleyiciyi yapılandırır
- **Değiştir**: bir kaynaktaki tanımlı etiketleri ekler, güncelleştirir veya kaldırır

Her etkileriyle ilgili tüm ayrıntılar, değerlendirme, özellik ve örnek sıralaması için bkz. [Azure Ilke efektlerini anlama](effects.md).

### <a name="policy-functions"></a>İlke işlevleri

Tüm [Kaynak Yöneticisi şablonu işlevleri](../../../azure-resource-manager/resource-group-template-functions.md) , aşağıdaki işlevler ve Kullanıcı tanımlı işlevler dışında bir ilke kuralı içinde kullanılabilir:

- Copyındex ()
- Dağıtım ()
- Listele
- newGuid ()
- pickZones ()
- sağlayıcılar ()
- başvuru ()
- RESOURCEID ()
- değişkenler ()

Aşağıdaki işlevler bir ilke kuralında kullanılabilir, ancak Azure Resource Manager şablonunda kullanımı farklıdır:

- addDays (TarihSaat, numberOfDaysToAdd)
  - **DateTime**: [Required] Universal ISO 8601 DateTime biçiminde dize dizesi ' yyyy-mm-ddTHH: mm: ss. fffffffZ '
  - **Numberofdaystoadd**: [gerekli] tamsayı-eklenecek gün sayısı
- utcNow ()-Kaynak Yöneticisi şablondan farklı olarak bu, defaultValue dışında kullanılabilir.
  - Universal ISO 8601 DateTime biçimindeki ' yyyy-aa-ddTHH: mm: ss. fffffffZ ' içinde geçerli tarih ve saate ayarlanmış bir dize döndürür

Ayrıca, `field` işlevi ilke kuralları tarafından kullanılabilir. `field`, öncelikle, değerlendirilen kaynaktaki alanlara başvurmak için **Auditınotexists** ve **deployifnotexists** ile birlikte kullanılır. Bu kullanım örneği, [Deployifnotexists örneğinde](effects.md#deployifnotexists-example)görülebilir.

#### <a name="policy-function-example"></a>İlke işlevi örneği

Bu ilke kuralı örneği, kaynak adının kaynak grubu adıyla başlamasını zorladığı bir `like` koşulu oluşturmak için, **ad** özelliğini almak üzere `concat` Array ve Object işleviyle birlikte `resourceGroup` Resource işlevini kullanır.

```json
{
    "if": {
        "not": {
            "field": "name",
            "like": "[concat(resourceGroup().name,'*')]"
        }
    },
    "then": {
        "effect": "deny"
    }
}
```

## <a name="aliases"></a>Diğer adlar

Bir kaynak türü için belirli özelliklere erişmek üzere özellik diğer adlarını kullanırsınız. Diğer adlar, bir kaynaktaki bir özellik için izin verilen değerleri veya koşulları kısıtlamanıza olanak sağlar. Her diğer ad, belirli bir kaynak türü için farklı API sürümlerindeki yollarla eşlenir. İlkesi değerlendirmesi sırasında, ilke altyapısı bu API sürümünün özellik yolunu alır.

Diğer adların listesi her zaman büyüyordur. Şu anda Azure Ilkesi tarafından desteklenen diğer adları bulmak için aşağıdaki yöntemlerden birini kullanın:

- Azure PowerShell

  ```azurepowershell-interactive
  # Login first with Connect-AzAccount if not using Cloud Shell

  # Use Get-AzPolicyAlias to list available providers
  Get-AzPolicyAlias -ListAvailable

  # Use Get-AzPolicyAlias to list aliases for a Namespace (such as Azure Compute -- Microsoft.Compute)
  (Get-AzPolicyAlias -NamespaceMatch 'compute').Aliases
  ```

- Azure CLI

  ```azurecli-interactive
  # Login first with az login if not using Cloud Shell

  # List namespaces
  az provider list --query [*].namespace

  # Get Azure Policy aliases for a specific Namespace (such as Azure Compute -- Microsoft.Compute)
  az provider show --namespace Microsoft.Compute --expand "resourceTypes/aliases" --query "resourceTypes[].aliases[].name"
  ```

- REST API/ARMClient

  ```http
  GET https://management.azure.com/providers/?api-version=2017-08-01&$expand=resourceTypes/aliases
  ```

### <a name="understanding-the--alias"></a>[*] Diğer adını anlama

Kullanılabilir diğer adların bazıları, ' normal ' ad olarak görünen bir sürüme ve buna ekli **[\*]** sahip. Örneğin:

- `Microsoft.Storage/storageAccounts/networkAcls.ipRules`
- `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]`

' Normal ' diğer ad, alanı tek bir değer olarak temsil eder. Bu alan, tüm değer kümesinin tam olarak tanımlanmış olması, daha fazla olmaması ve daha az olmaması durumunda tam eşleşme karşılaştırma senaryolarına yöneliktir.

**[\*]** diğer adı dizideki her öğenin değerine ve her öğenin belirli özelliklerine göre karşılaştırma yapmayı mümkün kılar. Bu yaklaşım, ' if None ', ' varsa ', ' veya ' varsa ' senaryolarından oluşan öğe özelliklerini karşılaştırmayı mümkün kılar. **Iprules [\*]** kullanarak, her _eylemin_ reddetme, ancak kaç kural var olduğunu veya IP _değerinin_ ne olduğunu _endişelenmeyi_doğrulayan bir örnektir. Bu örnek kural, **ıprules [\*].** **10.0.4.1** değerine ait tüm eşleşmeleri denetler **ve yalnızca en** az bir eşleşme bulmazsa, bu değeri uygular:

```json
"policyRule": {
    "if": {
        "allOf": [
            {
                "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
                "exists": "true"
            },
            {
                "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].value",
                "notEquals": "10.0.4.1"
            }
        ]
    },
    "then": {
        "effect": "[parameters('effectType')]"
    }
}
```

Daha fazla bilgi için bkz. [[\*] diğer adını değerlendirme](../how-to/author-policies-for-arrays.md#evaluating-the--alias).

## <a name="initiatives"></a>Girişimler

Girişimler, tek bir öğe olarak bir grupla birlikte çalışırken atamaları ve yönetimi basitleştirmek için çeşitli ilgili ilke tanımlarını gruplandırmaya olanak sağlar. Örneğin, ilgili etiketleme ilkesi tanımlarını tek bir girişimde gruplandırabilirsiniz. Her ilkeyi ayrı ayrı atamak yerine girişim uygularsınız.

Aşağıdaki örnek, iki etiket işlemek için bir girişim oluşturmayı gösterir: `costCenter` ve `productName`. Varsayılan etiket değerini uygulamak için iki yerleşik ilke kullanır.

```json
{
    "properties": {
        "displayName": "Billing Tags Policy",
        "policyType": "Custom",
        "description": "Specify cost Center tag and product name tag",
        "parameters": {
            "costCenterValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for Cost Center tag"
                }
            },
            "productNameValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for product Name tag"
                }
            }
        },
        "policyDefinitions": [{
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            }
        ]
    },
    "id": "/subscriptions/<subscription-id>/providers/Microsoft.Authorization/policySetDefinitions/billingTagsPolicy",
    "type": "Microsoft.Authorization/policySetDefinitions",
    "name": "billingTagsPolicy"
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure ilke örneklerindeki](../samples/index.md)örnekleri gözden geçirin.
- [İlkenin etkilerini anlama](effects.md) konusunu gözden geçirin.
- [Program aracılığıyla ilkelerin nasıl oluşturulduğunu](../how-to/programmatically-create.md)anlayın.
- [Uyumluluk verilerini nasıl alabileceğinizi](../how-to/getting-compliance-data.md)öğrenin.
- [Uyumlu olmayan kaynakları nasıl düzelteceğinizi](../how-to/remediate-resources.md)öğrenin.
- [Kaynakları Azure Yönetim gruplarıyla düzenleme](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu inceleyin.
