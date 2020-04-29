---
title: API Management 'de istemci sertifikası kimlik doğrulaması kullanarak API 'Leri güvenli hale getirme
titleSuffix: Azure API Management
description: İstemci sertifikalarını kullanarak API 'lere erişimi güvenli hale getirme hakkında bilgi edinin
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/13/2020
ms.author: apimpm
ms.openlocfilehash: 8c1d126f01580574a83850e63945aa7e513eaeda
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76713138"
---
# <a name="how-to-secure-apis-using-client-certificate-authentication-in-api-management"></a>API Management'ta istemci sertifikası kimlik doğrulamasını kullanarak API'lerin güvenliğini sağlama

API Management, istemci sertifikaları kullanarak API 'lere (yani API Management istemci) erişimi güvenli hale getirme yeteneği sağlar. İlke ifadelerini kullanarak, gelen sertifikayı doğrulayabilir ve istenen değerlere göre sertifika özelliklerini denetleyebilirsiniz.

İstemci sertifikaları kullanarak bir API 'nin arka uç hizmetine erişimin güvenliğini sağlama hakkında bilgi için (örneğin, arka uca API Management), bkz. [istemci sertifikası kimlik doğrulaması kullanarak arka uç hizmetlerini güvenli hale](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates) getirme

> [!IMPORTANT]
> Geliştirici, temel, standart veya Premium katmanlarda HTTP/2 üzerinden istemci sertifikaları almak ve doğrulamak için aşağıda gösterildiği gibi "özel etki alanları" dikey penceresinde "istemci sertifikası anlaş" ayarını açmanız gerekir.

![İstemci sertifikası ile anlaş](./media/api-management-howto-mutual-certificates-for-clients/negotiate-client-certificate.png)

> [!IMPORTANT]
> Tüketim katmanında istemci sertifikalarını almak ve doğrulamak için aşağıda gösterildiği gibi "özel etki alanları" dikey penceresinde "istemci sertifikası ıste" ayarını açmanız gerekir.

![İstemci sertifikası iste](./media/api-management-howto-mutual-certificates-for-clients/request-client-certificate.png)

## <a name="checking-the-issuer-and-subject"></a>Veren ve Subject denetleniyor

Aşağıdaki ilkeler bir istemci sertifikasının vereni ve konusunu denetlemek için yapılandırılabilir:

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Request.Certificate.Verify() || context.Request.Certificate.Issuer != "trusted-issuer" || context.Request.Certificate.SubjectName.Name != "expected-subject-name")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

> [!NOTE]
> Yerine sertifika iptal listesi kullanımını `context.Request.Certificate.VerifyNoRevocation()` denetlemeyi devre dışı bırakmak `context.Request.Certificate.Verify()`için.
> İstemci sertifikası kendinden imzalanmışsa, `context.Request.Certificate.Verify()` ve ' `context.Request.Certificate.VerifyNoRevocation()` nin çalışması için API Management, kök (veya ara) CA sertifikalarının [yüklenmiş](api-management-howto-ca-certificates.md) olması gerekir.

## <a name="checking-the-thumbprint"></a>Parmak izi denetleniyor

Aşağıdaki ilkeler bir istemci sertifikasının parmak izini denetlemek için yapılandırılabilir:

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Request.Certificate.Verify() || context.Request.Certificate.Thumbprint != "DESIRED-THUMBPRINT-IN-UPPER-CASE")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

> [!NOTE]
> Yerine sertifika iptal listesi kullanımını `context.Request.Certificate.VerifyNoRevocation()` denetlemeyi devre dışı bırakmak `context.Request.Certificate.Verify()`için.
> İstemci sertifikası kendinden imzalanmışsa, `context.Request.Certificate.Verify()` ve ' `context.Request.Certificate.VerifyNoRevocation()` nin çalışması için API Management, kök (veya ara) CA sertifikalarının [yüklenmiş](api-management-howto-ca-certificates.md) olması gerekir.

## <a name="checking-a-thumbprint-against-certificates-uploaded-to-api-management"></a>API Management yüklenen sertifikalara yönelik parmak izi denetleniyor

Aşağıdaki örnek, API Management yüklenen sertifikalara karşı bir istemci sertifikasının parmak izini nasıl denet, gösterir:

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Request.Certificate.Verify()  || !context.Deployment.Certificates.Any(c => c.Value.Thumbprint == context.Request.Certificate.Thumbprint))" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>

```

> [!NOTE]
> Yerine sertifika iptal listesi kullanımını `context.Request.Certificate.VerifyNoRevocation()` denetlemeyi devre dışı bırakmak `context.Request.Certificate.Verify()`için.
> İstemci sertifikası kendinden imzalanmışsa, `context.Request.Certificate.Verify()` ve ' `context.Request.Certificate.VerifyNoRevocation()` nin çalışması için API Management, kök (veya ara) CA sertifikalarının [yüklenmiş](api-management-howto-ca-certificates.md) olması gerekir.

> [!TIP]
> Bu [makalede](https://techcommunity.microsoft.com/t5/Networking-Blog/HTTPS-Client-Certificate-Request-freezes-when-the-Server-is/ba-p/339672) açıklanan istemci sertifikası kilitlenmesi sorunu, tek bir şekilde kendi kendine bildirimde bulunabilir. Örneğin, istekler dondurduktan sonra istek `403 Forbidden` zaman aşımından sonra durum koduna neden `context.Request.Certificate` olur `null`. Bu sorun genellikle yaklaşık `POST` 60 `PUT` KB veya daha büyük içerik uzunluğuna sahip olan istekleri etkiler.
> Bu sorunun oluşmasını önlemek için, aşağıda gösterildiği gibi, "özel etki alanları" dikey penceresinde istenen ana bilgisayar adları için "istemci sertifikası anlaş" ayarını etkinleştirin. Bu özellik, tüketim katmanında kullanılamaz.

![İstemci sertifikası ile anlaş](./media/api-management-howto-mutual-certificates-for-clients/negotiate-client-certificate.png)

## <a name="next-steps"></a>Sonraki adımlar

-   [İstemci sertifikası kimlik doğrulaması kullanarak arka uç hizmetlerini güvenli hale getirme](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates)
-   [Sertifikaları karşıya yükleme](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates)
