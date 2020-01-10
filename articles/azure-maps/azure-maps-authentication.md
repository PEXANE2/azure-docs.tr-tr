---
title: Azure haritalar ile kimlik doğrulama | Microsoft Docs
description: Microsoft Azure haritalar hizmetlerini kullanmak için Azure Active Directory (Azure AD) veya paylaşılan anahtar kimlik doğrulaması. Azure Maps abonelik anahtarını almayı öğrenin.
author: walsehgal
ms.author: v-musehg
ms.date: 12/30/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: a58436063009b732a15e74c8a3fc3f95b8df29cf
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75834194"
---
# <a name="authentication-with-azure-maps"></a>Azure Haritalar ile kimlik doğrulaması

Azure haritalar isteklerin kimliğini doğrulamak için iki yolu destekler: paylaşılan anahtar ve Azure Active Directory (Azure AD). Bu makalede, uygulamanıza kılavuzluk eden bu kimlik doğrulama yöntemleri açıklanmaktadır.

## <a name="shared-key-authentication"></a>Paylaşılan anahtar kimlik doğrulaması

Paylaşılan anahtar kimlik doğrulaması, her Azure Maps isteğiyle bir Azure Maps hesabı tarafından oluşturulan anahtarları geçirir. Azure haritalar hizmetlerine yönelik her istek için, *abonelik ANAHTARıNıN* URL 'ye bir parametre olarak eklenmesi gerekir. Birincil ve ikincil anahtarlar, Azure Maps hesabı oluşturulduktan sonra oluşturulur. Paylaşılan anahtar kimlik doğrulamasını kullanarak Azure haritalar 'ı çağırdığınızda, birincil anahtarı abonelik anahtarı olarak kullanmanızı öneririz. İkincil anahtar, kayan anahtar değişiklikleri gibi senaryolarda kullanılabilir.  

Azure portal anahtarlarınızı görüntüleme hakkında daha fazla bilgi için bkz. [kimlik doğrulamasını yönetme](https://aka.ms/amauthdetails).

> [!Tip]
> Anahtarlarınızın düzenli olarak yeniden oluşturulması önerilir. Diğerini yeniden oluştururken bir anahtarla bağlantıları koruyabilmeniz için iki anahtarla birlikte sağlanmış olursunuz. Anahtarlarınızı yeniden oluşturduğunuzda, yeni anahtarları kullanmak için hesaba erişen tüm uygulamaları güncelleştirmeniz gerekir.



## <a name="authentication-with-azure-active-directory-preview"></a>Azure Active Directory (Önizleme) ile kimlik doğrulaması

Azure haritalar artık Azure haritalar hizmetleri isteklerinin kimlik doğrulaması için [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) tümleştirmesi sunmaktadır. Azure AD, [rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview)dahil olmak üzere kimlik tabanlı kimlik doğrulaması sağlar. bu sayede, Azure Maps kaynaklarına Kullanıcı düzeyi, grup düzeyi ve uygulama düzeyinde erişim izni verilir. Aşağıdaki bölümler Azure AD ile Azure Maps tümleştirmesinin kavramlarını ve bileşenlerini anlamanıza yardımcı olabilir.

## <a name="authentication-with-oauth-access-tokens"></a>OAuth erişim belirteçleriyle kimlik doğrulama

Azure Maps, Azure Maps hesabı içeren bir Azure aboneliğiyle ilişkili Azure AD kiracılar için **OAuth 2,0** erişim belirteçleri kabul eder. Azure haritalar şu belirteçleri kabul eder:

* Azure AD kullanıcıları. 
* Kullanıcılar tarafından temsilci atanmış izinleri kullanan iş ortağı uygulamaları.
* Azure kaynakları için Yönetilen kimlikler.

Azure Maps, her Azure Maps hesabı için *benzersiz bir tanımlayıcı (ISTEMCI kimliği)* oluşturur. Bu istemci KIMLIĞINI ek parametrelerle birleştirdiğinizde, Azure ortamınıza bağlı olarak aşağıdaki tabloda yer alan değerleri belirterek Azure AD 'den belirteç isteğinde bulunabilir.

| Azure ortamı   | Azure AD belirteç uç noktası |
| --------------------|-------------------------|
| Azure Genel        | https://login.microsoftonline.com |
| Azure Devlet Kurumları    | https://login.microsoftonline.us |


Azure haritalar için Azure AD ve istek belirteçlerini yapılandırma hakkında daha fazla bilgi için bkz. [Azure haritalar 'da kimlik doğrulamasını yönetme](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

Azure AD 'den belirteç isteme hakkında genel bilgi için bkz. [kimlik doğrulaması nedir?](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).

## <a name="request-azure-map-resources-with-oauth-tokens"></a>OAuth belirteçleri ile Azure harita kaynakları isteme

Azure AD 'den bir belirteç alındıktan sonra, aşağıdaki iki zorunlu istek üst bilgisi kümesiyle bir istek Azure Maps 'a gönderilebilir:

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

Azure AD, RBAC kullanarak güvenli kaynaklara erişimi denetlemenize olanak tanır. Azure haritalar hesabınızı oluşturduktan ve Azure AD kiracınızda Azure Maps Azure AD uygulamanızı kaydettikten sonra, Azure Maps hesap portalı sayfasında bir Kullanıcı, Grup, uygulama veya Azure kaynağı için RBAC ayarlayabilirsiniz.

Azure haritalar, Azure kaynakları için Yönetilen kimlikler aracılığıyla tek tek Azure AD kullanıcıları, grupları, uygulamaları ve Azure hizmetleri için okuma erişim denetimini destekler.

![Azure haritalar veri okuyucu (Önizleme)](./media/azure-maps-authentication/concept.png)

RBAC ayarlarınızı görüntüleme hakkında daha fazla bilgi için bkz. [Azure Maps IÇIN RBAC yapılandırma](https://aka.ms/amrbac).

## <a name="managed-identities-for-azure-resources-and-azure-maps"></a>Azure kaynakları ve Azure Maps için Yönetilen kimlikler

Azure [kaynakları Için Yönetilen kimlikler](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) , Azure Maps hizmetlerine erişim yetkisine sahip olabilecek otomatik olarak yönetilen bir kimlikle Azure hizmetleri (Azure App Service, Azure Işlevleri, Azure sanal makineleri vb.) sağlar.  

## <a name="next-steps"></a>Sonraki adımlar

* Azure AD ve Azure Maps ile bir uygulamanın kimliğini doğrulama hakkında daha fazla bilgi edinmek için bkz. [Azure haritalar 'da kimlik doğrulamasını yönetme](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

* Azure haritalar Harita Denetimi ve Azure AD 'nin kimlik doğrulaması hakkında daha fazla bilgi edinmek için bkz. [Azure maps harita denetimi kullanma](https://aka.ms/amaadmc).
