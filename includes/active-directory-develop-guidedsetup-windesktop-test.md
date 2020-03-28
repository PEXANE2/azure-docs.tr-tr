---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: f121be4ec8c3d3ab618e2955d9dbd8ab5eea461d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "79128645"
---
## <a name="test-your-code"></a>Kodunuza test etme

Projenizi çalıştırmak için Visual Studio'da **F5'i**seçin. Uygulamanız **MainWindow** burada gösterildiği gibi görüntülenir:

![Başvurunuzu test edin](./media/active-directory-develop-guidedsetup-windesktop-test/samplescreenshot.png)

Uygulamayı ilk çalıştırdığınızda ve **Microsoft Graph API'yi ara** düğmesini seçtiğinizde oturum açmanız istenir. Test etmek için azure Active Directory hesabını (iş veya okul hesabı) veya Microsoft hesabı (live.com, outlook.com) kullanın.

![Uygulamada oturum açma](./media/active-directory-develop-guidedsetup-windesktop-test/signinscreenshot.png)

### <a name="provide-consent-for-application-access"></a>Uygulama erişimi için onay sağlama

Uygulamanızda ilk oturum açtığınızda, uygulamanın profilinize erişmesine ve burada gösterildiği gibi oturum açmasına izin vermek için izin vermeniz de istenir:

![Başvuru erişimi için onayınızı sağlayın](./media/active-directory-develop-guidedsetup-windesktop-test/consentscreen.png)

### <a name="view-application-results"></a>Uygulama sonuçlarını görüntüleme

Oturum açmadan sonra, Microsoft Graph API'ye yapılan çağrıyla döndürülen kullanıcı profili bilgilerini görmeniz gerekir. Sonuçlar **API Çağrı Sonuçları** kutusunda görüntülenir. Çağrı yoluyla edinilen `AcquireTokenInteractive` veya `AcquireTokenSilent` **Belirteç Bilgileri** kutusunda görünür olması gereken belirteç hakkında temel bilgiler. Sonuçlar aşağıdaki özellikleri içerir:

|Özellik  |Biçimlendir  |Açıklama |
|---------|---------|---------|
|**Username** |<span>user@domain.com</span> |Kullanıcıyı tanımlamak için kullanılan kullanıcı adı.|
|**Belirteç Süresi Doluyor** |DateTime |Belirteç süresinin dolduğu saat. MSAL, belirteci gerektiği gibi yenileyerek son kullanma tarihini uzatar.|


<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri hakkında daha fazla bilgi

Microsoft Graph API' si, kullanıcının profilini okuması için *kullanıcının okuma* kapsamını gerektirir. Bu kapsam, Uygulama Kayıt Portalı'na kayıtlı olan her uygulamada varsayılan olarak otomatik olarak eklenir. Microsoft Graph için diğer API'lerin yanı sıra arka uç sunucunuz için özel API'ler ek kapsamlar gerektirebilir. Microsoft Graph API, kullanıcının takvimlerini listelemek için *Takvimler.Oku* kapsamını gerektirir.

Bir uygulama bağlamında kullanıcının takvimlerine erişmek için, uygulama kayıt bilgilerine *Takvimler.Read* temsilcisi iznini ekleyin. Ardından, `acquireTokenSilent` aramaiçin *Takvimler.Oku* kapsamını ekleyin.

>[!NOTE]
>Kapsam sayısını artırdıkça kullanıcıdan ek izinler istenebilir.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
