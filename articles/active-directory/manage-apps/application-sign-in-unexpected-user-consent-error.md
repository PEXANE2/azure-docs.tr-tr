---
title: Bir uygulamaya onay gerçekleştirilirken beklenmeyen hata oluştu | Microsoft Docs
description: Bir uygulamaya yönelik yarışmaya yönelik işlemler sırasında oluşabilecek ve bunlarla ilgili neler yapabileceğiniz hataları açıklar
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: ea14e02920cf7ba6c5e0a7b415cb92137c915576
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80519700"
---
# <a name="unexpected-error-when-performing-consent-to-an-application"></a>Bir uygulamaya onay gerçekleştirilirken beklenmeyen hata oluştu

Bu makalede, bir uygulamaya yarışmaya yönelik işlem sırasında oluşabilecek hatalar ele alınmaktadır. Hata iletileri içermeyen beklenmedik onay istemlerinde sorun yaşıyorsanız, bkz. [Azure AD Için kimlik doğrulama senaryoları](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios).

Azure Active Directory ile tümleştirilen birçok uygulama, çalışması için diğer kaynaklara erişim izinleri gerektirir. Bu kaynaklar aynı zamanda Azure Active Directory ile tümleştirildiğinde, bunlara erişim izinleri genellikle ortak onay çerçevesi kullanılarak istenir. Genellikle uygulamanın ilk kez kullanıldığı, ancak uygulamanın sonraki kullanımıyla da gerçekleşebileceği bir onay istemi görüntülenir.

Bir kullanıcının bir uygulamanın gerektirdiği izinleri onaylaması için belirli koşulların doğru olması gerekir. Bu koşullar karşılanmazsa aşağıdaki hatalar meydana gelebilir.

## <a name="requesting-not-authorized-permissions-error"></a>Yetkilendirilmemiş izinleri isteme hatası
* **AADSTS90093:** &lt;clientappdisplayname&gt; , izin vermek için yetkiniz olmayan bir veya daha fazla izin istiyor. Sizin adınıza bu uygulamayı kabul edebilen bir yöneticiye başvurun.
* **AADSTS90094:** &lt;clientappdisplayname&gt; , kuruluşunuzda yalnızca bir yöneticinin verebileceği kaynaklara erişmek için izne sahip olmalıdır. Kullanabilmek için önce lütfen yöneticiden bu uygulamaya izin vermesini isteyin.

Bu hata, şirket yöneticisi olmayan bir Kullanıcı, yalnızca bir yöneticinin verebileceği izinleri isteyen bir uygulamayı kullanmayı denediğinde oluşur. Bu hata, kuruluş adına uygulamaya erişim izni veren bir yönetici tarafından çözülebilir.

Bu hata, Microsoft 'un izin isteğinin riskli olduğunu algılaması nedeniyle, bir kullanıcının bir uygulamaya katılmasının engellenmesinin önlenmesi durumunda da oluşabilir. Bu durumda, bir denetim olayı "ApplicationManagement" kategorisi, etkinlik türü "uygulamaya onay onayı" ve "riskli uygulama algılandı" durum nedeni ile de günlüğe kaydedilir.

## <a name="policy-prevents-granting-permissions-error"></a>İlke, izin verilmesini engelliyor
* **AADSTS90093:** Bir &lt;tenantdisplayname&gt; Yöneticisi, istenen izinleri uygulamanın &lt;&gt; adına vermemenizi önleyen bir ilke ayarladı. Bu uygulamaya sizin adınıza &lt;izin veren bir&gt;tenantdisplayname yöneticisiyle iletişim kurun.

Bu hata, bir şirket yöneticisi kullanıcıların uygulamalara onay vermesini kapattığında, yönetici olmayan bir kullanıcı izin gerektiren bir uygulamayı kullanmayı denediğinde oluşur. Bu hata, kuruluş adına uygulamaya erişim izni veren bir yönetici tarafından çözülebilir.

## <a name="intermittent-problem-error"></a>Aralıklı sorun hatası
* **AADSTS90090:** Oturum açma işlemi, clientappdisplayname &lt;&gt;'e vermeye çalıştığınız izinleri kaydederek aralıklı bir sorunla karşılaştı gibi görünüyor. daha sonra yeniden deneyin.

Bu hata, aralıklı bir hizmet tarafı sorununun oluştuğunu gösterir. Uygulamayı yeniden doğrulamaya çalışırken çözülebilir.

## <a name="resource-not-available-error"></a>Kaynak kullanılamıyor hatası
* **AADSTS65005:** Uygulama &lt;, kullanılamayan bir kaynak&gt; &lt;resourceappdisplayname&gt; 'e erişmek için istenen izinleri istedi. 

Uygulama geliştiricisine başvurun.

##  <a name="resource-not-available-in-tenant-error"></a>Kaynak kiracı hatası içinde kullanılamıyor
* **AADSTS65005:** &lt;clientappdisplayname&gt; , kuruluşunuzda &lt;kullanılan bir kaynak&gt; &lt;resourceappdisplayname&gt; 'e erişim istiyor. 

Bu kaynağın kullanılabilir olduğundan emin olun veya bir &lt;tenantdisplayname&gt;Yöneticisi ile iletişime geçin.

## <a name="permissions-mismatch-error"></a>İzin uyumsuzluğu hatası
* **AADSTS65005:** Uygulama, kaynak &lt;resourceappdisplayname&gt;'e erişmek için izin istedi. Uygulama kaydı sırasında uygulamanın önceden nasıl yapılandırıldığına eşleşmediğinden bu istek başarısız oldu. Uygulama satıcısına başvurun. * *

Bu hatalar, bir Kullanıcı, bir kullanıcının, kuruluşun dizininde (kiracı) bulunamayan bir kaynak uygulamasına erişmek için izin isteme girişimi olduğunda oluşur. Bu durum çeşitli nedenlerden kaynaklanabilir:

-   İstemci uygulama geliştiricisi, uygulamasını yanlış olarak yapılandırmıştır ve bu da geçersiz bir kaynağa erişim istemesine neden olur. Bu durumda, uygulama geliştiricisi bu sorunu çözmek için istemci uygulamasının yapılandırmasını güncelleştirmelidir.

-   Hedef kaynak uygulamasını temsil eden bir hizmet sorumlusu kuruluşta yok veya geçmişte var, ancak kaldırıldı. Bu sorunu çözmek için, istemci uygulamasının bu uygulamaya izin isteyebilmesi için kaynak uygulamaya yönelik bir hizmet sorumlusu kuruluşta sağlanmalıdır. Hizmet sorumlusu, uygulamanın türüne bağlı olarak çeşitli yollarla sağlanabilir:

    -   Kaynak uygulama (Microsoft tarafından yayımlanan uygulamalar) için bir abonelik edinme

    -   Kaynak uygulamasına yarışmaya

    -   Uygulama izinlerini Azure portal aracılığıyla verme

    -   Azure AD uygulama galerisinden uygulamayı ekleme

## <a name="next-steps"></a>Sonraki adımlar 

[Azure Active Directory (v1 uç noktası) üzerinde uygulamalar, izinler ve onay](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)<br>

[Azure Active Directory (v 2.0 uç noktası) kapsamlar, izinler ve onay](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


