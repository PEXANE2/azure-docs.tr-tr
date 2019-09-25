---
title: Android uygulamasından kullanıcıların oturumunu açma ve Microsoft Graph API’sini çağırma | Microsoft Docs
description: Kullanıcıların oturum açma ve Android uygulamamın Microsoft Graph API 'sini çağırma hakkında bilgi edinin.
services: active-directory
documentationcenter: android
author: rwike77
manager: CelesteDG
ms.assetid: da1ee39f-89d3-4d36-96f1-4eabbc662343
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: quickstart
ms.date: 05/21/2019
ms.author: ryanwi
ms.reviewer: brandwe, jmprieur, saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: ac4007cc9379aa98d88099df13ba303063f9831f
ms.sourcegitcommit: 263a69b70949099457620037c988dc590d7c7854
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71268516"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-android-app"></a>Hızlı Başlangıç: Kullanıcıların oturum açması ve Android uygulamasından Microsoft Graph API 'sini çağırma

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

Android uygulaması geliştiriyorsanız, Microsoft Azure Active Directory (Azure AD) kullanıcılarının oturum açmasını kolaylaştırıyor ve basit hale getirir. Azure AD, Microsoft Graph veya kendi korumalı web API’niz üzerinden uygulamanızın kullanıcı verilerine erişmesini sağlar.

