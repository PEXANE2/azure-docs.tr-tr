---
title: Geliştirdiğiniz uygulamalarda FIDO2 anahtarlarıyla passwordless kimlik doğrulamasını destekleme | Mavisi
titleSuffix: Microsoft identity platform
description: Bu dağıtım kılavuzunda, geliştirdiğiniz uygulamalarda FIDO2 güvenlik anahtarlarıyla passwordless kimlik doğrulamasının nasıl destekleneceği açıklanır
services: active-directory
author: knicholasa
ms.service: active-directory
ms.subservice: develop
ms.topic: reference
ms.workload: identity
ms.date: 1/29/2021
ms.author: nichola
ms.custom: aaddev
ms.openlocfilehash: f63d7aed75b14f5f008a639d667d8806b233b9fa
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102174607"
---
# <a name="support-passwordless-authentication-with-fido2-keys-in-apps-you-develop"></a>Geliştirdiğiniz uygulamalarda FIDO2 anahtarlarıyla passwordless kimlik doğrulamasını destekleme

Bu yapılandırma ve en iyi uygulamalar, [FIDO2 parolasız kimlik doğrulamanın](../../active-directory/authentication/concept-authentication-passwordless.md) uygulamalarınızın kullanıcıları tarafından kullanılabilmesini engelleyen yaygın senaryolardan kaçınmanıza yardımcı olur.

## <a name="general-best-practices"></a>Genel en iyi uygulamalar

### <a name="domain-hints"></a>Etki alanı ipuçları

[Giriş bölgesi bulmayı](../../active-directory/manage-apps/configure-authentication-for-federated-users-portal.md)atlamak için bir etki alanı ipucu kullanmayın. Bu özellik, oturum açma işlemlerini daha kolay hale getirmek için tasarlanmıştır, ancak federal kimlik sağlayıcısı, passwordless kimlik doğrulamasını desteklemeyebilir.

### <a name="requiring-specific-credentials"></a>Belirli kimlik bilgileri gerektirme

SAML kullanıyorsanız, [Requestedadncontext öğesini kullanarak](single-sign-on-saml-protocol.md#requestauthncontext)bir parolanın gerekli olduğunu belirtmeyin.

Requestedaduthncontext öğesi isteğe bağlıdır, bu nedenle bunu çözmek için SAML kimlik doğrulama isteklerinden kaldırabilirsiniz. Bu, genel bir en iyi uygulamadır. bu öğenin kullanılması, Multi-Factor Authentication gibi diğer kimlik doğrulama seçeneklerinin doğru şekilde çalışmasını da engelleyebilir.

### <a name="using-the-most-recently-used-authentication-method"></a>En son kullanılan kimlik doğrulama yöntemini kullanma

İlk olarak bir kullanıcı tarafından en son kullanılan oturum açma yöntemi bu kullanıcılara sunulacaktır. Bu, kullanıcılar sunulan ilk seçeneği kullanmaları gerektiğini düşünzaman karışıklığına neden olabilir. Bununla birlikte, aşağıda gösterildiği gibi, "oturum açmak için diğer yollar" seçeneğini belirleyerek başka bir seçenek de seçebilirler.

:::image type="content" source="./media/support-fido2-authentication/most-recently-used-method.png" alt-text="Kullanıcı kimlik doğrulama deneyiminin görüntüsü, kullanıcının kimlik doğrulama yöntemini değiştirmesine izin veren düğmeyi vurgular.":::

## <a name="platform-specific-best-practices"></a>Platforma özgü en iyi uygulamalar

### <a name="desktop"></a>Masaüstü

Kimlik doğrulaması uygulamak için önerilen seçenekler sırasıyla:

- Microsoft kimlik doğrulama kitaplığı 'nı (MSAL) kullanan .NET masaüstü uygulamaları, Windows kimlik doğrulama Yöneticisi 'Ni (WAM) kullanmalıdır. Bu tümleştirme ve avantajları [GitHub üzerinde belgelenmiştir](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/wam).
- Katıştırılmış bir tarayıcıda FIDO2 desteklemek için [WebView2](/microsoft-edge/webview2/) kullanın.
- Sistem tarayıcısını kullanın. Masaüstü platformları için MSAL kitaplıkları varsayılan olarak bu yöntemi kullanır. Kullandığınız tarayıcının FIDO2 kimlik doğrulamasını desteklediğinden emin olmak için FIDO2 tarayıcı uyumluluğu sayfasında sayfamıza başvurabilirsiniz.

### <a name="mobile"></a>Mobil

Şubat 2021 itibariyle, FIDO2 Şu anda yerel iOS veya Android uygulamaları için desteklenmemektedir ancak geliştirme aşamasındadır.

Uygulamaları kullanılabilirliğine hazırlamak için ve genel bir en iyi uygulama olarak, iOS ve Android Uygulamaları, MSAL kullanarak sistem Web tarayıcısını varsayılan yapılandırmasıyla kullanmalıdır.

MSAL kullanmıyorsanız, kimlik doğrulaması için hala sistem Web tarayıcısını kullanmanız gerekir. Çoklu oturum açma ve koşullu erişim gibi özellikler, sistem Web tarayıcısı tarafından sunulan paylaşılan bir Web yüzeyine dayanır. Bu, [Chrome özel sekmeleri](https://developer.chrome.com/docs/multidevice/android/customtabs/) (Android) veya bir [Web hizmeti aracılığıyla bir kullanıcının kimliğini doğrulamak için kullanılan anlamına gelir | Apple geliştirici belgeleri](https://developer.apple.com/documentation/authenticationservices/authenticating_a_user_through_a_web_service) (iOS).

### <a name="web-and-single-page-apps"></a>Web ve tek sayfalı uygulamalar

Bir Web tarayıcısında çalışan uygulamalar için FIDO2 parolasız kimlik doğrulamasının kullanılabilirliği, tarayıcı ve platformun birleşimine göre farklılık gösterir. Kullanıcılarınızın karşılaşacağınız birleşimin desteklenip desteklenmediğini denetlemek için [FIDO2 uyumluluk matrimize](../authentication/fido2-compatibility.md) başvurabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Active Directory için passwordless kimlik doğrulama seçenekleri](../../active-directory/authentication/concept-authentication-passwordless.md)