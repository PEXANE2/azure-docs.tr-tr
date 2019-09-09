---
title: Dağıtım Privileged Identity Management (PıM)-Azure Active Directory | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) dağıtımının nasıl planlanacağını açıklar.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 02/08/2019
ms.author: curtand
ms.custom: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4ff46a693766ab12b53baba1ad8e4a56e174a076
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804493"
---
# <a name="deploy-azure-ad-privileged-identity-management-pim"></a>Azure AD Privileged Identity Management dağıtma (PıM)

Bu adım adım kılavuzda, kuruluşunuzda Azure Active Directory (Azure AD) Privileged Identity Management (PıM) dağıtımının nasıl planlanacağı açıklanmaktadır.

> [!TIP]
> Bu belgenin tamamında şöyle işaretlenmiş öğeleri göreceksiniz:
> 
> :heavy_check_mark: **Microsoft şunları öneriyor**
> 
> Bunlar genel önerilerdir ve yalnızca belirli kurumsal gereksinimleriniz için geçerlidir.

## <a name="step-1-learn-about-pim"></a>1\.Adım PıM hakkında bilgi edinin

Azure AD Privileged Identity Management (PıM), Azure AD, Azure kaynakları ve diğer Microsoft Online Services genelinde ayrıcalıklı yönetim rollerini yönetmenize yardımcı olur. Ayrıcalıklı kimliklerin atandığı ve unutulmuş bir dünyada, PıM, tam zamanında erişim, onay iş akışları isteme ve tamamen tümleşik erişim incelemeleri gibi çözümler sağlayarak ayrıcalıklı kötü amaçlı etkinlikler tanımlayabilir, bunları açabilir ve engelleyebilir gerçek zamanlı roller. Kuruluşunuz genelinde ayrıcalıklı rollerinizi yönetmek için PıM 'yi dağıtmak, ayrıcalıklı rollerinizin etkinlikleri hakkında değerli içgörüler elde edilirken riski önemli ölçüde azaltır.

### <a name="business-value-of-pim"></a>PıM iş değeri

**Risk yönetme** - [en az ayrıcalık erişimi](/windows-server/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models) ve tam zamanında erişim ilkesini zorunlu tutarak kuruluşunuzun güvenliğini sağlayın. Ayrıcalıklı rollere sahip kullanıcıların kalıcı atamalarının sayısını en aza indirerek ve yükseltme için onayları ve MFA 'yı zorunlu tutarak, kuruluşunuzda ayrıcalıklı erişimle ilgili güvenlik risklerini büyük ölçüde azaltabilirsiniz. En az ayrıcalık ve tam zamanında erişim zorlama, ayrıcalıklı rollere erişim geçmişini görüntülemenize ve güvenlik sorunlarını ortaya çıkan şekilde izlemenize olanak tanır.

**Adres uyumluluğu ve idare** -dağıtım PIM, şirket içi kimlik yönetimi için bir ortam oluşturur. Ayrıcalıklı kimliklerin tam zamanında yükseltilmesi, PıM 'nin kuruluşunuzda ayrıcalıklı erişim etkinliklerini takip etmesini sağlamak için bir yol sağlar. Ayrıca, kuruluşunuzdaki kalıcı ve uygun rollerin tüm atamaları için bildirimleri görüntüleyebilir ve alabilirsiniz. Erişim gözden geçirmesi sayesinde, gereksiz ayrıcalıklı kimlikleri düzenli olarak denetleyebilir ve kaldırabilir ve kuruluşunuzun en ciddi kimlik, erişim ve güvenlik standartları ile uyumlu olduğundan emin olabilirsiniz.

**Maliyetleri azaltın** -PIM 'yi doğru dağıtarak verimsizlikleri, insan hatası ve güvenlik sorunlarını ortadan kaldırarak maliyetleri düşürün. Net sonuç, yüksek maliyetli ve kurtarılması zor olan ayrıcalıklı kimliklerle ilişkili siber suçlar 'in bir azalmasıyla sonuçlanır. PıM, kuruluşunuzun yönetmelikleri ve standartları ile uyumlu hale geldiğinde denetim erişim bilgileriyle ilişkili maliyeti azaltmasına de yardımcı olur.

Daha fazla bilgi için [Azure AD Privileged Identity Management nedir?](pim-configure.md).

### <a name="licensing-requirements"></a>Lisanslama gereksinimleri

PıM 'yi kullanmak için dizininiz aşağıdaki ücretli veya deneme lisanslarından birine sahip olmalıdır:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5
- Microsoft 365 M5

