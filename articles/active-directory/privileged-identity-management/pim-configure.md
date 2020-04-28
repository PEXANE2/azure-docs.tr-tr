---
title: Ayrıcalıklı Kimlik Yönetimi Nedir? - Azure REKLAM | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'ne (PIM) genel bakış sağlar.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: pim
ms.topic: overview
ms.date: 04/21/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: bfb0cc61b61328df86c27498a1007f2372fb9548
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81867440"
---
# <a name="what-is-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management nedir?

Azure Etkin Dizin (Azure AD) Ayrıcalıklı Kimlik Yönetimi (PIM), kuruluşunuzdaki önemli kaynaklara erişimi yönetmenize, denetlemenize ve izlemenize olanak tanıyan bir hizmettir. Bu kaynaklar, Azure AD, Azure ve Office 365 veya Microsoft Intune gibi diğer Microsoft Çevrimiçi Hizmetleri'ndeki kaynakları içerir.

## <a name="reasons-to-use"></a>Kullanım nedenleri

Kuruluşlar, kötü amaçlı bir aktörün bu erişimi alma olasılığını azalttığı veya yetkili bir kullanıcının hassas bir kaynağı yanlışlıkla etkilemesi nedeniyle güvenli bilgilere veya kaynaklara erişimi olan kişi sayısını en aza indirmek ister. Ancak kullanıcıların Azure AD, Azure, Office 365 veya SaaS uygulamalarında yine de ayrıcalıklı işlemler gerçekleştirmesi gerekir. Kuruluşlar kullanıcılara Azure kaynaklarına ve Azure AD'ye tam zamanında (JIT) ayrıcalıklı erişim verebilir. Bu kullanıcıların yönetici ayrıcalıklarıyla ne yaptıkları için gözetim eihtiyacı vardır.

## <a name="what-does-it-do"></a>Ne işe yarıyor?

Ayrıcalıklı Kimlik Yönetimi, önem verdiğiniz kaynaklardaki aşırı, gereksiz veya kötüye erişim izinlerinin risklerini azaltmak için zamana dayalı ve onay tabanlı rol etkinleştirme sağlar. Ayrıcalıklı Kimlik Yönetimi'nin bazı temel özellikleri şunlardır:

- Azure AD ve Azure kaynaklarına **tam zamanında** ayrıcalıklı erişim sağlayın
- Başlangıç ve bitiş tarihlerini kullanarak kaynaklara **zamana bağlı** erişim atama
- Ayrıcalıklı rolleri etkinleştirmek için **onay** gerektirme
- Herhangi bir rolü etkinleştirmek için **çok faktörlü kimlik doğrulamayı** zorla
- Kullanıcıların neden etkinleştirmelerini anlamak için **yaslama** yı kullanma
- Ayrıcalıklı roller etkinleştirildiğinde **bildirim alma**
- Kullanıcıların hala rollere ihtiyaç duyduğundan emin olmak için **erişim incelemeleri** gerçekleştirin
- İç veya dış denetim için **denetim geçmişini** indirin

## <a name="what-can-i-do-with-it"></a>Onunla ne yapabilirim?

Ayrıcalıklı Kimlik Yönetimi'ni ayarladıktan sonra, sol daki gezinme menüsünde **Görevler**, **Yönet**ve **Etkinlik** seçeneklerini görürsünüz. Yönetici olarak, **Azure AD rollerini** yönetmekle **Azure kaynak** rolleri arasında seçim yapabilirsiniz. Yönetilecek rol türünü seçtiğinizde, bu rol türü için benzer bir seçenek kümesi görürsünüz.

![Azure portalında Ayrıcalıklı Kimlik Yönetimi ekran görüntüsü](./media/pim-configure/pim-quickstart.png)

## <a name="who-can-do-what"></a>Kim ne yapabilir?

Ayrıcalıklı Kimlik Yönetimi'ndeki Azure AD rolleri için, yalnızca Ayrıcalıklı rol yöneticisi veya Genel yönetici rolünde olan bir kullanıcı diğer yöneticilerin atamalarını yönetebilir. Ayrıcalıklı [Kimlik Yönetimi'ni yönetmek için diğer yöneticilere erişim izni](pim-how-to-give-access-to-pim.md)verebilirsiniz. Genel Yöneticiler, Güvenlik Yöneticileri, Genel Okuyucular ve Güvenlik Okuyucuları, Ayrıcalıklı Kimlik Yönetimi'ndeki Azure AD rollerine atamaları da görüntüleyebilir.

Ayrıcalıklı Kimlik Yönetimi'ndeki Azure kaynak rolleri için, yalnızca bir abonelik yöneticisi, kaynak Sahibi veya kaynak Kullanıcı Erişimi yöneticisi diğer yöneticilerin atamalarını yönetebilir. Ayrıcalıklı Rol Yöneticileri, Güvenlik Yöneticileri veya Güvenlik Okuyucuları olan kullanıcılar varsayılan olarak Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rollerindeki atamaları görüntüleme ye erişemez.

