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
ms.openlocfilehash: 2325509f68ced7c66d9f733b07247ae01301b565
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82181553"
---
## <a name="test-your-code"></a>Kodunuza test etme

Projenizi çalıştırmak için, Visual Studio 'da **F5**' i seçin. Uygulama **MainWindow** , burada gösterildiği gibi görüntülenir:

![Uygulamanızı test etme](./media/active-directory-develop-guidedsetup-windesktop-test/samplescreenshot.png)

Uygulamayı ilk kez çalıştırdığınızda ve **çağrı MICROSOFT Graph API** düğmesini seçtiğinizde, oturum açmanız istenir. Test etmek için bir Azure Active Directory hesabı (iş veya okul hesabı) veya bir Microsoft hesabı (live.com, outlook.com) kullanın.

![Uygulamada oturum açma](./media/active-directory-develop-guidedsetup-windesktop-test/signinscreenshot.png)

### <a name="provide-consent-for-application-access"></a>Uygulama erişimi için onay sağlayın

Uygulamanızda ilk kez oturum açtığınızda, uygulamanın profilinize erişmesine izin vermek ve burada gösterildiği gibi, oturumunuzu açmak için de onay vermeniz istenir:

![Uygulama erişimi için izin verin](./media/active-directory-develop-guidedsetup-windesktop-test/consentscreen.png)

### <a name="view-application-results"></a>Uygulama sonuçlarını görüntüle

Oturum açtıktan sonra, Microsoft Graph API 'sine yapılan çağrı tarafından döndürülen Kullanıcı profili bilgilerini görmeniz gerekir. Sonuçlar **API çağrı sonuçları** kutusunda görüntülenir. `AcquireTokenInteractive` Veya `AcquireTokenSilent` çağrısı yoluyla elde edilen belirteç hakkındaki temel bilgiler, **belirteç bilgi** kutusunda görünür olmalıdır. Sonuçlar aşağıdaki özellikleri içerir:

|Özellik  |Biçimlendir  |Açıklama |
|---------|---------|---------|
|**Nitelen** |<span>user@domain.com</span> |Kullanıcıyı tanımlamak için kullanılan Kullanıcı adı.|
|**Belirtecin süresi doluyor** |DateTime |Belirtecin süresinin dolacağı zaman. MSAL, belirteci gerektiği şekilde yenileyerek sona erme tarihini genişletir.|

### <a name="more-information-about-scopes-and-delegated-permissions"></a>Kapsamlar ve temsilci izinleri hakkında daha fazla bilgi

Microsoft Graph API 'SI, kullanıcının profilini okumak için *Kullanıcı. Read* kapsamını gerektirir. Bu kapsam, uygulama kayıt portalı 'nda kayıtlı her uygulamada varsayılan olarak otomatik olarak eklenir. Microsoft Graph için diğer API 'Ler ve arka uç sunucunuza yönelik özel API 'Ler için ek kapsamlar gerekebilir. Microsoft Graph API 'SI, kullanıcıların takvimlerini listelemek için *takvimler. Read* Scope 'ı gerektirir.

Kullanıcının takvimlerine bir uygulama bağlamında erişmek için, *takvimler.* uygulama kayıt bilgilerine, temsilci olarak oku iznini ekleyin. Ardından, *takvimlere. Read* kapsamını `acquireTokenSilent` çağrıya ekleyin.

>[!NOTE]
>Kapsam sayısını artırdıkça kullanıcıdan ek Yarışması istenebilir.

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
