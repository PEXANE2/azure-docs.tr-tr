---
title: Azure Active Directory REST API-kimlik doğrulaması
description: Azure Uygulama yapılandırmasında kimlik doğrulamak için Azure Active Directory kullanın REST API kullanarak
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: fb3d00fb79c55e29d578f5e068e4ae025414a935
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "93424418"
---
# <a name="azure-active-directory-authentication"></a>Azure Active Directory kimlik doğrulaması

Azure Active Directory (Azure AD) tarafından alınan bir belirteç ile **taşıyıcı** kimlik doğrulama DÜZENI kullanılarak http isteklerinin kimliği doğrulanabilir. Bu istekler TLS üzerinden aktarılmalıdır.

## <a name="prerequisites"></a>Ön koşullar

Azure AD belirteci istemek için kullanılacak sorumlu, geçerli [uygulama yapılandırma rollerinden](./rest-api-authorization-azure-ad.md) birine atanmalıdır

Kimlik doğrulaması için gereken tüm HTTP üstbilgilerini içeren her isteği sağlayın. Gerekli olan en düşük değer şunlardır:

|  İstek Başlığı | Açıklama  |
| --------------- | ------------ |
| **Yetkilendirme** | **Taşıyıcı** şeması için gereken kimlik doğrulama bilgileri. Biçim ve Ayrıntılar aşağıda açıklanmıştır. |

**Örnek:**

```http
Host: {myconfig}.azconfig.io
Authorization: Bearer {{AadToken}}
```

## <a name="azure-active-directory-token-acquisition"></a>Azure Active Directory belirteç alımı

Bir Azure AD belirtecinin alınmadan önce, hangi kullanıcının kimlik doğrulaması yapmasını istediğini, hangi hedef kitlesi için belirteç istediğini ve hangi Azure AD uç noktasının (Authority) kullanılacağını belirlemesi gerekir.

### <a name="audience"></a>Hedef kitle

Azure AD belirtecinin uygun bir hedef kitle ile istenmiş olması gerekir. Azure Uygulama yapılandırması için, bir belirteç istenirken aşağıdaki seyircilerin biri belirtilmelidir. Hedef kitle, belirtecin istendiği "kaynak" olarak da adlandırılabilir.

- {configurationStoreName}. azconfig. IO
- *. azconfig.io

> [!IMPORTANT]
> İstenen hedef kitle {configurationStoreName}. azconfig. IO olduğunda, isteği göndermek için kullanılan "ana bilgisayar" istek üstbilgisiyle (büyük/küçük harfe duyarlı) tam olarak eşleşmesi gerekir.

### <a name="azure-ad-authority"></a>Azure AD yetkilisi

Azure AD yetkilisi, bir Azure AD belirteci almak için kullanılan uç noktadır. Bu biçiminde olur `https://login.microsoftonline.com/{tenantId}` . `{tenantId}`Segment, kimlik doğrulamaya çalışan kullanıcı/uygulamanın ait olduğu Azure Active Directory KIRACı kimliğini ifade eder.

### <a name="authentication-libraries"></a>Kimlik doğrulama kitaplıkları

Azure, bir Azure AD belirteci alma işlemini basitleştirmek için Azure Active Directory kimlik doğrulama kitaplıkları (ADAL) adlı bir kitaplıklar kümesi sağlar. Bu kitaplıklar birden çok dil için oluşturulmuştur. Belgeler [burada](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries)bulunabilir.

## <a name="errors"></a>**Hatalar**

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: HMAC-SHA256, Bearer
```

**Neden:** Taşıyıcı düzenine sahip yetkilendirme isteği üst bilgisi sağlanmadı.
**Çözüm:** Geçerli ```Authorization``` http istek üst bilgisi sağlayın

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: HMAC-SHA256, Bearer error="invalid_token", error_description="Authorization token failed validation"
```

**Neden:** Azure AD belirteci geçerli değil.
**Çözüm:** Azure AD yetkilisinden bir Azure AD belirteci alın ve uygun kitlesinin kullanıldığından emin olun.

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: HMAC-SHA256, Bearer error="invalid_token", error_description="The access token is from the wrong issuer. It must match the AD tenant associated with the subscription to which the configuration store belongs. If you just transferred your subscription and see this error message, please try back later."
```

**Neden:** Azure AD belirteci geçerli değil.
**Çözüm:** Azure AD yetkilisinden bir Azure AD belirteci alın ve Azure AD kiracısının, yapılandırma deposunun ait olduğu abonelikle ilişkili bir tane olduğundan emin olun. Sorumlu birden fazla Azure AD kiracısına aitse bu hata görünebilir.
