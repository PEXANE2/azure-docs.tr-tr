---
title: Azure Active Directory| için AppSource sertifikası nasıl alabilirsiniz? Microsoft Dokümanlar
description: Uygulamanızın AppSource'unu Azure Active Directory için nasıl sertifikalı alacağınızla ilgili ayrıntılar.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.topic: conceptual
ms.workload: identity
ms.date: 08/21/2018
ms.author: ryanwi
ms.reviewer: jeedes
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: 3ad4efa3b8126a9b9c6557822f61e3bfff3fe120
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154891"
---
# <a name="how-to-get-appsource-certified-for-azure-active-directory"></a>Azure Active Directory için AppSource Sertifikası nasıl alabilirsiniz?

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

[Microsoft AppSource,](https://appsource.microsoft.com/) iş kullanıcılarının iş yeri olan SaaS uygulamalarını (bağımsız SaaS ve varolan Microsoft SaaS ürünlerine eklentisi) keşfetmeleri, denemeleri ve yönetmeleri için bir hedeftir.

AppSource'ta tek başına bir SaaS uygulamasını listelemek için, uygulamanızın Azure Etkin Dizini (Azure AD) olan herhangi bir şirket veya kuruluşun iş hesaplarından tek oturum açma kabul etmesi gerekir. Oturum açma işlemi [OpenID Connect](v1-protocols-openid-connect-code.md) veya [OAuth 2.0](v1-protocols-oauth-code.md) protokollerini kullanmalıdır. SAML entegrasyonu AppSource sertifikası için kabul edilmez.

## <a name="guides-and-code-samples"></a>Kılavuzlar ve kod örnekleri

Open ID bağlantısını kullanarak uygulamanızı Azure AD ile nasıl entegre ettiğinizi öğrenmek istiyorsanız, [Azure Active Directory geliştirici kılavuzundaki](v1-overview.md#get-started "Geliştiriciler için Azure AD ile başlayın")kılavuzlarımızı ve kod örneklerimizi izleyin.

## <a name="multi-tenant-applications"></a>Çok kiracılı uygulamalar

*Çok kiracılı uygulama,* ayrı bir örnek, yapılandırma veya dağıtım gerektirmeden Azure AD'si olan herhangi bir şirket veya kuruluştaki kullanıcılardan oturum açma kabul eden bir uygulamadır. AppSource, uygulamaların *tek tıklamayla* ücretsiz deneme deneyimini etkinleştirmek için çoklu kira uygulaması önerir.

Uygulamanızda çoklu kira yı etkinleştirmek için aşağıdaki adımları izleyin:
1. Azure `Multi-Tenanted` `Yes` [portalındaki](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps)uygulama kaydıbilgilerinizdeki özelliği ayarlayın. Varsayılan olarak, Azure portalında oluşturulan uygulamalar *[tek kiracı](#single-tenant-applications)* olarak yapılandırılır.
1. İstekleri `common` bitiş noktasına göndermek için kodunuzu güncelleştirin. Bunu yapmak için bitiş noktasını `https://login.microsoftonline.com/{yourtenant}` `https://login.microsoftonline.com/common*`' dan ' a güncelleştirin
1. ASP .NET gibi bazı platformlariçin, birden çok vereni kabul etmek için kodunuzu güncelleştirmeniz gerekir.

Çoklu kiralama hakkında daha fazla bilgi için, [birden çok kiracılı uygulama deseni kullanarak herhangi bir Azure Active Directory (Azure AD) kullanıcısını nasıl oturum alabildiğini](../develop/howto-convert-app-to-be-multi-tenant.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)öğrenin.

### <a name="single-tenant-applications"></a>Tek kiracı uygulamaları

*Tek kiracılı uygulama,* yalnızca tanımlanmış bir Azure AD örneğinin kullanıcılarının oturum açmalarını kabul eden bir uygulamadır. Harici kullanıcılar (diğer kuruluşlardaki iş veya okul hesapları veya kişisel hesaplar dahil), her kullanıcıyı uygulamanın kayıtlı olduğu Azure REKLAM örneğine konuk hesabı olarak ekledikten sonra tek kiracılı bir uygulamada oturum açabilir. 

[Azure AD B2B işbirliği](../b2b/what-is-b2b.md) aracılığıyla kullanıcıları Azure AD'ye konuk hesabı olarak ekleyebilirsiniz ve bunu [programlı olarak](../../active-directory-b2c/code-samples.md)yapabilirsiniz. B2B kullanırken, kullanıcılar oturum açma daveti gerektirmeyen bir self servis portalı oluşturabilir. Daha fazla bilgi [için Azure AD B2B işbirliği kaydolma için Self servis portalına](https://docs.microsoft.com/azure/active-directory/b2b/self-service-portal)bakın.

Tek kiracılı uygulamalar *Bana Ulaşın* deneyimini etkinleştirebilir, ancak AppSource'un önerdiği tek tıklamalı/ücretsiz deneme deneyimini etkinleştirmek istiyorsanız, uygulamanızda çoklu kirayı etkinleştirin.

## <a name="appsource-trial-experiences"></a>AppSource deneme deneyimleri

### <a name="free-trial-customer-led-trial-experience"></a>Ücretsiz deneme deneyimi (müşteri liderliğindeki deneme deneyimi)

Müşteri liderliğindeki deneme, Uygulama Kaynağı'nın uygulamanıza tek tıklamayla erişim sağladığı için önerdiği deneyimdir. Aşağıdaki örnek, bu deneyimin nasıl göründüğünü gösterir:

<table >
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step1.png" width="85%" alt-text="Shows Free trial for customer-led trial experience"/><ul><li>Kullanıcı uygulamanızı AppSource Web Sitesinde bulur</li><li>'Ücretsiz deneme' seçeneğini seçer</li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step2.png" width="85%" alt-text="Shows how user is redirected to a URL in your web site"/><ul><li>AppSource, kullanıcıyı web sitenizdeki bir URL'ye yönlendirir</li><li>Web siteniz <i>tek oturum açma</i> işlemini otomatik olarak başlatır (sayfa yüklemesi üzerine)</li></ul></td>
    <td valign="top" width="33%">3.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step3.png" width="85%" alt-text="Shows the Microsoft sign-in page"/><ul><li>Kullanıcı Microsoft Oturum Açma sayfasına yönlendirilir</li><li>Kullanıcı oturum açma kimlik bilgileri sağlar</li></ul></td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step4.png" width="85%" alt-text="Example: Consent page for an application"/><ul><li>Kullanıcı uygulamanız için onay verir</li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%" alt-text="Shows the experience the user sees when redirected back to your site"/><ul><li>Oturum açma tamamlar ve kullanıcı web sitenize geri yönlendirilir</li><li>Kullanıcı ücretsiz deneme başlar</li></ul></td>
    <td></td>
</tr>
</table>

### <a name="contact-me-partner-led-trial-experience"></a>Bana ulaşın (ortak liderliğindeki deneme deneyimi)

Kullanıcıya/şirkete sağlamak için bir el kitabı veya uzun vadeli bir işlem olması gerektiğinde iş ortağı deneme deneyimini kullanabilirsiniz ( örneğin, uygulamanızın sanal makineleri, veritabanı örneklerini veya tamamlanması çok zaman alan işlemleri sağlaması gerekir. Bu durumda, kullanıcı **Deneme Talebi** düğmesini seçtikten ve bir form doldurduktan sonra AppSource size kullanıcının iletişim bilgilerini gönderir. Bu bilgileri aldığınızda, ortamı sağlar sınız ve deneme deneyimine nasıl erişireceklerine ilişkin talimatları kullanıcıya gönderirsiniz:<br/><br/>

<table valign="top">
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step1.png" width="85%" alt-text="Shows Contact me for partner-led trial experience"/><ul><li>Kullanıcı Uygulamanızı AppSource web sitesinde bulur</li><li>'Bana Ulaşın' seçeneğini seçer</li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step2.png" width="85%" alt-text="Shows an example form with contact info"/><ul><li>Bir formu iletişim bilgileriyle doldurur</li></ul></td>
     <td valign="top" width="33%">3.<br/><br/>
        <table bgcolor="#f7f7f7">
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/usercontact.png" width="55%" alt-text="Shows placeholder for user information"/></td>
            <td>Kullanıcı bilgilerini alırsınız</td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/setupenv.png" width="55%" alt-text="Shows placeholder for setup environment info"/></td>
            <td>Kurulum ortamı</td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/contactcustomer.png" width="55%" alt-text="Shows placeholder for trial info"/></td>
            <td>Deneme bilgileriyle kullanıcıyla iletişim kurun</td>
        </tr>
        </table><br/><br/>
        <ul><li>Kullanıcı bilgilerini ve kurulum deneme örneğini alırsınız</li><li>Uygulamanıza erişmek için köprügöndermek kullanıcıya</li></ul>
    </td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step3.png" width="85%" alt-text="Shows the application sign-in screen"/><ul><li>Kullanıcı uygulamanıza erişir ve tek oturum açma işlemini tamamlar</li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step4.png" width="85%" alt-text="Shows an example consent page for an application"/><ul><li>Kullanıcı uygulamanız için onay verir</li></ul></td>
    <td valign="top" width="33%">6.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%" alt-text="Shows the experience the user sees when redirected back to your site"/><ul><li>Oturum açma tamamlar ve kullanıcı web sitenize geri yönlendirilir</li><li>Kullanıcı ücretsiz deneme başlar</li></ul></td>
   
</tr>
</table>

### <a name="more-information"></a>Daha fazla bilgi

AppSource deneme deneyimi hakkında daha fazla bilgi için [bu videoyu](https://aka.ms/trialexperienceforwebapps)izleyin. 

## <a name="next-steps"></a>Sonraki Adımlar

- Azure AD oturum açma oturumlarını destekleyen uygulamalar oluşturma hakkında daha fazla bilgi [için Azure AD için Kimlik Doğrulama senaryolarına](https://docs.microsoft.com/azure/active-directory/develop/v1-authentication-scenarios)bakın.
- SaaS uygulamanızı AppSource'da nasıl listeleyecekleriniz hakkında bilgi için [AppSource İş Ortağı Bilgileri'ne](https://appsource.microsoft.com/partners) bakın

## <a name="get-support"></a>Destek alın

Azure AD tümleştirmesi için, destek sağlamak için [Yığın Taşma'yı](https://stackoverflow.com/questions/tagged/azure-active-directory+appsource) toplulukla birlikte kullanırız.

Önce Stack Overflow ile ilgili sorularınızı sormanızı ve sorunuza daha önce birinin sorup sormadığınızı görmek için mevcut sorunlara göz atmanızı öneririz. Sorularınızın veya yorumlarınızın etiketlendirilip [ `[azure-active-directory]` `[appsource]`. ](https://stackoverflow.com/questions/tagged/azure-active-directory+appsource)

Geri bildirim sağlamak ve içeriğimizi geliştirmemize ve şekillendirmemize yardımcı olmak için aşağıdaki yorumlar bölümünü kullanın.

<!--Reference style links -->
[AAD-Auth-Scenarios]:v1-authentication-scenarios.md
[AAD-Auth-Scenarios-Browser-To-WebApp]:v1-authentication-scenarios.md#web-browser-to-web-application
[AAD-Dev-Guide]: v1-overview.md
[AAD-Howto-Multitenant-Overview]: howto-convert-app-to-be-multi-tenant.md
[AAD-QuickStart-Web-Apps]: v1-overview.md#get-started

<!--Image references-->
