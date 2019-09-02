---
title: 'Azure AD Connect: Sorunsuz çoklu oturum açma-sık sorulan sorular | Microsoft Docs'
description: Azure Active Directory sorunsuz çoklu oturum açma hakkında sık sorulan soruların yanıtları.
services: active-directory
keywords: Azure AD Connect nedir, yükler Active Directory, Azure AD, SSO, çoklu oturum açma için gerekli bileşenler
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/14/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 709fb3be37850be37d6378652921ce26f4ff15fe
ms.sourcegitcommit: dcea3c1ab715a79ebecd913885fbf9bbee61606a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2019
ms.locfileid: "60242223"
---
# <a name="azure-active-directory-seamless-single-sign-on-frequently-asked-questions"></a>Kesintisiz çoklu oturum açma Azure Active Directory: Sık sorulan sorular

Bu makalede, sorunsuz çoklu oturum açma Azure Active Directory (sorunsuz SSO) hakkında sık sorulan soruları ele aldık. Yeni içerik için yeniden kullanıma devam edin.

## <a name="what-sign-in-methods-do-seamless-sso-work-with"></a>Sorunsuz SSO hangi oturum açma yöntemlerine göre çalışır?

Kesintisiz SSO, [Parola karması eşitlemesi](how-to-connect-password-hash-synchronization.md) veya [geçişli kimlik doğrulaması](how-to-connect-pta.md) oturum açma yöntemleriyle birleştirilebilir. Ancak bu özellik Active Directory Federasyon Hizmetleri (AD FS) (ADFS) ile kullanılamaz.

## <a name="is-seamless-sso-a-free-feature"></a>Sorunsuz SSO ücretsiz bir özelliktir mi?

Sorunsuz SSO ücretsiz bir özelliktir ve Azure AD 'nin bu sürümü kullanabilmesi için ücretli bir sürümü gerekmez.

## <a name="is-seamless-sso-available-in-the-microsoft-azure-germany-cloudhttpswwwmicrosoftdecloud-deutschland-and-the-microsoft-azure-government-cloudhttpsazuremicrosoftcomfeaturesgov"></a>[Microsoft Azure Almanya bulutta](https://www.microsoft.de/cloud-deutschland) ve [MICROSOFT Azure Kamu bulutta](https://azure.microsoft.com/features/gov/)sorunsuz SSO kullanılabilir mi?

Hayır. Sorunsuz SSO yalnızca dünya çapındaki Azure AD örneğinde bulunabilir.

## <a name="what-applications-take-advantage-of-domain_hint-or-login_hint-parameter-capability-of-seamless-sso"></a>Sorunsuz SSO 'nun `domain_hint` veya `login_hint` parametre özelliğinden faydalanan uygulamalar nelerdir?

Aşağıda, bu parametreleri Azure AD 'ye gönderebilen kapsamlı bir uygulama listesi verilmiştir ve bu nedenle kullanıcılara sorunsuz SSO (yani, kullanıcılarınızın Kullanıcı adlarını veya parolalarını girmesi gerekmez) kullanarak sessiz bir oturum açma deneyimi sağlar:

| Uygulama adı | Kullanılacak uygulama URL 'SI |
| -- | -- |
| Erişim paneli | https:\//myapps.Microsoft.com/contoso.com |
| Web 'de Outlook | https:\//Outlook.office365.com/contoso.com |
| Office 365 portalları | https:\//Portal.Office.com? domain_hint = contoso. com, https:\//www.Office.com? domain_hint = contoso. com |

