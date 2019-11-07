---
title: Azure portal eğitim kılavuzunda Uygulama kayıtları-Azure
description: Microsoft Identity platformunda yeni uygulama kayıt deneyimine giriş.
services: active-directory
documentationcenter: ''
author: archieag
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: aragra
ms.reviewer: lenalepa, keyam
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: a539859fb4853467863f3fd2ab4144bcb789b9f5
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73662495"
---
# <a name="training-guide-app-registrations-in-the-azure-portal"></a>Eğitim Kılavuzu: Azure portal Uygulama kayıtları

Azure portal yeni [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyiminde birçok geliştirme bulabilirsiniz. Eski deneyimle daha fazla bilginiz varsa, yeni deneyimi kullanmaya başlamanızı sağlamak için bu eğitim kılavuzunu kullanın.

Azure Active Directory, burada açıklanan yeni uygulama kayıt deneyimi genel kullanıma sunulmuştur (GA). Azure Active Directory B2C (Azure AD B2C) ' de bu deneyim önizlemededir.

## <a name="key-changes"></a>Anahtar değişiklikleri

- Uygulama kayıtları, bir **Web uygulaması/API 'si** ya da **Yerel** bir uygulama ile sınırlı değildir. İlgili yeniden yönlendirme URI 'Lerini kaydederek bunların hepsi için aynı uygulama kaydını kullanabilirsiniz.
- Eski deneyim yalnızca kuruluş (Azure AD) hesaplarında oturum açmak için desteklenen uygulamalar. Uygulamalar tek kiracılı olarak kaydedildi (yalnızca uygulamanın kaydedildiği dizinden kurumsal hesapları destekleme) ve çok kiracılı (tüm kuruluş hesaplarını destekleme) olarak değiştirilebilir. Yeni deneyim, bu seçeneklerin yanı sıra üçüncü bir seçenek de destekleyebilen uygulamalar kaydetmenizi sağlar: tüm kurumsal hesaplar ve kişisel Microsoft hesapları.
- Eski deneyim yalnızca kurumsal hesap kullanılarak Azure portal oturum açmışsa kullanılabilir. Yeni deneyimle, bir dizinle ilişkilendirilmemiş kişisel Microsoft hesaplarını kullanabilirsiniz.

## <a name="list-of-applications"></a>Uygulama listesi

- Yeni uygulama listesi, [uygulama kayıt portalı](https://apps.dev.microsoft.com/) (oturum açma uygulamaları) ile kaydedilen uygulamaların yanı sıra Azure Portal (Azure AD hesaplarında oturum açarken kullanılan uygulamalar) üzerinde eski uygulama kayıtları deneyimi aracılığıyla kaydedilmiş uygulamaları gösterir. Azure AD ve kişisel Microsoft hesapları).
- Yeni uygulama listesinin bir **uygulama türü** sütunu yoktur (tek bir uygulama kaydı birçok tür olabilir) ve iki ek sütuna sahiptir: **oluşturulan** bir sütun ve **Sertifikalar &** durumu gösteren bir gizli dizi sütunu. (geçerli, uygulamada kayıtlı olan kimlik bilgilerinin süresi dolmak üzere veya süresi doluyor.

## <a name="new-app-registration"></a>Yeni uygulama kaydı

Eski deneyimde, sağlamanız gereken bir uygulamayı kaydetmek için: **ad**, **uygulama türü**ve **oturum açma URL 'SI/yeniden yönlendirme URI 'si**. Oluşturulan uygulamalar yalnızca Azure AD 'nin yalnızca uygulamanın kaydedildiği dizinden kurumsal hesapları destekledikleri anlamına gelir.

Yeni deneyimde uygulama için bir **ad** belirtmeniz ve **Desteklenen hesap türlerini**seçmeniz gerekir. İsteğe bağlı olarak bir **yeniden yönlendirme URI 'si**sağlayabilirsiniz. Yeniden yönlendirme URI 'SI sağlarsanız, bunun Web/genel (mobil ve Masaüstü) olup olmadığını belirtmeniz gerekir. Yeni uygulama kaydı deneyimini kullanarak bir uygulamayı kaydetme hakkında daha fazla bilgi için bkz. [Microsoft Identity platformu ile uygulama kaydetme](quickstart-register-app.md). Azure AD B2C için bkz. [bir uygulamayı Azure Active Directory B2C kaydetme](../../active-directory-b2c/tutorial-register-applications.md).

## <a name="the-legacy-properties-page"></a>Eski Özellikler sayfası

Eski deneyimde yeni deneyimin sahip olmadığı bir **Özellikler** sayfası vardı. **Özellikler** dikey penceresinde şu alanlar vardı: **ad**, **nesne kimliği**, **uygulama kimliği**, **uygulama kimliği URI 'si**, **logo**, **giriş sayfası URL 'si**, **oturum kapatma URL**'si, **hizmet koşulları URL 'si**, **Gizlilik bildirimi URL**, **uygulama türü**ve **çok kiracılı.**

İşte bu yeni deneyimde eşdeğer işlevselliği bulabilirsiniz:

- **Ad**, **logo**, **giriş sayfası URL**'si, **hizmet koşulları URL 'si**ve **Gizlilik bildirimi URL 'si** artık uygulamanın **marka** sayfasıdır.
- **Nesne kimliği** ve **uygulama (Istemci) kimliği** **genel bakış** sayfasıdır.
- Eski deneyimdeki **çok kiracılı** geçiş tarafından denetlenen Işlevsellik, **kimlik doğrulama** sayfasındaki **Desteklenen hesap türleriyle** değiştirilmiştir. Birden çok kiracının desteklenen hesap türü seçenekleriyle nasıl eşleştiği hakkında daha fazla bilgi için [Bu hızlı başlangıç](quickstart-modify-supported-accounts.md)bölümüne bakın.
- **Oturum kapatma URL 'si** Şu anda **kimlik doğrulama** sayfasıdır.
- **Uygulama türü** artık geçerli bir alan değil. Bunun yerine, hangi uygulama türlerinin desteklendiğini yeniden yönlendirme URI 'Leri ( **kimlik doğrulama** sayfasında bulabilirsiniz) belirlenir.
- Uygulama **KIMLIĞI URI** 'Si artık **uygulama kimliği URI 'si** olarak adlandırılmaktadır ve bunu **bir API 'yi kullanıma** sunma dikey penceresinde bulabilirsiniz. Eski deneyimde bu özellik şu biçim kullanılarak otomatik olarak kaydettirildi: `https://{tenantdomain}/{appID}` (örneğin, `https://microsoft.onmicrosoft.com/aeb4be67-a634-4f20-9a46-e0d4d4f1f96d`). Yeni deneyimde `api://{appID}`olarak otomatik olarak oluşturulur, ancak açıkça kaydedilmesi gerekir. Azure AD B2C kiracılarda, `https://{tenantdomain}/{appID}` biçimi hala kullanılmaktadır.

## <a name="reply-urlsredirect-urls"></a>Yanıt URL 'Leri/yeniden yönlendirme URL 'Leri

Eski deneyimde bir uygulamada bir **yanıt URL** 'si sayfası vardı. Yeni deneyimde, yanıt URL 'Lerinin bir uygulamanın **kimlik doğrulama** bölümünde bulunabilir. Ayrıca, bunlara **yeniden yönlendirme URI 'leri**denir. Ayrıca, yeniden yönlendirme URI 'Leri biçimi değişmiştir. Uygulama türü (Web veya ortak) ile ilişkilendirilmesi gerekir. Ayrıca, güvenlik nedenleriyle, joker karakterler ve http://şemaları desteklenmez (http://localhost)özel durumu ile).

## <a name="keyscertificates--secrets"></a>Anahtarlar/sertifikalar & gizli dizileri

Eski deneyimde bir uygulamada **anahtarlar** sayfası vardı. Yeni deneyimde **sertifikalar & gizli**dizi olarak yeniden adlandırılmıştır. Bunlara ek olarak, **ortak anahtarlar** **sertifika** ve **parolalar** **istemci gizli**dizileri olarak adlandırılır.

## <a name="required-permissionsapi-permissions"></a>Gerekli izinler/API izinleri

- Eski deneyimde bir uygulamada **gerekli izinler** sayfası vardı. Yeni deneyimde, **API izinleri**olarak yeniden adlandırılmıştır.
- Eski deneyimde bir API seçerken, Microsoft API 'lerinin küçük bir listesinden seçim yapabilirsiniz veya Kiracıdaki hizmet sorumluları arasında arama yapabilirsiniz. Yeni deneyimde birden çok sekmeden birini seçebilirsiniz: **Microsoft API**'leri, **Kuruluşumun kullandığı API 'Ler**veya **API 'lerim**. **API 'lerim** üzerindeki arama çubuğu, Kiracıdaki hizmet sorumluları aracılığıyla sekme aramalarını kullanır.

   > [!NOTE]
   > Uygulamanız bir kiracı ile ilişkili değilse bu sekmeyi görmezsiniz. Yeni deneyimi kullanarak izin isteme hakkında daha fazla bilgi için [Bu hızlı başlangıç](quickstart-configure-app-access-web-apis.md)bölümüne bakın.

- Eski deneyim, **istenen izinler** sayfasının en üstünde **izin ver** düğmesine sahipti. Yeni deneyimde, bir uygulamanın **API izinleri** bölümünde **yönetici izni verme** düğmesine sahip bir **izin ver** bölümü vardır. Ayrıca, düğmelerin işlev açısından bazı farklılıklar vardır:
   - Eski deneyimde, mantık oturum açan kullanıcıya ve istenmekte olan izinlere bağlı olarak değişir. Mantık:
      - Yalnızca Kullanıcı onaylamasına izin isteniyorsa ve oturum açan kullanıcı yönetici değilse, kullanıcı istenen izinler için Kullanıcı onayı verebilir.
      - Yönetici onayı gerektiren en az bir izin isteniyorsa ve oturum açan kullanıcı yönetici değilse, Kullanıcı izin vermeye çalışırken bir hata ile karşılaştı.
      - Oturum açmış olan Kullanıcı bir yöneticiydi, istenen tüm izinler için yönetici onayı verildi.
   - Yeni deneyimde yalnızca bir yönetici izin verebilir. Bir yönetici **yönetici onayı ver** düğmesini seçtiğinde, istenen tüm izinlere yönetici izni verilir.

## <a name="deleting-an-app-registration"></a>Uygulama kaydını silme

Eski deneyimde, bir uygulamanın silinmesi için tek kiracılı olması gerekiyordu. Çok kiracılı uygulamalar için Sil düğmesi devre dışı bırakıldı. Yeni deneyimde, uygulamalar herhangi bir durumda silinebilir, ancak eylemi onaylamanız gerekir. Uygulama kayıtlarını silme hakkında daha fazla bilgi için [Bu hızlı](quickstart-remove-app.md)başlangıca bakın.

## <a name="application-manifest"></a>Uygulama bildirimi

Eski ve yeni deneyimler, bildirim düzenleyicisinde JSON biçimi için farklı sürümler kullanır. Daha fazla bilgi için bkz. [uygulama bildirimi](reference-app-manifest.md).

## <a name="new-ui"></a>Yeni Kullanıcı arabirimi

Daha önce yalnızca bildirim Düzenleyicisi veya API kullanılarak ayarlanan özellikler için yeni kullanıcı arabirimi var veya yok.

- **Örtük verme akışı** (Oauth2AllowImplicitFlow) **kimlik doğrulama** sayfasında bulunabilir. Eski deneyimden farklı olarak, **erişim belirteçlerini** veya **kimlik belirteçlerini**veya her ikisini de etkinleştirebilirsiniz.
- Bu API (oauth2Permissions) ve **yetkili istemci uygulamaları** (ön kimlik doğrulama) **tarafından tanımlanan kapsamlar** , **bir API 'yi kullanıma** sunma sayfasından yapılandırılabilir. Bir uygulamayı Web API 'SI olarak yapılandırma ve izinleri/kapsamları kullanıma sunma hakkında daha fazla bilgi için [Bu hızlı başlangıç](quickstart-configure-app-expose-web-apis.md)bölümüne bakın.
- **Yayımcı etki alanı** ( [uygulamanın izin isteminde](application-consent-experience.md)kullanıcılara gösterilir), **marka dikey** penceresi sayfasında bulunabilir. Yayımcı etki alanını yapılandırma hakkında daha fazla bilgi için, bkz. [nasıl yapılır](howto-configure-publisher-domain.md).

## <a name="limitations"></a>Sınırlamalar

Yeni deneyim aşağıdaki sınırlamalara sahiptir:

- İstemci gizli dizileri (Uygulama parolaları) biçimi, eski deneyimden farklı ve CLı 'yi bozabilir.
- Desteklenen hesapların değerini değiştirmek Kullanıcı arabiriminde desteklenmez. Azure AD tek kiracılı ve çok kiracılı bir şekilde geçiş yapmadığınız takdirde uygulama bildirimini kullanmanız gerekir.
