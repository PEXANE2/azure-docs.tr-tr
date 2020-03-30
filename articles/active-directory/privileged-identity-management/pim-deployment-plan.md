---
title: Ayrıcalıklı Kimlik Yönetimi (PIM) Dağıtma - Azure AD | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'nin (PIM) dağıtımının nasıl planlanır olduğunu açıklar.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 02/04/2020
ms.author: curtand
ms.custom: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: b8c77b3454026aa309d979bd938674e7c3ae7b6a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77026005"
---
# <a name="deploy-azure-ad-privileged-identity-management-pim"></a>Azure AD Ayrıcalıklı Kimlik Yönetimi (PIM) dağıtma

Bu adım adım kılavuz, Azure Etkin Dizin (Azure AD) kuruluşunuzdaki Ayrıcalıklı Kimlik Yönetimi (PIM) dağıtımının nasıl planlanır olduğunu açıklar.

> [!TIP]
> Bu makale boyunca, olarak işaretlenmiş öğeleri göreceksiniz:
> 
> :heavy_check_mark: **Microsoft önerir**
> 
> Bunlar genel önerilerdir ve yalnızca belirli kuruluş gereksinimleriniz için geçerliyse uygulamanız gerekir.

## <a name="learn-about-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi hakkında bilgi edinin

Azure AD Ayrıcalıklı Kimlik Yönetimi, Azure AD, Azure kaynakları ve diğer Microsoft Çevrimiçi Hizmetleri'nde ayrıcalıklı yönetim rollerini yönetmenize yardımcı olur. Ayrıcalıklı kimliklerin atandığı ve unutulduğu bir dünyada, Ayrıcalıklı Kimlik Yönetimi, kötü amaçlı kişileri tanımlayabilmeniz, ortaya çıkarabilmeniz ve önlemek için tam zamanında erişim, onay iş akışları isteği ve tam entegre erişim incelemeleri gibi çözümler sunar gerçek zamanlı olarak ayrıcalıklı rollerin faaliyetleri. Kuruluşunuz boyunca ayrıcalıklı rollerinizi yönetmek için Ayrıcalıklı Kimlik Yönetimi'ni dağıtmak, ayrıcalıklı rollerinizin faaliyetleri hakkında değerli öngörüler ortaya çıkarırken riski büyük ölçüde azaltır.

### <a name="business-value-of-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetiminin Ticari Değeri

**Riski yönet** - En az ayrıcalık [erişimi](/windows-server/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models) ve tam zamanında erişim ilkesini uygulayarak kuruluşunuzun güvenliğini sağla. Kullanıcıların ayrıcalıklı rollere kalıcı atama sayısını en aza indirerek ve yüksekliğe yönelik onayları ve MFA'yı uygulayarak, kuruluşunuzdaki ayrıcalıklı erişimle ilgili güvenlik risklerini büyük ölçüde azaltabilirsiniz. En az ayrıcalık ve tam zamanında erişim uygulamak, ayrıcalıklı rollere erişim geçmişini görüntülemenize ve güvenlik sorunlarını olduğu gibi izlemenize de olanak sağlar.

**Adres uyumluluğu ve yönetişim** - Ayrıcalıklı Kimlik Yönetiminin dağıtılması, devam eden kimlik yönetimi için bir ortam oluşturur. İmtiyazlı kimliklerin tam zamanında yükselmesi, Ayrıcalıklı Kimlik Yönetimi'nin kuruluşunuzdaki ayrıcalıklı erişim etkinliklerini izlemesi için bir yol sağlar. Ayrıca, kuruluşunuz daki kalıcı ve uygun rollerin tüm atamaları için bildirimleri görüntüleyebilir ve alabilirsiniz. Erişim incelemesi sayesinde, gereksiz ayrıcalıklı kimlikleri düzenli olarak denetleyebilir ve kaldırabilir ve kuruluşunuzun en sıkı kimlik, erişim ve güvenlik standartlarına uygun olduğundan emin olabilirsiniz.

**Maliyetleri azaltın** - Ayrıcalıklı Kimlik Yönetimini doğru bir şekilde dağıtarak verimsizlikleri, insan hatalarını ve güvenlik sorunlarını ortadan kaldırarak maliyetleri azaltın. Net sonuç, pahalı ve telafisi zor olan ayrıcalıklı kimliklerle ilişkili siber suçların azaltılmasıdır. Ayrıcalıklı Kimlik Yönetimi, mevzuat ve standartlara uyma konusunda kuruluşunuzun erişim bilgilerini denetlemeyle ilişkili maliyeti azaltmasına da yardımcı olur.

Daha fazla bilgi için Azure [AD Ayrıcalıklı Kimlik Yönetimi nedir?](pim-configure.md)

### <a name="licensing-requirements"></a>Lisanslama gereksinimleri

Ayrıcalıklı Kimlik Yönetimi'ni kullanmak için dizininizin aşağıdaki ücretli veya deneme lisanslarından birine sahip olması gerekir:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5
- Microsoft 365 M5

