---
title: 'Azure AD Connect: Aşamalı kullanıma göre bulut kimlik doğrulaması | Microsoft Dokümanlar'
description: Bu makalede, aşamalı bir rollout kullanarak federe kimlik doğrulamadan bulut kimlik doğrulamasına nasıl geçirilmeye başlanmıştır.
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/07/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: f3044ebdd716eb85dc63d3a77089912d0d51d8b6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74915223"
---
# <a name="migrate-to-cloud-authentication-by-using-staged-rollout-preview"></a>Aşamalı kullanıma (önizleme) kullanarak bulut kimlik doğrulaması için geçiş

Aşamalı bir kullanıma alma yaklaşımı kullanarak, federal kimlik doğrulamadan bulut kimlik doğrulamasına geçirebilirsiniz. Bu makalede, geçiş nasıl yapılacağını açıklanmıştır. Ancak, aşamalı kullanıma başlamadan önce, aşağıdaki koşullardan biri veya daha fazlası doğruysa, bunun sonuçlarını göz önünde bulundurmanız gerekir:
    
-  Şu anda şirket içinde çok faktörlü kimlik doğrulama sunucusu kullanıyorsunuz. 
-  Kimlik doğrulaması için akıllı kartlar kullanıyorsunuz. 
-  Geçerli sunucunuz yalnızca federasyona özel özellikler sunar.

