---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 01/18/2021
ms.author: mimart
ms.openlocfilehash: f94076f06fb13bae2a26e8ab6003d7574a2dacfd
ms.sourcegitcommit: fc401c220eaa40f6b3c8344db84b801aa9ff7185
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98674247"
---
## <a name="configure-the-relying-party-policy"></a>Bağlı olan taraf ilkesini yapılandırma

Bağlı olan taraf ilkesi, örneğin [SignUpSignIn.xml](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/blob/master/SocialAndLocalAccounts/SignUpOrSignin.xml), Azure AD B2C yürütecektir Kullanıcı yolculuğu belirtir. [Bağlı olan taraf](../articles/active-directory-b2c/relyingparty.md)Içinde **Defaultuseryolculuney** öğesini bulun. **Referenceıd** 'yi, kimlik sağlayıcısını eklediğiniz kullanıcı yolculuğu kimliğiyle eşleşecek şekilde güncelleştirin. 

Aşağıdaki örnekte, `CustomSignUpOrSignIn` Kullanıcı yolculuğu Için **referenceıd** şu şekilde ayarlanır `CustomSignUpOrSignIn` :

```xml
<RelyingParty>
  <DefaultUserJourney ReferenceId="CustomSignUpSignIn" />
  ...
</RelyingParty>
```

### <a name="upload-the-custom-policy"></a>Özel ilkeyi karşıya yükle

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure portal, araması yapın ve **Azure AD B2C** seçin.
1. **İlkeler** altında **kimlik deneyimi çerçevesi**' ni seçin.
1. **Özel Ilkeyi karşıya yükle**' yi seçin ve ardından, değiştirdiğiniz iki ilke dosyasını aşağıdaki sırada karşıya yükleyin: uzantı ilkesi, örneğin, `TrustFrameworkExtensions.xml` `SignUpSignIn.xml` .

## <a name="test-your-custom-policy"></a>Özel ilkenizi test etme

1. Bağlı olan taraf ilkenizi seçin, örneğin `B2C_1A_signup_signin`
1. **Uygulama** için, daha önce kaydetmiş olduğunuz bir Web uygulamasını seçin. **Yanıt URL 'si** gösterilmesi gerekir `https://jwt.ms` .
1. **Şimdi Çalıştır** düğmesini seçin.

Oturum açma işlemi başarılı olursa, tarayıcınız öğesine yönlendirilir `https://jwt.ms` ve bu, Azure AD B2C tarafından döndürülen belirtecin içeriğini görüntüler.

