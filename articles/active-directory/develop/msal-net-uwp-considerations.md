---
title: UWP konuları (MSAL.NET) | Mavisi
titleSuffix: Microsoft identity platform
description: .NET için Microsoft kimlik doğrulama kitaplığı (MSAL.NET) ile Evrensel Windows Platformu (UWP) kullanmayla ilgili hususlar hakkında bilgi edinin.
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
ms.openlocfilehash: 502bbe65cdc0aef768ff4f017b1f6a920815b001
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88118867"
---
# <a name="considerations-for-using-universal-windows-platform-with-msalnet"></a>MSAL.NET ile Evrensel Windows Platformu kullanma konuları
MSAL.NET ile Evrensel Windows Platformu (UWP) kullanan uygulamaların geliştiricileri, bu makalenin sunduğu kavramları göz önünde bulundurmalıdır.

## <a name="the-usecorporatenetwork-property"></a>UseCorporateNetwork özelliği
Windows Çalışma Zamanı (WinRT) platformunda `PublicClientApplication` Boole özelliği vardır `UseCorporateNetwork` . Bu özellik Windows 8.1 uygulamalar ve UWP uygulamalarının, kullanıcı federe Azure Active Directory (Azure AD) kiracısına sahip bir hesapta oturum açmış olması durumunda tümleşik Windows kimlik doğrulamasından (ıWA) faydalanmalarını sağlar. İşletim sisteminde oturum açan kullanıcılar çoklu oturum açma (SSO) da kullanabilir. `UseCorporateNetwork`Özelliği ayarladığınızda, msal.net bir Web kimlik doğrulama Aracısı (WAB) kullanır.

> [!IMPORTANT]
> `UseCorporateNetwork`Özelliği true olarak ayarlamak, uygulama geliştiricisinin uygulamada IWA 'yu etkinleştirdiğinizi varsayar. IWA 'yi etkinleştirmek için:
> - UWP uygulamanızın `Package.appxmanifest` ' de, **yetenekler** sekmesinde, aşağıdaki özellikleri etkinleştirin:
>   - **Kurumsal kimlik doğrulama**
>   - **Özel ağlar (Istemci & sunucusu)**
>   - **Paylaşılan Kullanıcı sertifikası**

Microsoft Store, kurumsal kimlik doğrulama veya paylaşılan kullanıcı sertifikalarının yeteneklerini isteyen uygulamaları kabul etmeden önce yüksek düzeyde bir doğrulama gerektirdiğinden, varsayılan olarak ıWA etkin değildir. Tüm geliştiriciler bu doğrulama düzeyini yapmak istememe.

UWP platformunda, temel alınan WAB uygulamasının Koşullu erişimin etkinleştirildiği kurumsal senaryolarda düzgün şekilde çalışmıyor. Kullanıcılar, Windows Hello kullanarak oturum açmaya çalıştıklarında bu sorunun belirtilerini görür. Kullanıcıdan bir sertifika seçmesi istendiğinde:

- PIN sertifikası bulunamadı.
- Kullanıcı bir sertifika seçtikten sonra PIN kodu istenmez.

Kullanıcı adı-parola ve telefon kimlik doğrulaması gibi alternatif bir yöntem kullanarak bu sorundan kaçınabilirsiniz, ancak deneyim iyi değildir.

## <a name="troubleshooting"></a>Sorun giderme

Bazı müşteriler, bir internet bağlantısı olduğunu ve bağlantının ortak bir ağla birlikte çalışıp çalışmadığını bildiren belirli kurumsal ortamlarda aşağıdaki oturum açma hatasını bildirdi.

```Text
We can't connect to the service you need right now. Check your network connection or try this again later.
```

WAB (temeldeki Windows bileşeni) özel bir ağa izin verdiğinden emin olmak için bu sorundan kurtulabilirsiniz. Bir kayıt defteri anahtarı ayarlayarak bunu yapabilirsiniz:

```Text
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\authhost.exe\EnablePrivateNetwork = 00000001
```

Daha fazla bilgi için bkz. [Web kimlik doğrulama Aracısı-Fiddler](/windows/uwp/security/web-authentication-broker#fiddler).

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki örnekler daha fazla bilgi sağlamaktadır.

Örnek | Platform | Açıklama 
|------ | -------- | -----------|
|[Active-Directory-DotNet-Native-UWP-v2](https://github.com/azure-samples/active-directory-dotnet-native-uwp-v2) | UWP | MSAL.NET kullanan bir UWP istemci uygulaması. Bir Azure AD 2,0 uç noktası kullanarak kimlik doğrulaması yapan bir kullanıcının Microsoft Graph erişir. <br>![Topoloji](media/msal-net-uwp-considerations/topology-native-uwp.png)|
|[Active-Directory-Xamarin-Native-v2](https://github.com/Azure-Samples/active-directory-xamarin-native-v2) | Xamarin iOS, Android, UWP | Azure AD 2,0 uç noktası aracılığıyla Microsoft kişisel hesaplarının ve Azure AD kimlik doğrulaması için MSAL kullanmayı gösteren basit bir Xamarin Forms uygulaması. Ayrıca, Microsoft Graph nasıl erişebileceğini ve elde edilen belirtecin nasıl gösterileceğini gösterir. <br>![Topoloji](media/msal-net-uwp-considerations/topology-xamarin-native.png)|