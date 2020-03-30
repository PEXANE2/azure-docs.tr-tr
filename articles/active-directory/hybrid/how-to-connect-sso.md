---
title: 'Azure AD Connect: Sorunsuz Tek Oturum Açma | Microsoft Dokümanlar'
description: Bu konu, Azure Active Directory (Azure AD) Sorunsuz Tek Oturum Açma'yı ve kurumsal ağınızdaki kurumsal masaüstü kullanıcıları için gerçek tek oturum açma nızı nasıl sağladığını açıklar.
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
ms.date: 08/13/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: f1b7e4716e731e6b73e3ac60b64baa71043906fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77483763"
---
# <a name="azure-active-directory-seamless-single-sign-on"></a>Azure Active Directory Sorunsuz Tek Oturum Açma

## <a name="what-is-azure-active-directory-seamless-single-sign-on"></a>Azure Active Directory Seamless Tek Oturum Açma nedir?

Azure Active Directory Sorunsuz Çoklu Oturum Açma (Azure AD Sorunsuz SSO) özelliği, kurumsal ağınıza bağlı kuruluş cihazlarını kullanan kullanıcıların otomatik olarak oturum açmasını sağlar. Etkinleştirildiğinde, kullanıcıların Azure AD'de oturum açabilmek için parolalarını yazmaları gerekmez ve genellikle kullanıcı adlarını bile yazın. Bu özellik kullanıcılarınızın şirket içi ek bileşenlere ihtiyaç duymadan bulut tabanlı uygulamalarınıza kolayca erişmesini sağlar.