## <a name="scenarios"></a>Senaryolar

Privileged Identity Management aşağıdaki senaryoları destekler:

### <a name="privileged-role-administrator-permissions"></a>Ayrıcalıklı Rol yöneticisi izinleri

- Belirli roller için onay etkinleştirmek
- İstekleri onaylamak için onaylayıcı kullanıcıları veya grupları belirtin
- Tüm ayrıcalıklı roller için istek ve onay geçmişini görüntülemek

### <a name="approver-permissions"></a>Onaylayıcı izinleri

- Bekleyen onayları (istekler) görüntülemek
- Rol yüksekliği isteklerini onaylama veya reddetme (tek ve toplu)
- Onayım veya reddetmem için gerekçe sağlayın

### <a name="eligible-role-user-permissions"></a>Uygun rol kullanıcı izinleri

- Onay gerektiren bir rolün etkinleştirilmesini istemek
- Etkinleştirme isteğinizin durumunu görüntülemek
- İstek onaylanmışsa Azure AD'deki görevinizi tamamlamak

## <a name="terminology"></a>Terminoloji

Ayrıcalıklı Kimlik Yönetimi'ni ve belgelerini daha iyi anlamak için aşağıdaki koşulları gözden geçirmelisiniz.

| Terim veya kavram | Rol atama kategorisi | Açıklama |
| --- | --- | --- |
| Uygun | Tür | Bir kullanıcının rolü kullanmak için bir veya daha fazla eylemi gerçekleştirmesini gerektiren bir rol ataması. Bir kullanıcı bir rol için uygun hale getirilmişse, bu, ayrıcalıklı görevleri gerçekleştirmeleri gerektiğinde rolü etkinleştirebileceği anlamına gelir. Kalıcı ve uygun bir rol ataması olan birine verilen erişimde bir fark yoktur. Tek fark, bazı insanların her zaman bu erişim gerekmez. |
| Etkin | Tür | Bir kullanıcının rolü kullanmak için herhangi bir eylem gerçekleştirmesini gerektirmeyen bir rol ataması. Etkin olarak atanan kullanıcılar, role atanan ayrıcalıklara sahiptir. |
| etkinleştirme |  | Bir kullanıcının uygun olduğu bir rolü kullanmak için bir veya daha fazla eylemi gerçekleştirme işlemi. Eylemler, çok faktörlü bir kimlik doğrulama (MFA) denetimi gerçekleştirmeyi, bir iş gerekçesi sağlamayı veya atanan onaylayıcılardan onay istemeyi içerebilir. |
| Atanan | Durum | Etkin bir rol ataması olan bir kullanıcı. |
| Aktif | Durum | Uygun bir rol ataması olan, rolü etkinleştirmek için eylemleri gerçekleştiren ve şimdi etkin olan bir kullanıcı.  Etkinleştirildikten sonra, kullanıcı rolü yeniden etkinleştirmeden önce önceden yapılandırılmış bir zaman dilimi için kullanabilir. |
| kalıcı uygun | Süre | Bir kullanıcının rolü etkinleştirmeye her zaman uygun olduğu bir rol ataması. |
| kalıcı aktif | Süre | Kullanıcının herhangi bir eylem gerçekleştirmeden rolü her zaman kullanabileceği bir rol ataması. |
| uygun süresi dolmak | Süre | Kullanıcının rolü belirli bir başlangıç ve bitiş tarihinde etkinleştirmeye uygun olduğu bir rol ataması. |
| etkin sona erer | Süre | Kullanıcının belirli bir başlangıç ve bitiş tarihinde herhangi bir eylem gerçekleştirmeden rolü kullanabileceği rol ataması. |
| tam zamanında (JIT) erişim |  | Kullanıcıların ayrıcalıklı görevleri gerçekleştirmek için geçici izinler aldığı ve kötü amaçlı veya yetkisiz kullanıcıların izinlerin süresi dolduktan sonra erişim elde etmesini engelleyen bir model. Erişim yalnızca kullanıcıların ihtiyacı olduğunda verilir. |
| en az ayrıcalık erişim ilkesi |  | Her kullanıcıya, gerçekleştirmeye yetkili oldukları görevleri gerçekleştirmek için gereken minimum ayrıcalıkların sağlandığı önerilen güvenlik uygulaması. Bu uygulama, Genel Yönetici sayısını en aza indirir ve bunun yerine belirli senaryolar için belirli yönetici rolleri kullanır. |

## <a name="license-requirements"></a>Lisans gereksinimleri

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

Kullanıcılar ait lisanslar hakkında daha fazla bilgi için, [Ayrıcalıklı Kimlik Yönetimi'ni kullanmak için Lisans gereksinimlerine](subscription-requirements.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetimi'ni kullanmak için lisans gereksinimleri](subscription-requirements.md)
- [Azure AD'de karma ve bulut dağıtımları için ayrıcalıklı erişim güvenliğini sağlama](../users-groups-roles/directory-admin-roles-secure.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json)
- [Privileged Identity Management'ı dağıtma](pim-deployment-plan.md)
