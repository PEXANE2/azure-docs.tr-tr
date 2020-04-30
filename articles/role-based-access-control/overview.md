---
title: Azure kaynakları için rol tabanlı erişim denetimi (RBAC) nedir? | Microsoft Belgeleri
description: Azure kaynakları için rol tabanlı erişim denetimine (RBAC) genel bir bakış alın. Azure kaynaklarına erişimi denetlemek için rol atamalarını kullanın.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8f8aadeb-45c9-4d0e-af87-f1f79373e039
ms.service: role-based-access-control
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/17/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 4a1db200b88d0eabde967961d956cdd2854e828d
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81686481"
---
# <a name="what-is-role-based-access-control-rbac-for-azure-resources"></a>Azure kaynakları için rol tabanlı erişim denetimi (RBAC) nedir?

Bulut kaynakları için erişim yönetimi, bulutu kullanan tüm kuruluşlar için kritik öneme sahip bir işlevdir. Rol tabanlı erişim denetimi (RBAC) Azure kaynaklarına erişebilen kişileri, bu kişilerin bu kaynaklarla yapabileceklerini ve erişebildikleri alanları yönetmenize yardımcı olur.

RBAC, Azure kaynakları üzerinde ayrıntılı erişim yönetimi sağlayan [Azure Resource Manager](../azure-resource-manager/management/overview.md) yerleşik bir yetkilendirme sistemidir.

## <a name="what-can-i-do-with-rbac"></a>RBAC ile ne yapabilirim?

Aşağıda RBAC ile gerçekleştirebileceğiniz işlemlere örnekler verilmiştir:

- Bir kullanıcıya abonelikteki sanal makineleri yönetme, başka bir kullanıcıya ise sanal ağları yönetme izni verme
- Bir DBA grubuna abonelikteki SQL veritabanlarını yönetme izni verme
- Bir kullanıcının sanal makineler, web siteleri ve alt ağlar gibi bir kaynak grubundaki tüm kaynakları yönetmesine izin verme
- Bir uygulamaya bir kaynak grubundaki tüm kaynaklara erişim izni verme

## <a name="how-rbac-works"></a>RBAC nasıl çalışır?

RBAC özelliğini kullanarak kaynaklara erişimi denetlemek için rol ataması oluşturmanız gerekir. Bu, anlaşılması için önemli bir kavramdır. Bu, izinlerin nasıl zorlandığını öğrenin. Rol ataması üç öğeden oluşur: güvenlik sorumlusu, rol tanımı ve kapsam.

### <a name="security-principal"></a>Güvenlik sorumlusu

*Güvenlik sorumlusu* , Azure kaynaklarına erişim isteyen bir Kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimliği temsil eden bir nesnedir.

![Rol ataması için güvenlik sorumlusu](./media/overview/rbac-security-principal.png)

- Kullanıcı: Azure Active Directory'de bir profile sahip olan kişidir. Diğer kiracılardaki kullanıcılara da rol atayabilirsiniz. Diğer kuruluşlardaki kullanıcılar hakkında bilgi almak için bkz. [Azure Active Directory B2B](../active-directory/b2b/what-is-b2b.md).
- Grup: Azure Active Directory'de oluşturulan kullanıcı kümesidir. Bir gruba rol atadığınızda ilgili gruptaki tüm kullanıcılar o role sahip olur. 
- Hizmet sorumlusu: Uygulamalar veya hizmetler tarafından belirli Azure kaynaklarına erişmek için kullanılan güvenlik kimliğidir. Bunu bir uygulamanın *kullanıcı kimliği* (kullanıcı adı ve parola veya sertifika) olarak düşünebilirsiniz.
- Yönetilen kimlik-Azure tarafından otomatik olarak yönetilen Azure Active Directory bir kimlik. Azure hizmetlerinde kimlik doğrulaması için kimlik bilgilerini yönetmek üzere bulut uygulamaları geliştirirken genellikle [Yönetilen kimlikler](../active-directory/managed-identities-azure-resources/overview.md) kullanırsınız.

### <a name="role-definition"></a>Rol tanımı

*Rol tanımı*, izinlerden oluşan bir koleksiyondur. Genellikle bir *rol*olarak adlandırılır. Rol tanımı; okuma, yazma ve silme gibi gerçekleştirilebilecek işlemleri listeler. Roller sahip gibi üst düzey veya sanal makine okuyucusu gibi sınırlı olabilir.

![Rol ataması için rol tanımı](./media/overview/rbac-role-definition.png)

