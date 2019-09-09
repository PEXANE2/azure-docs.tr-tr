---
title: Privileged Identity Management nedir? -Azure Active Directory | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) için genel bir bakış sağlar.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: pim
ms.topic: overview
ms.date: 04/09/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5009310dd4ea4e99e1da2871085ee2d2d99a6eab
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804512"
---
# <a name="what-is-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management nedir?

Azure Active Directory (Azure AD) Privileged Identity Management (PıM), kuruluşunuzdaki önemli kaynaklara erişimi yönetmenizi, denetlemenizi ve izlemenizi sağlayan bir hizmettir. Azure AD, Azure kaynakları ve Office 365 veya Microsoft Intune gibi diğer Microsoft Online Services hizmetlerindeki kaynaklara erişim de bu kapsama dahildir.

## <a name="why-should-i-use-pim"></a>PıM 'yi neden kullanmalıyım?

Kuruluşlar, kötü niyetli bir aktörün bu erişimi veya yetkili bir kullanıcının yanlışlıkla hassas bir kaynağı etkileme olasılığını azalttığından, güvenli bilgi veya kaynaklara erişimi olan kişilerin sayısını en aza indirmek ister. Ancak kullanıcıların Azure AD, Azure, Office 365 veya SaaS uygulamalarında yine de ayrıcalıklı işlemler gerçekleştirmesi gerekir. Kuruluşlar, kullanıcıların Azure kaynaklarına ve Azure AD 'ye tam zamanında (JıT) ayrıcalıklı erişim izni verebilir. Bu kullanıcıların yönetici ayrıcalıklarıyla ne yaptığına ilişkin daha fazla bakış için bir gereksinim vardır. PıM, aşırı, gereksiz veya yanlış erişimli erişim haklarının riskini azaltmaya yardımcı olur.

## <a name="what-can-i-do-with-pim"></a>PıM ile ne yapabilirim?

PıM temel bilgileri kim, ne, ne zaman, nerede ve ilgilendiğiniz kaynakların nedenini yönetmenize yardımcı olur. PıM 'nin temel özelliklerinden bazıları şunlardır:

- Azure AD ve Azure kaynaklarına **tam zamanında** ayrıcalıklı erişim sağlama
- Başlangıç ve bitiş tarihlerini kullanarak kaynaklara **zamana bağlıysa** erişim atama
- Ayrıcalıklı rolleri etkinleştirmek için **onay** gerektir
- Herhangi bir rolü etkinleştirmek için **Multi-Factor Authentication** 'ı zorlayın
- Kullanıcıların neden etkinleştireceğinize anlamak için **gerekçe** kullanın
- Ayrıcalıklı roller etkinleştirildiğinde **bildirim** alın
- Kullanıcıların hala rollere ihtiyacı olduğundan emin olmak için **erişim gözden geçirmeleri** gerçekleştirin
- İç veya dış denetim için **denetim geçmişini** indirin

## <a name="terminology"></a>Terminoloji

PıM ve belgelerini daha iyi anlamak için aşağıdaki koşulları gözden geçirmeniz gerekir.

| Kavram veya sözleşme | Rol atama kategorisi | Açıklama |
| --- | --- | --- |
| lebilecek | Type | Bir kullanıcının rolü kullanmak için bir veya daha fazla eylem gerçekleştirmesini gerektiren bir rol ataması. Bir Kullanıcı bir rol için uygun hale getirildiğinde, ayrıcalıklı görevleri gerçekleştirmeleri gerektiğinde rolü etkinleştirebilecekleri anlamına gelir. Kalıcı ve uygun bir rol atamasına karşılık gelen bir kişiye verilen erişimde fark yoktur. Tek fark, bazı kişilerin her zaman erişim gereksinimi yoktur. |
| etkin | Type | Rolü kullanmak için bir kullanıcının herhangi bir eylem gerçekleştirmesini gerektirmeyen bir rol ataması. Etkin olarak atanan kullanıcılara role atanmış ayrıcalıklar vardır. |
| etkinleştir |  | Kullanıcının uygun olduğu bir rolü kullanmak için bir veya daha fazla eylem gerçekleştirme işlemi. Eylemler, bir Multi-Factor Authentication (MFA) denetimi gerçekleştirmeye, iş gerekçesinin sağlanmasından veya belirlenen onaylayanlardan onay isteğinde bulunabilir. |
| atanan | State | Etkin rol ataması olan bir kullanıcı. |
| etkinleştirilemediğini | State | Uygun rol atamasına sahip bir Kullanıcı, rolü etkinleştirmek için eylemleri gerçekleştirmiştir ve artık etkindir.  Etkinleştirildikten sonra, Kullanıcı yeniden etkinleştirilmeleri için önceden yapılandırılmış bir süre için rolü kullanabilir. |
| kalıcı uygun | Duration | Kullanıcının her zaman rolü etkinleştirmesine uygun olduğu rol ataması. |
| kalıcı etkin | Duration | Bir kullanıcının herhangi bir eylem yapmadan her zaman rol kullanabileceği rol ataması. |
| kullanım süresi uygun | Duration | Bir kullanıcının belirtilen başlangıç ve bitiş tarihi içinde rolü etkinleştirmesine uygun olduğu rol ataması. |
| etkin süre sonu | Duration | Bir kullanıcının belirtilen bir başlangıç ve bitiş tarihi içinde herhangi bir eylem gerçekleştirmeden rolü kullanabileceği rol ataması. |
| tam zamanında (JıT) erişim |  | Kullanıcıların ayrıcalıklı görevleri gerçekleştirmek için geçici izinleri aldığı, kötü amaçlı veya yetkisiz kullanıcıların izinlerin süre dolduktan sonra erişim kazanmasını önleyen bir model. Erişim yalnızca kullanıcılara ihtiyaç duymadan verilir. |
| en az ayrıcalık erişimi ilkesi |  | Her kullanıcının yalnızca, gerçekleştirme yetkisine sahip oldukları görevleri gerçekleştirmek için gereken en düşük ayrıcalıklarla sağlandığı önerilen bir güvenlik uygulaması. Bu uygulama, genel yönetici sayısını en aza indirir ve bunun yerine belirli senaryolar için belirli yönetici rollerini kullanır. |