>[!VIDEO https://www.youtube.com/embed/PyeAC85Gm7w]

Sorunsuz SSO, [Parola Karma Senkronizasyonu](how-to-connect-password-hash-synchronization.md) veya [Geçiş Kimlik Doğrulama](how-to-connect-pta.md) oturum açma yöntemleriyle birleştirilebilir. Sorunsuz SSO, Active Directory Federation Services (ADFS) için geçerli _değildir._

![Sorunsuz Tek Oturum Açma](./media/how-to-connect-sso/sso1.png)

>[!IMPORTANT]
>Sorunsuz SSO'nun yalnızca etki **alanına katılması** için kullanıcının aygıtına ihtiyacı vardır, ancak [Azure AD Joined](../devices/concept-azure-ad-join.md) veya [Karma Azure AD birleştirilmiş](../devices/concept-azure-ad-join-hybrid.md) aygıtlarda kullanılmaz. Azure AD'deki SSO katıldı ve Karma Azure AD [birincil yenileme belirteci](../devices/concept-primary-refresh-token.md)temel alınabilmek için çalışmalara katıldı.

## <a name="key-benefits"></a>Önemli avantajlar

- *Harika kullanıcı deneyimi*
  - Kullanıcılar hem şirket içinde hem de bulut tabanlı uygulamalarda otomatik olarak oturum açtı.
  - Kullanıcıların parolalarını tekrar tekrar girmeleri gerekmez.
- *Dağıtmak & yönetimi kolay*
  - Bu işi yapmak için şirket içinde ek bileşenlere gerek yok.
  - Bulut kimlik doğrulama herhangi bir yöntem ile çalışır - [Şifre Karma Senkronizasyon](how-to-connect-password-hash-synchronization.md) veya [Pass-through Kimlik Doğrulama.](how-to-connect-pta.md)
  - Grup İlkesi'ni kullanarak bazı veya tüm kullanıcılarınıza dağıtılabilir.
  - Herhangi bir AD FS altyapısına gerek kalmadan Windows 10 olmayan aygıtları Azure AD ile kaydedin. Bu özellik, sürüm 2.1 veya daha sonra [işyeri birleştirme istemcisi](https://www.microsoft.com/download/details.aspx?id=53554)kullanmanız gerekir.

## <a name="feature-highlights"></a>Özellik vurguları

- Oturum açma kullanıcı adı şirket içi varsayılan kullanıcı`userPrincipalName`adı ( ) veya Azure AD`Alternate ID`Connect 'te yapılandırılan başka bir öznitelik olabilir. Her iki kullanım örneğinde de `securityIdentifier` çalışır, çünkü Seamless SSO, Azure AD'deki ilgili kullanıcı nesnesini aramak için Kerberos biletindeki talebi kullanır.
- Sorunsuz SSO fırsatçı bir özelliktir. Herhangi bir nedenle başarısız olursa, kullanıcı oturum açma deneyimi normal davranışına geri döner - yani, kullanıcının oturum açma sayfasında parolasını girmesi gerekir.
- Bir uygulama (örneğin, `https://myapps.microsoft.com/contoso.com`) bir `domain_hint` (OpenID Connect) veya `whr` (SAML) parametresini `login_hint` ilerlerse - kiracınızı veya parametrenizi tanımlarsa - kullanıcıyı tanımlarsa, Azure AD oturum açma isteğinde kullanıcılar kullanıcı adları veya parolalar girmeden otomatik olarak oturum açar.
- `https://contoso.sharepoint.com`Kullanıcılar ayrıca, bir uygulama (örneğin, ) Azure AD'nin kiracı olarak ayarlanan uç noktalarına oturum açma istekleri `https://login.microsoftonline.com/contoso.com/<..>` `https://login.microsoftonline.com/<tenant_ID>/<..>` gönderirse (yani Azure AD'nin ortak `https://login.microsoftonline.com/common/<...>`bitiş noktası yerine- oturum açma isteği de yaşarlar.
- Oturum açma desteklenir. Bu, kullanıcıların Sorunsuz SSO'da otomatik olarak oturum açmaları yerine oturum açabilmeleri için başka bir Azure REKLAM hesabı seçmelerine olanak tanır.
- 16.0.8730.xxxx ve üzeri sürümlere sahip Office 365 Win32 istemcileri (Outlook, Word, Excel ve diğerleri) etkileşimli olmayan bir akış kullanılarak desteklenir. OneDrive için sessiz bir oturum açma deneyimi için [OneDrive sessiz config özelliğini](https://techcommunity.microsoft.com/t5/Microsoft-OneDrive-Blog/Previews-for-Silent-Sync-Account-Configuration-and-Bandwidth/ba-p/120894) etkinleştirmeniz gerekir.
- Azure AD Connect üzerinden etkinleştirilebilir.
- Ücretsiz bir özelliktir ve kullanmak için Azure AD'nin ücretli sürümlerine ihtiyacınız yoktur.
- Kerberos kimlik doğrulaması yapabilen platformlarda ve tarayıcılarda [modern kimlik doğrulamasını](https://docs.microsoft.com/office365/enterprise/modern-auth-for-office-2013-and-2016) destekleyen web tarayıcısı tabanlı istemcilerde ve Office istemcilerinde desteklenir:

| İşletim Sistemi\Tarayıcı |Internet Explorer|Microsoft Edge|Google Chrome|Mozilla Firefox|Safari|
| --- | --- |--- | --- | --- | -- 
|Windows 10|Evet\*|Evet|Evet|Evet\*\*\*|Yok
|Windows 8.1|Evet\*|Yok|Evet|Evet\*\*\*|Yok
|Windows 8|Evet\*|Yok|Evet|Evet\*\*\*|Yok
|Windows 7|Evet\*|Yok|Evet|Evet\*\*\*|Yok
|Windows Server 2012 R2 veya üzeri|Evet\*\*|Yok|Evet|Evet\*\*\*|Yok
|Mac OS X|Yok|Yok|Evet\*\*\*|Evet\*\*\*|Evet\*\*\*


\*Internet Explorer sürümleri 10 veya üzeri gerektirir

\*\*Internet Explorer sürümleri 10 veya üzeri gerektirir. Gelişmiş Korumalı Modu devre dışı

\*\*\*[Ek yapılandırma](how-to-connect-sso-quick-start.md#browser-considerations) gerektirir

>[!NOTE]
>Windows 10 için öneri, Azure AD ile en iyi tek oturum açma deneyimi için [Azure AD Join'i](../devices/concept-azure-ad-join.md) kullanmaktır.

## <a name="next-steps"></a>Sonraki adımlar

- [**Hızlı Başlangıç**](how-to-connect-sso-quick-start.md) - Azure REKLAM Sorunsuz SSO'ya başlayın ve çalıştırın.
- [**Dağıtım Planı**](https://aka.ms/deploymentplans/sso) - Adım adım dağıtım planı.
- [**Teknik Derin Dalış**](how-to-connect-sso-how-it-works.md) - Bu özelliğin nasıl çalıştığını anlayın.
- [**Sık Sorulan Sorular**](how-to-connect-sso-faq.md) - Sık sorulan soruların yanıtları.
- [**Sorun Giderme**](tshoot-connect-sso.md) - Özellik ile ilgili sık karşılaşılan sorunları nasıl çözeceğinizi öğrenin.
- [**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - Yeni özellik istekleri dosyalama için.

