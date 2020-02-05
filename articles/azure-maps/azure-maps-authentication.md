---
title: Kimlik doğrulama yöntemleri | Microsoft Azure haritaları
description: Bu makalede Azure Active Directory (Azure AD) ve paylaşılan anahtar kimlik doğrulaması hakkında bilgi edineceksiniz. Her ikisi de Microsoft Azure haritaları Hizmetleri için kullanılır. Azure Maps abonelik anahtarını almayı öğrenin.
author: walsehgal
ms.author: v-musehg
ms.date: 01/28/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 2bcc2d4c92e903b723bffa8461a8a1a10534d3e4
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025631"
---
# <a name="authentication-with-azure-maps"></a>Azure Haritalar ile kimlik doğrulaması

Azure haritalar isteklerin kimliğini doğrulamak için iki yolu destekler: paylaşılan anahtar kimlik doğrulaması ve Azure Active Directory kimlik doğrulaması. Bu makalede, uygulamanıza kılavuzluk eden bu kimlik doğrulama yöntemleri açıklanmaktadır.

## <a name="shared-key-authentication"></a>Paylaşılan anahtar kimlik doğrulaması

Paylaşılan anahtar kimlik doğrulaması, her Azure Maps isteğiyle bir Azure Maps hesabı tarafından oluşturulan anahtarları geçirir. Azure haritalar hizmetlerine yönelik her istek için, *abonelik ANAHTARıNıN* URL 'ye bir parametre olarak eklenmesi gerekir. Birincil ve ikincil anahtarlar, Azure Maps hesabı oluşturulduktan sonra oluşturulur. Paylaşılan anahtar kimlik doğrulamasını kullanarak Azure haritalar 'ı çağırdığınızda, birincil anahtarı abonelik anahtarı olarak kullanmanızı öneririz. İkincil anahtar, kayan anahtar değişiklikleri gibi senaryolarda kullanılabilir.  

