---
title: UWP hususlar (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: .NET (MSAL.NET) için Microsoft Kimlik Doğrulama Kitaplığı ile Evrensel Windows Platformu 'nı (UWP) kullanmak için dikkat edilmesi gereken hususlar hakkında bilgi edinin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/16/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2eeec28569cf31af4542d6cd7aca1fb27d77b1e0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77132522"
---
# <a name="considerations-for-using-universal-windows-platform-with-msalnet"></a>Evrensel Windows Platform'u MSAL.NET ile kullanmak için dikkat edilmesi gerekenler
MSAL.NET ile Evrensel Windows Platformu (UWP) kullanan uygulamaların geliştiricileri bu makalenin sunduğu kavramları göz önünde bulundurmalıdır.

## <a name="the-usecorporatenetwork-property"></a>UseCorporateNetwork özelliği
Windows Runtime (WinRT) platformunda Boolean `PublicClientApplication` `UseCorporateNetwork`özelliği vardır. Bu özellik, kullanıcı federe Azure Active Directory (Azure AD) kiracısı olan bir hesapta oturum açmışsa, Windows 8.1 uygulamalarının ve UWP uygulamalarının Tümleşik Windows kimlik doğrulamasından (IWA) yararlanmasını sağlar. İşletim sisteminde oturum açmış olan kullanıcılar tek oturum açma (SSO) de kullanabilirler. `UseCorporateNetwork` Özelliği ayarladığınızda, MSAL.NET bir web kimlik doğrulama aracısı (WAB) kullanır.

> [!IMPORTANT]
> `UseCorporateNetwork` Özelliği niçin doğru ayarladığınız, uygulama geliştiricisinin uygulamada IWA'yı etkinleştirdiğini varsayar. IWA'yı etkinleştirmek için:
> - UWP uygulamanızın `Package.appxmanifest`, **Yetenekler** sekmesinde aşağıdaki yetenekleri etkinleştirin:
>   - **Kurumsal Kimlik Doğrulama**
>   - **Özel Ağlar (İstemci & Sunucusu)**
>   - **Paylaşılan Kullanıcı Sertifikası**

Microsoft Mağazası, kurumsal kimlik doğrulama veya paylaşılan kullanıcı sertifikalarının özelliklerini isteyen uygulamaları kabul etmeden önce yüksek düzeyde doğrulama gerektirdiğinden, IWA varsayılan olarak etkinleştirilemez. Tüm geliştiriciler bu doğrulama düzeyini yapmak istemez.

UWP platformunda, temel WAB uygulaması koşullu erişimin etkin olduğu kuruluş senaryolarında doğru çalışmaz. Kullanıcılar, Windows Hello kullanarak oturum açmaya çalıştıklarında bu sorunun belirtilerini görürler. Kullanıcıdan bir sertifika seçmesi istendiğinde:

- PIN sertifikası bulunamadı.
- Kullanıcı bir sertifika seçtikten sonra PIN için istenmez.

Kullanıcı adı-parola ve telefon kimlik doğrulaması gibi alternatif bir yöntem kullanarak bu sorunu önlemeyi deneyebilirsiniz, ancak deneyim iyi değildir.

## <a name="troubleshooting"></a>Sorun giderme

Bazı müşteriler, internet bağlantıları olduğunu ve bağlantının ortak bir ağla çalıştığını bildikleri belirli kurumsal ortamlarda aşağıdaki oturum açma hatasını bildirmiş olur.

```Text
We can't connect to the service you need right now. Check your network connection or try this again later.
```

WAB'nin (temel Windows bileşeni) özel bir ağa izin verdiğinden emin olarak bu sorunu önleyebilirsiniz. Bunu bir kayıt defteri anahtarı ayarlayarak yapabilirsiniz:

```Text
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\authhost.exe\EnablePrivateNetwork = 00000001
```

Daha fazla bilgi için [Web kimlik doğrulama aracısı - Fiddler](https://docs.microsoft.com/windows/uwp/security/web-authentication-broker#fiddler)adresine bakın.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki örnekler daha fazla bilgi sağlar.

Örnek | Platform | Açıklama 
|------ | -------- | -----------|
|[aktif-dizin-dotnet-yerli-uwp-v2](https://github.com/azure-samples/active-directory-dotnet-native-uwp-v2) | UWP | MSAL.NET kullanan bir UWP istemci uygulaması. Azure AD 2.0 bitiş noktası kullanarak kimlik doğrulaması yapan bir kullanıcı için Microsoft Graph'a erişir. <br>![Topoloji](media/msal-net-uwp-considerations/topology-native-uwp.png)|
|[aktif-dizin-xamarin-yerli-v2](https://github.com/Azure-Samples/active-directory-xamarin-native-v2) | Xamarin iOS, Android, UWP | Azure AD 2.0 bitiş noktası üzerinden Microsoft kişisel hesaplarını ve Azure AD'yi doğrulamak için MSAL'ın nasıl kullanılacağını gösteren basit bir Xamarin Forms uygulaması. Ayrıca, Microsoft Graph'a nasıl erişilir ve ortaya çıkan belirteci gösterir. <br>![Topoloji](media/msal-net-uwp-considerations/topology-xamarin-native.png)|
