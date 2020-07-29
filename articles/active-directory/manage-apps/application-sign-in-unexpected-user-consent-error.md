---
title: Bir uygulamaya onay gerçekleştirilirken beklenmeyen hata oluştu | Microsoft Docs
description: Bir uygulamaya yönelik yarışmaya yönelik işlemler sırasında oluşabilecek ve bunlarla ilgili neler yapabileceğiniz hataları açıklar
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 07/11/2017
ms.author: kenwith
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: e2a7709cf0522727257025b2dddc495b20fe8448
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84763763"
---
# <a name="unexpected-error-when-performing-consent-to-an-application"></a>Bir uygulamaya onay gerçekleştirilirken beklenmeyen hata oluştu

Bu makalede, bir uygulamaya yarışmaya yönelik işlem sırasında oluşabilecek hatalar ele alınmaktadır. Hata iletileri içermeyen beklenmedik onay istemlerinde sorun yaşıyorsanız, bkz. [Azure AD Için kimlik doğrulama senaryoları](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios).

Azure Active Directory ile tümleştirilen birçok uygulama, çalışması için diğer kaynaklara erişim izinleri gerektirir. Bu kaynaklar aynı zamanda Azure Active Directory ile tümleştirildiğinde, bunlara erişim izinleri genellikle ortak onay çerçevesi kullanılarak istenir. Genellikle uygulamanın ilk kez kullanıldığı, ancak uygulamanın sonraki kullanımıyla da gerçekleşebileceği bir onay istemi görüntülenir.

Bir kullanıcının bir uygulamanın gerektirdiği izinleri onaylaması için belirli koşulların doğru olması gerekir. Bu koşullar karşılanmazsa aşağıdaki hatalar meydana gelebilir.

## <a name="requesting-not-authorized-permissions-error"></a>Yetkilendirilmemiş izinleri isteme hatası
* **AADSTS90093:** &lt; clientAppDisplayName &gt; , izin vermek için yetkiniz olmayan bir veya daha fazla izin istiyor. Sizin adınıza bu uygulamayı kabul edebilen bir yöneticiye başvurun.
* **AADSTS90094:** &lt; clientAppDisplayName &gt; , kuruluşunuzda yalnızca bir yöneticinin verebileceği kaynaklara erişmek için izne sahip olmalıdır. Kullanabilmek için önce lütfen yöneticiden bu uygulamaya izin vermesini isteyin.

Bu hata, şirket yöneticisi olmayan bir Kullanıcı, yalnızca bir yöneticinin verebileceği izinleri isteyen bir uygulamayı kullanmayı denediğinde oluşur. Bu hata, kuruluş adına uygulamaya erişim izni veren bir yönetici tarafından çözülebilir.

Bu hata, Microsoft 'un izin isteğinin riskli olduğunu algılaması nedeniyle, bir kullanıcının bir uygulamaya katılmasının engellenmesinin önlenmesi durumunda da oluşabilir. Bu durumda, bir denetim olayı "ApplicationManagement" kategorisi, etkinlik türü "uygulamaya onay onayı" ve "riskli uygulama algılandı" durum nedeni ile de günlüğe kaydedilir.

## <a name="policy-prevents-granting-permissions-error"></a>İlke, izin verilmesini engelliyor
* **AADSTS90093:** Bir &lt; tenantdisplayname Yöneticisi, &gt; &lt; istenen izinleri uygulamanın adına vermemenizi önleyen bir ilke ayarladı &gt; . &lt; &gt; Bu uygulamaya sizin adınıza izin veren bir tenantdisplayname yöneticisiyle iletişim kurun.

Bu hata, bir şirket yöneticisi kullanıcıların uygulamalara onay vermesini kapattığında, yönetici olmayan bir kullanıcı izin gerektiren bir uygulamayı kullanmayı denediğinde oluşur. Bu hata, kuruluş adına uygulamaya erişim izni veren bir yönetici tarafından çözülebilir.

## <a name="intermittent-problem-error"></a>Aralıklı sorun hatası
* **AADSTS90090:** Oturum açma işlemi, clientappdisplayname 'e vermeye çalıştığınız izinleri kaydederek aralıklı bir sorunla karşılaştı gibi görünüyor &lt; &gt; . daha sonra yeniden deneyin.

Bu hata, aralıklı bir hizmet tarafı sorununun oluştuğunu gösterir. Uygulamayı yeniden doğrulamaya çalışırken çözülebilir.

## <a name="resource-not-available-error"></a>Kaynak kullanılamıyor hatası
* **AADSTS65005:** Uygulama, &lt; &gt; kullanılamayan bir kaynak resourceappdisplayname 'e erişmek için istenen izinleri istedi &lt; &gt; . 

Uygulama geliştiricisine başvurun.

##  <a name="resource-not-available-in-tenant-error"></a>Kaynak kiracı hatası içinde kullanılamıyor
* **AADSTS65005:** &lt; clientAppDisplayName &gt; , &lt; kuruluşunuzda kullanılan bir kaynak resourceappdisplayname 'e erişim istiyor &gt; &lt; &gt; . 

Bu kaynağın kullanılabilir olduğundan emin olun veya bir tenantdisplayname Yöneticisi ile iletişime geçin &lt; &gt; .

## <a name="permissions-mismatch-error"></a>İzin uyumsuzluğu hatası
* **AADSTS65005:** Uygulama, kaynak resourceappdisplayname 'e erişmek için izin istedi &lt; &gt; . Uygulama kaydı sırasında uygulamanın önceden nasıl yapılandırıldığına eşleşmediğinden bu istek başarısız oldu. Uygulama satıcısına başvurun. * *

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


