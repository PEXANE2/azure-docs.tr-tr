---
title: 'Azure AD Connect: Sorunsuz Tek Oturum Açma - Sık sorulan sorular | Microsoft Dokümanlar'
description: Azure Active Directory Seamless Tek Oturum Açma hakkında sık sorulan soruların yanıtları.
services: active-directory
keywords: Azure AD Connect nedir, Active Directory'yi yükler, Azure AD, SSO, Tek Oturum Açma için gerekli bileşenler
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/07/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7241c8dfbedb24f95c29ea9e1c3f763218a5668d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72025665"
---
# <a name="azure-active-directory-seamless-single-sign-on-frequently-asked-questions"></a>Azure Active Directory Seamless Tek Oturum Açma: Sık sorulan sorular

Bu makalede, Azure Active Directory Seamless Tek Oturum Açma (Sorunsuz SSO) hakkında sık sorulan soruları ele alıyoruz. Yeni içerik için tekrar kontrol etmeye devam edin.

**S: Oturum açma yöntemleri Sorunsuz SSO ile ne işe yarar?**

Sorunsuz SSO, [Parola Karma Senkronizasyonu](how-to-connect-password-hash-synchronization.md) veya [Geçiş Kimlik Doğrulama](how-to-connect-pta.md) oturum açma yöntemleriyle birleştirilebilir. Ancak bu özellik Active Directory Federation Services (ADFS) ile kullanılamaz.

**S: Seamless SSO ücretsiz bir özellik mi?**

Sorunsuz SSO ücretsiz bir özelliktir ve kullanmak için Azure AD'nin ücretli sürümlerine ihtiyacınız yoktur.