Azure AD kimlik doğrulama kitaplığı (ADAL) Android kitaplığı, uygulamanızın endüstri standardı kullanarak [Microsoft Azure Active Directory hesaplarını](https://azure.microsoft.com/services/active-directory/) destekleyerek [Microsoft Azure bulut](https://azure.microsoft.com/free/cloud-services/) ve [Microsoft Graph API](https://developer.microsoft.com/graph) 'sini kullanmaya başlama olanağı sağlar OAuth 2,0 ve OpenID Connect.

Bu hızlı başlangıçta şunları yapmayı öğreneceksiniz:

* Microsoft Graph için belirteç alma
* Belirteci yenileme
* Microsoft Graph'ı çağırma
* Kullanıcının oturumu kapatma

## <a name="prerequisites"></a>Önkoşullar

Başlamak için, kullanıcıları oluşturabildiğiniz ve uygulama kaydedebildiğiniz bir Azure AD kiracısına ihtiyacınız vardır. Henüz bir kiracınız yoksa [nasıl kiracı alınabileceğini öğrenin](quickstart-create-new-tenant.md).

## <a name="scenario-sign-in-users-and-call-the-microsoft-graph"></a>Senaryo: Kullanıcıları oturum açın ve Microsoft Graph çağırın

![Azure AD ve Android topolojisini gösterir](./media/quickstart-v1-android/active-directory-android-topology.png)

Bu uygulamayı tüm Azure AD hesaplarında kullanabilirsiniz. Hem tek kiracılı hem de çok kiracılı senaryoları destekler (adımlarda açıklanmıştır). Kurumsal kullanıcılarla bağlantı kurmak ve Microsoft Graph üzerinden onların Azure + O365 verilerine erişmek için nasıl uygulama oluşturabileceğinizi gösterir. Kimlik doğrulaması akışı sırasında son kullanıcıların oturum açması ve uygulama izinlerini onaylaması gerekebilir. Bazı durumlarda da yöneticinin uygulamaya onay vermesi gerekebilir. Bu örnekteki mantığın büyük bölümü, son kullanıcının kimliğini doğrulamayı ve Microsoft Graph’a temel bir çağrı yapmayı gösterir.

## <a name="sample-code"></a>Örnek kod

[GitHub 'da](https://github.com/Azure-Samples/active-directory-android)tam örnek kodu bulabilirsiniz.

```Java
// Initialize your app with MSAL
AuthenticationContext mAuthContext = new AuthenticationContext(
        MainActivity.this,
        AUTHORITY,
        false);


// Perform authentication requests
mAuthContext.acquireToken(
    getActivity(),
    RESOURCE_ID,
    CLIENT_ID,
    REDIRECT_URI,
    PromptBehavior.Auto,
    getAuthInteractiveCallback());

// ...

// Get tokens to call APIs like the Microsoft Graph
mAuthResult.getAccessToken()
```

## <a name="step-1-register-and-configure-your-app"></a>1\. adım: Uygulamanızı kaydedin ve yapılandırın

[Azure portalını](https://portal.azure.com) kullanarak Microsoft'a kaydedilmiş yerel bir istemci uygulamanız olması gerekir.

1. Uygulama kaydını alma
    - [Azure portalına](https://aad.portal.azure.com) gidin.
    - ***Azure Active Directory*** > ***Uygulama kayıtları***’nı seçin.

2. Uygulama oluşturma
    - **Yeni kayıt**seçeneğini belirleyin.
    - **Ad** alanına bir uygulama adı girin.
    - **Desteklenen hesap türleri**altında, **herhangi bir kurumsal dizin ve kişisel Microsoft hesabında hesaplar**' ı seçin.
    - **Yeniden yönlendirme URI 'si**içinde, açılan listeden **ortak istemci (mobil ve Masaüstü)** öğesini seçin `http://localhost`ve girin.
    - **Kaydol**' a tıklayın.

3. Microsoft Graph’ı yapılandırma
    - Seçin **API izinleri**.
    - **Izin Ekle**' yi SEÇIN, **API Seç** ' in altında ***Microsoft Graph***seçin.
    - **Temsilci izinleri**altında **Kullanıcı. oku**seçeneğini belirleyin ve sonra kaydetmek için **Ekle** düğmesine basın.        
    
4. Tebrikler! Uygulamanız başarıyla yapılandırıldı. Sonraki bölümde size gerekecekler:
    - `Application ID`
    - `Redirect URI`

## <a name="step-2-get-the-sample-code"></a>2\. adım: Örnek kodunu alma

1. Kodu kopyalayın.
    ```
    git clone https://github.com/Azure-Samples/active-directory-android
    ```
2. Örneği Android Studio’da açın.
    - **Var olan Android Studio projesini aç**'ı seçin.

## <a name="step-3-configure-your-code"></a>3\. adım: Kodunuzu yapılandırın

Bu kod örneğinin tüm yapılandırmasını ***src/main/java/com/azuresamples/azuresampleapp/MainActivity.java*** dosyasında bulabilirsiniz.

1. `CLIENT_ID` sabitini `ApplicationID` ile değiştirin.
2. `REDIRECT URI` sabitini daha önce yapılandırdığınız `Redirect URI` ile (`http://localhost`) değiştirin.

## <a name="step-4-run-the-sample"></a>4\. Adım: Örneği çalıştırma

1. **Derle > Projeyi Temizle**’yi seçin.
2. **Çalıştır > Uygulamayı çalıştır**’ı seçin.
3. Uygulama oluşturulmuş olma.ı biraz temel UX göstermelidir. `Call Graph API` düğmesine tıkladığınızda, oturum açılmasını isteyecek ve ardından sessizce yeni belirteçle Microsoft Graph API'sini çağıracaktır.

## <a name="next-steps"></a>Sonraki adımlar

1. Kitaplığı mekanizması ve yeni senaryolarla özellikleri yapılandırma hakkında daha fazla bilgi için [ADAL Android Wiki'sini](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki) gözden geçirin.
2. Yerel senaryolarda uygulama, eklenmiş bir Webview kullanır ve uygulamadan çıkılmaz. `Redirect URI` rastgele olabilir.
3. Sorun mu buldunuz yoksa istekleriniz mi var? Etiketiyle `azure-active-directory`Stack Overflow bir sorun oluşturabilir veya gönderebilirsiniz.

### <a name="cross-app-sso"></a>Uygulamalar arası SSO

[ADAL kullanarak Android’de uygulamalar arası SSO’nun nasıl etkinleştirildiğini](howto-v1-enable-sso-android.md) öğrenin.

### <a name="auth-telemetry"></a>Kimlik doğrulaması telemetrisi

ADAL kitaplığı, uygulama geliştiricilerin kendi uygulamalarının davranışlarını anlamalarına ve daha iyi deneyimler oluşturmalarına yardımcı olmak için kimlik doğrulama telemetrisi sunar. Bu sayede başarılı oturum açma işlemlerini, etkin kullanıcıları ve bunlar dışında bazı ilginç içgörüleri yakalayabilirsiniz. Kimlik doğrulama telemetrisini kullanmak için, uygulama geliştiricilerin olayları toplamak ve depolamak üzere bir telemetri hizmeti kurması gerekmez.

Kimlik doğrulaması telemetrisi hakkında daha fazla bilgi için [ADAL Android kimlik doğrulaması telemetrisi](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/Telemetry) konusunu gözden geçirin.