Bu özelliği denemeden önce, doğru kimlik doğrulama yöntemini seçme kılavuzumuzu gözden geçirmenizi öneririz. Daha fazla bilgi için Azure [Active Directory karma kimlik çözümünüz için doğru kimlik doğrulama yöntemini seçin'deki](https://docs.microsoft.com/azure/security/fundamentals/choose-ad-authn#comparing-methods)"Karşılaştırma yöntemleri" tablosuna bakın.

Özelliğe genel bir bakış için şu "Azure Etkin Dizini: Aşamalı kullanıma göre nedir?" Video:

>[!VIDEO https://www.microsoft.com/videoplayer/embed/RE3inQJ]



## <a name="prerequisites"></a>Ön koşullar

-   Federe etki alanları olan bir Azure Etkin Dizin (Azure AD) kiracınız vardır.

-   İki seçeneklerden herhangi biri için hareket etmeye karar verdiniz:
    - **Seçenek A** - *şifre karma senkronizasyon (eşitleme)* + *sorunsuz tek oturum açma (SSO)*
    - **Seçenek B** - *geçiş kimlik doğrulama* + *sorunsuz SSO*
    
    *Sorunsuz SSO* isteğe bağlı olmasına rağmen, şirket ağının içinden etki alanına bağlı makineler çalıştıran kullanıcılar için sessiz bir oturum açma deneyimi elde etmesini sağlamanızı öneririz.

-   Bulut kimlik doğrulamasına geçirilen kullanıcılar için ihtiyacınız olan tüm uygun kiracı markave koşullu erişim ilkelerini yapılandırıldınız.

-   Azure Çok Faktörlü Kimlik Doğrulama'yı kullanmayı planlıyorsanız, kullanıcılarınızın kimlik doğrulama yöntemlerini bir kez kaydettirmek [için self servis parola sıfırlama (SSPR) ve Çok Faktörlü Kimlik Doğrulama için yakınsama kaydı](../authentication/concept-registration-mfa-sspr-combined.md) kullanmanızı öneririz.

-   Aşamalı kullanıma alma özelliğini kullanmak için kiracınızda genel bir yönetici olmanız gerekir.

-   Belirli bir Active Directory ormanında *sorunsuz SSO* sağlamak için bir etki alanı yöneticisi olmanız gerekir.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

Aşağıdaki senaryolar aşamalı kullanıma sunulması için desteklenir. Özellik yalnızca aşağıdakiler için çalışır:

- Azure AD Connect'i kullanarak Azure AD'ye verilen kullanıcılar. Yalnızca bulut kullanıcıları için geçerli değildir.

- Tarayıcılarda ve modern kimlik *doğrulama* istemcilerinde kullanıcı oturum açma trafiği. Eski kimlik doğrulaması kullanan uygulamalar veya bulut hizmetleri, federe kimlik doğrulama akışlarına geri döner. Modern kimlik doğrulaması kapalı olan Exchange çevrimiçi exchange veya modern kimlik doğrulamasını desteklemeyen Outlook 2010 olabilir.

## <a name="unsupported-scenarios"></a>Desteklenmeyen senaryolar

Aşağıdaki senaryolar aşamalı kullanıma sunulması için desteklenmez:

- Bazı uygulamalar kimlik doğrulama sırasında Azure AD'ye "domain_hint" sorgu parametresini gönderir. Bu akışlar devam edecek ve aşamalı kullanıma hazır kullanıcılar kimlik doğrulaması için federasyonu kullanmaya devam edecektir.

<!-- -->

- Yöneticiler, güvenlik gruplarını kullanarak bulut kimlik doğrulamasını kullanıma verebilir. Şirket içi Active Directory güvenlik gruplarını kullanırken eşitleme gecikmesini önlemek için bulut güvenlik grupları kullanmanızı öneririz. Aşağıdaki koşullar geçerlidir:

    - Özellik başına en fazla 10 grup kullanabilirsiniz. Diğer bir süre, parola karma *senkronizasyonu,* *geçiş kimlik doğrulaması*ve *sorunsuz SSO*için 10'er grup kullanabilirsiniz.
    - İç içe gruplar *desteklenmez.* Bu kapsam genel önizleme için de geçerlidir.
    - Dinamik gruplar aşamalı kullanıma yönelik olarak *desteklenmez.*
    - Grup içindeki ilgili nesneler grubun eklenmesini engeller.

- Azure AD Connect veya PowerShell'i kullanarak federeden bulut kimlik doğrulamasına son kesmeyi yapmanız gerekir. Aşamalı kullanıma göre, etki alanlarını federeden yönetilene değiştirmez.

- Aşamalı kullanıma yönelik bir güvenlik grubu ilk eklediğinizde, UX zaman ödemesini önlemek için 200 kullanıcıyla sınırlıkalırsınız. Grubu ekledikten sonra, gerektiğinde doğrudan daha fazla kullanıcı ekleyebilirsiniz.

## <a name="get-started-with-staged-rollout"></a>Aşamalı kullanıma başlayın

Aşamalı kullanıma sunarak *parola karma eşitleme* oturum açma'yı test etmek için bir sonraki bölümdeki ön çalışma yönergelerini izleyin.

Hangi PowerShell cmdlets'in kullanılacağı hakkında bilgi için Azure [AD 2.0 önizlemesine](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview#staged_rollout)bakın.

## <a name="pre-work-for-password-hash-sync"></a>Parola karma eşitleme için ön çalışma

1. Azure AD Connect'teki [İsteğe Bağlı özellikler](how-to-connect-install-custom.md#optional-features) sayfasından *parola karma eşitlemesini* etkinleştirin. 

   ![Azure Active Directory Connect'teki "İsteğe bağlı özellikler" sayfasının ekran görüntüsü](media/how-to-connect-staged-rollout/sr1.png)

1. Tüm kullanıcıların parola karmalarının Azure AD ile senkronize edilebilmesi için tam parola *karma eşitleme* döngüsünün çalıştırıldığından emin olun. *Parola karma eşitleme*durumunu kontrol etmek için, [Azure AD Connect eşitleme ile Sorun Giderme şifre karma senkronizasyonunda](tshoot-connect-password-hash-synchronization.md)PowerShell tanılama yı kullanabilirsiniz.

   ![AADConnect Sorun Giderme günlüğünün ekran görüntüsü](./media/how-to-connect-staged-rollout/sr2.png)

Aşamalı kullanıma giriş kullanarak *geçiş kimlik doğrulama* oturumunu test etmek istiyorsanız, bir sonraki bölümdeki ön çalışma yönergelerini izleyerek bunu etkinleştirin.

## <a name="pre-work-for-pass-through-authentication"></a>Geçiş kimlik doğrulaması için ön çalışma

1. Windows Server 2012 R2 çalıştıran bir sunucu tanımlayın veya daha sonra *geçiş kimlik doğrulama* aracısının çalışmasını istediğiniz yeri belirleyin. 

   Azure AD Connect sunucusunu *seçmeyin.*Sunucunun etki alanına katıldığından, Active Directory ile seçili kullanıcıların kimliğinin doğruladığından ve giden bağlantı noktaları ve URL'lerde Azure AD ile iletişim kurabileceğinden emin olun. Daha fazla bilgi için [Quickstart: Azure AD sorunsuz tek oturum açma](how-to-connect-sso-quick-start.md)bölümünün "Adım 1: Ön koşulları kontrol edin" bölümüne bakın.

1. [Azure AD Connect kimlik doğrulama aracısını indirin](https://aka.ms/getauthagent)ve sunucuya yükleyin. 

1. Yüksek [kullanılabilirlik](how-to-connect-sso-quick-start.md)sağlamak için, diğer sunuculara ek kimlik doğrulama aracıları yükleyin.

1. [Akıllı Kilitleme ayarlarınızı](../authentication/howto-password-smart-lockout.md) uygun şekilde yapılandırdığınızdan emin olun. Bunu yapmak, kullanıcılarınızın şirket içi Active Directory hesaplarının kötü aktörler tarafından kilitlenmesine yardımcı olur.

Aşamalı kullanıma sunulması için seçtiğiniz oturum açma yöntemine *(parola karma eşitleme* veya *geçiş kimlik doğrulama)* bakılmaksızın *sorunsuz SSO'yu* etkinleştirmenizi öneririz. Sorunsuz *SSO*sağlamak için, sonraki bölümde ön çalışma yönergeleri izleyin.

## <a name="pre-work-for-seamless-sso"></a>Sorunsuz SSO için ön çalışma

PowerShell'i kullanarak Active Directory ormanlarında *sorunsuz SSO'yu* etkinleştirin. Birden fazla Active Directory ormanın varsa, her orman için ayrı ayrı etkinleştirin.  *Sorunsuz SSO* yalnızca aşamalı kullanıma seçilmek üzere seçilen kullanıcılar için tetiklenir. Mevcut federasyon kurulumunuzu etkilemez.

Aşağıdakileri yaparak *sorunsuz SSO'yu* etkinleştirin:

1. Azure AD Connect Server'da oturum açın.

2.  *\\%programdosyaları% Microsoft Azure Active Directory Connect* klasörüne gidin.

3. Aşağıdaki komutu çalıştırarak *sorunsuz SSO* PowerShell modüllerini içeri aktarın: 

   `Import-Module .\AzureADSSO.psd1`

4. PowerShell'i yönetici olarak çalıştırın. PowerShell'de, `New-AzureADSSOAuthenticationContext`arayın. Bu komut, kiracınızın genel yönetici kimlik bilgilerini girebileceğiniz bir bölme açar.

5. Arayın. `Get-AzureADSSOStatus | ConvertFrom-Json` Bu komut, üzerinde bu özelliğin etkin olduğu Etkin Dizin ormanlarının bir listesini (bkz. "Etki Alanları" listesi) görüntüler. Varsayılan olarak, kiracı düzeyinde yanlış olarak ayarlanır.

   ![Windows PowerShell çıkışı örneği](./media/how-to-connect-staged-rollout/sr3.png)

6. Arayın. `$creds = Get-Credential` İsteme komut uyla, amaçlanan Active Directory ormanı için etki alanı yöneticisi kimlik bilgilerini girin.

7. Arayın. `Enable-AzureADSSOForest -OnPremCredentials $creds` Bu komut, *kesintisiz SSO*için gerekli olan Active Directory ormanı için şirket içi etki alanı denetleyicisinden AZUREADSSOACC bilgisayar hesabını oluşturur.

8. *Sorunsuz SSO,* URL'lerin intranet bölgesinde olmasını gerektirir. Bu URL'leri grup ilkelerini kullanarak dağıtmak için [Bkz. Hızlı Başlangıç: Azure AD sorunsuz tek oturum açma.](how-to-connect-sso-quick-start.md#step-3-roll-out-the-feature)

9. Tam bir iziçin, *sorunsuz SSO*için [dağıtım planlarımızı](https://aka.ms/SeamlessSSODPDownload) da indirebilirsiniz.

## <a name="enable-staged-rollout"></a>Aşamalı kullanıma etkinleştirme

Belirli bir özelliği *(geçiş kimlik doğrulaması,* *parola karma eşitleme*veya *sorunsuz SSO)* bir gruptaki belirli bir kullanıcı kümesine kullanıma çıkarmak için sonraki bölümlerdeki yönergeleri izleyin.

### <a name="enable-a-staged-rollout-of-a-specific-feature-on-your-tenant"></a>Kiracınızda belirli bir özelliğin aşamalı olarak kullanıma sunulmasını etkinleştirme

Bu seçeneklerden birini kullanıma sunabilirsiniz:

- **Seçenek A** - *şifre karma eşitleme* + *sorunsuz SSO*
- **Seçenek B** - *geçiş kimlik doğrulama* + *sorunsuz SSO*
- **Desteklenmeyen** - *parola karma eşitleme* + geçiş kimlik*doğrulama* + *sorunsuz SSO*

Şunları yapın:

1. Önizleme UX'sine erişmek için [Azure AD portalında](https://aka.ms/stagedrolloutux)oturum açın.

2. Yönetilen **kullanıcı oturum açma (Önizleme)** bağlantısı için aşamalı kullanıma hazırol'u etkinleştir'i seçin.

   Örneğin, *Seçenek A'yı*etkinleştirmek istiyorsanız, Aşağıdaki resimlerde gösterildiği gibi **Parola Karma Eşitleme** ve **Sorunsuz tek oturum açma** denetimlerini **On'a**kaydırın.

   ![Azure AD Connect sayfası](./media/how-to-connect-staged-rollout/sr4.png)

   !["Aşamalı kullanıma alma özelliklerini etkinleştir (Önizleme)" sayfası](./media/how-to-connect-staged-rollout/sr5.png)

3. *Geçiş kimlik doğrulaması* ve *sorunsuz SSO*sağlamak için grupları özelliğe ekleyin. UX zaman ayarı önlemek için, güvenlik gruplarının başlangıçta en fazla 200 üye içermediğinden emin olun.

   !["Parola Karma Eşitleme (Önizleme)" sayfası için grupları yönet](./media/how-to-connect-staged-rollout/sr6.png)

   >[!NOTE]
   >Bir gruptaki üyeler, aşamalı kullanıma sunulması için otomatik olarak etkinleştirilir. İç içe ve dinamik gruplar aşamalı kullanıma yönelik olarak desteklenmez.

## <a name="auditing"></a>Denetim

Aşamalı kullanıma yönelik gerçekleştirdiğimiz çeşitli eylemler için denetim etkinliklerini etkinleştirdik:

- *Parola karma eşitleme,* *geçiş kimlik doğrulaması*veya *sorunsuz SSO*için aşamalı bir kullanıma olanak sağladığında etkinliği denetle.

  >[!NOTE]
  >Sorunsuz *SSO* aşamalı kullanıma kullanılarak açıklandığında bir denetim olayı günlüğe kaydedilir.

  !["Özellik için kullanıma alma ilkesi oluşturma" bölmesi - Etkinlik sekmesi](./media/how-to-connect-staged-rollout/sr7.png)

  !["Özellik için kullanıma alma ilkesi oluşturma" bölmesi - Değiştirilmiş Özellikler sekmesi](./media/how-to-connect-staged-rollout/sr8.png)

- Parola *karma eşitleme,* *geçiş kimlik doğrulaması*veya sorunsuz *SSO'ya*bir grup eklendiğinde denetim olayı.

  >[!NOTE]
  >Bir grup, aşamalı kullanıma sunulması için *parola karma eşitlemesine* eklendiğinde bir denetim olayı günlüğe kaydedilir.

  !["Kullanıma sunulması için grup ekle" bölmesi - Etkinlik sekmesi](./media/how-to-connect-staged-rollout/sr9.png)

  !["Kullanıma sunulması için grup ekle" bölmesi - Değiştirilen Özellikler sekmesi](./media/how-to-connect-staged-rollout/sr10.png)

- Gruba eklenen bir kullanıcı aşamalı kullanıma etkinleştirildiğinde denetim olayı.

  !["Özellik kullanıma kullanıcı ekle" bölmesi - Etkinlik sekmesi](media/how-to-connect-staged-rollout/sr11.png)

  !["Özellik rollout kullanıcı ekle" bölmesi - Hedef(ler) sekmesi](./media/how-to-connect-staged-rollout/sr12.png)

## <a name="validation"></a>Doğrulama

Oturum açma yı *parola karma eşitleme* veya *geçiş kimlik doğrulaması* (kullanıcı adı ve parola oturum açma) ile test etmek için aşağıdakileri yapın:

1. Extranet'te, özel bir tarayıcı [oturumundaki Uygulamalar sayfasına](https://myapps.microsoft.com) gidin ve ardından aşamalı kullanıma sunulması için seçilen kullanıcı hesabının UserPrincipalName'ini (UPN) girin.

   Aşamalı kullanıma yönelik olarak hedeflenen kullanıcılar, federe giriş sayfanıza yönlendirilmez. Bunun yerine, Azure AD kiracı markalı oturum açma sayfasında oturum açmaları istenir.

1. UserPrincipalName ile filtre uygulayarak oturum açmanın [Azure AD oturum açma faaliyet raporunda](../reports-monitoring/concept-sign-ins.md) başarıyla göründüğünden emin olun.

*Sorunsuz SSO*ile oturum açma test etmek için:

1. Intranet'te, özel bir tarayıcı [oturumundaKi Uygulamalar sayfasına](https://myapps.microsoft.com) gidin ve ardından aşamalı kullanıma sunulması için seçilen kullanıcı hesabının UserPrincipalName'ini (UPN) girin.

   *Sorunsuz SSO'nun* aşamalı olarak piyasaya sürülmesi için hedeflenen kullanıcılara "Oturum açmaya çalışmak..." sessizce oturum girmeden önce mesaj gönderin.

1. UserPrincipalName ile filtre uygulayarak oturum açmanın [Azure AD oturum açma faaliyet raporunda](../reports-monitoring/concept-sign-ins.md) başarıyla göründüğünden emin olun.

   Seçilen aşamalı kullanıma sunulan kullanıcılar için Active Directory Federation Services 'da (AD FS) hala bulunan kullanıcı oturum açma oturumlarını izlemek [için, AD FS sorun giderme: Etkinlikler ve günlüğe kaydetme](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-logging#types-of-events)yönergelerini izleyin. Bunu üçüncü taraf federasyon sağlayıcılarında nasıl denetleyin satıcı belgelerini denetleyin.

## <a name="remove-a-user-from-staged-rollout"></a>Bir kullanıcıyı aşamalı kullanıma çıkarmadan kaldırma

Bir kullanıcıyı gruptan kaldırmak, o kullanıcı için aşamalı kullanıma son landırılır. Aşamalı kullanıma alma özelliğini devre dışı bırakabilmek için denetimi **Off'a**geri kaydırın.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**S: Bu yeteneği üretimde kullanabilir miyim?**

C: Evet, bu özelliği üretim kiracınızda kullanabilirsiniz, ancak önce test kiracınızda denemenizi öneririz.

**S: Bu özellik, bazı kullanıcıların federe kimlik doğrulaması kullandığı ve diğerlerinin bulut kimlik doğrulaması kullandığı kalıcı bir "birlikte yaşam"ı korumak için kullanılabilir mi?**

C: Hayır, bu özellik aşamalı olarak federal bulut kimlik doğrulamasına geçiş yapmak ve daha sonra bulut kimlik doğrulamasına kesmek için tasarlanmıştır. Bu yaklaşım beklenmeyen kimlik doğrulama akışlarına yol açabileceğinden, kalıcı bir karma durum kullanmanızı önermiyoruz.

**S: PowerShell'i aşamalı kullanıma sunmayı gerçekleştirmek için kullanabilir miyim?**

C: Evet. Aşamalı kullanıma son vermek için PowerShell'i nasıl kullanacağınızı öğrenmek için [Azure AD Önizleme'ye](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview#staged_rollout)bakın.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure AD 2.0 önizleme](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview#staged_rollout )