Daha fazla bilgi için, [Ayrıcalıklı Kimlik Yönetimi'ni kullanmak için Lisans gereksinimlerine](subscription-requirements.md)bakın.

### <a name="key-terminology"></a>Anahtar terminoloji

| Terim veya kavram | Açıklama |
| --- | --- |
| Uygun | Bir kullanıcının rolü kullanmak için bir veya daha fazla eylemi gerçekleştirmesini gerektiren bir rol ataması. Bir kullanıcı bir rol için uygun hale getirilmişse, bu, ayrıcalıklı görevleri gerçekleştirmeleri gerektiğinde rolü etkinleştirebileceği anlamına gelir. Kalıcı ve uygun bir rol ataması olan birine verilen erişimde bir fark yoktur. Tek fark, bazı insanların her zaman bu erişim gerekmez. |
| etkinleştirme | Bir kullanıcının uygun olduğu bir rolü kullanmak için bir veya daha fazla eylemi gerçekleştirme işlemi. Eylemler, çok faktörlü bir kimlik doğrulama (MFA) denetimi gerçekleştirmeyi, bir iş gerekçesi sağlamayı veya atanan onaylayıcılardan onay istemeyi içerebilir. |
| tam zamanında (JIT) erişim | Kullanıcıların ayrıcalıklı görevleri gerçekleştirmek için geçici izinler aldığı ve kötü amaçlı veya yetkisiz kullanıcıların izinlerin süresi dolduktan sonra erişim elde etmesini engelleyen bir model. Erişim yalnızca kullanıcıların ihtiyacı olduğunda verilir. |
| en az ayrıcalık erişim ilkesi | Her kullanıcıya, gerçekleştirmeye yetkili oldukları görevleri gerçekleştirmek için gereken minimum ayrıcalıkların sağlandığı önerilen güvenlik uygulaması. Bu uygulama, Genel Yönetici sayısını en aza indirir ve bunun yerine belirli senaryolar için belirli yönetici rolleri kullanır. |

Daha fazla bilgi için [terminolojiye](pim-configure.md#terminology)bakın.

### <a name="high-level-overview-of-how-privileged-identity-management-works"></a>Ayrıcalıklı Kimlik Yönetimi'nin nasıl çalıştığına üst düzey genel bakış

1. Ayrıcalıklı Kimlik Yönetimi, kullanıcıların ayrıcalıklı roller için uygun olması için ayarlanır.
1. Uygun bir kullanıcı ayrıcalıklı rolünü kullanması gerektiğinde, Ayrıcalıklı Kimlik Yönetimi'ndeki rolü etkinleştirir.
1. Rol için yapılandırılan Ayrıcalıklı Kimlik Yönetimi ayarlarına bağlı olarak, kullanıcının belirli adımları tamamlaması gerekir (çok faktörlü kimlik doğrulama gerçekleştirme, onay alma veya bir neden belirtme gibi.)
1. Kullanıcı rolünü başarıyla etkinleştirdikten sonra, rolü önceden yapılandırılmış bir süre için alır.
1. Yöneticiler denetim günlüğünde tüm Ayrıcalıklı Kimlik Yönetimi etkinliklerinin geçmişini görüntüleyebilir. Ayrıca, erişim incelemeleri ve uyarıları gibi Ayrıcalıklı Kimlik Yönetimi özelliklerini kullanarak Azure REKLAM kuruluşlarını daha da güvenli hale getirebilir ve uyumluluğu karşılayabilirler.

Daha fazla bilgi için Azure [AD Ayrıcalıklı Kimlik Yönetimi nedir?](pim-configure.md)

### <a name="roles-that-can-be-managed-by-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi tarafından yönetilebilen roller

**Azure AD rolleri** – Bu rollerin tümü Azure Etkin Dizini'ndedir (Genel Yönetici, Exchange Yöneticisi ve Güvenlik Yöneticisi gibi). [Azure Etkin Dizin'inde Yönetici rol izinlerinde](../users-groups-roles/directory-assign-admin-roles.md)roller ve bunların işlevleri hakkında daha fazla bilgi edinebilirsiniz. Yöneticilerinizin hangi rolleri atayacağını belirlemekonusunda yardımcı olmak [için, göreve göre en az ayrıcalıklı rolleri](../users-groups-roles/roles-delegate-by-task.md)görün.

**Azure kaynak rolleri** – Bu roller bir Azure kaynağına, kaynak grubuna, aboneye veya yönetim grubuna bağlıdır. Ayrıcalıklı Kimlik Yönetimi, Sahibi, Kullanıcı Erişim Yöneticisi ve Katılımcı gibi yerleşik rollerin yanı sıra [özel rollere](../../role-based-access-control/custom-roles.md)de tam zamanında erişim sağlar. Azure kaynak rolleri hakkında daha fazla bilgi için [rol tabanlı erişim denetimine (RBAC)](../../role-based-access-control/overview.md)bakın.

Daha fazla bilgi için, [Ayrıcalıklı Kimlik Yönetiminde yönetemeyeceğiniz Roller'e](pim-roles.md)bakın.

## <a name="plan-your-deployment"></a>Dağıtımınızı planlama

Bu bölümde, kuruluşunuzda Ayrıcalıklı Kimlik Yönetimi'ni dağıtmadan önce yapmanız gerekenler üzerinde duruluyor. Kuruluşunuzun ayrıcalıklı kimlikleri için uyarlanmış en iyi planı oluşturmanız için size rehberlik edeceği için yönergeleri takip etmek ve bu bölümdeki kavramları anlamak önemlidir.

### <a name="identify-your-stakeholders"></a>Paydaşlarınızı belirleyin

Aşağıdaki bölüm, projede yer alan ve oturum açmanız, gözden geçirmeniz veya haberdar olmanız gereken tüm paydaşları belirlemenize yardımcı olur. Azure REKLAM rolleri için Ayrıcalıklı Kimlik Yönetimi ve Azure kaynak rolleri için Ayrıcalıklı Kimlik Yönetimi dağıtmak için ayrı tablolar içerir. Kuruluşunuz için uygun olan aşağıdaki tabloya hissedarlar ekleyin.

- SO = Bu projeyi imzalayın
- R = Bu projeyi gözden geçirin ve giriş sağlayın
- I = Bu projeden haberdar

#### <a name="stakeholders-privileged-identity-management-for-azure-ad-roles"></a>Hissedarlar: Azure REKLAM rolleri için Ayrıcalıklı Kimlik Yönetimi

| Adı | Rol | Eylem |
| --- | --- | --- |
| Ad ve e-posta | **Kimlik mimarı veya Azure Global Yöneticisi**<br/>Bu değişikliğin kuruluşunuzdaki temel kimlik yönetimi altyapısıyla nasıl uyumlu olduğunu tanımlamaktan sorumlu kimlik yönetimi ekibinden bir temsilci. | SO/R/I |
| Ad ve e-posta | **Servis sahibi / Hat yöneticisi**<br/>Bir hizmetin veya bir hizmet grubunun BT sahiplerinden bir temsilci. Onlar karar verme ve takım için Ayrıcalıklı Kimlik Yönetimi dışarı rulo yardımcı anahtardır. | SO/R/I |
| Ad ve e-posta | **Güvenlik sahibi**<br/>Planın kuruluşunuzun güvenlik gereksinimlerini karşıladığını kapatabilen güvenlik ekibinden bir temsilci. | SO/R |
| Ad ve e-posta | **BT destek yöneticisi / Yardım Masası**<br/>BT destek kuruluşundan, bu değişikliğin desteklenebilirliği konusunda yardım masası perspektifinden bilgi sağlayabilen bir temsilci. | R/I |
| Pilot kullanıcılar için ad ve e-posta | **Ayrıcalıklı rol kullanıcıları**<br/>Ayrıcalıklı kimlik yönetiminin uygulandığı kullanıcı grubu. Ayrıcalıklı Kimlik Yönetimi uygulandıktan sonra rollerini nasıl etkinleştireceklerini bilmeleri gerekir. | I |

#### <a name="stakeholders-privileged-identity-management-for-azure-resource-roles"></a>Hissedarlar: Azure kaynak rolleri için Ayrıcalıklı Kimlik Yönetimi

| Adı | Rol | Eylem |
| --- | --- | --- |
| Ad ve e-posta | **Abonelik / Kaynak sahibi**<br/>Ayrıcalıklı Kimlik Yönetimi'ni dağıtmak istediğiniz her aboneliğin veya kaynağın BT sahiplerinden bir temsilci | SO/R/I |
| Ad ve e-posta | **Güvenlik sahibi**<br/>Planın kuruluşunuzun güvenlik gereksinimlerini karşıladığını kapatabilen güvenlik ekibinden bir temsilci. | SO/R |
| Ad ve e-posta | **BT destek yöneticisi / Yardım Masası**<br/>BT destek kuruluşundan, bu değişikliğin desteklenebilirliği konusunda yardım masası perspektifinden bilgi sağlayabilen bir temsilci. | R/I |
| Pilot kullanıcılar için ad ve e-posta | **RBAC rol kullanıcıları**<br/>Ayrıcalıklı kimlik yönetiminin uygulandığı kullanıcı grubu. Ayrıcalıklı Kimlik Yönetimi uygulandıktan sonra rollerini nasıl etkinleştireceklerini bilmeleri gerekir. | I |

### <a name="enable-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimini Etkinleştirin

Planlama sürecinin bir parçası olarak, öncelikle Ayrıcalıklı Kimlik Yönetimi makalesini [kullanmaya başladığımız makaleyi](pim-getting-started.md) takip ederek Ayrıcalıklı Kimlik Yönetimi'ni kabul etmeli ve etkinleştirmelisiniz. Ayrıcalıklı Kimlik Yönetimi'ni etkinleştirmek, dağıtımınıza yardımcı olmak için özel olarak tasarlanmış bazı özelliklere erişmenizi sağlar.

Amacınız Azure kaynakları için Ayrıcalıklı Kimlik Yönetimi dağıtmaksa, [Ayrıcalıklı Kimlik Yönetimi makalesinde yönetmek için Azure kaynaklarını keşfedin'](pim-resource-roles-discover-resources.md) i takip etmelisiniz. Bu kaynakları yalnızca abonelik ve yönetim gruplarının sahipleri, Ayrıcalıklı Kimlik Yönetimi'ne ekleyebilir ve bu kaynakları kullanabilir. Gemiye bindikten sonra PIM işlevi yönetim grubu, abonelik, kaynak grubu ve kaynak dahil olmak üzere tüm düzeylerdeki sahipler için kullanılabilir. Azure kaynaklarınız için Ayrıcalıklı Kimlik Yönetimi dağıtmaya çalışan bir Global Administrator iseniz, keşif dizinindeki tüm Azure kaynaklarına erişim sağlamak için [tüm Azure aboneliklerini yönetmeye erişimi yükseltebilirsiniz.](../../role-based-access-control/elevate-access-global-admin.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json) Ancak, ayrıcalıklı kimlik yönetimi ile kaynaklarını yönetmeden önce abonelik sahiplerinizin her birinden onay almanızı tavsiye ederiz.

### <a name="enforce-principle-of-least-privilege"></a>En az ayrıcalık ilkesini uygulayın

Hem Azure REKLAM'ınız hem de Azure kaynak rolleriniz için kuruluşunuzda en az ayrıcalık ilkesini uyguladığınızdan emin olmak önemlidir.

#### <a name="azure-ad-roles"></a>Azure AD rolleri

Azure AD rolleri için, çoğu yöneticinin yalnızca bir veya iki özel yönetici rolüne ihtiyaç duyduğunda, kuruluşların Genel Yönetici rolünü önemli sayıda yöneticiye ataması yaygındır. Ayrıcalıklı rol atamaları da yönetilmeyen bırakılma eğilimindedir.

> [!NOTE]
> Rol delegasyonunda ortak tuzaklar:
>
> - Exchange'den sorumlu yöneticiye, günlük işlerini yerine getirmek için yalnızca Exchange Yöneticisi rolüne ihtiyaç duymalarına rağmen, Genel Yönetici rolü verilir.
> - Yöneticinin hem Güvenlik hem de SharePoint yönetici rollerine ihtiyacı olduğundan ve yalnızca bir rolü devretmek daha kolay olduğundan, Genel Yönetici rolü bir Office yöneticisine atanır.
> - Yöneticiye bir görevi gerçekleştirmek için uzun zaman önce bir Güvenlik Yöneticisi rolü atandı, ancak hiçbir zaman kaldırılmadı.

Azure REKLAM rolleriniz için en az ayrıcalık ilkesini uygulamak için aşağıdaki adımları izleyin.

1. [Kullanılabilir Azure AD yöneticisi rollerini](../users-groups-roles/directory-assign-admin-roles.md#available-roles)okuyup anlayarak rollerin ayrıntılılığını anlayın. Siz ve ekibiniz, belirli görevler için en az ayrıcalıklı rolü açıklayan [Azure AD'de kimlik görevine göre yönetici rollerine](../users-groups-roles/roles-delegate-by-task.md)de başvurumalısınız.

1. Kuruluşunuzda ayrıcalıklı rolleri olanları listele. Aşağıdaki gibi bir sayfaya ulaşmak için [Ayrıcalıklı Kimlik Yönetimi sihirbazını](pim-security-wizard.md#run-the-wizard) kullanabilirsiniz.

    ![Kimin ayrıcalıklı rolleri olduğunu gösteren ayrıcalıklı roller bölmesi keşfedin](./media/pim-deployment-plan/discover-privileged-roles-users.png)

1. Kuruluşunuzdaki tüm Global Yöneticiler için, bu role neden ihtiyaç duyduğunu öğrenin. Önceki belgeleri okumaya bağlı olarak, kişinin işi bir veya daha fazla parçalı yönetici rolüyle gerçekleştirilebiliyorsa, bu belgeleri Global Administrator rolünden kaldırmalı ve atamaları Azure Active Directory içinde buna göre yapmalısınız (Başvuru olarak: Microsoft'un şu anda Global Administrator rolüne sahip yalnızca 10 yöneticisi vardır. [Microsoft'un Ayrıcalıklı Kimlik Yönetimi'ni nasıl kullandığı](https://www.microsoft.com/itshowcase/Article/Content/887/Using-Azure-AD-Privileged-Identity-Management-for-elevated-access)hakkında daha fazla bilgi edinin).

1. Diğer tüm Azure AD rolleri için atama listesini gözden geçirin, role artık ihtiyacı olmayan yöneticileri belirleyin ve bunları görevlerinden kaldırın.

Son iki adımı otomatikleştirmek için, Ayrıcalıklı Kimlik Yönetimi'nde erişim değerlendirmelerini kullanabilirsiniz. Ayrıcalıklı Kimlik [Yönetimi'ndeki Azure AD rolleri için erişim incelemesi başlatma](pim-how-to-start-security-review.md)adımlarını takiben, bir veya daha fazla üyesi olan her Azure REKLAM rolü için bir erişim incelemesi ayarlayabilirsiniz.

![Azure AD rolleri için bir erişim gözden geçirme bölmesi oluşturma](./media/pim-deployment-plan/create-access-review.png)

Gözden geçirenleri **Üyelere (kendi nisül)** ayarlamanız gerekir. Bu, bu roldeki tüm üyelere erişime ihtiyaç duyup duymadıklarını onaylamaları için bir e-posta gönderir. Ayrıca, kullanıcıların role neden ihtiyaç duyduğunu belirtebilmeleri için gelişmiş ayarlarda **onay verme nedenini etkinleştir'i** de açmalısınız. Bu bilgilere dayanarak, kullanıcıları gereksiz rollerden kaldırabilir ve Genel Yöneticiler durumunda daha ayrıntılı yönetici rollerini devredebilirsiniz.

Erişim incelemeleri, insanları rollere erişimlerini gözden geçirmelerini bildirmek için e-postalara güvenir. Bağlı e-postaları olmayan ayrıcalıklı hesaplarınız varsa, bu hesaplardaki ikincil e-posta alanını dolduraldığınızdan emin olun. Daha fazla bilgi için [Azure AD'deki proxyAdres özniteliğine](https://support.microsoft.com/help/3190357/how-the-proxyaddresses-attribute-is-populated-in-azure-ad)bakın.

#### <a name="azure-resource-roles"></a>Azure kaynağı rolleri

Azure abonelikleri ve kaynakları için, her abonelik veya kaynaktaki rolleri gözden geçirmek için benzer bir Access gözden geçirme işlemi ayarlayabilirsiniz. Bu işlemin amacı, her aboneye veya kaynağa bağlı Sahip ve Kullanıcı Erişim Yöneticisi atamalarını en aza indirmek ve gereksiz atamaları kaldırmaktır. Ancak, kuruluşlar belirli rolleri (özellikle özel roller) daha iyi anladıkları için bu tür görevleri genellikle her aboneliğin veya kaynağın sahibine devreder.

Kuruluşunuzdaki Azure kaynakları için Ayrıcalıklı Kimlik Yönetimi dağıtmaya çalışan Global Administrator rolüne sahip bir BT yöneticisiyseniz, her aboneliğe erişmek [için tüm Azure aboneliklerini yönetmeye erişimi yükseltebilirsiniz.](../../role-based-access-control/elevate-access-global-admin.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json) Daha sonra her abonelik sahibini bulabilir ve gereksiz atamaları kaldırmak ve sahip rol atamasını en aza indirmek için onlarla birlikte çalışabilirsiniz.

Azure aboneliği için Sahip rolüne sahip kullanıcılar, Azure REKLAM rolleri için daha önce açıklanan işleme benzer gereksiz rol atamalarını denetlemek ve kaldırmak [için Azure kaynaklarıiçin erişim incelemelerini](pim-resource-roles-start-access-review.md) de kullanabilir.

### <a name="decide-which-role-assignments-should-be-protected-by-privileged-identity-management"></a>Hangi rol atamalarının Ayrıcalıklı Kimlik Yönetimi tarafından korunacağına karar verin

Kuruluşunuzdaki ayrıcalıklı rol atamalarını temizledikten sonra, Ayrıcalıklı Kimlik Yönetimi ile hangi rolleri koruyacağınıza karar vermeniz gerekir.

Bir rol Ayrıcalıklı Kimlik Yönetimi tarafından korunuyorsa, bu rol için atanan uygun kullanıcılar, rolün verdiği ayrıcalıkları kullanmak için yükseltmelidir. Yükseklik işlemi ayrıca onay almayı, çok faktörlü kimlik doğrulamayı gerçekleştirmeyi ve/veya neden etkinleştirdiklerini niçin sağlamayı da içerebilir. Ayrıcalıklı Kimlik Yönetimi, bildirimler ve Ayrıcalıklı Kimlik Yönetimi ve Azure AD denetim etkinlik günlükleri aracılığıyla yükseklikleri de izleyebilir.

Ayrıcalıklı Kimlik Yönetimi ile hangi rollerin korunacağını seçmek zor olabilir ve her kuruluş için farklı olacaktır. Bu bölümde, Azure AD ve Azure kaynak rolleri için en iyi uygulama önerilerimiz verilmektedir.

#### <a name="azure-ad-roles"></a>Azure AD rolleri

En çok izine sahip Azure AD rollerinin korunmasına öncelik vermek önemlidir. Tüm Ayrıcalıklı Kimlik Yönetimi müşterileri arasındaki kullanım alışkanlıklarına bağlı olarak, Ayrıcalıklı Kimlik Yönetimi tarafından yönetilen en iyi 10 Azure AD rolü şunlardır:

1. Genel yönetici
1. Güvenlik yöneticisi
1. Kullanıcı yöneticisi
1. Exchange yöneticisi
1. SharePoint yöneticisi
1. Intune yöneticisi
1. Güvenlik okuyucusu
1. Hizmet yöneticisi
1. Faturalama yöneticisi
1. Skype Kurumsal yöneticisi

> [!TIP]
> :heavy_check_mark: Microsoft, ayrıcalıklı kimlik yönetimini kullanarak tüm Global Yöneticilerinizi ve Güvenlik Yöneticilerinizi ilk adım olarak **yönetmenizi önerir,** çünkü bu yöneticiler tehlikeye atıldığında en fazla zararı verebilirler.

Kuruluşunuz için hangi verilerin ve izinlerin en hassas olduğunu göz önünde bulundurmanız önemlidir. Örnek olarak, bazı kuruluşlar verilere erişebildikleri ve/veya temel iş akışlarını değiştirebildikleri için Ayrıcalıklı Kimlik Yönetimi'ni kullanarak Power BI Administrator rollerini veya Teams Administrator rollerini korumak isteyebilirler.

Konuk kullanıcıların atandığı herhangi bir rol varsa, bunlar özellikle saldırıya karşı savunmasızdır.

> [!TIP]
> :heavy_check_mark: Microsoft, gizliliği ihlal edilen konuk kullanıcı hesaplarıyla ilişkili riskleri azaltmak için Ayrıcalıklı Kimlik Yönetimi'ni kullanarak konuk kullanıcılarla tüm rolleri yönetmenize **önerir.**

Dizin Okuyucusu, İleti Merkezi Okuyucusu ve Güvenlik Okuyucusu gibi okuyucu rolleri, yazma izni olmadığı için bazen diğer rollere göre daha az önemli olduğuna inanılmaktadır. Ancak, bu hesaplara erişim elde eden saldırganlar kişisel veriler gibi hassas verileri okuyabildiklerinden, bazı müşterilerin de bu rolleri koruduğuni gördük. Kuruluşunuzdaki okuyucu rollerinin Ayrıcalıklı Kimlik Yönetimi kullanılarak yönetilmesi gerekip gerekmediğine karar verirken bunu göz önünde bulundurmalısınız.

#### <a name="azure-resource-roles"></a>Azure kaynağı rolleri

Azure kaynağı için Ayrıcalıklı Kimlik Yönetimi kullanılarak hangi rol atamalarının yönetilmesi gerektiğine karar verirken, öncelikle kuruluşunuz için en önemli olan abonelikleri/kaynakları belirlemeniz gerekir. Bu tür aboneliklere/kaynaklara örnek olarak şunlar verilebilir:

- En hassas verileri barındıran kaynaklar
- Temel, müşteriye yönelik uygulamaların bağlı olduğu kaynaklar

Hangi aboneliklerin/kaynakların en önemli olduğuna karar vermekte sorun yaşıyorsanız, her abonelik tarafından yönetilen kaynakların listesini toplamak için kuruluşunuzdaki abonelik sahiplerine ulaşmanız gerekir. Daha sonra, kaynakları tehlikeye girmeleri durumunda (düşük, orta, yüksek) önem düzeyine göre gruplandırmak için abonelik sahipleriyle birlikte çalışmalısınız. Bu önem düzeyine göre Ayrıcalıklı Kimlik Yönetimi ile kaynakları yönetmeye öncelik vermeniz gerekir.

> [!TIP]
> :heavy_check_mark: Microsoft, hassas abonelikler/kaynaklar içindeki tüm roller için Ayrıcalıklı Kimlik Yönetimi iş akışı ayarlamak için kritik hizmetlerin abonelik/kaynak sahipleri ile çalışmanızı **önerir.**

Azure kaynakları için ayrıcalıklı kimlik yönetimi, zamana bağlı hizmet hesaplarını destekler. Hizmet hesaplarını normal bir kullanıcı hesabına nasıl davrandığınızla aynı şekilde ele almalısınız.

Kritik olmayan abonelikler/kaynaklar için, tüm roller için Ayrıcalıklı Kimlik Yönetimi'ni ayarlamanız gerekmez. Ancak, Ayrıcalıklı Kimlik Yönetimi ile Sahibi ve Kullanıcı Erişim Yöneticisi rollerini korumaya devam edebilirsiniz.

> [!TIP]
> :heavy_check_mark: Microsoft, Ayrıcalıklı Kimlik Yönetimi'ni kullanarak tüm aboneliklerin/kaynakların Sahibi rollerini ve Kullanıcı Erişim Yöneticisi rollerini yönetmenize **önerir.**

### <a name="decide-which-role-assignments-should-be-permanent-or-eligible"></a>Hangi rol atamalarının kalıcı veya uygun olacağına karar verin

Ayrıcalıklı Kimlik Yönetimi tarafından yönetilecek rollerin listesine karar verdikten sonra, kalıcı olarak etkin rolün yanında hangi kullanıcıların uygun rolü alması gerektiğine karar vermelisiniz. Kalıcı olarak etkin roller, Azure Etkin Dizini ve Azure kaynakları aracılığıyla atanan normal rollerdir, uygun roller ise yalnızca Ayrıcalıklı Kimlik Yönetimi'nde atanabilir.

> [!TIP]
> heavy_check_mark: Microsoft, hem Azure AD rolleri hem de kalıcı Global Administrator rolüne sahip olması gereken önerilen [iki kesme camlı acil durum erişim hesabı](../users-groups-roles/directory-emergency-access.md)dışındaki Azure kaynak rolleri için sürekli olarak etkin olmayan atamalarınız **olmamasını önerir.**

Sıfır ayakta yönetici önersek de, kuruluşların bunu hemen başarması bazen zordur. Bu kararı verirken göz önünde bulundurulması gerekenler şunlardır:

- Yükseklik sıklığı – Kullanıcının ayrıcalıklı atamaya yalnızca bir kez ihtiyacı varsa, kalıcı atamaya sahip olmamalıdır. Öte yandan, kullanıcı günlük iş için rol ihtiyacı ve Ayrıcalıklı Kimlik Yönetimi kullanarak büyük ölçüde verimliliklerini azaltacak, onlar kalıcı rol için kabul edilebilir.
- Kuruluşunuza özgü davalar – Uygun rol verilen kişi çok uzak bir ekipten veya yüksek rütbeli bir yöneticiden, yükselme sürecini iletişim kurmanın ve uygulamanın zor olduğu noktaya kadar, kalıcı rol için düşünülebilir.

> [!TIP]
> :heavy_check_mark: **Microsoft,** kalıcı rol atamaları olan kullanıcılar için yinelenen erişim incelemeleri ayarlamanızı önerir (varsa). Bu dağıtım planının son bölümünde yinelenen erişim incelemesi hakkında daha fazla bilgi edinin

### <a name="draft-your-privileged-identity-management-settings"></a>Ayrıcalıklı Kimlik Yönetimi ayarlarınızı hazırlayın

Ayrıcalıklı Kimlik Yönetimi çözümünüzü uygulamadan önce, kuruluşunuzun kullandığı her ayrıcalıklı rol için Ayrıcalıklı Kimlik Yönetimi ayarlarınızı hazırlamak iyi bir uygulamadır. Bu bölümde belirli roller için Ayrıcalıklı Kimlik Yönetimi ayarlarına bazı örnekler vardır (bunlar yalnızca başvuru amaçlıdır ve kuruluşunuz için farklı olabilir). Bu ayarların her biri tablolardan sonra Microsoft'un önerileri ile ayrıntılı olarak açıklanır.

#### <a name="privileged-identity-management-settings-for-azure-ad-roles"></a>Azure AD rolleri için ayrıcalıklı Kimlik Yönetimi ayarları

| Rol | MFA gerektirme | Bildirim | Olay bileti | Onay gerektirme | Onaylayan | Aktivasyon Süresi | Daimi yönetici |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Genel Yönetici | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | Diğer Global Yöneticiler | 1 Saat | Acil erişim hesapları |
| Exchange Yöneticisi | :heavy_check_mark: | :heavy_check_mark: | :x: | :x: | None | 2 Saat | None |
| Yardım Masası Yöneticisi | :x: | :x: | :heavy_check_mark: | :x: | None | 8 Saat | None |

#### <a name="privileged-identity-management-settings-for-azure-resource-roles"></a>Azure kaynak rolleri için ayrıcalıklı Kimlik Yönetimi ayarları

| Rol | MFA gerektirme | Bildirim | Onay gerektirme | Onaylayan | Etkinleştirme süresi | Etkin yönetici | Etkin son kullanma süresi | Uygun zaman aşımı |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Kritik aboneliklerin sahibi | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | Aboneliğin diğer sahipleri | 1 Saat | None | yok | 3 ay |
| Daha az kritik aboneliklerin Kullanıcı Erişim Yöneticisi | :heavy_check_mark: | :heavy_check_mark: | :x: | None | 1 Saat | None | yok | 3 ay |
| Sanal Makine Katılımcısı | :x: | :heavy_check_mark: | :x: | None | 3 Saat | None | yok | 6 ay |

Aşağıdaki tabloda her ayarı açıklanmaktadır.

| Ayar | Açıklama |
| --- | --- |
| Rol | Ayarları tanımladığınız rolün adı. |
| MFA gerektirme | Uygun kullanıcının rolü etkinleştirmeden önce MFA gerçekleştirmesi gerekip gerekmediği.<br/><br/> :heavy_check_mark: Microsoft, özellikle rollerin konuk kullanıcıları varsa, tüm yönetici rolleri için MFA'yı uygulamanızı **önerir.** |
| Bildirim | Doğru olarak ayarlanırsa, kuruluştaki Global Administrator, Ayrıcalıklı Rol Yöneticisi ve Güvenlik Yöneticisi, uygun bir kullanıcı rolü etkinleştirdiğinde bir e-posta bildirimi alır.<br/><br/>**Not:** Bazı kuruluşların yönetici hesaplarına bağlı bir e-posta adresi yoktur, bu e-posta bildirimlerini almak için yöneticilerin bu e-postaları alması için alternatif bir e-posta adresi ayarlamanız gerekir. |
| Olay bileti | Uygun kullanıcının rolünü etkinleştirirken bir olay bilet numarasını kaydetmesi gerekip gerekmediği. Bu ayar, istenmeyen etkinleştirmeleri azaltmak için kuruluşun her etkinleştiriyi dahili olay numarasıyla tanımlamasına yardımcı olur.<br/><br/> :heavy_check_mark: Microsoft, Ayrıcalıklı Kimlik Yönetimini dahili sisteminize bağlamak için olay bilet numaralarından yararlanmanızı **önerir.** Bu, etkinleştirme için içeriğe ihtiyaç duyan onaylayıcılar için özellikle yararlıdır. |
| Onay gerektirme | Uygun kullanıcının rolü etkinleştirmek için onay alması gerekip gerekmediği.<br/><br/> :heavy_check_mark: Microsoft, en fazla izinle roller için onay ayarlamanızı **önerir.** Tüm Ayrıcalıklı Kimlik Yönetimi müşterilerinin kullanım kalıplarını temel alan Global Administrator, User Administrator, Exchange Administrator, Security Administrator ve Password Administrator onay kurulumuile en yaygın rollerdir. |
| Onaylayan | Uygun rolü etkinleştirmek için onay gerekiyorsa, isteği onaylaması gereken kişileri listelayın. Varsayılan olarak, Ayrıcalıklı Kimlik Yönetimi, onaylayıcıyı kalıcı veya uygun olsun ayrıcalıklı bir rol yöneticisi olan tüm kullanıcılar olarak ayarlar.<br/><br/>**Not:** Bir kullanıcı hem Azure REKLAM rolü hem de rolü onaylayan biri için uygunsa, kendilerini onaylayamayacaktır.<br/><br/> :heavy_check_mark: Microsoft, onaylayıcıları, Global Administrator yerine belirli rol ve sık sık kullananlar hakkında en bilgili kişiler olarak seçmenizi **önerir.** |
| Etkinleştirme süresi | Bir kullanıcının süresi dolmadan önce rolde etkinleştirilecek süre. |
| Daimi yönetici | Rol için kalıcı yönetici olacak kullanıcıların listesi (etkinleştirmek zorunda kalmamak).<br/><br/> :heavy_check_mark: Microsoft, Global Yöneticiler dışında tüm roller için sıfır daimi yöneticiniz olduğunu **önerir.** Kimler uygun yapılmalı ve bu planın kalıcı olarak etkin bir bölümü olmalıdır bu konuda daha fazla bilgi edinin. |
| Etkin yönetici | Azure kaynakları için etkin yönetici, rolü kullanmak için hiçbir zaman etkinleştirmek zorunda olmayan kullanıcıların listesidir. Kullanıcının bu rolü ne zaman kaybedeceğine göre bir son kullanma süresi ayarlayabildiğinizden, azure REKLAM rollerindeki gibi kalıcı yönetici olarak adlandırılmaz. |
| Etkin son kullanma süresi | Azure kaynak rolleri için etkin bir rol ataması, bu yapılandırılmış süreden sonra sona erer. 15 gün, 1 ay, 3 ay, 6 ay, 1 yıl veya kalıcı olarak aktif seçebilirsiniz. |
| Uygun zaman aşımı | Azure kaynak rolleri için uygun bir rol ataması, bu yapılandırılmış süreden sonra sona erer. 15 gün, 1 ay, 3 ay, 6 ay, 1 yıl veya kalıcı olarak uygun seçim yapabilirsiniz. |

## <a name="implement-your-solution"></a>Çözümünüzü uygulayın

Doğru planlamanın temeli, Azure Active Directory ile bir uygulamayı başarıyla dağıtabileceğiniz temeldir.  Başarılı dağıtımsüresini azaltırken biniş süresini kolaylaştıran akıllı güvenlik ve tümleştirme sağlar.  Bu kombinasyon, son kullanıcılarınız için kapalı kalma süresini azaltırken uygulamanızın kolaylıkla entegre edilmesini sağlar.

### <a name="identify-test-users"></a>Test kullanıcılarını belirleme

Uygulamayı doğrulamak için bir kullanıcı kümesini ve kullanıcı gruplarını tanımlamak için bu bölümü kullanın. Planlama bölümünde seçtiğiniz ayarlara bağlı olarak, her rol için sınamak istediğiniz kullanıcıları tanımlayın.

> [!TIP]
> :heavy_check_mark: Microsoft, her Azure REKLAM rolünün hizmet sahiplerini test kullanıcıları haline getirmenizi **önerir,** böylece bu işlem hakkında bilgi sahibi olabilir ve kullanıma sunulması için dahili bir savunucu olmalarını önerir.

Bu tabloda, her rol için ayarların çalıştığını doğrulayacak test kullanıcılarını tanımlayın.

| Rol adı | Test kullanıcıları |
| --- | --- |
| &lt;Rol adı&gt; | &lt;Rolü test etmek için kullanıcılar&gt; |
| &lt;Rol adı&gt; | &lt;Rolü test etmek için kullanıcılar&gt; |

### <a name="test-implementation"></a>Test uygulaması

Artık test kullanıcılarını tanımladığınıza göre, test kullanıcılarınız için Ayrıcalıklı Kimlik Yönetimi'ni yapılandırmak için bu adımı kullanın. Kuruluşunuz Azure portalında Ayrıcalıklı Kimlik Yönetimi'ni kullanmak yerine Ayrıcalıklı Kimlik Yönetimi iş akışını kendi iç uygulamanıza dahil etmek istiyorsa, Ayrıcalıklı Kimlik Yönetimi'ndeki tüm işlemler grafik [API'miz](https://docs.microsoft.com/graph/api/resources/privilegedidentitymanagement-root)aracılığıyla da desteklenir.

#### <a name="configure-privileged-identity-management-for-azure-ad-roles"></a>Azure AD rolleri için Ayrıcalıklı Kimlik Yönetimini Yapılandırma

1. [Azure AD rol ayarlarını](pim-how-to-change-default-settings.md) planladığınız şeye göre yapılandırın.

1. Azure **REKLAM rollerine**gidin, **Roller'i**tıklatın ve sonra yeni yapılandırdığınız rolü seçin.

1. Test kullanıcıları grubu için, zaten kalıcı bir yöneticiyseler, onları arayarak ve satırlarındaki üç noktayı tıklatarak onları kalıcıdan uygun alabilene dönüştürerek uygun hale getirebilirsiniz. Henüz rol atamaları yoksa, [yeni bir uygun atama yapabilirsiniz.](pim-how-to-add-role-to-user.md#make-a-user-eligible-for-a-role)

1. Test etmek istediğiniz tüm roller için 1-3 adımlarını yineleyin.

1. Test kullanıcılarını ayarladıktan sonra, azure [AD rollerini](pim-how-to-activate-role.md)etkinleştirme için onlara bağlantı göndermelisiniz.

#### <a name="configure-privileged-identity-management-for-azure-resource-roles"></a>Azure kaynak rolleri için Ayrıcalıklı Kimlik Yönetimini yapılandırma

1. Test etmek istediğiniz bir abonelik veya kaynak taki rolü için [Azure kaynak rol ayarlarını yapılandırın.](pim-resource-roles-configure-role-settings.md)

1. Bu abonelik için **Azure kaynaklarına** gidin ve **Roller'i**tıklatın, yeni yapılandırdığınız rolü seçin.

1. Test kullanıcıları grubu için, zaten etkin bir yöneticiyseler, onları arayarak ve [rol atamalarını güncelleştirerek](pim-resource-roles-assign-roles.md#update-or-remove-an-existing-role-assignment)onları uygun hale getirebilirsiniz. Henüz rol yoksa, [yeni bir rol atayabilirsiniz.](pim-resource-roles-assign-roles.md#assign-a-role)

1. Test etmek istediğiniz tüm roller için 1-3 adımlarını yineleyin.

1. Test kullanıcılarını ayarladıktan sonra, azure kaynak rollerini nasıl [etkinleştireceklerine](pim-resource-roles-activate-your-roles.md)yönelik bağlantıyı göndermelisiniz.

Roller için ayarladığınız tüm yapılandırmanın doğru çalışıp çalışmadığını doğrulamak için bu aşamayı kullanmanız gerekir. Testlerinizi belgelemek için aşağıdaki tabloyu kullanın. Etkilenen kullanıcılarla iletişimi en iyi duruma getirmek için de bu aşamayı kullanmalısınız.

| Rol | Etkinleştirme sırasında beklenen davranış | Gerçek sonuçlar |
| --- | --- | --- |
| Genel Yönetici | (1) MFA gerektirir<br/>(2) Onay talep<br/>(3) Onaylayan bildirim alır ve onaylayabilir<br/>(4) Rol önceden ayarlanmış süre sonra sona erer |  |
| Abonelik *X* sahibi | (1) MFA gerektirir<br/>(2) uygun atama yapılandırılmış süre sonra sona erer |  |

### <a name="communicate-privileged-identity-management-to-affected-stakeholders"></a>Ayrıcalıklı Kimlik Yönetimini etkilenen paydaşlara iletin

Ayrıcalıklı Kimlik Yönetimi'nin dağıtılması, ayrıcalıklı rollere sahiptir olan kullanıcılar için ek adımlar başlatacaktır. Ayrıcalıklı Kimlik Yönetimi, ayrıcalıklı kimliklerle ilişkili güvenlik sorunlarını büyük ölçüde azaltsa da, değişikliğin kiracı çapındaki dağıtımdan önce etkili bir şekilde iletilmesi gerekir. Etkilenen yöneticilerin sayısına bağlı olarak, kuruluşlar genellikle değişiklikle ilgili bir dahili belge, video veya e-posta oluşturmayı seçer. Sık sık bu iletişimdahil şunlardır:

- PIM nedir
- Organizasyonun yararı nedir
- Kimler etkilenecek
- PIM ne zaman kullanıma sunulacak?
- Kullanıcıların rollerini etkinleştirmeleri için hangi ek adımlar gerekir?
    - Belgelerimize bağlantılar göndermelisiniz:
    - [Azure AD rollerini etkinleştirme](pim-how-to-activate-role.md)
    - [Azure kaynak rollerini etkinleştirme](pim-resource-roles-activate-your-roles.md)
- PIM ile ilişkili sorunlar için iletişim bilgileri veya yardım masası bağlantısı

> [!TIP]
> :heavy_check_mark: Microsoft, ayrıcalıklı kimlik yönetimi iş akışında (kuruluşunuzun dahili bir BT destek ekibi varsa) bunları yürümek için yardım masanızla/destek ekibinizle zaman ayarlamanızı **önerir.** İletişim bilgilerinizin yanı sıra uygun belgeleri de sağlayın.

### <a name="move-to-production"></a>Üretime taşıma

Testiniz tamamlandıktan ve başarılı olduktan sonra, Ayrıcalıklı Kimlik Yönetimi yapılandırmanızda tanımladığınız her rolün tüm kullanıcıları için test aşamalarındaki tüm adımları yineleyerek Ayrıcalıklı Kimlik Yönetimi'ni üretime taşıyın. Azure AD rolleri için Ayrıcalıklı Kimlik Yönetimi için kuruluşlar, diğer roller için Ayrıcalıklı Kimlik Yönetimini sınamadan ve kullanıma çıkarmadan önce genellikle Global Yöneticiler için Ayrıcalıklı Kimlik Yönetimini test eder ve kullanıma sunar. Bu arada Azure kaynağı için kuruluşlar normalde Ayrıcalıklı Kimlik Yönetimi'ni aynı anda bir Azure aboneliğini sınayıp kullanıma sunar.

### <a name="in-the-case-a-rollback-is-needed"></a>Bir geri alma gerekli olduğunda

Ayrıcalıklı Kimlik Yönetimi üretim ortamında istenildiği gibi çalışamazsa, aşağıdaki geri alma adımları Ayrıcalıklı Kimlik Yönetimi'ni kurmadan önce bilinen iyi duruma geri dönmenize yardımcı olabilir:

#### <a name="azure-ad-roles"></a>Azure AD rolleri

1. [Azure portalında](https://portal.azure.com/)oturum açın.
1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.
1. **Azure REKLAM rollerini** tıklatın ve ardından **Roller'i**tıklatın.
1. Yapılandırdığınız her rol için, uygun bir atamaya sahip tüm kullanıcılar için elipsis (**...**) seçeneğini tıklatın.
1. Rol atamasını kalıcı hale getirmek için **kalıcı** yap seçeneğini tıklatın.

#### <a name="azure-resource-roles"></a>Azure kaynağı rolleri

1. [Azure portalında](https://portal.azure.com/)oturum açın.
1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.
1. **Azure kaynaklarını** tıklatın ve ardından geri almak istediğiniz bir aboneliği veya kaynağı tıklatın.
1. **Roller'i**tıklatın.
1. Yapılandırdığınız her rol için, uygun bir atamaya sahip tüm kullanıcılar için elipsis (**...**) seçeneğini tıklatın.
1. Rol atamasını kalıcı hale getirmek için **kalıcı** yap seçeneğini tıklatın.

## <a name="next-steps-after-deploying"></a>Dağıttıktan sonraki adımlar

Ayrıcalıklı Kimlik Yönetimini üretimde başarıyla dağıtmak, kuruluşunuzun ayrıcalıklı kimliklerini güvence altına almak açısından ileriye doğru atılmış önemli bir adımdır. Ayrıcalıklı Kimlik Yönetimi'nin dağıtımıyla birlikte, güvenlik ve uyumluluk için kullanmanız gereken ek Ayrıcalıklı Kimlik Yönetimi özellikleri gelir.

### <a name="use-privileged-identity-management-alerts-to-safeguard-your-privileged-access"></a>Ayrıcalıklı erişiminizi korumak için Ayrıcalıklı Kimlik Yönetimi uyarılarını kullanın

Kiracınızı daha iyi korumak için Ayrıcalıklı Kimlik Yönetimi'nin yerleşik uyarı işlevini kullanmalısınız. Daha fazla bilgi için [güvenlik uyarılarına](pim-how-to-configure-security-alerts.md#security-alerts)bakın. Bu uyarılar şunlardır: yöneticiler ayrıcalıklı rolleri kullanmıyor, roller Ayrıcalıklı Kimlik Yönetimi dışında atanıyor, roller çok sık ve daha fazla etkinleştiriliyor. Kuruluşunuzu tam olarak korumak için, düzenli olarak uyarı listenizi gözden geçirmeli ve sorunları gidermelisiniz. Uyarılarınızı aşağıdaki şekilde görüntüleyebilir ve düzeltebilirsiniz:

1. [Azure portalında](https://portal.azure.com/)oturum açın.
1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.
1. **Azure AD rollerini** tıklatın ve ardından **Uyarılar'ı**tıklatın.

> [!TIP]
> :heavy_check_mark: Microsoft, yüksek öneme sahip olarak işaretlenmiş tüm uyarıları hemen ele **almanı önerir.** Orta ve düşük önem emzitli uyarılar için, bir güvenlik tehdidi olduğuna inanıyorsanız, bilgi sahibi olmalı ve değişiklikler yapmalısınız.

Belirli uyarılardan herhangi biri yararlı değilse veya kuruluşunuz için geçerli değilse, uyarılar sayfasındaki uyarıyı her zaman kapatabilirsiniz. Bu işten çıkarmayı daha sonra Azure REKLAM ayarları sayfasından her zaman geri alabilirsiniz.

### <a name="set-up-recurring-access-reviews-to-regularly-audit-your-organizations-privileged-identities"></a>Kuruluşunuzun ayrıcalıklı kimliklerini düzenli olarak denetlemek için yinelenen erişim incelemeleri ayarlama

Erişim incelemeleri, ayrıcalıklı rollerle veya belirli gözden geçirenlere atanan kullanıcılara her kullanıcının ayrıcalıklı kimliğe ihtiyacı olup olmadığını sormanın en iyi yoludur. Saldırı yüzeyini azaltmak ve uyumlu kalmak istiyorsanız, erişim değerlendirmeleri harikadır. Erişim incelemesi başlatma hakkında daha fazla bilgi için Azure [REKLAM rolleri erişim incelemeleri](pim-how-to-start-security-review.md) ve [Azure kaynak rolleri incelemelere erişin](pim-resource-roles-start-access-review.md)bakın. Bazı kuruluşlar için, periyodik erişim incelemesi yapmak yasalara ve yönetmeliklere uygun kalmak için gereklidir, diğerleri için erişim gözden geçirme kuruluşunuz boyunca en az ayrıcalık ilkesini uygulamak için en iyi yoldur.

> [!TIP]
> Heavy_check_mark: **Microsoft,** tüm Azure AD ve Azure kaynak rolleriniz için üç ayda bir erişim incelemeleri ayarlamanızı önerir.

Çoğu durumda, Azure AD rollerini gözden geçiren kullanıcılardır, Azure kaynak rolleri için gözden geçiren ise aboneliğin sahibidir ve bu rol de bu rolün içinde dir. Ancak, genellikle şirketlerin herhangi bir kişinin e-posta adresi ile bağlantılı olmayan ayrıcalıklı hesapları olduğu durumdur. Bu gibi durumlarda, kimse erişimi okumaz ve incelemez.

> [!TIP]
> :heavy_check_mark: **Microsoft,** düzenli olarak denetlenen bir e-posta adresine bağlı olmayan ayrıcalıklı rol atamaları olan tüm hesaplar için ikincil bir e-posta adresi eklemenizi önerir

### <a name="get-the-most-out-of-your-audit-log-to-improve-security-and-compliance"></a>Güvenliği ve uyumluluğu artırmak için denetim günlüğünden en iyi şekilde

Denetim günlüğü, güncel kalabileceğiniz ve yönetmeliklere uygun olabileceğiniz yerdir. Ayrıcalıklı Kimlik Yönetimi şu anda kuruluşunuzun tüm geçmişini denetim günlüğünde saklar:

- Uygun rollerin etkinleştirme/devre dışı bırakılması
- Ayrıcalıklı Kimlik Yönetimi içinde ve dışında rol atama faaliyetleri
- Rol ayarlarındaki değişiklikler
- Onay kurulumu ile rol etkinleştirme için etkinlikleri isteme/onaylama/reddetme
- Uyarıları güncelleştirme

Global Yönetici veya ayrıcalıklı bir rol yöneticisiyseniz bu denetim günlüklerine erişebilirsiniz. Daha fazla bilgi için Azure [REKLAM rolleri için denetim geçmişine](pim-how-to-use-audit-log.md) ve Azure kaynak rolleri için denetim [geçmişine](azure-pim-resource-rbac.md)bakın.

> [!TIP]
> :heavy_check_mark: Microsoft, en az bir yöneticinin tüm denetim olaylarını haftalık olarak okumanızı ve denetim etkinliklerinizi aylık olarak dışa aktarmanızı **önerir.**

Denetim etkinliklerinizi otomatik olarak daha uzun bir süre depolamak istiyorsanız, Ayrıcalıklı Kimlik Yönetimi'nin denetim günlüğü Otomatik olarak [Azure REKLAM denetim günlüklerine](../reports-monitoring/concept-audit-logs.md)eşitlenir.

> [!TIP]
> :heavy_check_mark: Microsoft, güvenlik ve uyumluluk gereksinimi için denetim olaylarını bir Azure depolama hesabında arşivlemek için [Azure günlük izlemesini](../reports-monitoring/concept-activity-logs-azure-monitor.md) ayarlamanızı **önerir.**
