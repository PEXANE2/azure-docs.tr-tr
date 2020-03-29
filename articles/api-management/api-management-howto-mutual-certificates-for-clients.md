---
title: API Yönetimi'nde istemci sertifikası kimlik doğrulamasını kullanarak Güvenli API'ler
titleSuffix: Azure API Management
description: İstemci sertifikalarını kullanarak API'lere erişimi nasıl güvenli hale erdireceklerini öğrenin
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76713138"
---
# <a name="how-to-secure-apis-using-client-certificate-authentication-in-api-management"></a>API Management'ta istemci sertifikası kimlik doğrulamasını kullanarak API'lerin güvenliğini sağlama

API Yönetimi, istemci sertifikalarını kullanarak API'lere (örneğin, istemciden API Yönetimi'ne) erişimi güvence altına alma olanağı sağlar. Gelen sertifikayı doğrulayabilir ve ilke ifadelerini kullanarak sertifika özelliklerini istenilen değerlere göre denetleyebilirsiniz.

İstemci sertifikalarını kullanarak bir API'nin arka uç hizmetine erişimi sağlama hakkında bilgi için (örneğin, ARKA UÇ A.Ş.), [istemci sertifikası kimlik doğrulamasını kullanarak arka uç hizmetlerinin nasıl güvenli hale getirilebildiğini](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates) görmek

> [!IMPORTANT]
> Geliştirici, Temel, Standart veya Premium katmanlarda HTTP/2 üzerinden istemci sertifikalarını almak ve doğrulamak için aşağıda gösterildiği gibi "Özel etki alanları" bıçağındaki "Müşteri sertifikasını pazarla" ayarını açmanız gerekir.

![İstemci sertifikasını görüşün](./media/api-management-howto-mutual-certificates-for-clients/negotiate-client-certificate.png)

> [!IMPORTANT]
> Tüketim katmanındaki istemci sertifikalarını almak ve doğrulamak için aşağıda gösterildiği gibi "Özel etki alanları" bıçağındaki "İstemci sertifikası" ayarını açmanız gerekir.

![İsteme istemci sertifikası](./media/api-management-howto-mutual-certificates-for-clients/request-client-certificate.png)

## <a name="checking-the-issuer-and-subject"></a>İhraççının ve konunun denetle

Aşağıdaki ilkeler, müşteri sertifikasının vereni ve konusunu denetlemek için yapılandırılabilir:

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
> Sertifika iptal listesi kullanımını `context.Request.Certificate.VerifyNoRevocation()` denetlemek için `context.Request.Certificate.Verify()`.
> İstemci sertifikası kendi imzası varsa, root (veya ara) CA sertifikası(lar) `context.Request.Certificate.Verify()` `context.Request.Certificate.VerifyNoRevocation()` için ve çalışması için API Yönetimi'ne [yüklenmelidir.](api-management-howto-ca-certificates.md)

## <a name="checking-the-thumbprint"></a>Parmak izini kontrol etme

Aşağıdaki ilkeler, istemci sertifikasının parmak izini denetlemek için yapılandırılabilir:

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
> Sertifika iptal listesi kullanımını `context.Request.Certificate.VerifyNoRevocation()` denetlemek için `context.Request.Certificate.Verify()`.
> İstemci sertifikası kendi imzası varsa, root (veya ara) CA sertifikası(lar) `context.Request.Certificate.Verify()` `context.Request.Certificate.VerifyNoRevocation()` için ve çalışması için API Yönetimi'ne [yüklenmelidir.](api-management-howto-ca-certificates.md)

## <a name="checking-a-thumbprint-against-certificates-uploaded-to-api-management"></a>API Yönetimi'ne yüklenen sertifikalara karşı parmak izini denetleme

Aşağıdaki örnek, bir istemci sertifikasının parmak izini API Yönetimi'ne yüklenen sertifikalarla karşı karşıya nasıl denetleriz gösterir:

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
> Sertifika iptal listesi kullanımını `context.Request.Certificate.VerifyNoRevocation()` denetlemek için `context.Request.Certificate.Verify()`.
> İstemci sertifikası kendi imzası varsa, root (veya ara) CA sertifikası(lar) `context.Request.Certificate.Verify()` `context.Request.Certificate.VerifyNoRevocation()` için ve çalışması için API Yönetimi'ne [yüklenmelidir.](api-management-howto-ca-certificates.md)

> [!TIP]
> Bu [makalede](https://techcommunity.microsoft.com/t5/Networking-Blog/HTTPS-Client-Certificate-Request-freezes-when-the-Server-is/ba-p/339672) açıklanan istemci sertifikası kilitlenme sorunu çeşitli şekillerde kendini gösterebilir, örneğin `403 Forbidden` istekleri dondurma, istekleri `context.Request.Certificate` `null`zamanlama sonra durum kodu neden, olduğunu . Bu sorun `POST` genellikle `PUT` yaklaşık 60KB veya daha büyük içerik uzunluğu ile etkiler ve istekleri.
> Bu sorunun oluşmasını önlemek için, aşağıda gösterildiği gibi "Özel etki alanları" bıçağında istenen ana bilgisayar adları için "Müşteri sertifikasını görüşün" ayarını açın. Bu özellik Tüketim katmanında kullanılamaz.

![İstemci sertifikasını görüşün](./media/api-management-howto-mutual-certificates-for-clients/negotiate-client-certificate.png)

## <a name="next-steps"></a>Sonraki adımlar

-   [İstemci sertifikası kimlik doğrulamasını kullanarak arka uç hizmetlerini sağlama](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates)
-   [Sertifikalar nasıl yüklenir?](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates)