Azure portal anahtarlarınızı görüntüleme hakkında daha fazla bilgi için bkz. [kimlik doğrulamasını yönetme](https://aka.ms/amauthdetails).

> [!Tip]
> Anahtarlarınızın düzenli olarak yeniden oluşturulması önerilir. İki anahtarla sunulmaktadır, böylece diğer bir anahtarla bağlantıları sürdürebilmeniz için, diğerini yeniden oluşturma. Anahtarlarınızı yeniden oluşturduğunuzda, yeni anahtarlarla hesabınıza erişen tüm uygulamaları güncelleştirmeniz gerekir.



## <a name="authentication-with-azure-active-directory-preview"></a>Azure Active Directory (Önizleme) ile kimlik doğrulaması

Azure haritalar artık [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis)kullanarak Azure haritalar Hizmetleri için istek kimlik doğrulaması sunmaktadır. Azure AD, [rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview)dahil olmak üzere kimlik tabanlı kimlik doğrulaması sağlar. RBAC, Azure Maps kaynaklarına Kullanıcı düzeyi, grup düzeyi veya uygulama düzeyinde erişim sağlamak için kullanılır. Sonraki bölümlerde Azure AD ile Azure Maps tümleştirmesinin kavramlarını ve bileşenlerini anlamanıza yardımcı olabilir.
## <a name="authentication-with-oauth-access-tokens"></a>OAuth erişim belirteçleriyle kimlik doğrulama

Azure Maps, Azure Maps hesabı içeren bir Azure aboneliğiyle ilişkili Azure AD kiracılar için **OAuth 2,0** erişim belirteçleri kabul eder. Azure haritalar şu belirteçleri kabul eder:

* Azure AD kullanıcıları
* Kullanıcılar tarafından yetkilendirilen izinleri kullanan iş ortağı uygulamaları
* Azure kaynakları için yönetilen kimlikler

Azure Maps, her Azure Maps hesabı için *benzersiz bir tanımlayıcı (ISTEMCI kimliği)* oluşturur. Bu istemci KIMLIĞINI ek parametrelerle birleştirdiğinizde Azure AD 'den belirteç isteğinde bulunabilir. Belirteç istemek için, Azure ortamınıza göre aşağıdaki tabloda yer alan değerleri belirtmeniz gerekir.

| Azure ortamı   | Azure AD belirteç uç noktası |
| --------------------|-------------------------|
| Azure genel        | https://login.microsoftonline.com |
| Azure Devlet Kurumları    | https://login.microsoftonline.us |


Azure haritalar için Azure AD ve istek belirteçlerini yapılandırma hakkında daha fazla bilgi için bkz. [Azure haritalar 'da kimlik doğrulamasını yönetme](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

Azure AD 'den belirteç isteme hakkında genel bilgi için bkz. [kimlik doğrulaması nedir?](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).

## <a name="request-azure-map-resources-with-oauth-tokens"></a>OAuth belirteçleri ile Azure harita kaynakları isteme

Azure AD 'den bir belirteç alındıktan sonra, Azure Maps 'e aşağıdaki gerekli istek üst bilgileri kümesiyle bir istek gönderilir:

| İstek üst bilgisi    |    Değer    |
|:------------------|:------------|
| x-MS-istemci kimliği    | 30d7cc.... 9F55|
| Yetkilendirme     | Taşıyıcı eyJ0e.... HNIVN |

> [!Note]
> `x-ms-client-id` Azure Maps kimlik doğrulaması sayfasında görüntülenen Azure haritalar hesap tabanlı GUID 'sidir.

Bir OAuth belirteci kullanan bir Azure Maps Route isteği örneği aşağıda verilmiştir:

```
GET /route/directions/json?api-version=1.0&query=52.50931,13.42936:52.50274,13.43872 
Host: atlas.microsoft.com 
x-ms-client-id: 30d7cc….9f55 
Authorization: Bearer eyJ0e….HNIVN 
```

İstemci KIMLIĞINIZI görüntüleme hakkında daha fazla bilgi için bkz. [kimlik doğrulama ayrıntılarını görüntüleme](https://aka.ms/amauthdetails).

## <a name="control-access-with-rbac"></a>RBAC ile erişimi denetleme

Azure AD 'de, güvenli kaynaklara erişimi denetlemek için RBAC kullanın. Azure haritalar hesabınızı ayarlayın ve Azure haritalar Azure AD KIRACıNıZı kaydedin. Azure haritalar, Azure kaynakları için Yönetilen kimlikler aracılığıyla tek tek Azure AD kullanıcıları, grupları, uygulamaları, Azure kaynakları ve Azure hizmetleri için okuma erişim denetimini destekler. Azure haritalar portalı sayfasında, istediğiniz rolleriniz için RBAC 'yi ayarlayabilirsiniz.

![Azure haritalar veri okuyucu (Önizleme)](./media/azure-maps-authentication/concept.png)

RBAC ayarlarınızı görüntüleme hakkında daha fazla bilgi için bkz. [Azure Maps IÇIN RBAC yapılandırma](https://aka.ms/amrbac).

## <a name="managed-identities-for-azure-resources-and-azure-maps"></a>Azure kaynakları ve Azure Maps için Yönetilen kimlikler

Azure [kaynakları Için Yönetilen kimlikler](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) , Azure Maps hizmetlerine erişme yetkisine sahip olan, otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Yönetilen kimliklere örnek olarak şunlar verilebilir: Azure App Service, Azure Işlevleri ve Azure sanal makineleri.

## <a name="next-steps"></a>Sonraki adımlar

* Azure AD ve Azure Maps ile bir uygulamanın kimliğini doğrulama hakkında daha fazla bilgi edinmek için bkz. [Azure haritalar 'da kimlik doğrulamasını yönetme](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

* Azure haritalar Harita Denetimi ve Azure AD 'nin kimlik doğrulaması hakkında daha fazla bilgi edinmek için bkz. [Azure maps harita denetimi kullanma](https://aka.ms/amaadmc).
