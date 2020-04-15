---
title: Kaynaklarınızı yönetim gruplarıyla düzenleme - Azure Yönetimi
description: Yönetim grupları, izinlerinin nasıl çalıştığı ve bu grupların nasıl kullanıldığı hakkında bilgi edinin.
ms.date: 04/15/2020
ms.topic: overview
ms.openlocfilehash: cc60e4555f0fb2b920b8061fb044ce5dde990d38
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81381531"
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Kaynaklarınızı Azure yönetim gruplarıyla düzenleme

Kuruluşunuzda birden fazla abonelik varsa bu abonelikler için verimli bir şekilde erişim, ilke ve uyumluluk yönetimi gerçekleştirmek isteyebilirsiniz. Azure yönetim grupları, aboneliklerin üzerinde bir kapsam düzeyi sunar. Abonelikleri "yönetim grupları" adlı kapsayıcılarla düzenler ve idare koşullarınızı bu yönetim gruplarına uygularsınız. Bir yönetim grubu içindeki aboneliklerin tümü otomatik olarak yönetim grubuna uygulanmış olan koşulları devralır. Yönetim grupları, sahip olabileceğiniz abonelik türüne bakılmaksızın kurumsal düzeyde yönetimi büyük ölçekte sunar.
Tek bir yönetim grubu içindeki tüm abonelikler aynı Azure Active Directory kiracısına güvenmelidir.

Örneğin, sanal makine (VM) oluşturma işlemi için kullanılabilir olan bölgeleri sınırlayan bir yönetim grubuna ilkeleri uygulayabilirsiniz. Bu ilke, yalnızca o bölgede VM oluşturulmasına izin vererek söz konusu yönetim grubu altındaki tüm yönetim gruplarına, aboneliklere ve kaynaklara uygulanır.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>Yönetim grupları ve abonelikler hiyerarşisi

Birleşik ilke ve erişim yönetimi için kaynaklarınızı bir hiyerarşi altında düzenlemek amacıyla yönetim grupları ve aboneliklerden oluşan esnek bir yapı inşa edebilirsiniz. Aşağıdaki diyagramda, yönetim grupları kullanılarak idare amaçlı bir hiyerarşi oluşturma örneği gösterilmektedir.

:::image type="content" source="./media/tree.png" alt-text="Yönetim grubu hiyerarşi ağacı örneği" border="false":::

İlke uygulayan bir hiyerarşi oluşturabilirsiniz. Örneğin bu, “Üretim” adlı grupta VM konumlarını ABD Batı Bölgesiyle sınırlayan bir ilke olabilir. Bu politika, bu yönetim grubunun soyundan gelen tüm Kurumsal Sözleşme (EA) aboneliklerine devredilecek ve bu abonelikler kapsamındaki tüm VM'ler için geçerli olacaktır. Bu güvenlik ilkesi kaynak veya abonelik sahibi tarafından değiştirilemez ve bu da idarenin geliştirilmesine olanak tanır.

Yönetim gruplarını kullanacağınız başka bir senaryo ise birden fazla aboneliğe kullanıcı erişimi sağlamaktır. Birden çok aboneliği söz konusu yönetim grubu altına taşıyarak, yönetim grubu üzerinde tüm aboneliklere erişimi devralacak bir [rol tabanlı erişim denetimi](../../role-based-access-control/overview.md) (RBAC) ataması oluşturabilirsiniz. Yönetim grubunda bir atama olması, farklı abonelikler üzerinde RBAC komut dosyası kullanmak yerine kullanıcıların ihtiyaç duydukları her şeye erişmesini sağlayabilir.

### <a name="important-facts-about-management-groups"></a>Yönetim grupları hakkında önemli bilgiler

- Tek bir dizinde 10.000 yönetim grubu desteklenebilir.
- Bir yönetim grubu en fazla altı derinlik düzeyini destekleyebilir.
  - Bu sınır, Kök düzeyini veya abonelik düzeyini içermez.
