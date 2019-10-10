---
title: Kaynakları yönetim gruplarıyla düzenleme-Azure Idare
description: Yönetim grupları, izinlerinin nasıl çalıştığı ve nasıl kullanılacağı hakkında bilgi edinin.
author: rthorn17
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: governance
ms.date: 04/22/2019
ms.author: rithorn
ms.topic: overview
ms.openlocfilehash: aa4c5a7bfe7333c02fe79612b828df5680b83b26
ms.sourcegitcommit: 1c2659ab26619658799442a6e7604f3c66307a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72254723"
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Azure Yönetim gruplarıyla kaynaklarınızı düzenleyin

Kuruluşunuzun birçok aboneliği varsa, bu abonelikler için erişimi, ilkeleri ve uyumluluğu verimli bir şekilde yönetmeniz için bir yol gerekebilir. Azure Yönetim grupları, aboneliklerin üzerinde bir kapsam düzeyi sağlar. Abonelikleri "Yönetim grupları" adlı kapsayıcılara düzenler ve yönetim gruplarına idare koşullarınızı uygulayın. Bir yönetim grubu içindeki tüm abonelikler, yönetim grubuna uygulanan koşulları otomatik olarak devralınır. Yönetim grupları, sahip olabileceğiniz Abonelik türleri ne olduğuna bakılmaksızın büyük ölçekte kurumsal düzeyde yönetim sağlar. Tek bir yönetim grubu içindeki tüm abonelikler aynı Azure Active Directory kiracıya güvenmelidir.

Örneğin, sanal makine (VM) oluşturma için kullanılabilir bölgeleri sınırlayan bir yönetim grubuna ilkeler uygulayabilirsiniz. Bu ilke, bu yönetim grubu altındaki tüm yönetim gruplarına, aboneliklere ve kaynaklara yalnızca söz konusu bölgede sanal makinelerin oluşturulmasını sağlayarak uygulanır.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>Yönetim grupları ve abonelikler hiyerarşisi

Kaynaklarınızı birleştirilmiş ilke ve erişim yönetimi için bir hiyerarşiye göre düzenlemek üzere yönetim gruplarının ve aboneliklerinin esnek bir yapısını oluşturabilirsiniz. Aşağıdaki diyagramda yönetim grupları kullanılarak idare için hiyerarşi oluşturma örneği gösterilmektedir.

![Yönetim grubu hiyerarşisi ağacı örneği](./media/tree.png)

Örneğin, VM konumlarını "üretim" adlı gruptaki ABD Batı bölgesiyle sınırlayan bir ilke uygulayan bir hiyerarşi oluşturabilirsiniz. Bu ilke, bu yönetim grubu kapsamındaki her iki EA aboneliğine de devralınır ve bu abonelikler altındaki tüm sanal makinelere uygulanır. Bu güvenlik ilkesi, geliştirilmiş idare için izin veren kaynak veya abonelik sahibi tarafından değiştirilemez.

Yönetim gruplarını kullanacağınız başka bir senaryo ise birden çok aboneliğe Kullanıcı erişimi sağlamaktır. Bu yönetim grubunun altına birden çok abonelik taşıyarak, yönetim grubunda bir [rol tabanlı erişim denetimi](../../role-based-access-control/overview.md) (RBAC) ataması oluşturabilirsiniz ve bu da tüm Aboneliklerle ilgili erişimi devralacak.
Yönetim grubundaki bir atama, kullanıcıların farklı abonelikler üzerinde RBAC sağlamak yerine ihtiyaç duydukları her şeye erişmesini sağlayabilir.

### <a name="important-facts-about-management-groups"></a>Yönetim grupları hakkında önemli olgular

- 10.000 yönetim grubu, tek bir dizinde desteklenebilir.
- Bir yönetim grubu ağacı, en fazla altı derinlik düzeyi destekleyebilir.
  - Bu sınır, kök düzeyi veya abonelik düzeyini içermez.
