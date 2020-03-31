---
title: Hata ayıklama SAML tabanlı tek oturum açma - Azure Etkin Dizin | Microsoft Dokümanlar
description: Azure Active Directory'deki uygulamalarda SAML tabanlı tek oturum açma hatası.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.topic: conceptual
ms.workload: identity
ms.date: 02/18/2019
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: luleon, hirsin, paulgarn
ROBOTS: NOINDEX
ms.openlocfilehash: 5d92b43b47a20a75d2c8b6becb69cfee5829e80f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154857"
---
# <a name="debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Azure Active Directory'deki uygulamalarda SAML tabanlı tek oturum açma hatası

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Azure Etkin Dizini'ndeki (Azure AD) güvenlik [öne çıkarma dili (SAML) 2.0'ı](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language)destekleyen uygulamalar için tek oturum [açma](../manage-apps/what-is-single-sign-on.md) sorunlarını nasıl bulabileceğinizi ve düzelteceklerini öğrenin. 

## <a name="before-you-begin"></a>Başlamadan önce

[Uygulamalarım Güvenli Oturum Açma Uzantısı'nı](../user-help/my-apps-portal-end-user-troubleshoot.md#im-having-trouble-installing-the-my-apps-secure-sign-in-extension)yüklemenizi öneririz. Bu tarayıcı uzantısı, sorunları tek oturum açma yla çözmek için gereken SAML isteğini ve SAML yanıt bilgilerini toplamanızı kolaylaştırır. Uzantıyı yükleyemiyorsanız, bu makalede, uzantı yüklü olan ve yüklenmemiş sorunları nasıl çözeceğiniz gösterilmektedir.

Uygulamalarım Güvenli Oturum Açma Uzantısı'nı indirmek ve yüklemek için aşağıdaki bağlantılardan birini kullanın.

- [Chrome](https://go.microsoft.com/fwlink/?linkid=866367)
- [Microsoft Edge](https://go.microsoft.com/fwlink/?linkid=845176)
- [Firefox](https://go.microsoft.com/fwlink/?linkid=866366)

## <a name="test-saml-based-single-sign-on"></a>SAML tabanlı tek oturum açma yı test edin

Azure AD ile hedef uygulama arasında SAML tabanlı tek oturum açma test etmek için:

1. [Azure portalında](https://portal.azure.com) uygulamaları yönetmeye yetkili küresel bir yönetici veya başka bir yönetici olarak oturum açın.
1. Sol bıçakta **Azure Etkin Dizini'ni**seçin ve ardından **Kurumsal uygulamaları**seçin. 
1. Kurumsal uygulamalar listesinden, tek oturum açma test etmek istediğiniz uygulamayı seçin ve ardından soldaki **seçeneklerden Tek oturum açma'yı**seçin.
1. SAML tabanlı tek oturum açma testi deneyimini açmak için **Test tek oturum açma** (adım 5) gidin. **Test** düğmesi soluksa, temel **SAML Yapılandırması** bölümünde önce gerekli öznitelikleri doldurmanız ve kaydetmeniz gerekir.
1. Test **tek oturum açma** bıçağında, hedef uygulamada oturum açabilmek için şirket kimlik bilgilerinizi kullanın. Geçerli kullanıcı olarak veya farklı bir kullanıcı olarak oturum açabilirsiniz. Farklı bir kullanıcı olarak oturum açarsanız, bir istem sizden kimlik doğrulamanızı ister.

    ![Saml SSO sayfasını gösteren ekran görüntüsü](./media/howto-v1-debug-saml-sso-issues/test-single-sign-on.png)

Başarılı bir şekilde oturum açmışsanız, test geçmiştir. Bu durumda, Azure AD uygulamaya bir SAML yanıtı belirteci yayınladı. Uygulama, başarılı bir şekilde oturum açmanız için SAML belirteci'ni kullandı.

Şirket oturum açma sayfasında veya uygulamanın sayfasında bir hata varsa, hatayı gidermek için sonraki bölümlerden birini kullanın.

## <a name="resolve-a-sign-in-error-on-your-company-sign-in-page"></a>Şirket oturum açma sayfanızdaki oturum açma hatasını giderme

Oturum açmayı denediğinizde, şirket oturum açma sayfanızda aşağıdaki örneğe benzer bir hata görebilirsiniz.

![Şirket oturum açma sayfasında bir hata gösteren örnek](./media/howto-v1-debug-saml-sso-issues/error.png)

Bu hatayı hata ayıklamak için hata iletisi ve SAML isteği gerekir. Uygulamalarım Güvenli Oturum Açma Uzantısı bu bilgileri otomatik olarak toplar ve Azure AD'de çözünürlük kılavuzu görüntüler.

### <a name="to-resolve-the-sign-in-error-with-the-my-apps-secure-sign-in-extension-installed"></a>Yüklü Uygulamalar Güvenli Oturum Açma Uzantısı ile oturum açma hatasını gidermek için

1. Bir hata oluştuğunda, uzantı sizi Azure AD **Testi tek oturum açma** bıçağına yönlendirir.
1. Test **tek oturum açma** **bıçağında SAML isteğini indir'i**seçin.
1. SAML isteğindeki hataya ve değerlere dayalı belirli çözünürlük kılavuzu görmeniz gerekir.
1. Sorunu gidermek için Azure AD'deki yapılandırmayı otomatik olarak güncelleştirmek için **Bir Düzelt** düğmesi görürsünüz. Bu düğmeyi görmüyorsanız, oturum açma sorunu Azure AD'deki bir yanlış yapılandırmadan kaynaklanmaz.

Oturum açma hatası için herhangi bir çözüm sağlanmışsa, bizi bilgilendirmek için geri bildirim textbox'ını kullanmanızı öneririz.

### <a name="to-resolve-the-error-without-installing-the-my-apps-secure-sign-in-extension"></a>Uygulamalarım Güvenli Oturum Açma Uzantısı'nı yüklemeden hatayı gidermek için

1. Sayfanın sağ alt köşesindeki hata iletisini kopyalayın. Hata iletisi şunları içerir:
    - KorelasyonID ve Zaman Damgası. Bu değerler, mühendislerin sorununuzu belirlemesine ve sorununuza doğru bir çözüm sağlamasına yardımcı olduğundan, Microsoft ile bir destek örneği oluşturduğunuzda önemlidir.
    - Sorunun temel nedenini tanımlayan bir deyim.
1. Azure AD'ye geri dön ve **Test tek oturum açma** bıçağını bulun.
1. Yukarıdaki metin kutusunda **çözünürlük kılavuzu alın,** hata iletisi yapıştırın.
1. Sorunu çözmek için adımları görüntülemek için **çözünürlük kılavuzu al'ı** tıklatın. Kılavuz, SAML isteği veya SAML yanıtından bilgi gerektirebilir. Uygulamalarım Güvenli Oturum Açma Uzantısı'nı kullanmıyorsanız, SAML isteğini ve yanıtını almak için [Fiddler](https://www.telerik.com/fiddler) gibi bir araca ihtiyacınız olabilir.
1. SAML isteğindeki hedefin Azure AD'den alınan SAML Tek Oturum Açma Hizmeti URL'sine karşılık geldiğini doğrulayın.
1. SAML isteğindeki verenin, Azure AD'deki uygulama için yapılandırdığınız tanımlayıcıyla aynı olduğunu doğrulayın. Azure AD, dizininizde bir uygulama bulmak için vereni kullanır.
1. Uygulamanın Azure AD'den SAML belirteci almayı beklediği yer, AssertionConsumerServiceURL'yi doğrulayın. Bu değeri Azure AD'de yapılandırabilirsiniz, ancak SAML isteğinin bir parçasıysa zorunlu değildir.


## <a name="resolve-a-sign-in-error-on-the-application-page"></a>Uygulama sayfasındaki oturum açma hatasını giderme

Başarılı bir şekilde oturum açabilirsiniz ve uygulamanın sayfasında bir hata görebilirsiniz. Bu, Azure AD uygulamaya bir belirteç verdiğinde oluşur, ancak uygulama yanıtı kabul etmez.

Hatayı gidermek için aşağıdaki adımları izleyin:

1. Uygulama Azure AD Galerisi'ndeyse, uygulamayı Azure AD ile tümleştirmek için tüm adımları izlediğinizi doğrulayın. Uygulamanızın tümleştirme yönergelerini bulmak için [SaaS uygulama tümleştirme öğreticilerinin listesine](../saas-apps/tutorial-list.md)bakın.
1. SAML yanıtını alın.
    - Uygulamalarım Güvenli Oturum Açma uzantısı yüklüyse, **Test tek oturum açma** **bıçağından SAML yanıtını indir'e**tıklayın.
    - Uzantı yüklü değilse, SAML yanıtını almak için [Fiddler](https://www.telerik.com/fiddler) gibi bir araç kullanın.
1. SAML yanıt belirtecinde şu öğelere dikkat edin:
   - NameID değeri ve biçiminin kullanıcı benzersiz tanımlayıcısı
   - Belirteçte verilen talepler
   - Belirteci imzalamak için kullanılan sertifika.

     SAML yanıtı hakkında daha fazla bilgi için [Tek Oturum AÇMA SAML protokolüne](../develop/single-sign-on-saml-protocol.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)bakın.

1. SAML yanıtını gözden geçirdiğiniz için, sorunun nasıl çözüleceği konusunda kılavuz için [oturum açmadan sonra uygulamanın sayfasındahata](../manage-apps/application-sign-in-problem-application-error.md) sayfasına bakın. 
1. Başarılı bir şekilde oturum açamıyorsanız, uygulama satıcısına SAML yanıtında nelerin eksik olduğunu sorabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Artık tek oturum açma uygulamanız için çalışıyorsa, [SaaS uygulamalarına kullanıcı sağlama ve de-provisioning otomatikleştirmek](../manage-apps/user-provisioning.md) veya [Koşullu Erişim ile başlamak](../conditional-access/app-based-conditional-access.md)olabilir.