**S: Sorunsuz SSO, [Microsoft Azure Almanya bulutu](https://www.microsoft.de/cloud-deutschland) ve [Microsoft Azure Devlet bulutu](https://azure.microsoft.com/features/gov/)içinde kullanılabilir mi?**

Hayır. Sorunsuz SSO yalnızca dünya çapında Azure AD örneğinde kullanılabilir.

**S: Sorunsuz SSO'nun `login_hint` hangi uygulamalardan veya parametre özelliğinden `domain_hint` yararlanır?**

Aşağıda, bu parametreleri Azure AD'ye gönderebilen kapsamlı olmayan bir uygulama listesi listelenmiştir ve bu nedenle kullanıcılara Kesintisiz SSO (örneğin, kullanıcı adlarını veya parolalarını girebilmelerine gerek yoktur) kullanarak sessiz bir oturum açma deneyimi sağlar:

| Uygulama adı | Kullanılacak uygulama URL'si |
| -- | -- |
| Erişim paneli | https:\//myapps.microsoft.com/contoso.com |
| Web'de Outlook | https:\//outlook.office365.com/contoso.com |
| Office 365 portalları | https:\//portal.office.com?domain_hint=contoso.com,\/https: /www.office.com?domain_hint=contoso.com |

Buna ek olarak, bir uygulama Azure AD'nin kiracı olarak ayarlanan uç noktalarına oturum açma istekleri gönderirse kullanıcılar sessiz oturum açma deneyimi yaşar - yani https:\//login.microsoftonline.com/contoso.com/<..> veya https:\//login.microsoftonline.com/<tenant_ID>/<... > - Azure AD'nin ortak bitiş noktası yerine\/- yani https: /login.microsoftonline.com/common/<...>. Aşağıda, bu tür oturum açma isteklerini yapan uygulamaların kapsamlı olmayan bir listesi listelenmiştir.

| Uygulama adı | Kullanılacak uygulama URL'si |
| -- | -- |
| SharePoint Online | https:\//contoso.sharepoint.com |
| Azure portalında | https:\//portal.azure.com/contoso.com |

Yukarıdaki tablolarda, kiracınız için doğru uygulama URL'lerine ulaşmak için "contoso.com" ile alan adınız değiştirin.

Sessiz oturum açma deneyimimizi kullanarak başka uygulamalar da istiyorsanız, geri bildirim bölümünden bize bildirin.

**S: Seamless SSO `Alternate ID` yerine, kullanıcı adı `userPrincipalName`olarak destek mi?**

Evet. Azure AD `Alternate ID` Connect'te [burada](how-to-connect-install-custom.md)gösterildiği gibi yapılandırıldığında Kesintisiz SSO kullanıcı adı olarak destekler. Tüm Office 365 uygulamaları `Alternate ID`desteklenmez. Destek bildirimi için belirli uygulamabelgelerine bakın.

**S: [Azure AD Join](../active-directory-azureadjoin-overview.md) ve Sorunsuz SSO tarafından sağlanan tek oturum açma deneyimi arasındaki fark nedir?**

[Azure AD Join,](../active-directory-azureadjoin-overview.md) aygıtları Azure AD'ye kayıtlıysa kullanıcılara SSO sağlar. Bu aygıtların etki alanına katılması gerekmez. SSO birincil *yenileme belirteçleri* veya *İİT'ler*kullanılarak sağlanmaktadır, Kerberos değil. Kullanıcı deneyimi en uygun windows 10 cihazlarda. SSO, Microsoft Edge tarayıcıda otomatik olarak gerçekleşir. Ayrıca chrome'da bir tarayıcı uzantısı kullanarak çalışır.

Hem Azure AD Join'i hem de Kesintisiz SSO'u kiracınızda kullanabilirsiniz. Bu iki özellik birbirini tamamlar. Her iki özellik de açıksa, Azure AD Join'in SSO'su Sorunsuz SSO'dan önce gelir.

**S: Windows olmayan 10 aygıtları AD FS kullanmadan Azure AD'ye kaydetmek istiyorum. Bunun yerine Dikişsiz SSO kullanabilir miyim?**

Evet, bu senaryonun 2.1 sürümü veya daha sonra [işyeri birleştirme istemcisi](https://www.microsoft.com/download/details.aspx?id=53554)gerekir.

**S: Bilgisayar hesabının Kerberos şifre çözme anahtarını `AZUREADSSOACC` nasıl devredebilirim?**

Şirket içi AD ormanınızda oluşturulan bilgisayar hesabının `AZUREADSSOACC` (Azure AD'sini temsil eden) Kerberos şifre çözme anahtarını sık sık devretmek önemlidir.

>[!IMPORTANT]
>Kerberos şifre çözme anahtarını en az 30 günde bir teslim almanızı şiddetle öneririz.

Azure AD Connect çalıştırdığınız şirket içi sunucuda aşağıdaki adımları izleyin:

   **Adım 1. Sorunsuz SSO'nun etkin hale geldiği AD ormanlarının listesini alın**

   1. İlk olarak Azure [AD PowerShell'i](https://docs.microsoft.com/powershell/azure/active-directory/overview)indirin ve yükleyin.
   2. `%programfiles%\Microsoft Azure Active Directory Connect` klasörüne gidin.
   3. Bu komutu kullanarak Dikişsiz SSO `Import-Module .\AzureADSSO.psd1`PowerShell modüllerini içeri aktarın: .
   4. PowerShell'i Yönetici olarak çalıştırın. PowerShell'de, `New-AzureADSSOAuthenticationContext`arayın. Bu komut, kiracınızın Genel Yönetici kimlik bilgilerini girmeniz için size bir açılır pencere verecektir.
   5. Arayın. `Get-AzureADSSOStatus | ConvertFrom-Json` Bu komut, bu özelliğin etkinleştirildiği AD ormanlarının listesini ("Etki Alanları" listesine bakın) sağlar.

   **Adım 2. Ayarlandığının her bir AD ormanındaki Kerberos şifre çözme anahtarını güncelleştirin**

   1. Arayın. `$creds = Get-Credential` İstendiğinde, amaçlanan AD ormanı için Etki Alanı Yöneticisi kimlik bilgilerini girin.

   > [!NOTE]
   >Etki alanı yöneticisi kimlik bilgileri kullanıcı adı SAM hesap adı biçiminde girilmelidir (contoso\johndoe veya contoso.com\johndoe). DNS kullanarak Etki Alanı Yöneticisi'nin Etki Alanı Denetleyicisini bulmak için kullanıcı adının etki alanı bölümünü kullanırız.

   >[!NOTE]
   >Kullanılan etki alanı yöneticisi hesabı Korumalı Kullanıcılar grubunun bir üyesi olmamalıdır. Bu nedenle, işlem başarısız olur.

   2. Arayın. `Update-AzureADSSOForest -OnPremCredentials $creds` Bu komut, bu özel AD ormanındaki `AZUREADSSOACC` bilgisayar hesabı için Kerberos şifre çözme anahtarını güncelleştirir ve Azure AD'de güncelleştirir.
   3. Özelliği ayarladığınız her AD ormanı için önceki adımları yineleyin.

   >[!IMPORTANT]
   >Komutu birden fazla kez `Update-AzureADSSOForest` _çalıştırmadığınızdan_ emin olun. Aksi takdirde, özellik kullanıcılarınızın Kerberos biletlerinin süresi dolana kadar çalışmayı durdurur ve şirket içi Active Directory tarafından yeniden verilir.

**S: Sorunsuz SSO'yı nasıl devre dışı dışı bebilirim?**

   **Adım 1. Kiracınızdaki özelliği devre dışı**

   **Seçenek A: Azure AD Connect'i kullanarak devre dışı**
    
   1. Azure AD Connect'i çalıştırın, **kullanıcı oturum aç'T-in sayfasını** değiştir'i seçin ve **İleri'yi**tıklatın.
   2. **Tek oturum** açma seçeneğini etkinleştir seçeneğinin işaretini kaldırın. Sihirbazdan devam et.

   Sihirbazı tamamladıktan sonra, Sorunsuz SSO kiracınız üzerinde devre dışı bırakılır. Ancak, ekranda aşağıdaki gibi okuyan bir ileti görürsünüz:

   "Tek oturum açma artık devre dışı bırakılmıştır, ancak temizlemeyi tamamlamak için gerçekleştirmesi gereken ek el ile adımlar vardır. Daha fazla bilgi edinin"

   Temizleme işlemini tamamlamak için, Azure AD Connect çalıştırdığınız şirket içi sunucuda 2 ve 3 adımlarını izleyin.

   **Seçenek B: PowerShell'i kullanarak devre dışı**

   Azure AD Connect'i çalıştırdığınız şirket içi sunucuda aşağıdaki adımları çalıştırın:

   1. İlk olarak Azure [AD PowerShell'i](https://docs.microsoft.com/powershell/azure/active-directory/overview)indirin ve yükleyin.
   2. `%programfiles%\Microsoft Azure Active Directory Connect` klasörüne gidin.
   3. Bu komutu kullanarak Dikişsiz SSO `Import-Module .\AzureADSSO.psd1`PowerShell modüllerini içeri aktarın: .
   4. PowerShell'i Yönetici olarak çalıştırın. PowerShell'de, `New-AzureADSSOAuthenticationContext`arayın. Bu komut, kiracınızın Genel Yönetici kimlik bilgilerini girmeniz için size bir açılır pencere verecektir.
   5. Arayın. `Enable-AzureADSSO -Enable $false`

   >[!IMPORTANT]
   >PowerShell kullanarak Kesintisiz SSO'yu devre dışı bırakmak Azure AD Connect'teki durumu değiştirmez. Sorunsuz **SSO, Kullanıcı oturum açma** yı değiştir sayfasında etkin olarak gösterecektir.

   **Adım 2. Sorunsuz SSO'nun etkin hale geldiği AD ormanlarının listesini alın**

   Azure AD Connect'i kullanarak Kesintisiz SSO'ya devre dışı bıraktıysanız aşağıdaki 1'den 4'e kadar olan görevleri izleyin. Bunun yerine PowerShell'i kullanarak Seamless SSO'ya devre dışı bıraktıysanız, aşağıdaki 5.

   1. İlk olarak Azure [AD PowerShell'i](https://docs.microsoft.com/powershell/azure/active-directory/overview)indirin ve yükleyin.
   2. `%programfiles%\Microsoft Azure Active Directory Connect` klasörüne gidin.
   3. Bu komutu kullanarak Dikişsiz SSO `Import-Module .\AzureADSSO.psd1`PowerShell modüllerini içeri aktarın: .
   4. PowerShell'i Yönetici olarak çalıştırın. PowerShell'de, `New-AzureADSSOAuthenticationContext`arayın. Bu komut, kiracınızın Genel Yönetici kimlik bilgilerini girmeniz için size bir açılır pencere verecektir.
   5. Arayın. `Get-AzureADSSOStatus | ConvertFrom-Json` Bu komut, bu özelliğin etkinleştirildiği AD ormanlarının listesini ("Etki Alanları" listesine bakın) sağlar.

   **Adım 3. Listelenen her `AZUREADSSOACCT` AD ormanından bilgisayar hesabını el ile silin.**

## <a name="next-steps"></a>Sonraki adımlar

- [**Quickstart**](how-to-connect-sso-quick-start.md) - Azure REKLAM Sorunsuz SSO'ya başlayın ve çalıştırın.
- [**Teknik Derin Dalış**](how-to-connect-sso-how-it-works.md) - Bu özelliğin nasıl çalıştığını anlayın.
- [**Sorun Giderme**](tshoot-connect-sso.md) - Özellik ile ilgili sık karşılaşılan sorunları nasıl çözeceğinizi öğrenin.
- [**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - Yeni özellik istekleri dosyalama için.