- Her yönetim grubu ve abonelik yalnızca bir üst öğeyi destekleyebilir.
- Her yönetim grubunun birçok alt öğesi olabilir.
- Tüm abonelikler ve yönetim grupları her bir dizinde tek bir hiyerarşi içindedir. [Kök yönetim grubuyla Ilgili önemli olgulara](#important-facts-about-the-root-management-group)bakın.

## <a name="root-management-group-for-each-directory"></a>Her dizin için kök yönetim grubu

Her dizine, "kök" yönetim grubu adlı tek bir üst düzey yönetim grubu verilir.
Bu kök yönetim grubu, tüm yönetim gruplarının ve aboneliklerinin kendisine katlaması için hiyerarşide yerleşik olarak bulunur. Bu kök yönetim grubu, genel ilkelerin ve RBAC atamalarının dizin düzeyinde uygulanmasını sağlar. [Azure AD Genel Yöneticisi 'nin kendilerini](../../role-based-access-control/elevate-access-global-admin.md) başlangıçta bu kök grubunun Kullanıcı erişimi Yöneticisi rolüne yükseltmesine ihtiyacı vardır. Erişim yükseltildikten sonra, yönetici, hiyerarşiyi yönetmek için diğer dizin kullanıcılarına veya gruplarına herhangi bir RBAC rolü atayabilir. Yönetici olarak, kök yönetim grubunun sahibi olarak kendi hesabınızı atayabilirsiniz.

### <a name="important-facts-about-the-root-management-group"></a>Kök yönetim grubuyla ilgili önemli olgular

- Varsayılan olarak, kök yönetim grubunun görünen adı **kiracı kök grubudur**. KIMLIK Azure Active Directory KIMLIĞIDIR.
- Görünen adı değiştirmek için hesabınıza kök yönetim grubunda sahip veya katkıda bulunan rolü atanmalıdır. Adı değiştirme adımları için bkz. [bir yönetim grubunun adını değiştirme](manage.md#change-the-name-of-a-management-group).
- Kök yönetim grubu, diğer yönetim gruplarından farklı olarak taşınamaz veya silinemez.  
- Tüm abonelikler ve yönetim grupları, dizin içindeki bir kök yönetim grubuna katlayın.
  - Dizindeki tüm kaynaklar genel yönetim için kök yönetim grubuna katlayın.
  - Yeni abonelikler oluşturulduğunda otomatik olarak kök yönetim grubuna varsayılan olarak ayarlanır.
- Tüm Azure müşterileri kök yönetim grubunu görebilir, ancak tüm müşterilerin bu kök yönetim grubunu yönetme erişimi yoktur.
  - Bir aboneliğe erişimi olan herkes, aboneliğin hiyerarşide bulunduğu yerin bağlamını görebilir.  
  - Kök yönetim grubuna hiçbir birinin varsayılan erişimi verilmez. Azure AD Genel yöneticileri, erişimi kazanmak için kendilerini yükseltebilirler.  Kök yönetim grubuna erişim izni olduktan sonra, genel yöneticiler onu yönetmek için diğer kullanıcılara herhangi bir RBAC rolü atayabilir.  

> [!IMPORTANT]
> Kök yönetim grubundaki Kullanıcı erişimi veya ilke atamasının herhangi bir ataması **, dizindeki tüm kaynaklar için geçerlidir**.
> Bu nedenle, tüm müşteriler bu kapsamda tanımlı öğe gereksinimini değerlendirmelidir.
> Kullanıcı erişimi ve ilke atamalarının yalnızca bu kapsamda "olması gerekir".  

## <a name="initial-setup-of-management-groups"></a>Yönetim gruplarının ilk kurulumu

Herhangi bir kullanıcı yönetim gruplarını kullanmaya başladığında, gerçekleşen bir başlangıç kurulum işlemi vardır. İlk adım, kök yönetim grubu dizinde oluşturulur. Bu Grup oluşturulduktan sonra, dizinde bulunan tüm mevcut abonelikler kök yönetim grubunun alt öğeleri haline getirilir. Bu işlemin nedeni, bir dizin içinde yalnızca bir yönetim grubu hiyerarşisinin olduğundan emin olmak içindir. Dizindeki tek hiyerarşi, yönetim müşterilerinin, dizindeki diğer müşterilerin atlayamayacağını genel erişim ve ilkeler uygulamasına olanak tanır. Köke atanan herhangi bir şey, tüm yönetim gruplarını, abonelikleri, kaynak grupları ve bu Azure AD kiracısındaki kaynakları içeren hiyerarşinin tamamına uygulanır.

## <a name="trouble-seeing-all-subscriptions"></a>Tüm abonelikleri görme sorunu

Önizlemede Haziran 25 2018 ' den önceki yönetim gruplarını kullanmaya başlayan birkaç dizin, tüm aboneliklerin hiyerarşide olmadığı bir sorunu görebilir. Dizindeki kök yönetim grubunda bir rol veya ilke atamasının yapılmasının ardından hiyerarşideki tüm aboneliklerde yer alan işlem konur. 

### <a name="how-to-resolve-the-issue"></a>Sorunu çözme

Bu sorunu çözmek için yapabileceğiniz iki seçenek vardır.

1. Kök yönetim grubundan tüm rol ve Ilke atamalarını kaldır
   1. Kök yönetim grubundan herhangi bir ilke ve rol ataması kaldırılarak, hizmet tüm abonelikleri sonraki gece ömrü boyunca hiyerarşiye doldurur.  Bu işlem, kiracı aboneliklerine yanlışlıkla erişim verilmesi veya ilke ataması yoktur.
   1. Hizmetlerinizi etkilemeden bu işlemi yapmanın en iyi yolu, rol veya ilke atamalarını kök yönetim grubunun altına tek bir düzeye uygulayacaksınız. Ardından, kök kapsamdan tüm atamaları kaldırabilirsiniz.
1. Geri doldurma işlemini başlatmak için API 'YI doğrudan çağırın
   1. Dizindeki herhangi bir müşteri, *Tenantbackfillstatusrequest* veya *starttenantbackfillrequest* API 'lerini çağırabilir. StartTenantBackfillRequest API 'SI çağrıldığında, tüm abonelikleri hiyerarşiye taşıma ilk kurulum işlemini devre dışı bırakır. Bu işlem Ayrıca, tüm yeni aboneliğin uygulanmasını kök yönetim grubunun bir alt öğesi olarak başlatır. Bu işlem, kök düzeyindeki hiçbir atama değiştirilmeden yapılabilir. API 'yi çağırarak, köke yönelik herhangi bir ilkenin veya erişim atamasının tüm aboneliklere uygulanabileceğini belirten bir sorun olduğunu söyleiyoruz.

Bu geri doldurma işleminde sorularınız varsa, managementgroups@microsoft.com ' a başvurun.  
  
## <a name="management-group-access"></a>Yönetim grubu erişimi

Azure Yönetim grupları tüm kaynak erişimleri ve rol tanımları için [Azure rol tabanlı Access Control (RBAC)](../../role-based-access-control/overview.md) destekler.
Bu izinler, hiyerarşide bulunan alt kaynaklara devralınır. Herhangi bir yerleşik RBAC rolü, hiyerarşiyi kaynaklara devralacak bir yönetim grubuna atanabilir.
Örneğin, RBAC rolü sanal makine katılımcısı bir yönetim grubuna atanabilir. Bu rolün yönetim grubu üzerinde hiçbir eylemi yoktur, ancak bu yönetim grubu altındaki tüm VM 'lere devralınır.

Aşağıdaki grafikte, rol listesi ve yönetim gruplarında desteklenen eylemler gösterilmektedir.

| RBAC rol adı             | Create | Rename | Taşı * * | Sil | Erişim ata | Ilke ata | Oku  |
|:-------------------------- |:------:|:------:|:------:|:------:|:-------------:| :------------:|:-----:|
|Sahip                       | X      | X      | X      | X      | X             | X             | X     |
|Mcý                 | X      | X      | X      | X      |               |               | X     |
|MG katkıda bulunan *             | X      | X      | X      | X      |               |               | X     |
|Okuyucu                      |        |        |        |        |               |               | X     |
|MG okuyucusu *                  |        |        |        |        |               |               | X     |
|Kaynak Ilkesi Katılımcısı |        |        |        |        |               | X             |       |
|Kullanıcı erişimi Yöneticisi   |        |        |        |        | X             | X             |       |

*: MG katkıda bulunan ve MG Reader yalnızca kullanıcıların yönetim grubu kapsamında bu eylemleri yapmasına izin verir.  
\* *: Kök yönetim grubundaki rol atamaları, bir aboneliği veya yönetim grubunu bu gruba taşımak için gerekli değildir.  Hiyerarşi içinde öğeleri taşıma hakkında daha fazla bilgi için bkz. [Yönetim gruplarıyla kaynaklarınızı yönetme](manage.md) .

### <a name="custom-rbac-role-definition-and-assignment"></a>Özel RBAC rol tanımı ve ataması

Özel RBAC rolleri Şu anda yönetim gruplarında desteklenmiyor. Bu öğenin durumunu görüntülemek için [Yönetim grubu geri bildirim forumuna](https://aka.ms/mgfeedback) bakın.

## <a name="audit-management-groups-using-activity-logs"></a>Etkinlik günlüklerini kullanarak Yönetim gruplarını denetleme

Yönetim grupları [Azure etkinlik günlüğü](../../azure-monitor/platform/activity-logs-overview.md)'nde desteklenir. Diğer Azure kaynaklarıyla aynı merkezi konumdaki bir yönetim grubunda gerçekleşen tüm olayları arayabilirsiniz.  Örneğin, belirli bir yönetim grubunda yapılan tüm rol atamalarını veya Ilke ataması değişikliklerini görebilirsiniz.

![Yönetim Grupları olan etkinlik günlükleri](media/al-mg.png)

Azure portal dışında Yönetim Grupları sorgulamak ararken, yönetim grupları için hedef kapsam **"/Providers/Microsoft.Management/managementGroups/{yourMgID}"** gibi görünür.

## <a name="next-steps"></a>Sonraki adımlar

Yönetim grupları hakkında daha fazla bilgi için bkz.

- [Azure kaynaklarını düzenlemek için yönetim grupları oluşturma](create.md)
- [Yönetim gruplarınızı değiştirme, silme veya yönetme](manage.md)
- [Azure PowerShell Resources modülündeki Yönetim gruplarını gözden geçirme](/powershell/module/az.resources#resources)
- [REST API içindeki yönetim gruplarını gözden geçirme](/rest/api/resources/managementgroups)
- [Azure CLı 'de Yönetim gruplarını gözden geçirme](/cli/azure/account/management-group)
