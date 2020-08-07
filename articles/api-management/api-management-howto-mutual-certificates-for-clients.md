---
title: API Management 'de istemci sertifikası kimlik doğrulaması kullanarak API 'Leri güvenli hale getirme
titleSuffix: Azure API Management
description: İstemci sertifikalarını kullanarak API 'lere erişimi güvenli hale getirme hakkında bilgi edinin. İlke ifadelerini, gelen sertifikaları doğrulamak için kullanabilirsiniz.
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
ms.openlocfilehash: 19e0d741d959eba704f26e7e8f7b5d311aa77775
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87904879"
---
# <a name="how-to-secure-apis-using-client-certificate-authentication-in-api-management"></a>API Management'ta istemci sertifikası kimlik doğrulamasını kullanarak API'lerin güvenliğini sağlama

API Management, istemci sertifikaları kullanarak API 'lere (yani API Management istemci) erişimi güvenli hale getirme yeteneği sağlar. İlke ifadelerini kullanarak, gelen sertifikayı doğrulayabilir ve istenen değerlere göre sertifika özelliklerini denetleyebilirsiniz.

İstemci sertifikaları kullanarak bir API 'nin arka uç hizmetine erişimin güvenliğini sağlama hakkında bilgi için (örneğin, arka uca API Management), bkz. [istemci sertifikası kimlik doğrulaması kullanarak arka uç hizmetlerini güvenli hale](./api-management-howto-mutual-certificates.md) getirme

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
> Yerine sertifika iptal listesi kullanımını denetlemeyi devre dışı bırakmak için `context.Request.Certificate.VerifyNoRevocation()` `context.Request.Certificate.Verify()` .
> İstemci sertifikası kendinden imzalanmışsa, ve ' nin çalışması için API Management, kök (veya ara) CA sertifikalarının [yüklenmiş](api-management-howto-ca-certificates.md) olması gerekir `context.Request.Certificate.Verify()` `context.Request.Certificate.VerifyNoRevocation()` .

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
> Yerine sertifika iptal listesi kullanımını denetlemeyi devre dışı bırakmak için `context.Request.Certificate.VerifyNoRevocation()` `context.Request.Certificate.Verify()` .
> İstemci sertifikası kendinden imzalanmışsa, ve ' nin çalışması için API Management, kök (veya ara) CA sertifikalarının [yüklenmiş](api-management-howto-ca-certificates.md) olması gerekir `context.Request.Certificate.Verify()` `context.Request.Certificate.VerifyNoRevocation()` .

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
> Yerine sertifika iptal listesi kullanımını denetlemeyi devre dışı bırakmak için `context.Request.Certificate.VerifyNoRevocation()` `context.Request.Certificate.Verify()` .
> İstemci sertifikası kendinden imzalanmışsa, ve ' nin çalışması için API Management, kök (veya ara) CA sertifikalarının [yüklenmiş](api-management-howto-ca-certificates.md) olması gerekir `context.Request.Certificate.Verify()` `context.Request.Certificate.VerifyNoRevocation()` .

> [!TIP]
> Bu [makalede](https://techcommunity.microsoft.com/t5/Networking-Blog/HTTPS-Client-Certificate-Request-freezes-when-the-Server-is/ba-p/339672) açıklanan istemci sertifikası kilitlenmesi sorunu, tek bir şekilde kendi kendine bildirimde bulunabilir. Örneğin, istekler `403 Forbidden` dondurduktan sonra istek zaman aşımından sonra durum koduna neden `context.Request.Certificate` olur `null` . Bu sorun genellikle `POST` `PUT` yaklaşık 60 KB veya daha büyük içerik uzunluğuna sahip olan istekleri etkiler.
> Bu sorunun oluşmasını önlemek için, aşağıda gösterildiği gibi, "özel etki alanları" dikey penceresinde istenen ana bilgisayar adları için "istemci sertifikası anlaş" ayarını etkinleştirin. Bu özellik, tüketim katmanında kullanılamaz.

![İstemci sertifikası ile anlaş](./media/api-management-howto-mutual-certificates-for-clients/negotiate-client-certificate.png)

## <a name="next-steps"></a>Sonraki adımlar

-   [İstemci sertifikası kimlik doğrulaması kullanarak arka uç hizmetlerini güvenli hale getirme](./api-management-howto-mutual-certificates.md)
-   [Sertifikaları karşıya yükleme](./api-management-howto-mutual-certificates.md)
