---
title: Yeni Azure portalı uygulama kaydı eğitim kılavuzu
description: Yeni Azure portalı Uygulama kayıt deneyimini sunar
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: marsma
ms.reviewer: lenalepa, keyam
ms.custom: aaddev
ms.openlocfilehash: a437d54dac50be7ddaad899a1cf0a3e93aade8f5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154585"
---
# <a name="new-azure-portal-app-registration-training-guide"></a>Yeni Azure portalı uygulama kaydı eğitim kılavuzu

Azure portalında yeni Uygulama [kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyiminde birçok iyileştirme bulabilirsiniz. Azure portalındaki Uygulama kayıtları (eski) deneyimini biliyorsanız, yeni deneyimi kullanmaya başlamak için bu eğitim kılavuzunu kullanın.

Azure Etkin Dizini'nde, burada açıklanan yeni uygulama kaydı deneyimi genellikle kullanılabilir (GA). Azure Etkin Dizin B2C'de (Azure AD B2C) bu deneyim önizlemededir.

## <a name="key-changes"></a>Önemli değişiklikler

- Uygulama kayıtları yalnızca *bir web uygulaması/API* veya *yerel* bir uygulama olmakla sınırlı değildir. İlgili yönlendirme URL'lerini kaydederek tüm bu uygulamalar için aynı uygulama kaydını kullanabilirsiniz.

- Eski deneyim, yalnızca kuruluş (Azure AD) hesaplarını kullanarak oturum açan uygulamaları destekledi. Uygulamalar tek kiracı olarak kaydedildi. Uygulamalar yalnızca uygulamanın kayıtlı olduğu dizindeki kuruluş hesaplarını destekler. Uygulamalar çok kiracı lı olacak ve tüm kuruluş hesaplarını destekleyecek şekilde değiştirilebilir. Yeni deneyim, hem bu seçenekleri hem de üçüncü bir seçeneği desteklenebilen uygulamaları kaydetmenize olanak tanır: tüm kuruluş hesapları nın yanı sıra kişisel Microsoft hesapları.

- Eski deneyim yalnızca kurumsal bir hesap kullanılarak Azure portalında oturum açıldığında kullanılabilir. Yeni deneyimle, bir dizinle ilişkili olmayan kişisel Microsoft hesaplarını kullanabilirsiniz.

## <a name="list-of-applications"></a>Başvuru listesi

Yeni uygulama listesi, Azure portalındaki eski uygulama kayıtları deneyimi aracılığıyla kaydedilmiş uygulamaları gösterir. Bu uygulamalar Azure AD hesaplarını kullanarak oturum açarak oturum aç. Yeni uygulama listesi, Uygulama Kayıt Portalı'na rağmen kayıtlı uygulamaları da gösterir. Bu uygulamalar Azure AD ve kişisel Microsoft hesaplarını kullanarak oturum açarak oturum aç.00.000.000.000.000.000.000

>[!NOTE]
>Başvuru Kayıt Portalı amortismana kaldırıldı.

Tek bir uygulama kaydı birkaç tür olabileceğinden, yeni uygulama listesinde **Uygulama türü** sütunu yoktur. Listede iki sütun daha vardır: **Üzerinde oluşturulan** ve **Sertifikalar & sırları.** **Sertifikalar & sırları,** uygulamada kayıtlı olan kimlik bilgilerinin durumunu gösterir. Durumlar **Geçerli**, **yakında sona eren**ve Süresi **Dolmuş**içerir.

## <a name="new-app-registration"></a>Yeni uygulama kaydı

Eski deneyimde, sağlamanız gereken bir uygulamayı kaydetmek için: **Ad,** **Uygulama türü**ve Oturum **Açma URL'si/Uri'yi Yönlendirme**. Oluşturulan uygulamalar Yalnızca tek kiracılı uygulamalar olan Azure AD idi. Yalnızca uygulamanın kayıtlı olduğu dizindeki kuruluş hesaplarını desteklediler.

Yeni deneyimde, uygulama için bir **Ad** sağlamalı ve **Desteklenen hesap türlerini**seçmelisiniz. İsteğe bağlı olarak bir **Yönlendirme URI**sağlayabilirsiniz. Uri'yi yeniden yönlendirme sağlıyorsanız, web/herkese açık (mobil ve masaüstü) olup olmadığını belirtmeniz gerekir. Daha fazla bilgi için [Quickstart: Microsoft kimlik platformuna bir uygulama kaydedin.](quickstart-register-app.md) Azure AD B2C için [bkz.](../../active-directory-b2c/tutorial-register-applications.md)

## <a name="differences-between-the-application-registration-portal-and-app-registrations-page"></a>Başvuru Kayıt Portalı ve Uygulama kayıtları sayfası arasındaki farklar

### <a name="the-legacy-properties-page"></a>Eski Özellikler sayfası

Eski **deneyimin** özellikleri sayfası vardı. **Özellikleri** aşağıdaki alanları vardı:

- **Adı**
- **Nesne Kimliği**
- **Başvuru Kimliği**
- **Uygulama Kimliği URI'si**
- **Logo**
- **Ana sayfa URL'si**
- **Giriş URL'si**
- **Hizmet şartları URL'si**
- **Gizlilik bildirimi URL'si**
- **Uygulama türü**
- **Çok Kiracılı**

Yeni deneyim bu sayfa yok. Eşdeğer işlevselliği burada bulabilirsiniz:

- **Ad,** **Logo,** **Ana sayfa URL'** si, **Hizmet Şartları URL'si**ve Gizlilik bildirimi **URL'si** artık uygulamanın **Markalandırma** sayfasındadır.
- **Nesne Kimliği** ve **Uygulama (istemci) kimliği** Genel **Bakış** sayfasındadır.
- Eski deneyimdeki **Çok kiracılı** geçiş tarafından denetlenen işlev, **Kimlik Doğrulama** sayfasında desteklenen **hesap türleri** ile değiştirildi. Daha fazla bilgi için [Bkz. Hızlı Başlangıç: Bir uygulama tarafından desteklenen hesapları değiştirin.](quickstart-modify-supported-accounts.md)
- **Giriş** URL'si artık **Kimlik Doğrulama** sayfasında.
- **Uygulama türü** artık geçerli bir alan değildir. Bunun yerine, **Kimlik Doğrulama** sayfasında bulabileceğiniz Yeniden Yönlendirme UUİ'leri, hangi uygulama türlerinin desteklenebileceğini belirleyin.
- **App ID URI** artık **Uygulama Kimliği URI** denir ve bir **API açığa**üzerinde bulabilirsiniz. Eski deneyimde, bu özellik aşağıdaki biçim kullanılarak `https://{tenantdomain}/{appID}`otomatik olarak `https://microsoft.onmicrosoft.com/492439af-3282-44c3-b297-45463339544b`kaydedildi: , örneğin, . Yeni deneyim, olarak `api://{appID}`otomatik olarak üretilen 's , ama açıkça kaydedilmesi gerekir. Azure AD B2C kiracılarında `https://{tenantdomain}/{appID}` biçim hala kullanılır.

### <a name="reply-urlsredirect-urls"></a>YANıT URL'leri/yeniden yönlendirme R'leri

Eski deneyimde, bir uygulamanın **Yanıt URL'leri** sayfası vardı. Yeni deneyimde, yanıt URL'leri bir uygulamanın **Kimlik Doğrulama** sayfasında bulunabilir. Artık **Yönlendirme URI'leri**olarak anılıyorlar.

Yönlendirme URL'leri biçimi değişti. Web'de veya herkese açık bir uygulama türüyle ilişkilendirilmesi gerekir. Güvenlik nedeniyle, joker `http://` karakterler ve düzenler ilerler *http://localhost*hariç olmak üzere desteklenmez.

### <a name="keyscertificates--secrets"></a>Anahtarlar/Sertifikalar & sırları

Eski deneyimde, bir uygulamanın **Keys** sayfası vardı. Yeni deneyim, bu **sertifikalar & sırları**olarak değiştirildi.

**Ortak anahtarlar** artık **Sertifikalar**olarak adlandırılır. **Parolalar** artık **İstemci sırları**olarak adlandırılır.

### <a name="required-permissionsapi-permissions"></a>Gerekli izinler/API izinleri

Eski deneyimde, bir uygulamanın **gerekli izinler** sayfası vardı. Yeni deneyimde, **API izinleri**olarak yeniden adlandırıldı.

Eski deneyimde bir API seçtiğinizde, küçük bir Microsoft API listesinden seçim yapabilirsiniz. Ayrıca kiracı hizmet müdürleri aracılığıyla arama olabilir. Yeni deneyimde, birden çok sekme arasından seçim yapabilirsiniz: **Microsoft API'leri,** **kuruluşumun kullandığı API'ler**veya **API'lerim.** **Kuruluşumdaki API'ler** üzerindeki arama çubuğu, kiracıdaki servis ilkeleri aracılığıyla sekme aramaları kullanır.

> [!NOTE]
> Uygulamanız bir kiracıyla ilişkili değilse bu sekmeyi görmezsiniz. İzinleri nasıl isteyeceğihakkında daha fazla bilgi için Bkz. [Quickstart: Web API'lerine erişmek için istemci uygulamasını yapılandırın.](quickstart-configure-app-access-web-apis.md)

Eski deneyim, **İstenen izinler** sayfasının üst kısmında bir **Hibe izinleri** düğmesine sahipti. Yeni deneyimde, **Hibe onay** sayfasında bir uygulamanın **API izinleri** bölümünde Grant **yöneticisi onay** düğmesi vardır. Düğmelerin çalışma biçimlerinde de bazı farklılıklar vardır.

Eski deneyimde, mantık oturum açmış kullanıcıya ve istenen izinlere bağlı olarak değişir. Mantık şuydu:

- Yalnızca kullanıcı nın izinleri isteniyorsa ve oturum açmış kullanıcı bir yönetici değilse, kullanıcı istenen izinler için kullanıcı ya da kullanıcı onayı verebilir.
- Yönetici onayı gerektiren en az bir izin istenirse ve oturum açmış kullanıcı yönetici değilse, kullanıcı onay vermeye çalışırken bir hata aldı.
- Oturum açmış kullanıcı bir yönetici ise, istenen tüm izinler için yönetici onayı verilir.

Yeni deneyimde, yalnızca bir yönetici izin verebilir. Bir yönetici **Grant admin onayı**seçtiğinde, yönetici onayı istenen tüm izinlere verilir.

## <a name="deleting-an-app-registration"></a>Uygulama kaydını silme

Eski deneyimde, yalnızca tek kiracılı uygulamaları silebilirsiniz. Çok kiracılı uygulamalar için silme düğmesi devre dışı bırakıldı. Yeni deneyimde, uygulamaları herhangi bir durumda silebilirsiniz, ancak eylemi onaylamanız gerekir. Daha fazla bilgi için [Bkz. Quickstart: Microsoft kimlik platformuna kayıtlı bir uygulamayı kaldırın.](quickstart-remove-app.md)

## <a name="application-manifest"></a>Uygulama bildirimi

Eski ve yeni deneyimler, manifesto düzenleyicisinde JSON formatı için farklı sürümler kullanır. Daha fazla bilgi için [Azure Active Directory uygulama bildirimine](reference-app-manifest.md)bakın.

## <a name="new-ui"></a>Yeni UI

Yeni deneyim, aşağıdaki özellikler için UI denetimleri ekler:

- **Kimlik Doğrulama** sayfasında Örtülü hibe`oauth2AllowImplicitFlow` **akışı** vardır ( ). Eski deneyimin aksine, **Access belirteçlerini** veya **kimlik belirteçlerini**veya her ikisini de etkinleştirebilirsiniz.
- **Bir API'yi Ortaya Çıkarma** **sayfası, bu API** ( ) tarafından`oauth2Permissions`tanımlanan Kapsamlar ve **Yetkili İstemci uygulamaları** (`preAuthorizedApplications`) içerir. Bir uygulamanın web API'si olarak nasıl yapılandırılabildiğini ve izinleri/kapsamları nasıl açıkhale erdireceğiniz hakkında daha fazla bilgi için Bkz. [Quickstart: Web API'lerini ortaya çıkarmak için bir uygulamayı yapılandırın.](quickstart-configure-app-expose-web-apis.md)
- **Markalama** sayfası **Publisher etki alanını**içerir. Yayımcı etki [alanı, uygulamanın onay isteminde](application-consent-experience.md)kullanıcılara görüntülenir. Daha fazla bilgi için [bkz: Bir uygulamanın yayımcı etki alanını yapılandırma.](howto-configure-publisher-domain.md)

## <a name="limitations"></a>Sınırlamalar

Yeni deneyimaşağıdaki sınırlamalar vardır:

- İstemci sırlarının biçimi (uygulama parolaları) eski deneyimden farklıdır ve CLI'yi bozabilir.
- Desteklenen hesapların değerini değiştirmek UI'de desteklenmez. Azure AD tek kiracılı ve çok kiracılı arasında geçiş yapmadığınız sürece uygulama bildirimini kullanmanız gerekir.
