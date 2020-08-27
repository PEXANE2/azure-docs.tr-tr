---
title: İlke tanımı yapısının ayrıntıları
description: Kuruluşunuzda Azure kaynakları için kural oluşturmak üzere ilke tanımlarının nasıl kullanıldığını açıklar.
ms.date: 08/27/2020
ms.topic: conceptual
ms.openlocfilehash: 3b6509f684e611fbb79184383e1b332d793458b9
ms.sourcegitcommit: 648c8d250106a5fca9076a46581f3105c23d7265
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88958788"
---
# <a name="azure-policy-definition-structure"></a>Azure İlkesi tanım yapısı

Azure Ilkesi, kaynaklar için kurallar oluşturur. İlke tanımları, kaynak uyumluluk [koşullarını](#conditions) ve bir koşul karşılanırsa gerçekleştirilecek etkiyi tanımlar. Bir koşul, kaynak özellik [alanını](#fields) gerekli bir değerle karşılaştırır. Kaynak özelliği alanlarına [diğer adlar](#aliases)kullanılarak erişilir. Kaynak özelliği alanı, tek değerli bir alan ya da birden çok değerden oluşan bir [dizidir](#understanding-the--alias) . Koşul değerlendirmesi diziler üzerinde farklıdır.
[Koşullar](#conditions)hakkında daha fazla bilgi edinin.

Kuralları tanımlayarak, maliyetlerinizi denetleyebilir ve kaynaklarınızı daha kolay yönetebilirsiniz. Örneğin, yalnızca belirli türlerdeki sanal makinelere izin verileceğini belirtebilirsiniz. Ya da kaynakların belirli bir etiketi olmasını isteyebilirsiniz. İlke atamaları alt kaynaklar tarafından devralınır. Bir kaynak grubuna bir ilke ataması uygulanmışsa, bu kaynak grubundaki tüm kaynaklar için geçerlidir.

İlke tanımı şeması şurada bulunur: [https://schema.management.azure.com/schemas/2019-09-01/policyDefinition.json](https://schema.management.azure.com/schemas/2019-09-01/policyDefinition.json)

Bir ilke tanımı oluşturmak için JSON kullanırsınız. İlke tanımı öğeleri içerir:

- görünen ad
- açıklama
- mod
- meta veriler
- parameters
- ilke kuralı
  - mantıksal değerlendirme
  - etkinleşmesi

Örneğin, aşağıdaki JSON kaynakların dağıtıldığı yeri sınırlayan bir ilke gösterir:

```json
{
    "properties": {
        "displayName": "Allowed locations",
        "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
        "mode": "Indexed",
        "metadata": {
            "version": "1.0.0",
            "category": "Locations"
        },
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

Azure Ilkesi yerleşik bileşenleri ve desenleri [Azure ilke örnekleri](../samples/index.md)' nde bulunur.

## <a name="display-name-and-description"></a>Görünen ad ve açıklama

**DisplayName** ve **Description** kullanarak ilke tanımını tanımlayabilir ve ne zaman kullanılacağı için bağlam sağlayabilirsiniz. **DisplayName** , en fazla _128_ karakter uzunluğunda ve en fazla _512_ karakter uzunluğunda bir **Açıklama** içeriyor.

> [!NOTE]
> Bir ilke tanımı, **kimliği**, **türü**ve **ADıNıN** oluşturulması veya güncelleştirilmesi sırasında JSON harici özellikleri tarafından tanımlanır ve json dosyasında gerekli değildir. İlke tanımını SDK aracılığıyla getirmek, JSON 'ın bir parçası olarak **kimliği**, **türü**ve **ad** özelliklerini döndürür, ancak her biri ilke tanımıyla ilgili salt okunurdur.

## <a name="type"></a>Tür

**Tür** özelliği ayarlanılarken, SDK tarafından döndürülen ve portalda görünen üç değer vardır:

- `Builtin`: Bu ilke tanımları Microsoft tarafından sağlanır ve sürdürülür.
- `Custom`: Müşteriler tarafından oluşturulan tüm ilke tanımlarında bu değer vardır.
- `Static`: Microsoft **sahiplik**Ile [mevzuat uyumluluk](./regulatory-compliance.md) ilkesi tanımını gösterir. Bu ilke tanımlarının uyumluluk sonuçları, Microsoft altyapısına ilişkin üçüncü taraf denetimlerin sonuçlardır. Azure portal, bu değer bazen **Microsoft tarafından yönetilen**olarak görüntülenir. Daha fazla bilgi için bkz. [bulutta paylaşılan sorumluluk](../../../security/fundamentals/shared-responsibility.md).

## <a name="mode"></a>Mod

İlke bir Azure Resource Manager özelliğini veya kaynak sağlayıcısı özelliğini hedeflediğinden, **mod** yapılandırılır.

### <a name="resource-manager-modes"></a>Kaynak Yöneticisi modları

**Modu** , bir ilke tanımı için hangi kaynak türlerinin değerlendirildiğini belirler. Desteklenen modlar şunlardır:

- `all`: kaynak gruplarını, abonelikleri ve tüm kaynak türlerini değerlendir
- `indexed`: yalnızca etiketleri ve konumu destekleyen kaynak türlerini değerlendir

Örneğin, kaynak `Microsoft.Network/routeTables` etiketleri ve konumu destekler ve her iki modda değerlendirilir. Ancak, kaynak `Microsoft.Network/routeTables/routes` etiketlenemez ve `Indexed` modda değerlendirilmez.

**Modu** `all` çoğu durumda ayarlamanız önerilir. Portal üzerinden oluşturulan tüm ilke tanımları `all` modunu kullanır. PowerShell veya Azure CLı kullanıyorsanız, **mod** parametresini el ile belirtebilirsiniz. İlke tanımı bir **mod** değeri içermiyorsa, varsayılan olarak `all` Azure PowerShell ve ' de Azure CLI ' de olur `null` . `null`Mod, `indexed` geriye dönük uyumluluğu desteklemek için kullanmayla aynıdır.

`indexed` Etiketler veya konumlar uygulayan ilkeler oluşturulurken kullanılmalıdır. Gerekli olmasa da, etiketleri ve konumları desteklemeyen kaynakların, uyumluluk sonuçlarında uyumlu değil olarak gösterilmesini engeller. Özel durum **kaynak grupları** ve **aboneliklerdir**. Bir kaynak grubunda veya abonelikte konum veya etiket uygulayan ilke tanımlarının **modu** olarak ayarlanması `all` ve ya da türünü hedeflemesi gerekir `Microsoft.Resources/subscriptions/resourceGroups` `Microsoft.Resources/subscriptions` . Bir örnek için bkz. [model: Etiketler-örnek #1](../samples/pattern-tags.md). Etiketleri destekleyen kaynakların listesi için bkz. [Azure kaynakları Için etiket desteği](../../../azure-resource-manager/management/tag-support.md).

### <a name="resource-provider-modes-preview"></a><a name="resource-provider-modes"></a>Kaynak sağlayıcısı modları (Önizleme)

Şu anda önizleme sırasında şu kaynak sağlayıcısı modları destekleniyor:

- `Microsoft.ContainerService.Data`[Azure Kubernetes hizmetinde](../../../aks/intro-kubernetes.md)giriş denetleyicisi kurallarını yönetmek için. Bu kaynak sağlayıcısı modunu kullanan tanımların, [Enforceregopolicy](./effects.md#enforceregopolicy) efektini kullanması **gerekir** . Bu mod _kullanım dışı_bırakılıyor.
- `Microsoft.Kubernetes.Data` Kubernetes kümelerinizi Azure üzerinde veya kapalı olarak yönetmek için. Bu kaynak sağlayıcısı modunu kullanan tanımlar, etkileri _Denetim_, _reddetme_ve _devre dışı_bırakma kullanır. [Enforceopaconstraint](./effects.md#enforceopaconstraint) efektinin kullanılması _kullanım dışı_bırakılıyor.
- `Microsoft.KeyVault.Data`[Azure Key Vault](../../../key-vault/general/overview.md)' deki kasaların ve sertifikaların yönetilmesi için.

> [!NOTE]
> Kaynak sağlayıcısı modları yalnızca yerleşik ilke tanımlarını destekler ve önizleme aşamasında girişimleri desteklemez.

## <a name="metadata"></a>Meta Veriler

İsteğe bağlı `metadata` özelliği, ilke tanımıyla ilgili bilgileri depolar. Müşteriler, ' de kuruluşları için yararlı olan özellikleri ve değerleri tanımlayabilir `metadata` . Ancak, Azure Ilkesi tarafından ve yerleşik olarak kullanılan bazı _ortak_ özellikler vardır.

### <a name="common-metadata-properties"></a>Ortak meta veri özellikleri

- `version` (dize): bir ilke tanımının içeriğinin sürümü hakkındaki ayrıntıları Izler.
- `category` (dize): ilke tanımının Azure portal hangi kategori altında görüntülendiğini belirler.
- `preview` (Boolean): ilke tanımı _Önizleme_Ise, true veya false bayrağı.
- `deprecated` (Boolean): ilke tanımı _kullanım dışı_olarak işaretlenmişse true veya false bayrağı.

> [!NOTE]
> Azure Ilke hizmeti `version` , `preview` `deprecated` bir yerleşik ilke tanımına veya girişim ve duruma yapılan değişiklik düzeyini iletmek için, ve özelliklerini kullanır. Biçimi `version` : `{Major}.{Minor}.{Patch}` . _Kullanım dışı_ veya _Önizleme_gibi belirli durumlar, `version` özelliğe veya başka bir özellikte **Boole**olarak eklenir. Azure Ilke sürümlerinin yerleşik yolu hakkında daha fazla bilgi için bkz. [yerleşik sürüm oluşturma](https://github.com/Azure/azure-policy/blob/master/built-in-policies/README.md).

## <a name="parameters"></a>Parametreler

Parametreler, ilke tanımlarının sayısını azaltarak ilke yönetiminizi basitleştirmeye yardımcı olur. Parametreleri,,,, biçiminde alanlar gibi düşünün `name` `address` `city` `state` . Bu parametreler her zaman aynı kalır, ancak değerleri formu dolduran kişiye göre değişir.
Parametreler, ilke oluştururken de aynı şekilde çalışır. Bir ilke tanımına parametreler ekleyerek, farklı değerler kullanarak bu ilkeyi farklı senaryolar için yeniden kullanabilirsiniz.

> [!NOTE]
> Parametreler, var olan ve atanmış bir tanıma eklenebilir. Yeni parametre **DefaultValue** özelliğini içermelidir. Bu, ilke veya girişim için mevcut atamaların dolaylı olarak geçersiz hale getirilmelerini engeller.

### <a name="parameter-properties"></a>Parametre özellikleri

Bir parametre, ilke tanımında kullanılan aşağıdaki özelliklere sahiptir:

- `name`: Parametresinin adı. `parameters`İlke kuralı içindeki dağıtım işlevi tarafından kullanılır. Daha fazla bilgi için bkz. [parametre değeri kullanma](#using-a-parameter-value).
- `type`: Parametrenin **dize**, **dizi**, **nesne**, **Boole**, **tamsayı**, **float**veya **TarihSaat**olduğunu belirler.
- `metadata`: Kullanıcı dostu bilgileri göstermek için öncelikle Azure portal tarafından kullanılan alt özellikleri tanımlar:
  - `description`: Parametresinin hangi amaçla kullanıldığına ilişkin açıklama. , Kabul edilebilir değer örnekleri sağlamak için kullanılabilir.
  - `displayName`: Parametre için portalda gösterilen kolay ad.
  - `strongType`: (İsteğe bağlı) Portal üzerinden ilke tanımı atanırken kullanılır. Bağlama duyarlı bir liste sağlar. Daha fazla bilgi için bkz. [Strongtype](#strongtype).
  - `assignPermissions`: (İsteğe bağlı) ilke ataması sırasında rol atamaları oluşturmak Azure portal için _true_ olarak ayarlayın. Bu özellik, izinleri atama kapsamının dışına atamak istemeniz durumunda faydalıdır. İlkede rol tanımı başına bir rol ataması vardır (veya girişim içindeki tüm ilkelerin her biri için rol tanımı). Parametre değeri geçerli bir kaynak veya kapsam olmalıdır.
- `defaultValue`: (İsteğe bağlı) değer verilmezse bir atamadaki parametresinin değerini ayarlar.
  Atanan mevcut bir ilke tanımı güncelleştirilirken gereklidir.
- `allowedValues`: (İsteğe bağlı), atama sırasında parametrenin kabul ettiği bir değer dizisi sağlar.

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

İlke kuralında, parametrelere aşağıdaki `parameters` işlev sözdizimiyle başvurulamıyor:

```json
{
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

Bu örnek, [parametre özelliklerinde](#parameter-properties)gösterilen **allowedlocations** parametresine başvurur.

### <a name="strongtype"></a>strongType

Özelliği içinde `metadata` , Azure Portal içinde çoklu seçim listesi sağlamak Için **strongtype** kullanabilirsiniz. **strongtype** desteklenen bir _kaynak türü_ veya izin verilen bir değer olabilir. Bir _kaynak türünün_ **strongtype**için geçerli olup olmadığını anlamak Için [Get-azresourceprovider](/powershell/module/az.resources/get-azresourceprovider)' ı kullanın. **Strongtype** _kaynak türü_ için biçim `<Resource Provider>/<Resource Type>` . Örneğin, `Microsoft.Network/virtualNetworks/subnets`.

**Get-AzResourceProvider** tarafından döndürülen bazı _kaynak türleri_ desteklenir. Bu türler şunlardır:

- `Microsoft.RecoveryServices/vaults/backupPolicies`

**Strongtype** için izin verilen _kaynak olmayan tür_ değerleri şunlardır:

- `location`
- `resourceTypes`
- `storageSkus`
- `vmSKUs`
- `existingResourceGroups`

## <a name="definition-location"></a>Tanım konumu

Bir girişim veya ilke oluştururken, tanım konumunu belirtmeniz gerekir. Tanım konumu bir yönetim grubu veya abonelik olmalıdır. Bu konum, girişim veya ilkenin atanabileceği kapsamı belirler. Kaynaklar, atama için hedeflemek üzere tanım konumunun hiyerarşisinde doğrudan veya alt üyeleri olmalıdır.

Tanım konumu bir ise:

- Yalnızca **abonelik** kapsamındaki kaynaklara ilke atanabilir.
- **Yönetim grubu** -yalnızca alt yönetim grupları ve alt abonelikler içindeki kaynaklara ilke atanabilir. İlke tanımını birkaç aboneliğe uygulamayı planlıyorsanız, konum abonelik içeren bir yönetim grubu olmalıdır.

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
- `"less": "dateValue"` | `"less": "stringValue"` | `"less": intValue`
- `"lessOrEquals": "dateValue"` | `"lessOrEquals": "stringValue"` | `"lessOrEquals": intValue`
- `"greater": "dateValue"` | `"greater": "stringValue"` | `"greater": intValue`
- `"greaterOrEquals": "dateValue"` | `"greaterOrEquals": "stringValue"` |
  `"greaterOrEquals": intValue`
- `"exists": "bool"`

**Daha az**, **lessotalals**, **büyüktür**ve **greaterOrEquals**için, özellik türü koşul türüyle eşleşmiyorsa bir hata oluşur. Dize karşılaştırmaları kullanılarak yapılır `InvariantCultureIgnoreCase` .

**LIKE** ve **NOTLIKE** koşullarını kullanırken, değerinde bir joker karakter sağlarsınız `*` .
Değer birden fazla joker karakter içermelidir `*` .

**Match** ve **notmatch** koşullarını kullanırken, bir `#` harf için, bir `?` harf için, herhangi bir karakterle eşleşecek `.` şekilde ve diğer karakteri bu gerçek karakterle eşleşecek şekilde eşleştirin. **Match** ve **notmatch** büyük/küçük harfe duyarlı olsa da, bir _StringValue_ 'yi değerlendiren diğer tüm koşullar büyük/küçük harfe duyarlıdır. Büyük/küçük harf duyarsız alternatifler **matchInsensitively** ve **notMatchInsensitively**' de mevcuttur.

** \[ \* \] Diğer ad** dizi alanı değerinde dizideki her öğe mantıksal **ve** öğe arasında ayrı ayrı değerlendirilir. Daha fazla bilgi için bkz. [ \[ \* \] diğer adı değerlendirme](../how-to/author-policies-for-arrays.md#evaluating-the--alias).

### <a name="fields"></a>Alanlar

Koşullar, alanları kullanılarak oluşturulur. Bir alan, kaynak isteği yükünde özelliklerle eşleşir ve kaynağın durumunu açıklar.

Aşağıdaki alanlar desteklenir:

- `name`
- `fullName`
  - Kaynağın tam adını döndürür. Kaynağın tam adı herhangi bir üst kaynak adı (örneğin, "sunucum/myDatabase") tarafından eklenmiş kaynak adıdır.
- `kind`
- `type`
- `location`
  - Konum belirsiz olan kaynaklar için **küresel** kullanın.
- `identity.type`
  - Kaynak üzerinde etkin [yönetilen kimliğin](../../../active-directory/managed-identities-azure-resources/overview.md) türünü döndürür.
- `tags`
- `tags['<tagName>']`
  - Bu köşeli ayraç sözdizimi, noktalama işareti, nokta veya boşluk gibi noktalama işaretlerinin bulunduğu etiket adlarını destekler.
  - Burada, **\<tagName\>** koşulun doğrulanması için etiketin adıdır.
  - Örnekler: `tags['Acct.CostCenter']` **ACCT. costcenter** etiketin adıdır.
- `tags['''<tagName>''']`
  - Bu köşeli ayraç sözdizimi, çift tırnak işaretiyle kaçış ile kesme işareti olan etiket adlarını destekler.
  - Burada **' \<tagName\> '** , koşulun doğrulanması için etiketin adıdır.
  - Örnek: `tags['''My.Apostrophe.Tag''']` burada **' My. kesme Işareti. Tag '** etiketinin adıdır.
- Özellik diğer adları-bir liste için bkz. [diğer adlar](#aliases).

> [!NOTE]
> `tags.<tagName>`, `tags[tagName]` ve, `tags[tag.with.dots]` bir Etiketler alanı bildirmek için kabul edilebilir yollar. Ancak, tercih edilen ifadeler yukarıda listelenmiş olanlardır.

#### <a name="use-tags-with-parameters"></a>Parametrelerle etiketleri kullanma

Bir parametre değeri, bir etiket alanına geçirilebilir. Bir parametreyi bir etiket alanına geçirmek, ilke ataması sırasında ilke tanımının esnekliğini artırır.

Aşağıdaki örnekte, `concat` **TagName** parametresinin değeri adlı etiket için bir etiket alanı araması oluşturmak üzere kullanılır. Bu etiket yoksa, arama işlevini kullanarak, denetlenen kaynaklar üst kaynak grubunda ayarlanan aynı adlandırılmış etiketin değerini kullanarak etiketi eklemek için **değişiklik** efekti kullanılır `resourcegroup()` .

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

Koşullar, **değer**kullanılarak da oluşturulabilir. **değer** [parametrelere](#parameters), [desteklenen şablon işlevlerine](#policy-functions)veya değişmez değerlere karşı koşulları denetler. **değer** , desteklenen herhangi bir [koşulla](#conditions)eşleştirildi.

> [!WARNING]
> Bir _şablon işlevinin_ sonucu bir hata ise, ilke değerlendirmesi başarısız olur. Başarısız bir değerlendirme örtük bir **reddetme**. Daha fazla bilgi için bkz. [şablon arızalarını önleme](#avoiding-template-failures). Yeni veya güncelleştirilmiş kaynaklarda, yeni bir ilke tanımını test etme ve doğrulama sırasında başarısız bir değerlendirmesinin etkisini engellemek için **Donotenzorlamalı** 'ın [Enforcementmode](./assignment-structure.md#enforcement-mode) kullanın.

#### <a name="value-examples"></a>Değer örnekleri

Bu ilke kuralı örneği, **value** `resourceGroup()` işlevin sonucunu ve döndürülen **ad** özelliğini **benzer** bir koşula göre karşılaştırmak için değeri kullanır `*netrg` . Kural, `Microsoft.Network/*` adı içinde sonlanan herhangi bir kaynak grubundaki **türden** olmayan tüm kaynakları reddeder `*netrg` .

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

Bu ilke kuralı örneği, birden çok iç içe işlevlerin sonucunun **eşit** olup olmadığını denetlemek için **değerini** kullanır `true` . Kural, en az üç etiketi olmayan tüm kaynakları reddeder.

```json
{
    "mode": "indexed",
    "policyRule": {
        "if": {
            "value": "[less(length(field('tags')), 3)]",
            "equals": "true"
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

Yukarıdaki örnek ilke kuralı, **adın** ilk üç karakterini **ABC**olarak karşılaştırmak için [substring ()](../../../azure-resource-manager/templates/template-functions-string.md#substring) kullanır. **Ad** üç karakterden kısaysa, `substring()` işlev bir hatayla sonuçlanır. Bu hata, ilkenin **reddetme** efekti olmasına neden olur.

Bunun yerine **, adın ilk** üç karakterinin bir hataya neden olmak üzere üç **karakterden kısa olmasına** izin vermeden eşittir **ABC** değerine eşit olup olmadığını denetlemek için [IF ()](../../../azure-resource-manager/templates/template-functions-logical.md#if) işlevini kullanın:

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

Düzeltilen ilke kuralıyla, `if()` üç karakterden kısa bir değerde bir değer almaya çalışmadan önce **adın** uzunluğunu denetler `substring()` . **Ad** çok kısaysa, bunun yerine "ABC ile başlamıyor" değeri döndürülür ve **ABC**ile karşılaştırılır. **ABC** ile başlamayan kısa bir ada sahip bir kaynak, hala ilke kuralına neden oluyor, ancak değerlendirme sırasında hataya neden olmaz.

### <a name="count"></a>Count

Kaynak yükünde bir dizinin kaç üyesinin bir koşul ifadesini karşılayıp karşılamadığını sayan **sayı** ifadesi kullanılarak oluşturulabilir koşullar. Yaygın senaryolar ', ' ' veya ' hiçbiri ', ' tamamen ' veya ' hiçbiri ' ' veya ' hiçbiri ' olan dizi üyelerinin koşulu karşılayıp karşılamadığını kontrol etmekte. **Count** , bir koşul ifadesi için her bir [ \[ \* \] diğer ad](#understanding-the--alias) dizisi üyesini değerlendirir ve daha sonra ifade işleciyle karşılaştırılan _doğru_ sonuçları toplar. **Count** ifadeleri, tek bir **policyrule** tanımına en fazla üç kez eklenebilir.

**Count** ifadesinin yapısı:

```json
{
    "count": {
        "field": "<[*] alias>",
        "where": {
            /* condition expression */
        }
    },
    "<condition>": "<compare the count of true condition expression array members to this value>"
}
```

Şu Özellikler **sayısıyla**kullanılır:

- **Count. Field** (zorunlu): dizinin yolunu içerir ve bir dizi diğer adı olmalıdır. Dizi eksikse, ifade koşul ifadesi düşünülmeden _false_ olarak değerlendirilir.
- **Count. where** (isteğe bağlı): **Count. Field**öğesinin her bir [ \[ \* \] diğer ad](#understanding-the--alias) dizisi üyesini ayrı ayrı değerlendirmek için koşul ifadesi. Bu özellik sağlanmazsa, ' Field ' yolunu taşıyan tüm dizi üyeleri _true_olarak değerlendirilir. Herhangi bir [koşul](../concepts/definition-structure.md#conditions) , bu özelliğin içinde kullanılabilir.
  [Mantıksal işleçler](#logical-operators) , bu özelliğin içinde karmaşık değerlendirme gereksinimleri oluşturmak için kullanılabilir.
- **\<condition\>** (gerekli): değer **Count. where** koşul ifadesini karşılayan öğelerin sayısıyla karşılaştırılır. Sayısal bir [koşul](../concepts/definition-structure.md#conditions) kullanılmalıdır.

#### <a name="count-examples"></a>Sayı örnekleri

Örnek 1: bir dizinin boş olup olmadığını denetleyin

```json
{
    "count": {
        "field": "Microsoft.Network/networkSecurityGroups/securityRules[*]"
    },
    "equals": 0
}
```

Örnek 2: koşul ifadesini karşılamak için yalnızca bir dizi üyesi olup olmadığını denetleyin

```json
{
    "count": {
        "field": "Microsoft.Network/networkSecurityGroups/securityRules[*]",
        "where": {
            "field": "Microsoft.Network/networkSecurityGroups/securityRules[*].description",
            "equals": "My unique description"
        }
    },
    "equals": 1
}
```

Örnek 3: koşul ifadesini karşılamak için en az bir dizi üyesini denetleyin

```json
{
    "count": {
        "field": "Microsoft.Network/networkSecurityGroups/securityRules[*]",
        "where": {
            "field": "Microsoft.Network/networkSecurityGroups/securityRules[*].description",
            "equals": "My common description"
        }
    },
    "greaterOrEquals": 1
}
```

Örnek 4: tüm nesne dizisi üyelerinin koşul ifadesini karşıladığından emin olun

```json
{
    "count": {
        "field": "Microsoft.Network/networkSecurityGroups/securityRules[*]",
        "where": {
            "field": "Microsoft.Network/networkSecurityGroups/securityRules[*].description",
            "equals": "description"
        }
    },
    "equals": "[length(field('Microsoft.Network/networkSecurityGroups/securityRules[*]'))]"
}
```

Örnek 5: en az bir dizi üyesinin koşul ifadesinde birden çok özelliklerle eşleştiğinden emin olun

```json
{
    "count": {
        "field": "Microsoft.Network/networkSecurityGroups/securityRules[*]",
        "where": {
            "allOf": [
                {
                    "field": "Microsoft.Network/networkSecurityGroups/securityRules[*].direction",
                    "equals": "Inbound"
                },
                {
                    "field": "Microsoft.Network/networkSecurityGroups/securityRules[*].access",
                    "equals": "Allow"
                },
                {
                    "field": "Microsoft.Network/networkSecurityGroups/securityRules[*].destinationPortRange",
                    "equals": "3389"
                }
            ]
        }
    },
    "greater": 0
}
```

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

Tüm [Kaynak Yöneticisi şablonu işlevleri](../../../azure-resource-manager/templates/template-functions.md) , aşağıdaki işlevler ve Kullanıcı tanımlı işlevler dışında bir ilke kuralı içinde kullanılabilir:

- Copyındex ()
- Dağıtım ()
- Listele
- newGuid ()
- pickZones ()
- sağlayıcılar ()
- başvuru ()
- RESOURCEID ()
- değişkenler ()

> [!NOTE]
> Bu işlevler, `details.deployment.properties.template` bir **Deployifnotexists** ilke tanımındaki şablon dağıtımının bölümünde hala kullanılabilir.

Aşağıdaki işlev bir ilke kuralında kullanılabilir, ancak bir Azure Resource Manager şablonunda (ARM şablonu) kullanımı farklıdır:

- `utcNow()` -ARM şablonundan farklı olarak, bu özellik _DefaultValue_dışında kullanılabilir.
  - Universal ISO 8601 DateTime biçimindeki ' yyyy-aa-ddTHH: mm: ss. fffffffZ ' içinde geçerli tarih ve saate ayarlanmış bir dize döndürür

Aşağıdaki işlevler yalnızca ilke kurallarında kullanılabilir:

- `addDays(dateTime, numberOfDaysToAdd)`
  - **DateTime**: [Required] Universal ISO 8601 DateTime biçiminde dize dizesi ' yyyy-mm-ddTHH: mm: ss. fffffffZ '
  - **Numberofdaystoadd**: [gerekli] tamsayı-eklenecek gün sayısı
- `field(fieldName)`
  - **Alanadı**: [gerekli] dize-alınacak [alanın](#fields) adı
  - If koşulu tarafından değerlendirilen kaynaktaki bu alanın değerini döndürür
  - `field` Öncelikle, değerlendirilen kaynaktaki alanlara başvurmak için **Auditınotexists** ve **deployifnotexists** ile birlikte kullanılır. Bu kullanım örneği, [Deployifnotexists örneğinde](effects.md#deployifnotexists-example)görülebilir.
- `requestContext().apiVersion`
  - İlke değerlendirmesini tetikleyen isteğin API sürümünü döndürür (örnek: `2019-09-01` ).
    Bu değer, kaynak oluşturma/güncelleştirme değerlendirmesi için PUT/PATCH isteğinde kullanılan API sürümüdür. En son API sürümü, mevcut kaynaklardaki uyumluluk değerlendirmesi sırasında her zaman kullanılır.
  
#### <a name="policy-function-example"></a>İlke işlevi örneği

Bu ilke kuralı örneği, kaynak `resourceGroup` **name** `concat` `like` adını kaynak grubu adıyla başlatmak üzere zorlayan bir koşul oluşturmak için, ad özelliğini almak üzere Array ve Object işleviyle birlikte, kaynak işlevini kullanır.

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

- Visual Studio Code için Azure Ilke uzantısı (önerilir)

  Kaynak özelliklerinin diğer adlarını görüntülemek ve saptamak için [Visual Studio Code Için Azure ilke uzantısını](../how-to/extension-for-vscode.md) kullanın.

  :::image type="content" source="../media/extension-for-vscode/extension-hover-shows-property-alias.png" alt-text="Visual Studio Code için Azure Ilke uzantısı" border="false":::

- Azure Kaynak Grafiği

  `project`Bir kaynağın **diğer adını** göstermek için işlecini kullanın.

  ```kusto
  Resources
  | where type=~'microsoft.storage/storageaccounts'
  | limit 1
  | project aliases
  ```
  
  ```azurecli-interactive
  az graph query -q "Resources | where type=~'microsoft.storage/storageaccounts' | limit 1 | project aliases"
  ```
  
  ```azurepowershell-interactive
  Search-AzGraph -Query "Resources | where type=~'microsoft.storage/storageaccounts' | limit 1 | project aliases"
  ```

- Azure PowerShell

  ```azurepowershell-interactive
  # Login first with Connect-AzAccount if not using Cloud Shell

  # Use Get-AzPolicyAlias to list available providers
  Get-AzPolicyAlias -ListAvailable

  # Use Get-AzPolicyAlias to list aliases for a Namespace (such as Azure Compute -- Microsoft.Compute)
  (Get-AzPolicyAlias -NamespaceMatch 'compute').Aliases
  ```

  > [!NOTE]
  > [Değiştirme](./effects.md#modify) efektiyle kullanılabilecek diğer adları bulmak için aşağıdaki komutu kullanın:
  >
  > ```azurepowershell-interactive
  > Get-AzPolicyAlias | Select-Object -ExpandProperty 'Aliases' | Where-Object { $_.DefaultMetadata.Attributes -eq 'Modifiable' }
  > ```

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
  GET https://management.azure.com/providers/?api-version=2019-10-01&$expand=resourceTypes/aliases
  ```

### <a name="understanding-the--alias"></a>[*] Diğer adını anlama

Kullanılabilir diğer adların birkaçı, ' normal ' ad olarak görünen bir sürüme ve ona eklenmiş bir sürümüne sahiptir **\[\*\]** . Örneğin:

- `Microsoft.Storage/storageAccounts/networkAcls.ipRules`
- `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]`

' Normal ' diğer ad, alanı tek bir değer olarak temsil eder. Bu alan, tüm değer kümesinin tam olarak tanımlanmış olması, daha fazla olmaması ve daha az olmaması durumunda tam eşleşme karşılaştırma senaryolarına yöneliktir.

**\[\*\]** Diğer ad, dizideki her öğenin değerine ve her bir öğenin belirli özelliklerine göre karşılaştırma mümkün hale getirir. Bu yaklaşım, ' if None ', ' varsa ', ' veya ' varsa ' senaryolarından oluşan öğe özelliklerini karşılaştırmayı mümkün kılar. Daha karmaşık senaryolar için, [sayı](#count) koşulu ifadesini kullanın. ** \[ \* Iprules \] **kullanarak, her _eylemin_ _reddetme_, ancak kaç kuralın var olduğunu veya IP _değerinin_ ne olduğunu kaygılandığını doğrulayan bir örnektir.
Bu örnek kural, **ıprules \[ \* \] . Value** ile **10.0.4.1** arasında herhangi bir eşleşme olup olmadığını denetler **ve yalnızca en** az bir eşleşme bulmazsa, bu değeri uygular:

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

Daha fazla bilgi için bkz. [[ \* ] diğer adını değerlendirme](../how-to/author-policies-for-arrays.md#evaluating-the--alias).

## <a name="next-steps"></a>Sonraki adımlar

- [Girişim tanımı yapısına](./initiative-definition-structure.md) bakın
- [Azure ilke örneklerindeki](../samples/index.md)örnekleri gözden geçirin.
- [İlkenin etkilerini anlama](effects.md) konusunu gözden geçirin.
- [Program aracılığıyla ilkelerin nasıl oluşturulduğunu](../how-to/programmatically-create.md)anlayın.
- [Uyumluluk verilerini nasıl alabileceğinizi](../how-to/get-compliance-data.md)öğrenin.
- [Uyumlu olmayan kaynakları nasıl düzelteceğinizi](../how-to/remediate-resources.md)öğrenin.
- [Kaynakları Azure Yönetim gruplarıyla düzenleme](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu inceleyin.
