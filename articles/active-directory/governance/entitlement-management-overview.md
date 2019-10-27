---
title: Azure AD yetkilendirme yönetimi nedir? (Önizleme)-Azure Active Directory
description: Azure Active Directory yetkilendirme yönetimine genel bakış ve bu bilgileri, iç ve dış kullanıcılar için gruplara, uygulamalara ve SharePoint Online sitelerine erişimi yönetmek üzere nasıl kullanabileceğinizi öğrenin.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 09/03/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 136a9994415b42c456ebdb0caa8ed6edcc7b4534
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72934369"
---
# <a name="what-is-azure-ad-entitlement-management-preview"></a>Azure AD yetkilendirme yönetimi nedir? (Önizleme)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) yetkilendirme yönetimi şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Kuruluşlardaki çalışanların işlerini gerçekleştirmek için çeşitli gruplar, uygulamalar ve sitelere erişmesi gerekir. Bu erişimin yönetilmesi zorlayıcı bir şekilde yapılır. Çoğu durumda, bir kullanıcının proje için ihtiyacı olan tüm kaynakların düzenlenmiş bir listesi yoktur. Proje Yöneticisi, gereken kaynakları, söz konusu kişileri ve projenin son ne kadar süreyle daha iyi şekilde öğreniyor. Ancak, proje yöneticisinin genellikle başkalarına erişimi onaylama veya verme izinleri yoktur. Bu senaryo, dış bireyler veya şirketlerle çalışmayı denediğinizde daha karmaşıktır.

Azure Active Directory (Azure AD) yetkilendirme yönetimi, iç kullanıcılar ve ayrıca kuruluşunuzun dışındaki kullanıcılar için gruplara, uygulamalara ve SharePoint Online sitelerine erişimi yönetmenize yardımcı olabilir.

Bu videoda, yetkilendirme yönetimine ve iş değerine genel bir bakış sunulmaktadır:

>[!VIDEO https://www.youtube.com/embed/_Lss6bFrnQ8]

## <a name="why-use-entitlement-management"></a>Yetkilendirme Yönetimi neden kullanılmalıdır?

Kurumsal kuruluşlar, şu gibi kaynaklara erişimi yönetirken genellikle güçlüklere sahiptir:

- Kullanıcılar, sahip olmaları gereken erişimi bilmiyor olabilir
- Kullanıcılar, doğru bireyler veya doğru kaynakları bulmada zorluk gösterebilir
- Kullanıcılar bir kaynağa erişim bulup aldıktan sonra, iş amaçları için gerekenden daha uzun bir süre erişim için açık kalabilir

Bu sorunlar, tedarik zinciri kuruluşlarından veya diğer iş ortaklarından gelen harici kullanıcılar gibi başka bir dizinden erişmesi gereken kullanıcılar için bir bileşim oluşturur. Örnek:

- Kuruluşlar, diğer dizinlerdeki tüm kişilerin bunları davet edebilmeleri için bu dizinlerin tümünü bilmiyor olabilir
- Kuruluşlar bu kullanıcıları davet edebilse bile, kuruluşlar tüm Kullanıcı erişimini sürekli olarak yönetmeyi anımsamayabilir

Azure AD Yetkilendirme Yönetimi, bu zorlukları ele almanıza yardımcı olabilir.

## <a name="what-can-i-do-with-entitlement-management"></a>Yetkilendirme yönetimiyle ne yapabilirim?

Yetkilendirme yönetiminin bazı özellikleri şunlardır:

- Kullanıcıların isteyeceği ilgili kaynakların paketlerini oluşturun
- Kaynakların nasıl isteneceğini ve erişimin süresinin dolacağını belirleyen kuralları tanımlayın
- Hem iç hem de dış kullanıcılar için erişim yaşam döngüsünü yönetir
- Kaynak yönetimine temsilci seçme
- İstekleri onaylamak için onaylayanları belirleyin
- Geçmişi izlemek için rapor oluşturma

Identity Idare ve yetkilendirme yönetimine genel bir bakış için, Ignite 2018 konferansında aşağıdaki videoyu izleyin:

>[!VIDEO https://www.youtube.com/embed/aY7A0Br8u5M]

## <a name="what-resources-can-i-manage"></a>Hangi kaynakları yönetebilirim?

Yetkilendirme yönetimiyle erişimi yönetebileceğiniz kaynak türleri şunlardır:

- Azure AD güvenlik grupları
- Office 365 Grupları
- SaaS uygulaması ve Federasyon veya sağlamayı destekleyen özel tümleşik uygulamalar dahil olmak üzere Azure AD kurumsal uygulamaları
- SharePoint Online site koleksiyonları ve siteleri

Ayrıca, Azure AD güvenlik gruplarına veya Office 365 gruplarına bağlı diğer kaynaklara erişimi de denetleyebilirsiniz.  Örnek:

- Erişim paketindeki bir Azure AD güvenlik grubunu kullanarak ve bu grup için [grup tabanlı lisanslamayı](../users-groups-roles/licensing-groups-assign.md) yapılandırarak, Microsoft Office 365 için kullanıcılara lisans verebilirsiniz
- Erişim paketindeki bir Azure AD güvenlik grubunu kullanarak ve bu grup için bir [Azure rol ataması](../../role-based-access-control/role-assignments-portal.md) oluşturarak kullanıcılara Azure kaynaklarını yönetmeye yönelik erişim izni verebilirsiniz.

## <a name="what-are-access-packages-and-policies"></a>Erişim paketleri ve ilkeler nelerdir?

Yetkilendirme Yönetimi, bir *erişim paketi*kavramını tanıtır. Erişim paketi, kullanıcının bir projede çalışması veya işini gerçekleştirmesi için gereken tüm kaynakların bir paketidir. Kaynaklar, gruplara, uygulamalara veya sitelere erişimi içerir. Erişim paketleri, dahili çalışanlarınızın ve ayrıca kuruluşunuzun dışındaki kullanıcıların erişimini yönetmek için kullanılır. Erişim paketleri, *kataloglar*olarak adlandırılan kapsayıcılar içinde tanımlanır.

Erişim paketleri bir veya daha fazla *ilke*de içerir. Bir ilke, erişim paketine erişmek için kuralları veya guardrayları tanımlar. Bir ilkeyi etkinleştirmek, yalnızca doğru kullanıcılara, doğru kaynaklara ve doğru süre için erişim izni verilmesini zorunlu tutar.

![Paket ve ilkelere erişim](./media/entitlement-management-overview/elm-overview-access-package.png)

Bir erişim paketiyle ve ilkelerine sahip olan erişim paketi Yöneticisi şunları tanımlar:

- Kaynaklar
- Kullanıcıların kaynaklar için ihtiyaç duyduğu roller
- Erişim istemek için uygun olan dış kullanıcıların iç kullanıcıları ve iş ortağı kuruluşları
- Onay işlemi ve erişimi onaylayabilen veya reddeden kullanıcılar
- Kullanıcının erişim süresi

Aşağıdaki diyagramda, yetkilendirme yönetiminde farklı öğelerin bir örneği gösterilmektedir. İki örnek erişim paketi gösterir.

- **Erişim paketi 1** , kaynak olarak tek bir grup içerir. Erişim, dizinde erişim isteğinde bulunan bir kullanıcı kümesini sağlayan bir ilkeyle tanımlanır.
- **Erişim paketi 2** , kaynak olarak bir grup, uygulama ve SharePoint Online sitesi içerir. Erişim iki farklı ilke ile tanımlanır. İlk ilke, dizinde erişim istemek için dizindeki bir kullanıcı kümesini sağlar. İkinci ilke, bir dış dizindeki kullanıcıların erişim istemesine olanak sağlar.

![Yetkilendirme yönetimine genel bakış](./media/entitlement-management-overview/elm-overview.png)

## <a name="terminology"></a>Terminoloji

Yetkilendirme yönetimini ve belgelerini daha iyi anlamak için aşağıdaki koşulları gözden geçirmeniz gerekir.

| Terim veya kavram | Açıklama |
| --- | --- |
| Yetkilendirme Yönetimi | Erişim paketlerini atayan, iptal eden ve yöneten bir hizmet. |
| erişim paketi | Bir ekibin veya projenin ihtiyaç duyacağı ve ilkelerle ilişkilendirilen kaynak demeti. Bir erişim paketi her zaman bir katalogda bulunur. |
| erişim isteği | Erişim paketindeki kaynaklara erişim isteği. İstek genellikle bir iş akışından geçer. |
| ilke | Kullanıcıların erişim alma, kimlerin ne kadar süreceğine ve kullanıcıların ne kadar süreceğine ilişkin erişim yaşam döngüsünü tanımlayan bir kurallar kümesi. Örnek ilkeler, çalışan erişimini ve dış erişimi içerir. |
| kataloglarını | İlgili kaynakların ve erişim paketlerinin kapsayıcısı. |
| Genel Katalog | Her zaman kullanılabilir olan yerleşik bir katalog. Genel kataloğa kaynak eklemek için belirli izinler gerekir. |
| Kaynak | Bir kullanıcıya izin verilebilecek bir varlık veya hizmet (örneğin, bir Office grubu, güvenlik grubu, uygulama veya SharePoint Online sitesi). |
| Kaynak türü | Gruplar, uygulamalar ve SharePoint Online siteleri içeren kaynak türü. |
| Kaynak rolü | Bir kaynakla ilişkili izinler koleksiyonu. |
| Kaynak dizini | Paylaşılacak bir veya daha fazla kaynağı olan bir dizin. |
| bağlı kuruluş | İle ilişkiniz olan bir dış Azure AD dizini veya etki alanı. |
| atanan kullanıcılar | Kullanıcının bu erişim paketinin tüm kaynak rollerine sahip olması için bir erişim paketinin kullanıcıya atanması. |
| etkinleştirebilir | Kullanıcılara istemek için bir erişim paketi oluşturma işlemi. |

## <a name="license-requirements"></a>Lisans gereksinimleri

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

Azure Kamu, Azure Almanya ve Azure Çin 21Vianet gibi özel bulutlar Şu anda bu önizlemede kullanıma sunulmamaktadır.

### <a name="which-users-must-have-licenses"></a>Hangi kullanıcıların Lisansı olmalıdır?

Kiracınızda, etkin üye kullanıcılarınız olduğu için en az sayıda Azure AD Premium P2 Lisansı olmalıdır. Yetkilendirme yönetiminde etkin üye kullanıcıları şunları içerir:

- Bir erişim paketi için isteği başlatan veya onaylayan bir kullanıcı.
- Erişim paketi atanan bir kullanıcı. 
- Erişim paketlerini yöneten bir kullanıcı.

Üye kullanıcılara yönelik lisansların bir parçası olarak, bir dizi Konuk kullanıcının yetkilendirme yönetimiyle etkileşime girmesine de izin verebilirsiniz. Dahil edebilirsiniz konuk kullanıcıların sayısını hesaplama hakkında daha fazla bilgi için bkz. [Azure ACTIVE DIRECTORY B2B işbirliği Lisanslama Kılavuzu](../b2b/licensing-guidance.md).

Kullanıcılarınıza lisansları atama hakkında daha fazla bilgi için, bkz. [Azure Active Directory portalını kullanarak lisans atama veya kaldırma](../fundamentals/license-users-groups.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: ilk erişim paketinizi oluşturma](entitlement-management-access-package-first.md)
- [Genel senaryolar](entitlement-management-scenarios.md)
