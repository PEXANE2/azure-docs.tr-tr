---
title: Yönetici rolü temsilcisini anlama-Azure Active Directory | Microsoft Docs
description: Azure Active Directory içinde temsilciliğini, örnekleri ve rol güvenliğini devretme
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: conceptual
ms.date: 04/29/2020
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 556871b55de60ee592622f9f93c7b7123ba2ed32
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84731979"
---
# <a name="delegate-administration-in-azure-active-directory"></a>Azure Active Directory 'de temsilci yönetimi

Kuruluş büyümesi karmaşıklık sunar. Yaygın olarak kullanılan bir yanıt, Azure Active Directory (AD) yönetici rolleriyle erişim yönetimi iş yükünün bazılarını azaltmaktır. Kullanıcılara uygulamalarına erişmek ve görevlerini gerçekleştirmek için mümkün olan en düşük ayrıcalıkları atayabilirsiniz. Her uygulama sahibine genel yönetici rolünü atamasanız bile, uygulama yönetimi sorumluluklarını mevcut küresel yöneticilere yerleştirmezsiniz. Bir kuruluşun daha ayrıntılı bir yönetime doğru hareket eden pek çok nedeni vardır. Bu makale, kuruluşunuzda temsili için plan yapmanıza yardımcı olabilir.

<!--What about reporting? Who has which role and how do I audit?-->

## <a name="centralized-versus-delegated-permissions"></a>Merkezi ve temsilci izinleri

Bir kuruluş büyüdükçe, hangi kullanıcıların belirli yönetici rollerine sahip olduğunu izlemek zor olabilir. Bir çalışan yönetici haklarına sahip değilse, kuruluşunuz güvenlik ihlallerine karşı daha savunmasız olabilir. Genellikle, kaç yönetici destekliyoruz ve izinlerinin ne kadar ayrıntılı olması, dağıtımınızın boyutuna ve karmaşıklığına bağlıdır.

* Küçük veya kavram kanıtı olan dağıtımlarda, bir veya birkaç yönetici her şeyi ister. temsili yok. Bu durumda, her bir yöneticiyi genel yönetici rolüyle oluşturun.
* Daha fazla makine, uygulama ve Masaüstü bulunan daha büyük dağıtımlarda, daha fazla temsili gerekir. Birçok yönetici daha belirli işlevsel sorumluluklara (roller) sahip olabilir. Örneğin, bazıları ayrıcalıklı kimlik yöneticileri olabilir ve diğerleri uygulama yöneticileri olabilir. Ayrıca, bir yönetici yalnızca cihazlar gibi belirli nesne gruplarını yönetebilir.
* Daha büyük dağıtımlar bile daha ayrıntılı izinler gerektirebilir ve büyük olasılıkla geleneksel olmayan veya karma rollere sahip yöneticilerdir.

Azure AD portalında, [herhangi bir rolün tüm üyelerini görüntüleyebilir](directory-manage-roles-portal.md), bu da dağıtım ve temsilci izinleriniz üzerinde hızlı bir şekilde denetim sağlamanıza yardımcı olabilir.

Azure AD 'de Yönetim erişimi yerine Azure kaynaklarına erişim yetkisini aktarmaya ilgileniyorsanız, bkz. [rol tabanlı erişim denetimi (RBAC) rolü atama](../../role-based-access-control/role-assignments-portal.md).

## <a name="delegation-planning"></a>Temsili planlama

Gereksinimlerinize uygun bir temsilcilik modeli geliştirmek için çalışır. Bir temsilciyi geliştirme modeli, yinelemeli bir tasarım işlemidir ve aşağıdaki adımları izlemenizi öneririz:

* İhtiyacınız olan rolleri tanımlayın
* Uygulama yönetimi temsilcisi
* Uygulamaları kaydetme yeteneği verme
* Uygulama sahipliğini devretmek
* Güvenlik planı geliştirme
* Acil durum hesapları oluşturma
* Yönetici rollerinizin güvenliğini sağlama
* Ayrıcalıklı yükseltmeyi geçici yap

## <a name="define-roles"></a>Rolleri tanımlama

Yöneticiler tarafından yürütülen Active Directory görevlerini ve bunların rollerle nasıl eşlendiğini belirleme. [Ayrıntılı rol açıklamalarını Azure Portal görüntüleyebilirsiniz](directory-manage-roles-portal.md) .

Her görev sıklık, önem ve zorluk açısından değerlendirilmelidir. Bu ölçütler, görev tanımının önemli yönlerinden biri, bir iznin temsilci olarak oluşturulup oluşturulmayacağını yönettiğinden,

* Düzenli olarak yaptığınız görevler, sınırlı risk taşır ve tamamen tamamlanmak için çok iyi bir adaydır.
* Nadiren yaptığınız, ancak kuruluş genelinde harika bir etkiye sahip olan ve yetkilendirmeden önce yüksek yetenek seviyelerinin çok dikkatli bir şekilde düşünülmesini gerektiren görevler. Bunun yerine, [bir hesabı gerekli rolüne geçici olarak yükseltebilir](../active-directory-privileged-identity-management-configure.md) veya görevi yeniden atayabilirsiniz.

## <a name="delegate-app-administration"></a>Uygulama yönetimi temsilcisi

Kuruluşunuzdaki uygulamaların kullanımı, temsilcilerinizi zorlayıp temin edebilir. Uygulama erişim yönetimi için genel yöneticiye yük yerleştiriyor, büyük olasılıkla modelin zaman içindeki yükü artar. Şirket uygulamalarını yapılandırma gibi kullanıcılara genel yönetici rolü verdiyseniz, artık bunları daha düşük ayrıcalıklı rollere devretebilmeniz gerekir. Bunun yapılması güvenlik duruşunuzu iyileştirmenize yardımcı olur ve olası talihsiz hataları azaltır. En ayrıcalıklı uygulama yöneticisi rolleri şunlardır:

