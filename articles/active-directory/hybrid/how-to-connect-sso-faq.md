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
ms.openlocfilehash: 96d0253e1e656f4bdb5180af0d57824aceb4f0dd
ms.sourcegitcommit: 83df2aed7cafb493b36d93b1699d24f36c1daa45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2019
ms.locfileid: "71176653"
---
# <a name="azure-active-directory-seamless-single-sign-on-frequently-asked-questions"></a>Kesintisiz çoklu oturum açma Azure Active Directory: Sık sorulan sorular

Bu makalede, sorunsuz çoklu oturum açma Azure Active Directory (sorunsuz SSO) hakkında sık sorulan soruları ele aldık. Yeni içerik için yeniden kullanıma devam edin.

**S: Her oturum açma yöntemi, sorunsuz SSO üzerinde çalışır**

Kesintisiz SSO, [Parola karması eşitlemesi](how-to-connect-password-hash-synchronization.md) veya [geçişli kimlik doğrulaması](how-to-connect-pta.md) oturum açma yöntemleriyle birleştirilebilir. Ancak bu özellik Active Directory Federasyon Hizmetleri (AD FS) (ADFS) ile kullanılamaz.

**S: Sorunsuz SSO ücretsiz bir özelliktir mi?**

Sorunsuz SSO ücretsiz bir özelliktir ve Azure AD 'nin bu sürümü kullanabilmesi için ücretli bir sürümü gerekmez.