- Her yönetim grubu ve abonelik yalnızca bir üst öğeyi destekler.
- Her yönetim grubunda birçok alt öğe olabilir.
- Tüm abonelikler ve yönetim grupları, her bir dizindeki tek bir hiyerarşi içinde yer alır. Bkz. [Kök yönetim grubu hakkında önemli bilgiler](#important-facts-about-the-root-management-group).

## <a name="root-management-group-for-each-directory"></a>Her dizin için kök yönetim grubu

Her dizinde "Kök" yönetim grubu olarak adlandırılan tek bir üst düzey yönetim grubu bulunur. Diğer tüm yönetim grupları ve abonelikler hiyerarşide en üstte yer alan bu kök yönetim grubunun altındadır. Bu kök yönetim grubu, genel ilkelerin ve RBAC atamalarının dizin düzeyinde uygulanmasını sağlar. Başlangıçta Azure AD Genel Yöneticisinin bu kök grubunun Kullanıcı Erişimi Yönetici rolüne [kendisini yükseltmesi gerekir](../../role-based-access-control/elevate-access-global-admin.md). Yönetici, erişimi yükselttikten sonra hiyerarşiyi yönetmek için diğer dizin kullanıcılarına veya gruplara herhangi bir RBAC rolü atayabilir. Yönetici olarak, kendi hesabınızı kök yönetim grubunun sahibi olarak atayabilirsiniz.

### <a name="important-facts-about-the-root-management-group"></a>Kök yönetim grubu hakkında önemli bilgiler

- Varsayılan olarak kök yönetim grubunun görünen adı, **Kiracı kök grubu**’dur. Kimlik, Azure Active Directory Kimliği’dir.
- Görünen adı değiştirmek için hesabınızın, kök yönetim grubunun Sahip veya Katkıda Bulunan rolüne atanması gerekir. Bkz. Bir yönetim grubunun adını güncelleştirmek için [bir yönetim grubunun adını değiştir.](manage.md#change-the-name-of-a-management-group)
- Diğer yönetim gruplarının aksine kök yönetim grubu taşınamaz veya silinemez.  
- Tüm abonelikler ve yönetim grupları, dizinin içindeki bir kök yönetim grubu altında birleşir.
  - Dizindeki tüm kaynaklar, genel yönetim için kök yönetim grubu altında birleşir.
  - Yeni abonelikler, oluşturulduğunda kök yönetim grubuna otomatik olarak eklenir.
- Tüm Azure müşterileri kök yönetim grubunu görebilir ancak tüm müşteriler o kök yönetim grubunu yönetmek için erişime sahip değildir.
  - Bir aboneliğe erişimi olan herkes bu aboneliğin hiyerarşide bulunduğu bağlamı görebilir.  
  - Kök yönetim grubuna hiç kimsenin varsayılan erişimi yoktur. Erişim kazanmak için yalnızca Azure AD Genel Yöneticileri kendi rollerini yükseltebilir. Kök yönetim grubuna erişebildiklerinde, global yöneticiler yönetmek için diğer kullanıcılara herhangi bir RBAC rolü atayabilir  
    bu kadar.
- SDK'da, kök yönetim grubu veya 'Kiracı Kök', bir yönetim grubu olarak çalışır.

> [!IMPORTANT]
> Kök yönetim grubu grubunda yapılan herhangi bir kullanıcı erişimi ataması veya ilke ataması, **dizin içindeki tüm kaynaklara uygulanır**. Bu nedenle, tüm müşterilerin bu kapsamda tanımlanmış öğelere sahip olma gereksinimini değerlendirmesi gerekir. Kullanıcı erişimi ve ilke atamaları yalnızca bu noktada "Sahip Olunması Gereken" olmalıdır  
> Kapsam.

## <a name="initial-setup-of-management-groups"></a>Yönetim gruplarının ilk ayarı

Herhangi bir kullanıcı yönetim gruplarını kullanmaya başladığında gerçekleştirilen bir ilk ayar işlemi vardır. İlk adım, dizinde kök yönetim grubunun oluşturulmasıdır. Bu grup oluşturulduktan sonra dizinde mevcut olan tüm abonelikler, kök yönetim grubunun alt öğesi yapılır.
Bu işlemin yapılmasının nedeni, bir dizin içinde yalnızca bir yönetim grubu hiyerarşisi olmasını sağlamaktır. Dizin içinde tek hiyerarşi olması, yönetim müşterilerinin dizindeki diğer müşteriler tarafından atlanamayacak genel erişim ve ilkeler uygulamasına olanak tanır. Kökte atanan her şey hiyerarşinin tamamına uygulanır. Buna söz konusu Azure AD Kiracısı içindeki tüm yönetim grupları, abonelikler, kaynak grupları ve kaynaklar dahildir.

## <a name="trouble-seeing-all-subscriptions"></a>Tüm abonelikler görüntülenirken oluşan sorun

25 Haziran 2018’den önce, önizlemenin ilk aşamasında yönetim gruplarını kullanmaya başlayan birkaç dizin, tüm aboneliklerin hiyerarşi içinde yer almamasıyla ilgili bir hatayla karşılaşabiliyordu. Tüm abonelikleri hiyerarşiye almaya yönelik işlem, dizindeki kök yönetim grubunda bir rol veya ilke ataması yapıldıktan sonra gerçekleştiriliyordu.

### <a name="how-to-resolve-the-issue"></a>Sorunu çözme

Bu sorunu çözmek için kullanabileceğiniz iki seçenek vardır.

- Kök yönetim grubundan tüm Rol ve İlke atamalarını kaldırma
  - Kök yönetim grubundan bir ilke ve rol atamasını kaldırdığınızda, bir sonraki gece döngüsünde hizmet tüm abonelikleri hiyerarşiye geriye dönük olarak doldurur. Bu işlemde, tüm kiracı aboneliklerine yanlışlıkla erişim verilmediğinden ve ilke ataması yapılmadığından emin olunur.
  - Hizmetlerinizi etkilemeden bu işlemi yapmanın en iyi yolu, rolü veya ilke atamasını Kök yönetim grubunun bir alt düzeyine uygulamaktır. Daha sonra tüm atamaları kök kapsamından çıkarabilirsiniz.
- Geriye dönük doldurma işlemini başlatmak için doğrudan API’yi çağırın
  - Dizindeki herhangi bir müşteri _TenantBackfillStatusRequest_ veya _StartTenantBackfillRequest_ API’lerini çağırabilir. StartTenantBackfillRequest API’si çağrıldığında tüm abonelikleri hiyerarşiye taşımanın ilk kurulum işlemini başlatır. Ayrıca bu işlem, tüm yeni abonelikleri kök yönetim grubunun alt öğesi olmaya zorlama işlemini de başlatır.
    Bu işlem, kök düzeyde hiçbir atama değiştirilmeden yapılabilir. API'yi çağırarak, kökteki her ilke veya erişim atamasının tüm aboneliklere uygulanmasının sorun olmadığını belirtmiş olursunuz.

Bu geriye dönük doldurma işlemi hakkında sorularınız varsa şu adresten bize ulaşın: `managementgroups@microsoft.com`
  
## <a name="management-group-access"></a>Yönetim grubu erişimi

Azure yönetim grupları tüm kaynak erişimleri ve rol tanımları için [Azure Rol Tabanlı Erişim Denetimi’ni (RBAC)](../../role-based-access-control/overview.md) destekler.
Bu izinler, hiyerarşide mevcut olan alt kaynaklara devredilir. Herhangi bir RBAC rolü, hiyerarşiyi kaynaklara devralacak bir yönetim grubuna atanabilir. Örneğin, VM katılımcısı adlı RBAC rolü bir yönetim grubuna atanabilir. Bu rolün yönetim grubu üzerinde herhangi bir eylemi yoktur ancak o yönetim grubu altındaki tüm VM’lere devredilir.

Aşağıdaki grafikte rollerin listesi ve yönetim gruplarında desteklenen eylemler gösterilmektedir.

| RBAC Rol Adı             | Oluştur | Rename | Hareket\*\* | Sil | Erişim Ata | İlke Ata | Okuma  |
|:-------------------------- |:------:|:------:|:--------:|:------:|:-------------:| :------------:|:-----:|
|Sahip                       | X      | X      | X        | X      | X             | X             | X     |
|Katılımcı                 | X      | X      | X        | X      |               |               | X     |
|MG Katılımcısı\*            | X      | X      | X        | X      |               |               | X     |
|Okuyucu                      |        |        |          |        |               |               | X     |
|MG Okuyucu\*                 |        |        |          |        |               |               | X     |
|Kaynak İlkesine Katkıda Bulunan |        |        |          |        |               | X             |       |
|Kullanıcı Erişimi Yöneticisi   |        |        |          |        | X             | X             |       |

\*: MG Katılımcısı ve MG Reader, kullanıcıların bu eylemleri yalnızca yönetim grubu kapsamında yapmalarına izin verir.  
\*\*: Root yönetim grubundaki Rol Atamaları'nın bir abonelik veya yönetim grubunu bu gruba taşıması gerekmez. Hiyerarşi içindeki öğeleri taşımayla ilgili ayrıntılar için bkz. [Kaynaklarınızı yönetim gruplarıyla yönetme](manage.md).

## <a name="custom-rbac-role-definition-and-assignment"></a>Özel RBAC rol tanımı ve atama

Yönetim grupları için özel RBAC rol desteği şu anda bazı [sınırlamalarla](#limitations)önizlemededir. Rol Tanımının atanabilir kapsamında yönetim grubu kapsamını tanımlayabilirsiniz. Ardından bu özel RBAC Rolü söz konusu yönetim grubunda ve onun altında yer alan tüm yönetim grupları, abonelikler, kaynak grupları veya kaynaklarda atama için kullanılabilir. Bu özel rol, herhangi bir yerleşik rol gibi hiyerarşide aşağıya doğru devralınır.  

### <a name="example-definition"></a>Örnek tanım

[Özel bir rol tanımlamak ve oluşturmak,](../../role-based-access-control/custom-roles.md) yönetim gruplarının eklenmesiyle değişmez. Yönetim grubunu **/sağlayıcıları/Microsoft.Management/managementgroups/{groupId}** tanımlamak için tam yolu kullanın.

Yönetim grubunun görüntü adını değil, yönetim grubunun kimliğini kullanın. Bu yaygın hata, her ikisi de bir yönetim grubu oluştururken özel tanımlanmış alanlar olduğundan olur.

```json
...
{
  "Name": "MG Test Custom Role",
  "Id": "id", 
  "IsCustom": true,
  "Description": "This role provides members understand custom roles.",
  "Actions": [
    "Microsoft.Management/managementgroups/delete",
    "Microsoft.Management/managementgroups/read",
    "Microsoft.Management/managementgroup/write",
    "Microsoft.Management/managementgroup/subscriptions/delete",
    "Microsoft.Management/managementgroup/subscriptions/write",
    "Microsoft.resources/subscriptions/read",
    "Microsoft.Authorization/policyAssignments/*",
    "Microsoft.Authorization/policyDefinitions/*",
    "Microsoft.Authorization/policySetDefinitions/*",
    "Microsoft.PolicyInsights/*",
    "Microsoft.Authorization/roleAssignments/*",
    "Microsoft.Authorization/roledefinitions/*"
  ],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": [
        "/providers/microsoft.management/managementGroups/ContosoCorporate"
  ]
}
...
```

### <a name="issues-with-breaking-the-role-definition-and-assignment-hierarchy-path"></a>Rol tanımı ve atama hiyerarşisi yolunun kırılmasıyla ilgili sorunlar

Rol tanımları, yönetim grubu hiyerarşisi içinde herhangi bir yerde atanabilir kapsamdır. Alt abonelikte gerçek rol ataması varken, bir üst yönetim grubunda rol tanımı tanımlanabilir. İki öğe arasında bir ilişki olduğundan, atamayı tanımından ayırmaya çalışırken bir hata alırsınız.

Örneğin, bir görsel için hiyerarşinin küçük bir bölümüne bakalım.

:::image type="content" source="./media/subtree.png" alt-text="alt ağaç" border="false":::

Pazarlama yönetim grubunda tanımlanmış özel bir rol olduğunu varsayalım. Bu özel rol daha sonra iki ücretsiz deneme aboneliğine atanır.  

Bu aboneliklerden birini Üretim yönetimi grubunun çocuğu olarak taşımaya çalışırsak, bu hareket abonelik rol atamasından Pazarlama yönetimi grubu rol tanımına giden yolu kırar. Bu senaryoda, bu ilişkiyi keseceği için taşımaya izin verilmediğini belirten bir hata alırsınız.  

Bu senaryoyu düzeltmek için birkaç farklı seçenek vardır:
- Aboneliği yeni bir üst MG'ye taşımadan önce rol atamasını abonelikten kaldırın.
- Aboneliği Rol Tanımı'nın devredilebilir kapsamına ekleyin.
- Rol tanımı içinde devratılabilir kapsamı değiştirin. Yukarıdaki örnekte, tanım hiyerarşinin her iki dalı tarafından ulaşılabilmesi için devratılabilir kapsamları Pazarlama'dan Kök Yönetim Grubu'na güncelleştirebilirsiniz.  
- Diğer dalda tanımlanacak ek bir Özel Rol oluşturun. Bu yeni rol, rol atamasının abonelikte de değiştirilmesini gerektirir.  

### <a name="limitations"></a>Sınırlamalar  

Yönetim gruplarında özel roller kullanırken var olan sınırlamalar vardır. 

 - Yeni bir rolün devredilebilir kapsamlarında yalnızca bir yönetim grubu tanımlayabilirsiniz. Bu sınırlama, rol tanımlarının ve rol atamalarının kesildiği durumların sayısını azaltmak için yapılır. Bu, rol ataması olan bir abonelik veya yönetim grubu rol tanımı olmayan farklı bir üst öğeye taşındığında gerçekleşir.  
 - RBAC Veri Düzlemi eylemlerinin yönetim grubu özel rollerinde tanımlanmasına izin verilmez. Veri düzlemi kaynak sağlayıcılarını güncelleştirmek için RBAC eylemleriyle ilgili bir gecikme sorunu olduğu için bu kısıtlama uygulanır. Bu gecikme sorunu üzerinde çalışılmaktadır ve bu eylemler herhangi bir riski azaltmak için rol tanımıdevre dışı bırakılır.
 - Azure Kaynak Yöneticisi, rol tanımının devredilebilir kapsamında yönetim grubunun varlığını doğrulamaz. Bir yazım hatası veya yanlış bir yönetim grubu kimliği listelenirse, rol tanımı yine de oluşturulur.  

## <a name="moving-management-groups-and-subscriptions"></a>Yönetim gruplarını ve abonelikleri taşıma 

Bir yönetim grubunun veya aboneliğin başka bir yönetim grubunun çocuğu olması için üç kuralın doğru olarak değerlendirilmesi gerekir.

Taşıma eylemini yapıyorsanız, şunları yapmanız gerekir: 

- Yönetim grubu yazma ve Rol Atama çocuk abonelik veya yönetim grubu üzerinde izinleri yazmak.
  - Yerleşik rol örneği **Sahibi**
- Yönetim grubu hedef üst yönetim grubuna erişim yazar.
  - Yerleşik rol örneği: **Sahibi**, **Katılımcısı**, **Yönetim Grubu Katılımcısı**
- Yönetim grubu, varolan üst yönetim grubuna erişim yazar.
  - Yerleşik rol örneği: **Sahibi**, **Katılımcısı**, **Yönetim Grubu Katılımcısı**

**Özel Durum**: Hedef veya varolan üst yönetim grubu Root yönetim grubuysa, izin gereksinimleri geçerli değildir. Root yönetim grubu tüm yeni yönetim grupları ve abonelikler için varsayılan iniş noktası olduğundan, bir öğeyi taşımak için izinlere ihtiyacınız yoktur.

Abonelikteki Sahip rolü geçerli yönetim grubundan devralılırsa, hareket hedefleriniz sınırlıdır. Aboneliği yalnızca Sahip rolüne sahip olduğunuz başka bir yönetim grubuna taşıyabilirsiniz. Aboneliğin sahipliğini kaybedeceğiniz için katılımcı olduğunuz bir yönetim grubuna taşıyamazsınız. Abonelik için Doğrudan Sahip rolüne atandıysanız (yönetim grubundan devralınmadı), bu görevi katkıda bulunan olduğunuz herhangi bir yönetim grubuna taşıyabilirsiniz.

## <a name="audit-management-groups-using-activity-logs"></a>Etkinlik günlüklerini kullanarak yönetim gruplarını denetleme

Yönetim grupları [Azure Etkinlik Günlüğü](../../azure-monitor/platform/platform-logs-overview.md)'nde desteklenir. Yönetim grubunda gerçekleşen tüm olayları, diğer Azure kaynaklarıyla aynı merkezi konumda arayabilirsiniz. Örneğin, belirli bir yönetim grubunda yapılan tüm Rol Atamalarını veya İlke Ataması değişikliklerini görebilirsiniz.

:::image type="content" source="./media/al-mg.png" alt-text="Yönetim Gruplarıyla Etkinlik Günlükleri" border="false":::

Azure portalının dışında Yönetim Gruplarını sorgulamak istediğinizde, yönetim gruplarının hedef kapsamı **"/providers/Microsoft.Management/managementGroups/{yourMgID}"** gibi görünür.

## <a name="next-steps"></a>Sonraki adımlar

Yönetim grupları hakkında daha fazla bilgi almak için bkz.:

- [Azure kaynaklarını düzenlemek için yönetim grupları oluşturma](./create.md)
- [Yönetim gruplarınızı değiştirme, silme veya yönetme](./manage.md)
- [Azure PowerShell Kaynak Modülünde yönetim gruplarını gözden geçirme](/powershell/module/az.resources#resources)
- [REST API'de yönetim gruplarını gözden geçirme](/rest/api/resources/managementgroups)
- [Azure CLI'de yönetim gruplarını gözden geçirme](/cli/azure/account/management-group)