Ayrıca, bir uygulama Azure AD uç noktalarına kiracı olarak ayarlanan (https:\//Login.microsoftonline.com/contoso.com/<) bir oturum açma isteği gönderdiğinde, kullanıcılar sessiz oturum açma deneyimi alırlar > veya https:\//Login.microsoftonline.com/<tenant_ID>/<.. >-Azure AD 'nin ortak uç noktası yerine, https:\//Login.microsoftonline.com/Common/<... >. Aşağıda, bu tür oturum açma isteklerini yapan, kapsamlı bir uygulamalar listesi aşağıda verilmiştir.

| Uygulama adı | Kullanılacak uygulama URL 'SI |
| -- | -- |
| SharePoint Online | https:\//contoso.SharePoint.com |
| Azure portal | https:\//Portal.Azure.com/contoso.com |

Yukarıdaki tablolarda, kiracınızın doğru uygulama URL 'Lerine ulaşmak için "contoso.com" yerine etki alanı adınızı koyun.

Diğer uygulamaların sessiz oturum açma deneyimimizi kullanmasını istiyorsanız geri bildirim bölümünde bize bilgi verin.

## <a name="does-seamless-sso-support-alternate-id-as-the-username-instead-of-userprincipalname"></a>Kullanıcı adı `Alternate ID` `userPrincipalName`yerine sorunsuz SSO Desteği ister misiniz?

Evet. Sorunsuz SSO, `Alternate ID` [burada](how-to-connect-install-custom.md)gösterildiği gibi Azure AD Connect ' de yapılandırıldığında Kullanıcı adı olarak destekler. Office 365 uygulamalarının `Alternate ID`hepsi desteklenmez. Destek bildirimiyle ilgili uygulamanın belgelerine bakın.

## <a name="what-is-the-difference-between-the-single-sign-on-experience-provided-by-azure-ad-joinactive-directory-azureadjoin-overviewmd-and-seamless-sso"></a>[Azure AD JOIN](../active-directory-azureadjoin-overview.md) ve sorunsuz SSO tarafından sunulan çoklu oturum açma deneyimi arasındaki fark nedir?

[Azure AD JOIN](../active-directory-azureadjoin-overview.md) , CIHAZLARı Azure AD 'ye KAYıTLıYSA kullanıcılara SSO sağlar. Bu cihazların etki alanına katılmış olması gerekmez. SSO, *birincil yenileme belirteçleri* veya *prts*'Ler kullanılarak sağlanır ve Kerberos değildir. Kullanıcı deneyimi Windows 10 cihazlarında en iyi şekilde kullanılır. SSO, Microsoft Edge tarayıcısında otomatik olarak gerçekleşir. Ayrıca, tarayıcı uzantısı kullanılarak Chrome üzerinde de kullanılabilir.

Kiracınızda Azure AD JOIN ve sorunsuz SSO kullanabilirsiniz. Bu iki özellik tamamlayıcı bir özelliklerdir. Her iki özellik de açıksa, Azure AD birleştirmelerinin SSO 'SU sorunsuz SSO 'dan önceliklidir.

## <a name="i-want-to-register-non-windows-10-devices-with-azure-ad-without-using-ad-fs-can-i-use-seamless-sso-instead"></a>Windows 10 olmayan cihazları Azure AD 'ye AD FS kullanmadan kaydetmek istiyorum. Bunun yerine sorunsuz SSO kullanabilir miyim?

Evet, bu senaryo, [çalışma alanına katılma istemcisinin](https://www.microsoft.com/download/details.aspx?id=53554)2,1 veya sonraki bir sürümünü gerektirir.

## <a name="how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account"></a>`AZUREADSSOACC` Bilgisayar hesabının Kerberos şifre çözme anahtarını nasıl alabilirim?

Şirket içi ad ormanınızda oluşturulan `AZUREADSSOACC` bilgisayar hesabının (Azure AD 'yi temsil eden) Kerberos şifre çözme anahtarını çok sık almak önemlidir.

>[!IMPORTANT]
>Kerberos şifre çözme anahtarını en az 30 günde bir almanızı önemle tavsiye ederiz.

Azure AD Connect çalıştırdığınız şirket içi sunucuda bu adımları izleyin:

### <a name="step-1-get-list-of-ad-forests-where-seamless-sso-has-been-enabled"></a>1\.Adım Sorunsuz SSO 'nun etkinleştirildiği AD ormanları listesini alın

1. İlk olarak, [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview)'i indirin ve yükleyin.
2. `%programfiles%\Microsoft Azure Active Directory Connect` klasörüne gidin.
3. Bu komutu kullanarak sorunsuz SSO PowerShell modülünü içeri aktarın: `Import-Module .\AzureADSSO.psd1`.
4. PowerShell 'i yönetici olarak çalıştırın. PowerShell 'de, çağırın `New-AzureADSSOAuthenticationContext`. Bu komut, kiracınızın genel yönetici kimlik bilgilerini girebileceğiniz bir açılan menü vermelidir.
5. Çağrısı `Get-AzureADSSOStatus | ConvertFrom-Json`yapın. Bu komut, bu özelliğin etkinleştirildiği AD ormanları listesini ("etki alanları" listesine bakın) sağlar.

### <a name="step-2-update-the-kerberos-decryption-key-on-each-ad-forest-that-it-was-set-it-up-on"></a>2\.Adım Üzerinde ayarlandığı her bir AD ormanında Kerberos şifre çözme anahtarını güncelleştirme

1. Çağrısı `$creds = Get-Credential`yapın. İstendiğinde, hedeflenen AD ormanının etki alanı yönetici kimlik bilgilerini girin.

   > [!NOTE]
   > Hedeflenen ad ormanını bulmak için Kullanıcı asıl adı (UPN) (johndoe@contoso.com) biçiminde veya etki alanı uygun Sam hesabı adı (contoso\johntikan veya contoso. com\johntikan) biçiminde belirtilen etki alanı yöneticisinin Kullanıcı adını kullanırız. Etki alanı nitelikli Sam hesabı adı kullanırsanız, [DNS kullanarak etki alanı yöneticisinin etki alanı denetleyicisini bulmak](https://social.technet.microsoft.com/wiki/contents/articles/24457.how-domain-controllers-are-located-in-windows.aspx)için Kullanıcı adının etki alanı bölümünü kullanırız. Bunun yerine UPN kullanırsanız, uygun etki alanı denetleyicisini bulmadan önce [etki alanı tam bir SAM hesap adına çeviririz](https://docs.microsoft.com/windows/desktop/api/ntdsapi/nf-ntdsapi-dscracknamesa) .

2. Çağrısı `Update-AzureADSSOForest -OnPremCredentials $creds`yapın. Bu komut, bu belirli ad ormanındaki `AZUREADSSOACC` bilgisayar hesabının Kerberos şifre çözme anahtarını güncelleştirir ve Azure AD 'de güncelleştirir.
3. Özelliği ayarladığınız her AD Ormanı için önceki adımları tekrarlayın.

>[!IMPORTANT]
>`Update-AzureADSSOForest` Komutu birden çok kez _çalıştırmayın_ emin olun. Aksi takdirde, kullanıcılarınızın Kerberos biletlerinin süresi dolana ve şirket içi Active Directory tarafından yeniden yayımlanıncaya kadar özellik çalışmayı sonlandırır.

## <a name="how-can-i-disable-seamless-sso"></a>Sorunsuz SSO 'yu nasıl devre dışı bırakabilirim?

### <a name="step-1-disable-the-feature-on-your-tenant"></a>1\.Adım Kiracınızda özelliği devre dışı bırakın

#### <a name="option-a-disable-using-azure-ad-connect"></a>Seçenek A: Azure AD Connect kullanarak devre dışı bırak

1. Azure AD Connect çalıştırın, **Kullanıcı oturum açma sayfasını Değiştir** ' i seçin ve **İleri**' ye tıklayın.
2. **Çoklu oturum açmayı etkinleştir** seçeneğinin işaretini kaldırın. Sihirbaza devam edin.

Sihirbazı tamamladıktan sonra, kiracınızda sorunsuz SSO devre dışı bırakılacaktır. Bununla birlikte, ekranda şu şekilde okuyan bir ileti görürsünüz:

"Çoklu oturum açma artık devre dışı bırakıldı, ancak temizleme işlemini tamamlamak için el ile gerçekleştirilecek ek adımlar var. Daha fazla bilgi "

Temizleme işlemini gerçekleştirmek için Azure AD Connect çalıştırdığınız şirket içi sunucuda adım 2 ve 3 ' ü izleyin.

#### <a name="option-b-disable-using-powershell"></a>Seçenek B: PowerShell 'i kullanarak devre dışı bırak

Azure AD Connect çalıştırdığınız şirket içi sunucuda aşağıdaki adımları çalıştırın:

1. İlk olarak, [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview)'i indirin ve yükleyin.
2. `%programfiles%\Microsoft Azure Active Directory Connect` klasörüne gidin.
3. Bu komutu kullanarak sorunsuz SSO PowerShell modülünü içeri aktarın: `Import-Module .\AzureADSSO.psd1`.
4. PowerShell 'i yönetici olarak çalıştırın. PowerShell 'de, çağırın `New-AzureADSSOAuthenticationContext`. Bu komut, kiracınızın genel yönetici kimlik bilgilerini girebileceğiniz bir açılan menü vermelidir.
5. Çağrısı `Enable-AzureADSSO -Enable $false`yapın.

>[!IMPORTANT]
>PowerShell kullanarak sorunsuz SSO devre dışı bırakmak, Azure AD Connect durumunu değiştirmez. Sorunsuz SSO, **Kullanıcı oturum açma** sayfasında etkin olarak görünür.

### <a name="step-2-get-list-of-ad-forests-where-seamless-sso-has-been-enabled"></a>2\.Adım Sorunsuz SSO 'nun etkinleştirildiği AD ormanları listesini alın

Azure AD Connect kullanarak sorunsuz SSO 'yu devre dışı bıraktığınız takdirde, aşağıdaki görevleri 1 ' den izleyin. Bunun yerine PowerShell kullanarak sorunsuz SSO 'yu devre dışı bırakırsanız, aşağıdaki görev 5 ' e atlayın.

1. İlk olarak, [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview)'i indirin ve yükleyin.
2. `%programfiles%\Microsoft Azure Active Directory Connect` klasörüne gidin.
3. Bu komutu kullanarak sorunsuz SSO PowerShell modülünü içeri aktarın: `Import-Module .\AzureADSSO.psd1`.
4. PowerShell 'i yönetici olarak çalıştırın. PowerShell 'de, çağırın `New-AzureADSSOAuthenticationContext`. Bu komut, kiracınızın genel yönetici kimlik bilgilerini girebileceğiniz bir açılan menü vermelidir.
5. Çağrısı `Get-AzureADSSOStatus | ConvertFrom-Json`yapın. Bu komut, bu özelliğin etkinleştirildiği AD ormanları listesini ("etki alanları" listesine bakın) sağlar.

### <a name="step-3-manually-delete-the-azureadssoacct-computer-account-from-each-ad-forest-that-you-see-listed"></a>Adım 3. Listede gördüğünüz her `AZUREADSSOACCT` ad ormanındaki bilgisayar hesabını el ile silin.

## <a name="next-steps"></a>Sonraki adımlar

- [**Hızlı başlangıç**](how-to-connect-sso-quick-start.md) -Azure AD sorunsuz SSO 'yu alın ve çalıştırın.
- [**Teknik kapsamlı**](how-to-connect-sso-how-it-works.md) bakış-bu özelliğin nasıl çalıştığını anlayın.
- [**Sorun giderme**](tshoot-connect-sso.md) -özellikle ilgili yaygın sorunları çözmeyi öğrenin.
- [**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) -yeni özellik isteklerini dosyalama.