* Kayıtlar, çoklu oturum açma ayarları, Kullanıcı ve Grup atamaları ve lisanslama, uygulama proxy ayarları ve onay dahil olmak üzere dizindeki tüm uygulamaları yönetme olanağı veren **Uygulama Yöneticisi** rolü. Koşullu erişimi yönetme yeteneği vermez.
* Uygulama yöneticisinin tüm yeteneklerini sağlayan **bulut uygulama Yöneticisi** rolü, uygulama proxy 'si ayarlarına erişim izni verilmediği durumlar (Şirket içi izni olmadığı için).

## <a name="delegate-app-registration"></a>Uygulama kaydını devretmek

Varsayılan olarak, tüm kullanıcılar uygulama kayıtları oluşturabilir. Uygulama kayıtları oluşturma özelliğini seçmeli olarak vermek için:

* Kullanıcıları, **Kullanıcı ayarlarında** Hayır 'a **uygulama kaydedebilir**
* Kullanıcıyı uygulama geliştirici rolüne atama

Bir uygulamanın verilere erişmesine izin verme özelliğini seçmeli olarak vermek için:

* Kullanıcıları, şirket verilerine **Kullanıcı ayarlarında** Hayır olarak **erişen uygulamalara izin verebilir**
* Kullanıcıyı uygulama geliştirici rolüne atama

Bir uygulama geliştiricisi yeni bir uygulama kaydı oluşturduğunda, bunlar otomatik olarak ilk sahip olarak eklenir.

## <a name="delegate-app-ownership"></a>Uygulama sahipliğini devretmek

Daha da ayrıntılı uygulama erişimi temsilciliğini sağlamak için, tek tek kurumsal uygulamalara sahiplik atayabilirsiniz. Bu, uygulama kayıt sahiplerini atamaya yönelik mevcut desteği tamamlar. Sahiplik, kurumsal uygulamalar dikey penceresinde kurumsal bir uygulamaya göre atanır. Bu avantajlara sahip oldukları kurumsal uygulamaları yönetebilir. Örneğin, Salesforce uygulaması için bir sahip atayabilirsiniz ve bu sahip, Salesforce ve diğer uygulamalar için erişimi yönetebilir. Bir kurumsal uygulama birçok sahibe sahip olabilir ve bir kullanıcı birçok kurumsal uygulama için sahip olabilir. İki uygulama sahibi rolü vardır:

* **Kurumsal uygulama sahibi** rolü, kullanıcının sahip olduğu kurumsal uygulamaları, çoklu oturum açma ayarları, Kullanıcı ve Grup atamaları ve ek sahipler ekleme gibi yönetme olanağı verir. Uygulama proxy 'Si ayarlarını veya koşullu erişimi yönetme yeteneği vermez.
* Uygulama **kayıt sahibi** rolü, uygulama bildirimi ve ek sahipler ekleme dahil olmak üzere, kullanıcının sahip olduğu uygulama için uygulama kayıtlarını yönetme olanağı verir.

## <a name="develop-a-security-plan"></a>Güvenlik planı geliştirme

Azure AD, Azure AD yönetici rolleriniz üzerinde bir güvenlik planı planlamak ve yürütmek için kapsamlı bir kılavuz sağlar ve [karma ve bulut dağıtımları için ayrıcalıklı erişimin güvenliğini](directory-admin-roles-secure.md)sağlar.

## <a name="establish-emergency-accounts"></a>Acil durum hesapları oluşturma

Sorun ortaya çıkarsa kimlik yönetimi deponuza erişimi sürdürmek için acil durum erişim hesaplarını [acil durum erişimi yönetim hesapları oluşturmaya](directory-emergency-access.md)göre hazırlayın.

## <a name="secure-your-administrator-roles"></a>Yönetici rollerinizin güvenliğini sağlama

Ayrıcalıklı hesapların denetimini alan saldırganlar büyük ölçüde zarar verebilir, bu nedenle, varsayılan olarak tüm Azure AD kuruluşları (genel önizlemede) olarak kullanılabilen [temel erişim ilkesini](https://cloudblogs.microsoft.com/enterprisemobility/2018/06/22/baseline-security-policy-for-azure-ad-admin-accounts-in-public-preview/) kullanarak bu hesapları önce koruyun. İlke, ayrıcalıklı Azure AD hesaplarında Multi-Factor Authentication 'ı zorlar. Aşağıdaki Azure AD rolleri, Azure AD temel ilkesi tarafından ele alınmıştır:

* Genel yönetici
* SharePoint yöneticisi
* Exchange yöneticisi
* Koşullu Erişim Yöneticisi
* Güvenlik yöneticisi

## <a name="elevate-privilege-temporarily"></a>Ayrıcalığı geçici olarak yükselt

Çoğu günlük etkinlikler için, tüm kullanıcılar genel yönetici haklarına ihtiyaç duymamalıdır ve bunların tümünün genel yönetici rolüne kalıcı olarak atanmamalıdır. Kullanıcılar genel yönetici izinlerine ihtiyaç duyduklarında, Azure AD [Privileged Identity Management](../active-directory-privileged-identity-management-configure.md) 'de rol atamasını kendi hesaplarına veya alternatif bir yönetim hesabına etkinleştirmeleri gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD rol açıklamalarına yönelik bir başvuru için bkz. [Azure AD 'de yönetici rolleri atama](directory-assign-admin-roles.md)