Daha fazla bilgi için bkz. [PIM 'yi kullanmak Için lisans gereksinimleri](subscription-requirements.md).

### <a name="key-pim-terminology"></a>Anahtar PıM terminolojisi

| Kavram veya sözleşme | Açıklama |
| --- | --- |
| lebilecek | Bir kullanıcının rolü kullanmak için bir veya daha fazla eylem gerçekleştirmesini gerektiren bir rol ataması. Bir Kullanıcı bir rol için uygun hale getirildiğinde, ayrıcalıklı görevleri gerçekleştirmeleri gerektiğinde rolü etkinleştirebilecekleri anlamına gelir. Kalıcı ve uygun bir rol atamasına karşılık gelen bir kişiye verilen erişimde fark yoktur. Tek fark, bazı kişilerin her zaman erişim gereksinimi yoktur. |
| etkinleştir | Kullanıcının uygun olduğu bir rolü kullanmak için bir veya daha fazla eylem gerçekleştirme işlemi. Eylemler, bir Multi-Factor Authentication (MFA) denetimi gerçekleştirmeye, iş gerekçesinin sağlanmasından veya belirlenen onaylayanlardan onay isteğinde bulunabilir. |
| tam zamanında (JıT) erişim | Kullanıcıların ayrıcalıklı görevleri gerçekleştirmek için geçici izinleri aldığı, kötü amaçlı veya yetkisiz kullanıcıların izinlerin süre dolduktan sonra erişim kazanmasını önleyen bir model. Erişim yalnızca kullanıcılara ihtiyaç duymadan verilir. |
| en az ayrıcalık erişimi ilkesi | Her kullanıcının yalnızca, gerçekleştirme yetkisine sahip oldukları görevleri gerçekleştirmek için gereken en düşük ayrıcalıklarla sağlandığı önerilen bir güvenlik uygulaması. Bu uygulama, genel yönetici sayısını en aza indirir ve bunun yerine belirli senaryolar için belirli yönetici rollerini kullanır. |

Daha fazla bilgi için bkz. [terminoloji](pim-configure.md#terminology).

### <a name="high-level-overview-of-how-pim-works"></a>PıM 'nin nasıl çalıştığına ilişkin üst düzey genel bakış

1. PıM, kullanıcıların ayrıcalıklı rollere uygun olması için ayarlanır.
1. Uygun bir kullanıcının ayrıcalıklı rolünü kullanması gerektiğinde, bu rolü PıM 'de etkinleştirir.
1. Rol için yapılandırılmış PıM ayarlarına bağlı olarak, kullanıcının belirli adımları tamamlaması gerekir (Multi-Factor Authentication, onay alma veya bir nedeni belirtme gibi)
1. Kullanıcı rolünü başarıyla etkinleştirdikten sonra, önceden yapılandırılmış bir zaman aralığı için rol alırlar.
1. Yöneticiler denetim günlüğündeki tüm PıM etkinliklerinin geçmişini görüntüleyebilir. Ayrıca kiralamaları daha da güvenli hale getirirler ve erişim gözden geçirmeleri ve uyarıları gibi PıM özelliklerini kullanarak uyumluluğa uyum sağlayabilir

Daha fazla bilgi için [Azure AD Privileged Identity Management nedir?](pim-configure.md).

### <a name="roles-that-can-be-managed-by-pim"></a>PıM tarafından yönetilebilecek roller

**Azure AD rolleri** – bu rollerin hepsi Azure Active Directory (genel yönetici, Exchange Yöneticisi ve Güvenlik Yöneticisi gibi). [Azure Active Directory Içindeki yönetici rolü izinlerinde](../users-groups-roles/directory-assign-admin-roles.md)roller ve bunların işlevleri hakkında daha fazla bilgi edinebilirsiniz. Yöneticilerinize hangi rollerin atanacağını belirlemeye yönelik yardım için bkz. [göreve göre en az ayrıcalıklı roller](../users-groups-roles/roles-delegate-by-task.md).

**Azure Kaynak rolleri** – bu roller bir Azure kaynağına, kaynak grubuna, aboneliğe veya yönetim grubuna bağlanır. PıM, sahip, Kullanıcı erişimi Yöneticisi ve katkıda bulunan, ayrıca [özel roller](../../role-based-access-control/custom-roles.md)gibi yerleşik rollere tam zamanında erişim sağlar. Azure Kaynak rolleri hakkında daha fazla bilgi için bkz. [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md).

Daha fazla bilgi için bkz. [PIM 'de yöneteceğiniz roller](pim-roles.md).

## <a name="step-2-plan-your-deployment"></a>2\.Adım Dağıtımınızı planlama

Bu bölüm, kuruluşunuzda PıM 'yi dağıtılmadan önce yapmanız gerekenler konusunda odaklanır. Bu bölümde, kuruluşunuzun ayrıcalıklı kimlikleri için uygun olan en iyi planı oluşturmak için size kılavuzluk edecek şekilde yönergeler izlemeniz ve bu bölümdeki kavramların anlaşılması önemlidir.

### <a name="identify-your-stakeholders"></a>Paydaşlarınızı tanımla

Aşağıdaki bölümde, projede yer alan tüm paydaşların belirlenmesi ve oturum kapatması, gözden geçirilmesi veya haberdar olmanız gerekir. Azure AD rolleri için PıM ve Azure Kaynak rolleri için PıM dağıtmak üzere ayrı tablolar içerir. Aşağıdaki tabloda bulunan paydaşlara kuruluşunuz için uygun şekilde ekleyin.

- Bu nedenle = bu projede oturumu Kapat
- R = bu projeyi gözden geçirin ve giriş sağlayın
- I = bu projenin bilgilendirilmesi

#### <a name="stakeholders-pim-for-azure-ad-roles"></a>Paydaşlar Azure AD rolleri Için PıM

| Name | Role | Action |
| --- | --- | --- |
| Ad ve e-posta | **Kimlik mimarı veya Azure genel Yöneticisi**<br/>Kimlik Yönetimi ekibinin, bu değişikliğin kuruluşunuzdaki temel kimlik yönetimi altyapısına nasıl hizalanacağını tanımlamaya yönelik bir temsilcisidir. | SO/R/I |
| Ad ve e-posta | **Hizmet sahibi/satır Yöneticisi**<br/>Bir hizmetin veya bir hizmet grubunun BT sahiplerine bir temsilci. Bunlar, kararlar verirken ve takımlarına ait PıM 'yi kullanıma almanıza yardımcı olan bir anahtarlardır. | SO/R/I |
| Ad ve e-posta | **Güvenlik sahibi**<br/>Güvenlik ekibinden, planın kuruluşunuzun güvenlik gereksinimlerini karşıladığı oturumu kapatan bir temsilcisidir. | SO/R |
| Ad ve e-posta | **BT Destek Yöneticisi/Yardım Masası**<br/>BT destek kuruluşundan, bu değişikliğin bir yardım masası perspektifinden desteklenebilirliği hakkında giriş sağlayabilen bir temsilcisidir. | R/I |
| Pilot kullanıcılar için ad ve e-posta | **Ayrıcalıklı rol kullanıcıları**<br/>Ayrıcalıklı kimlik yönetiminin uygulandığı Kullanıcı grubu. Bu kişilerin, PıM uygulandıktan sonra rollerini nasıl etkinleştireceğinize bilmeleri gerekir. | I |

#### <a name="stakeholders-pim-for-azure-resource-roles"></a>Paydaşlar Azure Kaynak rolleri Için PıM

| Name | Role | Action |
| --- | --- | --- |
| Ad ve e-posta | **Abonelik/kaynak sahibi**<br/>PıM dağıtmak istediğiniz her abonelik veya kaynağa ait BT sahiplerinden bir temsilci | SO/R/I |
| Ad ve e-posta | **Güvenlik sahibi**<br/>Güvenlik ekibinden, planın kuruluşunuzun güvenlik gereksinimlerini karşıladığı oturumu kapatan bir temsilcisidir. | SO/R |
| Ad ve e-posta | **BT Destek Yöneticisi/Yardım Masası**<br/>BT destek kuruluşundan, bu değişikliğin bir yardım masası perspektifinden desteklenebilirliği hakkında giriş sağlayabilen bir temsilcisidir. | R/I |
| Pilot kullanıcılar için ad ve e-posta | **RBAC rol kullanıcıları**<br/>Ayrıcalıklı kimlik yönetiminin uygulandığı Kullanıcı grubu. Bu kişilerin, PıM uygulandıktan sonra rollerini nasıl etkinleştireceğinize bilmeleri gerekir. | I |

### <a name="enable-pim"></a>PıM 'yi etkinleştir

Planlama işleminin bir parçası olarak, [PIM belgesi 'ni kullanmaya başlama](pim-getting-started.md)sürecimizi IZLEYEREK önce PIM 'yi kabul etmeniz ve etkinleştirmeniz gerekir. PıM 'nin etkinleştirilmesi, dağıtımınıza yardımcı olmak üzere özel olarak tasarlanan bazı özelliklere erişmenizi sağlar.

Amacınız Azure kaynakları için PıM 'yi dağıtmaktır, [PIM belgesinde yönetmek için bulma Azure kaynaklarını](pim-resource-roles-discover-resources.md)izlemelisiniz. Yalnızca her bir kaynağın, kaynak grubunun ve aboneliğin sahipleri, bu dosyaları PıM içinde keşfedebilecektir. Azure kaynaklarınız için PıM dağıtmaya çalışan bir genel yöneticiyseniz, [tüm Azure aboneliklerini yönetmek için erişimi](../../role-based-access-control/elevate-access-global-admin.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json) , dizinde bulunan tüm Azure kaynaklarına erişim sağlamak üzere yükseltebilir. Ancak, bu kaynakları PıM ile yönetmeden önce, abonelik sahiplerinizin her birinden onay almanızı tavsiye ederiz.

### <a name="enforce-principle-of-least-privilege"></a>En az ayrıcalık ilkesini zorla

Kuruluşunuzda hem Azure AD hem de Azure Kaynak rolleriniz için en az ayrıcalık ilkesini zorlediğinizden emin olmanız önemlidir.

#### <a name="azure-ad-roles"></a>Azure AD rolleri

Azure AD rolleri için, çoğu yöneticinin yalnızca bir veya iki özel yönetici rolüne ihtiyacı olduğunda, kuruluşların küresel yönetici rolünü önemli sayıda yöneticiye ataması yaygındır. Ayrıcalıklı rol atamaları da yönetilmeyen olarak sola kalır.

> [!NOTE]
> Rol temsilinin ortak yolları:
>
> - Exchange 'in ücretlendirmiş olması, genel yönetici rolü olarak yalnızca Exchange Yöneticisi rolüne gerek bıraksa bile, bunlara ait günlük işleri gerçekleştirmesini sağlar.
> - Yönetici hem güvenlik hem de SharePoint yönetici rollerine ihtiyaç duyduğundan ve yalnızca bir rolün atanması daha kolay olduğundan, genel yönetici rolü bir Office yöneticisine atanır.
> - Yöneticiye bir görevi uzun süre önce gerçekleştirecek bir güvenlik yöneticisi rolü atandı, ancak hiç kaldırılmadı.

Azure AD rolleriniz için en az ayrıcalık ilkesini zorlamak için aşağıdaki adımları izleyin.

1. [Kullanılabilir Azure AD yönetici rollerini](../users-groups-roles/directory-assign-admin-roles.md#available-roles)okuyup anlayarak rollerin ayrıntı düzeyini anlayın. Siz ve takımınız Ayrıca, belirli görevler için en az ayrıcalıklı rolü açıklayan [Azure AD 'de kimlik görevine göre yönetici rollerine](../users-groups-roles/roles-delegate-by-task.md)başvurmalıdır.

1. Kuruluşunuzda ayrıcalıklı rollere sahip olan liste. Aşağıdaki gibi bir sayfaya ulaşmak için [PIM Sihirbazı 'nı](pim-security-wizard.md#run-the-wizard) kullanabilirsiniz.

    ![Ayrıcalıklı rollere sahip kişileri gösteren ayrıcalıklı roller bölmesini bul](./media/pim-deployment-plan/discover-privileged-roles-users.png)

1. Kuruluşunuzdaki tüm genel Yöneticiler için, bu rolün neden gerekli olduğunu öğrenin. Önceki belgelerin okunmasına bağlı olarak, kişinin işi bir veya daha fazla ayrıntılı yönetici rolü tarafından gerçekleştirilebileceği takdirde, bunları genel yönetici rolünden kaldırmalı ve atamaları Azure Active Directory içinde (başvuru olarak) yapmalısınız: Şu anda Microsoft 'un yalnızca genel yönetici rolüne sahip 10 yönetici vardır. [Microsoft 'un PIM 'yi nasıl kullandığı hakkında](https://www.microsoft.com/itshowcase/Article/Content/887/Using-Azure-AD-Privileged-Identity-Management-for-elevated-access)daha fazla bilgi edinin.

1. Diğer tüm Azure AD rolleri için, atama listesini gözden geçirin, role artık gerek olmayan yöneticileri ve bunları atamalarından kaldırın.

3 ve 4. adımları otomatik hale getirmek için PıM içindeki erişim gözden geçirme işlevini kullanabilirsiniz. [PIM 'de Azure AD rolleri için erişim gözden geçirmesi başlatma](pim-how-to-start-security-review.md)' daki adımları izleyerek, bir veya daha fazla üyeye sahip olan her Azure AD rolü için bir erişim incelemesi ayarlayabilirsiniz.

![Azure AD rolleri için erişim gözden geçirme bölmesi oluşturma](./media/pim-deployment-plan/create-access-review.png)

Gözden geçirenleri **Üyeler (kendi)** olarak ayarlamanız gerekir. Bu işlem, erişim ihtiyacı olup olmadığını doğrulamak için, roldeki tüm üyelere bir e-posta gönderir. Ayrıca, kullanıcıların role ihtiyacı olan nedenleri belirleyebilmeleri için, Gelişmiş ayarlar ' da **onay sırasında neden iste** ' i açmanız gerekir. Bu bilgilere bağlı olarak, gereksiz rollerden kullanıcıları kaldırabileceksiniz ve genel Yöneticiler söz konusu olduğunda daha ayrıntılı yönetici rolleri temsilciliceksiniz.

Erişim gözden geçirmeleri, kişilerin rollere erişimini gözden geçirmesine bildirmek için e-postalara bağımlıdır. E-postaları bağlantılı olmayan ayrıcalıklı hesaplarınız varsa, bu hesapların ikincil e-posta alanını doldurduğunuzdan emin olun. Daha fazla bilgi için bkz. [Azure AD 'de proxyAddresses özniteliği](https://support.microsoft.com/help/3190357/how-the-proxyaddresses-attribute-is-populated-in-azure-ad).

#### <a name="azure-resource-roles"></a>Azure kaynağı rolleri

Azure abonelikleri ve kaynakları için, her bir abonelik veya kaynaktaki rolleri gözden geçirmek üzere benzer bir erişim gözden geçirme işlemi ayarlayabilirsiniz. Bu işlemin amacı, her bir aboneliğe veya kaynağa bağlı olan sahip ve Kullanıcı erişimi yönetici atamalarının yanı sıra gereksiz atamaları kaldırmak için de en aza indirmektir. Ancak kuruluşlar, belirli rolleri daha iyi anlamak (özellikle özel roller) olduğundan, genellikle her bir aboneliğin veya kaynağın sahibine bu tür görevleri devredebilir.

Kuruluşunuzda Azure kaynakları için PıM dağıtmaya çalışan genel yönetici rolü olan bir BT yöneticisiyseniz, her aboneliğe erişim sağlamak için [tüm Azure aboneliklerini yönetmek üzere erişimi de yükseltebilir](../../role-based-access-control/elevate-access-global-admin.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json) . Böylece, her bir abonelik sahibini bulabilir ve bunlarla birlikte çalışarak gereksiz atamaları kaldırabilir ve sahip rolü atamasını en aza indirdirebilirsiniz.

Azure aboneliği için sahip rolüne sahip kullanıcılar, Azure AD rolleri için daha önce açıklanan işleme benzer gereksiz rol atamalarını denetlemek ve kaldırmak için [Azure kaynakları için erişim incelemelerini](pim-resource-roles-start-access-review.md) de kullanabilir.

### <a name="decide-which-role-assignments-should-be-protected-by-pim"></a>Hangi rol atamalarının PıM tarafından korunması gerektiğine karar verme

Kuruluşunuzdaki ayrıcalıklı rol atamalarını kaldırdıktan sonra, PıM ile hangi rollerin korunacağına karar vermeniz gerekir.

Bir rol PıM tarafından korunuyorsa, kendisine atanan uygun kullanıcılar, rol tarafından verilen ayrıcalıkları kullanmak için yükseltilmelidir. Yükseltme işlemi, onay alma, çok faktörlü kimlik doğrulaması gerçekleştirme ve/veya neden etkinleştirildikleri bir nedeni sağlama de içerebilir. PıM ayrıca bildirimler ve PıM ve Azure AD denetim olay günlükleri aracılığıyla yükseltme gerçekleştirebilir.

PıM ile korunacak rolleri seçme zor olabilir ve her kuruluş için farklı olacaktır. Bu bölümde Azure AD ve Azure Kaynak rolleri için en iyi yöntem önerileri sunulmaktadır.

#### <a name="azure-ad-roles"></a>Azure AD rolleri

En fazla izin sayısı olan Azure AD rollerinin korunmasını önceliklendirmek önemlidir. PıM tarafından yönetilen ilk 10 Azure AD rolü olan PıM müşterileri arasındaki kullanım düzenlerine göre:

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
> :heavy_check_mark: **Microsoft** , güvenliği tehlikeye atıldığında en çok zararlı bir şey YAPABILECEKLERI için PIM 'yi kullanarak tüm genel yöneticilerinizi ve güvenlik yöneticilerini bir ilk adım olarak yönetmenizi önerir.

Kuruluşunuz için en önemli verileri ve izinleri göz önünde bulundurmanız önemlidir. Örnek olarak, bazı kuruluşlar, verilere erişme ve/veya çekirdek iş akışlarını değiştirme yeteneğine sahip olduklarından, Power BI yönetici rollerini veya bunların takımlar yönetici rollerini PıM kullanarak korumak isteyebilir.

Konuk kullanıcıların atandığı roller varsa, bunlar özellikle saldırılara açıktır.

> [!TIP]
> :heavy_check_mark: **Microsoft** , güvenliği aşılmış Konuk Kullanıcı hesaplarıyla ilişkili riski azaltmak için PIM kullanarak Konuk kullanıcıları olan tüm rolleri yönetmenizi önerir.

Dizin okuyucu, Ileti merkezi okuyucu ve güvenlik okuyucusu gibi okuyucu rolleri, bazen yazma iznine sahip olmadıkları sürece diğer rollere kıyasla daha az önem taşımaktadır. Ancak, bu hesaplara erişim elde etmiş saldırganlar, kişisel bilgiler (PII) gibi hassas verileri okuyabildiğinden, bazı müşterileri bu rolleri de koruduk. Kuruluşunuzdaki okuyucu rollerinin PıM kullanılarak yönetilmesi gerekip gerekmediğini saptarken bunu dikkate almanız gerekir.

#### <a name="azure-resource-roles"></a>Azure kaynağı rolleri

Azure kaynağı için PıM kullanılarak hangi rol atamalarının yönetilmesi gerektiğine karar verirken, öncelikle kuruluşunuz için en çok önem taşıyan abonelikleri/kaynakları belirlemeniz gerekir. Bu abonelik/kaynak örnekleri şunlardır:

- En hassas verileri barındıran kaynaklar
- Temel, müşteriye yönelik uygulamaların bağımlı olduğu kaynaklar

Genel bir yöneticiyseniz, hangi aboneliklerin/kaynakların en önemli olduğuna karar verirken, her abonelik tarafından yönetilen kaynakların bir listesini toplamak için kuruluşunuzdaki abonelik sahiplerine ulaşmanız gerekir. Daha sonra, kaynakları tehlikeye çıkarak önem düzeyine göre gruplamak için abonelik sahipleriyle birlikte çalışmanız gerekir (düşük, orta, yüksek). Bu önem düzeyine bağlı olarak, PıM ile kaynakları yönetmeyi önceliklendirmelisiniz.

> [!TIP]
> :heavy_check_mark: **Microsoft** , hassas abonelikler/kaynaklar içindeki tüm roller için PIM iş akışını ayarlamak üzere kritik hizmetlerin abonelik/kaynak sahipleri ile çalışmanızı önerir.

Azure kaynakları için PıM, zamana dayalı hizmet hesaplarını destekler. Hizmet hesaplarını, normal bir kullanıcı hesabını nasıl değerlendikiyle tam olarak aynı şekilde ele almanız gerekir.

Kritik olmayan abonelikler/kaynaklar için, tüm roller için PıM 'yi ayarlamanız gerekmez. Ancak, hala sahip ve Kullanıcı erişimi yönetici rollerini PıM ile korumanız gerekir.

> [!TIP]
> :heavy_check_mark: **Microsoft** , PIM kullanarak tüm aboneliklerin/kaynakların sahip rollerini ve Kullanıcı erişimi yönetici rollerini yönetmenizi önerir.

### <a name="decide-which-role-assignments-should-be-permanent-or-eligible"></a>Hangi rol atamalarının kalıcı veya uygun olacağını belirleyin

PıM tarafından yönetilecek rollerin listesini karardıktan sonra, hangi kullanıcıların uygun rolü ve kalıcı olarak etkin rolü alması gerektiğine karar vermelisiniz. Uygun roller yalnızca PıM 'de atanabileceği için, kalıcı olarak etkin roller Azure Active Directory ve Azure kaynakları aracılığıyla atanan normal rollerdir.

> [!TIP]
> :heavy_check_mark: **Microsoft** , hem Azure AD rolleri hem de Azure Kaynak rolleri için, kalıcı genel yöneticiye sahip olması gereken, önerilen [iki kesme camı acil durum erişim hesabı](../users-groups-roles/directory-emergency-access.md)dışında sıfır kalıcı etkin atama yapmanızı önerir rolü.

Yöneticinin azalmasına izin verdiğimiz halde, kuruluşların bu hakkı elde etmelerini bazen zorlaştırıyor. Bu kararı verirken göz önünde bulundurmanız gereken noktalar şunlardır:

- Yükselme sıklığı – Kullanıcı yalnızca ayrıcalıklı atamaya ihtiyaç duyuyorsa, kalıcı atamaya sahip olmamalıdır. Öte yandan, Kullanıcı kendi günlük işleri için role ihtiyaç duyuyorsa ve PıM kullanmak üretkenliği önemli ölçüde azalttıklarında, kalıcı rol için kabul edilebilir.
- Kuruluşunuza özel durumlar – uygun rolün verildiği kişi çok uzak bir takımdan veya yükseltme işlemini karşılayan ve zorlayan noktaya yönelik yüksek öncelikli bir yöneticiye ait ise kalıcı rol için kabul edilebilir.

> [!TIP]
> :heavy_check_mark: **Microsoft** , kalıcı rol atamaları olan kullanıcılar için yinelenen erişim İncelemeleri ayarlamanıza (herhangi birine sahip olmanız gerekir) önerir. Bu dağıtım planının son bölümünde yinelenen erişim incelemesi hakkında daha fazla bilgi edinin

### <a name="draft-your-pim-settings"></a>PıM ayarlarınızı taslağı

PıM çözümünüzü uygulamadan önce, kuruluşunuzun kullandığı her ayrıcalıklı rol için PıM ayarlarınızı taslağı yapmak iyi bir uygulamadır. Bu bölümde belirli roller için PıM ayarlarına bazı örnekler vardır (yalnızca başvuru amaçlıdır ve kuruluşunuz için farklı olabilir). Bu ayarların her biri, Microsoft 'un tablolarındaki önerilerle ayrıntılı olarak açıklanmıştır.

#### <a name="pim-settings-for-azure-ad-roles"></a>Azure AD rolleri için PıM ayarları

| Role | MFA gerektirme | Bildirim | Olay bileti | Onay iste | Onaylayan | Etkinleştirme süresi | Kalıcı yönetici |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Genel Yönetici | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | Diğer genel Yöneticiler | 1 Saat | Acil durum erişim hesapları |
| Exchange Yöneticisi | :heavy_check_mark: | :heavy_check_mark: | sayı | sayı | Yok. | 2 saat | Yok. |
| Yardım Masası Yöneticisi | sayı | sayı | :heavy_check_mark: | sayı | Yok. | 8 saat | Yok. |

#### <a name="pim-settings-for-azure-resource-roles"></a>Azure Kaynak rolleri için PıM ayarları

| Role | MFA gerektirme | Bildirim | Onay iste | Onaylayan | Etkinleştirme süresi | Etkin yönetici | Etkin süre sonu | Uygun süre sonu |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Kritik aboneliklerin sahibi | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | Aboneliğin diğer sahipleri | 1 Saat | Yok. | yok | 3 ay |
| Daha az kritik aboneliğin Kullanıcı erişimi Yöneticisi | :heavy_check_mark: | :heavy_check_mark: | sayı | Yok. | 1 Saat | Yok. | yok | 3 ay |
| Sanal Makine Katılımcısı | sayı | :heavy_check_mark: | sayı | Yok. | 3 saat | Yok. | yok | 6 ay |

Aşağıdaki tabloda ayarların her biri açıklanmaktadır.

| Ayar | Açıklama |
| --- | --- |
| Role | Ayarlarını tanımladığınız rolün adı. |
| MFA gerektirme | Rolü etkinleştirmeden önce uygun kullanıcının MFA gerçekleştirmesi gerekip gerekmediği.<br/><br/> :heavy_check_mark: Microsoft, özellikle de rollerin Konuk kullanıcıları varsa tüm yönetici rolleri için MFA 'yı zorunlu **kılmanızı önerir** . |
| Bildirim | True olarak ayarlanırsa, genel yönetici, ayrıcalıklı rol yöneticisi ve uygun bir kullanıcı rolü etkinleştirdiğinde kuruluştaki Güvenlik Yöneticisi bir e-posta bildirimi alır.<br/><br/>**Not:** Bazı kuruluşların yönetici hesaplarına bağlı bir e-posta adresi yoksa, bu e-posta bildirimlerini almak için, yöneticilerin bu e-postaları alabilmesi için alternatif bir e-posta adresi ayarlamanız gerekir. |
| Olay bileti | Uygun kullanıcının rolünü etkinleştirirken bir olay bileti numarası kaydetmesi gerekip gerekmediği. Bu ayar, istenmeyen etkinleştirmeleri azaltmak için bir kuruluşun iç olay numarasıyla her bir etkinleştirmeyi belirlemesine yardımcı olur.<br/><br/> :heavy_check_mark: Microsoft, iç sisteminizdeki PıM 'yi bağlamak için olay bilet numaralarının avantajlarından **faydalanmanızı önerir** . Bu, özellikle etkinleştirme için bağlam gerektiren onaylayanlar için yararlıdır. |
| Onay iste | Rolü etkinleştirmek için uygun kullanıcının onay alması gerekip gerekmediği.<br/><br/> :heavy_check_mark: **Microsoft** , en fazla izne sahip roller için onay ayarlamanızı önerir. Tüm PıM müşterileri, genel yönetici, Kullanıcı Yöneticisi, Exchange Yöneticisi, güvenlik yöneticisi ve parola Yöneticisi kullanım düzenlerini temel alan, onay kurulumu ile en yaygın rollerdir. |
| Onaylayan | Uygun rolü etkinleştirmek için onay gerekiyorsa, isteği onaylaması gereken kişileri listeleyin. Varsayılan olarak, PıM, onaylayanın kalıcı veya uygun olup olmadığına bakılmaksızın, ayrıcalıklı rol yöneticisi olan tüm kullanıcılar olarak ayarlanır.<br/><br/>**Not:** Bir kullanıcı hem bir Azure AD rolüne hem de rolün bir onaylayana uygun ise kendilerini onaylayamaz.<br/><br/> :heavy_check_mark: **Microsoft** , onaylayanları, belirli bir rol ve genel yönetici yerine sık kullananlar hakkında bilgi sahibi olacak şekilde seçmenizi önerir. |
| Etkinleştirme süresi | Kullanıcı, süresi dolmadan önce rolde etkinleştirilecek zaman uzunluğu. |
| Kalıcı yönetici | Rol için kalıcı yönetici olacak (hiçbir şekilde etkinleştirmesi gerekmez) kullanıcıların listesi.<br/><br/> :heavy_check_mark: **Microsoft** , genel Yöneticiler hariç tüm roller için sıfırdan fazla yönetici olmasını önerir. BT hakkında daha fazla bilgi edinmek isteyen kişiler ve bu planın kalıcı olarak etkin olması gerekir. |
| Etkin yönetici | Azure kaynakları için etkin yönetici, rolünü kullanmak için hiçbir şekilde etkinleştirmesi gereken kullanıcıların listesidir. Bu, kullanıcının bu rolü kaybedeceğinizi için bir sona erme saati ayarlayabildiğinden Azure AD rollerinde olduğu gibi kalıcı yönetici olarak kabul edilmez. |
| Etkin süre sonu | Azure Kaynak rollerinin etkin bir rol ataması, bu yapılandırma süresinden sonra sona eriyor. 15 gün, 1 ay, 3 ay, 6 ay, 1 yıl veya kalıcı olarak etkin seçeneklerinden birini belirleyebilirsiniz. |
| Uygun süre sonu | Bu yapılandırılan süre dolduktan sonra Azure Kaynak rolleri için uygun bir rol atamasının süresi dolacak. 15 gün, 1 ay, 3 ay, 6 ay, 1 yıl veya kalıcı olarak uygun bir seçim yapabilirsiniz. |

## <a name="step-3-implement-your-solution"></a>Adım 3. Çözümünüzü uygulama

Uygun planlama temeli, Azure Active Directory ile bir uygulamayı başarıyla dağıtabilmeniz için temeldir.  Başarılı dağıtımlar için zamanı azaltırken, ekleme işlemini kolaylaştıran akıllı güvenlik ve tümleştirme sağlar.  Bu birleşim, son kullanıcılarınız için zaman hafifletmek için uygulamanızın kolayca tümleştirilebilmesini sağlar.

### <a name="identify-test-users"></a>Test kullanıcılarını tanımla

Bu bölümü, uygulamayı doğrulamak üzere bir grup kullanıcıyı ve kullanıcı grubunu belirlemek için kullanın. Planlama bölümünde seçtiğiniz ayarlara bağlı olarak, her bir rol için test etmek istediğiniz kullanıcıları belirlersiniz.

> [!TIP]
> :heavy_check_mark: **Microsoft** , her BIR Azure AD rolünün hizmet sahiplerini test kullanıcıları olacak şekilde yapmanızı önerir ve bu sayede işleme yönelik bir iç advocator haline gelir.

Bu tabloda, her bir rolün ayarlarının çalıştığını doğrulayan test kullanıcılarını tanımla.

| Rol adı | Test kullanıcıları |
| --- | --- |
| &lt;Rol adı&gt; | &lt;Rolü test etmek için kullanıcılar&gt; |
| &lt;Rol adı&gt; | &lt;Rolü test etmek için kullanıcılar&gt; |

### <a name="test-implementation"></a>Test uygulama

Test kullanıcılarını tanımladığınıza göre, test kullanıcılarınız için PıM 'yi yapılandırmak üzere bu adımı kullanın. Kuruluşunuz, Azure portal içinde PıM 'nin Kullanıcı arabirimini kullanmak yerine, PıM iş akışını kendi iç uygulamanıza eklemek isterse, PıM 'deki tüm işlemler [Graph API 'si](https://docs.microsoft.com/graph/api/resources/privilegedidentitymanagement-root)aracılığıyla da desteklenir.

#### <a name="configure-pim-for-azure-ad-roles"></a>Azure AD rolleri için PıM 'yi yapılandırma

1. [Azure AD rol ayarlarını,](pim-how-to-change-default-settings.md) planladıklarınız temelinde yapılandırın.

1. **Azure AD rolleri**' ne gidin, **Roller**' e tıklayın ve ardından yeni yapılandırdığınız rolü seçin.

1. Test kullanıcıları grubu için, zaten kalıcı bir yöneticse, bunları arayarak ve satır üzerindeki üç noktaya tıklayarak bunları kalıcı hale getirerek kalıcı olarak dönüştürerek uygun hale getirebilirsiniz. Henüz rol atamaları yoksa, [yeni uygun bir atama](pim-how-to-add-role-to-user.md#make-a-user-eligible-for-a-role)yapabilirsiniz.

1. Test etmek istediğiniz tüm roller için 1-3 arasındaki adımları yineleyin.

1. Test kullanıcılarını ayarladıktan sonra, [Azure AD rolünü nasıl etkinleştireceğinize](pim-how-to-activate-role.md)ilişkin bağlantıyı göndermeniz gerekir.

#### <a name="configure-pim-for-azure-resource-roles"></a>Azure Kaynak rolleri için PıM 'yi yapılandırma

1. Test etmek istediğiniz bir abonelik veya kaynak içindeki bir rol için [Azure Kaynak rolü ayarlarını yapılandırın](pim-resource-roles-configure-role-settings.md) .

1. Bu abonelik için **Azure kaynakları** ' na gidin ve **Roller**' e tıklayın, yeni yapılandırdığınız rolü seçin.

1. Test kullanıcıları grubu için, zaten etkin bir yöneticse, bunları arayarak ve [rol atamasını güncelleyerek](pim-resource-roles-assign-roles.md#update-or-remove-an-existing-role-assignment)uygun hale getirebilirsiniz. Henüz rol yoksa, [Yeni bir rol atayabilirsiniz](pim-resource-roles-assign-roles.md#assign-a-role).

1. Test etmek istediğiniz tüm roller için 1-3 arasındaki adımları yineleyin.

1. Test kullanıcılarını ayarladıktan sonra, [Azure Kaynak rolünü nasıl etkinleştireceğinize](pim-resource-roles-activate-your-roles.md)ilişkin bağlantıyı göndermeniz gerekir.

Roller için ayarladığınız tüm yapılandırmanın doğru çalışıp çalışmadığını doğrulamak için bu aşamayı kullanmanız gerekir. Testlerinizi belgelemek için aşağıdaki tabloyu kullanın. Ayrıca, etkilenen kullanıcılarla iletişimi iyileştirmek için bu aşamayı kullanmanız gerekir.

| Role | Etkinleştirme sırasında beklenen davranış | Gerçek sonuçlar |
| --- | --- | --- |
| Genel Yönetici | (1) MFA gerektir<br/>(2) onay gerektir<br/>(3) onaylayan, bildirimi alır ve onaylayabilir<br/>(4) rolün ön ayar zamanından sonra süresi doluyor |  |
| *X* Aboneliğinin sahibi | (1) MFA gerektir<br/>(2) uygun atama, yapılandırılan süre dolduktan sonra sona erecek |  |

### <a name="communicate-pim-to-affected-stakeholders"></a>PıM 'yi etkilenen hissedarlara iletişim kurma

PıM dağıtımı, ayrıcalıklı rol kullanıcıları için ek adımlar getirir. PıM, ayrıcalıklı kimliklerle ilişkili güvenlik sorunlarını büyük ölçüde azaltsa da, değişikliğin kiracı genelinde dağıtımdan önce etkili bir şekilde iletilmesi gerekir. Etkilenen yöneticilerin sayısına bağlı olarak kuruluşlar genellikle dahili bir belge, bir video veya değişiklik hakkında bir e-posta oluşturmayı tercih etmesidir. Bu iletişimlere genellikle şunlar dahildir:

- PıM nedir?
- Kuruluşun avantajı nedir?
- Kimler etkilenecek
- Ne zaman PıM alınacaktır
- Kullanıcıların rolünü etkinleştirmesi için hangi ek adımların gerekli olacağı
    - Belgelerimize bağlantı göndermeniz gerekir:
    - [Azure AD rollerini etkinleştirme](pim-how-to-activate-role.md)
    - [Azure Kaynak rollerini etkinleştirin](pim-resource-roles-activate-your-roles.md)
- PıM ile ilgili herhangi bir sorun için iletişim bilgileri veya Yardım Masası bağlantısı

> [!TIP]
> :heavy_check_mark: **Microsoft** , yardım masasına/destek EKIBINIZE, PIM iş akışı aracılığıyla (kuruluşunuzun BIR dahili BT destek ekibi varsa) yol ayarlamanıza olanak sağlar. Bunlara uygun belge ve iletişim bilgilerinizi sağlayın.

### <a name="move-to-production"></a>Üretime taşıma

Testiniz tamamlandıktan ve başarılı olduktan sonra, PıM yapılandırmanızda tanımladığınız her rolün tüm kullanıcıları için test aşamalardaki tüm adımları yineleyerek PıM 'yi üretime taşıyın. Azure AD rollerine yönelik PıM için, kuruluşlar test etmeden ve diğer roller için PıM 'yi kullanıma almadan önce genel Yöneticiler için PıM 'yi test ediyor ve kullanıma alıyor. Azure kaynağı için, kuruluşlar normalde PıM One Azure aboneliğini aynı anda test ve kullanıma sunırlar.

### <a name="in-the-case-a-rollback-is-needed"></a>Geri alma gerekli olduğunda

PıM, üretim ortamında istenen şekilde çalışamadı, PıM 'yi ayarlamadan önce aşağıdaki geri alma adımları bilinen iyi duruma geri dönmenize yardımcı olabilir:

#### <a name="azure-ad-roles"></a>Azure AD rolleri

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. **Azure AD Privileged Identity Management**açın.
1. **Azure AD rolleri** ' ne tıklayın ve ardından **Roller**' e tıklayın.
1. Yapılandırdığınız her bir rol için uygun atamaya sahip tüm kullanıcılar için üç nokta ( **...** ) simgesine tıklayın.
1. Rol atamasını kalıcı hale getirmek için **kalıcı yap** seçeneğine tıklayın.

#### <a name="azure-resource-roles"></a>Azure kaynağı rolleri

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. **Azure AD Privileged Identity Management**açın.
1. **Azure kaynakları** ' na tıklayın ve ardından geri almak istediğiniz bir aboneliğe veya kaynağa tıklayın.
1. **Roller**' e tıklayın.
1. Yapılandırdığınız her bir rol için uygun atamaya sahip tüm kullanıcılar için üç nokta ( **...** ) simgesine tıklayın.
1. Rol atamasını kalıcı hale getirmek için **kalıcı yap** seçeneğine tıklayın.

## <a name="step-4-next-steps-after-deploying-pim"></a>4\. adımı. PıM 'yi dağıttıktan sonra sonraki adımlar

PıM 'nin üretimde başarıyla dağıtımı, kuruluşunuzun ayrıcalıklı kimliklerinin güvenli hale getirilmesi açısından önemli bir adımdır. PıM dağıtımı ile, güvenlik ve uyumluluk için kullanmanız gereken ek PıM özellikleri gelir.

### <a name="use-pim-alerts-to-safeguard-your-privileged-access"></a>Ayrıcalıklı erişiminizi korumak için PıM uyarıları kullanın

Kiracınızı daha iyi korumak için PıM 'nin yerleşik uyarı işlevlerini kullanmanız gerekir. Daha fazla bilgi için bkz. [güvenlik uyarıları](pim-how-to-configure-security-alerts.md#security-alerts). Bu uyarılar şunları içerir: Yöneticiler ayrıcalıklı roller kullanmıyor, roller PıM dışında atanıyor, roller çok sık etkinleştiriliyor ve daha fazla. Kuruluşunuzu tam olarak korumak için, uyarı listenizde düzenli olarak gidip sorunları düzelmelisiniz. Uyarılarınızı aşağıdaki şekilde görüntüleyebilir ve çözebilirsiniz:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. **Azure AD Privileged Identity Management**açın.
1. **Azure AD rolleri** ' ne tıklayın ve ardından **Uyarılar**' a tıklayın.

> [!TIP]
> :heavy_check_mark: **Microsoft** , yüksek önem derecesine sahip olarak işaretlenen tüm uyarılarla doğrudan işlem yapmanızı önerir. Orta ve düşük önem derecesine sahip uyarılar için, bir güvenlik tehdidi olduğunu düşündüğünüz takdirde bilgilendirilmesi ve değişiklikler yapmanız gerekir.

Belirli uyarılardan herhangi biri yararlı değilse veya kuruluşunuz için uygulanmadığından, uyarıları her zaman Uyarılar sayfasında kapatabilirsiniz. Bu ayırtıcı 'yi her zaman Azure AD ayarları sayfasında geri döndürebilirsiniz.

### <a name="set-up-recurring-access-reviews-to-regularly-audit-your-organizations-privileged-identities"></a>Kuruluşunuzun ayrıcalıklı kimliklerini düzenli olarak denetlemek için yinelenen erişim İncelemeleri ayarlayın

Erişim gözden geçirmeleri, kullanıcıların ayrıcalıklı rollere veya belirli gözden geçirenlere atanmasını, her kullanıcının ayrıcalıklı kimliğe ihtiyacı olup olmadığını sormanız için en iyi yoldur. Saldırı yüzeyini azaltmak ve uyumlu kalmak istiyorsanız erişim gözden geçirmeleri harika. Erişim gözden geçirmesi başlatma hakkında daha fazla bilgi için bkz. [Azure AD rolleri erişim İncelemeleri](pim-how-to-start-security-review.md) ve [Azure Kaynak rolleri erişim İncelemeleri](pim-resource-roles-start-access-review.md). Bazı kuruluşlarda, bazı kuruluşlar için düzenli erişim incelemesi gerçekleştirmek gerekir, çünkü diğer bir deyişle, diğer bir deyişle, diğer bir deyişle, kuruluşunuz genelinde en az ayrıcalık sorumlusunu zorlamak için en iyi yoldur.

> [!TIP]
> :heavy_check_mark: **Microsoft** , tüm Azure AD ve Azure Kaynak rolleriniz için üç aylık erişim gözden geçirmeleri ayarlamanıza önerilir.

Çoğu durumda, Azure AD rolleri için gözden geçiren, Azure Kaynak rolleri gözden geçireni, rolün bulunduğu aboneliğin sahibidir. Ancak, çoğu zaman şirketlerin belirli bir kişinin e-posta adresiyle bağlantılı olmayan ayrıcalıklı hesaplara sahip olduğu durumdur. Bu durumlarda, hiç kimse okuma ve erişimi İnceleme.

> [!TIP]
> :heavy_check_mark: **Microsoft** , düzenli olarak denetlenen bir e-posta adresiyle bağlantılı olmayan ayrıcalıklı rol atamalarına sahip tüm hesaplar için ikincil bir e-posta adresi eklemenizi önerir

### <a name="get-the-most-out-of-your-audit-log-to-improve-security-and-compliance"></a>Güvenlik ve uyumluluğu geliştirmek için denetim günlüğünden en iyi şekilde yararlanın

Denetim günlüğü, güncel kalabileceğiniz ve yönetmeliklerle uyumlu olabilecek yerdir. PıM Şu anda, kendi denetim günlüğü içindeki tüm kuruluşunuzun geçmişiyle ilgili 30 günlük bir geçmişi depolar; örneğin:

- Uygun rollerin etkinleştirilmesi/devre dışı bırakılması
- PıM 'nin içinde ve dışında rol atama etkinlikleri
- Rol ayarlarındaki değişiklikler
- Onay kurulumu ile rol etkinleştirme için etkinlikleri isteme/onaylama/reddetme
- Uyarılara Güncelleştir

Genel yönetici veya ayrıcalıklı rol yöneticisiyseniz bu denetim günlüklerine erişebilirsiniz. Daha fazla bilgi için bkz. Azure [ad rolleri için denetim geçmişi](pim-how-to-use-audit-log.md) ve [Azure Kaynak rolleri için denetim geçmişi](azure-pim-resource-rbac.md).

> [!TIP]
> :heavy_check_mark: Microsoft, en az bir yöneticinin tüm denetim olaylarını haftalık olarak okumasını ve denetim olaylarınızı aylık olarak dışarı **aktarmanızı önerir** .

Denetim olaylarınızı daha uzun bir süre için otomatik olarak depolamak istiyorsanız, PıM 'nin denetim günlüğü otomatik olarak [Azure AD denetim günlükleriyle](../reports-monitoring/concept-audit-logs.md)eşitlenir.

> [!TIP]
> :heavy_check_mark: **Microsoft** , Azure depolama hesabındaki denetim olaylarını güvenlik ve uyumluluk açısından arşivlemek için [Azure günlük izlemeyi](../reports-monitoring/concept-activity-logs-azure-monitor.md) ayarlamanızı önerir.
