---
title: Efektlerin nasıl çalıştığını anlama
description: Azure Ilke tanımlarının uyumluluğun nasıl yönetildiğini ve raporlanmadığını belirten çeşitli etkileri vardır.
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: azure-policy
ms.openlocfilehash: 9a21242cbb16466ed4c12746ff64bd7352925fed
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2019
ms.locfileid: "72592809"
---
# <a name="understand-azure-policy-effects"></a>Azure Ilke efektlerini anlama

Azure Ilkesindeki her ilke tanımının tek bir etkisi vardır. Bu efekt, ilke kuralı eşleşecek şekilde değerlendirildiğinde ne olacağını belirler. Yeni bir kaynak, güncelleştirilmiş bir kaynak veya mevcut bir kaynak için olmaları durumunda etkiler farklı davranır.

Bu efektler Şu anda bir ilke tanımında destekleniyor:

- [Ýna](#append)
- [Denetim](#audit)
- [Auditınotexists](#auditifnotexists)
- [Reddedebilir](#deny)
- [DeployIfNotExists](#deployifnotexists)
- [Devre dışı](#disabled)
- [Enforceregopolicy](#enforceregopolicy) (Önizleme)
- [Değiştirebilirler](#modify)

## <a name="order-of-evaluation"></a>Değerlendirme sırası

Azure Resource Manager aracılığıyla kaynak oluşturma veya güncelleştirme istekleri, önce Azure Ilkesi tarafından değerlendirilir. Azure Ilkesi, kaynak için uygulanan tüm atamaların bir listesini oluşturur ve ardından kaynağı her bir tanıma karşı değerlendirir. Azure Ilkesi, isteği uygun kaynak sağlayıcısına teslim etmeden önce çeşitli etkileri işler. Bunun yapılması, bir kaynak Azure Ilkesinin tasarlanan idare denetimlerini karşılamıyorsa kaynak sağlayıcısı tarafından gereksiz işlemeyi önler.

- İlke kuralının değerlendirilmemesi gerekip gerekmediğini belirlemekte önce **devre dışı bırakıldı** .
- **Ekleme** ve **değiştirme** daha sonra değerlendirilir. Bu, isteği değiştiremediği için bir değişiklik, bir denetim veya reddetme efektinin tetiklemesini engelleyebilir.
- **Reddet** daha sonra değerlendirilir. Denetimi yapılandırmadan önce değerlendirerek, istenmeyen bir kaynağın çift günlüğü engellenir.
- Sonra **Denetim** , Istek kaynak sağlayıcısına geçmeden önce değerlendirilir.

Kaynak sağlayıcı bir başarı kodu döndürdüğünde, **Auditınotexists** ve **deployifnotexists** , ek uyumluluk günlüğü veya eyleminin gerekip gerekmediğini belirleme işlemini değerlendirir.

Şu anda **Enforceregopolicy** efekti için herhangi bir değerlendirme sırası yoktur.

## <a name="disabled"></a>Devre dışı

Bu efekt, durumları test etmek veya ilke tanımının etkiyi ne zaman parametreleştirildiği için faydalıdır. Bu esneklik, bu ilkenin atamalarının tümünü devre dışı bırakmak yerine tek bir atamayı devre dışı bırakmayı mümkün kılar.

Devre dışı bırakılmış etkine bir alternatif, ilke atamasında ayarlanan **Enforcementmode** ' dır.
**Enforcementmode** _devre dışı bırakıldığında_, kaynaklar yine de değerlendirilir. Etkinlik günlükleri ve ilke efekti gibi günlüğe kaydetme gerçekleşmez. Daha fazla bilgi için bkz. [ilke atama-zorlama modu](./assignment-structure.md#enforcement-mode).

## <a name="append"></a>Ekle

Ekleme veya güncelleştirme sırasında istenen kaynağa ek alanlar eklemek için ekleme kullanılır. Ortak bir örnek, bir depolama kaynağı için izin verilen IP 'Leri belirtmektir.

> [!IMPORTANT]
> Append etiketi olmayan özelliklerle kullanılmak üzere tasarlanmıştır. Append, oluşturma veya güncelleştirme isteği sırasında bir kaynağa etiket ekleyese de, bunun yerine Etiketler için [değiştirme](#modify) efektini kullanmanız önerilir.

### <a name="append-evaluation"></a>Değerlendirme Ekle

Append, bir kaynağın oluşturulması veya güncelleştirilmesi sırasında istek bir kaynak sağlayıcısı tarafından işlenmeden önce değerlendirilir. Append, ilke kuralının **IF** koşulu karşılandığında alanları kaynağa ekler. Ekleme efekti, özgün istekteki bir değeri farklı bir değerle geçersiz kılabilir, bu durumda bir reddetme efekti görevi görür ve isteği reddeder. Varolan bir diziye yeni bir değer eklemek için diğer adın **[\*]** sürümünü kullanın.

Ekleme efektini kullanan bir ilke tanımı, değerlendirme döngüsünün bir parçası olarak çalıştırıldığında, zaten mevcut olan kaynaklarda değişiklik yapmaz. Bunun yerine, **IF** koşulunu uyumlu değil olarak karşılayan herhangi bir kaynağı işaretler.

### <a name="append-properties"></a>Ekleme özellikleri

Append efektinin yalnızca bir **ayrıntı** dizisi vardır ve bu gereklidir. **Ayrıntılar** bir dizi olduğundan, tek bir **alan/değer** çifti ya da katları alabilir. Kabul edilebilir alanların listesi için [tanım yapısına](definition-structure.md#fields) bakın.

### <a name="append-examples"></a>Ekleme örnekleri

Örnek 1: bir depolama hesabında IP kuralları ayarlamak için bir dizi **değeri** olan **[\*]** olmayan bir [diğer ad](definition-structure.md#aliases) kullanan tek **alan/değer** çifti. **[@No__t_1]** olmayan diğer ad bir dizi olduğunda, efekt **değeri** tüm dizi olarak ekler. Dizi zaten varsa, çakışmadan bir reddetme olayı oluşur.

```json
"then": {
    "effect": "append",
    "details": [{
        "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
        "value": [{
            "action": "Allow",
            "value": "134.5.0.0/21"
        }]
    }]
}
```

Örnek 2: bir depolama hesabında IP kuralları ayarlamak için bir dizi **değeri** ile bir **[\*]** [diğer adı](definition-structure.md#aliases) kullanan tek **alan/değer** çifti. **[@No__t_1]** diğer adını kullanarak, efekt **değeri** önceden mevcut olabilecek bir diziye ekler. Dizi henüz yoksa, oluşturulur.

```json
"then": {
    "effect": "append",
    "details": [{
        "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]",
        "value": {
            "value": "40.40.40.40",
            "action": "Allow"
        }
    }]
}
```

## <a name="modify"></a>Değiştir

Değişiklik, oluşturma veya güncelleştirme sırasında bir kaynağa etiket eklemek, güncelleştirmek veya kaldırmak için kullanılır. Ortak bir örnek, costCenter gibi kaynaklardaki etiketleri güncelleştirmedir. Hedef kaynak bir kaynak grubu değilse, bir değiştirme ilkesinde `mode` ' ın her zaman _dizinli_ olarak ayarlanmış olması gerekir. Mevcut uyumlu olmayan kaynaklar bir [Düzeltme göreviyle](../how-to/remediate-resources.md)düzeltilebilir. Tek bir değiştirme kuralında herhangi bir sayıda işlem olabilir.

> [!IMPORTANT]
> Şu anda yalnızca etiketleriyle kullanım için değiştirin. Etiketleri yönetiyorsanız, Değiştir yerine Değiştir kullanılması önerilir, ek işlem türleri ve mevcut kaynakları düzeltme yeteneği sağlar. Bununla birlikte, yönetilen bir kimlik oluşturabilebilmeniz durumunda ekleme önerilir.

### <a name="modify-evaluation"></a>Değerlendirmeyi Değiştir

Değişiklik, bir kaynağın oluşturulması veya güncelleştirilmesi sırasında istek bir kaynak sağlayıcısı tarafından işlenmeden önce değerlendirilir. İlke kuralının **IF** koşulu karşılandığında, bir kaynaktaki ekleme veya güncelleştirme etiketlerini değiştirme.

Değişiklik efektini kullanan bir ilke tanımı, değerlendirme döngüsünün bir parçası olarak çalıştırıldığında, zaten mevcut olan kaynaklarda değişiklik yapmaz. Bunun yerine, **IF** koşulunu uyumlu değil olarak karşılayan herhangi bir kaynağı işaretler.

### <a name="modify-properties"></a>Özellikleri Değiştir

Değişiklik efektinin **Ayrıntılar** özelliği, düzeltme için gereken izinleri ve etiket değerlerini eklemek, güncelleştirmek veya kaldırmak için kullanılan **işlemleri** tanımlayan tüm alt özellikleri içerir.

- **Roledefinitionıds** [gerekli]
  - Bu özellik, abonelik tarafından erişilebilen rol tabanlı erişim denetimi rol KIMLIĞIYLE eşleşen bir dize dizisi içermelidir. Daha fazla bilgi için bkz. [Düzeltme-ilke tanımını yapılandırma](../how-to/remediate-resources.md#configure-policy-definition).
  - Tanımlanan rol, [katkıda bulunan](../../../role-based-access-control/built-in-roles.md#contributor) rolüne verilen tüm işlemleri içermelidir.
- **işlemler** [gerekli]
  - Eşleşen kaynaklarda tamamlanacak tüm etiket işlemlerinin dizisi.
  - Özelliklerinin
    - **işlem** [gerekli]
      - Eşleşen bir kaynakta hangi eylemin yapılacağını tanımlar. Seçenekler şunlardır: _Addorreplace_, _Add_, _Remove_. _Ekleme_ [efektine](#append) benzer şekilde davranır.
    - **alan** [gerekli]
      - Eklenecek, değiştirilecek veya kaldırılacak etiket. Etiket adları diğer [alanlar](./definition-structure.md#fields)için aynı adlandırma kuralına uymalıdır.
    - **değer** (isteğe bağlı)
      - Etiketi ayarlanacak değer.
      - **Işlem** _addorreplace_ veya _Add_ise bu özellik gereklidir.

### <a name="modify-operations"></a>İşlemleri değiştirme

**Operations** özelliği dizisi, birkaç etiketi tek bir ilke tanımından farklı şekillerde değiştirmeyi mümkün kılar. Her işlem, **işlem**, **alan**ve **değer** özelliklerinden oluşur. İşlem düzeltme görevinin etiketlere ne yaptığını belirler, alan hangi etiketin değiştirilmekte olduğunu belirler ve değer bu etiketin yeni ayarını tanımlar. Aşağıdaki örnekte aşağıdaki etiket değişiklikleri yapılır:

- Farklı bir değerle zaten mevcut olsa bile, `environment` etiketini "test" olarak ayarlar.
- @No__t_0 etiketini kaldırır.
- @No__t_0 etiketini ilke atamasında yapılandırılan _Deptname_ ilke parametresi olarak ayarlar.

```json
"details": {
    ...
    "operations": [
        {
            "operation": "addOrReplace",
            "field": "tags['environment']",
            "value": "Test"
        },
        {
            "operation": "Remove",
            "field": "tags['TempResource']",
        },
        {
            "operation": "addOrReplace",
            "field": "tags['Dept']",
            "value": "[parameters('DeptName')]"
        }
    ]
}
```

**Operation** özelliği aşağıdaki seçeneklere sahiptir:

|İşlem |Açıklama |
|-|-|
|addOrReplace |Etiket, farklı bir değerle zaten mevcut olsa bile, kaynağa tanımlı etiketi ve değeri ekler. |
|Ekle |Kaynağa tanımlı etiketi ve değeri ekler. |
|Kaldır |Tanımlanan etiketi kaynaktan kaldırır. |

### <a name="modify-examples"></a>Örnekleri Değiştir

Örnek 1: `environment` etiketini ekleyin ve var olan `environment` etiketlerini "test" ile değiştirin:

```json
"then": {
    "effect": "modify",
    "details": {
        "roleDefinitionIds": [
            "/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c"
        ],
        "operations": [
            {
                "operation": "addOrReplace",
                "field": "tags['environment']",
                "value": "Test"
            }
        ]
    }
}
```

Örnek 2: `env` etiketini kaldırın ve `environment` etiketini ekleyin ya da var olan `environment` etiketlerini parametreli bir değerle değiştirin:

```json
"then": {
    "effect": "modify",
    "details": {
        "roleDefinitionIds": [
            "/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c"
        ],
        "operations": [
            {
                "operation": "Remove",
                "field": "tags['env']"
            },
            {
                "operation": "addOrReplace",
                "field": "tags['environment']",
                "value": "[parameters('tagValue')]"
            }
        ]
    }
}
```

## <a name="deny"></a>Deny

Reddetme, bir ilke tanımı aracılığıyla tanımlı standartlarla eşleşmeyen bir kaynak isteğini engellemek için kullanılır ve istekte başarısız olur.

### <a name="deny-evaluation"></a>Değerlendirmeyi Reddet

Eşleşen bir kaynağı oluştururken veya güncelleştirirken reddetme, isteği kaynak sağlayıcısına gönderilmeden önce engeller. İstek `403 (Forbidden)` olarak döndürülür. Portalda, ilke ataması tarafından engellenmiş olan dağıtımda durum olarak yasak görüntülenir.

Mevcut kaynakların değerlendirmesi sırasında, bir reddetme ilke tanımıyla eşleşen kaynaklar uyumlu değil olarak işaretlenir.

### <a name="deny-properties"></a>Reddetme özellikleri

Reddetme etkisi, ilke tanımının **daha sonra** koşulunda kullanılmak üzere ek özellikler içermez.

### <a name="deny-example"></a>Reddetme örneği

Örnek: reddetme efektini kullanma.

```json
"then": {
    "effect": "deny"
}
```

## <a name="audit"></a>Denetim

Denetim, uyumlu olmayan bir kaynağı değerlendirirken etkinlik günlüğünde bir uyarı olayı oluşturmak için kullanılır, ancak isteği durdurmaz.

### <a name="audit-evaluation"></a>Denetim değerlendirmesi

Denetim, bir kaynağın oluşturulması veya güncelleştirilmesi sırasında Azure Ilkesi tarafından denetlenen son etkiye sahiptir. Daha sonra Azure Ilkesi kaynağı kaynak sağlayıcısına gönderir. Denetim, bir kaynak isteği ve bir değerlendirme çevrimi için aynı şekilde çalışmaktadır. Azure Ilkesi, etkinlik günlüğüne `Microsoft.Authorization/policies/audit/action` işlemi ekler ve kaynağı uyumlu değil olarak işaretler.

### <a name="audit-properties"></a>Denetim Özellikleri

Denetim efektinin, ilke tanımının **daha sonra** koşulunda kullanılmak üzere ek özellikleri yoktur.

### <a name="audit-example"></a>Denetim örneği

Örnek: denetim efektini kullanma.

```json
"then": {
    "effect": "audit"
}
```

## <a name="auditifnotexists"></a>Auditınotexists

Auditınotexists, **IF** koşuluyla eşleşen kaynaklarda denetim imkanı sunar, ancak **daha sonra** koşulun **ayrıntılarında** belirtilen bileşenleri yoktur.

### <a name="auditifnotexists-evaluation"></a>Auditınotexists değerlendirmesi

Bir kaynak sağlayıcısı bir oluşturma veya güncelleştirme kaynak isteği tamamladıktan ve başarı durum kodu döndürdüğünden Auditınotexists çalışır. İlgili kaynaklar yoksa veya **ExistenceCondition** tarafından tanımlanan kaynaklar true olarak değerlendirilmiyorsa denetim gerçekleşir. Azure Ilkesi, etkinlik günlüğüne denetim efektiyle aynı şekilde `Microsoft.Authorization/policies/audit/action` işlemi ekler. Tetiklendiğinde **, durum durumunu karşılayan** kaynak, uyumlu değil olarak işaretlenen kaynaktır.

### <a name="auditifnotexists-properties"></a>Auditınotexists özellikleri

Auditınotexists etkilerinin **Details** özelliği, eşleştirilecek ilgili kaynakları tanımlayan tüm alt özellikleri içerir.

- **Tür** [gerekli]
  - Eşleştirilecek ilgili kaynağın türünü belirtir.
  - **Ayrıntılar. Type** , **IF** koşulu kaynağı altında bir kaynak türü ise, ilke değerlendirilen kaynağın kapsamındaki bu **türden** kaynakları sorgular. Aksi takdirde, ilke, değerlendirilen kaynakla aynı kaynak grubu içinde sorgular.
- **Ad** (isteğe bağlı)
  - Eşleştirilecek kaynağın tam adını belirtir ve ilkenin belirtilen türdeki tüm kaynaklar yerine belirli bir kaynağı almasına neden olur.
  - **IF. Field. Type** ve **then. details. Type** ile ilgili koşul değerleri ne zaman eşleşiyorsa, **ad** _gerekli_ olur ve `[field('name')]` olmalıdır. Ancak, bunun yerine bir [Denetim](#audit) etkisi göz önünde bulundurulmalıdır.
- **Resourcegroupname** (isteğe bağlı)
  - İlgili kaynağın eşleştirmesinin farklı bir kaynak grubundan gelmesini sağlar.
  - **Tür** , **IF** koşulu kaynağı altında olacak bir kaynak ise uygulanmaz.
  - Varsayılan değer **IF** koşulu kaynağının kaynak grubudur.
- **Mevcut Tencescope** (isteğe bağlı)
  - İzin verilen değerler, _abonelik_ ve _ResourceGroup_.
  - İle eşleşecek ilgili kaynağı nereden getirecek yerin kapsamını ayarlar.
  - **Tür** , **IF** koşulu kaynağı altında olacak bir kaynak ise uygulanmaz.
  - _ResourceGroup_Için, **resourcegroupname**içinde belirtilen **IF** koşulu kaynağının kaynak grubu veya kaynak grubu ile sınırlı olur.
  - _Abonelik_için, ilgili kaynak için tüm aboneliği sorgular.
  - Varsayılan _kaynak grubu_.
- **ExistenceCondition** (isteğe bağlı)
  - Belirtilmemişse, **türdeki** ilgili kaynaklar etkiyi karşılar ve denetimi tetiklemez.
  - , **IF** koşulunun ilke kuralıyla aynı dili kullanır, ancak ilgili her kaynağa ayrı ayrı değerlendirilir.
  - Eşleşen ilgili herhangi bir kaynak true olarak değerlendirilirse, efekt karşılanır ve denetimi tetiklemez.
  - **IF** koşulunda değerlerle denklik denetlemek için [Field ()] kullanabilirsiniz.
  - Örneğin, üst kaynağın ( **IF** koşulunda) eşleşen ilgili kaynakla aynı kaynak konumunda olduğunu doğrulamak için kullanılabilir.

### <a name="auditifnotexists-example"></a>Auditınotexists örneği

Örnek: kötü amaçlı yazılımdan koruma uzantısının mevcut olup olmadığını belirlemesi için sanal makineleri değerlendirir ve ne zaman yoksa denetler.

```json
{
    "if": {
        "field": "type",
        "equals": "Microsoft.Compute/virtualMachines"
    },
    "then": {
        "effect": "auditIfNotExists",
        "details": {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "existenceCondition": {
                "allOf": [{
                        "field": "Microsoft.Compute/virtualMachines/extensions/publisher",
                        "equals": "Microsoft.Azure.Security"
                    },
                    {
                        "field": "Microsoft.Compute/virtualMachines/extensions/type",
                        "equals": "IaaSAntimalware"
                    }
                ]
            }
        }
    }
}
```

## <a name="deployifnotexists"></a>DeployIfNotExists

Bir DeployIfNotExists öğesine benzer şekilde, bir DeployIfNotExists ilke tanımı, koşul karşılandığında bir şablon dağıtımı yürütür.

> [!NOTE]
> [Iç içe şablonlar](../../../azure-resource-manager/resource-group-linked-templates.md#nested-template) **Deployifnotexists**ile desteklenir, ancak [bağlantılı şablonlar](../../../azure-resource-manager/resource-group-linked-templates.md) Şu anda desteklenmiyor.

### <a name="deployifnotexists-evaluation"></a>DeployIfNotExists değerlendirmesi

DeployIfNotExists, bir kaynak sağlayıcısı bir oluşturma veya güncelleştirme kaynağı isteği tamamladıktan sonra ve başarı durum kodu döndürdüğünden çalışır. İlişkili kaynaklar yoksa veya **ExistenceCondition** tarafından tanımlanan kaynaklar true olarak değerlendirilmiyorsa, şablon dağıtımı oluşur.

Bir değerlendirme çevrimi sırasında, kaynaklarla eşleşen bir DeployIfNotExists etkisi olan ilke tanımları uyumlu değil olarak işaretlenir, ancak bu kaynakta hiçbir işlem yapılmaz.

### <a name="deployifnotexists-properties"></a>DeployIfNotExists özellikleri

DeployIfNotExists efektinin **Details** özelliği, eşleştirilecek ilgili kaynakları ve yürütülecek şablon dağıtımını tanımlayan tüm alt özellikleri içerir.

- **Tür** [gerekli]
  - Eşleştirilecek ilgili kaynağın türünü belirtir.
  - , **IF** koşulu kaynağı altında bir kaynağı getirmeye çalışırken başlar ve sonra, **IF** koşulu kaynağıyla aynı kaynak grubu içinde sorgular.
- **Ad** (isteğe bağlı)
  - Eşleştirilecek kaynağın tam adını belirtir ve ilkenin belirtilen türdeki tüm kaynaklar yerine belirli bir kaynağı almasına neden olur.
  - **IF. Field. Type** ve **then. details. Type** ile ilgili koşul değerleri ne zaman eşleşiyorsa, **ad** _gerekli_ olur ve `[field('name')]` olmalıdır.
- **Resourcegroupname** (isteğe bağlı)
  - İlgili kaynağın eşleştirmesinin farklı bir kaynak grubundan gelmesini sağlar.
  - **Tür** , **IF** koşulu kaynağı altında olacak bir kaynak ise uygulanmaz.
  - Varsayılan değer **IF** koşulu kaynağının kaynak grubudur.
  - Bir şablon dağıtımı yürütülürse, bu değerin kaynak grubunda dağıtılır.
- **Mevcut Tencescope** (isteğe bağlı)
  - İzin verilen değerler, _abonelik_ ve _ResourceGroup_.
  - İle eşleşecek ilgili kaynağı nereden getirecek yerin kapsamını ayarlar.
  - **Tür** , **IF** koşulu kaynağı altında olacak bir kaynak ise uygulanmaz.
  - _ResourceGroup_Için, **resourcegroupname**içinde belirtilen **IF** koşulu kaynağının kaynak grubu veya kaynak grubu ile sınırlı olur.
  - _Abonelik_için, ilgili kaynak için tüm aboneliği sorgular.
  - Varsayılan _kaynak grubu_.
- **ExistenceCondition** (isteğe bağlı)
  - Belirtilmemişse, **türü** ilgili herhangi bir kaynak etkiyi karşılar ve dağıtımı tetiklemez.
  - , **IF** koşulunun ilke kuralıyla aynı dili kullanır, ancak ilgili her kaynağa ayrı ayrı değerlendirilir.
  - Eşleşen ilgili herhangi bir kaynak true olarak değerlendirilirse, efekt karşılanır ve dağıtımı tetiklemez.
  - **IF** koşulunda değerlerle denklik denetlemek için [Field ()] kullanabilirsiniz.
  - Örneğin, üst kaynağın ( **IF** koşulunda) eşleşen ilgili kaynakla aynı kaynak konumunda olduğunu doğrulamak için kullanılabilir.
- **Roledefinitionıds** [gerekli]
  - Bu özellik, abonelik tarafından erişilebilen rol tabanlı erişim denetimi rol KIMLIĞIYLE eşleşen bir dize dizisi içermelidir. Daha fazla bilgi için bkz. [Düzeltme-ilke tanımını yapılandırma](../how-to/remediate-resources.md#configure-policy-definition).
- **Deploymentscope** (isteğe bağlı)
  - İzin verilen değerler, _abonelik_ ve _ResourceGroup_.
  - Tetiklenecek dağıtımın türünü ayarlar. _Abonelik_ , [abonelik düzeyinde bir dağıtımı](../../../azure-resource-manager/deploy-to-subscription.md)gösterir, _ResourceGroup_ bir kaynak grubuna yönelik bir dağıtımı gösterir.
  - Abonelik düzeyinde dağıtımlar kullanılırken _dağıtımda_ bir _konum_ özelliği belirtilmelidir.
  - Varsayılan _kaynak grubu_.
- **Dağıtım** [gerekli]
  - Bu özellik, `Microsoft.Resources/deployments` PUT API 'sine geçirilecek şekilde tam şablon dağıtımını içermelidir. Daha fazla bilgi için bkz. [dağıtımlar REST API](/rest/api/resources/deployments).

  > [!NOTE]
  > **Dağıtım** özelliğinin içindeki tüm işlevler, ilke değil, şablonun bileşenleri olarak değerlendirilir. Özel durum, ilkeden şablona değer geçiren **Parameters** özelliğidir. Bu bölümdeki **değer** bir şablon parametresi adı altında bu değer geçişini gerçekleştirmek için kullanılır (Deployıfnotexists örneğinde bkz. _fulldbname_ ).

### <a name="deployifnotexists-example"></a>DeployIfNotExists örneği

Örnek: transparentDataEncryption etkinleştirilip etkinleştirilmediğini belirlemede SQL Server veritabanlarını değerlendirir. Aksi takdirde, etkinleştirilecek bir dağıtım yürütülür.

```json
"if": {
    "field": "type",
    "equals": "Microsoft.Sql/servers/databases"
},
"then": {
    "effect": "DeployIfNotExists",
    "details": {
        "type": "Microsoft.Sql/servers/databases/transparentDataEncryption",
        "name": "current",
        "roleDefinitionIds": [
            "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/{roleGUID}",
            "/providers/Microsoft.Authorization/roleDefinitions/{builtinroleGUID}"
        ],
        "existenceCondition": {
            "field": "Microsoft.Sql/transparentDataEncryption.status",
            "equals": "Enabled"
        },
        "deployment": {
            "properties": {
                "mode": "incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {
                        "fullDbName": {
                            "type": "string"
                        }
                    },
                    "resources": [{
                        "name": "[concat(parameters('fullDbName'), '/current')]",
                        "type": "Microsoft.Sql/servers/databases/transparentDataEncryption",
                        "apiVersion": "2014-04-01",
                        "properties": {
                            "status": "Enabled"
                        }
                    }]
                },
                "parameters": {
                    "fullDbName": {
                        "value": "[field('fullName')]"
                    }
                }
            }
        }
    }
}
```

## <a name="enforceregopolicy"></a>EnforceRegoPolicy

Bu efekt `Microsoft.ContainerService.Data` ' in bir ilke tanımı *moduyla* kullanılır. [Azure Kubernetes hizmetinde](../../../aks/intro-kubernetes.md) [ilke aracısını](https://www.openpolicyagent.org/) (Opa) açmak üzere [rego](https://www.openpolicyagent.org/docs/how-do-i-write-policies.html#what-is-rego) ile tanımlanan giriş denetimi kurallarını geçirmek için kullanılır.

> [!NOTE]
> [Kubernetes Için Azure Ilkesi](rego-for-aks.md) genel önizlemede ve yalnızca yerleşik ilke tanımlarını destekler.

### <a name="enforceregopolicy-evaluation"></a>EnforceRegoPolicy değerlendirmesi

Açık Ilke Aracısı giriş denetleyicisi, kümede bulunan yeni istekleri gerçek zamanlı olarak değerlendirir.
Her 5 dakikada bir küme için tam tarama tamamlanır ve sonuçlar Azure Ilkesine bildirilir.

### <a name="enforceregopolicy-properties"></a>EnforceRegoPolicy özellikleri

EnforceRegoPolicy efektinin **Details** özelliği, Rego giriş denetimi kuralını tanımlayan alt özellikler içerir.

- **PolicyId** [gerekli]
  - Rego giriş denetimi kuralına parametre olarak geçirilen benzersiz bir ad.
- **ilke** [gerekli]
  - Rego giriş denetimi kuralının URI 'sini belirtir.
- **policyParameters** [isteğe bağlı]
  - Rego ilkesine geçirilecek parametreleri ve değerleri tanımlar.

### <a name="enforceregopolicy-example"></a>EnforceRegoPolicy örneği

Örnek: rego giriş denetimi kuralı, AKS içinde yalnızca belirtilen kapsayıcı görüntülerine izin vermek için.

```json
"if": {
    "allOf": [
        {
            "field": "type",
            "equals": "Microsoft.ContainerService/managedClusters"
        },
        {
            "field": "location",
            "equals": "westus2"
        }
    ]
},
"then": {
    "effect": "EnforceRegoPolicy",
    "details": {
        "policyId": "ContainerAllowedImages",
        "policy": "https://raw.githubusercontent.com/Azure/azure-policy/master/built-in-references/KubernetesService/container-allowed-images/limited-preview/gatekeeperpolicy.rego",
        "policyParameters": {
            "allowedContainerImagesRegex": "[parameters('allowedContainerImagesRegex')]"
        }
    }
}
```

## <a name="layering-policies"></a>İlkeleri katmanlama

Bir kaynak, birkaç atamadan etkilenebilir. Bu atamalar aynı kapsamda veya farklı kapsamlardadır olabilir. Bu atamaların her biri de tanımlı farklı bir etkiye sahip olabilir. Her ilke için koşul ve efekt bağımsız olarak değerlendirilir. Örnek:

- İlke 1
  - Kaynak konumunu ' westus ' olarak kısıtlar
  - A aboneliğine atandı
  - Reddetme etkisi
- İlke 2
  - Kaynak konumunu ' eastus ' olarak kısıtlar
  - A aboneliği içindeki kaynak grubu B 'ye atandı
  - Denetim etkisi
  
Bu kurulum aşağıdaki sonuca neden olur:

- ' Eastus ' içindeki kaynak grubu B 'de bulunan herhangi bir kaynak, ilke 2 ile uyumlu ve ilke 1 ile uyumlu değil
- Kaynak grubu B 'de ' eastus ' içinde olmayan herhangi bir kaynak, ilke 2 ile uyumlu değil ve ' westus ' içinde değilse ilke 1 ile uyumlu değil
- ' Westus ' içinde olmayan abonelikte bulunan yeni kaynaklar ilke 1 tarafından reddedildi
- A aboneliği ve ' westus ' içindeki kaynak grubu B 'deki tüm yeni kaynaklar, ilke 2 ' de oluşturulur ve uyumlu değildir

İlke 1 ve ilke 2 ' nin reddetme etkisi varsa, durum şu şekilde değişir:

- Kaynak grubu B 'de ' eastus ' içinde olmayan herhangi bir kaynak, ilke 2 ile uyumlu değil
- Kaynak grubu B 'de ' westus ' içinde olmayan herhangi bir kaynak, ilke 1 ile uyumlu değil
- ' Westus ' içinde olmayan abonelikte bulunan yeni kaynaklar ilke 1 tarafından reddedildi
- A aboneliğinin B kaynak grubundaki tüm yeni kaynaklar reddedildi

Her atama tek tek değerlendirilir. Bu nedenle, bir kaynağın kapsamdaki farklılıklar arasından bir boşluk ile bir kaynak için bir fırsat yoktur. Katman ilkelerinin veya ilke çakışmasayısının net sonucu, **en çok kısıtlayıcı**olarak kabul edilir. Örnek olarak, hem ilke 1 hem de 2 bir reddetme efekti içeriyorsa, çakışan ve çakışan ilkeler tarafından bir kaynak engellenir. Hala hedef kapsamda oluşturulacak kaynak gerekiyorsa, doğru ilkelerin doğru kapsamları etkilediğini doğrulamak için her atamadaki dışlamaları gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure ilke örneklerindeki](../samples/index.md)örnekleri gözden geçirin.
- [Azure İlkesi tanımı yapısını](definition-structure.md) gözden geçirin.
- [Program aracılığıyla ilkelerin nasıl oluşturulduğunu](../how-to/programmatically-create.md)anlayın.
- [Uyumluluk verilerini nasıl alabileceğinizi](../how-to/getting-compliance-data.md)öğrenin.
- [Uyumlu olmayan kaynakları nasıl düzelteceğinizi](../how-to/remediate-resources.md)öğrenin.
- [Kaynakları Azure Yönetim gruplarıyla düzenleme](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu inceleyin.