## <a name="what-does-pim-look-like"></a>PıM nasıl görünür?

PıM 'yi ayarladıktan sonra sol gezinti menüsünde **Görevler**, **Yönet**ve **etkinlik** seçeneklerini görürsünüz. Yönetici olarak, **Azure AD rollerini** ve **Azure Kaynak** rollerini yönetme arasında seçim yapabilirsiniz. Yönetilecek rol türünü seçtiğinizde, bu rol türü için benzer bir seçenek kümesi görürsünüz.

![Azure portal PıM 'nin ekran görüntüsü](./media/pim-configure/pim-overview.png)

## <a name="who-can-do-what-in-pim"></a>PıM 'de kimler yapabilir?

PıM 'yi kullanmak için ilk kişiyseniz, otomatik olarak dizinde [Güvenlik Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#security-administrator) ve [ayrıcalıklı rol yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rolleri atanır.

Azure AD rolleri için, yalnızca ayrıcalıklı rol yöneticisi rolünde olan bir Kullanıcı, PıM 'deki diğer yöneticiler için atamaları yönetebilir. PıM 'yi [yönetmek için diğer yöneticilere erişim izni](pim-how-to-give-access-to-pim.md)verebilirsiniz. Genel Yöneticiler, güvenlik yöneticileri ve güvenlik okuyucuları, PıM 'de Azure AD rollerine yönelik atamaları görüntüleyebilir.

Azure Kaynak rolleri için, yalnızca bir abonelik Yöneticisi, bir kaynak sahibi veya bir kaynak Kullanıcı erişimi Yöneticisi, PıM 'deki diğer yöneticiler için atamaları yönetebilir. Ayrıcalıklı rol yöneticileri, güvenlik yöneticileri veya güvenlik okuyucuları olan kullanıcılar varsayılan olarak PıM 'de Azure Kaynak rollerine atamaları görüntüleme erişimine sahip değildir.

## <a name="scenarios"></a>Senaryolar

PıM aşağıdaki senaryoları destekler:

**Ayrıcalıklı Rol Yöneticisi olarak şunları yapabilirsiniz:**

- Belirli roller için onay etkinleştirmek
- İstekleri onaylayacak onaylayan kullanıcıları ve/veya grupları belirlemek
- Tüm ayrıcalıklı roller için istek ve onay geçmişini görüntülemek

**Bir onaylayan olarak şunları yapabilirsiniz:**

- Bekleyen onayları (istekler) görüntülemek
- Rol yükseltme (tek ve/veya toplu) isteklerini onaylamak veya reddetmek
- Onay/red için gerekçe sağlamak 

**Uygun bir rol kullanıcısı olarak şunları yapabilirsiniz:**

- Onay gerektiren bir rolün etkinleştirilmesini istemek
- Etkinleştirme isteğinizin durumunu görüntülemek
- İstek onaylanmışsa Azure AD'deki görevinizi tamamlamak

## <a name="license-requirements"></a>Lisans gereksinimleri

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

Kullanıcılar için lisanslar hakkında daha fazla bilgi için bkz. [PIM 'yi kullanmak Için lisans gereksinimleri](subscription-requirements.md).

## <a name="next-steps"></a>Sonraki adımlar

- [PıM 'yi kullanmak için lisans gereksinimleri](subscription-requirements.md)
- [Azure AD'de karma ve bulut dağıtımları için ayrıcalıklı erişim güvenliğini sağlama](../users-groups-roles/directory-admin-roles-secure.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json)
- [PıM dağıtma](pim-deployment-plan.md)