**S: [Microsoft Azure Almanya bulutta](https://www.microsoft.de/cloud-deutschland) ve [MICROSOFT Azure Kamu bulutta](https://azure.microsoft.com/features/gov/)sorunsuz SSO kullanılabilir mi?**

Hayır. Sorunsuz SSO yalnızca dünya çapındaki Azure AD örneğinde bulunabilir.

**S: Sorunsuz SSO 'nun `domain_hint` veya `login_hint` parametre özelliğinden faydalanan uygulamalar nelerdir?**

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

**S: Kullanıcı adı `Alternate ID` `userPrincipalName`yerine sorunsuz SSO Desteği ister misiniz?**

Evet. Sorunsuz SSO, `Alternate ID` [burada](how-to-connect-install-custom.md)gösterildiği gibi Azure AD Connect ' de yapılandırıldığında Kullanıcı adı olarak destekler. Office 365 uygulamalarının `Alternate ID`hepsi desteklenmez. Destek bildirimiyle ilgili uygulamanın belgelerine bakın.

**S: [Azure AD JOIN](../active-directory-azureadjoin-overview.md) ve sorunsuz SSO tarafından sunulan çoklu oturum açma deneyimi arasındaki fark nedir?**

[Azure AD JOIN](../active-directory-azureadjoin-overview.md) , CIHAZLARı Azure AD 'ye KAYıTLıYSA kullanıcılara SSO sağlar. Bu cihazların etki alanına katılmış olması gerekmez. SSO, *birincil yenileme belirteçleri* veya *prts*'Ler kullanılarak sağlanır ve Kerberos değildir. Kullanıcı deneyimi Windows 10 cihazlarında en iyi şekilde kullanılır. SSO, Microsoft Edge tarayıcısında otomatik olarak gerçekleşir. Ayrıca, tarayıcı uzantısı kullanılarak Chrome üzerinde de kullanılabilir.

Kiracınızda Azure AD JOIN ve sorunsuz SSO kullanabilirsiniz. Bu iki özellik tamamlayıcı bir özelliklerdir. Her iki özellik de açıksa, Azure AD birleştirmelerinin SSO 'SU sorunsuz SSO 'dan önceliklidir.

**S: Windows 10 olmayan cihazları Azure AD 'ye AD FS kullanmadan kaydetmek istiyorum. Bunun yerine sorunsuz SSO kullanabilir miyim?**

Evet, bu senaryo, [çalışma alanına katılma istemcisinin](https://www.microsoft.com/download/details.aspx?id=53554)2,1 veya sonraki bir sürümünü gerektirir.

**S: `AZUREADSSOACC` Bilgisayar hesabının Kerberos şifre çözme anahtarını nasıl alabilirim?**

Şirket içi ad ormanınızda oluşturulan `AZUREADSSOACC` bilgisayar hesabının (Azure AD 'yi temsil eden) Kerberos şifre çözme anahtarını çok sık almak önemlidir.

>[!IMPORTANT]
>Kerberos şifre çözme anahtarını en az 30 günde bir almanızı önemle tavsiye ederiz.

Azure AD Connect çalıştırdığınız şirket içi sunucuda bu adımları izleyin:

    **Step 1. Get list of AD forests where Seamless SSO has been enabled**

    1. İlk olarak, [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview)'i indirin ve yükleyin.
    2. `%programfiles%\Microsoft Azure Active Directory Connect` klasörüne gidin.
    3. Bu komutu kullanarak sorunsuz SSO PowerShell modülünü içeri aktarın: `Import-Module .\AzureADSSO.psd1`.
    4. PowerShell 'i yönetici olarak çalıştırın. PowerShell 'de, çağırın `New-AzureADSSOAuthenticationContext`. Bu komut, kiracınızın genel yönetici kimlik bilgilerini girebileceğiniz bir açılan menü vermelidir.
    5. Çağrısı `Get-AzureADSSOStatus | ConvertFrom-Json`yapın. Bu komut, bu özelliğin etkinleştirildiği AD ormanları listesini ("etki alanları" listesine bakın) sağlar.

    **2. adım. Üzerinde ayarlandığı her bir AD ormanında Kerberos şifre çözme anahtarını güncelleştirme**

    1. Çağrısı `$creds = Get-Credential`yapın. İstendiğinde, hedeflenen AD ormanının etki alanı yönetici kimlik bilgilerini girin.

    > [!NOTE]
    > Hedeflenen ad ormanını bulmak için Kullanıcı asıl adı (UPN) (johndoe@contoso.com) biçiminde veya etki alanı uygun Sam hesabı adı (contoso\johntikan veya contoso. com\johntikan) biçiminde belirtilen etki alanı yöneticisinin Kullanıcı adını kullanırız. Etki alanı nitelikli Sam hesabı adı kullanırsanız, [DNS kullanarak etki alanı yöneticisinin etki alanı denetleyicisini bulmak](https://social.technet.microsoft.com/wiki/contents/articles/24457.how-domain-controllers-are-located-in-windows.aspx)için Kullanıcı adının etki alanı bölümünü kullanırız. Bunun yerine UPN kullanırsanız, uygun etki alanı denetleyicisini [bulmadan önce etki alanı tam bir SAM hesap adına çeviririz](https://docs.microsoft.com/windows/desktop/api/ntdsapi/nf-ntdsapi-dscracknamesa) .

    2. Çağrısı `Update-AzureADSSOForest -OnPremCredentials $creds`yapın. Bu komut, bu belirli ad ormanındaki `AZUREADSSOACC` bilgisayar hesabının Kerberos şifre çözme anahtarını güncelleştirir ve Azure AD 'de güncelleştirir.
    3. Özelliği ayarladığınız her AD Ormanı için önceki adımları tekrarlayın.

    >[!IMPORTANT]
    >`Update-AzureADSSOForest` Komutu birden çok kez _çalıştırmayın_ emin olun. Aksi takdirde, kullanıcılarınızın Kerberos biletlerinin süresi dolana ve şirket içi Active Directory tarafından yeniden yayımlanıncaya kadar özellik çalışmayı sonlandırır.

**S: Sorunsuz SSO 'yu nasıl devre dışı bırakabilirim?**

    **Step 1. Disable the feature on your tenant**

        **Option A: Disable using Azure AD Connect**

        1. Run Azure AD Connect, choose **Change user sign-in page** and click **Next**.
        2. Uncheck the **Enable single sign on** option. Continue through the wizard.

        After completing the wizard, Seamless SSO will be disabled on your tenant. However, you will see a message on screen that reads as follows:

        "Single sign-on is now disabled, but there are additional manual steps to perform in order to complete clean-up. Learn more"

        To complete the clean-up process, follow steps 2 and 3 on the on-premises server where you are running Azure AD Connect.

        **Option B: Disable using PowerShell**

        Run the following steps on the on-premises server where you are running Azure AD Connect:

        1. First, download, and install [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview).
        2. Navigate to the `%programfiles%\Microsoft Azure Active Directory Connect` folder.
        3. Import the Seamless SSO PowerShell module using this command: `Import-Module .\AzureADSSO.psd1`.
        4. Run PowerShell as an Administrator. In PowerShell, call `New-AzureADSSOAuthenticationContext`. This command should give you a popup to enter your tenant's Global Administrator credentials.
        5. Call `Enable-AzureADSSO -Enable $false`.

        >[!IMPORTANT]
        >Disabling Seamless SSO using PowerShell will not change the state in Azure AD Connect. Seamless SSO will show as enabled in the **Change user sign-in** page.

    **Step 2. Get list of AD forests where Seamless SSO has been enabled**

    Follow tasks 1 through 4 below if you have disabled Seamless SSO using Azure AD Connect. If you have disabled Seamless SSO using PowerShell instead, jump ahead to task 5 below.

    1. İlk olarak, [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview)'i indirin ve yükleyin.
    2. `%programfiles%\Microsoft Azure Active Directory Connect` klasörüne gidin.
    3. Bu komutu kullanarak sorunsuz SSO PowerShell modülünü içeri aktarın: `Import-Module .\AzureADSSO.psd1`.
    4. PowerShell 'i yönetici olarak çalıştırın. PowerShell 'de, çağırın `New-AzureADSSOAuthenticationContext`. Bu komut, kiracınızın genel yönetici kimlik bilgilerini girebileceğiniz bir açılan menü vermelidir.
    5. Çağrısı `Get-AzureADSSOStatus | ConvertFrom-Json`yapın. Bu komut, bu özelliğin etkinleştirildiği AD ormanları listesini ("etki alanları" listesine bakın) sağlar.

    **3. adım. Listede gördüğünüz her `AZUREADSSOACCT` ad ormanındaki bilgisayar hesabını el ile silin.**

## <a name="next-steps"></a>Sonraki adımlar

- [**Hızlı başlangıç**](how-to-connect-sso-quick-start.md) -Azure AD sorunsuz SSO 'yu alın ve çalıştırın.
- [**Teknik kapsamlı**](how-to-connect-sso-how-it-works.md) bakış-bu özelliğin nasıl çalıştığını anlayın.
- [**Sorun giderme**](tshoot-connect-sso.md) -özellikle ilgili yaygın sorunları çözmeyi öğrenin.
- [**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) -yeni özellik isteklerini dosyalama.
