---
title: İlke tanım yapısının ayrıntıları
description: İlke tanımlarının kuruluşunuzdaki Azure kaynakları için sözleşmeler oluşturmak için nasıl kullanıldığını açıklar.
ms.date: 04/03/2020
ms.topic: conceptual
ms.openlocfilehash: 0a7c4e05270ff242fa97b253b27a5de92895368a
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81461013"
---
# <a name="azure-policy-definition-structure"></a>Azure İlkesi tanım yapısı

Azure İlkesi, kaynaklar için sözleşmeler kurar. İlke tanımları kaynak uyumluluk [koşullarını](#conditions) ve bir koşul yerine getirildiğinde etkisini açıklar. Bir koşul, kaynak özellik [alanını](#fields) gerekli bir değerle karşılaştırır. Kaynak özelliği alanlarına [takma adlar](#aliases)kullanılarak erişilir. Kaynak özelliği alanı, tek değerli bir alan veya birden çok değer [dizisidir.](#understanding-the--alias) Durum değerlendirmesi dizilerde farklıdır.
[Koşullar](#conditions)hakkında daha fazla bilgi edinin.

Kuralları tanımlayarak maliyetleri kontrol edebilir ve kaynaklarınızı daha kolay yönetebilirsiniz. Örneğin, yalnızca belirli türdeki sanal makinelere izin verildiğini belirtebilirsiniz. Veya, tüm kaynakların belirli bir etiketi nin olması gerektirebilir. İlkeler tüm alt kaynaklartarafından devralınır. Bir ilke bir kaynak grubuna uygulanırsa, bu kaynak grubundaki tüm kaynaklar için geçerlidir.

İlke tanımı şeması burada bulunur:[https://schema.management.azure.com/schemas/2019-06-01/policyDefinition.json](https://schema.management.azure.com/schemas/2019-06-01/policyDefinition.json)

Bir ilke tanımı oluşturmak için JSON'u kullanırsınız. İlke tanımı aşağıdakiler için öğeleri içerir:

- mod
- parametreler
- görüntü adı
- açıklama
- politika kuralı
  - mantıksal değerlendirme
  - Etkisi

Örneğin, aşağıdaki JSON, kaynakların dağıtıldığı yeri sınırlayan bir ilke gösterir:

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

Tüm Azure İlkesi örnekleri [Azure İlkesi](../samples/index.md)örneklerindedir.

## <a name="mode"></a>Mod

**Mod,** ilkenin bir Azure Kaynak Yöneticisi özelliğini veya Kaynak Sağlayıcısı özelliğini hedeflemesine bağlı olarak yapılandırılır.

### <a name="resource-manager-modes"></a>Kaynak Yöneticisi modları

**Mod,** bir ilke için hangi kaynak türlerinin değerlendirileceğini belirler. Desteklenen modlar şunlardır:

- `all`: kaynak gruplarını ve tüm kaynak türlerini değerlendirmek
- `indexed`: yalnızca etiketleri ve konumu destekleyen kaynak türlerini değerlendirin

Örneğin, kaynak `Microsoft.Network/routeTables` etiketleri ve konumu destekler ve her iki modda da değerlendirilir. Ancak, `Microsoft.Network/routeTables/routes` kaynak etiketlenemez ve modda `Indexed` değerlendirilmez.

**Modu** çoğu `all` durumda ayarlamanızı öneririz. Portal aracılığıyla oluşturulan tüm ilke `all` tanımları modu kullanır. PowerShell veya Azure CLI kullanıyorsanız, **mod** parametresini el ile belirtebilirsiniz. İlke tanımı bir **mod** değeri içermiyorsa, `all` varsayılan olarak Azure `null` PowerShell'de ve Azure CLI'de olur. Mod, `null` geriye dönük `indexed` uyumluluğu desteklemek için kullanmakla aynıdır.

`indexed`etiketleri veya konumları zorlayan ilkeler oluştururken kullanılmalıdır. Gerekli olmamakla birlikte, etiketleri ve konumları desteklemeyen kaynakların uyumluluk sonuçlarında uyumsuz olarak gösterilmesini engeller. Özel durum **kaynak gruplarıdır.** Bir kaynak grubunda konum veya etiketleri zorlayan `all` **ilkeler,** modu `Microsoft.Resources/subscriptions/resourceGroups` türü ne göre ayarlamalı ve özellikle türünü hedeflemelidir. Örneğin, kaynak [grubu etiketlerini zorla.](../samples/enforce-tag-rg.md) Etiketleri destekleyen kaynakların listesi [için Azure kaynakları için Etiket desteği'ne](../../../azure-resource-manager/management/tag-support.md)bakın.

### <a name="resource-provider-modes-preview"></a><a name="resource-provider-modes" />Kaynak Sağlayıcı modları (önizleme)

Aşağıdaki Kaynak Sağlayıcı modları şu anda önizleme sırasında desteklenir:

- `Microsoft.ContainerService.Data`[Azure Kubernetes Hizmeti'nde](../../../aks/intro-kubernetes.md)kabul denetleyici kurallarını yönetmek için. Bu Kaynak Sağlayıcı modunu kullanan ilkeler [EnforceRegoPolicy](./effects.md#enforceregopolicy) efektini **kullanmalıdır.**
- `Microsoft.Kubernetes.Data`Azure'da kendi kendini yöneten AKS Engine Kubernetes kümelerini yönetmek için.
  Bu Kaynak Sağlayıcı modunu kullanan ilkeler [EnforceOPAConstraint efektini](./effects.md#enforceopaconstraint) **kullanmalıdır.**
- `Microsoft.KeyVault.Data`[Azure Key Vault'ta](../../../key-vault/general/overview.md)kasa ve sertifika yönetimi için.

> [!NOTE]
> Kaynak Sağlayıcı modları yalnızca yerleşik ilke tanımlarını destekler ve önizleme sırasında girişimleri desteklemez.

## <a name="parameters"></a>Parametreler

Parametreler, ilke tanımlarının sayısını azaltarak ilke yönetiminizi basitleştirmeye yardımcı olur. Formdaki alanlar gibi parametreleri `name`düşünün `address` `city`– `state`, , , . Bu parametreler her zaman aynı kalır, ancak değerleri formu dolduran kişiye göre değişir.
Parametreler, ilkeler oluşturmak için aynı şekilde çalışır. İlke tanımına parametreler ekleyerek, farklı değerler kullanarak bu ilkeyi farklı senaryolar için yeniden kullanabilirsiniz.

> [!NOTE]
> Parametreler varolan ve atanmış bir tanıma eklenebilir. Yeni parametre **varsayılanDeğer** özelliğini içermelidir. Bu, ilke veya girişimin varolan atamalarının dolaylı olarak geçersiz kılınmasını önler.

### <a name="parameter-properties"></a>Parametre özellikleri

Bir parametre, ilke tanımında kullanılan aşağıdaki özelliklere sahiptir:

- **ad**: Parametrenizin adı. İlke `parameters` kuralı içinde dağıtım işlevi tarafından kullanılır. Daha fazla bilgi için [bkz.](#using-a-parameter-value)
- `type`: Parametrenin bir **dize,** **dizi,** **nesne,** **boolean,** **tamsayı,** **float**veya **datetime**olup olmadığını belirler.
- `metadata`: Kullanıcı dostu bilgileri görüntülemek için öncelikle Azure portalı tarafından kullanılan alt özellikleri tanımlar:
  - `description`: Parametrenin ne için kullanıldığının açıklaması. Kabul edilebilir değerlere örnekler vermek için kullanılabilir.
  - `displayName`: Parametre için portalda gösterilen dost adı.
  - `version`: (İsteğe bağlı) İlke tanımının içeriğinin sürümüyle ilgili ayrıntıları izler.

    > [!NOTE]
    > Azure İlkesi `version` `preview`hizmeti, `deprecated` değişiklik düzeyini yerleşik bir ilke tanımına veya girişimine ve durumuna iletmek için kullanır ve özellikleri kullanır. Biçimi: `version` `{Major}.{Minor}.{Patch}`. _Amortismana veya_ _önizleme_gibi belirli durumlar, `version` özellik veya başka bir özellik bir **boolean**olarak eklenir.

  - `category`: (İsteğe bağlı) Azure portalında ilke tanımının hangi kategori altında görüntüleneceğini belirler.
  - `strongType`: (İsteğe bağlı) Portal üzerinden ilke tanımı nı atarken kullanılır. Bağlam farkında bir liste sağlar. Daha fazla bilgi için [strongType'a](#strongtype)bakın.
  - `assignPermissions`: (İsteğe bağlı) Azure portalının ilke ataması sırasında rol atamaları oluşturması için _doğru_ olarak ayarlayın. Bu özellik, atama kapsamı dışında izinatamak istiyorsanız yararlıdır. İlkedeki rol tanımı başına bir rol ataması (veya girişimdeki tüm ilkelerdeki rol tanımı başına) vardır. Parametre değeri geçerli bir kaynak veya kapsam olmalıdır.
- `defaultValue`: (İsteğe bağlı) Değer verilmiyorsa, bir atamadaki parametrenin değerini ayarlar.
  Atanmış varolan bir ilke tanımını güncellerken gereklidir.
- `allowedValues`: (İsteğe bağlı) Parametrenin atama sırasında kabul ettiği bir dizi değer sağlar.

Örnek olarak, kaynakların dağıtılabileceği konumları sınırlamak için bir ilke tanımı tanımlayabilirsiniz. Bu ilke tanımı için bir parametre **Yetilmesine izin**verilebilir. Bu parametre, kabul edilen değerleri sınırlamak için ilke tanımının her ataması tarafından kullanılır. **strongType** kullanımı, portal üzerinden atamayı tamamlarken gelişmiş bir deneyim sağlar:

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

İlke kuralında, parametreleri aşağıdaki `parameters` işlev sözdizimi ile başvurursunuz:

```json
{
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

Bu örnek, [parametre özelliklerinde](#parameter-properties)gösterilen **izin verilen Konumlar** parametresini başvurur.

### <a name="strongtype"></a>strongType

`metadata` Özellik içinde, Azure portalıiçinde çok seçimli seçenekler listesi sağlamak için **strongType'ı** kullanabilirsiniz. **strongType** desteklenen bir _kaynak türü_ veya izin verilen bir değer olabilir. Bir kaynak _türünün_ **strongType**için geçerli olup olmadığını belirlemek için [Get-AzResourceProvider'ı](/powershell/module/az.resources/get-azresourceprovider)kullanın.

**Get-AzResourceProvider** tarafından döndürülmeyen bazı _kaynak türleri_ desteklenir. Bunlar:

- `Microsoft.RecoveryServices/vaults/backupPolicies`

**StrongType** için izin verilen _kaynak türü_ olmayan değerler şunlardır:

- `location`
- `resourceTypes`
- `storageSkus`
- `vmSKUs`
- `existingResourceGroups`

## <a name="definition-location"></a>Tanım konumu

Bir girişim veya ilke oluştururken, tanım konumunu belirtmek gerekir. Tanım konumu bir yönetim grubu veya abonelik olmalıdır. Bu konum, girişimin veya ilkenin atanabileceği kapsamı belirler. Kaynaklar, atama için hedeflemek için tanım konumunun hiyerarşisi içinde doğrudan üyeleri veya çocuklar olmalıdır.

Tanım konumu aşağıdakilerden biriyse:

- **Abonelik** - Yalnızca bu abonelikteki kaynaklara poliçe atanabilir.
- **Yönetim grubu** - İlke yalnızca alt yönetim grupları ve alt abonelikler içindeki kaynaklar atanır. İlke tanımını birkaç abonelik için uygulamayı planlıyorsanız, konumun bu abonelikleri içeren bir yönetim grubu olması gerekir.

## <a name="display-name-and-description"></a>Görüntü adı ve açıklama

İlke tanımını tanımlamak ve ne zaman kullanılacağı na yönelik bağlam sağlamak için **displayName** ve **açıklama** kullanırsınız. **displayName** en fazla _128_ karakter uzunluğundadır ve en fazla _512_ karakter uzunluğunda açıklama **tanımlar.**

> [!NOTE]
> Bir ilke tanımının oluşturulması veya güncelleştirilmesi **sırasında, id**, **türü**ve **adı** JSON dışındaki özelliklerle tanımlanır ve JSON dosyasında gerekli değildir. SDK üzerinden ilke tanımının getirilmesi, JSON'un bir parçası olarak **id,** **type**ve **name** özelliklerini döndürür, ancak her biri ilke tanımıyla ilgili salt okunur bilgilerdir.

## <a name="policy-rule"></a>İlke kuralı

İlke kuralı **If** ve **Then** bloklarından oluşur. **If** bloğunda, ilkenin ne zaman zorlanacağını belirten bir veya daha fazla koşul tanımlarsınız. Bir ilke senaryosunu tam olarak tanımlamak için bu koşullara mantıksal işleçler uygulayabilirsiniz.

**Then** bloğunda, **If** koşulları yerine getirildiğinde gerçekleşen efekti tanımlarsınız.

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

Sözdizimi **olmayan** durum sonucunu tersine çevirer. **AllOf** sözdizimi (mantıksal **Ve** işlem benzer) tüm koşulların doğru olmasını gerektirir. **AnyOf** sözdizimi (mantıksal **Veya** işlemi benzer) bir veya daha fazla koşul doğru olmasını gerektirir.

Mantıksal işleçleri yuvalayabilirsiniz. Aşağıdaki örnek, **bir allOf** işlemi içinde iç içe bir **değil** işlemi gösterir.

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

Koşul, bir **alanın** veya **değer** erişime erişimcisinin belirli ölçütleri karşılayıp karşılamadığını değerlendirir. Desteklenen koşullar şunlardır:

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
- `"greaterOrEquals": "dateValue"` | `"greaterOrEquals": "stringValue"` | `"greaterOrEquals": intValue`
- `"exists": "bool"`

Daha **az**, **daha az VeyaEşittirs**, **büyük**, ve **daha büyükOrEşitler**, özellik türü durum türü eşleşmiyorsa, bir hata atılır. Dize karşılaştırmaları `InvariantCultureIgnoreCase`kullanılarak yapılır.

**Like** ve **notLike** koşullarını kullanırken, değerde bir joker karakter `*` sağlarsınız.
Değeri birden fazla joker karakter `*`olmamalıdır.

**Match** ve **notMatch** koşullarını kullanırken, bir `?` rakamı, bir `.` harf için, herhangi bir karakteri ve o gerçek karakteri eşleşecek şekilde başka bir karakteri eşleştirmeyi sağlayın. `#` **Match** ve **notMatch** büyük/küçük harf duyarlı iken, _stringValue'i_ değerlendiren diğer tüm koşullar büyük/küçük harf duyarsızdır. Case-duyarsız alternatifler **matchInsensitively** ve **notMatchInsensitively**mevcuttur.

** \[ Diğer \* \] ad** dizisi alan değerinde, dizideki her öğe mantıksal **ve** öğeler arasında ayrı ayrı değerlendirilir. Daha fazla bilgi için [ \[ \* \] bkz.](../how-to/author-policies-for-arrays.md#evaluating-the--alias)

### <a name="fields"></a>Alanlar

Koşullar alanları kullanılarak oluşturulur. Alan, kaynak isteği yükündeki özellikleri eşler ve kaynağın durumunu açıklar.

Aşağıdaki alanlar desteklenir:

- `name`
- `fullName`
  - Kaynağın tam adını verir. Kaynağın tam adı, herhangi bir ana kaynak adı tarafından hazırlanılan kaynak adıdır (örneğin"myServer/myDatabase").
- `kind`
- `type`
- `location`
  - Konum agnostik kaynaklar için **genel** kullanın.
- `identity.type`
  - Kaynakta [etkinleştirilmiş yönetilen kimlik](../../../active-directory/managed-identities-azure-resources/overview.md) türünü döndürür.
- `tags`
- `tags['<tagName>']`
  - Bu öttürme sözdizimi, tire, nokta veya boşluk gibi noktalama işaretleri olan etiket adlarını destekler.
  - Burada ** \<tagName\> ** için koşul doğrulamak için etiketin adıdır.
  - Örnekler: `tags['Acct.CostCenter']` **Acct.CostCenter** etiketinin adı nerede.
- `tags['''<tagName>''']`
  - Bu eşanlamlı sözdizimi, çift apostrophes ile kaçarak içinde apostrophes olan etiket adlarını destekler.
  - Nerede **\<'\>tagName '** için koşul doğrulamak için etiketin adıdır.
  - Örnek: `tags['''My.Apostrophe.Tag''']` **'My.Apostrophe.Tag'** etiketinin adıdır.
- özellik takma adları - bir liste için, [Bkz. Diğer Adlar](#aliases).

> [!NOTE]
> `tags.<tagName>`, `tags[tagName]`ve `tags[tag.with.dots]` yine de bir etiket alanı bildirmenin kabul edilebilir yollarıdır. Ancak, tercih edilen ifadeler yukarıda listelenen ifadelerdir.

#### <a name="use-tags-with-parameters"></a>Parametrelerle etiketleri kullanma

Parametre değeri bir etiket alanına geçirilebilir. Bir parametrenin etiket alanına geçirilmesi, ilke ataması sırasında ilke tanımının esnekliğini artırır.

Aşağıdaki örnekte, `concat` **etiketAdı** parametresinin değeri adlı etiket için bir etiket alanı araması oluşturmak için kullanılır. Bu etiket yoksa, **değiştirme** `resourcegroup()` efekti, arama işlevini kullanarak denetlenen kaynaklar üst kaynak grubunda aynı adlandırılmış etiket kümesinin değerini kullanarak etiketi eklemek için kullanılır.

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

Koşullar **da değer**kullanılarak oluşturulabilir. **değer,** koşulları [parametrelere,](#parameters) [desteklenen şablon işlevlerine](#policy-functions)veya literals'e karşı denetler.
**değeri** desteklenen herhangi bir [durumla](#conditions)eşlenir.

> [!WARNING]
> _Şablon işlevinin_ sonucu bir hataysa, ilke değerlendirmesi başarısız olur. Başarısız bir değerlendirme örtülü bir **inkardır.** Daha fazla bilgi için [şablon hatalarından kaçınma](#avoiding-template-failures)ya da bkz. Yeni bir ilke tanımını sınarken ve doğrularken başarısız bir değerlendirmenin yeni veya güncelleştirilmiş kaynaklar üzerindeki etkisini önlemek için [enforcementMode](./assignment-structure.md#enforcement-mode) of **DoNotEnforce'yi** kullanın.

#### <a name="value-examples"></a>Değer örnekleri

Bu ilke **value** kuralı `resourceGroup()` örneği, işlevin sonucunu ve döndürülen **ad** özelliğini **benzer** bir durumla karşılaştırmak için değer `*netrg`kullanır. Kural, adı .'de `Microsoft.Network/*` `*netrg`sona eren herhangi bir kaynak grubunda **türünden** olmayan herhangi bir kaynağı reddeder.

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

Bu ilke kuralı örneği, iç içe geçen birden çok işlevin sonucunun **eşit** `true`olup olmadığını kontrol etmek için **değer** kullanır. Kural, en az üç etiketi olmayan herhangi bir kaynağı reddeder.

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

_Şablon işlevlerinin_ **değer** olarak kullanılması, birçok karmaşık iç içe işleve izin verir. _Şablon işlevinin_ sonucu bir hataysa, ilke değerlendirmesi başarısız olur. Başarısız bir değerlendirme örtülü bir **inkardır.** Belirli senaryolarda başarısız olan bir **değer** örneği:

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

Yukarıdaki örnek ilke kuralı, **ismin** ilk üç karakterini **abc**ile karşılaştırmak için [substring()](../../../azure-resource-manager/templates/template-functions-string.md#substring) kullanır. **Ad** üç karakterden kısaysa, `substring()` işlev bir hatayla sonuçlanır. Bu hata, ilkenin **reddi** etkisi haline gelmesine neden olur.

Bunun yerine, üç karakterden kısa bir **adın** hataya neden olması için ad ilk üç karakterinin **abc'ye** **eşit** olup olmadığını kontrol etmek için [if()](../../../azure-resource-manager/templates/template-functions-logical.md#if) işlevini kullanın:

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

Gözden geçirilmiş ilke `if()` kuralıyla, üçkarakterden az `substring()` bir değere sahip bir değer elde etmeye çalışmadan önce **ad** uzunluğunu denetler. **Ad** çok kısaise, "abc ile başlamayan" değeri yerine döndürülür ve **abc**ile karşılaştırıldığında. **ABC** ile başlamayan kısa bir ada sahip bir kaynak yine de ilke kuralında başarısız olur, ancak değerlendirme sırasında artık hataya neden olmaz.

### <a name="count"></a>Sayı

Kaynak yükündeki bir dizinin kaç üyesinin bir koşul ifadesini karşılayan sayısını sayan **koşullar, sayım** ifadesi kullanılarak oluşturulabilir. Yaygın senaryolar, dizi üyelerinin 'en az biri', 'tam olarak biri', 'tümü' veya 'hiçbiri' koşulu karşılayıp karşılamadığını denetler. **sayım,** bir [ \[ \* \] ](#understanding-the--alias) koşul ifadesi için her diğer ad dizisi üyesini değerlendirir ve daha sonra ifade işleciyle karşılaştırılan _gerçek_ sonuçları toplamlar.

**Sayım** ifadesinin yapısı:

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

Aşağıdaki özellikler **sayısı**ile kullanılır:

- **count.field** (gerekli): Diziye giden yolu içerir ve bir dizi diğer adı olmalıdır. Dizi eksikse, koşul ifadesi dikkate alınmaksızın ifade _false_ olarak değerlendirilir.
- **count.where** (isteğe bağlı): **Count.field'ın**her [ \[ \* \] diğer ad](#understanding-the--alias) dizisi üyesini tek tek değerlendirmek için koşul ifadesi. Bu özellik sağlanmazsa, 'alan' yolu olan tüm dizi üyeleri _doğru_olarak değerlendirilir. Herhangi bir [koşul](../concepts/definition-structure.md#conditions) bu özellik içinde kullanılabilir.
  [Mantıksal işleçler](#logical-operators) karmaşık değerlendirme gereksinimleri oluşturmak için bu özellik içinde kullanılabilir.
- koşul (gerekli): **Değer, sayım.where** koşul ifadesini karşılayan öğe sayısıyla karşılaştırılır. ** \<\> ** Sayısal bir [durum](../concepts/definition-structure.md#conditions) kullanılmalıdır.

#### <a name="count-examples"></a>Sayma örnekleri

Örnek 1: Bir dizi boş olup olmadığını kontrol edin

```json
{
    "count": {
        "field": "Microsoft.Network/networkSecurityGroups/securityRules[*]"
    },
    "equals": 0
}
```

Örnek 2: Koşul ifadesini karşılamak için yalnızca bir dizi üyesini denetleyin

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

Örnek 3: Koşul ifadesini karşılamak için en az bir dizi üyesini denetleyin

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

Örnek 4: Tüm nesne dizisi üyelerinin koşul ifadesini karşıladığını denetleme

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

Örnek 5: Tüm dize dizi üyelerinin koşul ifadesini karşıladığını denetleyin

```json
{
    "count": {
        "field": "Microsoft.Sql/servers/securityAlertPolicies/emailAddresses[*]",
        "where": {
            "field": "Microsoft.Sql/servers/securityAlertPolicies/emailAddresses[*]",
            "like": "*@contoso.com"
        }
    },
    "equals": "[length(field('Microsoft.Sql/servers/securityAlertPolicies/emailAddresses[*]'))]"
}
```

Örnek 6: Tüm dizi üyelerinin koşul ifadesini karşıladığını kontrol etmek için **iç** **değer** alanını kullanın

```json
{
    "count": {
        "field": "Microsoft.Sql/servers/securityAlertPolicies/emailAddresses[*]",
        "where": {
            "value": "[last(split(first(field('Microsoft.Sql/servers/securityAlertPolicies/emailAddresses[*]')), '@'))]",
            "equals": "contoso.com"
        }
    },
    "equals": "[length(field('Microsoft.Sql/servers/securityAlertPolicies/emailAddresses[*]'))]"
}
```

Örnek 7: En az bir dizi üyesinin koşul ifadesinde birden çok özellikle eşleşin

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

Azure İlkesi aşağıdaki etki türlerini destekler:

- **Ek**: isteğe tanımlı alan kümesi ekler
- **Denetim**: etkinlik günlüğünde bir uyarı olayı oluşturur, ancak istek başarısız olmaz
- **AuditIfNotExists**: ilgili bir kaynak yoksa etkinlik günlüğünde bir uyarı olayı oluşturur
- **Deny**: etkinlik günlüğünde bir olay oluşturur ve isteği başarısız
- **DeployIfNotExists**: zaten yoksa ilgili bir kaynak dağıtır
- **Devre dışı :** kaynakları ilke kuralına uygunluk için değerlendirmez
- **EnforceOPAConstraint** (önizleme): Azure'da kendi kendini yöneten Kubernetes kümeleri için Open Policy Agent kabul denetleyicisini Gatekeeper v3 ile yapılandırır (önizleme)
- **EnforceRegoPolicy** (önizleme): Azure Kubernetes Hizmetinde Open Policy Agent kabul denetleyicisini Gatekeeper v2 ile yapılandırır
- **Değiştirin**: tanımlanan etiketleri kaynaktan ekler, güncelleştirir veya kaldırır

Her efekt, değerlendirme sırası, özellikler ve örnekler hakkında ayrıntılı bilgi [için](effects.md)bkz.

### <a name="policy-functions"></a>İlke işlevleri

Aşağıdaki işlevler ve kullanıcı tanımlı işlevler dışında tüm [Kaynak Yöneticisi şablon işlevleri](../../../azure-resource-manager/templates/template-functions.md) bir ilke kuralı içinde kullanılabilir:

- copyIndex()
- dağıtım()
- liste*
- newGuid()
- pickZones()
- sağlayıcılar()
- referans()
- resourceId()
- değişkenler()

> [!NOTE]
> Bu işlevler, bir `details.deployment.properties.template` **deployIfNotExists** ilke tanımında şablon dağıtımının bölümü içinde hala kullanılabilir.

Aşağıdaki işlev bir ilke kuralında kullanılabilir, ancak Azure Kaynak Yöneticisi şablonundaki kullanımdan farklıdır:

- `utcNow()`- Kaynak Yöneticisi şablonundan farklı olarak, bu varsayılanDeğer dışında kullanılabilir.
  - Evrensel ISO 8601 DateTime formatında geçerli tarih ve saate ayarlanmış bir dize döndürür 'yyyy-MM-ddTHH:mm:ss.fffffffZ'

Aşağıdaki işlevler yalnızca ilke kurallarında kullanılabilir:

- `addDays(dateTime, numberOfDaysToAdd)`
  - **dateTime**: [Gerekli] dize - Evrensel ISO 8601 DateTime formatında string 'yyyy-MM-ddTHH:mm:ss.fffffffZ'
  - **numberOfDaysToAdd**: [Gerekli] tümsavar - Eklenecek gün sayısı
- `field(fieldName)`
  - **fieldName**: [Gerekli] dize - Almak için [alanın](#fields) adı
  - If koşulu tarafından değerlendirilmekte olan kaynaktan bu alanın değerini verir
  - `field`öncelikle **AuditIfNotExists** ve **DeployIfNotExists** ile değerlendirilmekte olan kaynak üzerinde başvuru alanları kullanılır. Bu kullanıma bir örnek [DeployIfNotExists örneğinde](effects.md#deployifnotexists-example)görülebilir.
- `requestContext().apiVersion`
  - İlke değerlendirmesini tetikleyen isteğin API sürümünü `2019-09-01`döndürür (örnek: ).
    Bu, kaynak oluşturma/güncelleştirme yle ilgili değerlendirmeler için PUT/PATCH isteğinde kullanılan API sürümü olacaktır. En son API sürümü her zaman varolan kaynaklara ilişkin uyumluluk değerlendirmesi sırasında kullanılır.
  
#### <a name="policy-function-example"></a>İlke işlevi örneği

Bu ilke kuralı örneği, kaynak grubu adı ile `concat` başlamak için kaynak `like` adı zorlayan bir koşul oluşturmak için dizi ve nesne işlevi ile birlikte ad **özelliği** almak için `resourceGroup` kaynak işlevini kullanır.

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

Kaynak türüiçin belirli özelliklere erişmek için özellik takma adlarını kullanırsınız. Diğer adlar, kaynaktaki bir özellik için hangi değerlerin veya koşulların izin verildiğini kısıtlamanızı sağlar. Her diğer ad, belirli bir kaynak türü için farklı API sürümlerindeki yollara eş ler. İlke değerlendirmesi sırasında, ilke altyapısı bu API sürümü için özellik yolunu alır.

Takma adların listesi her zaman büyüyor. Azure İlkesi tarafından şu anda desteklenen diğer adları bulmak için aşağıdaki yöntemlerden birini kullanın:

- Visual Studio Code için Azure İlkesi uzantısı (önerilir)

  Kaynak özellikleri için diğer adları görüntülemek ve bulmak [için Visual Studio Code için Azure İlkesi uzantısını](../how-to/extension-for-vscode.md) kullanın.

  ![Visual Studio Code için Azure İlkesi uzantısı](../media/extension-for-vscode/extension-hover-shows-property-alias.png)

- Azure Kaynak Grafiği

  Kaynağın `project` **diğer adını** görüntülemek için işleci kullanın.

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

- Azure CLI

  ```azurecli-interactive
  # Login first with az login if not using Cloud Shell

  # List namespaces
  az provider list --query [*].namespace

  # Get Azure Policy aliases for a specific Namespace (such as Azure Compute -- Microsoft.Compute)
  az provider show --namespace Microsoft.Compute --expand "resourceTypes/aliases" --query "resourceTypes[].aliases[].name"
  ```

- REST API / ARMClient

  ```http
  GET https://management.azure.com/providers/?api-version=2017-08-01&$expand=resourceTypes/aliases
  ```

### <a name="understanding-the--alias"></a>[*] takma adını anlama

Kullanılabilir diğer adlardan bazıları, 'normal' bir ad olarak görünen bir sürüme ve ona ** \[ \* ** eklenen başka bir sürüme sahiptir. Örneğin:

- `Microsoft.Storage/storageAccounts/networkAcls.ipRules`
- `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]`

'Normal' diğer ad alanı tek bir değer olarak temsil eder. Bu alan, tüm değer kümesinin tam olarak tanımlandığı, daha fazla ve daha az olmaması gerektiğinde tam eşleşme karşılaştırma senaryoları içindir.

Diğer ** \[ \* ** ad, dizideki her öğenin değeri ve her öğenin belirli özellikleriyle karşılaştırmayı mümkün kılar. Bu yaklaşım, öğe özelliklerini 'varsa', 'varsa' veya 'varsa' senaryoları için karşılaştırmayı mümkün kılar. Daha karmaşık senaryolar [için, sayım](#count) koşulu ifadesini kullanın. **ipRules\[\*** kullanarak, bir örnek her _eylem_ _Reddet_olduğunu doğrulamak olacaktır, ancak kaç kural var veya IP _değeri_ ne endişe değil.
Bu örnek kural **ipRules.value'ın\[\*\]** **10.0.4.1** eşleşmelerini denetler ve yalnızca en az bir eşleşme bulamazsa **effectType'ı** uygular:

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

Daha fazla bilgi için [[\*] diğer adını değerlendirmeye](../how-to/author-policies-for-arrays.md#evaluating-the--alias)bakın.

## <a name="initiatives"></a>Girişimler

Girişimler, bir grupla tek bir öğe olarak çalıştığınız için atamaları ve yönetimi basitleştirmek için birkaç ilgili ilke tanımını gruplandırmanızı sağlar. Örneğin, ilgili etiketleme ilkesi tanımlarını tek bir girişimde gruplayabilirsiniz. Her ilkeyi ayrı ayrı atamak yerine, girişimi uygularsınız.

> [!NOTE]
> Bir girişim atandıktan sonra, girişim düzeyi parametreleri değiştirilemez. Bu nedenle, öneri parametre tanımlarken bir **defaultValue** ayarlamaktır.

Aşağıdaki örnek, iki etiketi işlemek için nasıl `costCenter` bir `productName`girişim oluşturulacak gösteriş: ve. Varsayılan etiket değerini uygulamak için iki yerleşik ilke kullanır.

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
                },
                "defaultValue": "DefaultCostCenter"
            },
            "productNameValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for product Name tag"
                },
                "defaultValue": "DefaultProduct"
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
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İlkesi örneklerindeki](../samples/index.md)örnekleri gözden geçirin.
- [İlkenin etkilerini anlama](effects.md) konusunu gözden geçirin.
- [İlkeleri programlı bir şekilde nasıl oluşturlayacağımı](../how-to/programmatically-create.md)anlayın.
- Uyumluluk verilerini nasıl [alacağınızı](../how-to/get-compliance-data.md)öğrenin.
- [Uyumlu olmayan kaynakları](../how-to/remediate-resources.md)nasıl düzelteriz öğrenin.
- [Azure yönetim gruplarıyla kaynaklarınızı düzenleyin](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu gözden geçirin.
