---
title: Efektlerin nasıl çalıştığını anlama
description: Azure İlke tanımlarının, uyumluluğun nasıl yönetildiğini ve raporlanın nasıl belirlendiğini belirleyen çeşitli etkileri vardır.
ms.date: 03/23/2020
ms.topic: conceptual
ms.openlocfilehash: 631c941173a500a4159a37c7c31107b9a6eab872
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80239975"
---
# <a name="understand-azure-policy-effects"></a>Azure İlkesi efektlerini anlama

Azure İlkesi'ndeki her ilke tanımı tek bir etkiye sahiptir. Bu efekt, ilke kuralı eşleşecek şekilde değerlendirildiğinde ne olacağını belirler. Efektler, yeni bir kaynak, güncelleştirilmiş bir kaynak veya varolan bir kaynak içinyse farklı şekilde davranılır.

Bu etkiler şu anda bir ilke tanımında desteklenir:

- [Ekle](#append)
- [Denetim](#audit)
- [AuditifNotExists](#auditifnotexists)
- [Inkar](#deny)
- [DeployIfNotExists](#deployifnotexists)
- [Devre dışı](#disabled)
- [EnforceOPAConstraint](#enforceopaconstraint) (önizleme)
- [EnforceRegoPolicy](#enforceregopolicy) (önizleme)
- [Değiştir](#modify)

## <a name="order-of-evaluation"></a>Değerlendirme sırası

Azure Resource Manager aracılığıyla kaynak oluşturma veya güncelleştirme istekleri önce Azure İlkesi tarafından değerlendirilir. Azure İlkesi, kaynağa uygulanan tüm atamaların bir listesini oluşturur ve sonra kaynağı her tanıma göre değerlendirir. Azure İlkesi, isteği ilgili Kaynak Sağlayıcısı'na vermeden önce birkaç efekti işler. Bunu yapmak, bir kaynak Azure İlkesi'nin tasarlanmış yönetim denetimlerini karşılamadığında Kaynak Sağlayıcı tarafından gereksiz olarak işlenmesini önler.

- İlke kuralının değerlendirilmesi gerekip gerekmediğini belirlemek için önce **devre dışı bırakılan** lar denetlenir.
- **Ek ve** **Değiştir** daha sonra değerlendirilir. İsteği değiştirebileceğinden, yapılan bir değişiklik denetimin tetiklemesini engelleyebilir veya efekti reddedebilir.
- **Reddet** daha sonra değerlendirilir. Denetimden önce reddet değerlendirilmesi yle, istenmeyen bir kaynağın çift günlüğe kaydedilmesi engellenir.
- **Denetim** daha sonra Kaynak Sağlayıcıya giden istek önce değerlendirilir.

Kaynak Sağlayıcı bir başarı kodu döndürür sonra, **AuditIfNotExists** ve **DeployIfNotExists** ek uyumluluk günlüğü veya eylem gerekli olup olmadığını belirlemek için değerlendirmek.

Şu anda **EnforceOPAConstraint** veya **EnforceRegoPolicy** etkileri için herhangi bir değerlendirme sırası yoktur.

## <a name="disabled"></a>Devre dışı

Bu efekt, durumları sınamak veya ilke tanımının efekti parametreye yaptığı durumlar için yararlıdır. Bu esneklik, bu ilkenin tüm atamalarını devre dışı bırakmak yerine tek bir atamayı devre dışı bırakmayı mümkün kılar.

Devre dışı bırakma efektine alternatif olarak, ilke ataması üzerinde ayarlanan **enforcementMode'dur.**
**EnforcementMode** _Devre Dışı_bırakıldığında, kaynaklar yine de değerlendirilir. Etkinlik günlükleri gibi günlüğe kaydetme ve ilke etkisi oluşmaz. Daha fazla bilgi için [ilke atama - zorlama moduna](./assignment-structure.md#enforcement-mode)bakın.

## <a name="append"></a>Ekle

Ek oluşturma veya güncelleştirme sırasında istenen kaynağa ek alanlar eklemek için kullanılır. Yaygın bir örnek, depolama kaynağı için izin verilen IP'leri belirtmektir.

> [!IMPORTANT]
> Ek, etiket olmayan özelliklere sahip kullanım için tasarlanmıştır. Append, oluşturma veya güncelleştirme isteği sırasında kaynağa etiket ekleyebilir, ancak bunun yerine etiketler için [Değiştir](#modify) efektini kullanması önerilir.

### <a name="append-evaluation"></a>Ek değerlendirme

Ek, bir kaynağın oluşturulması veya güncellenmesi sırasında bir Kaynak Sağlayıcısı tarafından istek işlenmeden önce değerlendirilir. İlke kuralının **if** koşulu karşılandığında ek kaynağa alanlar ekler. Ek efekti özgün istekteki bir değeri farklı bir değerle geçersiz kılarsa, reddet efekti görevi görür ve isteği reddeder. Varolan bir diziye yeni bir değer eklemek için takma adın **[\*]** sürümünü kullanın.

Ek efektini kullanan bir ilke tanımı bir değerlendirme döngüsünün parçası olarak çalıştırıldığında, zaten var olan kaynaklarda değişiklik yapmaz. Bunun yerine, **if** koşulunu uyumlu olmayan olarak karşılayan herhangi bir kaynağı işaretler.

### <a name="append-properties"></a>Ek özellikler

Bir ek efekti yalnızca gerekli olan bir **ayrıntı** dizisi vardır. **Ayrıntılar** bir dizi olduğundan, tek bir **alan/değer** çifti veya katları alabilir. Kabul edilebilir alanlar listesi için [tanım yapısına](definition-structure.md#fields) bakın.

### <a name="append-examples"></a>Ek örnekler

Örnek 1: Bir depolama hesabında IP kuralları ayarlamak için dizi **değeri** olan bir non-**[\*]** takma [ad](definition-structure.md#aliases) ını kullanan tek **alan/değer** çifti. [**]\*** olmayan diğer ad bir dizi olduğunda, efekt tüm dizi olarak **değeri** ekler. Dizi zaten varsa, çakışma bir reddi olayı oluşur.

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

Örnek 2: Bir depolama hesabında IP kuralları ayarlamak için dizi **değeri** olan bir **[\*]** [takma adını](definition-structure.md#aliases) kullanan tek **alan/değer** çifti. **[ ]\*** diğer adını kullanarak, efekt **değeri** önceden varolan bir diziye ekler. Dizi henüz yoksa, oluşturulur.

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

Değişiklik, oluşturma veya güncelleştirme sırasında bir kaynağa etiket eklemek, güncelleştirmek veya kaldırmak için kullanılır. Yaygın bir örnek costCenter gibi kaynaklarda etiketleri güncelleştirmektir. Hedef kaynak kaynak `mode` grubu olmadığı sürece, Değiştir ilkesi her zaman _Dizinlenmiş_ olarak ayarlanmış olmalıdır. Varolmayan uyumlu olmayan kaynaklar bir [düzeltme görevi](../how-to/remediate-resources.md)yle düzeltilebilir. Tek bir Değişiklik kuralının herhangi bir sayıda işlemi olabilir.

> [!IMPORTANT]
> Değiştir şu anda yalnızca etiketlerle kullanılmak üzere. Etiketleri yönetiyorsanız, Değiştir ek işlem türleri ve varolan kaynakları düzeltme olanağı sağladığından, Ek yerine Değiştir'i kullanmanız önerilir. Ancak, yönetilen bir kimlik oluşturamıyorsanız Append önerilir.

### <a name="modify-evaluation"></a>Değerlendirmeyi değiştirin

Kaynak oluşturulması veya güncellenmesi sırasında istek Bir Kaynak Sağlayıcısı tarafından işlenmeden önce değerlendirmeleri değiştirin. İlke kuralının **if** koşulu karşılandığında, kaynaktaki ekler veya güncelleştirme etiketlerini değiştirin.

Değiştir efektini kullanan bir ilke tanımı bir değerlendirme döngüsünün parçası olarak çalıştırıldığında, zaten var olan kaynaklarda değişiklik yapmaz. Bunun yerine, **if** koşulunu uyumlu olmayan olarak karşılayan herhangi bir kaynağı işaretler.

### <a name="modify-properties"></a>Özellikleri değiştirme

Değiştir efektinin **ayrıntılar** özelliği, düzeltme için gereken izinleri ve etiket değerlerini eklemek, güncelleştirmek veya kaldırmak için kullanılan **işlemleri** tanımlayan tüm alt özelliklere sahiptir.

- **roleDefinitionIds** [gerekli]
  - Bu özellik, abonelik tarafından erişilebilir rol tabanlı erişim denetimi rol kimliği eşleşen dizeleri bir dizi içermelidir. Daha fazla bilgi için düzeltmeye bakın [- ilke tanımını yapılandırın.](../how-to/remediate-resources.md#configure-policy-definition)
  - Tanımlanan [rol, Katılımcı](../../../role-based-access-control/built-in-roles.md#contributor) rolüne verilen tüm işlemleri içermelidir.
- **işlemleri** [gerekli]
  - Eşleşen kaynaklarda tamamlanacak tüm etiket işlemleri dizisi.
  - Özellikler:
    - **işlem** [gerekli]
      - Eşleşen bir kaynakta hangi eylemin gerçekleşsüreceğini tanımlar. Seçenekler şunlardır: _addOrReplace_, _Ekle_, _Kaldır_. [Ek efektine](#append) benzer şekilde ki gibi şeyler _ekleyin._
    - **alan** [gerekli]
      - Eklenecek, değiştirilecek veya kaldırılacak etiket. Etiket adları diğer [alanlar](./definition-structure.md#fields)için aynı adlandırma kuralına uymalıdır.
    - **değer** (isteğe bağlı)
      - Etiketi ayarlamak için değer.
      - Bu özellik, **işlem** _addOrReplace_ veya _Add_ise gereklidir.

### <a name="modify-operations"></a>İşlemleri değiştirme

**İşlemözelliği** dizisi, tek bir ilke tanımından farklı şekillerde birkaç etiketi değiştirmeyi mümkün kılar. Her işlem **işlem,** **alan**ve **değer** özelliklerinden oluşur. İşlem, düzeltme görevinin etiketlere ne yaptığını belirler, alan hangi etiketin değiştirildiğini belirler ve değer bu etiket için yeni ayarı tanımlar. Aşağıdaki örnekte aşağıdaki etiket değişiklikleri yapar:

- `environment` Etiketi, farklı bir değerle zaten var olsa bile "Test" olarak ayarlar.
- Etiketi `TempResource`kaldırır.
- İlke `Dept` ataması üzerinde yapılandırılan ilke parametresi _Bölümü_ etiketini ayarlar.

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

**İşlem** özelliği aşağıdaki seçeneklere sahiptir:

|İşlem |Açıklama |
|-|-|
|addOrReplace |Etiket zaten farklı bir değerle olsa bile, tanımlanan etiketi ve değeri kaynağa ekler. |
|Ekle |Tanımlanan etiketi ve değeri kaynağa ekler. |
|Kaldır |Tanımlanan etiketi kaynaktan kaldırır. |

### <a name="modify-examples"></a>Örnekleri değiştirme

Örnek 1: `environment` Etiketi ekleyin `environment` ve varolan etiketleri "Test" ile değiştirin:

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

Örnek 2: `env` Etiketi kaldırın `environment` ve etiketi `environment` ekleyin veya varolan etiketleri parametreli bir değerle değiştirin:

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

## <a name="deny"></a>Reddet

Reddet, bir ilke tanımı yla tanımlanan standartlarla eşleşmeyen ve isteği başarısız olan bir kaynak isteğini önlemek için kullanılır.

### <a name="deny-evaluation"></a>Değerlendirmeyi reddet

Eşleşen bir kaynak oluştururken veya güncellerken, kaynak sağlayıcısına gönderilmeden önce isteği reddet engellenir. İstek bir `403 (Forbidden)`. Portalda, Yasak, ilke ataması tarafından engellenen dağıtımda bir durum olarak görülebilir.

Varolan kaynakların değerlendirilmesi sırasında, reddet ilkesi tanımıyla eşleşen kaynaklar uyumlu olmayan olarak işaretlenir.

### <a name="deny-properties"></a>Özellikleri reddetme

İnkar efekti, ilke tanımının **ardından** kullanmak için ek özelliklere sahip değildir.

### <a name="deny-example"></a>Örneği reddet

Örnek: Reddet efektini kullanma.

```json
"then": {
    "effect": "deny"
}
```

## <a name="audit"></a>Denetim

Denetim, uyumlu olmayan bir kaynağı değerlendirirken etkinlik günlüğünde bir uyarı olayı oluşturmak için kullanılır, ancak isteği durdurmaz.

### <a name="audit-evaluation"></a>Denetim değerlendirmesi

Denetim, bir kaynağın oluşturulması veya güncelletilenAzure İlkesi tarafından denetlenen son etkidir. Azure İlkesi daha sonra kaynağı Kaynak Sağlayıcı'ya gönderir. Denetim, kaynak isteği ve değerlendirme döngüsü için aynı şekilde çalışır. Azure İlkesi etkinlik günlüğüne bir `Microsoft.Authorization/policies/audit/action` işlem ekler ve kaynağı uyumlu olmayan olarak işaretler.

### <a name="audit-properties"></a>Denetim özellikleri

Denetim efekti, ilke tanımının **ardından** kullanmak için ek özelliklere sahip değildir.

### <a name="audit-example"></a>Denetim örneği

Örnek: Denetim efektini kullanma.

```json
"then": {
    "effect": "audit"
}
```

## <a name="auditifnotexists"></a>AuditifNotExists

AuditIfNotExists, **if** koşuluyla eşleşen kaynaklar üzerinde denetim yapılmasını sağlar, ancak **daha sonra** koşulun **ayrıntılarında** belirtilen bileşenlere sahip değildir.

### <a name="auditifnotexists-evaluation"></a>AuditIfNotExists değerlendirmesi

AuditIfNotExists, kaynak sağlayıcısı oluşturma veya güncelleştirme kaynak isteğini ele aldıktan ve bir başarı durum kodu döndürdünden sonra çalışır. Denetim, ilgili kaynak yoksa veya **ExistenceCondition** tarafından tanımlanan kaynaklar doğru olarak değerlendirilmiyorsa oluşur. Azure İlkesi, etkinlik günlüğüne denetim efektiyle aynı şekilde bir `Microsoft.Authorization/policies/audit/action` işlem ekler. Tetiklendiğinde, **if** koşulunu karşılayan kaynak, uyumlu olmayan olarak işaretlenmiş kaynaktır.

### <a name="auditifnotexists-properties"></a>AuditIfNotExists özellikleri

AuditIfNotExists efektlerinin **ayrıntıları** özelliği, eşleşen ilgili kaynakları tanımlayan tüm alt özelliklere sahiptir.

- **Yazın** [gerekli]
  - Eşleşmek için ilgili kaynağın türünü belirtir.
  - **Details.type** **if** koşul kaynağının altında bir kaynak türüyse, değerlendirilen kaynak kapsamında bu **türdeki** kaynaklar için ilke sorguları. Aksi takdirde, değerlendirilen kaynakla aynı kaynak grubunda ilke sorguları.
- **Ad** (isteğe bağlı)
  - Eşleşmek için kaynağın tam adını belirtir ve belirtilen türdeki tüm kaynaklar yerine belirli bir kaynak almak için ilke neden olur.
  - **if.field.type** ve **then.details.type** match için koşul değerleri _required_ olduğunda, `[field('name')]` **Ad** gerekli olur ve olmalıdır . Ancak, bunun yerine bir [denetim](#audit) etkisi göz önünde bulundurulmalıdır.
- **ResourceGroupName** (isteğe bağlı)
  - İlgili kaynağın eşleşmesinin farklı bir kaynak grubundan gelmesini sağlar.
  - **Tür** **if** koşul kaynağı altında olacak bir kaynak ise uygulanmaz.
  - Varsayılan durum **kaynağının** kaynak grubudur.
- **ExistenceScope** (isteğe bağlı)
  - İzin verilen değerler _Abonelik_ ve _Kaynak Grubu'dur._
  - Eşleşmek için ilgili kaynağın nereden getirilebileceği kapsamını ayarlar.
  - **Tür** **if** koşul kaynağı altında olacak bir kaynak ise uygulanmaz.
  - _ResourceGroup_için, **if** koşul kaynağının kaynak grubu veya **ResourceGroupName'de**belirtilen kaynak grubu ile sınırlaştır.
  - _Abonelik_için, ilgili kaynak için tüm aboneliği sorgular.
  - Varsayılan _kaynak grubudur._
- **ExistenceCondition** (isteğe bağlı)
  - Belirtilmemişse, ilgili **türdeki** herhangi bir kaynak efekti karşılar ve denetimi tetiklemez.
  - **If** koşulu için ilke kuralıyla aynı dili kullanır, ancak ilgili her kaynağa karşı ayrı ayrı değerlendirilir.
  - Eşleşen ilişkili kaynaklardan herhangi biri doğru olarak değerlendirirse, efekt tatmin olur ve denetimi tetiklemez.
  - [alan()] **if** durumundaki değerlerle eşdeğerliği denetlemek için kullanabilirsiniz.
  - Örneğin, ana kaynağın **(if** koşulunda) eşleşen ilgili kaynakla aynı kaynak konumunda olduğunu doğrulamak için kullanılabilir.

### <a name="auditifnotexists-example"></a>AuditIfNotExists örneği

Örnek: Kötü Amaçlı Yazılımdan Koruma uzantısı nın var olup olmadığını belirlemek için Sanal Makineleri değerlendirir ve eksik olduğunda denetler.

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

AuditIfNotExists'e benzer şekilde, Bir DeployIfNotExists ilkesi tanımı, koşul karşılandığında şablon dağıtımını yürütür.

> [!NOTE]
> [İç içe şablonlar](../../../azure-resource-manager/templates/linked-templates.md#nested-template) **deployIfNotExists**ile desteklenir, ancak [bağlantılı şablonlar](../../../azure-resource-manager/templates/linked-templates.md#linked-template) şu anda desteklenmez.

### <a name="deployifnotexists-evaluation"></a>DeployIfNotExists değerlendirmesi

DeployIfNotExists, kaynak sağlayıcısı oluşturma veya güncelleştirme kaynak isteğini ele aldıktan ve bir başarı durum kodu döndürürsonra yaklaşık 15 dakika çalışır. İlgili kaynak yoksa veya **ExistenceCondition** tarafından tanımlanan kaynaklar doğru olarak değerlendirilmiyorsa şablon dağıtımı oluşur.
Dağıtım süresi şablonda yer alan kaynakların karmaşıklığına bağlıdır.

Bir değerlendirme döngüsü sırasında, kaynakları eşleştiren DeployIfNotExists etkisine sahip ilke tanımları uyumlu değil olarak işaretlenir, ancak bu kaynak üzerinde hiçbir işlem yapılmaz. Varolmayan uyumlu olmayan kaynaklar bir [düzeltme görevi](../how-to/remediate-resources.md)yle düzeltilebilir.

### <a name="deployifnotexists-properties"></a>DeployIfNotExists özellikleri

DeployIfNotExists efektinin **ayrıntılar** özelliği, eşleşmek için ilgili kaynakları tanımlayan tüm alt özelliklere ve yürütülecek şablon dağıtımına sahiptir.

- **Yazın** [gerekli]
  - Eşleşmek için ilgili kaynağın türünü belirtir.
  - **If** koşul kaynağının altında bir kaynak getirmeye çalışarak başlar, sonra **if** koşul kaynağı yla aynı kaynak grubu içinde sorgular.
- **Ad** (isteğe bağlı)
  - Eşleşmek için kaynağın tam adını belirtir ve belirtilen türdeki tüm kaynaklar yerine belirli bir kaynak almak için ilke neden olur.
  - **if.field.type** ve **then.details.type** match için koşul değerleri _required_ olduğunda, `[field('name')]` **Ad** gerekli olur ve olmalıdır .
- **ResourceGroupName** (isteğe bağlı)
  - İlgili kaynağın eşleşmesinin farklı bir kaynak grubundan gelmesini sağlar.
  - **Tür** **if** koşul kaynağı altında olacak bir kaynak ise uygulanmaz.
  - Varsayılan durum **kaynağının** kaynak grubudur.
  - Şablon dağıtımı yürütülürse, bu değerin kaynak grubunda dağıtılır.
- **ExistenceScope** (isteğe bağlı)
  - İzin verilen değerler _Abonelik_ ve _Kaynak Grubu'dur._
  - Eşleşmek için ilgili kaynağın nereden getirilebileceği kapsamını ayarlar.
  - **Tür** **if** koşul kaynağı altında olacak bir kaynak ise uygulanmaz.
  - _ResourceGroup_için, **if** koşul kaynağının kaynak grubu veya **ResourceGroupName'de**belirtilen kaynak grubu ile sınırlaştır.
  - _Abonelik_için, ilgili kaynak için tüm aboneliği sorgular.
  - Varsayılan _kaynak grubudur._
- **ExistenceCondition** (isteğe bağlı)
  - Belirtilmemişse, ilgili **türdeki** herhangi bir kaynak efekti karşılar ve dağıtımı tetiklemez.
  - **If** koşulu için ilke kuralıyla aynı dili kullanır, ancak ilgili her kaynağa karşı ayrı ayrı değerlendirilir.
  - Eşleşen ilişkili kaynaklardan herhangi biri doğru olarak değerlendirirse, efekt tatmin olur ve dağıtımı tetiklemez.
  - [alan()] **if** durumundaki değerlerle eşdeğerliği denetlemek için kullanabilirsiniz.
  - Örneğin, ana kaynağın **(if** koşulunda) eşleşen ilgili kaynakla aynı kaynak konumunda olduğunu doğrulamak için kullanılabilir.
- **roleDefinitionIds** [gerekli]
  - Bu özellik, abonelik tarafından erişilebilir rol tabanlı erişim denetimi rol kimliği eşleşen dizeleri bir dizi içermelidir. Daha fazla bilgi için düzeltmeye bakın [- ilke tanımını yapılandırın.](../how-to/remediate-resources.md#configure-policy-definition)
- **DeploymentScope** (isteğe bağlı)
  - İzin verilen değerler _Abonelik_ ve _Kaynak Grubu'dur._
  - Tetiklenecek dağıtım türünü ayarlar. _Abonelik,_ [abonelik düzeyinde](../../../azure-resource-manager/templates/deploy-to-subscription.md)bir dağıtımı, _Kaynak Grubu'nun_ bir kaynak grubuna dağıtımını gösterir.
  - Abonelik düzeyi dağıtımları kullanırken _Dağıtım'da_ bir _konum_ özelliği belirtilmelidir.
  - Varsayılan _kaynak grubudur._
- **Dağıtım** [gerekli]
  - Bu özellik, PUT API'ye geçirileceği `Microsoft.Resources/deployments` için tam şablon dağıtımını içermelidir. Daha fazla bilgi için [Dağıtımlar REST API'ye](/rest/api/resources/deployments)bakın.

  > [!NOTE]
  > **Dağıtım** özelliğiiçindeki tüm işlevler ilke olarak değil şablonun bileşenleri olarak değerlendirilir. Özel durum, ilkeden şablona değerleri geçen **parametreler** özelliğidir. Bu bölümdeki bir şablon parametre adı altındaki **değer,** bu değerin geçişini gerçekleştirmek için kullanılır _(bkz._

### <a name="deployifnotexists-example"></a>DeployIfNotExists örneği

Örnek: SaydamDataEncryption etkin olup olmadığını belirlemek için SQL Server veritabanlarını değerlendirir. Değilse, etkinleştirmek için bir dağıtım yürütülür.

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

## <a name="enforceopaconstraint"></a>EnforceOPAConstraint

Bu efekt, ilke *mode* tanımı `Microsoft.Kubernetes.Data`modu ile kullanılır. [Opa Constraint Framework](https://github.com/open-policy-agent/frameworks/tree/master/constraint#opa-constraint-framework) ile tanımlanan Gatekeeper v3 kabul kontrol kurallarını Azure'daki Kubernetes kümelerine [Açmak İlkes Ietkisini](https://www.openpolicyagent.org/) (OPA) geçirmek için kullanılır.

> [!NOTE]
> [Kubernetes için Azure İlkesi](aks-engine.md) Önizleme'dedir ve yalnızca yerleşik ilke tanımlarını destekler.

### <a name="enforceopaconstraint-evaluation"></a>EnforceOPAConstraint değerlendirmesi

Açık İlke Aracısı kabul denetleyicisi kümedeki yeni isteği gerçek zamanlı olarak değerlendirir.
Her 15 dakikada bir kümenin tam taraması tamamlanır ve sonuçlar Azure Politikası'na bildirilir.

### <a name="enforceopaconstraint-properties"></a>EnforceOPAConstraint özellikleri

EnforceOPAConstraint etkisinin **ayrıntıları** özelliği Gatekeeper v3 kabul kontrol kuralını açıklayan alt özelliklere sahiptir.

- **constraintTemplate** [gerekli]
  - Yeni Kısıtlamalar tanımlayan Kısıtlama şablonu CustomResourceDefinition (CRD). Şablon, Azure İlkesi'nden **gelen değerler** üzerinden geçirilen Rego mantığını, Kısıtlama şeasını ve Kısıtlama parametrelerini tanımlar.
- **kısıtlama** [gerekli]
  - Kısıtlama şablonunun CRD uygulaması. **Değerler** üzerinden geçirilen `{{ .Values.<valuename> }}`parametreleri kullanır. Aşağıdaki örnekte, bu `{{ .Values.cpuLimit }}` ve `{{ .Values.memoryLimit }}`.
- **değerleri** [isteğe bağlı]
  - Kısıtlamaya geçmek için herhangi bir parametre ve değer tanımlar. Her değer Kısıtlama şablonu CRD'de bulunmalıdır.

### <a name="enforceopaconstraint-example"></a>EnforceOPAConstraint örneği

Örnek: Gatekeeper v3 kabul kontrol kuralı Kubernetes konteyner CPU ve bellek kaynak sınırları ayarlamak için.

```json
"if": {
    "allOf": [
        {
            "field": "type",
            "in": [
                "Microsoft.ContainerService/managedClusters",
                "AKS Engine"
            ]
        },
        {
            "field": "location",
            "equals": "westus2"
        }
    ]
},
"then": {
    "effect": "enforceOPAConstraint",
    "details": {
        "constraintTemplate": "https://raw.githubusercontent.com/Azure/azure-policy/master/built-in-references/Kubernetes/container-resource-limits/template.yaml",
        "constraint": "https://raw.githubusercontent.com/Azure/azure-policy/master/built-in-references/Kubernetes/container-resource-limits/constraint.yaml",
        "values": {
            "cpuLimit": "[parameters('cpuLimit')]",
            "memoryLimit": "[parameters('memoryLimit')]"
        }
    }
}
```

## <a name="enforceregopolicy"></a>EnforceRegoPolitikası

Bu efekt, ilke *mode* tanımı `Microsoft.ContainerService.Data`modu ile kullanılır. [Azure Kubernetes Hizmeti'nde](../../../aks/intro-kubernetes.md) [Rego](https://www.openpolicyagent.org/docs/latest/policy-language/#what-is-rego) ile tanımlanan Gatekeeper v2 kabul kontrol kurallarını [Açık İlke Aracısına](https://www.openpolicyagent.org/) (OPA) geçirmek için kullanılır.

> [!IMPORTANT]
> [Kubernetes için Azure İlkesi](rego-for-aks.md) Önizleme'dedir ve yalnızca yerleşik ilke tanımlarını destekler. Yerleşik ilkeler **Kubernetes** kategorisindedir. **EnforceRegoPolicy** etkisi ve ilgili **Kubernetes Hizmet** kategori ilkeleri _amortismana sunuluyor._ Bunun yerine, güncelleştirilmiş [EnforceOPAConstraint efektini](#enforceopaconstraint) kullanın.

### <a name="enforceregopolicy-evaluation"></a>EnforceRegoPolicy değerlendirmesi

Açık İlke Aracısı kabul denetleyicisi kümedeki yeni isteği gerçek zamanlı olarak değerlendirir.
Her 5 dakikada bir, kümenin tam taraması tamamlanır ve sonuçlar Azure İlkesi'ne bildirilir.

### <a name="enforceregopolicy-properties"></a>EnforceRegoPolicy özellikleri

EnforceRegoPolicy etkisinin **ayrıntıları** özelliği Gatekeeper v2 kabul kontrol kuralını açıklayan alt özelliklere sahiptir.

- **policyId** [gerekli]
  - Benzersiz bir ad Rego kabul kontrol kuralına bir parametre olarak geçti.
- **politikası** [gerekli]
  - Rego kabul kontrol kuralının URI belirtir.
- **policyParametreler** [isteğe bağlı]
  - Rego ilkesine geçmek için herhangi bir parametre ve değer tanımlar.

### <a name="enforceregopolicy-example"></a>EnforceRegoPolicy örneği

Örnek: Gatekeeper v2 kabul kontrol kuralı AKS sadece belirtilen konteyner görüntüleri izin vermek.

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

## <a name="layering-policies"></a>Katmanlama ilkeleri

Bir kaynak birkaç atamadan etkilenebilir. Bu atamalar aynı kapsamda veya farklı kapsamlarda olabilir. Bu atamaların her biri de farklı bir etkisi tanımlanmış olması muhtemeldir. Her ilkenin durumu ve etkisi bağımsız olarak değerlendirilir. Örnek:

- İlke 1
  - Kaynak konumunu 'westus' ile sınırlandırıyor
  - Aboneye atanan A
  - Etkiyi reddet
- İlke 2
  - Kaynak konumunu 'eastus' ile sınırlandırıyor
  - A aboneliğinde kaynak grubuna atanan
  - Denetim etkisi
  
Bu kurulum aşağıdaki sonucu doğuracak:

- 'eastus'taki B kaynak grubunda bulunan herhangi bir kaynak ilke 2'ye uyumludur ve ilke 1'e uygun değildir
- 'eastus' olmayan kaynak grubu B'deki herhangi bir kaynak ilke 2'ye uygun değildir ve 'westus' değilse ilke 1'e uygun değildir
- Abonelikteki herhangi bir yeni kaynak 'westus'ta olmayan a ilke 1 tarafından reddedilir
- 'westus'taki abonelik A ve kaynak grubu B'deki herhangi bir yeni kaynak oluşturulur ve ilke 2'ye uygun değildir

Hem ilke 1 hem de ilke 2 reddetme etkisi yarattıysa, durum aşağıdakilere dönüşür:

- 'Eastus' kaynak grubu B'de olmayan herhangi bir kaynak ilke 2'ye uygun değildir
- B kaynak grubunda bulunan ve 'westus' olmayan herhangi bir kaynak ilke 1'e uygun değildir
- Abonelikteki herhangi bir yeni kaynak 'westus'ta olmayan a ilke 1 tarafından reddedilir
- Abonelik A'nın B kaynak grubundaki herhangi bir yeni kaynak reddedildi

Her atama ayrı ayrı değerlendirilir. Bu nedenle, bir kaynağın kapsam farklılıklarından bir boşluk tan kayması için bir fırsat yoktur. Katmanlama ilkelerinin veya ilke çakışmanın net sonucu **kümülatif en kısıtlayıcı**olarak kabul edilir. Örnek olarak, hem ilke 1 hem de 2'nin reddedici bir etkisi varsa, bir kaynak çakışan ve çakışan ilkeler tarafından engellenir. Hedef kapsamda oluşturulacak kaynağa hala ihtiyacınız varsa, doğru ilkeleri doğrulamak için her atamadaki dışlanmaları gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İlkesi örneklerindeki](../samples/index.md)örnekleri gözden geçirin.
- [Azure İlkesi tanımı yapısını](definition-structure.md) gözden geçirin.
- [İlkeleri programlı bir şekilde nasıl oluşturlayacağımı](../how-to/programmatically-create.md)anlayın.
- Uyumluluk verilerini nasıl [alacağınızı](../how-to/get-compliance-data.md)öğrenin.
- [Uyumlu olmayan kaynakları](../how-to/remediate-resources.md)nasıl düzelteriz öğrenin.
- [Azure yönetim gruplarıyla kaynaklarınızı düzenleyin](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu gözden geçirin.
