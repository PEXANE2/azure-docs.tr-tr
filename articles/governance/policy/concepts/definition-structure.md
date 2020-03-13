---
title: İlke tanımı yapısının ayrıntıları
description: Kuruluşunuzda Azure kaynakları için kural oluşturmak üzere ilke tanımlarının nasıl kullanıldığını açıklar.
ms.date: 02/26/2020
ms.topic: conceptual
ms.openlocfilehash: 95625894d0eb603ae9a37c96c91d01f3720346b1
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79280735"
---
# <a name="azure-policy-definition-structure"></a>Azure İlkesi tanım yapısı

Azure Ilkesi, kaynaklar için kurallar oluşturur. İlke tanımları, kaynak uyumluluk [koşullarını](#conditions) ve bir koşul karşılanırsa gerçekleştirilecek etkiyi tanımlar. Bir koşul, kaynak özellik [alanını](#fields) gerekli bir değerle karşılaştırır. Kaynak özelliği alanlarına [diğer adlar](#aliases)kullanılarak erişilir. Kaynak özelliği alanı, tek değerli bir alan ya da birden çok değerden oluşan bir [dizidir](#understanding-the--alias) . Koşul değerlendirmesi diziler üzerinde farklıdır.
[Koşullar](#conditions)hakkında daha fazla bilgi edinin.

Kuralları tanımlayarak daha kolayca kaynaklarınızı yönetmek ve maliyetleri denetleyebilirsiniz. Örneğin, yalnızca belirli türlerdeki sanal makinelere izin verildiğini belirtebilirsiniz. Veya tüm kaynakların belirli bir etikete sahip gerektirebilir. İlkeleri, tüm alt kaynaklar tarafından devralınır. Bir kaynak grubu için bir ilke uygulanırsa, bu kaynak grubundaki tüm kaynaklar için geçerlidir.

İlke tanımı şeması şurada bulunur: [https://schema.management.azure.com/schemas/2019-06-01/policyDefinition.json](https://schema.management.azure.com/schemas/2019-06-01/policyDefinition.json)

Bir ilke tanımı oluşturmak için JSON kullanın. İlke tanımı yönelik öğeleri içerir:

- mode
- parametreler
- Görünen ad
- açıklama
- ilke kuralı
  - mantıksal değerlendirme
  - Etkin

Örneğin, aşağıdaki JSON kaynakların dağıtıldığı sınırlayan ilke gösterilmektedir:

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

**Mod** , ilke için hangi kaynak türlerinin değerlendirileceğini belirler. İlişkin desteklenen modları şunlardır:

- `all`: kaynak gruplarını ve tüm kaynak türlerini değerlendir
- `indexed`: yalnızca etiketleri ve konumu destekleyen kaynak türlerini değerlendir

Örneğin, kaynak `Microsoft.Network/routeTables` etiketleri ve konumu destekler ve her iki modda değerlendirilir. Ancak, kaynak `Microsoft.Network/routeTables/routes` etiketlenemez `Indexed` modunda değerlendirilmez.

Çoğu durumda **modu** `all` ayarlamanızı öneririz. Portal üzerinden oluşturulan tüm ilke tanımları `all` modunu kullanır. PowerShell veya Azure CLı kullanıyorsanız, **mod** parametresini el ile belirtebilirsiniz. İlke tanımı bir **mod** değeri içermiyorsa, Azure PowerShell ve Azure clı 'de `null` `all` varsayılan olarak olur. `null` mod, geriye dönük uyumluluğu desteklemek için `indexed` kullanmayla aynıdır.

Etiketler veya konumlar uygulayan ilkeler oluşturulurken `indexed` kullanılmalıdır. Gerekli olmasa da, etiketleri ve konumları desteklemeyen kaynakların, uyumluluk sonuçlarında uyumlu değil olarak gösterilmesini engeller. Özel durum **kaynak gruplarıdır**. Bir kaynak grubunda konum veya etiket uygulayan ilkelerin **modu** `all` olarak ayarlaması ve özel olarak `Microsoft.Resources/subscriptions/resourceGroups` türünü hedeflemesi gerekir. Bir örnek için bkz. [kaynak grubu etiketlerini zorlama](../samples/enforce-tag-rg.md). Etiketleri destekleyen kaynakların listesi için bkz. [Azure kaynakları Için etiket desteği](../../../azure-resource-manager/management/tag-support.md).

### <a name="a-nameresource-provider-modes-resource-provider-modes-preview"></a><a name="resource-provider-modes" />kaynak sağlayıcısı modları (Önizleme)

Şu anda önizleme sırasında şu kaynak sağlayıcısı modları destekleniyor:

- [Azure Kubernetes hizmetinde](../../../aks/intro-kubernetes.md)giriş denetleyicisi kurallarını yönetmek için `Microsoft.ContainerService.Data`. Bu kaynak sağlayıcısı modunu kullanan ilkelerin [Enforceregopolicy](./effects.md#enforceregopolicy) efektini kullanması **gerekir** .
- Azure 'da kendi kendine yönetilen AKS motoru Kubernetes kümelerini yönetmeye yönelik `Microsoft.Kubernetes.Data`.
  Bu kaynak sağlayıcısı modunu kullanan ilkelerde [Enforceopaconstraint](./effects.md#enforceopaconstraint) etkisi **kullanılmalıdır** .
- [Azure Key Vault](../../../key-vault/key-vault-overview.md)içindeki kasaların ve sertifikaların yönetilmesine yönelik `Microsoft.KeyVault.Data`.

> [!NOTE]
> Kaynak sağlayıcısı modları yalnızca yerleşik ilke tanımlarını destekler ve önizleme aşamasında girişimleri desteklemez.

## <a name="parameters"></a>Parametreler

Parametreleri, ilke tanımlarının sayısını azaltarak ilke yönetiminizi basitleştirmeye yardımcı olur. Parametreleri, `name`, `address`, `city``state`alanları gibi düşünün. Bu parametreleri her zaman aynı kalır, tek tek formu dolduran üzerinde tabanlı ancak bunların değerlerini değiştirin.
Parametreler, ilkeleri oluştururken aynı şekilde çalışır. İlke tanımında parametreler ekleyerek, farklı değerler kullanarak farklı senaryolar için bu ilkeyi yeniden kullanabilirsiniz.

> [!NOTE]
> Parametreler, var olan ve atanmış bir tanıma eklenebilir. Yeni parametre **DefaultValue** özelliğini içermelidir. Bu, ilke veya girişim mevcut atamaları dolaylı olarak geçersiz yapılmasını önler.

### <a name="parameter-properties"></a>Parametre özellikleri

Bir parametre, ilke tanımında kullanılan aşağıdaki özelliklere sahiptir:

- **ad**: parametresinin adı. İlke kuralı içinde `parameters` dağıtım işlevi tarafından kullanılır. Daha fazla bilgi için bkz. [parametre değeri kullanma](#using-a-parameter-value).
- `type`: parametrenin **dize**, **dizi**, **nesne**, **Boole**, **tamsayı**, **float**veya **TarihSaat**olduğunu belirler.
- `metadata`: Kullanıcı dostu bilgileri göstermek için öncelikle Azure portal tarafından kullanılan alt özellikleri tanımlar:
  - `description`: parametresinin hangi amaçla kullanıldığına ilişkin açıklama. , Kabul edilebilir değer örnekleri sağlamak için kullanılabilir.
  - `displayName`: parametre için portalda gösterilen kolay ad.
  - `version`: (Isteğe bağlı) bir ilke tanımının içeriğinin sürümü hakkındaki ayrıntıları Izler.

    > [!NOTE]
    > Azure Ilke hizmeti, yerleşik bir ilke tanımına veya girişim ve duruma yapılan değişiklik düzeyini iletmek için `version`, `preview`ve `deprecated` özelliklerini kullanır. `version` biçimi: `{Major}.{Minor}.{Patch}`. _Kullanım dışı_ veya _Önizleme_gibi belirli durumlar `version` özelliğine ya da başka bir özelliğe **Boolean**olarak eklenir.

  - `category`: (Isteğe bağlı) ilke tanımının Azure portal hangi kategori altında görüntülendiğini belirler.
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

`metadata` özelliği içinde, Azure portal içinde çok seçimli bir seçenek listesi sağlamak için **Strongtype** kullanabilirsiniz. **strongtype** desteklenen bir _kaynak türü_ veya izin verilen bir değer olabilir. Bir _kaynak türünün_ **strongtype**için geçerli olup olmadığını anlamak Için [Get-azresourceprovider](/powershell/module/az.resources/get-azresourceprovider)' ı kullanın.

**Get-AzResourceProvider** tarafından döndürülen bazı _kaynak türleri_ desteklenir. Bunlar:

- `Microsoft.RecoveryServices/vaults/backupPolicies`

**Strongtype** için izin verilen _kaynak olmayan tür_ değerleri şunlardır:

- `location`
- `resourceTypes`
- `storageSkus`
- `vmSKUs`
- `existingResourceGroups`

## <a name="definition-location"></a>Tanım konumu

Oluşturulurken bir girişim veya tanımını konumu belirtmek gereklidir. Tanım konumu, bir yönetim grubu veya abonelik olmalıdır. Bu konum için girişim veya atanabilir kapsamı belirler. Kaynaklar doğrudan üyesi veya tanım konumunu hiyerarşi içinde alt atamanın hedef olmalıdır.

Tanım konumu c: ise

- Yalnızca **abonelik** kapsamındaki kaynaklara ilke atanabilir.
- **Yönetim grubu** -yalnızca alt yönetim grupları ve alt abonelikler içindeki kaynaklara ilke atanabilir. Konum, ilke tanımı birden fazla aboneliğe uygulayın planlıyorsanız, bu Aboneliklerdeki içeren yönetim grubu olması gerekir.

## <a name="display-name-and-description"></a>Görünen ad ve açıklama

**DisplayName** ve **Description** kullanarak ilke tanımını tanımlayabilir ve ne zaman kullanılacağı için bağlam sağlayabilirsiniz. **DisplayName** , en fazla _128_ karakter uzunluğunda ve en fazla _512_ karakter uzunluğunda bir **Açıklama** içeriyor.

> [!NOTE]
> Bir ilke tanımı, **kimliği**, **türü**ve **ADıNıN** oluşturulması veya güncelleştirilmesi sırasında JSON harici özellikleri tarafından tanımlanır ve json dosyasında gerekli değildir. İlke tanımını SDK aracılığıyla getirmek, JSON 'ın bir parçası olarak **kimliği**, **türü**ve **ad** özelliklerini döndürür, ancak her biri ilke tanımıyla ilgili salt okunurdur.

## <a name="policy-rule"></a>İlke kuralı

İlke kuralı **IF** ve **then** bloklarından oluşur. **IF** bloğunda, ilkenin ne zaman uygulanacağını belirten bir veya daha fazla koşul tanımlarsınız. Senaryo ilkesi için tam olarak tanımlamak için bu koşullar için mantıksal işleçler uygulayabilirsiniz.

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

Mantıksal işleçler iç içe yerleştirebilirsiniz. Aşağıdaki örnek, **allof** işleminin içinde iç içe **geçmiş bir işlem** gösterir.

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

**Match** ve **notmatch** koşullarını kullanırken, bir sayıyla eşleşecek `#`, bir harf için `?`, herhangi bir karakterle eşleşecek `.` ve bu gerçek karakterle eşleşecek başka herhangi bir karakter sağlayın. **Eşleştirme** ve **notmatch** büyük/küçük harf duyarlı olduğu sürece, bir _StringValue_ 'yı değerlendiren diğer tüm koşullar büyük/küçük harfe duyarlıdır. Büyük/küçük harf duyarsız alternatifler **matchInsensitively** ve **notMatchInsensitively**' de mevcuttur.

Bir **\[\*\] diğer ad** dizi alanı değerinde, dizideki her öğe mantıksal **ve** öğe arasında ayrı ayrı değerlendirilir. Daha fazla bilgi için bkz. [\[\*\] diğer adı değerlendirme](../how-to/author-policies-for-arrays.md#evaluating-the--alias).

### <a name="fields"></a>Alanlar

Koşullara alanları kullanılarak oluşturulur. Bir alan, kaynak isteği yükü özelliklerinde eşleştirir ve kaynak durumunu açıklar.

Aşağıdaki alanları desteklenir:

- `name`
- `fullName`
  - Kaynağın tam adını döndürür. Bir kaynağın tam adını, tüm üst kaynak adları (örneğin "myServer/Veritabanım") tarafından başına kaynak addır.
- `kind`
- `type`
- `location`
  - Konum belirsiz olan kaynaklar için **küresel** kullanın.
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
> Bir _şablon işlevinin_ sonucu bir hata ise, ilke değerlendirmesi başarısız olur. Başarısız bir değerlendirme örtük bir **reddetme**. Daha fazla bilgi için bkz. [şablon arızalarını önleme](#avoiding-template-failures). Yeni veya güncelleştirilmiş kaynaklarda, yeni bir ilke tanımını test etme ve doğrulama sırasında başarısız bir değerlendirmesinin etkisini engellemek için **Donotenzorlamalı** 'ın [Enforcementmode](./assignment-structure.md#enforcement-mode) kullanın.

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

Yukarıdaki örnek ilke kuralı, **adın** ilk üç karakterini **ABC**olarak karşılaştırmak için [substring ()](../../../azure-resource-manager/templates/template-functions-string.md#substring) kullanır. **Ad** üç karakterden kısaysa, `substring()` işlevi bir hatayla sonuçlanır. Bu hata, ilkenin **reddetme** efekti olmasına neden olur.

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

Düzeltilen ilke kuralıyla `if()`, üç karakterden kısa bir değerde `substring()` almaya çalışmadan önce **adın** uzunluğunu denetler. **Ad** çok kısaysa, bunun yerine "ABC ile başlamıyor" değeri döndürülür ve **ABC**ile karşılaştırılır. **ABC** ile başlamayan kısa bir ada sahip bir kaynak, hala ilke kuralına neden oluyor, ancak değerlendirme sırasında hataya neden olmaz.

### <a name="count"></a>Sayı

Kaynak yükünde bir dizinin kaç üyesinin bir koşul ifadesini karşılayıp karşılamadığını sayan **sayı** ifadesi kullanılarak oluşturulabilir koşullar. Yaygın senaryolar ', ' ' veya ' hiçbiri ', ' tamamen ' veya ' hiçbiri ' ' veya ' hiçbiri ' olan dizi üyelerinin koşulu karşılayıp karşılamadığını kontrol etmekte. **Count** , her bir [\[\*\] diğer ad](#understanding-the--alias) dizisi üyesini bir koşul ifadesi için değerlendirir ve daha sonra ifade işleciyle karşılaştırılan _doğru_ sonuçları toplar.

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
- **Count. where** (isteğe bağlı): her bir\[\***count. Field** [\] diğer ad](#understanding-the--alias) dizisi üyesini ayrı ayrı değerlendirmek için koşul ifadesi. Bu özellik sağlanmazsa, ' Field ' yolunu taşıyan tüm dizi üyeleri _true_olarak değerlendirilir. Herhangi bir [koşul](../concepts/definition-structure.md#conditions) , bu özelliğin içinde kullanılabilir.
  [Mantıksal işleçler](#logical-operators) , bu özelliğin içinde karmaşık değerlendirme gereksinimleri oluşturmak için kullanılabilir.
- **\<koşul\>** (zorunlu): değer **Count. where** koşul ifadesini karşılayan öğelerin sayısıyla karşılaştırılır. Sayısal bir [koşul](../concepts/definition-structure.md#conditions) kullanılmalıdır.

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

Örnek 5: tüm dize dizisi üyelerinin koşul ifadesini karşıladığından emin olun

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

Örnek 6: tüm dizi üyelerinin koşul ifadesini karşılayıp karşılamadığını denetlemek için **değer** Içindeki **alanı** kullanın

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

Örnek 7: en az bir dizi üyesinin koşul ifadesinde birden çok özelliklerle eşleştiğinden emin olun

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

Aşağıdaki işlev bir ilke kuralında kullanılabilir, ancak bir Azure Resource Manager şablonunda kullanımı farklıdır:

- `utcNow()`-Kaynak Yöneticisi şablonundan farklı olarak, bu, defaultValue dışında kullanılabilir.
  - Universal ISO 8601 DateTime biçimindeki ' yyyy-aa-ddTHH: mm: ss. fffffffZ ' içinde geçerli tarih ve saate ayarlanmış bir dize döndürür

Aşağıdaki işlevler yalnızca ilke kurallarında kullanılabilir:

- `addDays(dateTime, numberOfDaysToAdd)`
  - **DateTime**: [Required] Universal ISO 8601 DateTime biçiminde dize dizesi ' yyyy-mm-ddTHH: mm: ss. fffffffZ '
  - **Numberofdaystoadd**: [gerekli] tamsayı-eklenecek gün sayısı
- `field(fieldName)`
  - **Alanadı**: [gerekli] dize-alınacak [alanın](#fields) adı
  - If koşulu tarafından değerlendirilen kaynaktaki bu alanın değerini döndürür
  - `field`, öncelikle, değerlendirilen kaynaktaki alanlara başvurmak için **Auditınotexists** ve **deployifnotexists** ile birlikte kullanılır. Bu kullanım örneği, [Deployifnotexists örneğinde](effects.md#deployifnotexists-example)görülebilir.
- `requestContext().apiVersion`
  - İlke değerlendirmesini tetikleyen isteğin API sürümünü döndürür (örnek: `2019-09-01`). Bu, kaynak oluşturma/güncelleştirme değerlendirmesi için PUT/PATCH isteğinde kullanılan API sürümü olacaktır. En son API sürümü, mevcut kaynaklardaki uyumluluk değerlendirmesi sırasında her zaman kullanılır.
  


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

Bir kaynak türü için belirli özelliklerine erişmek için özelliği diğer adları kullanın. Diğer adlar, kaynak üzerinde bir özellik için hangi değerleri veya koşullara izin sınırlamak sağlar. Belirtilen kaynak türü için farklı bir API sürümlerinde yolları her diğer adın eşlenir. İlke değerlendirmesi sırasında ilke altyapısı bu API sürümü için özellik yolu alır.

Diğer adlar listesini her zaman artmaktadır. Hangi diğer adlar şu anda Azure İlkesi tarafından desteklenen bulmak için aşağıdaki yöntemlerden birini kullanın:

- Visual Studio Code için Azure Ilke uzantısı (önerilir)

  Kaynak özelliklerinin diğer adlarını görüntülemek ve saptamak için [Visual Studio Code Için Azure ilke uzantısını](../how-to/extension-for-vscode.md) kullanın.

  ![Visual Studio Code için Azure Ilke uzantısı](../media/extension-for-vscode/extension-hover-shows-property-alias.png)

- Azure Kaynak Grafiği

  Bir kaynağın **diğer adını** göstermek için `project` işlecini kullanın.

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

### <a name="understanding-the--alias"></a>[*] Diğer anlama

Kullanılabilir diğer adların bazıları, ' normal ' adı olarak görünen bir sürüme ve buna ekli **\]\[\*** . Örnek:

- `Microsoft.Storage/storageAccounts/networkAcls.ipRules`
- `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]`

' Normal ' diğer ad, alanı tek bir değer olarak temsil eder. Bu alan, tüm değer kümesinin tam olarak tanımlanmış olması, daha fazla olmaması ve daha az olmaması durumunda tam eşleşme karşılaştırma senaryolarına yöneliktir.

**\[\*\]** diğer adı dizideki her öğenin değerine ve her bir öğenin belirli özelliklerine göre karşılaştırma yapmayı olanaklı kılar. Bu yaklaşım, ' if None ', ' varsa ', ' veya ' varsa ' senaryolarından oluşan öğe özelliklerini karşılaştırmayı mümkün kılar. Daha karmaşık senaryolar için, [sayı](#count) koşulu ifadesini kullanın. **Iprules\[\*\]** kullanarak, her _eylemin_ _reddetme_, ancak kaç kuralın var olduğunu veya IP _değerinin_ ne olduğunu kaygılandığını doğrulayan bir örnektir.
Bu örnek kural, **\[\*\]. değer olan ıprules** ile eşleşen tüm eşleşmeleri **denetler ve yalnızca** en az bir eşleşme bulmazsa **, bu** değeri uygular:

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

Girişim atamaları ve yönetim grubu tek bir öğe olarak çalışmak için basitleştirmek için çeşitli ilgili ilke tanımlarını olanak sağlar. Örneğin, tek bir girişim etiketleme ilgili ilke tanımlarını gruplandırabilirsiniz. Tek tek her ilke atamak yerine, girişim uygulayın.

> [!NOTE]
> Bir girişim atandıktan sonra, giriş düzeyi parametreleri değiştirilemez. Bu nedenle, parametresini tanımlarken bir **DefaultValue** ayarlaması önerilir.

Aşağıdaki örnek, iki etiket işlemek için bir girişim oluşturmayı gösterir: `costCenter` ve `productName`. Varsayılan etiket değeri uygulamak için iki yerleşik ilkeleri kullanır.

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

- [Azure ilke örneklerindeki](../samples/index.md)örnekleri gözden geçirin.
- [İlkenin etkilerini anlama](effects.md) konusunu gözden geçirin.
- [Program aracılığıyla ilkelerin nasıl oluşturulduğunu](../how-to/programmatically-create.md)anlayın.
- [Uyumluluk verilerini nasıl alabileceğinizi](../how-to/get-compliance-data.md)öğrenin.
- [Uyumlu olmayan kaynakları nasıl düzelteceğinizi](../how-to/remediate-resources.md)öğrenin.
- [Kaynakları Azure Yönetim gruplarıyla düzenleme](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu inceleyin.