Azure'da kullanabileceğiniz birçok [yerleşik rol](built-in-roles.md) bulunur. Dört temel yerleşik rol aşağıda listelenmiştir. İlk üçü tüm kaynak türleri için geçerlidir.

- [Sahip](built-in-roles.md#owner): Erişimi başkalarına verme hakkı dahil olmak üzere tüm kaynaklara tam erişime sahiptir.
- [Katkıda bulunan](built-in-roles.md#contributor) -tüm Azure Kaynak türlerini oluşturabilir ve yönetebilir, ancak diğer kullanıcılara erişim izni veremez.
- [Okuyucu](built-in-roles.md#reader): Var olan Azure kaynaklarını görüntüleyebilirsiniz.
- [Kullanıcı Erişimi Yöneticisi](built-in-roles.md#user-access-administrator): Azure kaynaklarına kullanıcı erişimini yönetmenizi sağlar.

Yerleşik rollerin diğerleri belirli Azure kaynakları için yönetim özellikleri sunar. Örneğin [Sanal Makine Katılımcısı](built-in-roles.md#virtual-machine-contributor) rolü, kullanıcının sanal makine oluşturmasını ve yönetmesini sağlar. Yerleşik roller kuruluşunuzun belirli ihtiyaçlarını karşılamıyorsa, [Azure kaynakları için kendi özel rollerinizi](custom-roles.md)de oluşturabilirsiniz.

Azure 'da bir nesne içindeki verilere erişim izni vermenizi sağlayan veri işlemleri vardır. Örneğin kullanıcının bir depolama hesabında verileri okuma erişimi varsa bu kullanıcı ilgili depolama hesabındaki blobları veya iletileri okuyabilir. Daha fazla bilgi için bkz. [Azure kaynakları için rol tanımlarını anlama](role-definitions.md).

### <a name="scope"></a>Kapsam

*Kapsam* , erişimin uygulandığı kaynak kümesidir. Bir rol atadığınızda kapsam tanımlayarak izin verilen eylemleri sınırlandırabilirsiniz. Bu özellik bir kullanıcıyı yalnızca bir kaynak grubu için [Web Sitesi Katılımcısı](built-in-roles.md#website-contributor) yapmak istediğiniz durumlarda kullanışlıdır.

Azure'da [yönetim grubu](../governance/management-groups/overview.md), abonelik, kaynak grubu veya kaynak olmak üzere birden fazla seviyede kapsam belirtebilirsiniz. Kapsamlar üst-alt öğe ilişkisiyle yapılandırılmıştır.

![Rol ataması kapsamı](./media/overview/rbac-scope.png)

Üst kapsamda verdiğiniz erişim izinleri alt kapsamlar tarafından devralınır. Örneğin:

- [Sahip](built-in-roles.md#owner) rolünü yönetim grubu kapsamında bir kullanıcıya atarsanız bu kullanıcı yönetim grubundaki tüm aboneliklerde bulunan nesneleri yönetebilir.
- [Okuyucu](built-in-roles.md#reader) rolünü bir gruba abonelik kapsamında atadığınızda grubun üyeleri aboneliğin içindeki tüm kaynak gruplarını ve kaynakları görüntüleyebilir.
- [Katkıda bulunan](built-in-roles.md#contributor) rolünü bir uygulamaya kaynak grubu kapsamında atadığınızda ilgili grup bu kaynak grubundaki her türlü kaynağı yönetebilir ancak abonelikteki diğer kaynak gruplarını yönetemez.

### <a name="role-assignments"></a>Rol atamaları

*Rol ataması* , erişim vermek amacıyla belirli bir kapsamdaki Kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimliğe bir rol tanımı ekleme işlemidir. Erişim, rol ataması oluşturularak sağlanır ve rol ataması kaldırıldığında iptal edilir.

Aşağıdaki diyagramda rol ataması örneği gösterilmektedir. Bu örnekte Marketing grubuna pharma-sales kaynak grubu için [Katkıda bulunan](built-in-roles.md#contributor) rolü atanmıştır. Bu da Marketing grubundaki kullanıcıların pharma-sales kaynak grubunda tüm Azure kaynaklarını oluşturma veya yönetme işlemlerini gerçekleştirebileceği anlamına gelir. Başka bir rol atamasına sahip olmayan Marketing kullanıcılarının pharma-sales kaynak grubu dışındaki kaynaklara erişimi yoktur.

![Erişim denetimi için rol ataması](./media/overview/rbac-overview.png)

Rol atamalarını oluşturmak için Azure portal, Azure CLI, Azure PowerShell, Azure SDK'ları veya REST API'lerini kullanabilirsiniz. Her bir abonelikte en fazla **2000** rol ataması ve her bir yönetim grubunda **500** rol ataması olabilir. Rol ataması oluşturmak ve kaldırmak için `Microsoft.Authorization/roleAssignments/*` iznine sahip olmanız gerekir. Bu izin, [Sahip](built-in-roles.md#owner) veya [Kullanıcı Erişimi Yöneticisi](built-in-roles.md#user-access-administrator) rolleriyle verilir.

## <a name="multiple-role-assignments"></a>Çoklu rol atamaları

Birden çok çakışan rol atamalarınız varsa ne olur? RBAC bir eklenebilir modeldir, bu nedenle etkili izinleriniz rol atamalarınızın toplamıdır. Bir kullanıcıya, abonelik kapsamında katkıda bulunan rolü ve bir kaynak grubundaki okuyucu rolü verildiğinde aşağıdaki örneği göz önünde bulundurun. Katkıda bulunan izinlerinin ve okuyucu izinlerinin toplamı, kaynak grubu için katkıda bulunan roldür. Bu nedenle, bu durumda okuyucu rolü atamasının hiçbir etkisi yoktur.

![Çoklu rol atamaları](./media/overview/rbac-multiple-roles.png)

## <a name="deny-assignments"></a>Reddetme atamaları

Daha önce, RBAC reddetme seçeneği olmayan yalnızca izin verme modeliydi ama şimdi RBAC sınırlı bir yoldan reddetme atamalarını da destekliyor. Bir rol atamasına benzer şekilde, *reddetme atama* , erişimi reddetmek amacıyla belirli bir kapsamdaki bir Kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimliğe reddetme eylemleri kümesi iliştirir. Bir rol ataması *izin*verilen bir dizi eylemi tanımlar, ancak reddetme ataması *izin verilmeyen*bir dizi eylemi tanımlar. Başka bir deyişle, reddetme atamaları kullanıcıların belirtilen eylemleri gerçekleştirmesini (rol ataması izin vermiş olsa bile) engeller. Reddetme atamaları rol atamalarından daha önceliklidir. Daha fazla bilgi için bkz. [Azure kaynakları için reddetme atamalarını anlama](deny-assignments.md).

## <a name="how-rbac-determines-if-a-user-has-access-to-a-resource"></a>RBAC kullanıcının bir kaynağa erişimi olduğunu nasıl saptar?

Aşağıdakiler, RBAC'nin yönetim düzleminde bir kaynağa erişiminizin olup olmadığını saptamak için kullandığı üst düzey adımlardır. Bunları anlamak, bir erişim sorununu gidermeye çalışırken işinize yarayabilir.

1. Kullanıcı (veya hizmet sorumlusu) Azure Resource Manager için bir belirteç alır.

    Belirteç kullanıcının grup üyeliğini (geçişli grup üyelikleri de dahil) içerir.

1. Kullanıcı, belirteci de ekleyerek Azure Resource Manager'a bir REST API çağrısı yapar.

1. Azure Resource Manager, üzerinde eylem gerçekleştirilen kaynak için geçerli olan tüm rol atamalarını ve reddetme atamalarını alır.

1. Azure Resource Manager, rol atamalarını bu kullanıcıya veya onun grubuna uygulananlarla daraltır ve kullanıcının bu kaynak için hangi role sahip olduğunu saptar.

1. Azure Resource Manager, API çağrısındaki eylemin bu kaynak için kullanıcının sahip olduğu rollere eklenip eklenmediğini saptar.

1. Kullanıcının istenen kapsamda eyleme sahip bir rolü yoksa erişime izin verilmez. Aksi takdirde, Azure Resource Manager bir reddetme atamasının geçerli olup olmadığını denetler.

1. Reddetme ataması geçerliyse, erişim engellenir. Aksi takdirde erişim izni verilir.

## <a name="license-requirements"></a>Lisans gereksinimleri

[!INCLUDE [Azure AD free license](../../includes/active-directory-free-license.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı başlangıç: Azure portal kullanarak bir kullanıcının Azure kaynaklarına erişimi görüntüleme](check-access.md)
- [RBAC ve Azure portalını kullanarak Azure kaynaklarına erişimi yönetme](role-assignments-portal.md)
- [Azure'daki farklı rolleri anlama](rbac-and-directory-admin-roles.md)
- [Bulut benimseme çerçevesi: Azure 'da kaynak erişim yönetimi](/azure/cloud-adoption-framework/govern/resource-consistency/resource-access-management)
